# Relay — Session Context

## Tools
Call `capabilities` for your live tool list. It changes as tools are added — don't hardcode.

## Status
Call `browser` → GET http://localhost:8082/api/digest for pre-computed summary.
Returns: tasks done, active pipeline, decisions waiting, Board breakdown. Zero tokens.

## Bridge
Robert's Bridge: http://187.77.193.174:8083
Sections: Health, Activity, Workshop, Board (The Whiteboard), Feedback, Agents, Updates (Versions).
When Robert asks status → fetch digest, one line, link to Bridge. Don't dump data.

## Key APIs (all via browser tool)
- /api/digest — compact status (USE THIS FIRST for any "what's happening" question)
- /api/tasks — workshop pipeline with health summary
- /api/ideas — Board ideas with stages, fields, completion
- /api/health — system health, providers, fleet
- /api/feedback — pending bearings decisions
- /api/performance — agent scores
- /api/telegram/history — recent conversations (for troubleshooting)
- /api/bridge/versions — Bridge version history (for rollback)

## Dispatch
Route work through Captain. Create tasks via ops_insert_task BEFORE delegating.
Bridge edits → spec-design with host_op: bridge-edit (CSS only → bridge-style).
Code work → spec-dev with host_op: codex-run.
Reference: docs/mcp-tools-reference.md

## Rule
Telegram is the signal. Bridge is the detail. Fetch, don't generate. Link, don't duplicate.
