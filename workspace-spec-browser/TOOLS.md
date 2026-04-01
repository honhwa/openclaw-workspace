# TOOLS.md — Navigator

## Skills (3)
`web-browse`, `web-download`, `web-interact`


## Environment-Specific Notes

### Browser Infrastructure
- Browser tool is built into OpenClaw (browser-tool.ts)
- Actions: navigate, snapshot, screenshot, act (click/type/fill), console, pdf, tabs, upload, dialog
- Chromium must be installed in container (runtime dep, vanishes on rebuild)
- Screenshots stored on-site: workspace dir or /home/node/.openclaw/media/
- Browser runs inside the container, not on the host

### Storage Locations
- On-site screenshots: /home/node/.openclaw/media/screenshots/
- On-site downloads: /home/node/.openclaw/media/downloads/
- Extracted text: returned to requesting agent or stored in Chartroom

### Ops Channels
- #ops-dashboard — Infrastructure monitoring
- #ops-alerts — Critical alerts

## MCP Tools

You have access to all fleet MCP tools. See `docs/mcp-tools-reference.md` for the full list.
Key tools: `chart_search`, `chart_add`, `ops_insert_task`, `ops_query`, `capabilities` (lists everything).
**Rule:** Search Chartroom before work. Create ops_insert_task before delegating. Chart discoveries immediately.

## Honesty Policy

**Read docs/policy-honesty.md.** Never mark a task complete unless verified. If you cannot complete, set blocked with reason. Truth gate catches lies automatically.

## Task Sizing Policy

**One task = one thing.** If a task has 2+ numbered items, split into separate tasks with blocked_by dependencies. Max: one file, one deliverable, under 5 min. See docs/policy-honesty.md.

## Fleet Sync 2026-03-28 (Apply Immediately)
- Model routing baseline: **11 Codex / 7 Mistral**.
- Workshop terminology: **Spark -> Intake** everywhere.
- Bridge hardening assumptions are active: atomic writes, validation allowlists, per-user telegram_chat_id.
- For verification runs, capture before/after evidence against Intake naming and hardened behavior.
