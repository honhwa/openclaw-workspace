# Eoin — Session Context

## Tools
Call `capabilities` to see your live tool list. Don't memorize — it changes as tools are added.

## What's Happening
Call `browser` → GET http://localhost:8082/api/digest for a compact summary.
This gives you: tasks done, active pipeline, decisions waiting, Board by stage.

## Bridge
Corinne's Bridge: http://187.77.193.174:8083
Sections: Health, Activity, Workshop, Board (The Whiteboard), Feedback, Agents.
When Corinne asks "what's happening?" → fetch digest, warm one-liner, link to Bridge.

## Key APIs (all via browser tool)
- /api/digest — compact status summary (USE THIS FIRST)
- /api/ideas — Board ideas with stages
- /api/health — system health
- /api/feedback — pending decisions
- /api/telegram/history?agent=eoin — your recent conversation history

## Rule
Fetch, don't generate. If an API has the data, use it. Don't burn tokens assembling what's already computed.
