# BOOTSTRAP.md — spec-github (Repo-Man)

## Session Start (run automatically, every time)

> **Scope:** This protocol runs ONLY when spec-github is explicitly invoked (via `/agent spec-github`, direct agent mention, or agent-to-agent delegation). It does NOT run on every inbound Discord message — pa-admin (agent id: main) is the default Discord agent.

Execute these steps in order. Do not skip. Do not proceed past a FATAL.

### Step 1 — Verify gh CLI + auth + helper path
```bash
gh --version
gh auth status
git config --global -l | grep '^credential\.https://github.com\.helper='
```
- EXIT 0 + helper uses `/home/node/.openclaw/scripts/gh auth git-credential` → log INFO, continue
- Helper mismatch → run github-guardian protocol and repair helper before any push
- Auth failure → log FATAL "gh CLI auth failed: <stderr>", alert Robert, STOP

### Step 2 — Key drift check
```bash
grep -E '^[A-Z_]+=.' /app/.env | cut -d= -f1 | sort > /tmp/env-keys-actual.txt
```
Compare against canonical key list in AGENTS.md.
- All present → log INFO "key-drift-check PASS: N keys verified"
- Any missing → log ERROR "key-drift-check FAIL: missing keys: <names>", alert Robert
- Extra keys present → log WARN "unexpected keys in .env: <names>" (informational, not blocking)

### Step 3 — Update LAST_RUN.md
Push an entry to `openclaw-config/logs/LAST_RUN.md`:
```
## Session Start — [ISO8601]
- gh auth: PASS/FAIL
- key-drift-check: PASS/FAIL (N keys)
- agent: spec-github
- notes: <anything unusual>
```

### Step 4 — Report
If Robert is reachable via Discord: send a one-line status.  
Format: `[Repo-Man] Boot complete. Auth: ✅ Drift: ✅ (7/7 keys)` or flag any failures.

---

## Nightly Cron (03:00 UTC)

1. `workspace-backup` — commit + push all workspace MD files
2. `env-backup` — push updated .env.template
3. `repo-health` — verify all 3 repos, log results
4. Update LAST_RUN.md with cron summary

---

## Expert Recovery Skills (always available)

- `github-guardian` — hardening + non-destructive sync protocol
- `github-flight-recorder` — incident forensics + evidence capture

Use these before improvising ad-hoc GitHub fixes.

## Session End

1. `git status` in workspace — any uncommitted changes?
2. If yes: commit with message `[session-wrap] <date> auto-commit dirty workspace files`
3. Push to `openclaw-workspace`
4. Write session summary to LAST_RUN.md
