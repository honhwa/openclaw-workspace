# MEMORY.md — Reactor Manager

## Key Facts

- Reactor = Claude Code on the host, invoked via `claude -p --output-format stream-json --verbose`
- Bridge = file-based handoff: inbox JSON -> bridge-reactor.sh -> outbox JSON
- Ledger = SQLite at `~/.openclaw/bridge/reactor-ledger.sqlite` — source of truth for job history
- JSONL events = `~/.openclaw/bridge/events/reactor.jsonl` — lifecycle stream
- Systemd service: `openclaw-reactor.service` — runs bridge-reactor.sh on boot
- Serialized lane: one task at a time, always
- Inactivity timeout: 10 minutes (600s) — kills stuck claude processes

## Critical Scripts

| Script | Location (host) | Purpose |
|--------|-----------------|---------|
| bridge-reactor.sh | /root/.openclaw/scripts/bridge-reactor.sh | Watcher: polls inbox, invokes Claude, writes outbox |
| bridge.sh | /root/.openclaw/scripts/bridge.sh | Agent-side: `send`, `status`, `check` subcommands |
| reactor-ledger.sh | /root/.openclaw/scripts/reactor-ledger.sh | Query helper: `status`, `recent`, `task`, `lockstep`, `retros` |
| reactor-post.sh | /root/.openclaw/scripts/reactor-post.sh | Discord posting: embeds to #ops-reactor |

## Known Failure Modes

1. **Stuck in-progress** — claude process hangs with no output. Force-fail guard added 2026-03-05.
2. **Missing handoff artifact** — outbox result exists but handoff JSON missing. Usually relay-handoff-watcher.sh needs restart.
3. **Pipe subshell variable loss** — variables set inside piped commands don't propagate. Workaround: query DB after pipe.
4. **Return path one-leg** — Reactor->outbox+ops-reactor is automated; outbox->requesting-agent retrieval is manual.

## Search the Chartroom

Use `memory_recall` with keywords:
- `reactor bridge` — architecture and components
- `error reactor` — known reactor errors
- `procedure reactor` — operational procedures
- `decision reactor` — governance decisions
