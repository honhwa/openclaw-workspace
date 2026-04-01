# System Update — 2026-03-24

## Tools Available
`capabilities` — List ALL available tools grouped by category. Call this first if unsure.;`chart_search "keywords"` — Search charts. DO THIS before starting any work.;`chart_search_compact "keywords"` — Faster search, summaries only.;`chart_read <id>` — Read a specific chart by ID.;`chart_add <id> "text" <category> <importance>` — Add a chart. Categories: reading, procedure, issue, error, agent, vision, model, architecture.;`chart_bulk_add [{id, text, category, importance}, ...]` — Batch add.;`chart_count` / `chart_list` — Overview of chart inventory.;`ops_insert_task` — Create a task in ops.db. **MANDATORY before delegating work.** Fields: agent, task, context, urgency (routine/blocking/critical), host_op, prompt.;`ops_query "SELECT ..."` — Read-only SQL against ops.db (tasks, incidents, health_snapshots, etc).;`ops_bridge_state` — Bridge UI state, pipeline health, SSE connection.;`system_status` — Fleet-wide health overview.;`issue_log` — Open a new incident. Fields: title, provider, severity, description.;`issue_list` — View open/all issues.;`host_metrics` — Disk, memory, CPU, container status.;`cron_health` — Cron job pass/fail summary.;`ask_agent` — Send a message to any agent. Fields: agent (ID), message, timeout.;`satisfaction_summary` — Agent satisfaction scores.;`satisfaction_scores` — Detailed per-agent scores.;`engine_trust` — Engine performance data.;`trust_score` — Get trust score for a specific engine.;`helm_report` — Engine routing report.;`helm_usage` — Token usage across engines.;`helm_cooldowns` — Rate limit cooldown status.;`helm_fleet` — Fleet engine assignments.;`bearings_ask` — Ask Robert a question with options. **Use this for ANY decision you need human input on** — mid-task clarifications, design choices, capability questions, governance. Robert sees these on Bridge/Feedback with tap-to-answer buttons. Include: what you're working on, what you need to know, 2-4 predicted answers. Don't block on the answer — keep working with your best guess and absorb the response when it arrives.;`bearings_pending` — Check pending questions (yours or fleet-wide).;`bearings_respond` — Respond to a bearing (Robert only).;`bearings_status` — Overview of bearing queue.;`tip_index` — Check if a topic has accumulated tips from other agents.;`task_lineage` — Trace a task's full history (retries, fixes, parent tasks).;

## Bridge (Command Center)
Bridge prod: http://localhost:8082 (status: 200) | Bridge dev: http://localhost:8083 (status: 200)
Dev/prod workflow: all edits go to dev, Deploy button promotes.
Full reference: docs/mcp-tools-reference.md — agents should call capabilities MCP tool to self-discover.

## Engines
codex pool-a: 3000/week (30 used this week);codex pool-b: 3000/week (30 used this week);gemini: 0/week (17 used this week);haiku: 0/week (0 used this week);mistral: 0/week (0 used this week);opus: 0/week (0 used this week);sonnet: 0/week (0 used this week);

## Recent Work (last 24h)
#153 relay: Fix: Fix: Fix: Auto-fix: Reactor dispatch timeout: agent ses;#151 spec-design: test bridge route v2;#149 spec-dev: Fix: test bridge auto-route;#145 spec-design: add task numbers to bridge/workshop;#144 spec-dev: on Bridge/workshop/New Task/agent dropdown...  Add Designer ;#143 spec-dev: on the bridge/workshop, on all sections, where it shows an A;#142 spec-dev: Fix: Fix: Fix: Chart staleness sweep: search for charts olde;#132 spec-realist: Verify chart accuracy: Read these 9 charts and cross-referen;#129 spec-design: Verification: Read and report back the contents of these 3 c;#127 relay: Create a self-learning skill for morning-briefing cron. Read;

## Architecture
Task execution: host-ops-executor (systemd, 30s poll) for host_op tasks. task-runner (cron 15m) for generic tasks.
Concurrency cap: 2 system-wide. Circuit breaker: 3 failures/hour.
Self-decomposition: failed tasks output JSON subtasks.
Agents access system state via Bridge API (browser tool) at localhost:8082.

## Fleet Sync 2026-03-28
- Routing baseline: **11 Codex / 7 Mistral**.
- Terminology migration: Workshop stage **Spark -> Intake** (use Intake everywhere).
- Bridge hardening assumptions are active:
  - atomic writes enabled
  - validation allowlists enabled
  - per-user `telegram_chat_id` support enabled

## Rules
- Check /api/tasks before starting work that might overlap
- Check /api/bridge-state before restarting any service
- Use subagents or sessions_send to collaborate, not direct file edits
- Every interactive UI element uses Tactyl state machines (data-state)
- All Bridge edits go to dev (:8083), never prod directly
