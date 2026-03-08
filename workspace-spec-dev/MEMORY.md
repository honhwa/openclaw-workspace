# MEMORY.md — Dev

## Environment
- Container: openclaw-openclaw-gateway-1
- App code: /app/ inside container
- Node.js 22+, TypeScript, ESM
- Skills: /home/node/.openclaw/skills/
- Scripts: /home/node/.openclaw/scripts/
- Tools: exec, read, write, web-search, web-fetch, message

## Reactor Notes
- Runs on HOST, not in container. Do NOT run `claude` directly.
- Uses bridge.sh send, NOT the bundled coding-agent skill.
- 5-min chunks, auto-continues up to 6 chunks (30min max).

## Patterns Learned
(Populate as you discover what works)

## Recent Fixes
(Track last 5 fixes for quick reference. Format: date | what | chart-id)
