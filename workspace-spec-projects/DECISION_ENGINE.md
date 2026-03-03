# DECISION_ENGINE.md

This document describes the Decision Engine integration hooks for project workflows, including planning, piloting, and pinning decisions. It extends the core project docs with workflow specifics and governance signals.

## Plan
- Purpose: Define the steps to explore, validate, and commit a project-related decision.
- Roles: Captain ( requester ), Subagents ( implementers ), Scribe ( record-keeper ).
- Lifecycle states: planning -> ready (approve/modify/reject) -> executing (steps) -> gate (Robert's approval) -> complete.
- Output artifacts: decisions file per channel, tasks file per channel, archived plans as needed.

How to generate a plan:
- Read context from RESILIENCE.md and LIMITS.md for shadow context and seasoning guidance.
- Enumerate phases and steps with ownership and success criteria.
- Produce a Plan JSON/Markdown artifact stored under planning artifacts (see /home/.openclaw/plans). Note: This repository uses plan-manager tooling; avoid direct edits where possible.

## Pilot
- Definition: Pilot is the first concrete run of the plan in a controlled channel to validate feasibility and impact before full rollout.
- Steps:
  1. Create a dedicated project channel if not exists (project-<slug>).
  2. Initialize channel topic/scope (topic) with plan intent.
  3. Start a Clarification Loop to solicit feedback and confirm decisions.
  4. Track decisions and tasks via the DECISION storage model.
- Signals: use decisions/STATUS updates, task updates, and audit checks against chat history.

## Pinning
- Pin the current decision board in the project channel when the board is in a stable state and approved by the required authority level.
- Use the agent bus to post large summaries and pin via the /pin command when in scope.
- Do not pin in unrelated channels.

## Project workflow (high level)
- Channel naming: project-<slug> where slug is a durable identifier for the project (lowercase, hyphen-delimited).
- Session naming: session-<slug> for ephemeral runs within a project channel, keeping a link to the parent project.
- Decisions live in decisions/<channel-name>.md; Tasks live in tasks/<channel-name>.json; logs and audit trails accompany those artifacts.
- Archiving: when project is complete, archive the channel and the associated decision/taks artifacts.

## Clarification Loop
- Summary: Short description of the question with key options and implications.
- Response: Yes/No buttons for quick feedback, plus a 24h poll if needed for prolonged decisions.
- Flow:
  1. Post Clarification Loop message to channel.
  2. Await user input (Yes/No) or poll results.
  3. Resolve outcome and proceed or reopen clarifications as needed.

## Starter pinned log template (RACP markers)
- RACP: Record, Acknowledge, Commit, Preserve markers used by Scribe to anchor decisions.

Sample starter pinned log (insert into DECISION_ENGINE.md starter section or as a pinned template):

```
[RACP-START]
Project: <project-slug>
Channel: project-<slug>
Decision Series: 1
Date: 2026-03-02
Summary: Initial decision skeleton.
Status: UNDECIDED
Owner: <owner>

Clarification Loop:
- Summary: <one-line summary>
- Yes/No options: [Yes] [No]
- Poll window: 24h

Next steps:
- Approve/Modify/Reject decisions in the plan gate.

[RACP-END]
```

## Shadow Context and seasoning
- See RESILIENCE.md for Shadow Context integration guidelines.
- See LIMITS.md for seasoning guidance during decision framing.

