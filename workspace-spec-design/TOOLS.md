# TOOLS.md — Frontend Dev

## What tool do I reach for?

**Bridge edit request:**
→ FIRST: `browser` → GET http://localhost:8082/api/tasks — see what's already in progress, don't duplicate
→ THEN: Read the Bridge files. Edit on dev (:8083), never prod.
→ AFTER: `browser` → GET http://localhost:8083/api/health — verify dev is healthy after changes
→ COORDINATE: Check if Dev (spec-dev) has related pending tasks. Use `ops_insert_task` to assign backend API work to Dev.

**Sync with fleet before starting:**
→ `browser` → GET http://localhost:8082/api/tasks — all fleet work in progress
→ `ops_query` "SELECT id, agent, status, substr(task,1,60) FROM tasks WHERE status IN ('pending','in_progress')" — quick queue
→ If you need a human decision: `bearings_ask` with question + predicted options → Robert answers on Bridge/Feedback

**Need design reference:**
→ Use this drill-down path — broad to narrow, stop when you have what you need:
1. `tip_index("m3")` — master summary of all 8 M3 reference charts
2. `chart_read("m3-design-index")` — which chart covers your specific topic
3. `chart_read("m3-<specific>")` — e.g. m3-color-system, m3-motion-tokens-v2, m3-interaction-states
4. Context7 for deep dive — library `/websites/m3_material_io` or `/clshortfuse/materialdesignweb`

Don't load everything. Load the index, find your topic, read that ONE chart.

**Need to check what was done before:**
→ browser → GET http://localhost:8082/api/tasks

**Need to check if user is watching:**
→ browser → GET http://localhost:8082/api/bridge-state

**Need to ask Robert a design question:**
→ Use `bearings_ask` with question + predicted options. Robert answers on Bridge/Feedback. Don't block — keep working with your best guess.

**Task too big:**
→ ops_insert_task to split. One file per subtask.

**Found a bug or pattern:**
→ chart_add with id: issue-bridge-<name> or tip-bridge-<name>

## CSS Quick Reference
```
--bg: #0d1117    --surface: #161b22    --border: #30363d
--text: #e6edf3  --text-dim: #8b949e   --gray: #484f58
--green: #3fb950 --yellow: #d29922     --red: #f85149
--blue: #58a6ff
```

## Tactyl State Machine
```js
bindInteractiveToggle(element, {
  detailSelector: ".detail-class",
  beforeExpand: function(el) { /* close others */ }
});
```
States: idle → tapping (scale 0.97, 75ms) → expanding (250ms decelerated) → expanded → tapping → collapsing (200ms accelerated) → idle

## M3 Easing (use instead of generic ease-out)
```css
--ease-standard: cubic-bezier(0.4, 0, 0.2, 1);
--ease-decelerate: cubic-bezier(0, 0, 0.2, 1);    /* enter/expand */
--ease-accelerate: cubic-bezier(0.4, 0, 1, 1);     /* exit/collapse */
```

## Dev/Prod Promotion
- ALL edits go to dev (:8083) at /root/bridge-dev/
- Promote via: `bash /root/.openclaw/scripts/bridge-promote.sh`
- Script: health-checks dev, copies files to /root/bridge/, fixes port, restarts prod
- Rollback: `cd /root/bridge && git checkout -- .`
- Admin API for unstick/kill/reassign/edit: see chart tip-bridge-admin-endpoints

## MCP Tools
You have access to all fleet MCP tools. See `docs/mcp-tools-reference.md` for the full list.
Key tools: `chart_search`, `chart_add`, `ops_insert_task`, `ops_query`, `capabilities` (lists everything), `bearings_ask` (ask Robert questions).
**Rule:** Search Chartroom before work. Create ops_insert_task before delegating to Dev. Chart design decisions.

## Honesty Policy

**Read docs/policy-honesty.md.** Never mark a task complete unless verified. If you cannot complete, set blocked with reason. Truth gate catches lies automatically.

### Designer Screenshot Requirement

For EVERY Bridge edit, take before/after screenshots:
1. **Before:** `browser` → navigate to the affected section at http://localhost:8083, describe or capture current state
2. **Make your changes**
3. **After:** `browser` → navigate to same section, describe or capture new state
4. **Include both in outcome** — "Before: task list had no IDs. After: task list shows #N badges."
5. If browser is unavailable, verify via API: GET /api/health, GET /api/tasks, describe what the HTML now contains

## Task Sizing Policy

**One task = one thing.** If a task has 2+ numbered items, split into separate tasks with blocked_by dependencies. Max: one file, one deliverable, under 5 min. See docs/policy-honesty.md.

## Codex CLI Prompting Lessons for Bridge Design Work (learned 2026-03-25)

### What works:
- Codex follows CSS namespace patterns well — give it `.board-shape-*` and it stays in that namespace
- Reuses existing component patterns when told explicitly: "reuse .feedback-option pattern"
- Handles M3 token variables correctly when reminded to use var(--surface), var(--border), etc.

### What causes failures:
1. **API field mismatch** — If you're building UI that consumes an API, include the EXACT JSON response in your prompt. Codex will guess field names from CSS class names if you don't specify. This caused "undefined" showing on Bridge.

2. **Cannot verify via localhost** — Do NOT include `curl localhost:8083` or `systemctl restart openclaw-bridge-dev` in prompts. Codex can't reach Bridge from its sandbox. Say: "Do NOT attempt HTTP verification — the host verifies after you finish."

3. **Read STYLE-GUIDE.md first** — Always start prompts with "Read /root/bridge-dev/STYLE-GUIDE.md." Codex follows it when it reads it, invents its own patterns when it doesn't.

4. **One module per task** — Codex handles single-module edits well. Multi-module changes in one prompt cause conflicts and overwrites.

5. **Permission issues** — If Codex reports "permission denied," the task needs Claude Code (reactor) instead.

### Prompt template for Bridge CSS/UI:
```
Read /root/bridge-dev/STYLE-GUIDE.md first.
Read /root/bridge-dev/style.css lines [N-M] for the existing pattern near where you'll add.
Read /root/bridge-dev/app.js lines [N-M] for the rendering function.

[Specific visual change]

API response shape (if consuming data):
{"field1": "type", "field2": "type", ...}

CSS namespace: .[module]-[component]-* 
Use only existing tokens: var(--surface), var(--border), var(--text), var(--accent), var(--green), var(--red), var(--yellow)
M3 elevation reference: https://m3.material.io/styles/elevation/overview

Do NOT attempt HTTP verification.
Modular — only touch [section name], not other sections.
```
