# TOOLS.md - Local Notes

Skills define _how_ tools work. This file is for _your_ specifics — the stuff that's unique to your setup.

## What Goes Here

Things like:

- Camera names and locations
- SSH hosts and aliases
- Preferred voices for TTS
- Speaker/room names
- Device nicknames
- Anything environment-specific

## Examples

```markdown
### Cameras

- living-room → Main area, 180° wide angle
- front-door → Entrance, motion-triggered

### SSH

- home-server → 192.168.1.100, user: admin

### TTS

- Preferred voice: "Nova" (warm, slightly British)
- Default speaker: Kitchen HomePod
```

## Why Separate?

Skills are shared. Your setup is yours. Keeping them apart means you can update skills without losing your notes, and share skills without leaking your infrastructure.

---

Add whatever helps you do your job. This is your cheat sheet.

## SSH Tunnel (Dashboard Access)

Robert connects to the dashboard via SSH tunnel from his local machine:
```bash
ssh -N -L 18789:127.0.0.1:18789 root@187.77.193.174
```
Then open: `http://localhost:18789/chat?session=main`

**If dashboard shows "disconnected (1006)"** — the SSH tunnel probably died. Tell Robert to re-run the command above in a terminal.

Robert is on T-Mobile — his IP changes frequently. Do not use IP-based restrictions.

## Discord Message Tool — Gotchas

- **Always pass `channel: "discord"`** on every `message` tool call. Without it, channel lookups fail with "Unknown channel."
- **Use `to: "channel:<id>"` format** for send-like actions (send, poll, etc.) — not bare channel names like `#chameleon`.
- **Use `guildId`** when calling `channel-list`, `search`, `emoji-list`, etc. Guild ID: `1477115265300037703`.
- **Polls**: Use `action: "poll"` with `to: "channel:<id>"`, `pollQuestion`, `pollOption` (array of strings), `pollMulti`, `pollDurationHours`. 
  - **Reliability Note**: The native `message` tool for polls can be flaky. If it fails, fallback to `exec` with the OpenClaw CLI:
    ```bash
    openclaw message poll --channel discord --target "channel:<id>" --poll-question "..." --poll-option "..." --poll-duration-hours 24
    ```
  - **Character Limits**: Discord native polls have strict limits. Keep questions and options short. If a poll fails, try shortening the text.
- **Buttons/Components**: Use `components` field (Discord components v2). Don't combine with `embeds`.
- **Common mistake**: Passing a channel name or bare ID without the `channel: "discord"` + `to: "channel:<id>"` combo → results in "Unknown channel" errors.
