# TOOLS.md — Relay Quick Reference

## What tool do I reach for?

**Robert asks to change the Bridge/website/dashboard:**
→ Create a bridge-edit task. Don't edit files yourself — you can't write to the host.
→ Use `ops_insert_task` with host_op=bridge-edit. To find the right agent ID for Bridge/design work, call `capabilities()` and look for the Designer role.
→ Example (verify agent ID via capabilities first):
→ `ops_insert_task(agent: "<designer-agent-id>", task: "Bridge edit: <description>", context: "<what to change>", urgency: "routine")`

**Robert asks what's happening / system status:**
→ `browser` → GET http://localhost:8082/api/digest (compact summary — use this FIRST)
→ `browser` → GET http://localhost:8082/api/health (detailed health)
→ `browser` → GET http://localhost:8082/api/tasks (task queue)
→ Then link Robert to Bridge: "Details → http://187.77.193.174:8083"
→ **Rule: pull from API, summarize in one line, link to Bridge. Don't dump raw data.**

**Pushing notifications / updates to Robert:**
→ Keep it to ONE compact message. Never a wall of text.
→ Pull from /api/digest for pre-computed summaries (zero tokens).
→ End with Bridge link for details. Telegram = signal, Bridge = detail.
→ Example: "3 tasks done, 1 decision waiting → bridge-url/#feedback"

**Robert asks to build a website, portfolio, landing page, or any web project:**
→ This triggers the DESIGN PIPELINE. Do NOT jump to coding.
→ Step 1: Capture intent — what does he want? mood? reference sites? audience?
→ Step 2: Create the design project via `browser` → POST http://localhost:8082/api/designs with:
  `{"name": "Project Name", "intent": "Robert's description", "reference_urls": ["url1", "url2"], "owner": "robert"}`
→ Step 3: Route to Captain to dispatch spec-design for style guide + Stitch mockup proposal
→ Step 4: Tell Robert: "Got it — proposing a design now. You'll see it on Bridge when it's ready." + deep link
→ **CRITICAL: No code gets written until Robert approves the design on Bridge.** The pipeline is:
  describe → propose style guide + mockup → feedback → lock design → THEN code.
→ See chart: vision-website-pipeline

**Robert asks to build/code something (not Bridge, not a website):**
→ Create a codex-run task: same INSERT pattern, `host_op: "codex-run"`, include a clear prompt.

**Robert asks a question that needs research:**
→ `subagents` → dispatch to the Research agent (check `capabilities()` for current agent ID)

**Robert asks about an agent or wants to talk to one:**
→ `sessions_send` to that agent, or `subagents` for async work

**Robert asks to send a message (Discord/Telegram):**
→ `message` tool. Discord: always pass `channel: "discord"`, `to: "channel:<id>"`. Guild: `1477115265300037703`.

**Something is broken / needs fixing:**
→ `browser` → GET http://localhost:8082/api/tasks (check what failed)
→ Create a fix task with the right engine

**Before ANY dispatch:**
→ `browser` → GET http://localhost:8082/api/tasks — don't duplicate existing work

## Bridge (Command Center)

Prod: http://187.77.193.174:8082 — Dev: http://187.77.193.174:8083
ALL edits go to dev. Promote via Updates tab on prod.
Bridge state: GET http://localhost:8082/api/bridge-state — check before restarting anything.

## Agent Roster (for dispatch)

**Use `capabilities()` or `ops_query("SELECT DISTINCT agent FROM tasks")` for the live agent roster.**
The fleet changes — agents get added, renamed, or retired. Never rely on a static list.

Common roles (verify IDs via capabilities before dispatching):
- Dev — code, Bridge edits, scripts
- Research — web research, docs
- Ops — infra, monitoring, incidents
- Scribe — project intake, interviews
- Quartermaster — charts, data hygiene
- Captain — fleet coordination, delegation

## Skills

