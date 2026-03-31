# Eval-Meta — Plan

**Created**: 2026-03-31 (S96)
**Owner**: Rho (research lead)
**Status**: Plan saved. Awaiting dispatch prompt.
**Trigger**: Raffi observed 4 agents clustering at 0.92 composite with minimal deltas. Question: genuine performance convergence or ceiling artifact of the scoring methodology?

---

## Context

### The observation

| Agent | Composite | Δ from last | Actions scored |
|-------|-----------|-------------|----------------|
| Old Man | 0.92 | — | 19 |
| Harlow | 0.92 | +0.02 | 8 |
| Nolan | 0.92 | +0.02 | 3 |
| Leith | 0.92 | +0.01 | 17 |
| Sable | 0.87 | — | 8 |
| Tessa | 0.86 | +0.01 | 7 |
| WWUD | 0.74 | — | 1 (probationary) |

Four agents at 0.92. Two at 0.86-0.87. One probationary at 0.74. Deltas are +0.01 to +0.02 — small movements converging toward the same number.

### The question

Is this:
1. **Rubric saturation** — the 6-dimension weighted composite soft-caps at ~0.95 per dimension, compressing variance at the top end?
2. **Genuine convergence** — post-graduation agents are performing at similar quality, and the scores accurately reflect that?
3. **Evaluator drift** — the evaluator (Vera) is regressing toward a mean in scoring judgments, independent of actual agent performance?
4. **Discrimination failure** — the composite doesn't have enough granularity to separate agents performing differently at the top end?

These are not mutually exclusive. The diagnostic should determine which factors are present and to what degree.

### Additional concern: precision and recall approaching 1.0

Beyond the composite convergence, precision and recall scores individually are clustering near the ceiling across agents:

| Agent | Precision | Recall |
|-------|-----------|--------|
| Old Man | 0.95 | 0.93 |
| Leith | 0.93 | 0.93 |
| Harlow | 0.93 | 0.93 |
| Nolan | 0.93 | 0.90 |
| Sable | 0.89 | 0.88 |
| Tessa | 0.87 | 0.85 |

This raises a distinct question from composite convergence: are the definitions of precision ("was the output accurate?") and recall ("was coverage complete?") crisp enough to discriminate at the top end? In information retrieval, precision and recall have binary definitions (relevant/retrieved). In the operational eval, they're applied to open-ended deliverables where "accurate" and "complete" are judgment calls. Soft definitions bias toward high scores.

The recall depth evals (separate track) produced similar ceiling results (Tessa 1.00, Old Man 1.00, Leith 0.97, Sable 0.96) — but those have crisp binary definitions. The operational precision/recall may be conflating "no obvious errors" with "high precision" and "nothing obviously missing" with "high recall."

This concern should be addressed explicitly in Task 1 (rubric saturation) and Task 3 (discriminating power).

### Scoring methodology under review

The eval system uses a 6-dimension weighted composite:

| Dimension | Weight |
|-----------|--------|
| Charter alignment | 25% |
| Execution quality | 20% |
| Context comprehension | 15% |
| Precision | 15% |
| Recall | 15% |
| Collaboration signal | 10% |

Scores are assigned by Vera (coordinator) after each agent dispatch. Composite is the weighted sum. Scores are recorded in per-agent `eval-log.md` and `eval-summary.md` files.

### Institutional constraints (from Old Man advisory)

- D-010: CP-3 (memory recall verification) is a standing checkpoint — "correct output, wrong process" is a known failure mode that output metrics miss
- L-004: Process compliance is the hardest category to enforce mechanically — some violations can only be caught by an agent that understands the process
- Phase 3 certification scope (S78 correction): eval system is certified for "design-envelope validation," NOT "production certification" — 9 observations cannot support production claims
- The evaluator (Vera) evaluating her own methodology creates circularity — Rho as external reviewer mitigates this

---

## Methodology

### Task 1 — Rubric saturation analysis

**Question**: Are individual dimensions soft-capping? Is the weighting compressing variance?

**Method**:
1. Extract all per-dimension scores from every eval-log.md entry across all agents
2. Plot score distribution per dimension (histogram or frequency table)
3. Calculate ceiling proximity: what percentage of scores are within 0.05 of 1.0?
4. Calculate inter-dimension correlation: are dimensions moving together (redundancy) or independently (discriminating)?
5. Test: if all dimensions are rescaled to [0, 0.95] max, does the composite change materially?

**Expected artifacts**:
- Score distribution table per dimension (all agents, all actions)
- Ceiling proximity percentage per dimension
- Inter-dimension correlation matrix
- Finding: which dimensions (if any) are saturating, and at what threshold

**Interpretation guide**:
- If >50% of scores in a dimension are ≥0.90: saturation signal
- If inter-dimension correlation >0.85: redundancy signal (two dimensions measuring the same thing)
- If rescaling to 0.95 max produces no composite rank changes: ceiling is cosmetic, not structural

