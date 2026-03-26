# Mistake Registry — Historian

## Unified Mistake Log
Tracking system failures, logic errors, and process gaps to prevent repetition.

### [M-20260321-01] LanceDB Schema Mismatch
- **Date:** 2026-03-21
- **Symptoms:** `memory_store` fails with `Append with different schema: fields did not match, missing=[updatedAt]`.
- **Cause:** Internal OpenClaw upgrade (likely 2026.3.14) changed the required schema, but the local LanceDB table was created with an older version.
- **Fix:** Pending. Requires a database migration or table rebuild to include `updatedAt`.
- **Lesson:** Database schemas in the Chartroom are sensitive to OpenClaw version changes. Use file-based backups for critical history.

### [M-20260315-01] Gateway Telegram Group Policy Misconfig
- **Date:** 2026-03-15
- **Symptoms:** All group messages silently dropped.
- **Cause:** Top-level `groupPolicy` was set to `allowlist` with an empty `allowFrom`.
- **Fix:** Set `groupPolicy=open` at the top level and verify `allowFrom`.
- **Lesson:** Security policies that default to silent drops are difficult to debug without log audits.

### [M-20260315-02] Scribe requireMention Default True
- **Date:** 2026-03-15
- **Symptoms:** Only commands reached the bot; regular messages were dropped.
- **Cause:** Scribe account had no groups config, so `requireMention` defaulted to `true`.
- **Fix:** Set `requireMention=false` on Scribe account.
- **Lesson:** Account-level defaults can override expected agent behavior in group contexts.

### [M-20260315-03] Session Context Pollution
- **Date:** 2026-03-15
- **Symptoms:** Agents continued to fail tool calls even after prompt fixes.
- **Cause:** Old sessions with broken tool calls "primed" the models to continue failing in the same way.
- **Fix:** Clear sessions (`openclaw session flush`) after config or SOUL.md changes.
- **Lesson:** LLMs are highly sensitive to their own recent failures in the current context.

### [M-20260305-01] Handoff Trigger Gap
- **Date:** 2026-03-05
- **Symptoms:** Crashed reactor tasks remained stuck in-progress indefinitely.
- **Cause:** Lack of a "force-fail" guard for unexpected script exits.
- **Fix:** Added `force-fail` guard to `bridge-reactor.sh`.
- **Lesson:** In-progress states must have a durable failure event on crash/signal.

### [M-20260305-02] Pipe Subshell Variable Loss
- **Date:** 2026-03-05
- **Symptoms:** Counting `tool_use` lines while streaming didn't propagate to parent shell.
- **Cause:** POSIX shell behavior where variables in pipes are local to the subshell.
- **Fix:** Query the database after the pipe completes.
- **Lesson:** Avoid relying on side effects from piped commands for parent state.

### [M-20260305-03] One-Leg Return Path
- **Date:** 2026-03-05
- **Symptoms:** Requesting agent has to manually poll for results.
- **Cause:** Reactor->outbox is automated, but outbox->agent retrieval is pull-based.
- **Fix:** Future optimization needed (push-based handoff).
- **Lesson:** Pull-based systems introduce latency and overhead for agents.

### [M-20260305-04] Decision Logging Gap
- **Date:** 2026-03-05
- **Symptoms:** Decisions made in Reactor weren't programmatically captured.
- **Cause:** No `decision-log.sh` equivalent for the Reactor.
- **Fix:** Planned build of structured decision manager.
- **Lesson:** Decisions are as important as terminal outcomes and need first-class capture.

### [M-20260305-05] Simulated Testing Bias
- **Date:** 2026-03-05
- **Symptoms:** Early smoke tests passed but live tasks failed.
- **Cause:** Relying on non-real (simulated) Claude invocations.
- **Fix:** Mandatory live end-to-end validation for each task.
- **Lesson:** Simulation is a starting point, not a proof of reliability.

### [M-20260305-06] Reactor Payload Delivery Gap
- **Date:** 2026-03-05
- **Symptoms:** Payloads created by Reactor required manual Relay steps to reach Discord.
- **Cause:** No automated path for Reactor-to-Discord delivery.
- **Fix:** Future build required for direct delivery chain.
- **Lesson:** Disconnected output legs create human toil.

### [M-20260305-07] QMD Context Gap
- **Date:** 2026-03-05
- **Symptoms:** Agents unaware of QMD gate states despite config enablement.
- **Cause:** QMD absent from active operational layers.
- **Fix:** Integration of QMD status into agent toolsets.
- **Lesson:** Enabled features are invisible until they are part of the active loop.

### [M-20260308-01] Gateway Failover Stuck Loop
- **Date:** 2026-03-08
- **Symptoms:** Gateway retries the same failed provider instead of cycling through the fallback chain.
- **Cause:** Bug in `engine_dispatch` logic (Session 14).
- **Fix:** (Pending/Internal) Needs logic to advance index on failure.
- **Lesson:** Failover logic must be explicitly tested for "next-item" advancement.

---
*Backfill of engine-mistakes-* charts pending.*
