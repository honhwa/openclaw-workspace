# Decision Registry — Historian

## Unified Decision Log
Connecting system decisions to their original rationale and eventual outcomes.

### [D-20260320-01] Identity-First Architecture
- **Date:** 2026-03-20
- **Decision:** Establish `ARCHITECTURE.md` and `RELATIONSHIP-PROTOCOL.md` as the master system authority.
- **Why:** To prevent NVIDIA NemoClaw blueprints from overwriting Robert's month of custom relationship and policy work.
- **Outcome:** ✅ Success. The fleet now recognizes identity constraints as non-negotiable system requirements.

### [D-20260320-02] DGX Spark Migration Path
- **Date:** 2026-03-20
- **Decision:** Move OpenClaw from Hostinger VPS to local DGX Spark.
- **Why:** Survival of Georgia power outages and high-performance inference control.
- **Outcome:** 🏗️ In Progress. Migration roadmap finalized; execution pending hardware readiness.

### [D-20260315-01] Scribe Model Selection
- **Date:** 2026-03-15
- **Decision:** Set `gemini-3-flash-preview` as primary for Scribe.
- **Why:** Best tool calling performance and near-Pro reasoning at 50% cost of Pro.
- **Outcome:** ✅ Success. Tool calling reliability issues resolved once gateway config was aligned.

### [D-20260305-01] Memory-Cop BALANCED Baseline
- **Date:** 2026-03-04
- **Decision:** Lock success criteria to BALANCED thresholds (99.5% accuracy, <300ms failover).
- **Why:** Tradeoff between strictness and operational velocity during Phase 2.
- **Outcome:** ✅ Success. Enabled Phase 2 tasks to move forward with clear targets.

### [D-20260303-01] Model-Monitor Architecture
- **Date:** 2026-03-03
- **Decision:** Implement model monitor as a Gateway Cron spawning Gemini Flash subagents.
- **Why:** Efficient resource usage and cost-effective parsing.
- **Outcome:** ✅ Success. System now monitors provider status without persistent overhead.

### [D-20260302-01] CHAMELEON Emoji Protocol
- **Date:** 2026-03-02
- **Decision:** Formalize Emoji Protocol v1.0 for system-wide communication.
- **Why:** 33% token-efficiency savings and high visibility into agent status.
- **Outcome:** ✅ Success. Protocol validated and deployed across all channels.

### [D-20260308-01] NIM Fallback Ordering
- **Date:** 2026-03-08
- **Decision:** Reorder fallback chain to place free NVIDIA NIM at positions 4-7.
- **Why:** To optimize cost and utilize available free-tier resources after primary failures.
- **Outcome:** ✅ Success. Improved resiliency without increasing spend.

### [D-20260308-02] Satisfaction System Ownership
- **Date:** 2026-03-08
- **Decision:** Assign 16 crons to explicit agent owners.
- **Why:** To ensure accountability for system health monitoring and maintenance.
- **Outcome:** ✅ Success. Clearer sitrep reporting and automated resets.

### [D-20260307-01] No-Propaganda Metrics Policy
- **Date:** 2026-03-07
- **Decision:** Mandate evidence-based reporting over "all systems go" narrative.
- **Why:** To prevent "hallucinated health" and ensure real problems are surfaced.
- **Outcome:** ✅ Success. Agents now report specific error codes and log evidence.

### [D-20260307-02] ChatGPT Business Dual Pools
- **Date:** 2026-03-07
- **Decision:** Split agents across two ChatGPT Business seats to increase concurrency.
- **Why:** To bypass the 5-message/min limit on a single Plus account.
- **Outcome:** ✅ Success. Fleet concurrency significantly improved.

---
*Older decisions backfilled from project-specific logs.*
