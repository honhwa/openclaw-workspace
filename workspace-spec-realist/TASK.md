# TASK: Governance Audit — Incident Chart Compliance

**OBJECTIVE**
Audit `CHARTS/INCIDENT-REACTIVITY-20260322.md` for compliance with fleet governance standards:

## Requirements

1. **Sections Check**
   - Ensure the chart contains these mandatory headings: `# WHAT`, `# WHY`, `# FIX`, `# IMPACT`.

2. **Actionable FIX**
   - Verify the FIX section lists **executable steps** (not vague guidance).
   - Confirm each step has a clear owner (Reactor, Ops, Captain, etc.).

3. **Date Check**
   - Validate `VERIFIED: <date>` uses ISO-8601 format (`YYYY-MM-DD`).
   - Confirm date is ≤24h old (graph incident escalation timeline).

## Constraints
- Use **workspace-local files** only (`/home/node/.openclaw/workspace-main`).
- **Avoid MCP tools** (gateway unreliable; use `read`, `write`, `exec`).
- **Update the chart in-place** if gaps are found (use `## GAPS` section template).

## Deliverables
- Updated chart file (if audit finds gaps).
- Short execution summary appended to `/home/node/.openclaw/memory/realist-audit-20260322.log`.

## Example GAPS Section
```markdown
## GAPS
- **FIX**: Step 2 lacks owner assignment.
- **VERIFIED**: Date misformatted (`Mar 22 206` → `2026-03-22`).

INTENT: Governance [I07]
```

**Intent**: Governance [I07]
**Verification**: Append logs to `realist-audit-20260322.log`.