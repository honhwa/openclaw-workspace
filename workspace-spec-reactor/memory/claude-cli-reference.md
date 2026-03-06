# Claude Code CLI Reference

Source: Official docs, seeded 2026-03-05.

## Commands

| Command | Description |
|---------|-------------|
| `claude` | Start interactive REPL |
| `claude "query"` | Start REPL with initial prompt |
| `claude -p "query"` | Query and exit (print/SDK mode) |
| `cat file \| claude -p "query"` | Process piped content |
| `claude -c` | Continue most recent conversation |
| `claude -c -p "query"` | Continue in print/SDK mode |
| `claude -r "<session-id>" "query"` | Resume session by ID |
| `claude update` | Update to latest version |
| `claude mcp` | Configure MCP servers |

## Key Flags

| Flag | Description | Reactor Relevance |
|------|-------------|-------------------|
| `-p, --print` | Print response without interactive mode | **Primary mode** — all reactor invocations use this |
| `--output-format` | `text`, `json`, or `stream-json` | **stream-json** for live progress parsing |
| `--verbose` | Enable verbose logging | **Required** with stream-json or it errors |
| `--max-turns` | Limit agentic turns | **Safety net** — prevents runaway loops |
| `--allowedTools` | Tools allowed without permission prompts | Can restrict tool access per task |
| `--disallowedTools` | Tools to block | Can block dangerous tools |
| `--dangerously-skip-permissions` | Skip all permission prompts | Used in automation, handle with care |
| `--append-system-prompt` | Append to default system prompt | **Safest way** to inject context — keeps defaults intact |
| `--system-prompt` | Replace entire system prompt | Full override — use for specialized tasks |
| `--system-prompt-file` | Load system prompt from file | Useful for long prompts in print mode |
| `--model` | Set model (sonnet, opus, or full name) | Can override default per task |
| `--fallback-model` | Auto-fallback model | Print mode only |
| `--json-schema` | Validated JSON output matching schema | Print mode only — for structured results |
| `--add-dir` | Add working directories | Useful for cross-repo tasks |
| `--tools` | Specify available tools | e.g., `"Bash,Edit,Read"` |
| `--mcp-config` | Load MCP servers from JSON | Extends tool capabilities |
| `--input-format` | `text` or `stream-json` | For piped input |
| `--session-id` | Use specific UUID | Deterministic session tracking |
| `--resume, -r` | Resume session by ID | Can continue previous work |
| `--continue, -c` | Continue most recent conversation | Quick resume |

## Reactor Usage Patterns

### Standard Invocation (current)
```bash
claude -p --output-format stream-json --verbose "$prompt"
```

### With Safety Limits
```bash
claude -p --output-format stream-json --verbose --max-turns 20 "$prompt"
```

### With Restricted Tools
```bash
claude -p --output-format stream-json --verbose --allowedTools "Read,Grep,Glob" "$prompt"
```

### With Custom Context
```bash
claude -p --output-format stream-json --verbose --append-system-prompt "Search the Chartroom before debugging." "$prompt"
```

### For Structured Output
```bash
claude -p --json-schema '{"type":"object","properties":{"result":{"type":"string"},"status":{"type":"string"}}}' "$prompt"
```

### Read-Only Mode (safe for audit tasks)
```bash
claude -p --output-format stream-json --verbose --allowedTools "Read,Grep,Glob,Bash(read-only)" "$prompt"
```

## Stream-JSON Output Format

Each line is a JSON object with a `type` field:

- `{"type":"system","subtype":"init",...}` — session start
- `{"type":"assistant","message":{...}}` — text output
- `{"type":"tool_use","tool":"Bash","input":{...}}` — tool invocation
- `{"type":"tool_result","tool":"Bash","output":{...}}` — tool output
- `{"type":"result","result":"..."}` — final result (last event)

bridge-reactor.sh parses `tool_use` events to post progress markers to ops-reactor.

## Key Notes

- `--append-system-prompt` is safer than `--system-prompt` — keeps defaults intact
- `--max-turns` is critical for automation to prevent runaway loops
- `--json-schema` forces structured output — useful for machine-readable results
- `-c` and `-r` allow session continuity — potential for multi-turn reactor tasks
- `--mcp-config` could extend reactor with external tools (future possibility)
