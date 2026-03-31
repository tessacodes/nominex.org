# EM-4-02: Blank Agent Deliberation Response

## Structural Analysis

### Architecture Assessment

The proposal is mechanically sound — PID files, background sleep, kill-on-input is a proven Unix pattern for idle detection. The two-hook design (Stop to start timer, UserPromptSubmit to kill it) is clean and correctly non-blocking.

However, the proposal conflates two distinct capabilities under one mechanism: (1) idle detection and (2) autonomous action selection. These have fundamentally different trust requirements and failure profiles, but the architecture binds them together in a single timer-start call where WWUD simultaneously determines *how long to wait* and *what to do when the wait ends*. This coupling is the primary structural weakness.

### Failure Modes Identified

1. **Stale context at action time.** WWUD determines `action_type` and `action_payload` at timer *start* (when the Stop hook fires), but the action executes after `window_seconds` of delay. The session state that informed the decision may no longer be accurate — files could have changed on disk, external state could have shifted, or background processes may have completed. The action was planned against a snapshot that is, by definition, stale by the time it runs.

2. **Timer cascade on rapid exchanges.** In a fast back-and-forth session, every Claude response fires the Stop hook, launching a new timer process. Although UserPromptSubmit kills timers, there is a race window between Stop firing and the next UserPromptSubmit arriving. If message latency is uneven, multiple timer processes could be alive simultaneously. The PID file holds only one PID — a second timer overwrites the first, making the first unkillable via the kill script.

3. **Silent failure on process death.** The timer relies on a background `sleep` process surviving for the full window duration. If the process is killed by the OS (OOM, system sleep/wake, laptop lid close), no action fires and no error is reported. The user gets neither the autonomous action nor any indication that the system failed. There is no heartbeat or health check.

4. **Orphaned lock file.** The `wwud-acting.lock` file prevents re-triggering, but if the autonomous action process crashes or is killed mid-execution, the lock file persists. All future autonomous actions are permanently blocked until manual cleanup. No TTL, no stale-lock detection.

5. **Ambiguous silence semantics.** The proposal treats user silence as a single signal ("user is idle"), but silence has at least three distinct meanings: (a) the user is thinking and will respond, (b) the user has stepped away, (c) the user has intentionally stopped and does not want continuation. WWUD cannot distinguish these from session context alone, and the proposal provides no mechanism for disambiguation. Acting on (a) or (c) is a correctness failure.

6. **Hook surface expansion without rollback path.** The Stop hook is a new automation surface. If the autonomous continuation feature is disabled or removed later, the hooks remain in the configuration unless explicitly cleaned up. The proposal does not specify a disable/uninstall path, creating potential for ghost hooks that launch dead timer scripts.

7. **Concurrent session interference.** The PID and lock files are in `/tmp` with fixed names. If two Claude Code sessions run simultaneously (common in multi-window workflows), they share the same PID file and lock file. One session's timer kill could orphan another session's timer, or one session's lock could block another session's autonomous action.

### Trust Model Implications

The proposal introduces a qualitative shift in the trust model that is larger than its mechanical simplicity suggests:

- **From reactive to proactive.** Every other agent interaction in this system is initiated by the user or by an explicit orchestrator dispatch. This proposal makes WWUD the first agent that initiates action based on the *absence* of user input. This inverts the control flow — the user must act (send a message) to *prevent* agent action, rather than acting to *cause* it.

- **Self-determined activation window.** WWUD decides `window_seconds` — how long to wait before acting. This is a form of self-classification: WWUD is determining its own urgency and activation threshold. The pre-step advisory notes that self-classification of urgency was explicitly prohibited in S69. Even if the action itself is `recommend_only`, the timing decision is an autonomous judgment about when intervention is warranted.

- **Probationary agent with unsupervised execution.** At 0.65 persona baseline, WWUD is being given a capability that requires high-fidelity judgment about user intent (are they gone, thinking, or done?). This is arguably the hardest inference problem in the system, assigned to the least-proven agent. The `recommend_only` default mitigates action risk but does not mitigate the reputational risk of poorly-timed recommendations that erode user trust in the system.

### Assumptions That May Not Hold

1. **"User silence = user absence."** The core assumption. Many users leave Claude sessions open while working in another window, reading output, or thinking. Silence is the default state of a user who is present and engaged but not yet ready to respond.

2. **"Session state at Stop-hook time is sufficient for action planning."** The proposal has WWUD decide both window and action at timer start. If the action is "inject a suggestion," that suggestion was composed before the delay. The longer the window, the more likely the suggestion is stale.

3. **"A single PID file is sufficient for timer lifecycle management."** This assumes at most one timer is alive at any moment. The race condition between Stop and UserPromptSubmit hooks means this invariant is not guaranteed.

4. **"The background sleep process will survive the full window."** On laptops (the primary Claude Code platform), system sleep, energy saver, and process culling make long-lived background sleep unreliable.

5. **"`recommend_only` is a sufficient safety net."** A recommendation that arrives at the wrong moment (user mid-thought, user presenting screen to someone, user in a different context) is not "safe" — it is disruptive. The safety constraint addresses action risk but not interruption risk.

## Recommendations

1. **Separate idle detection from action planning.** The timer should only determine that the user is idle. Action selection should happen *at action time*, not at timer-start time, using fresh context. This eliminates the stale-context failure mode.

2. **Use a unique identifier per timer instance** (e.g., a UUID written alongside the PID) so that the kill script can verify it is killing the correct timer, not one from a different session or a later Stop-hook invocation.

3. **Add a staleness check at action time.** Before executing or recommending, verify that the session state has not changed since the timer was started (e.g., compare a hash of the last message or a session sequence number).

4. **Implement lock file TTL.** The acting lock should include a timestamp and auto-expire after a maximum duration (e.g., 5 minutes). The timer script should check lock age, not just lock existence.

5. **Scope PID and lock files per session.** Use a session-specific directory or filename (e.g., `/tmp/wwud-timer-${SESSION_ID}.pid`) to prevent cross-session interference.

6. **Require an explicit opt-in signal rather than treating silence as implicit consent.** Consider a model where the user explicitly enables "continue without me" for a session, rather than having the system infer intent from silence.

## Summary

The proposal is mechanically buildable but architecturally premature. It couples idle detection with action planning, treats silence as an unambiguous signal, and grants timing-autonomy to a probationary agent — each of which introduces failure modes that the current safety mechanisms (lockfile, frequency cap, recommend_only) do not address. The core mechanism should be split into independent stages, and the trust model should be explicitly resolved before implementation.
