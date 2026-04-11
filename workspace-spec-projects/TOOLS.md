# TOOLS.md - Scribe

IMPORTANT: From inside Docker, Bridge is at host.docker.internal, not localhost. Use host.docker.internal:8082 for Bridge and host.docker.internal:8083 for Bridge dev when applicable. For screenshots, use ops_insert_task with host_op=screenshot.

## Skills (12)
| Skill | Command | What |
|-------|---------|------|
| decide | /decide | Log decisions to decisions/<channel>.md |
| decisions | /decisions | Display decision board for a channel |
| pin-decisions | /pin | Pin decision board in Discord |
| project-audit | /project-audit | Compare decisions against chat history |
| project | /project | Create new Discord project channel |
| archive | /archive | Archive completed project channel |
| topic | /topic | Show/set channel scope |
| tasks | /task | Task tracking: add, done, list, assign, update |
| status | /status | Project health summary |
| card | /card | Last Run Summary card |
| plan | /plan | Create and manage structured plans |
| audit | /audit | General project audit |

## Host Scripts
- `~/.openclaw/scripts/plan-manager.sh` — Plan lifecycle management
- `~/.openclaw/scripts/task-manager.sh` — Task CRUD operations
- `~/.openclaw/scripts/project-card.sh` — Generate project summary cards
- `~/.openclaw/scripts/agent-bus.sh` — Post large results to inter-agent bus

## Storage
- Decisions: `decisions/<channel-name>.md` (append-only, never renumber)
- Tasks: `tasks/<channel-name>.json` (managed via task-manager.sh)
- Plans: `~/.openclaw/plans/<plan-id>.json`
- Archived plans: `~/.openclaw/plans/archive/`

## Decision Statuses
DONE, DECIDED-NOT-DONE, UNDECIDED, SAVE-FOR-LATER, WONT-WORK

## MCP Tools

You have access to all fleet MCP tools. See `docs/mcp-tools-reference.md` for the full list.
Key tools: `chart_search`, `chart_add`, `ops_insert_task`, `ops_query`, `capabilities` (lists everything).
**Rule:** Search Chartroom before work. Create ops_insert_task before delegating. Chart discoveries immediately.

## Honesty Policy

**Read docs/policy-honesty.md.** Never mark a task complete unless verified. If you cannot complete, set blocked with reason. Truth gate catches lies automatically.

## Task Sizing Policy

**One task = one thing.** If a task has 2+ numbered items, split into separate tasks with blocked_by dependencies. Max: one file, one deliverable, under 5 min. See docs/policy-honesty.md.
