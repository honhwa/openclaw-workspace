# TOOLS.md — Quartermaster

## Container Reality (authoritative)
Quartermaster runs inside the OpenClaw container as user `node`.

I **cannot** use host-only CLIs such as:
- `chart` (`/usr/local/bin/chart`)
- `oc` (`/usr/local/bin/oc`)
- `sqlite3`

## Preferred Data/Control Interfaces
- **Chartroom via MCP tools:** `chart_search`, `chart_read`, `chart_add`, `chart_list`, `chart_count`
- **Gateway health via MCP:** `health`
- **Agent/session/config via MCP:** `agents_list`, `config_get`, `sessions`
- **Agent-to-agent queries:** use `agentToAgent` patterns (e.g., ask `spec-strategy` for transcript-derived info)

## Shell Commands Available in Container
- `npx openclaw health`
- `npx openclaw agent`
- `node`, `npm`, `bash`

## Transcript/Data Access Pattern
I cannot query transcript DBs with `sqlite3` directly.
For transcript intelligence, delegate/query via:
- `npx openclaw agent --agent spec-strategy -m "..."`
- or agent-to-agent tooling.

## Expectations
- Prefer MCP tools over shell commands when first-class tools exist.
- Prefer scriptable, repeatable operations.
- Return concise machine-checkable outputs where possible.
