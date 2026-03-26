# Lessons Learned — Historian

## Unified Lessons Registry
Actionable principles derived from operational experience.

### [L-20260321-01] Identity over Blueprint
- **Lesson:** Prioritize established system identity and relationship protocols over automated infrastructure blueprints.
- **Evidence:** NemoClaw integration (Mar 20), `ARCHITECTURE.md` launch.
- **Applies To:** Architecture, Governance, Fleet Management.
- **Action:** Perform a "Relationship Impact Assessment" before deploying any new automated orchestration layer.

### [L-20260321-02] Direct API Proof
- **Lesson:** Verify model capabilities (like tool calling) via direct API calls to isolate Gateway or configuration bugs.
- **Evidence:** Gemini Flash tool-call passes (Mar 15).
- **Applies To:** Debugging, Model Selection, RCA.
- **Action:** If an agent fails a tool call, run a direct API test before attempting prompt engineering.

### [L-20260321-03] Foundation First
- **Lesson:** System-level issues (permissions, database schemas) block all higher-level agent autonomy; fix them immediately.
- **Evidence:** `root` permission lockdown and LanceDB `updatedAt` mismatch (Mar 21).
- **Applies To:** Maintenance, Self-Healing, Reliability.
- **Action:** Maintain a "Foundation Health" check in sitreps to flag host-level blockers early.

### [L-20260315-01] Read-First Priming
- **Lesson:** Force a real tool call (e.g., `read`) at the start of a session to "prime" cost-optimized models for structured output.
- **Evidence:** Flash Lite text-dump failures (Mar 15).
- **Applies To:** Prompt Engineering, Tool Calling.
- **Action:** Use the `read-first` pattern in `SOUL.md` for agents using Flash Lite in AUTO mode.

### [L-20260308-01] Failover Index Advancement
- **Lesson:** Failover logic must explicitly advance to the *next* provider in the chain to avoid retrying a known-dead endpoint.
- **Evidence:** Gateway failover bug (Mar 08).
- **Applies To:** Gateway logic, Resiliency.
- **Action:** Include "negative test cases" in gateway audits where the first 2-3 providers are simulated as failed.

### [L-20260305-01] Over-Track First, Trim Later
- **Lesson:** Deploy heavy instrumentation (SQL, logs, events) during new system builds, then reduce once stability is proven.
- **Evidence:** Reactor ledger build (Mar 05).
- **Applies To:** Development, Observability.
- **Action:** Default to "Verbose" logging for Phase 0-1 projects.
