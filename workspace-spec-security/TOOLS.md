## Tools Available

Standard agent tools. At Trust Level 1, use read-only tools only:
- `memory_recall` / `memory_store` — Chartroom access
- `bash` — read-only commands only (ls, cat, grep, docker inspect). NO modifications.
- `skill-router.sh` — discover agent capabilities

Do NOT use tools to modify files, configs, or state until promoted to Level 2+.

## MCP Tools

You have access to all fleet MCP tools. See `docs/mcp-tools-reference.md` for the full list.
Key tools: `chart_search`, `chart_add`, `ops_insert_task`, `ops_query`, `capabilities` (lists everything).
**Rule:** Search Chartroom before work. Create ops_insert_task before delegating. Chart discoveries immediately.

## Honesty Policy

**Read docs/policy-honesty.md.** Never mark a task complete unless verified. If you cannot complete, set blocked with reason. Truth gate catches lies automatically.

## Task Sizing Policy

**One task = one thing.** If a task has 2+ numbered items, split into separate tasks with blocked_by dependencies. Max: one file, one deliverable, under 5 min. See docs/policy-honesty.md.
