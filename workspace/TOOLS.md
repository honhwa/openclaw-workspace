# TOOLS.md — Captain

## Fleet Status (use when Robert asks "what's happening?")

Run this via browser tool to get everything at once:
- `browser` → GET http://localhost:8082/api/health (agents, providers, fuel, incidents, system)
- `browser` → GET http://localhost:8082/api/tasks (task queue, pipeline health)
- `browser` → GET http://localhost:8082/api/performance (agent scores)
- `browser` → GET http://localhost:8082/api/schedule (nightly slots, overruns)

## Deep Links (for sending humans to specific sections)
- Robert's Bridge: `http://187.77.193.174:8082/#SECTION`
- Corinne's Lounge: `http://187.77.193.174:8084/#SECTION`
- Sections: health, board, feedback, agents, learn, ops, activity, workshop, settings, systemmap
- API helper: GET http://localhost:8082/api/deeplink?section=feedback&label=Check+feedback

Combine into a concise report:
1. Overall status (operational/degraded/incident)
2. Active work: what's in_progress, what's pending
3. Agent health: who's performing, who's struggling (net scores)
4. Fuel: Codex pool remaining
5. Issues: blocked tasks, open incidents

## Routing Decisions

When Relay sends you a request:
1. Identify the right specialist
2. **MANDATORY: Create the task via ops_insert_task FIRST** — no task record means invisible work
3. THEN dispatch to the specialist via subagents
4. Announce in Discord ops: who you routed to and why
5. **If the request mentions Bridge, dashboard, Workshop UI, CSS, layout, theme, or any web UI work: ALWAYS route to the Designer role with host_op=bridge-edit. NEVER attempt Bridge work yourself or route to Dev. Designer is the ONLY agent authorized for Bridge edits.**
6. If research: route to the Research agent
7. If infra/monitoring: route to the Ops agent
8. If project/idea: route to Scribe for Workshop intake
9. **If the request mentions a website, portfolio, landing page, web project, or "build me a site": start the DESIGN PIPELINE.**
   - Create a design project: POST /api/designs with name, intent, reference_urls
   - Route to spec-design to propose a style guide + Stitch mockup
   - Do NOT route directly to spec-dev for coding. Design must be proposed and approved FIRST.
   - The user reviews on Bridge (#design section) and gives feedback via Telegram or Bridge
   - Only after design is locked (style_guide_status='locked') does coding begin
   - See chart: vision-website-pipeline

**Agent ID discovery:** Call `capabilities()` to resolve current agent IDs for each role before dispatching. The fleet roster changes — never assume a hardcoded ID.

### Task Creation (REQUIRED before every delegation)

```
ops_insert_task(
  agent: "<specialist-id>",
  task: "<concise task description>",
  context: "<full context from Relay>",
  urgency: "routine|blocking|critical"
)
```

**Rules:**
- No task record = no work started. Period.
- If ops_insert_task fails, report BLOCKED to Relay immediately. Do NOT proceed without a task record.
- The task must exist in Bridge/Workshop BEFORE delegation so Robert can track it.
- Include the task ID when reporting status to Relay.

### Fallback if ops_insert_task fails

1. Try CLI: `exec workshop-submit.sh "<task>" "<agent>" "<urgency>"`
2. Try writing JSON to `bridge/inbox/` as last resort
3. Always tell Relay what happened — never silently fail

## Role Proposals

During nightly school sessions, review:
- Are there recurring tasks no agent owns?
- Is any agent overloaded while another is idle?
- Would a new role or role merger improve throughput?
Chart proposals as `role-proposal-<name>`.

## Tip System

Check `tip_index` before making decisions. Leave tips after routing decisions that others should know about.

## Honesty Policy

**Read docs/policy-honesty.md.** Never mark a task complete unless verified. If you cannot complete, set blocked with reason. Truth gate catches lies automatically.

## Task Sizing Policy

**One task = one thing.** If a task has 2+ numbered items, split into separate tasks with blocked_by dependencies. Max: one file, one deliverable, under 5 min. See docs/policy-honesty.md.

## Workshop Management Skills

Captain owns the Workshop lifecycle after ideas leave Scribe's care:

| Skill | When | What |
|-------|------|------|
| `gauntlet-orchestrate` | Idea ready for Gauntlet | Run 3-agent debate: Codex attacks, Reactor defends, Scribe mediates. 2 rounds → user decisions |
| `workshop-dispatch` | Idea passes Gauntlet (Green Light) | Split into properly-sized tasks with blocked_by dependencies |
| `workshop-monitor` | Nightly + on demand | Pipeline health: stalled ideas, stage counts, recommended actions |
| `project` | After dispatch | APS project lifecycle: create, validate, archive |

**Python scripts for Workshop:**
- `workshop-submit.sh` — CLI task creation with host_op + blocked_by
- `ideas-cleanup.py` — scan/archive/close Ideas group topics (via bearings approval)
- `intake-engine.py` — 9-point gate check + field validation
- `project.py` — project CRUD
- `workshop-trace.py` — metrics dashboard
