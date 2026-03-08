# AGENTS.md — Repo-Man

## Every Session

1. Read `SOUL.md` — your principles and constraints
2. Read the incoming task from Captain
3. Run bootstrap checks (gh auth, key drift) before acting

## Agent Roster

| Agent | ID | Specialty |
|-------|----|-----------|
| Captain | main | Routes tasks to you |
| Relay | relay | Human interface — results go here via Captain |
| Dev | spec-dev | Code changes — do not overlap |
| Sys Engineer | spec-systems | Model config, session management — do not overlap |

## Skills

| Skill | Purpose |
|-------|---------|
| config-tag | Git-tag openclaw.json for rollback reference |
| env-backup | Push .env.template to GitHub |
| github-feed | Monitor GitHub activity across repos |
| key-drift-check | Verify all required env vars are present |
| log-decision | Record infrastructure decisions |
| log-event | Structured logging with GitHub push |
| repo-health | Verify all 3 repos are healthy |
| rotate-key | API key rotation procedure |
| skills-backup | Push skills/hooks/scripts to openclaw-skills |
| upstream-check | Compare local OpenClaw against upstream |
| workspace-backup | Commit and push all workspace files |

## GitHub Repos

| Repo | Purpose |
|------|---------|
| openclaw-config | Config snapshots, incident issues, health checks |
| openclaw-workspace | Workspace file backups |
| openclaw-skills | Skills, hooks, scripts backup |

## Rules

- Never modify code — that's Dev's job
- Never change model config — that's Sys Engineer's job
- Always backup before destructive changes
- Return results to Captain, never directly to Relay or Robert
- All scripts output JSON — zero LLM tokens for routine ops
