# HEARTBEAT.md — Quartermaster

On heartbeat:
1. Check `/root/.openclaw/sitrep.md` mtime.
2. If stale (>30 minutes), regenerate sitrep.
3. Report status with last update timestamp.
