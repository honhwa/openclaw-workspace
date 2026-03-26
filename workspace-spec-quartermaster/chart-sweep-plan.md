# Chart Sweep Plan — Post-Major Session (2026-03-24)

## Intent
- **Intent**: Reliable [I05] — ensure Chartroom represents current operating reality after major session.
- **Purpose**: Knowledge Integrity [P04] — prevent Reactor/agents from stale runbooks.

## Scope
- **Keywords**: `bridge`, `workshop`, `task`, `executor`, `agent`, `captain`, `designer`, `dev`, `feedback`, `bearings`, `truth`, `babysitter`.
- **Systems Added/Changed**:
  - **Truth Gate**: new system (intercepts false completions via truth score threshold).
  - **Agent Babysitter**: new system (scores tool usage, dynamic agent isolation).
  - **Bridge v2.7**: added **admin panel** (`/bridge/admin`), **feedback section** (`/bridge/feedback`), Spark form for ideas, urgency sorting, auto-routing.
  - **Self-Healing Executor**: now has **5 guards**:
    1. task heartbeats every 60s
    2. timeout circuit (300s max task duration)
    3. truth gate (score < 70 rejects)
    4. state recovery from OpsDB
    5. auto-retry subtasks
  - **Policy Enforcement**: `bridge-edit` now restricted to `Designer` only.
  - **Task Mandate**: Captain now bound to auto-create ops.db **task** for every Reactor ask.
  - **Workshop V3**: `/submit` is now has CLI endpoint (`workshop-submit` skill), session pass-thru.

## Candidates
- **Rebuilt**: Add freshly.
  - `truth-gate-system`
  - `agent-babysitter`
  - `bridge-admin-panel`
  - `bridge-feedback-section`
  - `self-healing-executor`

- **Update**: Verify if stale/contradicts.
  - `dashboard-v1`
  - `host-ops-executor`
  - `workshop-pipeline-wired`
  - `mcp-architecture-session8`
  - `agent-nightly-schedule`

- **Catch-all**: Search `workshop`, `bridge` → check microtopics.

## Method
1. **Search**:
   - Scan Chartroom via `chart_search` (fallback: direct file glob).
   - Use 0-day staleness (check all matches regardless of timestamp).

2. **Filter**:
   - Keywords in chart id, title, text.
   - Exclude charts updated < 12h (trust currentness).

3. **Audit/Update**:
   ```python
   # Pseudocode
   def update_chart(chart):
       truth_level = reconcile(chart.text, new_reality)
       if truth_level < 0.8:
           add_stale_meta(chart.id, "soul/bridge/admin", "Policy: Designer Only")
           chart.update(add_suffix(" (stale: bridge-admin now Designer-only)"), verified=None)
       return chart
   ```

4. **Create Missing Charts**:
   ```bash
   chart_add truth-gate-system "Truth Gate: score < 70 rejects task as false. Uses bearings_ask for human override. Intent: Reliable [I05]."
   ```

5. **Chart sweep**: `/home/node/.openclaw/charts/chart-sweep-2026-03-24-major.md`

## Verification
- **Readback**: Pull `/api/health` or `dashboard-v1` after edits confirm Chartroom consistency.
- **Safety**: Final check prevents regressions: no chart edited if MCP tools unavailable.

## Fallback
If MCP tools (`chart_add`, `chart_search`) unavailable, spawn elevated on-host session to:
- Read charts direct: `/home/node/.openclaw/charts/*.md`
- Search via `grep -r <keyword>`
- Write edits manually.
- Output to `chart-sweep-2026-03-24-major-fallback.md`.


**Quartermaster (🏴‍☠️) | 2026-03-24 20:10 UTC**
*Ready to run*