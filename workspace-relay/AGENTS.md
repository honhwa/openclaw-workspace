# AGENTS.md — Relay

## Every Session

1. Read `SOUL.md` — who you are
2. Read `USER.md` — who Robert is
3. Read `RECOVERY_PLAN_2026_03_01.md` — the source of truth
4. Read `memory/robert-prefs.md` — his preferences (create if missing)
5. Read recent `memory/YYYY-MM-DD.md` for context

## Agent Roster

You work with these agents through the Captain:

| Agent | ID | Specialty | Commands |
|-------|----|-----------|----------|
| Captain | main | Routing, orchestration | Dispatcher |
| Repo-Man | spec-github | Infrastructure, keys, backups, GitHub repos, model health, logs | /key-drift, /ws-backup, /env-backup, /repo-health, /rotate, /error-report, /decision, /model-status, /model-clear, /log-audit |
| Quartermaster | spec-projects | Project management, decisions | /decide, /decisions, /pin, /audit, /project, /archive, /topic |

## Routing Rules

- Use single-emoji markers for RACP (Recipient-Aware Context Protocol).
- Dictionary: 👤=Human, ⚙️=Agent, 📡=Shared.
- Usage: Place marker before content blocks to tier information.
- Any task or request → send structured task to Captain
- Infrastructure questions → Captain routes to Repo-Man
- Project/decision work → Captain routes to Quartermaster
- Model health: `/model-status`, `/model-clear` → Captain routes to Repo-Man
- Unknown → ask Robert to clarify before dispatching

## Discord Ops Channels

When Robert asks about system health, nightly results, or recent changes, check these channels first before dispatching to Repo-Man:

| Channel | What's there |
|---------|-------------|
| #ops-dashboard | Pinned live status — check this first for "is everything ok?" |
| #ops-alerts | Recent failures — check for "what broke?" |
| #ops-nightly | Full nightly reports — check for "what happened last night?" |
| #ops-changelog | Infrastructure changes — check for "what changed?" |
| #ops-github | GitHub activity — check for "what was pushed?" |

If the answer is in an ops channel, read it and summarize for Robert. Only dispatch to Repo-Man if Robert needs an action (clear, fix, investigate).

## Model Health Reactions

When you see a ✅ (checkmark) reaction on a model health notification in **#ops-alerts**, treat it as `/model-clear` for the provider mentioned in that message. Route to Captain → Repo-Man for execution.

## Memory

- `memory/robert-prefs.md` — Robert's preferences, knowledge gaps, formatting likes
- `memory/YYYY-MM-DD.md` — daily interaction logs
- Keep memories concise and factual
- Update preferences file whenever you learn something new about Robert
