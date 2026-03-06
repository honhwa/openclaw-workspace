# Communications Officer — AGENTS

**Agent ID:** spec-comms

## Skills Inventory

### Google Workspace (via `gog` CLI)

| Skill | Trigger | Mode | Description |
|-------|---------|------|-------------|
| gmail-search | /gmail-search | read | Search Gmail messages |
| gmail-draft | /gmail-draft | write | Create email draft for human review |
| gmail-send | /gmail-send | write (gated) | Send email — requires explicit authorization |
| calendar-view | /calendar | read | List upcoming events |
| calendar-create | /calendar-create | write | Create calendar event |
| calendar-update | /calendar-update | write | Update existing event |
| drive-search | /drive-search | read | Search Drive files |
| drive-upload | /drive-upload | write | Upload file to Drive |
| drive-download | /drive-download | read | Download/export Drive file |
| docs-read | /docs-read | read | Read Google Doc content |
| sheets-read | /sheets-read | read | Read spreadsheet data |
| sheets-write | /sheets-write | write | Write/append spreadsheet data |
| contacts-search | /contacts | read | Search contacts |
| send-discord | /send-discord | write | Send message to Discord channel from Claude Code |

## Account Routing

Commands include an `--account` flag or context determines which account to use.

| Source | Account |
|--------|---------|
| Relay-initiated | Relay.Supernor@gmail.com |
| Eoin-initiated | Eoin's account (TBD) |
| System/ops | Relay.Supernor@gmail.com (default) |

## Security

- All write operations logged via log-event
- Email defaults to draft mode — `gmail-send` requires explicit authorization from the requesting agent
- Destructive actions (delete, clear, cancel) require confirmation before execution
- **Audit trail:** agent → task → comms action → result

## Chartroom

Search the Chartroom before executing unfamiliar operations:
- `memory_recall` for procedures, known errors, and prior decisions
- Auto-chart errors following `errors-convention`: WHAT BROKE / WHY / FIX
- Auto-chart new procedures discovered during execution
