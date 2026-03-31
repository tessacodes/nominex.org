# Eval-Meta — Diagnostic Report

**Date**: 2026-03-31 (S96)
**Lead**: Rho (research lead)
**Institutional context**: Old Man (pre-step advisory)
**Control arm**: Vera (blank agent dispatch + scoring)
**Commissioned by**: Raffi, after observing 4 agents converging at 0.92 composite

---

## Executive Summary

The 0.92 convergence is **real performance improvement compressed by a measurement system that has lost discriminating power at the top end**. The eval system tracks genuine quality (lower scores predict more rework, rank orderings are correct within sprints) but cannot separate agents once they reach ~0.90. Five specific measurement problems combine to create a ceiling effect: charter dimension saturation, precision-recall entanglement, inconsistent dimension definitions, lane-correlated scoring bias, and cumulative averaging inertia.

The control arm result is the sharpest finding: **a blank agent with zero institutional memory scored within 0.02-0.03 of established team agents on the same tasks**. The eval rubric primarily measures model capability and task completion, not the accumulated value of agent-specific context.

**Verdict**: Mixed — partially genuine convergence, partially measurement artifact. The system needs recalibration, not replacement.

---

## Findings Summary

### 20 probes executed across 4 tasks

| Task | Probes | Healthy | Artifact | Mixed |
|------|--------|---------|----------|-------|
| Task 1: Rubric saturation | 6 | 1 partial | 3 | 2 |
| Task 2: Evaluator drift | 5 | 3 | 1 | 1 |
| Task 3: Discriminating power | 5 | 0 | 2 | 3 |
| Task 4: Control arm | 4 | 0 | 3 | 1 |
| **Total** | **20** | **4** | **9** | **7** |

---

## The Five Measurement Problems

### 1. Charter alignment is saturated (EM-1-01, EM-1-02)

57% of charter scores are ≥0.95. Old Man scores 1.00 on 11 of 14 entries. The dimension has collapsed from a continuous quality measure to a binary "stayed in lane" check — scored on a 0-1 scale but functioning as pass/fail. With 25% weight (highest of any dimension), this inflation lifts every composite.

**Impact**: Inflates all composites by ~0.02-0.03 above where they'd be with a functional charter dimension.

### 2. Precision and recall are one dimension with two names (EM-1-03, EM-1-04, EM-1-05, EM-3-05)

Precision-recall correlation: ~0.82. Among the top 4 agents, precision range is 0.02 (three agents at 0.93, one at 0.95) and recall range is 0.03. The definitions are inconsistent — precision means "absence of hallucination" in 27% of entries, "match against ground truth" in 24%, and "cannot determine" in 27%. Recall similarly splits between judgment-based coverage and checkable criteria.

**Impact**: The composite effectively has 5 dimensions, not 6. "Output quality" (precision+recall) occupies 30% of weight but provides no more discrimination than a single dimension would.

### 3. Lane-correlated scoring bias (EM-3-03)

Infrastructure/QA lanes score 0.04-0.06 higher than narrative/judgment lanes. The mechanism: verifiable tasks (tests pass, PRs merge, validation clean) produce higher precision and recall scores because the evaluator has concrete criteria. Judgment tasks (governance narratives, deliberation responses) produce lower scores because the evaluator hedges on continuous dimensions.

**Impact**: Tessa and Sable face a structural scoring disadvantage unrelated to performance quality.

### 4. Cumulative averaging inertia (EM-2-04, Old Man brief)

Leith has been at 0.91 for 9 consecutive recalculations. Old Man at 0.92 for 7. A new score of 0.95 on a 15-entry composite moves it by +0.003. A new score of 0.85 moves it by -0.004. The composite formula makes convergence a one-way mathematical process — once scores cluster, they stay clustered regardless of performance variation.

**Impact**: The eval system cannot reward improvement or penalise regression for established agents. Score is frozen by denominator size.

### 5. Task difficulty insensitivity (EM-2-03)

Harder tasks score *higher*, not lower. High-complexity sprints: mean 0.929, σ=0.016. Low-complexity tasks: mean 0.847, σ=0.083. Fifteen high-complexity actions spanning 5 agents and 3 sprints all score 0.90-0.95. The evaluator measures "did they complete it?" (binary completion against acceptance criteria) rather than adjusting for task difficulty.

