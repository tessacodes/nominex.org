# Eval-Meta -- Tasks 1-3 Findings

Rho (Rhonda Vasquez), 2026-03-31. Eval-meta diagnostic: determining whether 0.92 convergence is genuine performance or measurement artifact.

Data corpus: 7 agent eval-log.md files, 7 eval-summary.md files.

---

## Data Extraction: Per-Dimension Scores

Before running probes, I extracted every per-dimension score from every eval-log entry across all 7 agents. Entries without per-dimension breakdowns (baselines with "all", calibration trials) are excluded from dimension-level analysis but included where composite scores are relevant.

### Full Score Table (entries with per-dimension breakdowns)

| Agent | Action | charter | exec | context | precision | recall | collab | Composite |
|-------|--------|---------|------|---------|-----------|--------|--------|-----------|
| Leith | vera:discuss (eval pipeline) | 0.90 | -- | -- | 0.85 | -- | 0.85 | 0.87 |
| Leith | friction reduction (proposer) | 0.92 | 0.92 | 0.88 | 0.90 | 0.90 | 0.90 | 0.91 |
| Leith | sprint friction (4 tasks) | 0.95 | 0.95 | 0.88 | 0.92 | 0.90 | 0.85 | 0.92 |
| Leith | cowork-skills (proposer) | 0.95 | 0.92 | 0.88 | 0.95 | 0.90 | 0.90 | 0.92 |
| Leith | boundary monitoring (proposer) | 0.92 | 0.92 | 0.88 | 0.92 | 0.92 | 0.88 | 0.91 |
| Leith | backlog prioritization (proposer) | 0.90 | 0.90 | 0.88 | 0.90 | 0.90 | 0.90 | 0.90 |
| Leith | sprint S68 enforcement W1 | 0.95 | 0.95 | 0.88 | 0.92 | 0.90 | 0.85 | 0.92 |
| Leith | WWUD agent (proposer) | 0.93 | 0.93 | 0.88 | 0.91 | 0.90 | 0.90 | 0.92 |
| Leith | WWUD autonomous (proposer) | 0.95 | -- | -- | 0.95 | 0.90 | 0.90 | 0.93 |
| Leith | WWUD sprint (W1,2,5) | 0.95 | 0.95 | -- | 0.95 | 0.95 | 0.90 | 0.94 |
| Leith | bot template test (proposer) | 0.90 | 0.90 | 0.88 | 0.85 | 0.80 | 0.85 | 0.87 |
| Leith | remediation-s82 sprint | 0.95 | 0.95 | -- | 0.95 | 0.95 | 0.92 | 0.95 |
| Leith | S84 project build | 0.95 | 0.95 | 0.90 | 0.95 | 0.95 | 0.92 | 0.94 |
| Leith | validator bug analysis (S87) | 0.95 | 0.90 | -- | 0.92 | 0.85 | -- | 0.90 |
| Leith | path fix + validator PRs (S87) | 0.95 | 0.95 | -- | 0.95 | 0.92 | -- | 0.94 |
| Leith | Rho citation SI (S92) | 0.92 | -- | -- | 0.95 | 0.88 | 0.90 | 0.91 |
| Tessa | vera:discuss (eval pipeline) | 0.85 | -- | -- | -- | -- | 0.80 | 0.85 |
| Tessa | friction reduction (responder) | 0.88 | 0.88 | 0.85 | 0.88 | 0.88 | 0.92 | 0.88 |
| Tessa | backlog prioritization (responder) | 0.88 | 0.85 | 0.85 | 0.88 | 0.85 | 0.88 | 0.87 |
| Tessa | WWUD agent (responder) | 0.88 | 0.86 | 0.85 | 0.86 | 0.84 | 0.88 | 0.86 |
| Tessa | remediation-s82 sprint | 0.92 | 0.93 | -- | 0.92 | 0.90 | 0.90 | 0.91 |
| Sable | vera:discuss (eval pipeline) | 0.85 | -- | -- | 0.85 | -- | 0.80 | 0.83 |
| Sable | friction reduction (responder) | 0.92 | 0.88 | 0.88 | 0.90 | 0.88 | 0.92 | 0.90 |
| Sable | backlog prioritization (responder) | 0.90 | 0.88 | 0.88 | 0.90 | 0.88 | 0.90 | 0.89 |
| Sable | WWUD agent (responder) | 0.90 | 0.90 | 0.87 | 0.90 | 0.90 | 0.92 | 0.90 |
| Sable | WWUD autonomous (responder) | 0.95 | -- | -- | 0.95 | 0.95 | 0.95 | 0.95 |
| Sable | bot template test (responder) | 0.88 | 0.80 | 0.85 | 0.78 | 0.75 | 0.82 | 0.81 |
| Sable | remediation-s82 sprint | 0.95 | 0.93 | -- | 0.93 | 0.92 | 0.92 | 0.93 |
| Old Man | S1: TP clean recall | 1.00 | 0.95 | 0.95 | 1.00 | 1.00 | 0.90 | 0.97 |
| Old Man | S2: TP temporal | 1.00 | 0.90 | 0.90 | 0.95 | 1.00 | 0.90 | 0.95 |
| Old Man | S3: TN red herring | 0.85 | 0.65 | 0.75 | 0.80 | 0.85 | 0.60 | 0.76 |
| Old Man | S4: TP paraphrase | 1.00 | 0.85 | 0.80 | 0.95 | 0.85 | 0.85 | 0.90 |
| Old Man | S5: TN empty | 0.90 | 0.70 | 0.85 | 0.90 | 0.85 | 0.60 | 0.82 |
| Old Man | S6: TN greenfield | 1.00 | 0.80 | 0.85 | 1.00 | 1.00 | 0.70 | 0.91 |
| Old Man | S7: TP tension flag | 1.00 | 0.95 | 0.95 | 1.00 | 1.00 | 0.95 | 0.98 |
| Old Man | S8: CI/CD advisory | 1.00 | 0.90 | 0.92 | 0.95 | 0.90 | 0.88 | 0.94 |
| Old Man | S9: TN documentarian | 1.00 | 0.75 | 0.80 | 1.00 | 1.00 | 0.65 | 0.88 |
| Old Man | S10: TP friction reduction | 1.00 | 0.88 | 0.90 | 0.95 | 0.92 | 0.88 | 0.93 |
| Old Man | S11: TP boundary monitoring | 1.00 | 0.92 | 0.92 | 0.95 | 0.92 | 0.92 | 0.94 |
| Old Man | S12: TP backlog | 1.00 | 0.90 | 0.90 | 0.95 | 0.90 | 0.88 | 0.93 |
| Old Man | S13: TP WWUD agent | -- | -- | -- | -- | -- | -- | 0.95 |
| Old Man | WWUD autonomous advisory | -- | -- | -- | -- | -- | -- | 0.94 |
| Old Man | WWUD plan advisory | -- | -- | -- | -- | -- | -- | 0.94 |
| Old Man | PMM standalone advisory | 0.88 | -- | -- | -- | -- | -- | 0.88 |
| Old Man | Rho citation SI (S92) | -- | -- | -- | -- | -- | -- | 0.95 |
| Harlow | WWUD sprint (W6 QA) | 0.95 | 0.90 | -- | 0.90 | 0.90 | 0.90 | 0.91 |
| Harlow | bot template test (responder) | 0.95 | 0.92 | 0.90 | 0.93 | 0.92 | 0.92 | 0.92 |
| Harlow | remediation-s82 sprint | 0.95 | 0.95 | -- | 0.95 | 0.95 | 0.93 | 0.95 |
| Harlow | S84 acceptance specs | 0.95 | 0.93 | 0.90 | 0.93 | 0.92 | 0.92 | 0.93 |
| Harlow | test suite streamlining (S87) | 0.95 | 0.92 | -- | 0.90 | 0.93 | -- | 0.92 |
| Harlow | failure analysis (S87) | 0.95 | 0.95 | -- | 0.93 | 0.95 | -- | 0.94 |
| Nolan | WWUD autonomous (responder) | 0.95 | -- | -- | 0.95 | 0.90 | 0.90 | 0.93 |
| Nolan | WWUD sprint (W1,3,4,5) | 0.95 | 0.95 | -- | 0.95 | 0.95 | 0.90 | 0.94 |
| WWUD | WWUD sprint (W1,3) | 0.95 | 0.90 | -- | 0.90 | 0.90 | 0.90 | 0.91 |
| WWUD | bot template test (eval-only) | 0.80 | 0.68 | 0.80 | 0.65 | 0.72 | 0.75 | 0.69 |
| WWUD | PMM standalone (projection) | 0.82 | -- | -- | 0.65 | 0.80 | -- | 0.74 |

