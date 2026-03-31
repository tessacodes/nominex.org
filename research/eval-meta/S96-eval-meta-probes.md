# Eval-Meta — Probe Inventory

Probes for diagnosing whether eval score convergence is genuine or a measurement artifact.
Each probe is a specific, answerable question with an expected result range and interpretation.

Designed before execution. Rho runs them against the eval corpus.

---

## Probe Format

Each probe specifies:
- **ID**: EM-[task#]-[seq] (e.g., EM-1-01)
- **Question**: The specific analytical question
- **Data source**: Which files/fields to read
- **Method**: How to extract the answer
- **Expected if healthy**: What the result looks like if the eval system is working
- **Expected if artifact**: What the result looks like if the convergence is a measurement problem
- **Record**: Where to log the actual finding

---

## Task 1 Probes — Rubric Saturation

### EM-1-01: Dimension ceiling proximity

**Question**: What percentage of per-dimension scores fall within 0.05 of 1.0 (i.e., ≥0.95)?

**Data source**: All eval-log.md files (7 agents). Extract per-dimension scores from the Notes column (format: `charter:X collab:X precision:X recall:X`).

**Method**: For each of the 6 dimensions, count scores ≥0.95 as a fraction of total scores in that dimension.

**Expected if healthy**: <30% of scores in any dimension are ≥0.95. Scores spread across 0.70–0.95 range.

**Expected if artifact**: >50% of scores in one or more dimensions are ≥0.95. The dimension is soft-capping.

**Record**: Table — dimension, total scores, count ≥0.95, percentage, verdict (saturating/healthy).

---

### EM-1-02: Dimension score distribution shape

**Question**: For each dimension, what is the score distribution? Is it normal, left-skewed (ceiling-compressed), or bimodal?

**Data source**: Same as EM-1-01.

**Method**: For each dimension, bucket scores into 0.05-wide bins (0.50-0.54, 0.55-0.59, ... 0.95-1.00). Report counts per bin.

**Expected if healthy**: Roughly normal distribution centred around 0.85-0.90 with tails in both directions.

**Expected if artifact**: Left-skewed — long tail toward lower scores, sharp pile-up at 0.90-0.95, few or no scores above 0.95 (soft ceiling) or many at exactly 0.95 (hard ceiling).

**Record**: Frequency table per dimension. Shape classification (normal / left-skewed / bimodal / uniform).

---

### EM-1-03: Inter-dimension correlation

**Question**: Do dimensions move together (measuring the same thing) or independently (measuring different things)?

**Data source**: Same as EM-1-01. Need paired scores — all dimensions from the same eval-log entry.

**Method**: For each pair of dimensions (15 pairs from 6 dimensions), calculate Pearson correlation across all scored actions where both dimensions are recorded.

**Expected if healthy**: Most correlations <0.70. Some natural correlation expected (charter and execution tend to co-occur) but not universal.

**Expected if artifact**: Multiple pairs >0.85. Indicates redundant dimensions — two scores measuring the same underlying quality. Reduces effective dimensionality of the composite.

**Record**: 6×6 correlation matrix. Flag any pair >0.80.

---

### EM-1-04: Precision definition consistency

**Question**: Does "precision" mean the same thing across eval-log entries, or does it shift?

**Data source**: All eval-log.md files. Read the Notes column and the observation/context for entries where precision is scored.

**Method**: For each scored action, extract what precision was measured against:
- (a) Binary factual accuracy (output correct or not)
- (b) Absence of hallucination (no false claims)
- (c) Absence of obvious errors (low bar — nothing wrong)
- (d) Match against ground truth (high bar — output matches expected)
- (e) Cannot determine from the log entry

Count frequency of each definition type.

**Expected if healthy**: Consistent definition (mostly one type, applied uniformly).

**Expected if artifact**: Mixed definitions, or predominantly type (c) — "nothing obviously wrong" scores high by default because the bar is absence-of-error rather than presence-of-correctness.

**Record**: Definition type frequency table. Dominant type identified. Consistency verdict.

---

### EM-1-05: Recall definition consistency

**Question**: Does "recall" mean the same thing across eval-log entries, or does it shift?

**Data source**: Same as EM-1-04.

**Method**: For each scored action, extract what recall was measured against:
- (a) Coverage of explicit acceptance criteria (checkable list)
- (b) Coverage of the problem space (judgment call — did they address everything relevant?)
- (c) Absence of obvious gaps (low bar — nothing missing)
- (d) Completeness against a spec or requirements doc (high bar)
- (e) Cannot determine from the log entry

Count frequency of each definition type.

**Expected if healthy**: Consistent definition, predominantly (a) or (d) — measured against a concrete standard.

**Expected if artifact**: Predominantly (b) or (c) — judgment-based, biases toward "nothing obviously missing" = high recall.

**Record**: Definition type frequency table. Dominant type identified. Consistency verdict.

---

### EM-1-06: Weight sensitivity test

**Question**: If we change the dimension weights, do agent rankings change?

**Data source**: Per-dimension scores from EM-1-01.

**Method**: Recompute composite scores under 3 alternative weighting schemes:
- **Equal weight**: all 6 dimensions at 16.7%
- **Precision-heavy**: precision 30%, recall 30%, others 10% each
- **Charter-light**: charter 10%, execution 25%, precision 20%, recall 20%, context 15%, collab 10%

Compare agent rankings under each scheme to the current ranking.

**Expected if healthy**: Rankings shift under different schemes. The weighting matters — different emphasis produces different orderings.

**Expected if artifact**: Rankings don't change regardless of weighting. Indicates all dimensions are telling the same story — the composite adds no information beyond any single dimension.

**Record**: Agent ranking table under each weighting scheme. Rank changes noted. Verdict: weight-sensitive (healthy) or weight-invariant (artifact).

---

## Task 2 Probes — Evaluator Drift

### EM-2-01: Variance compression over time

**Question**: Is the spread of scores shrinking over sessions?

**Data source**: All eval-log.md files. Extract composite scores with session numbers.

**Method**: Split scored actions into two periods — early (≤S70) and late (>S70). Calculate mean and standard deviation of composite scores per period.

**Expected if healthy**: σ(late) ≥ 0.7 × σ(early). Some variance reduction is natural (agents improve), but the spread should not collapse.

**Expected if artifact**: σ(late) < 0.5 × σ(early). The evaluator is scoring in a narrower band over time, independent of actual performance variation.

**Record**: Period comparison table (n, mean, σ, range, min, max). Variance ratio. Verdict.

---

### EM-2-02: Sequential correlation (anchoring)

**Question**: Does the previous score influence the next score? (Within the same agent.)

**Data source**: Per-agent eval-log.md. Extract composite scores in chronological order.

**Method**: For each agent with ≥4 scored actions, calculate the Pearson correlation between score(n) and score(n+1). Also calculate across the full corpus (all agents pooled).

**Expected if healthy**: Correlation <0.3. Each action is scored independently.

**Expected if artifact**: Correlation >0.5. The evaluator is anchoring — a high score predicts the next score will be high, regardless of task.

**Record**: Per-agent sequential correlation. Pooled correlation. Verdict.

---

### EM-2-03: Task difficulty vs score relationship

**Question**: Do harder tasks produce lower scores, or does score stay flat regardless of difficulty?

**Data source**: All eval-log.md files. Need the task description and the composite score.

**Method**: Classify each scored action by complexity:
- **Low**: single-file deliverable, clear spec, no dependencies (e.g., a doc update, a config change)
- **Medium**: multi-file deliverable, some judgment required, dependencies exist (e.g., a skill build, a test suite)
- **High**: cross-agent coordination, novel problem, ambiguous requirements (e.g., a vera:discuss proposal, an architecture review, a sprint wave)

Calculate mean composite score per complexity tier.

**Expected if healthy**: Mean(high) < Mean(medium) < Mean(low), or at least Mean(high) < Mean(low). Harder tasks should produce more variation and lower average scores.

**Expected if artifact**: Mean scores are flat across complexity tiers (within 0.02). The evaluator is not adjusting for difficulty.

**Record**: Complexity classification per action. Mean score per tier. Spread per tier. Verdict.

---

### EM-2-04: Cross-agent score convergence trajectory

**Question**: Are agents converging toward the same score over time, or maintaining distinct trajectories?

**Data source**: Per-agent eval-log.md. Composite scores over time.

**Method**: Plot (or tabulate) each agent's composite trajectory from baseline to current. Calculate inter-agent σ at each session milestone where ≥3 agents have scores.

**Expected if healthy**: Inter-agent σ remains >0.03 over time. Agents maintain distinct levels reflecting their different lanes and capabilities.

**Expected if artifact**: Inter-agent σ shrinks to <0.02 over the last 3+ measurement points. All agents are converging to the same number regardless of lane.

**Record**: Inter-agent σ at each measurement point. Trajectory description per agent. Verdict.

---

### EM-2-05: Score-vs-rework correlation

**Question**: Do lower-scoring actions actually require more rework or correction than higher-scoring ones?

**Data source**: Eval-log.md entries plus the actual deliverables (check if corrections were needed post-delivery).

**Method**: For each scored action, determine: was the deliverable accepted as-is, or did it require correction/rework/follow-up? Cross-reference with the score. If low scores don't predict rework and high scores don't predict clean delivery, the scores aren't tracking real quality.

**Expected if healthy**: Actions scoring <0.85 have a higher rework rate than actions scoring >0.90.

**Expected if artifact**: No correlation between score and rework. Scores are decorative.

**Record**: Score bucket (≤0.85, 0.86-0.90, ≥0.91) × rework (yes/no) contingency table. Verdict.

---

## Task 3 Probes — Discriminating Power

### EM-3-01: Same-task agent comparison (S82 remediation sprint)

**Question**: In the S82 remediation sprint (4 agents, 18 tasks), did scores reflect observable differences in delivery quality?

**Data source**: Eval-log.md entries for S82 actions (Leith, Tessa, Sable, Harlow). Plus the actual deliverables referenced in the eval notes.

**Method**:
1. List each agent's S82 task(s), score, and deliverable
2. Rank deliverables by observable quality (scope, completeness, rework needed, acceptance)
3. Compare the quality ranking to the score ranking
4. Note the score spread across agents for this single sprint

**Expected if healthy**: Score ranking matches quality ranking. Spread ≥0.05 across agents.

**Expected if artifact**: Scores cluster within 0.03. Quality differences exist but scores don't reflect them.

**Record**: Agent × task × score × quality-rank table. Rank correlation. Spread. Verdict.

---

### EM-3-02: Same-task agent comparison (S70 WWUD sprint)

**Question**: In the S70 WWUD autonomous continuation sprint, did scores separate agents performing different roles?

**Data source**: Eval-log.md entries for S70 vera:discuss + vera:sprint actions.

**Method**: Same as EM-3-01 but for the S70 multi-agent sprint. Compare deliberation contributions (proposer vs responder) and sprint execution (wave deliverables).

**Expected if healthy**: Proposers and responders scored differently if their contributions differed in quality. Sprint deliverables scored based on what was actually built.

**Expected if artifact**: All contributions scored within 0.02 regardless of role or quality.

**Record**: Same format as EM-3-01.

---

### EM-3-03: Cross-lane difficulty comparison

**Question**: Are agents in different lanes (infrastructure vs marketing vs QA vs institutional memory) scored on comparable difficulty, or does lane assignment create a scoring advantage?

**Data source**: All eval-log.md files. Agent lane from agents.md.

**Method**: Group all scored actions by agent lane. Compare mean scores and variance per lane. Check: does a "easier" lane (e.g., narrow scope, clear deliverables) systematically produce higher scores than a "harder" lane (e.g., cross-cutting, judgment-heavy)?

**Expected if healthy**: Lane-level means vary but not systematically. No lane has a structural scoring advantage.

**Expected if artifact**: Some lanes consistently score 0.03-0.05 higher than others, not because of better performance but because their tasks are easier to score high on.

**Record**: Lane × mean score × σ × n table. Lane advantage analysis. Verdict.

---

### EM-3-04: Minimum detectable difference

**Question**: What is the smallest real performance difference the eval system has ever detected?

**Data source**: All eval-log.md files. Look for cases where agents scored differently on comparable tasks.

**Method**: Find the pair of scored actions with the smallest score difference where the evaluator explicitly noted a quality distinction in the notes/observation column. This is the empirical discrimination floor.

**Expected if healthy**: Discrimination floor ≤0.03. The system can detect small but real differences.

**Expected if artifact**: Discrimination floor >0.05. The system only separates obviously different performances.

**Record**: The specific action pair, scores, notes, and the delta. Verdict.

---

### EM-3-05: Precision/recall as discriminators at the top end

**Question**: Among agents scoring ≥0.90 composite, do precision and recall scores vary enough to distinguish performance?

**Data source**: Per-dimension scores from agents with composite ≥0.90 (Old Man, Harlow, Nolan, Leith).

**Method**: Extract all precision and recall scores for these 4 agents. Calculate the range and σ within this group. Check: do precision and recall scores separate these agents, or do they cluster within measurement noise (≤0.02)?

**Expected if healthy**: Range ≥0.05 and σ ≥0.03 within the top group. Precision and recall still discriminate.

**Expected if artifact**: Range ≤0.03 and σ ≤0.02. Precision and recall have lost discriminating power at the top end — all top agents score nearly identically on these dimensions.

**Record**: Agent × precision × recall table (top group only). Range, σ, verdict.

---

## Task 4 Probes — Control Arm (Blank Agent Baseline)

The sharpest test of the eval system: if a throwaway agent with no memory, no lane, no
eval history scores comparably to the team on the same rubric, the rubric is measuring
task completion, not agent quality.

### Design

Stamp a blank bot via vera:bot. No agent memory loaded. No composite brief institutional
role. No pre-step advisory. Just the task brief and the model's general capability.

Pick 2-3 tasks from the existing eval corpus that span different complexity tiers:
- One **low complexity** task (clear spec, single deliverable)
- One **medium complexity** task (multi-file, judgment required)
- One **high complexity** task (cross-cutting, ambiguous requirements)

Run the blank agent on each task. Score through the identical 6-dimension rubric.
Compare to the team agent's score on that same task.

### Task selection criteria

Tasks must be:
- Already scored in the eval corpus (so we have the team agent's score as comparison)
- Reproducible (the brief and acceptance criteria are recorded, not just verbal)
- Representative of the task types agents actually do

**Candidate tasks** (to be confirmed during execution — Rho selects from eval-log entries):
- Low: a doc update or config change from S82 remediation (clear spec, checkable)
- Medium: a vera:discuss contribution (requires judgment, synthesis, domain knowledge)
- High: a sprint wave deliverable from S70 or S84 (multi-file, dependencies, novel problem)

---

### EM-4-01: Blank agent vs team agent — low complexity task

**Question**: On a simple, well-specified task, does a blank agent score within 0.05 of the team agent?

**Data source**: Selected low-complexity task from eval corpus. Blank bot dispatch + scoring.

**Method**:
1. Select task. Record the original agent, score, and per-dimension scores.
2. Stamp blank bot (vera:bot, no memory injection, no brief roles filled).
3. Dispatch with the same task brief and acceptance criteria.
4. Score through the identical rubric (same evaluator, same dimensions, same weights).
5. Compare composite and per-dimension scores.

**Expected if healthy**: Blank agent scores 0.05-0.15 lower than team agent. The gap reflects accumulated context, lane expertise, and institutional knowledge that the blank agent lacks.

**Expected if artifact**: Blank agent scores within 0.03 of team agent. The rubric is measuring "did the model complete the task" not "did the agent's accumulated context improve the output."

**Record**: Side-by-side score table (team agent vs blank agent, all 6 dimensions + composite). Delta per dimension. The specific quality differences observed (if any). Verdict.

---

### EM-4-02: Blank agent vs team agent — medium complexity task

**Question**: On a judgment-heavy task, does accumulated agent context produce measurably better output?

**Data source**: Selected medium-complexity task. Blank bot dispatch + scoring.

**Method**: Same as EM-4-01 but medium complexity. The hypothesis: agent context matters more here — lane expertise and institutional memory should produce qualitatively different output.

**Expected if healthy**: Blank agent scores 0.10-0.20 lower. Gap is wider than low-complexity because judgment tasks benefit more from accumulated context. Charter alignment and context comprehension dimensions show the largest gaps.

**Expected if artifact**: Blank agent scores within 0.05. Even on judgment tasks, the model's general capability is sufficient — accumulated context adds little measurable value.

**Record**: Same format as EM-4-01. Note which dimensions show the largest/smallest gaps.

---

### EM-4-03: Blank agent vs team agent — high complexity task

**Question**: On a cross-cutting, ambiguous task, is the gap between blank and team agent largest?

**Data source**: Selected high-complexity task. Blank bot dispatch + scoring.

**Method**: Same as EM-4-01 but high complexity. This is the strongest test — if accumulated context and institutional memory matter, they matter most here.

**Expected if healthy**: Blank agent scores 0.15-0.25 lower. Gap is largest of the three. Collaboration signal and charter alignment dimensions degrade most (blank agent has no team context, no lane to align with).

**Expected if artifact**: Blank agent scores within 0.08. The rubric cannot distinguish between a seasoned team agent and a fresh model on the hardest tasks.

**Record**: Same format as EM-4-01. Specific quality gaps documented in detail — this is the most informative comparison.

---

### EM-4-04: Blank agent score ceiling

**Question**: What is the highest score a blank agent achieves across the three tasks?

**Data source**: Results from EM-4-01 through EM-4-03.

**Method**: Take the maximum composite score from the three blank agent runs. This is the rubric's "general capability floor" — the score any competent model achieves without agent-specific context.

**Expected if healthy**: Blank agent ceiling ≤0.85. Team agents scoring 0.90+ are measurably above what raw model capability produces.

**Expected if artifact**: Blank agent ceiling ≥0.88. The 0.92 team composite is only 0.04 above what a blank model achieves — the eval system has a ~0.04 effective range for measuring agent-specific value.

**Record**: Blank agent scores across all three tasks. Maximum score. The effective range (team composite minus blank ceiling). Verdict.

---

### Control arm constraints

- **Same model**: Blank agent uses the same model as the team agent being compared (typically sonnet for dispatched work). Model capability must be held constant.
- **Same evaluator**: Vera scores both. Rho audits both scores for consistency.
- **Blind where possible**: If feasible, Vera scores the blank agent output without knowing it's blank. If not feasible (Vera stamps the bot), Rho notes the potential bias.
- **No re-scoring originals**: Team agent scores are taken as-is from the eval corpus. Do not re-score them — that would introduce evaluator drift between the original and the comparison.

---

## Execution Notes

### Probe ordering

Probes are grouped by task but execution order within a task is flexible. Rho should complete all probes in a task before moving to synthesis.

### Recording discipline

For each probe, record:
1. The raw data extracted (tables, counts, scores)
2. The finding (which expected result it matched — healthy or artifact)
3. Any deviations from the method (data gaps, judgment calls, ambiguities)
4. Confidence level (high / medium / low) based on sample size and data quality

### Probes that cannot be answered

If a probe cannot be answered due to missing data (e.g., per-dimension scores not recorded in some eval-log entries), record:
- What was missing
- How many entries were affected
- Whether the missing data biases the finding in either direction
- The finding with the caveat noted

### Probe independence

Probes are designed to be independently answerable. A finding on one probe should not change the method for another. Cross-referencing happens in the Task 5 synthesis, not during individual probe execution.

---

## Summary

| Task | Probe count | Focus |
|------|-------------|-------|
| Task 1: Rubric saturation | 6 probes (EM-1-01 through EM-1-06) | Are dimensions soft-capping? Are definitions consistent? |
| Task 2: Evaluator drift | 5 probes (EM-2-01 through EM-2-05) | Is scoring converging over time? Is it tracking real quality? |
| Task 3: Discriminating power | 5 probes (EM-3-01 through EM-3-05) | Can the system separate agents at the top end? |
| Task 4: Control arm | 4 probes (EM-4-01 through EM-4-04) | Does a blank agent score comparably? |
| **Total** | **20 probes** | |

All findings feed into Task 5 (synthesis and recommendations) in the final report at `work/eval/eval-meta-report.md`.
