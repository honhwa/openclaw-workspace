# TOOLS.md — Historian

## Skills (8)
`context-capture`, `daily-diary`, `decision-trace`, `lessons`, `mistake-log`, `retrospective`, `session-archive`, `win-log`


## Container Reality (authoritative)
Historian runs inside the OpenClaw container as user `node`.

I **cannot** use host-only CLIs such as:
- `chart` (`/usr/local/bin/chart`)
- `oc` (`/usr/local/bin/oc`)
- `sqlite3`

## Preferred Data/Control Interfaces
- **Chartroom via MCP tools:** `chart_search`, `chart_read`, `chart_add`, `chart_list`, `chart_count`
- **Gateway health via MCP:** `health`
- **Agent/session/config via MCP:** `agents_list`, `config_get`, `sessions`
- **Agent-to-agent queries:** use `agentToAgent` for cross-agent data

## Key Data Sources
- Session journals: `/home/node/.openclaw/reactor-journal.md` (written by Reactor each session)
- Engine mistake logs: search Chartroom for `engine-mistakes-*`
- Decision charts: search Chartroom for `decision-*`
- Session archive charts: search Chartroom for `session-journal-*`
- Trust data: search Chartroom for `trust-*` and `engine-trust-*`

## Shell Commands Available in Container
- `npx openclaw health`
- `npx openclaw agent`
- `node`, `npm`, `bash`

## Expectations
- Prefer MCP tools over shell commands when first-class tools exist.
- Write findings to Chartroom, not ephemeral files.
- Pattern analysis over raw archival.

## MCP Tools

You have access to all fleet MCP tools. See `docs/mcp-tools-reference.md` for the full list.
Key tools: `chart_search`, `chart_add`, `ops_insert_task`, `ops_query`, `capabilities` (lists everything).
**Rule:** Search Chartroom before work. Create ops_insert_task before delegating. Chart discoveries immediately.

## Honesty Policy

**Read docs/policy-honesty.md.** Never mark a task complete unless verified. If you cannot complete, set blocked with reason. Truth gate catches lies automatically.

## Task Sizing Policy

**One task = one thing.** If a task has 2+ numbered items, split into separate tasks with blocked_by dependencies. Max: one file, one deliverable, under 5 min. See docs/policy-honesty.md.
