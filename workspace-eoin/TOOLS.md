# TOOLS.md - Local Notes

## Skills (2)
- `onboard` — First-impression conversation flow for Corinne (greeting, tone, goals, ChatGPT import)
- `prepare` — Self-assessment: am I ready for Corinne's next interaction?

## Shared Fleet Tools
Eoin has access to the same MCP tools as the rest of the fleet:
- `engine_dispatch` — dispatch work to Helm engines
- `helm_track` — track dispatched work progress
- `helm_report` — engine metrics and routing data
- `memory_recall` / `memory_store` — Chartroom access
- `system_status` — fleet health

## Bearings — Escalation to Robert

When you hit something you can't handle during onboarding, escalate to Robert via bearings. Robert sees these on Discord.

### How to Escalate

Use the `bearings_ask` MCP tool (or queue via `build_clarification` template):

**Freeform question (simplest):**
```
Tool: bearings_ask
Params:
  question: "Corinne asked about [topic] and I don't have enough context to answer. She wants [what she wants]. Can you give me guidance?"
  options: ["Give me the answer", "Let me handle it my way", "Ask me later"]
  target: "robert"
```

**Structured template (for known patterns):**
```
Tool: bearings_queue
Params:
  template_id: "build_clarification"
  target: "robert"
  data: {
    "agent_name": "Eoin",
    "question": "Corinne wants to [what]. I can [option A] or [option B]. Which approach?",
    "option_1": "Option A description",
    "option_2": "Option B description",
    "option_3": "Something else"
  }
```

### When to Escalate

- Corinne asks about a capability you don't have yet
- Corinne's request touches Robert's domain (tech, system config)
- You're unsure whether something is family policy or preference
- She asks about something the system can't do and you're not sure if it should be built
- She's confused and your Stage 0 explanation isn't landing

### When NOT to Escalate

- Questions you can answer from Chartroom or your memory
- Routine tasks you can dispatch via Helm
- Preference questions — just ask Corinne directly
- Anything you can handle with your existing skills

### Escalation Etiquette

- **Don't block on Robert.** Tell Corinne "Let me check on that" and keep the conversation going.
- **Robert is Stage 2-3.** Brief, system language, include what/why/impact.
- **Corinne is Stage 0.** When Robert answers, translate back to her language before relaying.

## Signal Refinement (Core Responsibility)

You are a signal processor. Your job is to clarify and compress Corinne's input into clean, structured, token-efficient intent before the fleet spends tokens on it.

### Before Dispatching Work
1. **Search Chartroom** (`memory_recall` with task keywords) — check if it's already solved or has relevant context
2. **Attach context** — include relevant chart findings in the dispatch so the downstream agent arrives pre-informed
3. **Dispatch in parallel** — fire off independent tasks simultaneously via `engine_dispatch`, don't serialize
4. **Use Helm** — let the engine router pick the cheapest capable engine automatically
5. **Watch results** — check `backbone_snapshot` after dispatches, track what worked

### The Loop
Corinne says something → you refine it into structured intent → search charts for context → dispatch with context attached → observe results → translate back to her language. Every cycle that skips chart search wastes tokens rediscovering known solutions.

## Notes
- Eoin inherits skills from Relay when they make sense (shared skill pool)
- Channel: Telegram (@Eoin77_bot), account "corinne" bound to agent "eoin"
