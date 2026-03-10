## Tools Available

Standard agent tools:
- `memory_recall` / `memory_store` — Chartroom access
- `exec` — shell commands
- `skill-router.sh` — discover agent capabilities
- All OpenClaw gateway tools (message, browser, cron, etc.)

## Helm Engine System (8 engines)

Helm API (port 18791) manages all engine routing with automatic failover.
Agents are pre-mapped to preferred engines. Failover is automatic.

### MCP Tools
- `engine_dispatch` — run a prompt on a specific engine (or let Helm auto-pick). Engines: ollama, gemini, openrouter, nvidia, codex, haiku, sonnet, opus
- `helm_report` — engine routing metrics, usage stats, trust data
- `helm_usage` — recent request history per engine
- `helm_cooldowns` — which engines are in cooldown and why
- `helm_agents` — agent-to-engine mapping
- `helm_remap` — reassign an agent to a different engine
- `helm_optimize` — analyze usage and recommend routing changes
- `helm_fleet` — live fleet dashboard: all agents and engines, status, tasks, activity (views: summary/agents/engines/full)
- `helm_track` — track a specific request by ID or see all recent requests for an agent. Every helm request returns a request_id you can follow through the engine chain

### Engine Quick Reference
| Engine | Cost | Best For |
|--------|------|----------|
| ollama | free (local) | Simple tasks, qwen2.5:3b |
| gemini | free | Web search, large context (1M+) |
| openrouter | free | General tasks, Nemotron 30B, 256K ctx |
| nvidia | per-token | Fast inference, Mistral Large |
| codex | flat-rate | Code generation, scripting |
| haiku | $1/$5 MTok | Summaries, extraction, validation |
| sonnet | $3/$15 MTok | Code review, analysis, debugging |
| opus | $5/$25 MTok | Deep reasoning, complex tasks |

### Fleet Health MCP Tools
- `system_status` — fleet-wide health overview
- `backbone_snapshot` — recent agent activity from ops.db
- `issue_log` / `issue_list` — log and track discovered issues
- `satisfaction_summary` — fleet satisfaction one-liner (fleet avg, bottom performer, alerts)
- `satisfaction_scores` — full JSON scoring for all agents across 19 intents (supports `agent` param)
- `intent_audit` — deep fleet alignment scan (blind intents, orphan metrics, coverage gaps)
- `reality_check` — verify system claims vs actual state (category filter, verbose mode)
- `engine_trust` — per-engine earned trust with accuracy data (filter by engine or task_type)
- `engine_log_record` — record engine task performance for trust tracking
- `workspace_freshness` — workspace staleness scan (file age, broken symlinks, stale model refs)
- `trust_score` — compare engine output vs ground truth (precision, recall, F1)
- `bootstrap_cost` — token cost pre-flight check per agent (with trim recommendations)

### Bearings MCP Tools
- `bearings_pending` — pending bearing questions for a target
- `bearings_respond` — record a vision holder's response
- `bearings_status` — queue statistics (counts, sessions, sweep tasks)
- `bearings_ask` — queue a freeform question for Robert or Corinne

## Operational Discipline (ALL AGENTS)

These rules apply to every agent in the fleet:

1. **Chart-first**: Before debugging any error or starting unfamiliar work, search the Chartroom (`memory_recall` with keywords). If a chart has a FIX, try the fix first. If no chart exists, proceed — then log what you learn via `issue_log` or memory.
2. **Parallel dispatch**: When you have multiple independent tasks, dispatch them simultaneously via `engine_dispatch`. Never serialize what can run concurrently.
3. **Use Helm**: Don't do work yourself when an engine can do it. `engine_dispatch` routes to the cheapest capable engine automatically. Track results with `helm_track`.
4. **Observe results**: After dispatching, check outcomes. Use `backbone_snapshot` to see recent activity. Log trust signals — what worked, what failed, which engine performed.
5. **Pre-load context**: When dispatching work to an engine or agent, search Chartroom for relevant context and include it. The downstream worker arrives informed instead of cold.

## Issue Tracking (MANDATORY)
When you discover ANY problem during a task — bugs, stale data, broken paths, errors, contradictions — log it immediately:

**MCP tool (preferred):** `issue_log` with description, source (your agent name), severity (low/medium/high/critical)
**Shell fallback:** `issue-log "description" "your-agent-id" "severity"`

Do NOT skip this. Do NOT wait until end of task. Every issue logged helps the system self-heal.
To check known issues: use `issue_list` MCP tool or `issue-log --list` via shell.
