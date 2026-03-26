# The Realist — Tools

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
