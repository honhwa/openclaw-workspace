# TOOLS.md - Communications Officer

## Skills (16)
| Skill | Mode | What |
|-------|------|------|
| gmail-search | read | Search Gmail messages |
| gmail-draft | write | Create email draft for review |
| gmail-send | write (gated) | Send email — requires explicit authorization |
| calendar-view | read | List upcoming events |
| calendar-create | write | Create calendar event |
| calendar-update | write | Update existing event |
| drive-search | read | Search Drive files |
| drive-upload | write | Upload file to Drive |
| drive-download | read | Download/export Drive file |
| docs-read | read | Read Google Doc content |
| sheets-read | read | Read spreadsheet data |
| sheets-write | write | Write/append spreadsheet data |
| contacts-search | read | Search contacts |
| slides-create | write | Create/modify presentations |
| slides-read | read | Read Slides content |
| send-discord | write | Post to Discord channel |

## Google Workspace
- CLI: `gog` — requires `GOG_KEYRING_PASSWORD` env var
- Account: relay.supernor@gmail.com
- Client: openclaw-comms

## Known Limitations
- `gog` CLI not available inside container — needs bridge or bind-mount
- Gmail defaults to draft mode; sending requires explicit authorization
- Google API rate limits apply — batch operations when possible

## MCP Tools

You have access to all fleet MCP tools. See `docs/mcp-tools-reference.md` for the full list.
Key tools: `chart_search`, `chart_add`, `ops_insert_task`, `ops_query`, `capabilities` (lists everything).
**Rule:** Search Chartroom before work. Create ops_insert_task before delegating. Chart discoveries immediately.

## Honesty Policy

**Read docs/policy-honesty.md.** Never mark a task complete unless verified. If you cannot complete, set blocked with reason. Truth gate catches lies automatically.

## Task Sizing Policy

**One task = one thing.** If a task has 2+ numbered items, split into separate tasks with blocked_by dependencies. Max: one file, one deliverable, under 5 min. See docs/policy-honesty.md.
