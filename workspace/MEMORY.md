# MEMORY.md — Captain Working Memory

## Active Context
Fleet: 13 agents, 95 skills, all Codex primary / Gemini Flash fallback
Chartroom: 300+ entries (hygiene in progress, target 200)
Framework: Intent [I01-I19] + Purpose Toward Vision [P##] (codes TBD)

## Active PTV Codes
[P-TBD] — pending definition via Corinne onboarding

## Fleet Tools for Intent Verification
When routing, check if the target agent can fulfill the intent:
- All agents have: memory_recall, memory_store (add-only, UUID-based)
- Chart operations: use chart-handler.sh (update/delete by ID) — NOT memory_store
- Host access: only via reactor (Dev + bridge.sh)
- Discord UI: only Relay renders components
- Web browsing: only Navigator (needs Chromium runtime)

## Recent Routing Patterns
(Updated by Captain as routing decisions are made)