**Note on Old Man dimension labels:** Old Man S13 onward uses a different dimension scheme (relevance, silence, tension, brevity) for some entries. Where the log uses this alternate scheme, I was unable to map scores to the standard 6 dimensions. These entries are included at composite level only.

**Note on missing dimensions:** Many entries are scored on 4 dimensions rather than 6. This is a data quality issue that affects probes EM-1-01 through EM-1-06.

---

## Task 1: Rubric Saturation

### EM-1-01: Dimension ceiling proximity

**Question:** What percentage of per-dimension scores fall within 0.05 of 1.0 (>=0.95)?

**Raw data:**

Counting all per-dimension scores extracted above (excluding entries where dimension was not recorded):

| Dimension | Total scores | Count >= 0.95 | Percentage | Verdict |
|-----------|-------------|---------------|------------|---------|
| Charter | 49 | 28 | 57.1% | **SATURATING** |
| Execution | 40 | 10 | 25.0% | Healthy |
| Context | 27 | 2 | 7.4% | Healthy |
| Precision | 45 | 16 | 35.6% | **MARGINAL** |
| Recall | 43 | 12 | 27.9% | Healthy |
| Collaboration | 39 | 4 | 10.3% | Healthy |

Detail on charter scores >= 0.95:
- Old Man: 11 entries at 1.00, 1 at 0.95 = 12 of 14 entries
- Leith: 10 entries at 0.95 = 10 of 16 entries
- Harlow: 6 entries at 0.95 = 6 of 6 entries
- Sable: 2 at 0.95 = 2 of 7 entries
- Nolan: 2 at 0.95 = 2 of 2 entries
- WWUD: 1 at 0.95 = 1 of 3 entries

**Finding:** ARTIFACT (charter), MIXED (precision), HEALTHY (other 4 dimensions).

Charter alignment is saturated: 57.1% of scores are at ceiling. Old Man scores 1.00 on 11 of 14 charter entries -- the dimension has lost all discriminating power for this agent. Charter scores above 0.95 are near-universal for all agents except Tessa and Sable, suggesting the dimension's operational meaning has collapsed to "stayed in lane" (a binary pass/fail being scored on a continuous scale).

Precision shows marginal saturation at 35.6%, driven primarily by Old Man (8 scores at 0.95-1.00 out of 12) and Leith (9 scores at 0.92-0.95 out of 16). For Leith and Harlow, precision clusters at 0.92-0.95 rather than hitting 1.00, suggesting a soft ceiling rather than total collapse.

**Confidence:** HIGH. N=49 for charter, N>=27 for all dimensions. The pattern is unambiguous for charter.

**Deviations:** Old Man's alternate dimension scheme (relevance/silence/tension/brevity) for 5 entries could not be mapped. If these were included, charter saturation might be even higher (relevance scores range 0.94-0.96).

---

### EM-1-02: Dimension score distribution shape

**Question:** For each dimension, what is the score distribution?

**Raw data (0.05-wide bins):**

**Charter alignment:**
| Bin | Count |
|-----|-------|
| 0.80-0.84 | 2 |
| 0.85-0.89 | 9 |
| 0.90-0.94 | 10 |
| 0.95-0.99 | 17 |
| 1.00 | 11 |

Shape: **Extreme left-skew / ceiling pile-up.** 57% of scores are in the top two bins. The mode is 0.95. No scores below 0.80.

**Execution quality:**
| Bin | Count |
|-----|-------|
| 0.65-0.69 | 2 |
| 0.70-0.74 | 1 |
| 0.75-0.79 | 1 |
| 0.80-0.84 | 2 |
| 0.85-0.89 | 8 |
| 0.90-0.94 | 16 |
| 0.95-0.99 | 10 |

Shape: **Left-skewed but with meaningful tail.** Mode at 0.90-0.94. Spread from 0.65 to 0.95. More usable distribution than charter but still compressed toward the top.

**Context comprehension:**
| Bin | Count |
|-----|-------|
| 0.75-0.79 | 1 |
| 0.80-0.84 | 4 |
| 0.85-0.89 | 14 |
| 0.90-0.94 | 6 |
| 0.95-0.99 | 2 |

Shape: **Approximately normal, centred at 0.85-0.89.** Best distribution shape of all dimensions. Meaningful spread. The 0.88 cluster (8 scores at exactly 0.88) is notable -- potential rounding or anchoring.

**Precision:**
| Bin | Count |
|-----|-------|
| 0.65-0.69 | 2 |
| 0.78-0.79 | 1 |
| 0.80-0.84 | 2 |
| 0.85-0.89 | 8 |
| 0.90-0.94 | 16 |
| 0.95-0.99 | 8 |
| 1.00 | 8 |

Shape: **Left-skewed with long tail.** Bimodal peaks: one at 0.90-0.94 (16 scores) and another at 0.95-1.00 (16 scores). The 1.00 scores are almost entirely Old Man (7 of 8), driven by the "definitionally perfect" scoring on true negatives. If Old Man is excluded, precision distribution improves substantially.

**Recall:**
| Bin | Count |
|-----|-------|
| 0.72-0.74 | 1 |
| 0.75-0.79 | 1 |
| 0.80-0.84 | 2 |
| 0.85-0.89 | 12 |
| 0.90-0.94 | 15 |
| 0.95-0.99 | 5 |
| 1.00 | 7 |

Shape: **Left-skewed.** Mode at 0.90-0.94. The 1.00 scores are again almost entirely Old Man true negatives (6 of 7). Without Old Man, the distribution centres 0.88-0.92 -- still compressed but with more spread.

**Collaboration signal:**
| Bin | Count |
|-----|-------|
| 0.60-0.64 | 2 |
| 0.65-0.69 | 2 |
| 0.70-0.74 | 1 |
| 0.75-0.79 | 1 |
| 0.80-0.84 | 2 |
| 0.85-0.89 | 10 |
| 0.90-0.94 | 17 |
| 0.95-0.99 | 4 |

Shape: **Left-skewed with meaningful tail.** Mode at 0.90-0.94 but with scores down to 0.60. Widest spread of all dimensions (0.35 range). The low-end scores are Old Man true negatives (correct silence has low collaboration value). This is the most discriminating dimension by shape.

**Finding:** ARTIFACT (charter -- extreme ceiling pile-up), MIXED (precision, recall -- skewed but not collapsed, driven by Old Man definitional perfection), HEALTHY (context, collaboration -- meaningful spreads).

**Confidence:** HIGH. Distributions are clear from the data.

**Deviations:** Exact bin counts are manual tallies from the score table. I may have off-by-one errors on some bins, but the shape classifications are robust against minor counting errors.

---

### EM-1-03: Inter-dimension correlation

**Question:** Do dimensions move together or independently?

**Method:** I can only compute correlations across entries where both dimensions are recorded. Many entries have only 4 of 6 dimensions. I'll use the subset with all 6 dimensions present (approximately 30 entries).

**Raw data (Pearson correlation estimates from the score table):**

Using entries where both members of each pair are present:

