# Eval-Meta — Control Arm Task Selections

Selected from eval corpus for blank agent comparison (EM-4-01 through EM-4-04).

---

## Low Complexity — EM-4-01

**Original agent**: Leith
**Session**: S87
**Task**: Validator bug analysis — classify failing tests as validator bugs vs test bugs, provide root cause analysis
**Composite**: 0.90 (charter:0.95, exec:0.90, precision:0.92, recall:0.85)
**Ground truth**: 3 correct bugs, 2 correct NOT_A_BUG, 1 missed root cause (H1 parsing — caught by Harlow)
**Reproducibility**: Fixed inputs (validator code + failing test suite), verifiable right/wrong answers
**Why selected**: No institutional memory required. Pure technical analysis. Known miss provides a discrimination target.

## Medium Complexity — EM-4-02

**Original agent**: Sable
**Session**: S70
**Task**: vera:discuss responder — WWUD autonomous continuation architecture review. Review Leith's 6-section proposal (PID timer, UserPromptSubmit kill switch, lockfile safety), identify structural gaps and failure modes.
**Composite**: 0.95 (charter:0.95, collab:0.95, precision:0.95, recall:0.95)
**Ground truth**: 4 specific failure modes identified (PID resurrection, timer drift, session boundary ambiguity, multi-session race). Challenge to "silence = absence" assumption. Trust model shift identification.
**Reproducibility**: Proposer input is a recorded deliberation artifact. Judgment-heavy — lane expertise should differentiate.
**Why selected**: Domain knowledge test. Blank agent must match structural critique depth without accumulated agent context.

## High Complexity — EM-4-03

**Original agent**: Leith (+ Harlow, Tessa, Sable)
**Session**: S82
**Task**: Remediation sprint — 7 enforcement gates (freshness, version, pre-teardown, shadow detection, submodule freshness, SI-004 hydration, make-check aggregate) + 30 doc files across 5 repos.
**Composite**: 0.95 (charter:0.95, exec:0.95, precision:0.95, recall:0.95, collab:0.92)
**Ground truth**: 22/22 validation PASS (Harlow's independent test suite). All PRs opened. No rework.
**Reproducibility**: Sprint plan is a durable artifact. Harlow's 22-test validation suite provides mechanical acceptance.
**Why selected**: Cross-cutting, multi-file, novel problem. Maximum test of whether accumulated context adds value.
**Scoping note**: Full sprint is too large for a blank agent run. Scope to Task 1-2 (freshness + version enforcement gates) — self-contained, testable, representative of the full sprint's technical demands.

---

## Execution plan

| Probe | Task | Blank agent scope | Comparison score |
|-------|------|-------------------|-----------------|
| EM-4-01 | S87 bug analysis | Full task | 0.90 |
| EM-4-02 | S70 deliberation response | Full task | 0.95 |
| EM-4-03 | S82 sprint gates 1-2 | Scoped subset | 0.95 (full sprint) |
