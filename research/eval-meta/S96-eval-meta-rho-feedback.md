# Eval-Meta: Paper and Video Impact Assessment

**Date**: 2026-03-31 (S96)
**Author**: Rho (research lead)
**Input**: S96 Eval-Meta Report (20 probes, Tasks 1-4), paper v3 at `agents/rho/work/paper/v3/`

---

## 1. Paper Impact Assessment (Claim-by-Claim)

### Claims that survive untouched

**The recall-depth evaluation findings are not affected.** The Eval-Meta diagnostic targeted the *operational eval rubric* (the 6-dimension composite used for sprint scoring: charter alignment, execution quality, context comprehension, precision, recall, collaboration). The paper's recall-depth evaluation uses a *completely different methodology*: attack vectors, arm decomposition (A/B/D), probe-specific scoring (0.1-1.0 on a failure-mode preference scale), confusion matrices, and lift calculation. These are separate instruments measuring different things.

Specifically, the following claims are grounded in the recall-depth protocol and are unaffected:

| Claim | Location | Status |
|---|---|---|
| Autonomous retrieval accounts for ~83% of lift (+0.440 vs +0.088) | Abstract, Section V, Table 2 | **Unaffected** |
| Partial-context trap: all three models scored 0.2 in window-only mode | Abstract, Section V, Section VI | **Unaffected** |
| Cross-model range ~0.07 per agent; Haiku scores 0.889-0.911 | Section V, Table 3 | **Unaffected** |
| Contradiction detection separates models (Sonnet 0.2, Opus 0.8 on AV-3) | Section V, Table 4 | **Unaffected** |
| Zero false positives for Opus across 18 probes | Section V, Tables 5-6 | **Unaffected** |
| Domain specificity amplifies lift (vis agent 75% vs infra agent 15%) | Section V, Table 7 | **Unaffected** |
| Window lift negative for infra agent (-0.08) | Appendix B | **Unaffected** |
| Prompted and autonomous retrieval produce identical results (both 0.97) | Section IV.F | **Unaffected** |
| Score thresholds encode a failure-mode preference ordering | Section IV.G | **Unaffected** |
| Evaluator calibration: Phase 2 failed (60%), Phase 3 passed (100%) | Section IV.B | **Unaffected** |

The recall-depth evaluation's scoring rubric (Section IV.G) is heuristic but *internally consistent* and *designed for the purpose it serves* -- measuring institutional grounding, not task completion. The Eval-Meta findings do not bear on it because the Eval-Meta targeted a different rubric used for different purposes.

### Claims that are affected

**1. Fitness scores per agent (Section VI, contradiction detection discussion)**

The paper states:

> "We evaluate our agents on the probes and attack vectors described in this paper, producing fitness scores per agent that measure demonstrated reliability. We posit that it is possible to combine these two signals, decay-based relevance and agent fitness scores, to derive an authority weight for each memory entry."

This passage conflates two systems. The "fitness scores per agent" referenced here are the *operational eval composites* -- the same scores the Eval-Meta just diagnosed as having ~0.04 effective range above baseline model capability. Using scores with 0.04 discrimination range to weight memory entries by author reliability is not viable. An entry written by a 0.91 agent and an entry written by a 0.89 agent are indistinguishable on this rubric.

**Severity**: Moderate. The claim is speculative ("we posit"), not empirical. It describes an unimplemented idea. But the foundation it rests on -- that fitness scores measure demonstrated reliability -- is exactly what the Eval-Meta showed they do not, at least not at the top end.

**Recommended action**: Caveat or remove. Options: (a) add a sentence noting that the current fitness scoring does not discriminate well at the top of the range, making the composite authority weight a research direction contingent on rubric recalibration; or (b) remove the fitness-score component from the speculation entirely and keep only decay-based relevance, which is unaffected.

**2. Appendix A: Sable's description**

> "Primary evaluation subject with the highest measured memory lift on the team (0.96 treatment vs 0.55 control, 75% relative lift)"

This is a recall-depth score, not an operational eval composite. **Unaffected.** I initially flagged this but on re-reading it is correctly sourced from the recall-depth calibration baseline (Table 7).

**3. Decision proxy persona fidelity scores**

Section VII states:

