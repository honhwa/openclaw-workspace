# TOOLS.md - Scribe

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
