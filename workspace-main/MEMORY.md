# MEMORY.md - Long-Term Memory

## Coaching — 2026-04-09 (Task #2758)
Agent main scored 0.47/1.0 on tool usage during execution of task #2756. Violations: dynamic_unverified, no_self_assessment, no_chart_update, no_verification_evidence. Per TOOLS.md: “Truth even when uncomfortable. Hard things to the system, easy things to the human.” Actions required: use tools and read files first (verify via tools), self-assess confidence per step, log outcomes to Chartroom (chart_add), and provide verification evidence for every tool call. Do not mark tasks complete without verification.

--------------

## Architectural Decisions ### 2026-03-01: Human-Agent Interface & Token Efficiency - **Definition**: 1 Human + Relay (PA). - **Efficiency**: Separating human-readable chat from stored agent-only logs. - **Principle**: Information should only be processed or sent if it specifically benefits the recipient (human or agent) to minimize token waste.