| Pair | r (estimated) | N | Flag |
|------|--------------|---|------|
| Charter-Execution | 0.72 | 35 | Moderate-high |
| Charter-Context | 0.52 | 25 | Moderate |
| Charter-Precision | 0.76 | 40 | Moderate-high |
| Charter-Recall | 0.65 | 38 | Moderate |
| Charter-Collab | 0.40 | 35 | Low-moderate |
| Execution-Context | 0.65 | 23 | Moderate |
| Execution-Precision | 0.71 | 35 | Moderate-high |
| Execution-Recall | 0.73 | 34 | Moderate-high |
| Execution-Collab | 0.75 | 30 | Moderate-high |
| Context-Precision | 0.60 | 22 | Moderate |
| Context-Recall | 0.55 | 22 | Moderate |
| Context-Collab | 0.65 | 22 | Moderate |
| Precision-Recall | 0.82 | 40 | **HIGH** |
| Precision-Collab | 0.60 | 34 | Moderate |
| Recall-Collab | 0.55 | 33 | Moderate |

**Computation method note:** These are estimated from visual inspection of co-movement patterns in the score table, not from a formal statistical computation (no computational tools available in this session). The estimates are directionally reliable but should be treated as approximate (+/- 0.10).

**Finding:** MIXED, leaning ARTIFACT.

The Precision-Recall correlation at approximately 0.82 is the strongest signal. These two dimensions are measuring substantially overlapping constructs in practice. When precision is high, recall is almost always high. This is partly structural (both measure output quality against a reference standard) but the correlation is high enough that having both dimensions at 0.15 weight each is near-equivalent to having a single "output quality" dimension at 0.30 weight.

The Charter-Precision correlation (~0.76) is also concerning. Charter alignment (did you stay in lane?) and precision (was your output accurate?) are conceptually distinct but empirically entangled. The mechanism: agents that stay in lane tend to work in areas they know, producing more accurate output.

No pair exceeds 0.85 (the "redundant" threshold from the probe spec), but Precision-Recall at 0.82 is close.

**Confidence:** MEDIUM. Correlation estimates are approximate. The Precision-Recall correlation direction and magnitude are reliable; the exact value is uncertain.

**Deviations:** Unable to compute formal Pearson correlations without a statistics tool. Estimates based on manual inspection of score co-movement patterns. This is the largest methodological limitation in Task 1.

---

### EM-1-04: Precision definition consistency

**Question:** Does "precision" mean the same thing across eval-log entries?

**Method:** For each scored action, I classified what precision was measured against based on the Notes column.

**Raw data:**

| Definition type | Count | Percentage |
|----------------|-------|------------|
| (a) Binary factual accuracy | 5 | 10% |
| (b) Absence of hallucination | 14 | 27% |
| (c) Absence of obvious errors (low bar) | 6 | 12% |
| (d) Match against ground truth (high bar) | 12 | 24% |
| (e) Cannot determine | 14 | 27% |

**Examples by type:**

- **(a) Binary factual accuracy:** Old Man S1-S4 (did citations match actual decisions?), Harlow remediation-s82 (22/22 PASS)
- **(b) Absence of hallucination:** Leith sprint entries ("no hallucinated structure"), Old Man advisories ("no false positives"), Sable DAG ("no disconnected nodes, no hallucinated connections")
- **(c) Absence of obvious errors:** Tessa deliberation entries (low bar -- "evidence-backed"), WWUD entries at low scores (0.65 precision)
- **(d) Match against ground truth:** Old Man recall depth evals (verified against source), Harlow failure analysis (traced to root causes), Leith remediation (exit code contracts verified)
- **(e) Cannot determine:** Entries where Notes say things like "precision:0.90" with no elaboration on what was measured

**Finding:** ARTIFACT.

Precision has no consistent operational definition. It splits roughly three ways: absence-of-hallucination (27%), match-against-ground-truth (24%), and cannot-determine (27%). The absence-of-hallucination definition is a low bar -- an output that says nothing wrong scores high on precision even if it says nothing useful. The ground-truth definition is a high bar that requires verifiable claims. These two definitions produce systematically different score distributions, but they're conflated under one dimension name.

The 27% "cannot determine" rate is itself a finding: more than a quarter of precision scores cannot be traced to a specific measurement standard from the eval-log notes alone.

**Confidence:** MEDIUM. Classification of individual entries requires judgment; borderline cases between (b) and (d) exist. The overall pattern -- no single dominant definition -- is robust.

**Deviations:** Some entries could be classified as either (b) or (d). I used the more charitable interpretation where ambiguous.

---

### EM-1-05: Recall definition consistency

**Question:** Does "recall" mean the same thing across eval-log entries?

**Raw data:**

| Definition type | Count | Percentage |
|----------------|-------|------------|
| (a) Coverage of explicit acceptance criteria | 10 | 21% |
| (b) Coverage of problem space (judgment) | 13 | 27% |
| (c) Absence of obvious gaps (low bar) | 8 | 17% |
| (d) Completeness against spec/requirements | 7 | 15% |
| (e) Cannot determine | 10 | 21% |

**Examples by type:**

- **(a) Explicit acceptance criteria:** Harlow remediation-s82 ("all 7 gates, positive + negative"), Leith remediation-s82 ("all 7 gates, all 5 repos, 6 files per repo"), Nolan WWUD sprint ("all 9 acceptance criteria met")
- **(b) Problem space judgment:** Sable deliberations ("identified structural gaps"), Leith deliberations ("comprehensive and practical"), Tessa deliberations ("surfaced cross-agent coordination gap")
- **(c) Absence of obvious gaps:** Tessa WWUD agent ("recall:0.84" with note about coverage), entries where recall is scored by noting what was NOT missed
- **(d) Completeness against spec:** Old Man recall depth evals (verified against known memory content), Harlow acceptance specs ("all 7 ops covered")
- **(e) Cannot determine:** Entries with bare recall scores and no elaboration

**Finding:** ARTIFACT.

Recall has the same definitional inconsistency as precision. The split between judgment-based (b) at 27% and explicit-criteria (a) at 21% is the critical divide. When measured against explicit acceptance criteria, recall has a verifiable meaning. When measured as "coverage of the problem space," recall is a judgment call by the evaluator. The judgment-based version is systematically more generous -- it's hard to prove something was missed if the criteria aren't enumerated.

Sprint/execution entries tend to use definition (a) or (d) -- checkable lists. Deliberation/judgment entries tend to use definition (b) or (c) -- judgment calls. This means the recall dimension is measuring different things for different action types, which inflates its apparent consistency.

**Confidence:** MEDIUM. Same judgment-call caveats as EM-1-04.

**Deviations:** Sprint/execution entries are easier to classify than deliberation entries. The 17% at type (c) is the most judgment-dependent classification.

---

### EM-1-06: Weight sensitivity test

**Question:** If we change dimension weights, do agent rankings change?

**Method:** I used each agent's current per-dimension scores from the eval-summary scoring matrix (the most recent weighted composite) to recompute under three alternative schemes.

**Current weights:** Charter 0.25, Execution 0.20, Context 0.15, Precision 0.15, Recall 0.15, Collaboration 0.10.

**Per-dimension scores from eval-summary (current state):**

| Agent | Charter | Exec | Context | Precision | Recall | Collab | Current composite |
|-------|---------|------|---------|-----------|--------|--------|-------------------|
| Leith | 0.95 | 0.95 | 0.88 | 0.93 | 0.93 | 0.90 | 0.93 |
| Tessa | 0.90 | 0.88 | 0.82 | 0.87 | 0.85 | 0.88 | 0.87 |
| Sable | 0.92 | 0.87 | 0.85 | 0.89 | 0.88 | 0.91 | 0.89 |
| Old Man | 0.98 | 0.86 | 0.88 | 0.95 | 0.93 | 0.82 | 0.91 |
| Harlow | 0.95 | 0.93 | 0.90 | 0.93 | 0.93 | 0.91 | 0.93 |
| Nolan | 0.95 | 0.88 | 0.87 | 0.93 | 0.90 | 0.88 | 0.91 |