**Impact**: The effective scoring range for competent agents on sprint tasks is 0.90-0.95 — a 0.05 band.

---

## Control Arm Results

### EM-4-01: Low complexity — Validator bug analysis

| | Leith (original) | Blank agent |
|---|---|---|
| Bugs found | 3 | 3 |
| Correct NOT_A_BUG | 2 | 1 |
| Key miss | H1 parsing root cause (caught by Harlow) | V3 hash target guard, M1 release dependency |
| Key catch | — | **Found the H1 bug Leith missed** |

The blank agent matched Leith's bug count and found the specific miss (H1 parsing) that dropped Leith's recall from 0.92 to 0.85. Different bugs found, same overall coverage.

**Estimated blank agent score**: ~0.88
**Leith's original score**: 0.90
**Delta**: 0.02

### EM-4-02: Medium complexity — Deliberation response

| | Sable (original) | Blank agent |
|---|---|---|
| Failure modes | 4 | 7 |
| Trust model analysis | 1 (trust shift) | 3 (reactive→proactive, self-determined activation, probationary risk) |
| Assumptions challenged | 1 (silence = absence) | 5 |
| Recommendations | 1 (minimum viable alternative) | 6 |

The blank agent produced a more comprehensive, more structured analysis than Sable's 0.95-scored response — with zero institutional memory, zero lane specialisation, zero team context.

**Estimated blank agent score**: ~0.92
**Sable's original score**: 0.95
**Delta**: 0.03

### EM-4-04: Blank agent ceiling

| Probe | Blank score | Team score | Delta |
|-------|-------------|------------|-------|
| EM-4-01 (low) | ~0.88 | 0.90 | 0.02 |
| EM-4-02 (medium) | ~0.92 | 0.95 | 0.03 |

**Blank agent ceiling**: 0.92
**Team composite range**: 0.86-0.92
**Effective range**: 0.00-0.04

The blank agent's ceiling (0.92) equals the team composite for 4 of 6 active agents. The eval system has approximately 0.04 of effective range for measuring agent-specific value above baseline model capability. On the medium-complexity task, the blank agent arguably outperformed the original agent's output while scoring only 0.03 lower — suggesting even the 0.03 gap may be evaluator bias toward the known team agent rather than genuine quality difference.

### Control arm interpretation

The rubric primarily measures: **Can the model complete this task competently?** The answer is yes for any task within the model's capability, regardless of whether the agent has accumulated memory, lane expertise, or institutional context.

What the rubric does not measure (but should, if agent-specific value is the goal):
- Did the agent draw on institutional memory to avoid known pitfalls?
- Did the agent cite prior decisions or lessons?
- Did the agent's output reflect accumulated team context?
- Did the agent's approach differ because of what it has learned?

These are CP-3 (memory recall verification) properties — the very failure mode Old Man flagged as "correct output, wrong process." The eval system rewards correct output. It does not reward (or even measure) the process of drawing on accumulated knowledge.

---

## Healthy Signals (What's Working)

Not everything is broken. Three findings confirm the eval system has real measurement capacity:

1. **Score-vs-rework correlation (EM-2-05)**: 33% rework rate at ≤0.85, 20% at 0.86-0.90, 4% at ≥0.91. Lower scores predict lower quality. The scores track something real.

2. **No sequential anchoring (EM-2-02)**: Sharp score reversals (0.94→0.87, 0.95→0.76) demonstrate the evaluator scores each action independently. No anchoring to previous scores.

3. **Correct rank ordering (EM-3-01, EM-3-02)**: Within shared tasks, agents with observably better output score higher. The directional discrimination works — the magnitude compression is the problem.

---

## Recommendations

### Per-Dimension Recommendations

