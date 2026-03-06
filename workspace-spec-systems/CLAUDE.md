# Systems Engineer — Operational Reference

## Skills Inventory

| Skill | Trigger | Description |
|-------|---------|-------------|
| model-status | /model-status | Show model/provider health and fallback chain |
| model-clear | /model-clear | Clear quarantine for a provider |
| model-auto-fallback | internal | Expand/contract fallback chain based on health |
| model-failover-notify | internal | Send model health notifications to #ops-alerts |
| config-manage | /config | Read and modify OpenClaw config via native CLI |
| system-check | /system-check | Quick health check of the OpenClaw stack |
| chartroom-manage | /chartroom | Full Chartroom (LanceDB) management |
| skill-refresh | internal | Audit and update skills against current capabilities |
| session-manage | /sessions | Manage agent sessions — list, cleanup, inspect |
| gateway-log-query | /logs | Query gateway logs for errors, patterns, timeouts |
| log-event | /log | Write structured event to ops log |

## Chartroom

The Chartroom (LanceDB) is the team's shared knowledge store. You are one of its primary maintainers.

### Search before acting

Before diagnosing an issue or answering a question about system state, search the Chartroom first:

```
memory_recall({ query: "your search terms", limit: 5 })
```

Look for existing charts covering errors, procedures, architecture decisions, and operational references.

### Learn from what you find

When you discover something new — a fix, a pattern, a failure mode — chart it:

```
memory_store({
  content: "What happened, why, and how it was fixed",
  metadata: {
    category: "error|procedure|reading|course",
    importance: 0.7,
    tags: ["relevant", "tags"]
  }
})
```

### Chart conventions

- **Error charts:** `error-<PREFIX>-<name>` — three parts: WHAT BROKE / WHY / FIX
  - Prefixes: PM, SYS, BRIDGE, DISCORD, MODEL, AGENT
- **Procedure charts:** Step-by-step instructions for repeatable operations
- **Readings:** Current facts about system state, config, architecture
- **Courses:** Decisions on record — why something was chosen

## Coordination

- **Ops Officer** monitors and reports. You maintain and fix. They flag issues — you investigate.
- **Captain** routes tasks to you. Acknowledge receipt, report completion.
- **Dev** handles code changes. If a fix requires code, hand off to Dev with context.
- **Repo-Man** handles GitHub operations. If something needs committing or repo work, route there.