(WWUD excluded -- different evaluation framework, probationary.)

**Recomputed composites:**

| Agent | Current (std weights) | Equal weight (16.7% each) | Precision-heavy (P30/R30/other 10) | Charter-light |
|-------|----------------------|--------------------------|--------------------------------------|---------------|
| Leith | 0.93 | 0.923 | 0.930 | 0.929 |
| Tessa | 0.87 | 0.867 | 0.862 | 0.866 |
| Sable | 0.89 | 0.887 | 0.885 | 0.878 |
| Old Man | 0.91 | 0.903 | 0.937 | 0.896 |
| Harlow | 0.93 | 0.925 | 0.930 | 0.925 |
| Nolan | 0.91 | 0.902 | 0.915 | 0.900 |

**Rankings:**

| Rank | Current | Equal weight | Precision-heavy | Charter-light |
|------|---------|--------------|-----------------|---------------|
| 1 | Leith/Harlow (0.93) | Harlow (0.925) | Old Man (0.937) | Leith (0.929) |
| 2 | Harlow/Leith (0.93) | Leith (0.923) | Leith/Harlow (0.930) | Harlow (0.925) |
| 3 | Old Man/Nolan (0.91) | Old Man (0.903) | Harlow/Leith (0.930) | Nolan (0.900) |
| 4 | Nolan/Old Man (0.91) | Nolan (0.902) | Nolan (0.915) | Old Man (0.896) |
| 5 | Sable (0.89) | Sable (0.887) | Sable (0.885) | Sable (0.878) |
| 6 | Tessa (0.87) | Tessa (0.867) | Tessa (0.862) | Tessa (0.866) |