> "Its persona fidelity was converging (0.65 to 0.78 across calibration rounds) but had not reached the threshold for inclusion."

These are WWUD-specific calibration scores, not operational eval composites. The Eval-Meta did not examine them. **Status unclear** -- if they use the same 6-dimension rubric, they inherit its problems. If they use a separate persona fidelity rubric, they are unaffected. This needs checking but is low priority because the claim is already presented as preliminary.

**4. Section VI: Emergent governance narrative**

The emergent governance discussion (coordinator routing models, dispatch strategy selection, provenance tracing) does not cite operational eval scores. These are qualitative observations with appropriate caveats already in place ("the same caveat about in-session versus cross-session attribution applies"). **Unaffected.**

**5. Section IV.A: "institutional retrieval, not general capability" framing**

This is the paper's most important methodological claim and it is *strengthened* by Eval-Meta, not weakened. The Eval-Meta control arm proved that a blank agent scores 0.88-0.92 on the operational rubric -- confirming that task-completion rubrics measure general model capability. The paper's recall-depth evaluation was designed precisely to avoid this trap: it scores institutional grounding, not correctness. The Eval-Meta validates the paper's design rationale.

> "A correct answer derived from general reasoning, without grounding in the agent's own institutional history, does not count as successful recall."

This is exactly the distinction the operational rubric fails to make. The paper's methodology anticipated the problem the Eval-Meta exposed.

### New finding worth including

**The Eval-Meta creates a potential addition to Section VII (Future Work) or Section IV.C (limitations).** The paper already lists "Independent evaluation" as the most important methodological improvement (Section VII). The Eval-Meta finding -- that a blank agent matches team composites on the operational rubric -- is a concrete, empirical demonstration of *why* independent evaluation matters, and more specifically, why rubrics must be designed to measure what accumulated context adds, not just whether the task was completed.

The paper could add a brief note in Section IV.C or Section VII:

> "A subsequent internal diagnostic confirmed this risk empirically: a control agent with no institutional memory scored within 0.02-0.03 of established team agents on an operational task-completion rubric, demonstrating that rubrics measuring output quality rather than institutional grounding cannot distinguish between model capability and agent-accumulated value. The recall-depth evaluation described in this paper was designed to avoid this confound by scoring institutional retrieval rather than general correctness."

This would strengthen the paper's methodological rationale. Whether to include it is a judgement call -- it adds credibility but also adds complexity.

### The partial-context trap finding

**Gets stronger, not weaker.** The Eval-Meta has no bearing on the partial-context trap result itself (different rubric, different protocol). But the Eval-Meta *does* reinforce the paper's argument that naive evaluation cannot detect the trap. The operational rubric would score a partial-context response highly (it is well-formed, responsive, looks correct). Only the recall-depth probe design, which knows the full answer and can score against it, catches the failure. The Eval-Meta is independent evidence that surface-level quality scoring is insufficient for agent memory evaluation.

---

## 2. Video Impact Assessment (Per Video)

### Context

The four videos were generated by NotebookLM from the paper. They present the paper's findings conversationally. The key question: do the Eval-Meta findings make any video conclusion misleading?

### YT-V1: "NotebookLM Explains Why More Information Makes AI Worse" (partial-context trap)

**Status: Unaffected.**

The partial-context trap is a recall-depth finding. The Eval-Meta used a different rubric, different protocol, different purpose. The V1 conclusion -- that giving agents half an answer makes them worse than giving them no answer -- stands on its own data (all three models at 0.2 in window-only mode on AV-7/AV-8). No correction needed.

### YT-V2: "NotebookLM Finds Out Why the Cheapest AI Memory System Worked Best" (cost-contrast angle)

**Status: Unaffected.**

The cost-contrast argument is about PMM (structured markdown + git) vs. infrastructure-heavy alternatives (vector stores, RAG pipelines). The Eval-Meta findings are about the *operational scoring rubric*, not about the architecture comparison. The lift decomposition numbers (Table 2, Table 3) come from the recall-depth protocol. Haiku's 0.889-0.911 autonomous retrieval score is a recall-depth score. None of these are touched.

### YT-V3: "NotebookLM Discovers When AI Outsmarts Its Own Design" (emergent behaviours + agent-first architecture)

