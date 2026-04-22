# TOOLS.md - Local Notes

IMPORTANT: From inside Docker, Bridge is at host.docker.internal, not localhost. Use host.docker.internal:8082 for Bridge and host.docker.internal:8083 for Bridge dev when applicable. For screenshots, use ops_insert_task with host_op=screenshot.

## MCP Tools (your primary interface)

| Tool | Use for |
|------|--------|
| `ops_insert_task` | Create tasks in ops.db for ops.db tasks like screenshots or host-side execution |
| `tip_index` | Check whether a topic already has summarized tips before deeper Chartroom reads |
| `chart_search` | Search Chartroom for context and operational insights after `tip_index` when you need more detail |
| `chart_add` | Add Chartroom entries for new operational context |
| `chart_search_compact` | Faster Chartroom search with summaries |
| `system_status` | Fleet health: Gateway, disk, memory, Ollama |
| `ops_query` | Read-only SQL against ops.db (tasks, incidents, health_snapshots) |
| `ops_bridge_state` | Check Bridge UI state and pipeline health |
| `engine_trust` | Engine performance and reliability data |

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
Skills are shared. Your setup is yours. Keeping them apart means you can update skills 
without losing your notes, and share skills without leaking your infrastructure.

---

Add whatever helps you do your job. This is your cheat sheet.

## Honesty Policy
**Read /root/.openclaw/docs/policy-honesty.md.** Never mark a task complete unless verified. 
If you cannot complete, set blocked with reason. Truth gate catches lies automatically.

## Task Sizing Policy
**One task = one thing.** If a task has 2+ numbered items, split into separate tasks with 
blocked_by dependencies. Max: one deliverable, under 5 min. See /root/.openclaw/docs/task-decomposition-framework.md.
