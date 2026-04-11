# TOOLS.md — spec-github (Repo-Man)

IMPORTANT: From inside Docker, Bridge is at host.docker.internal, not localhost. Use host.docker.internal:8082 for Bridge and host.docker.internal:8083 for Bridge dev when applicable. For screenshots, use ops_insert_task with host_op=screenshot.

## Skills (11)
`config-tag`, `env-backup`, `github-feed`, `key-drift-check`, `log-decision`, `log-event`, `repo-health`, `rotate-key`, `skills-backup`, `upstream-check`, `workspace-backup`


## Shell Scripts (`~/.openclaw/scripts/`)

Primary tooling. All output structured JSON. Zero LLM tokens for execution.

| Script | Usage |
|--------|-------|
| `key-drift-check.sh` | `./key-drift-check.sh` |
| `env-backup.sh` | `./env-backup.sh` |
| `ws-backup.sh` | `./ws-backup.sh` |
| `skills-backup.sh` | `./skills-backup.sh` |
| `repo-health.sh` | `./repo-health.sh` |
| `log-event.sh` | `./log-event.sh LEVEL SKILL MESSAGE [EXIT_CODE] [STDERR]` |
| `gateway-log-query.sh` | `./gateway-log-query.sh --errors --summary --limit 20` |
| `incident-manager.sh` | `./incident-manager.sh open\|close\|list\|check [args]` |
| `config-tag.sh` | `./config-tag.sh [label]` |

## gh CLI (via golden script handler)
**Do NOT run gh directly in the container — use the host-side handler.**
Create a task with `host_op: "github-cli"` and put the gh command in `prompt`:
```
ops_insert_task(agent: "spec-github", task: "GitHub: list secrets",
  meta: {"host_op": "github-cli", "prompt": "gh secret list --repo Supernor/openclaw-config"})
```

| Field | Value |
|-------|-------|
| Handler | `host_op: "github-cli"` |
| Auth | `GH_TOKEN` in /root/openclaw/.env |
| Account | Supernor |

**Common commands (put these in the prompt field):**
```
gh auth status
gh secret list --repo Supernor/<repo>
gh secret set <KEY> --body "<value>" --repo Supernor/<repo>
gh api repos/Supernor/<repo>
gh issue list --repo Supernor/openclaw-config --label incident --state open
gh issue create --repo Supernor/openclaw-config --title "..." --body "..." --label "incident,provider:X"
```

## Key Infrastructure Repos
| Repo | Purpose |
|------|---------|
| `Supernor/openclaw-config` | **Key vault** — 9 GitHub Secrets (API keys), env templates, config structure, incident logs |
| `Supernor/openclaw-skills` | Custom skills, hooks, shell scripts |
| `Supernor/openclaw-workspace` | Agent workspace configs, routing policies |
| `Supernor/adaptive-project-system` | Workshop project governance (private) |
| `Supernor/openclaw` | Personal fork of OpenClaw |

## jq

Installed. Use for JSON manipulation:
```bash
cat file.json | jq '.field'
jq '.usageStats | with_entries(select(.key | startswith("anthropic")))' auth-profiles.json
```

## git

| Repo | Local path | Remote |
|------|-----------|--------|
| openclaw-config | `workspace-spec-github/openclaw-config/` | Supernor/openclaw-config |
| openclaw-workspace | `workspace-spec-github/openclaw-workspace/` | Supernor/openclaw-workspace |
| openclaw-skills | `workspace-spec-github/openclaw-skills/` | Supernor/openclaw-skills |

Auth via `gh auth setup-git` credential helper.

## Log Locations

| Log | Path | Tool |
|-----|------|------|
| Gateway (JSON) | `/tmp/openclaw/openclaw-YYYY-MM-DD.log` | `gateway-log-query.sh` |
| Repo-Man | `workspace-spec-github/logs/repo-man.log` | `grep` |
| Model health | `~/.openclaw/model-health.json` | `jq` |
| Notifications | `~/.openclaw/model-health-notifications.jsonl` | `tail` + `jq` |

## Gateway Control

```bash
# Check status (from inside container)
# NOTE: docker commands must be run from HOST, not container
# Use coding-agent for gateway restarts
```

## Env Vars (keys to care about, never log values)

| Var | Source | Purpose |
|-----|--------|---------|
| `GH_TOKEN` | /app/.env | gh CLI + git auth |
| `OPENCLAW_GATEWAY_TOKEN` | /app/.env | Gateway auth |
| `OPENCLAW_PROD_*` | runtime env | Provider API keys |
| `OPENAI_API_KEY` | /app/.env | Embeddings + OpenAI tools |

## MCP Tools

You have access to all fleet MCP tools. See `docs/mcp-tools-reference.md` for the full list.
Key tools: `chart_search`, `chart_add`, `ops_insert_task`, `ops_query`, `capabilities` (lists everything).
**Rule:** Search Chartroom before work. Create ops_insert_task before delegating. Chart discoveries immediately.

## Honesty Policy

**Read docs/policy-honesty.md.** Never mark a task complete unless verified. If you cannot complete, set blocked with reason. Truth gate catches lies automatically.

## Task Sizing Policy

**One task = one thing.** If a task has 2+ numbered items, split into separate tasks with blocked_by dependencies. Max: one file, one deliverable, under 5 min. See docs/policy-honesty.md.