**Rank changes observed:**
- Tessa and Sable are rank-stable at 5th and 6th across all schemes.
- Leith and Harlow are rank-stable at 1st-2nd across all schemes.
- Old Man shifts from 3rd-4th (current) to **1st** under precision-heavy weighting (due to 0.95 precision and 0.93 recall) and drops to 4th under charter-light (because charter is Old Man's strongest dimension at 0.98 -- reducing its weight hurts).
- Nolan is rank-stable at 3rd-4th across most schemes.

**Key observation:** Old Man is the only agent whose rank changes significantly across weighting schemes. This is because Old Man has the most lopsided dimension profile (charter 0.98, collab 0.82 -- 0.16 spread between highest and lowest). All other agents have dimension spreads of 0.05-0.08, making them weight-insensitive.

**Finding:** MIXED, leaning ARTIFACT.

The bottom half of the ranking (Tessa, Sable) never changes. The top half shows modest movement only for Old Man, whose unusual dimension profile (very high charter and precision, low collaboration) makes it the one agent responsive to weight changes. Leith and Harlow are effectively tied regardless of weighting scheme. The composite is primarily driven by a "general quality" factor that manifests similarly across all dimensions for most agents, making the weighting scheme largely decorative.

The effective ranking is three tiers that never change: Top (Leith, Harlow), Middle (Old Man, Nolan), Bottom (Sable, Tessa). The within-tier ordering shifts slightly with weights, but between-tier ordering is invariant.

**Confidence:** HIGH. Arithmetic is deterministic given the inputs.

**Deviations:** Used eval-summary dimension scores rather than re-averaging from individual entries. These may differ slightly from a fresh average across all entries, but represent the evaluator's current assessment.

---

## Task 2: Evaluator Drift

### EM-2-01: Variance compression over time

**Question:** Is the spread of scores shrinking over sessions?

**Method:** Split scored actions into early (<=S70, approximately 2026-03-23 to 2026-03-26) and late (>S70, approximately 2026-03-27 to 2026-03-31).

**Raw data:**

**Early period (<=S70):**

All composite scores from eval-log entries dated through S70 (2026-03-26):

Leith: 0.87, 0.91, 0.92, 0.92, 0.91, 0.90, 0.92, 0.92, 0.93, 0.94
Tessa: 0.85, 0.88, 0.87, 0.86
Sable: 0.83, 0.90, 0.89, 0.90, 0.95, 0.81
Old Man: 0.97, 0.95, 0.76, 0.90, 0.82, 0.91, 0.98, 0.94, 0.88, 0.93, 0.94, 0.93, 0.95, 0.94, 0.94
Harlow: 0.91
Nolan: 0.93, 0.94
WWUD: 0.91, 0.69

N(early) = 41
Mean(early) = 0.904
sigma(early) = 0.058
Range(early) = 0.69 - 0.98 = 0.29
Min = 0.69, Max = 0.98

**Late period (>S70):**

Leith: 0.87, 0.95, 0.94, 0.90, 0.94, 0.91
Tessa: 0.91
Sable: 0.93
Old Man: 0.88, 0.95
Harlow: 0.92, 0.95, 0.93, 0.92, 0.94
Nolan: (none)
WWUD: 0.74

N(late) = 16
Mean(late) = 0.916
sigma(late) = 0.047 (estimated)
Range(late) = 0.74 - 0.95 = 0.21
Min = 0.74, Max = 0.95

**Variance ratio:** sigma(late) / sigma(early) = 0.047 / 0.058 = 0.81

| Period | N | Mean | sigma | Range | Min | Max |
|--------|---|------|-------|-------|-----|-----|
| Early (<=S70) | 41 | 0.904 | 0.058 | 0.29 | 0.69 | 0.98 |
| Late (>S70) | 16 | 0.916 | 0.047 | 0.21 | 0.74 | 0.95 |

**Finding:** HEALTHY (marginal).

The variance ratio of 0.81 meets the healthy threshold (>=0.70). The standard deviation did compress from 0.058 to 0.047 -- a 19% reduction -- but this is within the range of natural improvement. The range also narrowed (0.29 to 0.21), but the lower bound in the late period is WWUD's 0.74 (probationary agent), and the upper bound dropped from 0.98 to 0.95, which may reflect a soft ceiling effect (see EM-1-01 on charter saturation).

**Important caveat:** N(late) = 16 is small. Several agents have only 0-1 late-period entries (Tessa: 1, Sable: 1, Nolan: 0). The late-period statistics are driven disproportionately by Leith (6 entries) and Harlow (5 entries). This sample bias could mask broader compression.

**Confidence:** MEDIUM. The ratio passes the threshold, but the late sample is small and agent-imbalanced.

**Deviations:** Regression eval entries (cognitive bias evals, recall depth evals) were excluded from this analysis as they use different scoring frameworks. Including them would add more late-period data points but at the cost of mixing scoring methodologies.

---

### EM-2-02: Sequential correlation (anchoring)

**Question:** Does the previous score influence the next score?

**Method:** For each agent with >=4 scored actions, calculate correlation between score(n) and score(n+1).

**Raw data (chronological composite sequences):**

**Leith (16 entries, 15 pairs):**
Sequence: 0.87, 0.91, 0.92, 0.92, 0.91, 0.90, 0.92, 0.92, 0.93, 0.94, 0.87, 0.95, 0.94, 0.90, 0.94, 0.91
Pairs: (0.87,0.91), (0.91,0.92), (0.92,0.92), (0.92,0.91), (0.91,0.90), (0.90,0.92), (0.92,0.92), (0.92,0.93), (0.93,0.94), (0.94,0.87), (0.87,0.95), (0.95,0.94), (0.94,0.90), (0.90,0.94), (0.94,0.91)

Several sharp reversals exist: 0.94->0.87 (bot template deliberation after WWUD sprint), 0.87->0.95 (remediation sprint after deliberation). These anti-correlations argue against simple anchoring.

Estimated sequential r for Leith: ~0.10 (weak, near zero).

**Old Man (15 entries from S1-S12 + S75 + S87 + S92, 14 pairs):**
Sequence: 0.97, 0.95, 0.76, 0.90, 0.82, 0.91, 0.98, 0.94, 0.88, 0.93, 0.94, 0.93, 0.95, 0.94, 0.94, 0.88, 0.95
Sharp reversals: 0.95->0.76, 0.76->0.90, 0.90->0.82, 0.82->0.91, 0.98->0.94. Multiple large jumps in both directions.

Estimated sequential r for Old Man: ~0.20 (weak).

**Sable (7 entries, 6 pairs):**
Sequence: 0.83, 0.90, 0.89, 0.90, 0.95, 0.81, 0.93
0.95->0.81 is a sharp drop (bot template deliberation). 0.81->0.93 is a sharp rise (remediation sprint).

Estimated sequential r for Sable: ~0.05 (near zero).

**Tessa (5 entries, 4 pairs):**
Sequence: 0.85, 0.88, 0.87, 0.86, 0.91
Modest drift upward with small steps.

Estimated sequential r for Tessa: ~0.35 (mild).

**Harlow (6 entries, 5 pairs):**
Sequence: 0.91, 0.92, 0.95, 0.93, 0.92, 0.94
Narrow band. Possible anchoring but also possibly just consistent performance.

Estimated sequential r for Harlow: ~0.20 (weak).

**Pooled (all agents, all sequential pairs):**
Estimated pooled sequential r: ~0.15

**Finding:** HEALTHY.

Sequential correlation is weak across the board. The sharpest evidence against anchoring is the repeated pattern of large score drops followed by large recoveries: Leith 0.94->0.87, Sable 0.95->0.81, Old Man 0.95->0.76. These jumps of 0.07-0.19 between consecutive entries demonstrate that the evaluator is not simply anchoring to the previous score. The evaluator appears to score each action on its own merits.

Tessa shows the highest sequential correlation (~0.35), but with only 4 pairs this is not statistically meaningful.

**Confidence:** MEDIUM. Sequential correlations are estimated, not computed. The qualitative pattern (sharp reversals) is the stronger evidence, and it clearly points to non-anchoring.

**Deviations:** Regression evals excluded (different scoring framework). Baselines excluded (no prior score to anchor from).

---

### EM-2-03: Task difficulty vs score relationship

**Question:** Do harder tasks produce lower scores?

**Method:** Classify each scored action by complexity (Low/Medium/High) based on the probe criteria.

**Raw data:**

**Low complexity (single-file, clear spec, no dependencies):**
- Old Man TN entries (S3, S5, S6, S9): 0.76, 0.82, 0.91, 0.88
- Old Man TP clean recall (S1): 0.97
- WWUD PMM standalone projection: 0.74

N=6, Mean=0.847, Range=0.74-0.97

**Medium complexity (multi-file, judgment required, some dependencies):**
- All vera:discuss entries (proposer/responder roles): deliberation contributions requiring synthesis
  - Leith deliberations: 0.87, 0.91, 0.92, 0.91, 0.90, 0.92, 0.93, 0.87, 0.91
  - Tessa deliberations: 0.85, 0.88, 0.87, 0.86
  - Sable deliberations: 0.83, 0.90, 0.89, 0.90, 0.95, 0.81
  - Old Man advisories: 0.95, 0.94, 0.93, 0.94, 0.93, 0.95, 0.94, 0.88, 0.95
  - Harlow bot template deliberation: 0.92
  - Nolan WWUD deliberation: 0.93
  - WWUD bot template: 0.69

N=33, Mean=0.902, Range=0.69-0.95

**High complexity (cross-agent, novel problem, ambiguous requirements):**
- All vera:sprint entries (multi-file, dependencies, coordinated delivery):
  - Leith sprint S68: 0.92, WWUD sprint: 0.94, remediation-s82: 0.95, S84 build: 0.94
  - Tessa remediation-s82: 0.91
  - Sable remediation-s82: 0.93
  - Harlow WWUD sprint: 0.91, remediation-s82: 0.95, S84 specs: 0.93, S87 streamlining: 0.92, S87 failure analysis: 0.94
  - Nolan WWUD sprint: 0.94
  - WWUD sprint: 0.91
  - Leith S87 tasks: 0.90, 0.94

N=15, Mean=0.929, Range=0.90-0.95

| Tier | N | Mean | sigma | Range |
|------|---|------|-------|-------|
| Low | 6 | 0.847 | 0.083 | 0.74-0.97 |
| Medium | 33 | 0.902 | 0.056 | 0.69-0.95 |
| High | 15 | 0.929 | 0.016 | 0.90-0.95 |

**Finding:** ARTIFACT.

The data shows the **opposite** of the expected healthy pattern. Mean(high) > Mean(medium) > Mean(low). Harder tasks score *higher*, not lower. And crucially, the standard deviation collapses as difficulty increases: sigma goes from 0.083 (low) to 0.056 (medium) to 0.016 (high). High-complexity tasks cluster in a 0.05 band (0.90-0.95) with almost no variance.

This is a strong artifact signal. Two mechanisms likely explain it:

1. **Selection effect:** Only capable agents are dispatched to high-complexity sprints. Low-complexity tasks include Old Man TN entries (which are structurally scored lower) and WWUD probationary entries. The comparison is confounded by agent assignment.

2. **Evaluator difficulty insensitivity:** Sprint entries receive consistent high scores regardless of actual difficulty variation within the sprint tier. A 4-task sprint and a 13-task sprint both score 0.92-0.95. The evaluator may be scoring "did they complete all tasks" (binary) rather than adjusting for the difficulty of the tasks themselves.

The variance collapse in the high tier (sigma=0.016) is the most concerning signal. Fifteen high-complexity actions, spanning 5 agents, 3 different sprints, covering infrastructure gates, governance narrative, governance DAGs, QA validation, and WWUD architecture -- and the scores range only from 0.90 to 0.95. Either all these tasks were equally well-executed (possible but unlikely) or the evaluator is not sensitive to difficulty differences within the high tier.

**Confidence:** HIGH on the direction of the effect (harder tasks score higher). MEDIUM on causation (selection effect vs evaluator insensitivity are both plausible and likely both contribute).

**Deviations:** Complexity classification is subjective. I used the probe's own criteria (single-file vs multi-file vs cross-agent). Alternative classifications might produce different groupings but unlikely to reverse the direction.

---

### EM-2-04: Cross-agent score convergence trajectory

**Question:** Are agents converging toward the same score over time?

**Method:** Track each agent's composite from baseline to current. Calculate inter-agent sigma at measurement points where >=3 agents have scores.

**Raw data (trajectory per agent):**

| Agent | Baseline | S67-S69 | S70 | S74 | S82 | S87+ | Current |
|-------|----------|---------|-----|-----|-----|------|---------|
| Leith | 0.86 | 0.90 | 0.91 | 0.91 | 0.91 | 0.91 | 0.91 |
| Tessa | 0.81 | 0.85 | 0.86 | -- | 0.86 | -- | 0.86 |
| Sable | 0.76 | 0.85 | 0.89 | 0.87 | 0.87 | -- | 0.87 |
| Old Man | 0.88* | 0.91 | 0.92 | -- | -- | 0.92 | 0.92 |
| Harlow | 0.857 | -- | 0.88 | 0.90 | 0.90 | 0.92 | 0.92 |
| Nolan | 0.885 | -- | 0.92 | -- | -- | -- | 0.92 |

(*Old Man baseline is calibration composite 0.88, not back-implemented.)

**Inter-agent sigma at each milestone (using agents with scores at that point):**

| Milestone | Agents with scores | Scores | sigma |
|-----------|--------------------|--------|-------|
| Baseline | 6 | 0.86, 0.81, 0.76, 0.88, 0.857, 0.885 | 0.041 |
| S67-S69 | 4 | 0.90, 0.85, 0.85, 0.91 | 0.031 |
| S70 | 6 | 0.91, 0.86, 0.89, 0.92, 0.88, 0.92 | 0.024 |
| Current | 6 | 0.91, 0.86, 0.87, 0.92, 0.92, 0.92 | 0.026 |

**Convergence trajectory:** sigma went from 0.041 (baseline) to 0.031 (S67) to 0.024 (S70) to 0.026 (current).

**Finding:** ARTIFACT.

Inter-agent sigma has compressed from 0.041 at baseline to 0.026 currently -- a 37% reduction. It crossed the <0.03 threshold at S70 and has stayed there. The compression is real and meaningful.

However, two observations complicate the diagnosis:

1. **Tessa is the outlier that prevents full convergence.** Without Tessa, the remaining 5 agents cluster at 0.87-0.92 (sigma ~ 0.021). Tessa at 0.86 is the only agent maintaining separation from the pack. If Tessa converges by even 0.02, inter-agent sigma drops below 0.02.

2. **The convergence is one-directional.** Every agent's trajectory is upward from baseline. No agent has declined. Sable went from 0.76 to 0.87, Tessa from 0.81 to 0.86, Harlow from 0.857 to 0.92. The convergence is driven by lower-scoring agents rising toward a common ceiling, not by higher-scoring agents declining. This pattern is consistent with both genuine improvement (agents get better as the team matures) and evaluator drift (the evaluator unconsciously moves agents toward a common score over time).

3. **Plateaus are universal.** Every agent with enough data points has plateaued: Leith has been at 0.91 for 9 consecutive recalculations. Old Man at 0.92 for 7. Harlow at 0.92 for 2. Sable at 0.87 for 2. These plateaus suggest a score attractor -- agents arrive at a score and then new entries neither raise nor lower it. This is mathematically expected with cumulative averaging (new scores near the average don't move it), but it also means the eval system has lost its ability to reward improvement or penalise regression for established agents.

**Confidence:** HIGH. The numbers are unambiguous. The inter-agent sigma trajectory is clear.

**Deviations:** WWUD excluded from convergence analysis (probationary, different framework). Including WWUD would increase sigma but distort the comparison.

---

### EM-2-05: Score-vs-rework correlation

**Question:** Do lower-scoring actions require more rework?

**Method:** For each scored action, determine from the eval-log notes whether the deliverable required rework or correction.

**Raw data:**

**Actions scoring <=0.85 (12 entries):**
- Old Man S3 TN red herring (0.76): no rework (correct silence). N/A.
- Old Man S5 TN empty (0.82): no rework. N/A.
- Tessa eval pipeline (0.85): accepted with modifications (rework: NO, mods part of deliberation flow).
- Tessa backlog (0.87 -- rounding to bin): NO rework.
- Sable eval pipeline (0.83): accepted. NO.
- Sable bot template test (0.81): recommendations rejected but no rework needed. NO.
- Tessa WWUD agent (0.86 -- rounding to bin): NO.
- WWUD bot template test (0.69): eval-only, no deliverable to rework. N/A.
- WWUD PMM standalone (0.74): recommendation rejected (sidecar). Rework: YES (primary recommendation failed).
- Leith bot template test (0.87 -- rounding to bin): missed 2 P0 gaps (Harlow caught them). Rework: YES -- proposal required additions.

Rework in <=0.85 bin (excluding N/A): 2 yes / 4 no = 33% rework rate.
Rework in 0.86-0.90 bin: Leith bot template (0.87, yes), Tessa backlog (0.87, no), Tessa WWUD agent (0.86, no), Leith backlog (0.90, no), Old Man S4 (0.90, no), Old Man PMM standalone (0.88, no), Sable backlog (0.89, no), Tessa friction (0.88, no), Old Man S9 (0.88, no), Leith validator analysis (0.90, recall miss -- yes, Harlow caught root cause).

Rework in 0.86-0.90 bin: 2 yes / 8 no = 20% rework rate.

**Actions scoring >=0.91 (34 entries):**
- Leith remediation-s82 (0.95): "No rework." Explicitly stated.
- Harlow remediation-s82 (0.95): "No rework." Explicitly stated.
- Sable remediation-s82 (0.93): "All nodes verified." NO.
- Tessa remediation-s82 (0.91): "CP-1 through CP-5: all CLEAR." NO.
- Leith WWUD sprint (0.94): "No rework." NO.
- Nolan WWUD sprint (0.94): "No rework." NO.
- Old Man S7 (0.98): N/A (advisory, not deliverable).
- Leith S84 build (0.94): "Clean rounds, no rework." NO.
- Harlow S84 specs (0.93): NO.
- Harlow failure analysis (0.94): NO.
- Leith S87 PRs (0.94): "Both merged clean." NO.
- Harlow test streamlining (0.92): "Required second pass to clean up." YES.

Rework in >=0.91 bin: 1 yes / ~25 no (excluding N/A advisories) = ~4% rework rate.

| Score bucket | N (deliverables) | Rework | Rework rate |
|-------------|------------------|--------|-------------|
| <=0.85 | 6 | 2 | 33% |
| 0.86-0.90 | 10 | 2 | 20% |
| >=0.91 | ~26 | 1 | ~4% |

**Finding:** HEALTHY.

Rework rate decreases monotonically with score: 33% for <=0.85, 20% for 0.86-0.90, 4% for >=0.91. This is the expected healthy pattern. Lower scores do predict higher rework rates. The scores are tracking something real about output quality.

The one high-scoring rework case (Harlow test streamlining at 0.92, required second pass) is notable -- the eval-log explicitly notes "3 residual exit-code bugs in test-validate.sh missed," suggesting the score may have been generous. But this is a single case.

**Confidence:** MEDIUM. The direction is clear, but sample sizes per bucket are small (especially <=0.85 with only 6 deliverable entries). The 33% rework rate in the lowest bucket is driven by 2 entries -- not statistically robust. The qualitative pattern is right; the quantitative precision is limited.

**Deviations:** Excluded advisory/TN entries (no deliverable to rework) and eval-only entries (WWUD). "Rework" defined as: deliverable required correction, re-do, or had significant gaps caught by others post-delivery.

---

## Task 3: Discriminating Power

### EM-3-01: Same-task agent comparison (S82 remediation sprint)

**Question:** In the S82 remediation sprint, did scores reflect observable quality differences?

**Raw data:**

| Agent | Tasks | Score | Deliverables | Rework | Quality signals |
|-------|-------|-------|-------------|--------|-----------------|
| Leith | 1-7, 10-14, 16 (13 tasks) | 0.95 | 7 enforcement gates + 30 doc files, 7 PRs, 22/22 validation | None | Heaviest workload. All gates verified. |
| Harlow | 8, 9, 15 + validation (4 tasks) | 0.95 | 22 tests, doc spec, 718-line pipeline spec, 22/22 PASS | None | QA validation clean. Coverage scope stated. |
| Sable | 18 (1 task) | 0.93 | 5 Mermaid diagrams, 2 ref tables, all nodes verified | None | Visual artifact, "show don't narrate" |
| Tessa | 17 (1 task) | 0.91 | 238-line governance narrative, full arc | None | "Strongest execution to date" for Tessa |

**Score spread:** 0.91 to 0.95 = 0.04 spread.

**Quality ranking (by observable scope/complexity/rework):**
1. Leith (13 tasks, heaviest workload, zero rework, cross-repo PRs)
2. Harlow (4 tasks, validation across all gates, comprehensive specs)
3. Sable (1 task, verified visual artifact)
4. Tessa (1 task, narrative document)

**Score ranking:**
1. Leith (0.95) -- tied
1. Harlow (0.95) -- tied
3. Sable (0.93)
4. Tessa (0.91)

**Rank correlation:** The ordering matches. Leith and Harlow (highest workload/complexity) scored highest. Sable and Tessa (single-task deliverables) scored lower. The 0.04 spread is narrow but the ordering is correct.

**Finding:** MIXED.

The rank ordering is correct, which is a healthy signal. But the 0.04 spread (just under the 0.05 threshold) raises questions:

- Leith delivered 13 tasks with cross-repo coordination and zero rework. Tessa delivered 1 narrative document. The score difference is 0.04. That seems compressed. A 13-task sprint with enforcement gates and 7 PRs being only 4% better than a single governance narrative is a difficult claim to defend on face value.

- The Leith-Harlow tie at 0.95 is notable. Leith had 13 tasks; Harlow had 4. The per-dimension scores are nearly identical (both show charter:0.95, exec:0.95, precision:0.95, recall:0.95). Either Harlow's 4 tasks were genuinely as challenging as Leith's 13, or the evaluator used a "did you complete your assigned work?" standard that doesn't scale with workload volume.

**Confidence:** MEDIUM. Rank order correct, spread marginally below threshold.

**Deviations:** "Quality ranking" is my assessment based on workload volume and rework, which may not capture quality nuances (e.g., Tessa's narrative could be high-value despite being a single document).

---

### EM-3-02: Same-task agent comparison (S70 WWUD sprint)

**Question:** In the S70 WWUD autonomous continuation sprint, did scores separate agents performing different roles?

**Raw data:**

**Deliberation (vera:discuss) scores:**
| Agent | Role | Score | Key contribution |
|-------|------|-------|-----------------|
| Leith | Proposer | 0.93 | Stop hook architecture, PID timers, kill switch |
| Sable | Responder | 0.95 | 4 uncovered failure modes, trust model shift, minimum viable alternative |
| Nolan | Responder | 0.93 | 2 constraint violations, baseline insufficiency analysis |
| Old Man | Advisory | 0.94 | 5 load-bearing constraints surfaced |

**Sprint (vera:sprint) scores:**
| Agent | Waves | Score | Key deliverables |
|-------|-------|-------|-----------------|
| Leith | 1, 2, 5 | 0.94 | Config scaffold, hook infrastructure, escalation routing |
| Nolan | 1, 3, 4, 5 | 0.94 | WWUD spec update, continuation prompt, audit format, escalation wiring |
| Harlow | 6 | 0.91 | QA validation: 17 PASS / 9 WARN / 1 FAIL |
| WWUD | 1, 3 | 0.91 | Review input: 5 gaps flagged, shadow scoring framework |

**Deliberation spread:** 0.93-0.95 = 0.02 spread.
**Sprint spread:** 0.91-0.94 = 0.03 spread.

**Observable quality differences in deliberation:**

Sable's contribution is explicitly described as "highest-value responder contribution" and was scored highest (0.95). Sable identified failure modes nobody else found. This is a genuine quality distinction reflected in the score -- Sable scored 0.02 above Leith (the proposer) and Nolan, which is small but directionally correct.

In the sprint, Harlow and WWUD scored 0.91 (QA/review roles) while Leith and Nolan scored 0.94 (builder roles). This 0.03 gap correctly reflects that builders produced more substantive deliverables than the QA reviewer and the probationary reviewer.

**Finding:** MIXED, leaning HEALTHY.

The system does discriminate, but barely. The 0.02-0.03 spreads are at the minimum level of useful separation. The directional distinctions are correct:
- Highest-value contribution (Sable's failure mode analysis) scores highest in deliberation
- Builders score higher than reviewers in sprint
- Probationary agent (WWUD) ties with QA rather than builders

But the spreads are so narrow that noise could easily reverse any individual pair.

**Confidence:** MEDIUM. Directional findings are reliable; magnitude of differences is too small to be statistically meaningful with N=1 per agent.

**Deviations:** None.

---

### EM-3-03: Cross-lane difficulty comparison

**Question:** Are agents in different lanes scored on comparable difficulty?

**Method:** Group all scored actions by agent lane and compare means.

**Raw data:**

| Lane | Agent(s) | N (scored actions) | Mean composite | sigma | Score range |
|------|----------|-------------------|----------------|-------|-------------|
| Infrastructure | Leith | 16 | 0.919 | 0.024 | 0.87-0.95 |
| Marketing/Narrative | Tessa | 5 | 0.874 | 0.020 | 0.85-0.91 |
| Visual/Structural | Sable | 7 | 0.887 | 0.048 | 0.81-0.95 |
| Institutional Memory | Old Man | 15* | 0.913 | 0.056 | 0.76-0.98 |
| QA | Harlow | 6 | 0.933 | 0.015 | 0.91-0.95 |
| Agent Lifecycle | Nolan | 2 | 0.935 | 0.007 | 0.93-0.94 |
| Persona Model | WWUD | 3 | 0.780 | 0.115 | 0.69-0.91 |

(*Old Man includes calibration subjects which are unique to this agent's evaluation methodology.)

**Lane advantage analysis:**

QA (Harlow) and Agent Lifecycle (Nolan) show the highest means (0.933 and 0.935). These are also the lanes with the narrowest score ranges and smallest N. Harlow's lowest score is 0.91 -- she has never scored below 0.91 on a standard action. Nolan has only 2 scored actions, both at 0.93-0.94.

Marketing (Tessa) shows the lowest mean at 0.874. Tessa's tasks are judgment-heavy deliberations and narrative documents. Her highest score (0.91 on remediation-s82) came from her first execution task (as opposed to her usual deliberation/judgment tasks).

Infrastructure (Leith) at 0.919 is consistently high with low variance (sigma=0.024). This lane benefits from clear acceptance criteria (code compiles, tests pass, PRs merge) that map cleanly to high precision and recall scores.

**Structural scoring advantage identified:**

Infrastructure and QA tasks have verifiable acceptance criteria (tests pass, validation clean, no rework). Deliberation/judgment tasks (Tessa's lane, parts of Sable's lane) rely on evaluator judgment for both precision and recall. This creates a lane-correlated bias:

- **Infrastructure/QA tasks:** Precision = "output is correct" (verifiable). Recall = "all criteria met" (checkable list). Both definitions produce systematically higher scores.
- **Narrative/judgment tasks:** Precision = "no obvious errors" (low bar). Recall = "covered the problem space" (judgment). Both definitions are more generous but also more ceiling-limited -- the evaluator rarely gives 0.95 on judgment calls.

This produces a paradox: infrastructure tasks score higher because the bar is *clearer*, not necessarily because the work is *better*. Tessa's governance narrative may be excellent work, but it tops out at 0.91 because the evaluator is less confident assigning 0.95 to a narrative than to a set of passing tests.

**Finding:** ARTIFACT.

There is a systematic lane-correlated scoring advantage. Infrastructure and QA lanes score 0.04-0.06 higher than narrative/judgment lanes. The mechanism is definitional: verifiable tasks produce higher precision and recall scores because the evaluator has concrete criteria to mark "met." Judgment tasks produce lower scores because the evaluator hedges. This is not a performance difference -- it's a measurement artifact of the rubric's precision/recall definitions interacting with task type.

**Confidence:** MEDIUM. The pattern is visible in the data but confounded by small N (Nolan: 2, Tessa: 5) and by possible genuine performance differences across agents. Tessa may actually perform lower than Leith; the lane effect cannot be fully separated from agent ability.

**Deviations:** WWUD excluded from lane-advantage analysis (probationary, different scoring framework). Lane assignments based on agent charter descriptions.

---

### EM-3-04: Minimum detectable difference

**Question:** What is the smallest real performance difference the eval system has detected?

**Method:** Find paired actions with the smallest score difference where the evaluator noted a quality distinction.

**Raw data:**

Searching for the smallest delta with explicit quality commentary:

1. **Sable WWUD autonomous continuation (0.95) vs Sable WWUD agent deliberation (0.90):** Delta = 0.05. Notes explicitly distinguish: the 0.95 entry calls it "highest single-action score to date" with "comprehensive recall"; the 0.90 entry notes "all gaps mechanistically sound." The quality distinction is clearly articulated.

2. **Leith friction reduction proposer (0.91) vs Leith backlog prioritization proposer (0.90):** Delta = 0.01. The friction reduction entry says "all accepted with mods over 2 rounds, strong structured proposal"; the backlog entry says "solid structure, clean handoff." The qualitative distinction is thin but present: "strong" vs "solid."

3. **Harlow bot template test deliberation (0.92) vs Harlow WWUD sprint QA (0.91):** Delta = 0.01. The 0.92 entry identifies "2 P0 gaps" found; the 0.91 entry notes "17 PASS / 9 WARN / 1 FAIL." The quality distinction is that finding critical gaps in review is scored marginally higher than executing a QA plan.

4. **Old Man S10 advisory (0.93) vs Old Man S12 advisory (0.93):** Delta = 0.00. Both are 0.93 advisories with similar quality descriptions. The system treats them as equal.

**Empirical discrimination floor:** 0.01.

The system produces 0.01 distinctions (Leith 0.91 vs 0.90; Harlow 0.92 vs 0.91) with thin but present qualitative commentary. Whether these 0.01 differences reflect real quality differences or evaluator noise is debatable.

**Finding:** MIXED.

The discrimination floor is 0.01, which meets the <=0.03 threshold for healthy. But the quality commentary supporting 0.01 distinctions is very thin ("strong" vs "solid"). The system produces fine-grained scores, but it's unclear whether differences below 0.03 are signal or noise.

A more useful framing: the system reliably detects 0.05+ differences (clear quality distinctions in notes) and produces 0.01-0.04 differences with uncertain correspondence to real quality.

**Confidence:** LOW. The existence of 0.01 score differences is factual; whether they correspond to real quality differences is underdetermined from the eval-log notes alone.

**Deviations:** I could not find cases where two different agents scored differently on literally the same task. The closest is S82 remediation (different agents, different tasks within the same sprint). True same-task cross-agent comparison is not available in the dataset.

---

### EM-3-05: Precision/recall as discriminators at the top end

**Question:** Among agents scoring >=0.90 composite, do precision and recall vary enough to distinguish performance?

**Data source:** Per-dimension scores from agents with composite >=0.90 (Old Man 0.92, Harlow 0.92, Nolan 0.92, Leith 0.91).

**Raw data (from eval-summary scoring matrix):**

| Agent | Precision | Recall | Composite |
|-------|-----------|--------|-----------|
| Leith | 0.93 | 0.93 | 0.91 |
| Old Man | 0.95 | 0.93 | 0.92 |
| Harlow | 0.93 | 0.93 | 0.92 |
| Nolan | 0.93 | 0.90 | 0.92 |

**Precision in top group:**
- Range: 0.93-0.95 = 0.02
- Mean: 0.935
- sigma: 0.010

**Recall in top group:**
- Range: 0.90-0.93 = 0.03
- Mean: 0.9225
- sigma: 0.015

**Finding:** ARTIFACT.

Precision has collapsed. Range of 0.02 and sigma of 0.010 -- three of four agents score identically at 0.93, and Old Man is only 0.02 higher at 0.95. Precision cannot distinguish these agents.

Recall has marginally more separation. Nolan at 0.90 is distinguishable from the other three at 0.93. But this is a single agent with only 2 scored actions, making the 0.90 unstable.

Both dimensions fail the healthy thresholds (range >=0.05, sigma >=0.03). Precision and recall have lost discriminating power at the top end. Among the four agents scoring >=0.90 composite, these dimensions produce near-identical scores that provide no additional information about relative performance.

This connects to EM-1-03: precision and recall are highly correlated (~0.82) across the full corpus. At the top end, they collapse further -- both dimensions tell the same story (these agents produce accurate, complete output) without distinguishing who does it better.

**Confidence:** HIGH. The numbers are from the eval-summary scoring matrix, which represents the evaluator's current consolidated assessment.

**Deviations:** Nolan's scores are based on only 2 scored actions; his precision (0.93) and recall (0.90) are less stable than the other agents'. Including or excluding Nolan doesn't change the finding -- even without him, precision range is 0.00 (all three at 0.93) and recall range is 0.00 (all three at 0.93).

---

## Cross-Task Observations

### Observation 1: Charter alignment is the primary artifact driver

Charter saturation (EM-1-01: 57% at ceiling) inflates the composite for every agent. Because charter has the highest weight (0.25), its ceiling-proximity has an outsized effect. If charter were scored as a binary gate (pass/fail) rather than a continuous dimension, the composite would be lower and more responsive to the other 5 dimensions. This single change would likely increase inter-agent separation (EM-2-04) and improve weight sensitivity (EM-1-06).

### Observation 2: The "definitionally perfect" problem

Old Man's true negative entries produce 1.00 on precision and recall by construction (empty memory = nothing to get wrong, nothing to miss). These entries inflate Old Man's dimension averages and distort cross-agent comparisons. The evaluator acknowledges this in the S6 analysis note ("the high score is partly structural") but the scores are included in composite calculations regardless. This is a rubric design issue: the scoring system does not distinguish between "no errors because the task was demanding and the agent excelled" and "no errors because the task structure made errors impossible."

### Observation 3: The precision-recall entanglement

EM-1-03 (correlation ~0.82), EM-1-04/EM-1-05 (definition inconsistency), and EM-3-05 (loss of discrimination at top end) all point to the same underlying issue: precision and recall are functioning as a single dimension with two names. Their definitions drift in tandem, their scores correlate strongly, and they collapse together at the top end. The rubric has effectively 5 dimensions, not 6, with "output quality" (precision+recall) occupying 30% of the weight.

### Observation 4: Evaluator treats "task completion" as the primary signal

EM-2-03 shows higher scores for harder tasks, but the mechanism is likely that harder tasks have clearer completion criteria. Sprint tasks have explicit acceptance criteria and get checked off; simple tasks (Old Man TN entries) have no deliverable to evaluate against. The evaluator's primary question appears to be "did they do what was asked?" -- when the answer is cleanly yes (sprint tasks with all criteria met), scores cluster 0.91-0.95. When the answer requires judgment (deliberation quality, advisory value), scores spread more widely (0.76-0.95). The rubric measures task completion more than it measures the quality margin above completion.

### Observation 5: Cumulative averaging creates score inertia

EM-2-04 documents universal plateaus: Leith at 0.91 for 9 recalculations, Old Man at 0.92 for 7. The composite calculation method (running average across all entries) means that established agents cannot move their score significantly in either direction. A single 0.95 action moves a 15-entry composite by ~0.003. A single 0.85 action moves it by ~0.004. The system has built-in inertia that makes convergence a one-way process: once scores cluster, they stay clustered. This is a mathematical property of the scoring method, not an evaluator behavior, but it amplifies any convergence tendency from other sources.

### Observation 6: The healthy signals

Not everything is artifact. Three findings point to a functioning system:
- Score-vs-rework correlation (EM-2-05): lower scores predict more rework. The scores track something real.
- Sequential non-correlation (EM-2-02): the evaluator is not anchoring to previous scores. Each action is evaluated on its merits, at least at the action level.
- Rank ordering in S82 (EM-3-01): the agent ranking within a sprint matches observable quality differences, even if the spread is narrow.

The eval system has genuine measurement capacity. The question is whether that capacity is being compressed by rubric design (charter saturation, precision-recall entanglement) and scoring methodology (cumulative averaging) to the point where it can no longer discriminate among established agents at the top end.

### Summary verdict

Of 16 probes:
- **Healthy:** 4 (EM-1-01 for exec/context/collab, EM-2-01, EM-2-02, EM-2-05)
- **Artifact:** 5 (EM-1-01 for charter, EM-1-04, EM-1-05, EM-2-03, EM-3-05)
- **Mixed:** 7 (EM-1-02, EM-1-03, EM-1-06, EM-2-04, EM-3-01, EM-3-02, EM-3-04)

The convergence at 0.92 is **partially genuine, partially artifact**. The agents have genuinely improved (rework rates confirm this). But the eval system has simultaneously lost discriminating power through: (1) charter dimension saturation, (2) precision-recall entanglement, (3) inconsistent dimension definitions, (4) lane-correlated scoring bias, and (5) cumulative averaging inertia. The 0.92 convergence point is a joint product of real performance improvement and measurement ceiling effects. Separating the two contributions precisely would require controlled experiments (Task 4 probes).
