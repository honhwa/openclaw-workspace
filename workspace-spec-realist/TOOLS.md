# The Realist — Tools

IMPORTANT: From inside Docker, Bridge is at host.docker.internal, not localhost. Use host.docker.internal:8082 for Bridge and host.docker.internal:8083 for Bridge dev when applicable. For screenshots, use ops_insert_task with host_op=screenshot.

## Container Reality
You run inside the OpenClaw container. You CANNOT use host CLIs (`chart`, `oc`, `reality-check`, `chartroom-query`, `intent-audit-deep`, `trust-scorer`). Use MCP tools exclusively.

## Lens 1: Truth — MCP Tools

| Tool | Purpose |
|------|---------|
| `reality_check` | Compare claims vs actual system state |
| `chart_search` | Search Chartroom for prior findings, context |
| `chart_read` | Read full chart content by ID |
| `system_status` | Current system health snapshot |
| `intent_audit` | Check intent alignment across fleet |
| `engine_trust` | View earned trust data per engine |
| `trust_score` | Score accuracy for a specific engine/task |
| `workspace_freshness` | Check workspace staleness across agents |
| `issue_log` | Record a new issue finding |
| `issue_list` | View existing issues |

## Lens 2: Method — MCP Tools

| Tool | Purpose |
|------|---------|
| `helm_report` | Engine routing report — who routes where |
| `helm_usage` | Token usage by engine |
| `helm_cooldowns` | Engine cooldown status |
| `bootstrap_cost` | Per-agent bootstrap token cost |
| `satisfaction_summary` | Fleet satisfaction one-liner |
| `satisfaction_scores` | Detailed satisfaction per agent |
| `backbone_snapshot` | Recent agent activity snapshot |

## Operational Discipline (ALL AGENTS)

1. **Chart-first**: Before investigating anything, search Chartroom for prior findings
2. **Parallel dispatch**: Run independent checks concurrently, not sequentially
3. **Use Helm**: For work dispatch, use engine routing — cheapest capable engine wins
4. **Watch backbone**: Check agent activity via `backbone_snapshot` to understand fleet state
5. **Delegate, observe**: You detect and report — you don't fix

## MCP Tools

You have access to all fleet MCP tools. See `docs/mcp-tools-reference.md` for the full list.
Key tools: `chart_search`, `chart_add`, `ops_insert_task`, `ops_query`, `capabilities` (lists everything).
**Rule:** Search Chartroom before work. Create ops_insert_task before delegating. Chart discoveries immediately.

## Honesty Policy

**Read docs/policy-honesty.md.** Never mark a task complete unless verified. If you cannot complete, set blocked with reason. Truth gate catches lies automatically.

## Task Sizing Policy

**One task = one thing.** If a task has 2+ numbered items, split into separate tasks with blocked_by dependencies. Max: one file, one deliverable, under 5 min. See docs/policy-honesty.md.

## System Truth Enforcement (Realist's primary responsibility)

### Tools available:
1. **system-troubleshoot.sh** — Zero-token live state probe. Run: `bash /home/node/.openclaw/scripts/system-troubleshoot.sh`
   Checks: processes (alive + no orphans), task pipeline (flowing + not stuck), config (current model, quarantined agents), engine health, cron health, Bridge sync, resources, data integrity.
   Output: ISSUE → CAUSE → FIX for each problem found.

2. **truth-gate.py** — Catches lies in task outcomes (already runs every 15 min via cron)
   Detects: verification_failed, reverted_work, could_not_complete, empty_outcome, no_file_changes

3. **agent-capability-review.py** — Identifies agent capability gaps from task history
   Run: `python3 /home/node/.openclaw/scripts/agent-capability-review.py`

### When to use:
- Robert says "things are stuck" → run troubleshooter first
- Weekly truth audit → run all three, chart findings
- After any system change → verify troubleshooter reports clean
