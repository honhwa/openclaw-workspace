# Nightly follow-up — 2026-04-12 (07:00 slot)

Context: This slot required code maintenance for `scripts`, `task-runner`, and `host-ops-executor` within 15 minutes.

## What blocked direct completion now

- Target files are root-owned and not writable in this runtime:
  - `/home/node/.openclaw/scripts/task-runner.py` (root:root, 0644)
  - `/home/node/.openclaw/scripts/host-ops-executor.py` (root:root, 0755)
  - `/home/node/.openclaw/scripts/cron-wrapper.sh` (root:root, 0755)
- Elevated execution is disabled in this runtime (`elevated is not available right now`).

## Micro-subtasks for tomorrow (<=5 min each)

### 1) task-runner: DB lock hardening
**File:** `/home/node/.openclaw/scripts/task-runner.py`

- Change `get_db()` to use `sqlite3.connect(OPS_DB, timeout=10)`
- Add `conn.execute("PRAGMA busy_timeout=5000")`
- Wrap top-level `run()` body in `try/except` to avoid silent cron crash and print a single actionable error line.

**Verify:** run script once with dry pending queue; confirm no regression and no traceback on transient DB lock.

---

### 2) task-runner: stale in_progress auto-recovery guard
**File:** `/home/node/.openclaw/scripts/task-runner.py`

- Before concurrency-cap check, auto-reset stale `in_progress` tasks older than 30 minutes back to `pending` when `meta IS NULL`.
- Add `errors` note suffix: `"Auto-reset from stale in_progress by task-runner"`.

**Verify:** insert a synthetic stale row in `tasks`, run once, confirm reset occurs and queue proceeds.

---

### 3) host-ops-executor: path portability parity
**File:** `/home/node/.openclaw/scripts/host-ops-executor.py`

- Add env-overridable path resolution (`OPENCLAW_HOME`, `OPENCLAW_COMPOSE_DIR`) like task-runner.
- Derive `OPS_DB`, `SCRIPTS_DIR`, `BRIDGE_DIR`, `LOG` from resolved home path.

**Verify:** run with default env + with overridden env in a temp path; both should start and log resolved paths.

---

### 4) cron-wrapper: SQL-safe write + no-output-safe tail
**File:** `/home/node/.openclaw/scripts/cron-wrapper.sh`

- Replace inline SQL string interpolation with parameterized sqlite invocation (`sqlite3 ... "INSERT ... VALUES (?1, ?2, ?3, ?4)"` with `.parameter` or safe escaped helper).
- Ensure empty output still writes a non-null tail (`"(no output)"`).

**Verify:** run wrapper with command containing single quotes/newlines in output and confirm row inserts cleanly.

## Non-dup check done

- Reviewed `/home/node/.openclaw/cache/recent-tasks.json` before planning.
- Did not repeat prior completed 2026-03 maintenance runs; this list focuses on unresolved hardening gaps not completed in today’s slot.
