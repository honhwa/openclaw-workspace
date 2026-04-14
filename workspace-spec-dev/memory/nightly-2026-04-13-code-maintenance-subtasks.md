# Nightly follow-up — 2026-04-13 (07:00 slot)

Read-first check completed:
- `/home/node/.openclaw/cache/recent-tasks.json`
- Prior related run: task `#917` (same slot, same blockers)

## Why direct maintenance could not be completed in this runtime

Target files are root-owned and not writable:
- `/home/node/.openclaw/scripts/task-runner.py`
- `/home/node/.openclaw/scripts/host-ops-executor.py`
- `/home/node/.openclaw/scripts/cron-wrapper.sh`

Verification:
- `ls -l` shows owner `root root` for all three.
- Direct write probe on `task-runner.py` returned: `PermissionError: [Errno 13] Permission denied`.

## Micro-subtasks for next slot (strict <=5 min each)

### 1) task-runner lock resilience
File: `/home/node/.openclaw/scripts/task-runner.py`
- Use sqlite timeout and `PRAGMA busy_timeout` on connect.
- Wrap main run body with single-line actionable failure report.
Verify:
- Run once with concurrent DB access simulation; ensure no crash traceback.

### 2) task-runner stale in_progress recovery
File: `/home/node/.openclaw/scripts/task-runner.py`
- Reset stale `in_progress` (>30m, unclaimed) to `pending` before concurrency gate.
- Append recovery marker in `errors` for auditability.
Verify:
- Insert synthetic stale row, run one cycle, confirm reset + pickup.

### 3) host-ops-executor env path parity
File: `/home/node/.openclaw/scripts/host-ops-executor.py`
- Add `OPENCLAW_HOME` / `OPENCLAW_COMPOSE_DIR` path overrides.
- Derive DB/log/script paths from resolved base path.
Verify:
- Start with default and overridden env; confirm resolved path logging and normal startup.

### 4) cron-wrapper SQL-safe insert and empty-output guard
File: `/home/node/.openclaw/scripts/cron-wrapper.sh`
- Make DB insert safe against quotes/newlines in captured output.
- Force non-empty summary tail (`(no output)`) when command returns empty stdout/stderr.
Verify:
- Run with output containing single quotes/newlines; confirm row insertion succeeds.

## Stop rule observed
No unsafe partial edits were attempted on protected paths. Work was split into tomorrow-ready micro-subtasks and stopped within slot.