**Status: Minor caution warranted, no correction required.**

V3 discusses emergent governance behaviours (model-aware routing, dispatch strategy selection, provenance tracing). These are qualitative observations already caveated in the paper. The Eval-Meta does not directly contradict them.

However, if V3 references operational eval scores or agent composites as evidence of team performance quality, those references inherit the Eval-Meta's ceiling effect finding. The specific risk: if V3 presents the team's high operational scores (0.86-0.92 range) as evidence that the agents are performing well *because of* their accumulated memory, that claim is now problematic -- the Eval-Meta showed a blank agent reaches the same range.

**Recommended action**: Review the V3 transcript for any mention of operational composites. If present, note for a future follow-up that these scores reflect model capability baseline, not agent-specific value. If V3 discusses only the qualitative governance examples (which are already caveated), no action needed.

### YT-V4: "NotebookLM Reveals the Memory Gap Nobody Saw Coming" (benchmark gap)

**Status: Potentially strengthened, not weakened.**

V4's thesis is that existing benchmarks do not measure what multi-agent teams actually need. The Eval-Meta is *additional evidence* for this thesis: even the team's own operational rubric failed to measure what agent-accumulated context contributes. The benchmark gap is real and now demonstrated from two directions -- externally (no multi-agent institutional memory benchmarks exist) and internally (the team's own operational scoring could not distinguish a blank agent from an experienced one).

If V4 cites the operational eval composites as evidence of team quality, the same caution from V3 applies. But V4's core argument (the gap exists) is reinforced.

---

## 3. Recommended Actions (Prioritised)

### P0 (Before submission)

1. **Revise the fitness-score speculation in Section VI.** The passage about combining decay-based relevance with agent fitness scores to derive authority weights needs a caveat or rewrite. The fitness scores it references have ~0.04 effective range. Option A: add one sentence noting the current scoring does not discriminate at the top end. Option B: remove the fitness-score component and keep only decay-based relevance. Either is acceptable. This is the only claim in the paper that is directly undermined.

### P1 (Should do, adds credibility)

2. **Consider adding a brief Eval-Meta reference to Section IV.C or VII.** A 2-3 sentence note that the team subsequently ran a control-arm diagnostic on its own operational rubric, found ~0.04 effective range above model baseline, and that this validates the paper's design choice to score institutional grounding rather than task completion. This turns a vulnerability into a strength.

### P2 (Nice to have)

3. **Review V3 transcript for operational composite citations.** If V3 mentions specific agent scores from the operational rubric as evidence of quality, note this for a potential follow-up video or pinned comment clarifying that those scores reflect model capability baseline.

4. **Verify WWUD persona fidelity scores (0.65-0.78) are not from the 6-dimension operational rubric.** Low priority. The claim is already hedged ("converging... had not reached the threshold"). If they use a separate rubric, no action needed.

### Not recommended

5. **Do not issue corrections for any of the four videos.** None of V1-V4's core conclusions are contradicted by the Eval-Meta. V1 and V2 are entirely clean. V3 and V4 have at most minor exposure if they cite operational composites, and even then the core arguments hold.

---

## 4. Overall Assessment of Severity

**Low for the paper. Negligible for the videos.**

The paper dodged the bullet by design. The recall-depth evaluation was built to measure institutional grounding, not task completion. The Eval-Meta exposed a problem in the *operational* scoring rubric -- a different instrument used for different purposes. The paper's methodology section (IV.A) explicitly anticipated this distinction:

> "A correct answer derived from general reasoning, without grounding in the agent's own institutional history, does not count as successful recall."

That sentence is now empirically validated by the Eval-Meta control arm, which showed that general reasoning alone produces 0.88-0.92 on a task-completion rubric.

The single affected claim (fitness-score speculation in Section VI) is speculative, clearly flagged as unimplemented, and easily caveated. It does not support any of the paper's headline findings.

The four videos are clean. Their conclusions derive from the recall-depth evaluation, not the operational rubric.

The most productive use of the Eval-Meta findings, relative to the paper, is not damage control but reinforcement: the control-arm result is independent evidence that the paper's methodological design was correct. Consider citing it.

---

*Assessment compiled by Rho. All claims verified against paper v3 section files and S96 Eval-Meta Report.*