| Dimension | Finding | Recommendation | Rationale |
|-----------|---------|----------------|-----------|
| Charter alignment | Saturated (57% at ceiling) | **Redesign** → binary gate | It already functions as pass/fail. Make it explicit. Remove from weighted composite. Apply as a prerequisite: if charter alignment fails, cap composite at 0.75. |
| Execution quality | Healthy distribution | **Keep** | Meaningful spread (0.65-0.95). No saturation. |
| Context comprehension | Healthy, best distribution shape | **Keep** | Centred at 0.85-0.89 with meaningful tails. |
| Precision | Inconsistent definition, entangled with recall | **Recalibrate** | Split into two definitions: (a) factual accuracy (verifiable claims correct) and (b) grounded reasoning (drew on institutional knowledge, not just general capability). The second definition adds the CP-3 dimension the system currently lacks. |
| Recall | Inconsistent definition, entangled with precision | **Recalibrate** | Standardise on checkable acceptance criteria where available. For judgment tasks, require the evaluator to enumerate what "complete coverage" means before scoring. |
| Collaboration | Widest spread, most discriminating | **Keep** | 0.60-0.95 range. The only dimension that consistently separates agents. |

### Structural Recommendations

1. **Replace cumulative averaging with a sliding window composite.** Use the last N actions (e.g., N=10) rather than all actions ever. This restores score mobility and prevents denominator-driven freeze. Established agents can improve or regress visibly.

2. **Add a "grounded reasoning" dimension.** The control arm proved that general model capability produces ~0.92 output. The missing dimension is: did the agent's accumulated context improve the output beyond what a blank model would produce? Score this explicitly. This operationalises CP-3 as a scoring dimension rather than a binary checkpoint.

3. **Normalise for task difficulty.** Either score relative to a difficulty-adjusted baseline, or at minimum classify tasks by complexity tier and report scores within tier. Current data shows σ collapsing from 0.083 (low) to 0.016 (high) — the system has almost no variance on sprint tasks.

4. **Standardise precision and recall definitions in the eval-log.** Require the evaluator to state what precision and recall are measured against for each entry. "Precision: 3 claims verified against source" is meaningful. "Precision: 0.92" with no elaboration is not.

5. **Address lane bias explicitly.** Either weight dimensions differently for judgment-heavy lanes, or use lane-specific rubrics. Infrastructure work (verifiable acceptance criteria) and narrative work (judgment-based quality) are different measurement problems.

---

## Limitations

- Correlation estimates in EM-1-03 are approximate (no statistical computation tools available — manual inspection).
- Control arm sample size is N=2 tasks. More tasks would strengthen the finding.
- EM-4-03 (high complexity) was not executed — the S82 sprint was too large for a blank agent run.
- The blank agent scoring was not fully blind — the evaluator (Vera) knew the output was from a blank agent. A blind comparison would be more rigorous.
- Some probes have small N per agent (Nolan: 2 actions, WWUD: 1 scored action). Statistical confidence varies.

---

## Conclusion

The eval system works at the coarse level — it correctly separates good work from bad work, and rank orders agents directionally within shared tasks. But it has lost discriminating power at the top end through a combination of dimension saturation, definition inconsistency, scoring methodology (cumulative averaging), and a rubric that measures task completion rather than agent-specific value.

The control arm is the decisive finding. A blank agent — no memory, no lane, no team context — scores 0.88-0.92 on the same rubric. The team's 0.92 composite is approximately 0.00-0.04 above what raw model capability achieves. The eval system is measuring the model, not the agent.

To measure what agents actually add — institutional knowledge application, accumulated context, grounded reasoning, team coordination — the rubric needs a dimension that specifically tests for these properties. CP-3 (memory recall verification) already identifies this gap conceptually. The recommendation is to operationalise it as a scored dimension: "Did the agent's output demonstrably benefit from its accumulated context?"

---

## Artifacts

| File | Description |
|------|-------------|
| `work/eval/S96-eval-meta-plan.md` | Plan (6 tasks, 20 probes) |
| `work/eval/S96-eval-meta-probes.md` | Probe inventory with expected results |
| `work/eval/S96-eval-meta-control-arm-tasks.md` | Control arm task selections |
| `work/eval/S96-eval-meta-tasks-1-3-findings.md` | Rho's full 16-probe analysis (925 lines) |
| `/tmp/eval-meta-em-4-01-blank-output.md` | Blank agent output: validator bug analysis |
| `/tmp/eval-meta-em-4-02-blank-output.md` | Blank agent output: deliberation response |
| Old Man context brief | Inline (not filed — 278 words, delivered in conversation) |

---

*Report compiled by Vera from Rho's analysis (Tasks 1-3), Old Man's institutional brief (Task 5), and control arm execution (Task 4). 20 probes, 7 agent dispatches, ~40 minutes elapsed.*
