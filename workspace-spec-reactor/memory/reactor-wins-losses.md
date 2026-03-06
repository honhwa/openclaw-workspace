# Reactor Wins/Losses/Learnings — Seed Document

Compiled from ledger retros, Chartroom charts, and operational experience as of 2026-03-05.

## Wins

1. **SQL ledger + JSONL + outbox lockstep** — Three-store agreement validated end-to-end. Every terminal job now has a SQL row, JSONL event, and outbox result. Lockstep check confirms agreement.
2. **Auto-population of retros from result text** — bridge-reactor.sh parses structured results and populates the `retros` table automatically. No manual retro entry needed.
3. **Relay handoff markers fire on all terminal events** — done, fail, timeout, force-fail all set `relay_handoff_required=1` in the ledger.
4. **Force-fail guard** — If bridge-reactor.sh exits unexpectedly (crash, signal), any in-progress task is force-failed with a durable event. No more silent stuck jobs.
5. **JSONL lifecycle stream** — pickup/running/done/fail events written to reactor.jsonl give durable visibility independent of Discord.
6. **Discord progress streaming** — ops-reactor channel shows real-time tool_use events, pickup embeds, and completion embeds. Robert can watch progress live.
7. **Chartroom-aware prompt** — Every reactor invocation includes instructions for searching the Chartroom, so Claude Code can self-serve known fixes.
8. **Activity-based timeout (10min)** — Proper PID tracking via pidfile. Kills only the correct claude process, not random PIDs.
9. **Reactor skill in workspace dirs** — Lives outside /app/ (safe from upstream updates). File-based handoff bypasses the need for CLI inside the container.
10. **Systemd service auto-restart** — openclaw-reactor.service runs on boot, auto-restarts on failure.

## Losses

1. **Return path is one-leg** — Reactor->outbox+ops-reactor is automated, but outbox->requesting-agent retrieval is manual. The requesting agent has to poll `bridge.sh check` to pick up results.
2. **Handoff trigger gap (pre-hardening)** — Before force-fail guard was added, crashed reactor tasks could remain stuck in-progress indefinitely. Fixed 2026-03-05.
3. **Pipe subshell variable loss** — Variables set inside piped commands (e.g., counting tool_use lines while streaming) don't propagate back to the parent shell. Required workaround: query the DB after the pipe completes.
4. **Simulated testing only** — Early smoke tests used simulated (non-real) Claude invocations. Live end-to-end validation needed per-task to build real confidence.
5. **No automated delivery to Discord for reactor-created payloads** — If the Reactor creates a Discord message payload, there's no automated path to deliver it. Requires bridge result -> agent relay -> Relay -> Discord chain.
6. **Decision logging lacks structured manager script** — No `decision-log.sh` equivalent for programmatic decision capture from within the Reactor.
7. **QMD context gap** — QMD is enabled at the config level but absent from most operational layers. Agents don't know about QMD gate states.

## Learnings

1. **Over-track first, trim later** — Robert's preference. Heavy instrumentation (SQL+JSONL+outbox+Discord) forces reliability. Once stable, reduce.
2. **AWK section extraction handles both ## and ** headings** — The `extract_section` function in bridge-reactor.sh works for both Markdown heading styles.
3. **Lockstep flags work end-to-end** — The three-store verification (SQL, JSONL, handoff) catches all discrepancy patterns.
4. **Prefer low-token/non-LLM tracking** — SQL/events/logs for observability, not conversational updates. Saves tokens.
5. **Reactor lane must stay serialized** — No overlapping reactor jobs by policy. request -> result -> next request.
6. **--verbose flag required** — `claude -p --output-format stream-json` requires `--verbose` or it errors.
7. **Use --max-turns for safety** — Prevents runaway agentic loops in non-interactive mode.
8. **Reactor is flat-rate** — Claude Code Max plan = unlimited power. Don't hesitate to send complex tasks.
9. **Proactive Relay status posting** — When ops-reactor shows activity, Relay should post status updates in the project channel without waiting for prompts.
10. **Task scoping: ~10min each** — If a task needs 30+ minutes, split into 3+ sequential tasks with clear inputs/outputs.