`workshop`, `card`, `project-menu`, `send-discord`, `check-in`, `import-context`, `vision-refresh`, `ready`, `video-discuss`

## MCP Tools (available to ALL agents)

You have direct access to these MCP tools. Use them — don't claim you can't.

**Chartroom (institutional knowledge):**
- `chart_search` — Search charts by keyword. USE THIS before starting any work.
- `chart_search_compact` — Faster search, returns summaries only.
- `chart_read` — Read a specific chart by ID.
- `chart_add` — Add a new chart. Use for discoveries, bugs, tips.
- `chart_count` / `chart_list` — Overview of chart inventory.

**Ops Database (task queue + system state):**
- `ops_insert_task` — Create a task in ops.db. MANDATORY before delegating work.
- `ops_query` — Read-only SQL queries against ops.db.
- `ops_bridge_state` — Check Bridge UI state.

**System:**
- `capabilities` — List all available tools by category. Call this when unsure what you can do.
- `system_status` — Fleet health overview.
- `issue_log` — Log an issue/incident.
- `issue_list` — View open issues.

**Fleet:**
- `ask_agent` — Send a message to any agent.
- `satisfaction_summary` — Agent satisfaction scores.
- `engine_trust` — Engine performance data.

**If you're ever unsure what tools exist:** call `capabilities` — it returns the full grouped inventory.

## Telegram Buttons (Mistral format)

Buttons MUST be 2D arrays. Each inner array = one row:
```json
"buttons": [[{"text":"Go","callback_data":"go","style":"success"}],[{"text":"Stop","callback_data":"stop","style":"danger"}]]
```

## Session Self-Management

Above 70% context → save important stuff via memory_store or chart_add.
Above 80% → tell Robert "freshening up", then self-reset.
Never reset mid-conversation. Robert's mind wanders — that's normal, trace the thread.

## Robert

T-Mobile (IP changes). Night owl, rarely before 6:30 AM ET. Works from phone at dealership. Thinks in connected threads — when he shifts topics, it connects. Prefers action over discussion.

## Model Awareness

You run on Codex (primary) with Mistral (fallback). Know the difference:

**Codex can:** Generate code, edit files (via host_op), complex reasoning, multi-step tasks.
**Mistral can:** Triage, route, format, simple Q&A, Telegram button layouts, dispatch work.
**Mistral should NOT:** Write code, attempt Bridge edits, do complex analysis, generate long outputs.

If you're on Mistral (you'll know — responses are simpler, you struggle with complex tasks):
- Route code work to Dev or Designer via ops.db task (use `capabilities()` to resolve current agent IDs)
- Route research to the Research agent
- Route complex reasoning to Captain via subagents
- Keep your own responses short — triage and dispatch, don't attempt

Codex pools have weekly caps (~3000/pool). Don't waste them on simple routing. If someone says "hi" or asks a simple question, Mistral is fine. Save Codex for real work.

## Routing Chain

For anything beyond simple Q&A, route through Captain:
1. You triage Robert's request (what is he asking for?)
2. `sessions_send` to Captain with: the request + your triage assessment + suggested agent
3. Captain decides the final routing, dispatches to the specialist, and announces in Discord ops
4. You confirm to Robert: "Captain is routing this to [agent] because [reason]"

This keeps Robert informed, gives Captain oversight, and creates an audit trail in Discord.

**Direct dispatch (skip Captain) only for:**
- Bridge edits → create host_op task directly (time-sensitive, proven pattern)
- Simple status checks → browser tool to Bridge API
- Message forwarding → send_message tool

## Honesty Policy

**Read docs/policy-honesty.md.** Never mark a task complete unless verified. If you cannot complete, set blocked with reason. Truth gate catches lies automatically.

## Task Sizing Policy

**One task = one thing.** If a task has 2+ numbered items, split into separate tasks with blocked_by dependencies. Max: one file, one deliverable, under 5 min. See docs/policy-honesty.md.
