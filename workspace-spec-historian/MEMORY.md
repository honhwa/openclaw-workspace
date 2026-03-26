# MEMORY.md — Historian

## Session Archive Index
- session-journal-2026-03-08 — Reactor Session 18 archived (Corinne onboarding implementation Phase 1 complete: 5 PTV codes charted, 18 SOUL.md Purpose updates, Eoin onboarding/prepare/import/vision-capture skills, Relay check-in failure, policy enforcement gaps 79%, MCP memory chain broken, chartroom SN ratio 58-62%, enforcement-is-code architecture)
- session-journal-2026-03-09 — Reactor Session 19 archived (Telegram transcript system deployed with query/listen tools, Kimi K2.5 proving ground (0.2/10 avg, no tool calls, removed from config), multi-user secure storage gap identified; pending: Robert config approval, gateway restart, Telegram re-verify)
- session-journal-2026-03-09-s2 — Reactor Session 2 (Phase 0 recovery) archived (Found root cause of zero payloads: Gateway sends stream=True despite config, Helm returned JSON. Fixed via SSE translation in Helm. Installed jq and qmd. Identified Google 20 req/day limits. Anthropic zero credits. WIP: Agent test, Docker rebuild)
- session-journal-2026-03-09-s4 — Reactor Session 4 archived (Robert's stress and cost reckoning led to agents being brought back online with gemini-2.5-flash. Telegram output quality confirmed 'far better'. Key issues are Telegram double-write, bootstrap context size, GHL pause, and Helm proxy persistence).
- session-journal-2026-03-10 — Reactor Session 15/16 archived (Cost lockdown complete: $65+/mo -> ~$25/mo via ChatGPT Plus flat-rate and Gemini Free tier. Helm proxy learns rate-limit recovery times. 15 agents moved to Codex/Free Gemini; Relay/Eoin on paid Flash. Satisfaction fleet avg raised 7.7->8.2 via skill tags and error resets. WIP: Monitor Codex limits/quality, reset Strategist context).
- session-journal-2026-03-11 — Reactor Session 20 archived (Flash-Lite deployed for Relay: thought_signature blocker found+fixed in Helm (~40 lines), SKILL.md description-packed format template, API test matrix 3/3 pass. Only Relay swapped; Eoin/Research stay on 2.5 Flash. Charts: issue-gemini31-thought-signature, reading-openclaw-study-priority. WIP: e2e Telegram test pending Robert's YouTube URL, remove debug logging after confirm)
- session-journal-2026-03-15 — Tool Call Reliability & Gateway Bottleneck (Proved Gemini Flash 31/31 reliability via direct API; identified Gateway as the primary failure point. Overhauled Scribe config: groupPolicy=open, requireMention=false. Discovered message:preprocessed hook opportunity for tool call priming. 13 charts created.)
- diary-2026-03-20 — System Identity and Architecture Milestone (Launched System-Architecture and System-Alignment-Audit projects. Finalized DGX-Spark-Migration plan with Power-Safe protocol. Codified "Trusted Friend" voice. Identified root-cause permission and Discord connectivity blockers. Reconciling 116 custom scripts with NemoClaw blueprint.)
- retro-weekly-2026-03-21 — Architecture Pivot & Foundation Rot (Weekly retrospective Mar 15-21. Success: Direct API tool-call proof, Identity over Blueprint policy, DGX migration roadmap. Failure: Foundation rot from root permissions and LanceDB schema mismatch. Fleet self-healing paralyzed until host fix.)

## Patterns Identified
(Updated as retrospectives surface recurring themes)

## Working Style Notes
(How Robert and the system collaborate — updated from session observations)
- session-journal-2026-03-09-s4 — Reactor Session 4 archived (Robert's stress and cost reckoning led to agents being brought back online with gemini-2.5-flash. Telegram output quality confirmed 'far better'. Key issues are Telegram double-write, bootstrap context size, GHL pause, and Helm proxy persistence).
