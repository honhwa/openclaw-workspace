# openclaw-workspace

Agent workspace configuration files for a multi-agent [OpenClaw](https://github.com/openclaw/openclaw) deployment running 4 specialized agents.

## Agents

| Agent | ID | Workspace | Role |
|-------|----|-----------|------|
| Relay | `relay` | workspace-relay | Human-facing. All user communication. |
| Captain | `main` | workspace | Router/dispatcher. No direct execution. |
| Repo-Man | `spec-github` | workspace-spec-github | Infra, keys, backups, GitHub, model health |
| Quartermaster | `spec-projects` | workspace-spec-projects | Projects, decisions, auditing |

## Key Concepts

### Scoped Context

Every token in an agent's context must serve that agent's specific job. Shared documents use RACP (Role-Aware Context Projection) markers to generate per-agent targeted versions, reducing token waste by ~76% compared to blanket context.

### Filesystem Communication Bus

Agents communicate with external tools (like Claude Code on the host VPS) via:
- **Shared registry** (`registry.json`) — single source of truth for all IDs and constants
- **Task inbox/outbox** — structured file-based handoff protocol
- **SQLite ops database** (`ops.db`) — shared operational history

## Workspace Files

Each agent workspace contains some combination of:

| File | Purpose |
|------|---------|
| `AGENTS.md` | Agent-specific instructions, tools, skills, rules |
| `SOUL.md` | Personality, routing table, behavioral guidelines |
| `TOOLS.md` | Available tools and how to use them |
| `IDENTITY.md` | Agent identity and voice |
| `USER.md` | User preferences and communication style |
| `HEARTBEAT.md` | Periodic check instructions |
| `BOOTSTRAP.md` | Session startup protocol |
| `DISCORD-REFERENCE.md` | Targeted Discord capabilities (per-agent) |

## AI Attribution

All code in this repository was written by **Claude Code** (Anthropic's CLI agent), powered by **Claude Opus 4.6**.

- Human: Robert Supernor — product direction, priorities, review
- AI: Claude Code — implementation, architecture, agent design

Every commit includes:
```
Co-Authored-By: Claude Opus 4.6 <noreply@anthropic.com>
```
