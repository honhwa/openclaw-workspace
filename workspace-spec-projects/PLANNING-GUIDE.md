# Planning Guide — GSD-Inspired Principles

## Goal-Backward Thinking

Before building any plan, answer: **"What must be true when this is done?"**

Not "what tasks should we do" but "what observable outcomes prove success."

Every plan needs 2-5 success criteria written as testable statements:
- "Robert can create a project from Discord and see it tracked"
- "Model failover happens within 30 seconds with zero dropped messages"
- "The dev agent can receive a task, write code, and return results"

## Task Structure

Every plan step should have:
- **What**: One-line action description
- **Verify**: How to confirm it worked - a command, check, or observable result
- **Done**: Measurable acceptance criteria

Bad: "Set up authentication"
Good: "Create API key rotation script that runs nightly and posts results to #ops-changelog. Verify: bash rotate-key.sh --dry-run exits 0. Done: Key rotation runs unattended for 3 days."

## Phase Sizing

- Each phase: 3-7 steps
- Each step: completable in one agent session
- If a step needs more than one agent session, split it
- If a phase has 1-2 steps, merge it with an adjacent phase

## Wave Execution - Multi-Agent

When a plan involves multiple agents working in parallel:

**Wave 1:** Independent tasks with no dependencies between them
**Wave 2:** Tasks that depend on Wave 1 results
**Wave 3:** Integration tasks that wire everything together

Rule: No two parallel tasks should modify the same file.

## Research Before Planning

Before building a plan:
1. Read existing code/config - what already exists?
2. Check for reusable components - do not rebuild what is there
3. Identify constraints - token limits, API rate limits, disk space
4. Note risks and unknowns

Log findings with plan-manager.sh add-research -- show your work.

## Verification at Completion

When a plan completes, verify goal-backward:
1. Re-read the original success criteria
2. Test each one -- do not assume completing tasks = achieving goals
3. Check for stubs, placeholders, and unwired components
4. If any criterion fails, create a follow-up task -- do not silently ship

## Discord Interaction for Plans

Use the right Discord feature for each planning moment:
- **Buttons**: Approve/reject/modify decisions - fast, binary
- **Polls**: Choose between approaches - when Robert should weigh in
- **Modals**: Collect structured input - project scope, requirements
- **Threads**: Deep discussion on a specific phase or decision
- **Select menus**: Pick from options - agent assignment, priority

## Anti-Patterns

- Planning without research: assumptions fail at execution
- Huge monolithic plans: context rot, lost progress
- "Set up X" without verify criteria: done means nothing
- Executing without approval gates: Robert loses control
- Completing without verification: shipped but broken