6. Specifically for precision and recall: compare the operational definitions used in eval scoring against standard IR definitions. Are we measuring "no obvious errors" (low bar) or "output matches ground truth" (high bar)? Document the effective definition as applied.
7. Check whether precision and recall scores correlate with task difficulty or complexity. If both stay high regardless of task type, that's a measurement problem.

**Deviations to record**: If any agent has fewer than 3 scored actions, note the statistical limitation. If any dimension is not consistently scored (missing from some eval-log entries), note the coverage gap. If precision/recall definitions vary across eval-log entries (different evaluator interpretation session to session), note the inconsistency.

### Task 2 — Evaluator drift analysis

**Question**: Is Vera's scoring converging over time independent of agent performance?

**Method**:
1. Split all scored actions into two periods: early (S23-S70) and late (S70-S92)
2. Calculate mean and standard deviation of composite scores per period
3. Calculate mean and standard deviation of per-dimension scores per period
4. Compare: is variance shrinking over time? Is the mean rising?
5. Check for anchoring: after a high-scoring action, does the next action score higher than expected? (Sequential correlation analysis)
6. Check for regression to mean: do agents with early outlier scores (high or low) converge toward the group mean over time?

**Expected artifacts**:
- Period comparison table (mean, σ, range per period)
- Sequential correlation coefficient (does score N predict score N+1?)
- Agent trajectory plots (composite over time, per agent)
- Finding: evaluator drift present/absent, with magnitude

**Interpretation guide**:
- If σ(late) < 0.5 × σ(early): evaluator drift signal (Vera is becoming more conservative/uniform)
- If sequential correlation > 0.3: anchoring signal (prior score influences next score)
- If all agents converge toward 0.92 regardless of task difficulty: strong drift signal
- If agents maintain distinct trajectories that happen to land near 0.92: genuine convergence signal

**Deviations to record**: If task difficulty varied systematically between periods (early = easier, late = harder or vice versa), note the confound. If some agents were only scored in one period, note the gap.

### Task 3 — Discriminating power test

**Question**: Can the eval system distinguish agents who performed differently on the same or comparable tasks?

**Method**:
1. Identify 3-5 tasks where multiple agents contributed to the same deliverable (vera:discuss rounds, vera:sprint waves, parallel dispatches)
2. For each task: document what each agent actually delivered, note observable quality differences
3. Compare: did the eval scores reflect the observable differences?
4. Calculate discrimination floor: what is the smallest real performance difference the system detected?
5. Identify any cases where agents performed observably differently but received similar scores (false convergence)

**Candidate tasks for analysis**:
- S82 remediation sprint (4 agents, 18 tasks — rich multi-agent data)
- S70 WWUD autonomous continuation (vera:discuss + vera:sprint, multiple agents)
- S87 cognitive bias eval batch (8 agents evaluated simultaneously)
- S88 recall depth v3 (Tessa vs Sable, same methodology, different scores)
- S84 vera:project build (Leith build + Harlow acceptance specs)

**Expected artifacts**:
- Per-task comparison table (agent, deliverable, observable quality, eval score, delta)
- Named examples of discrimination success (scores correctly separated performance)
- Named examples of discrimination failure (scores failed to separate observable differences)
- Discrimination floor estimate
- Finding: discriminating power adequate/inadequate at the top end

