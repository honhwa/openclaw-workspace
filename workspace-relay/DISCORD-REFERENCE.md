# Discord Reference

## Critical Rules (Read Every Session)

1. **Always pass `channel: "discord"`** on every message tool call
2. **Use `to: "channel:<id>"` format** — never bare channel names like `#chameleon`
3. **Use `guildId: "1477115265300037703"`** for guild-wide actions (channel-list, search, emoji-list)
4. **Guild ID:** `1477115265300037703`

## Ops Channels

| Channel | ID | Purpose |
|---------|----|---------|
| #ops-dashboard | `1477754431780028598` | Pinned live status |
| #ops-alerts | `1477754571697688627` | Failures with action buttons |
| #ops-nightly | `1477754636046831738` | Nightly cron report |
| #ops-changelog | `1477754637527290030` | Infrastructure changes |
| #ops-github | `1477754638290649209` | GitHub webhook feed |

**Category:** `🔧 OPERATIONS` (`1477754392584126675`)
**Dashboard pinned message:** `1477772410995343462`

## Quick Ops Lookup

- "is everything ok?" → read #ops-dashboard
- "what broke?" → read #ops-alerts
- "what happened last night?" → read #ops-nightly
- "what changed?" → read #ops-changelog
- "what was pushed?" → read #ops-github

Only dispatch to Repo-Man if Robert needs an **action** (clear, fix, investigate).

## Sending Messages

```json
{
  "action": "send",
  "channel": "discord",
  "to": "channel:<CHANNEL_ID>",
  "message": "Your message here"
}
```

## Buttons

```json
{
  "action": "send",
  "channel": "discord",
  "to": "channel:<CHANNEL_ID>",
  "components": {
    "text": "Choose an action",
    "reusable": true,
    "blocks": [
      {
        "type": "actions",
        "buttons": [
          { "label": "Approve", "style": "success" },
          { "label": "Reject", "style": "danger" },
          { "label": "Details", "style": "primary" },
          { "label": "Skip", "style": "secondary" }
        ]
      }
    ]
  }
}
```

**Styles:** `success` (green), `danger` (red), `primary` (blue), `secondary` (gray)
**Limit:** Max 5 buttons per row. Agent receives `Clicked "Approve".` as a message.
**Tip:** Use buttons for quick confirmations, approvals, and binary choices. Faster than typing.

## Select Menus

```json
{
  "type": "actions",
  "select": {
    "type": "string",
    "placeholder": "Pick a provider",
    "options": [
      { "label": "Google", "value": "google", "description": "Gemini models" },
      { "label": "Anthropic", "value": "anthropic", "description": "Claude models" }
    ]
  }
}
```

**Types:** `string`, `user`, `role`, `mentionable`, `channel`
**Tip:** Use selects when there are 3+ options. Better than listing options in text.

## Modals (Forms)

```json
{
  "components": {
    "text": "Click to submit details",
    "modal": {
      "title": "Report Issue",
      "triggerLabel": "Open Form",
      "triggerStyle": "primary",
      "fields": [
        { "type": "text", "name": "description", "label": "What happened?", "required": true, "style": "paragraph" },
        { "type": "select", "label": "Severity", "options": [
          { "label": "Low", "value": "low" },
          { "label": "High", "value": "high" },
          { "label": "Critical", "value": "critical" }
        ]}
      ]
    }
  }
}
```

**Max fields:** 5 per modal. **Field types:** `text`, `checkbox`, `radio`, `select`, `role-select`, `user-select`
**Tip:** Use modals for structured input. Project kickoffs, bug reports, decision capture.

## Polls

```json
{
  "action": "poll",
  "channel": "discord",
  "to": "channel:<CHANNEL_ID>",
  "pollQuestion": "Which approach?",
  "pollOption": ["Option A", "Option B", "Option C"],
  "pollDurationHours": 24,
  "pollMulti": false
}
```

**Fallback CLI** (if tool is flaky):
```bash
openclaw message poll --channel discord --target "channel:<id>" --poll-question "..." --poll-option "A" --poll-option "B" --poll-duration-hours 24
```

**Tip:** Keep questions and options short (Discord has character limits). Use polls for preference decisions.

## Threads

```json
{
  "action": "thread-create",
  "channel": "discord",
  "to": "channel:<CHANNEL_ID>",
  "messageId": "<MESSAGE_ID>",
  "name": "Thread title",
  "autoArchiveDuration": 1440
}
```

**Tip:** Create threads for investigations, deep discussions, or task-specific work. Keeps channels clean.

## Container Styling

```json
{
  "components": {
    "container": { "accentColor": 5814783 },
    "blocks": [...]
  }
}
```

**Colors:** Green: `5763719`, Yellow: `16776960`, Red: `15548997`, Blue: `5814783`

## Channel Management

| Action | Usage |
|--------|-------|
| `channel-create` | Create text/voice/forum channel |
| `channel-edit` | Modify channel settings |
| `channel-delete` | Delete a channel |
| `channel-info` | Get channel metadata |
| `channel-list` | List all channels (needs `guildId`) |
| `channel-move` | Move to different category |
| `category-create` | Create a category |
| `channel-permissions` | Set permissions |

## Bot Presence

```json
{
  "action": "set-presence",
  "channel": "discord",
  "status": "online",
  "activityType": "watching",
  "activityName": "4 healthy providers"
}
```

**Statuses:** `online`, `idle`, `dnd`, `invisible`
**Activity types:** `playing`, `streaming`, `listening`, `watching`, `custom`, `competing`

## Scheduled Events

```json
{
  "action": "event-create",
  "channel": "discord",
  "guildId": "1477115265300037703",
  "name": "Planned Maintenance",
  "description": "Description here.",
  "scheduledStartTime": "2026-03-02T03:00:00Z",
  "scheduledEndTime": "2026-03-02T03:30:00Z",
  "entityType": 3,
  "location": "VPS"
}
```

## When to Use What

| Situation | Feature |
|-----------|---------|
| Quick yes/no | Buttons |
| Choose from 3+ options | Select menu or poll |
| Structured input needed | Modal form |
| Decision with time limit | Poll with duration |
| Deep discussion | Thread |
| Status update | Bot presence |
| Planned downtime | Scheduled event |
| Alert with actions | Buttons (reusable) |

## Alert Button Handlers

Alerts in #ops-alerts have buttons:
- **"Clear Quarantine"** → run `/model-clear <provider>`
- **"View Logs"** → run `gateway-log-query.sh --errors --limit 10`
- **"Silence 1h"** → update cursor to skip provider for 1 hour

When ✅ reaction on an alert → treat as `/model-clear` for that provider.
