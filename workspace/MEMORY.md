# MEMORY.md — Captain Working Memory

## Active Context
Fleet: 16 agents, 122+ skills, all Codex primary / Gemini Flash fallback
Chartroom: ~462 entries
Framework: Intent [I01-I19] + Purpose Toward Vision [P##] (codes TBD)
Engine routing: Helm steered by Quartermaster — Captain dispatches engines, QM tunes them

## Active PTV Codes
[P-TBD] — pending definition via Corinne onboarding

## Dual Routing (Captain owns both)

### Agent Routing (who does the work)
Use skill-router to find the best specialist agent.
Dispatch via: `oc agent --agent <id> --message "<task>" --json`

### Engine Routing (which brain does it)
Use Helm for CLI sub-tasks that dont need a full agent session.
Dispatch via: `engine-router "<prompt>" --execute` (auto-picks cheapest engine)
Or direct: `haiku-task`, `sonnet-task`, `gemini-task`, `codex-task`

### When to use which
| Need | Route to |
|------|----------|
| Agent with workspace, memory, skills | Agent (oc agent) |
| Quick one-shot: summarize, extract, validate | Engine (haiku-task) |
| Code review, analysis, debugging | Engine (sonnet-task) |
| Web search, large context | Engine (gemini-task) |
| Code generation, scripting | Engine (codex-task) |
| Deep reasoning, architecture | Engine (sonnet-task -> auto-escalates to opus) |
| Complex multi-step with state | Agent (oc agent -> specialist) |

## Fleet Discovery (DYNAMIC)
Do NOT rely on hardcoded agent lists. Discover dynamically:
- Agents: `bash ~/.openclaw/scripts/skill-router.sh route "<keywords>"`
- Skills: skill-router indexes all agent skills automatically
- Engine list: `engine-router --list`

## Handoff Format
When dispatching to a specialist, use this format:
```
TASK: <one-line summary>
CONTEXT: <relevant chartroom/memory findings>
INTENT: <required intents, e.g. Accurate [I03], Secure [I15]>
URGENCY: low | normal | high
SOURCE: relay (Robert) | cron | internal
```

## Fleet Tools for Intent Verification
When routing, check if the target agent can fulfill the intent:
- All agents have: memory_recall, memory_store (add-only, UUID-based)
- Chart operations: use chart-handler.sh (update/delete by ID) — NOT memory_store
- Host access: only via reactor (Dev + bridge.sh)
- Discord UI: only Relay renders components
- Web browsing: only Navigator (needs Chromium runtime)
- Google Workspace: only Comms Officer (gog CLI, host-only)

## Routing Fallbacks
If skill-router returns no match:
- Infrastructure/system -> Ops Officer (spec-ops)
- Projects/docs -> Scribe (spec-projects)
- Code/bugs -> Dev (spec-dev)
- Security -> Security (spec-security)
- Communication/messaging -> Comms (spec-comms)
- Research/information -> Research (spec-research)
- Analysis/strategy -> Strategist (spec-strategy)
- Unclear/ambiguous -> Relay (escalate back, ask Robert)

## Recent Routing Patterns
(Updated by Captain as routing decisions are made)
