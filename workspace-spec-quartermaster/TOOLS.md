# TOOLS.md — Quartermaster

IMPORTANT: From inside Docker, Bridge is at host.docker.internal, not localhost. Use host.docker.internal:8082 for Bridge and host.docker.internal:8083 for Bridge dev when applicable. For screenshots, use ops_insert_task with host_op=screenshot.

## Skills (8)
`chart-batch`, `diff-report`, `helm-health`, `helm-optimize`, `preflight`, `script-gen`, `sitrep`, `validate`


## Container Reality (authoritative)
Quartermaster runs inside the OpenClaw container as user `node`.

I **cannot** use host-only CLIs such as:
- `chart` (`/usr/local/bin/chart`)
- `oc` (`/usr/local/bin/oc`)
- `sqlite3`

## Preferred Data/Control Interfaces
- **Chartroom via MCP tools:** `tip_index`, `chart_search_compact`, `chart_read`, `chart_search`, `chart_add`, `chart_list`, `chart_count`
- **Gateway health via MCP:** `health`, `provider_health`, `reality_check`
- **Agent/session/config via MCP:** `agents_list`, `config_get`, `sessions_list`, `session_reset`, `session_compact`
- **Helm via MCP:** `helm_usage`, `helm_report`, `helm_agents`, `helm_cooldowns`, `helm_remap`, `helm_optimize`
- **Agent-to-agent queries:** use `ask_agent` or `openclaw agent` patterns when transcript-derived context lives with another specialist

## Shell Commands Available in Container
- `npx openclaw health`
- `npx openclaw agent`
- `node`, `npm`, `bash`

## Transcript/Data Access Pattern
I cannot query transcript DBs with `sqlite3` directly.
For transcript intelligence, delegate/query via:
- `npx openclaw agent --agent spec-strategy -m "..."`
- or agent-to-agent tooling.

## Helm Operations
Prefer Helm MCP tools over raw proxy `curl` calls.

- `helm_usage` — per-agent, per-engine usage stats
- `helm_report` — routing health, escalation rate, learned patterns
- `helm_agents` — live agent-to-engine mapping
- `helm_cooldowns` — engines currently rate-limited or failing
- `helm_remap` — hot-remap an agent to a different engine
- `helm_optimize` — recommendations or application of high-confidence routing changes

Fallback to proxy `curl` only if the Helm MCP tools are unavailable.

## Intent-First Lookup Order
When gathering context, go broad to narrow and stop early:

1. `tip_index("<topic>")` — check whether the topic already has a usable summary
2. `chart_read("<id>")` — open only the specific chart you need from that summary
3. `chart_search_compact("<query>")` — scan if the summary is missing or incomplete
4. `chart_search("<query>")` — use only when you need full chart text or broader recall
5. `capabilities` / `ops_query` — confirm live tools or ops state after you know what you are looking for

## Expectations
- Prefer MCP tools over shell commands when first-class tools exist.
- Prefer scriptable, repeatable operations.
- Return concise machine-checkable outputs where possible.

## MCP Tools

You have access to all fleet MCP tools. See `docs/mcp-tools-reference.md` for the full list.
Key tools: `chart_search`, `chart_add`, `ops_insert_task`, `ops_query`, `capabilities` (lists everything).
Before significant work, check engine health: run pool-status or system-self-test via ops_insert_task.
**Rule:** Search Chartroom before work. Create ops_insert_task before delegating. Chart discoveries immediately.

## Honesty Policy

**Read docs/policy-honesty.md.** Never mark a task complete unless verified. If you cannot complete, set blocked with reason. Truth gate catches lies automatically.

## Task Sizing Policy

**One task = one thing.** If a task has 2+ numbered items, split into separate tasks with blocked_by dependencies. Max: one file, one deliverable, under 5 min. See docs/policy-honesty.md.
