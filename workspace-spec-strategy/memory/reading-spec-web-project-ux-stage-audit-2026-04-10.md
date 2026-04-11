# Reading: spec-web-project-ux-stage-audit-2026-04-10

Web Project UX Spec (7-stage) capability audit.

## Stage 1 — Spark
- **Owner agent:** Relay (chat-facing), with `reactor-dispatch` classifier support
- **Tools needed:** `message` (Telegram buttons/deep link), intent detector, project-bootstrap API
- **Current gap:** Website-intent trigger + yes/no CTA flow is specified in UX but not as an explicit handoff contract that reliably creates `design_projects(status=intake)`.
- **Needed:** Better Relay skill doc + small MCP/API endpoint for project bootstrap

## Stage 2 — Intake
- **Owner agent:** spec-design (primary), Relay fallback
- **Tools needed:** Bridge state-machine UI, save-on-answer persistence, resume support, `bearings_queue` integration
- **Current gap:** No explicit typed mapping from Q1–Q6 into `design_projects`; validation/resume checkpoints are underspecified.
- **Needed:** MCP additions (`design_project.create/update/progress`) + host_op intake persistence handler

## Stage 3 — Design Choices (Style Picker)
- **Owner agent:** spec-design
- **Tools needed:** Palette/layout/font generation service, thumbnail generation, iterative regenerate endpoint with directional prompt memory
- **Current gap:** No tool contract for 3-option bundles with stable option IDs/versioning; “try again with direction” lacks persistence semantics.
- **Needed:** host_op `design-style-generate` + tighter skill docs for controlled regeneration

## Stage 3.5 — Gauntlet
- **Owner agent:** spec-strategy (debate framing), orchestrated via `reactor-dispatch`
- **Tools needed:** Multi-model orchestrator (Codex/Claude/Gemini), `web_search` grounding, accessibility checks, structured verdict synthesis
- **Current gap:** No canonical data model for argument threads, contested points, mediator evidence, and final verdict objects.
- **Needed:** MCP (`design_gauntlet.run/status/read`) + host_op `gauntlet-orchestrator`

## Stage 4 — First Preview
- **Owner agent:** spec-dev (build/deploy), with spec-design acceptance criteria
- **Tools needed:** Stitch generation, Cloudflare preview deployment, status streaming to Bridge Canvas
- **Current gap:** Build/deploy is described but not encoded as durable `task_sessions` workflow with retries/rollback artifacts.
- **Needed:** host_op `stitch-build` + `cf-preview-deploy` + failure recovery runbook

## Stage 5 — Iteration Loop
- **Owner agent:** spec-dev (implementation), with Relay/spec-design interpreting change intent
- **Tools needed:** Change parser, diff-aware regen, version history, `alignment_metrics` writer
- **Current gap:** No explicit revision model for “same URL, new version”; `alignment_metrics` event taxonomy not defined beyond prose.
- **Needed:** MCP (`design_iterate.apply`, `design_versions.list/compare`) + iteration taxonomy docs

## Stage 6 — QA + Go Live
- **Owner agent:** spec-ops (QA gate), spec-dev for fix loops
- **Tools needed:** Lighthouse, screenshots (mobile+desktop), link checker, a11y scanner, production deploy gate
- **Current gap:** Missing one normalized QA pipeline/report object with clear block/pass thresholds.
- **Needed:** host_op `design-qa-runner` + MCP `design_qa.report`

## Stage 7 — Post-Launch
- **Owner agent:** spec-projects (Scribe) for lifecycle state; Relay for re-entry prompts
- **Tools needed:** Status updater, live URL health checks, reopen flow, optional analytics hooks
- **Current gap:** No automated post-launch health loop (uptime/link rot/content drift), no lightweight “quick edit” loop contract.
- **Needed:** Skill docs + cron templates; optional MCP `design_project.reopen`

## Cross-cutting recommendation
1. **No immediate new agent is mandatory.** Gaps are mostly orchestration contracts and typed interfaces.
2. **Highest-value additions:**
   - **host_op handlers:** `design-style-generate`, `gauntlet-orchestrator`, `stitch-build`, `cf-preview-deploy`, `design-qa-runner`
   - **MCP tools:** `design_project.*`, `design_gauntlet.*`, `design_iterate.*`, `design_versions.*`, `design_qa.*`
3. **Docs to add now:**
   - Relay Spark detection + handoff skill
   - spec-design Style Picker/regen skill
   - spec-dev iteration + retry/runbook
   - spec-ops QA gate + release runbook
4. **Data model hardening:** Add typed payload fields to `design_projects` (`intake_answers`, `style_choices`, `gauntlet_decisions`, `preview_versions`, `qa_reports`) to avoid JSON drift.

**Bottom line:** Biggest blocker is not staffing; it is missing tool contracts (host_op + MCP) and insufficiently explicit skill docs.
