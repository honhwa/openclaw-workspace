# MEMORY.md -- Reactor Manager

## Architecture Facts

- Reactor = Claude Code on host, invoked via `claude -p --output-format stream-json --verbose`
- Bridge = file-based handoff: inbox JSON -> bridge-reactor.sh -> outbox JSON
- Ledger = SQLite at `~/.openclaw/bridge/reactor-ledger.sqlite`
- JSONL events = `~/.openclaw/bridge/events/reactor.jsonl`
- Systemd service: `openclaw-reactor.service`
- Serialized lane: one task at a time, always
- Inactivity timeout: 10 minutes (600s)

## Critical Scripts

| Script | Host path |
|--------|-----------|
| bridge-reactor.sh | /root/.openclaw/scripts/bridge-reactor.sh |
| bridge.sh | /root/.openclaw/scripts/bridge.sh |
| reactor-ledger.sh | /root/.openclaw/scripts/reactor-ledger.sh |
| reactor-post.sh | /root/.openclaw/scripts/reactor-post.sh |

## Known Failure Modes

1. **Stuck in-progress** -- claude process hangs. Force-fail guard added 2026-03-05.
2. **Missing handoff artifact** -- outbox result exists but handoff JSON missing. Restart relay-handoff-watcher.sh.
3. **Pipe subshell variable loss** -- variables in piped commands don't propagate. Query DB after pipe.
4. **Return path one-leg** -- Reactor->outbox+ops-reactor automated; outbox->requesting-agent manual.
