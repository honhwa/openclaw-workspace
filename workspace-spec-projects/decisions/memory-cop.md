# Decisions: memory-cop

## 2026-03-04

### D1: Primary Storage Pattern [DONE]
- **Decision**: Memory-cop will use a single primary database as the intake point for knowledge operations.
- **Rationale**: A primary intake avoids split-brain behavior and enables consistent routing logic.

### D2: Traffic-Cop Responsibility [DONE]
- **Decision**: Memory-cop is responsible for routing each item to the correct destination database based on explicit routing rules, or storing it in the primary database when no handoff is required.
- **Rationale**: Centralized routing keeps policy enforcement in one place and prevents accidental cross-pollination.

### D3: Repository vs Knowledge Boundary [DONE]
- **Decision**: File/versioning repos remain separate from searchable knowledge databases; memory-cop must enforce this boundary.
- **Rationale**: Preserves decision integrity that repos are for versioned files while memory DBs are for retrieval/query workflows.

### D4: Success Criteria Baseline (Legacy Draft) [DECIDED-NOT-DONE]
- **Decision**: Original undecided draft is **superseded** by **D4: Success Criteria Baseline (BALANCED) [DONE]** and is retained for historical traceability only.
- **Rationale**: Thresholds are now finalized in the BALANCED decision, so this legacy draft is no longer an active blocker.

### D4: Success Criteria Baseline (BALANCED) [DONE]
- **Decision**: Baseline is locked to **BALANCED** with thresholds:
  - Routing accuracy: **>=99.5% in staging** and **>=99.9% in production**
  - Fallback behavior: **p95 failover <300ms** and **hard-fail rate <0.1%**
  - Observability gate: **100% decision logs**, **traces on routed requests**, and **contract tests >=95% pass**
- **Rationale**: Best tradeoff for the current phase, with stricter targets available in later phases as operational confidence increases.

### D5: QMD Enablement Gate (End of Phase 2, Staging Only) [DONE]
- **Decision**: QMD may be enabled in **staging only** after Phase 2 exit criteria pass. Production enablement is deferred to later phase gates.
- **Rollback Guard**: Disable QMD in staging if either condition is met:
  - Hard-fail rate exceeds **0.1%** sustained for **15 minutes**, or
  - Routing accuracy drops below the **D4 baseline**.
- **Rationale**: Enables early validation without production blast radius.

## 2026-03-05

### D6: Phase 2 Task Reset (Workflow Observation Pass 2) [DONE]
- **Decision**: Tasks #2, #3, #4 reset from "in-progress" to "todo". Task #1 remains "done" (routing skeleton was verified). Reset executed via task-manager.sh through docker exec from the Reactor (host-side Claude Code).
- **Rationale**: Tasks were marked in-progress without execution artifacts. Clean reset ensures accurate state before real implementation begins. This reset also serves as a boundary observation for the Discord-OpenClaw-Reactor chain (Pass 2/5).
- **Method**: `docker compose exec openclaw-gateway task-manager.sh update memory-cop <id> todo` x3
- **Timestamp**: 2026-03-05T04:48:22Z
