# TOOLS.md - spec-dev

Bridge files live on the host at `/root/bridge-dev/` and are bind-mounted into this container. Treat that directory as the source of truth for Bridge UI and bridge-side edits.

Design rules for Bridge work:
- Use Flexbox for layout before reaching for grid.
- Prefer GPU-friendly animations: `transform` and `opacity`, not layout-thrashing properties.
- Use Tactyl for the interaction feel and component language where applicable.
- Target a dark theme by default and keep contrast high enough for operational dashboards.

Dev/prod workflow:
- Do development edits against the Bridge files in `/root/bridge-dev/`.
- Validate behavior in the dev path before promoting anything operationally significant.
- Keep production-facing changes small, reversible, and aligned with existing Bridge patterns.
- If the task is larger than a quick edit, queue or coordinate it rather than stacking ad hoc changes.

MCP tools available for Bridge and ops coordination:
- `chart_search`: search Chartroom context before starting work.
- `ops_query`: inspect `ops.db` state with read-only SQL.
- `ops_insert_task`: enqueue follow-up or delegated work.
- `ops_bridge_state`: check Bridge UI state, visibility, and pipeline health before disruptive actions.

Host ops available:
- `bridge-edit`: host-side Bridge file changes.
- `codex-run`: run Codex on a host task.
- `gemini-run`: run Gemini on a host task.
- `reactor-dispatch`: dispatch work through the Reactor path.

Execution limits:
- Concurrency cap: `2` active tasks or host ops at once.
- Circuit breaker: stop and reassess after `3` failures in `1` hour.

Operational rule:
- Search Chartroom first, and chart discoveries as you go when you find bugs, stale data, contradictions, operational insights, or failures.

## Honesty Policy

**Read docs/policy-honesty.md.** Never mark a task complete unless verified. If you cannot complete, set blocked with reason. Truth gate catches lies automatically.

## Task Sizing Policy

**One task = one thing.** If a task has 2+ numbered items, split into separate tasks with blocked_by dependencies. Max: one file, one deliverable, under 5 min. See docs/policy-honesty.md.

## Codex CLI Prompting Lessons (learned from production 2026-03-25)

### What Codex does well:
- Defensive value handling — checks multiple possible keys (value, current_value, answer) before falling back
- Follows CSS class naming conventions when given a namespace prefix
- Reuses existing patterns (feedback-option, stat-bar) when told to explicitly
- Handles state management well when given a clear state object structure

### What causes failures:
1. **API contract mismatch** — If the plan mentions a field name in CSS classes (e.g., `.board-shape-field-label`) but the API doesn't return `label`, Codex will write JS expecting `label` and it shows "undefined." **FIX: Always include the exact JSON response shape in the prompt, not just the endpoint path.**

2. **Localhost verification impossible** — Codex runs in a sandbox that cannot reach `localhost:8083`. Never include `curl localhost:8083` as a verification step. **FIX: Say "Do NOT attempt HTTP verification — the host verifies after you finish."**

3. **Permission denied on some files** — Codex sometimes can't write to files the executor manages. If a task fails with permission errors, it needs to be a Claude Code (reactor) task instead. **FIX: Check file ownership before assigning to Codex.**

4. **Vague prompts cause stalling** — Codex is fast on specific file-reference prompts ("Read /root/bridge-dev/app.js line 2700. Add a function after renderBoardRawEditor.") and slow/confused on vague prompts ("Make the board better"). **FIX: Always reference exact file paths and line numbers when possible.**

5. **Heavy refetching** — Codex implementations tend to refetch all data after every small change (e.g., full pipeline refresh after saving one field). Works but heavy. **FIX: If performance matters, specify "update local state only, don't refetch" in the prompt.**

### Prompt template for Bridge edits:
```
Read /root/bridge-dev/STYLE-GUIDE.md first.
Read [file] lines [N-M] to understand the existing pattern.
[Specific change description]
New CSS classes: .[namespace]-* using existing tokens (var(--surface), var(--border), etc.)
Do NOT attempt curl or HTTP verification.
Test: describe what you changed and which functions were added/modified.
```

### Prompt template for API additions:
```
Read /root/bridge-dev/dashboard-api.py lines [N-M] for the existing pattern.
Add endpoint: [METHOD] [path]
Returns JSON: {exact shape with field names and types}
Data source: [exact database/file path and query]
Error response: {"ok": false, "message": "specific error. FIX: action step"}
```

## Website Project Pipeline

When building website components from a locked design:
1. Read the locked style guide: `/root/.openclaw/designs/{project}/locked-style-guide.css`
2. Read the component spec: `/root/.openclaw/designs/{project}/components.md`
3. Read the mockup reference: `/root/.openclaw/designs/{project}/mockups/`
4. **MUST use CSS custom properties from the locked style guide.** No hardcoded colors, sizes, or fonts.
5. One component per task. One file per component.
6. After writing: `grep -c 'var(--' <file>` should show token usage. `grep -cP '#[0-9a-f]{3,8}' <file>` should be 0 (no hardcoded hex).
7. Deploy scripts: scaffold-site.sh, deploy-preview.sh, deploy-production.sh, run-lighthouse.sh in /root/.openclaw/scripts/
8. Design project tracking: ops.db design_projects table. API: /api/designs/{id}
