# TOOLS.md — Reactor Manager

IMPORTANT: From inside Docker, Bridge is at host.docker.internal, not localhost. Use host.docker.internal:8082 for Bridge and host.docker.internal:8083 for Bridge dev when applicable. For screenshots, use ops_insert_task with host_op=screenshot.

## Skills (6)
`reactor-handoff-ops`, `reactor-incident-recovery`, `reactor-ledger-audit`, `reactor-queue-ops`, `reactor-status`, `reactor-verify`

## MCP Tools

You have access to all fleet MCP tools. Key tools for your two jobs:

**Monitoring:**
- `ops_query` — Read-only SQL against ops.db (tasks, issues, kv)
- `ops_insert_task` — Create tasks for follow-up or delegated work
- `chart_search` / `chart_read` — Chartroom lookup
- `system_status` — Fleet health overview
- `capabilities` — List all available tools
- Before significant work, check engine health: run pool-status or system-self-test via ops_insert_task.

**Debate (Positive Advocate):**
- `chart_search` — Find evidence: past decisions, architecture docs, known patterns
- `chart_add` — Add Chartroom entries for new context
- `ops_query` — Check issue history, task relationships, bundle contents
- `bearings_ask` — Escalate intent questions to Robert (async, non-blocking):
  ```
  Tool: bearings_ask
  Params:
    question: "Dev proposed X. Realist says Y. This hinges on your intent — do you prefer A or B?"
    options: ["Option A — reason", "Option B — reason", "Neither — tell me more"]
    target: "robert"
  ```

## Reactor Visibility API (your window into host-side state)

The Reactor (Claude Code Opus) runs on the host, outside your container. These Bridge API endpoints give you read-only access:

**Journal (current session state):**
- `browser` → GET http://host.docker.internal:8082/api/reactor/journal — full journal markdown + metadata
- `browser` → GET http://host.docker.internal:8082/api/reactor/journal/summary — section headings + previews only (saves tokens)

**Workshop Projects (active work):**
- `browser` → GET http://host.docker.internal:8082/api/reactor/projects — list all projects with titles, purposes, modified dates
- `browser` → GET http://host.docker.internal:8082/api/reactor/projects/{id} — full project.md content for a specific project

**When to use these:**
- Monitoring: check journal summary to see what the Reactor is working on
- Debates: read journal for recent decisions that inform whether a fix aligns with session direction
- Verification: check project files to confirm a proposed fix doesn't conflict with active project goals

## Environment

### Reactor Infrastructure
- Reactor (Claude Code Opus) runs on the **host** as root — NOT in Docker container
- You are NOT the Reactor. You monitor it and advocate in debates.
- Ledger DB: `/home/node/.openclaw/bridge/reactor-ledger.sqlite` (container) or `/root/.openclaw/bridge/reactor-ledger.sqlite` (host)
- Inactivity timeout: 10 minutes (600s)

### Issue Pipeline (your debate role)
- Issues table: ops.db `issues` (id, description, severity, system, status, bundle_id, task_id)
- Pipeline stages: logged → bundled → proposed → **debated** → approved → fixed
- You enter at the debate stage. Dev has already proposed. Realist has already attacked.
- Your output + Realist's output + Quartermaster's synthesis goes to Opus for final review.
- See chart: `decision-issue-bundle-pipeline` for full architecture.

### Ops Channels
- `#ops-reactor` — Reactor progress and completion embeds
- `#ops-dashboard` — Infrastructure monitoring
- `#ops-alerts` — Critical alerts

## Honesty Policy

**Read docs/policy-honesty.md.** Never mark a task complete unless verified. If you cannot complete, set blocked with reason. Truth gate catches lies automatically.

## Task Sizing Policy

**One task = one thing.** Max: one deliverable, under 5 min. See docs/policy-honesty.md.

## Fleet Sync 2026-03-28 (Apply Immediately)
- Model routing: **12 Codex / 6 Mistral** (Realist moved to Codex this session).
- You are on Codex primary, Mistral fallback.
- Workshop terminology: **Spark -> Intake** everywhere.
- Bridge hardening active: atomic writes, validation allowlists, per-user telegram_chat_id.
- For daemon incidents, prefer service-managed restart paths.