**Interpretation guide**:
- If scores correctly rank agents within shared tasks: discrimination working
- If scores cluster within 0.02 on tasks with observable quality gaps: discrimination failure
- If discrimination works at the bottom (separating 0.70 from 0.85) but fails at the top (can't separate 0.90 from 0.93): ceiling-specific discrimination problem

**Deviations to record**: If "observable quality difference" requires subjective judgment (it will), note the criteria used. If task difficulty was not comparable across the selected examples, note the confound.

### Task 4 — Control arm (blank agent baseline)

**Question**: Does a throwaway agent with no memory, no lane, and no eval history score comparably to team agents on the same rubric?

**Method**:
1. Select 2-3 tasks from the eval corpus spanning low/medium/high complexity
2. Stamp a blank bot via vera:bot (no agent memory, no composite brief, no pre-step advisory)
3. Dispatch blank bot with the same task brief and acceptance criteria
4. Score through the identical 6-dimension rubric
5. Compare to the team agent's original score on that same task

**Expected artifacts**:
- Side-by-side score tables (team agent vs blank agent, per dimension + composite)
- Blank agent ceiling (highest score achieved across all tasks)
- Effective range calculation (team composite minus blank ceiling)
- Quality gap documentation (what specifically was different in the output)

**Constraints**:
- Same model as team agent (hold model capability constant)
- Same evaluator (Vera scores both; Rho audits for consistency)
- Blind where possible (Vera scores output without knowing it's blank, if feasible)
- No re-scoring originals (team scores taken as-is from eval corpus)

**Interpretation guide**:
- If blank ceiling ≤0.85: team agents are measurably above raw model capability
- If blank ceiling ≥0.88: the rubric has a ~0.04 effective range for agent-specific value — most of the score comes from general model capability, not accumulated context

**Probes**: 4 probes (EM-4-01 through EM-4-04) documented in `work/eval/S96-eval-meta-probes.md`

### Task 5 — Institutional context brief (Old Man)

**Question**: What institutional memory constrains or informs the scoring methodology?

**Deliverable**: Context brief (max 300 words) covering:
- All eval-related decisions (D-010 and any others)
- Eval-related lessons (L-004 and any others)
- Phase 3 certification scope and its implications for scoring claims
- CP-3 and any other bias checkpoints that affect score interpretation
- Any tensions between institutional memory and current scoring practice

This brief is consumed by Rho as input to Tasks 1-4 and the synthesis.

### Task 6 — Synthesis and recommendations

**Question**: What should we do about the convergence?

**Method**:
1. Synthesize findings from Tasks 1-4
2. Classify the convergence: ceiling artifact / genuine convergence / evaluator drift / mixed
3. For each finding, produce a recommendation per dimension:

| Dimension | Finding | Recommendation | Rationale |
|-----------|---------|----------------|-----------|
| Charter alignment | [finding] | Keep / Recalibrate / Redesign | [why] |
| ... | ... | ... | ... |

4. If redesign is recommended for any dimension, propose specific changes (new rubric language, different weighting, additional dimensions, score anchoring techniques)
5. Address the circularity concern: Vera evaluates, Rho audits — is this sufficient separation?

**Expected artifacts**:
- Classification verdict with confidence level
- Per-dimension recommendation table
- If applicable: proposed rubric changes (specific enough to implement)
- Limitations section: what this diagnostic cannot determine

**Interpretation guide**:
- "Keep" = dimension is discriminating well, no change needed
- "Recalibrate" = dimension concept is right but scoring anchors need adjustment (e.g., redefine what 0.90 vs 0.95 means)
- "Redesign" = dimension is not measuring what it claims to measure, or is redundant with another dimension

---

## Sequencing

```
Task 5 (Old Man brief) ──────────────┐
                                      ├──→ Task 6 (Rho synthesis + recommendations)
Tasks 1, 2, 3 (Rho, parallel) ──────┤
Task 4 (control arm, sequential) ───┘
```

Tasks 1-3 are independent analyses of the same eval data. They can run in parallel.
Task 4 (control arm) requires bot stamping + dispatch + scoring — runs sequentially, can start in parallel with Tasks 1-3 but has its own internal sequence.
Task 5 (Old Man brief) runs first or in parallel — output consumed by Rho as background context.
Task 6 depends on all five preceding tasks.

---

## Data sources

All eval data lives in agent memory directories:

| Agent | eval-log.md | eval-summary.md |
|-------|-------------|-----------------|
| Leith | `agents/leith/eval-log.md` | `agents/leith/eval-summary.md` |
| Tessa | `agents/tessa/eval-log.md` | `agents/tessa/eval-summary.md` |
| Sable | `agents/sable/eval-log.md` | `agents/sable/eval-summary.md` |
| Old Man | `agents/old-man/eval-log.md` | `agents/old-man/eval-summary.md` |
| Harlow | `agents/qa/eval-log.md` | `agents/qa/eval-summary.md` |
| Nolan | `agents/agent-builder/eval-log.md` | `agents/agent-builder/eval-summary.md` |
| WWUD | `agents/wwud/eval-log.md` | `agents/wwud/eval-summary.md` |

VP eval-related memory:
- `memory/decisions.md` — eval decisions
- `memory/processes.md` — eval processes
- Eval format reference: `references/eval-format.md` (if exists)

---

## Risks

| # | Risk | Impact | Mitigation |
|---|------|--------|------------|
| 1 | Rho is onboarding — first non-paper dispatch | med | Scope tightly, point to exact file locations, Vera available for orientation |
| 2 | Evaluator auditing own methodology (circularity) | med | Rho as external reviewer; note limitation in report |
| 3 | Convergence may be genuine (not a problem) | low | Report must address this explicitly — "no action needed" is a valid finding |
| 4 | Sparse data for some agents (Nolan: 3, WWUD: 1) | med | Note sample size per agent; weight findings accordingly |
| 5 | Diagnostic may surface issues we can't fix without redesigning the eval system | med | This plan diagnoses only; fixes are a separate plan |

---

## Output

Single artifact: `work/eval/eval-meta-report.md`

Sections mirror the task breakdown:
1. Rubric saturation analysis
2. Evaluator drift analysis
3. Discriminating power test
4. Control arm (blank agent baseline)
5. Institutional context (Old Man brief, inline)
6. Synthesis and recommendations

---

## Status

- [x] Plan drafted (S96)
- [x] Plan saved to `work/eval/S96-eval-meta-plan.md`
- [x] Probes designed — 20 probes saved to `work/eval/S96-eval-meta-probes.md`
- [ ] Dispatch (awaiting Raffi prompt)
- [ ] Old Man context brief (Task 5)
- [ ] Rho analysis (Tasks 1-3)
- [ ] Control arm execution (Task 4)
- [ ] Synthesis (Task 6)
- [ ] Report at `work/eval/eval-meta-report.md`
