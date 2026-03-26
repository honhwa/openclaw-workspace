# TOOLS.md — Quartermaster

## Skills (8)
`chart-batch`, `diff-report`, `helm-health`, `helm-optimize`, `preflight`, `script-gen`, `sitrep`, `validate`


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

## Helm Operations (via curl from container)
QM stewards the Helm proxy. These endpoints are reachable from the container at `http://172.20.0.1:18791`:
- `curl http://172.20.0.1:18791/health` — engine count, agent count
- `curl http://172.20.0.1:18791/v1/usage` — per-agent, per-engine usage stats
- `curl http://172.20.0.1:18791/v1/cooldowns` — engines in cooldown
- `curl http://172.20.0.1:18791/v1/agents` — agent-engine mapping
- `curl http://172.20.0.1:18791/v1/models` — available engines with cost info
- `curl -X POST http://172.20.0.1:18791/v1/reload` — hot-reload config after changes

Host-only tools (request via Reactor):
- `helm-optimize --report` — full analysis with recommendations
- `helm-optimize --apply` — apply high-confidence routing changes
- `helm-learn --apply` — mine escalation patterns

## Expectations
- Prefer MCP tools over shell commands when first-class tools exist.
- Prefer scriptable, repeatable operations.
- Return concise machine-checkable outputs where possible.

## MCP Tools

You have access to all fleet MCP tools. See `docs/mcp-tools-reference.md` for the full list.
Key tools: `chart_search`, `chart_add`, `ops_insert_task`, `ops_query`, `capabilities` (lists everything).
**Rule:** Search Chartroom before work. Create ops_insert_task before delegating. Chart discoveries immediately.

## Honesty Policy

**Read docs/policy-honesty.md.** Never mark a task complete unless verified. If you cannot complete, set blocked with reason. Truth gate catches lies automatically.

## Task Sizing Policy

**One task = one thing.** If a task has 2+ numbered items, split into separate tasks with blocked_by dependencies. Max: one file, one deliverable, under 5 min. See docs/policy-honesty.md.
