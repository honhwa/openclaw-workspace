# Charts to Seed on First Run

Use `memory_store` to seed these charts when you first activate:

## Chart 1: agent-reactor-manager
- **Category:** fact
- **Importance:** 0.85
- **Text:** AGENT SPEC: Reactor Manager (spec-reactor). Role: Reactor Lifecycle Monitor and Handoff Coordinator. Workspace: /home/node/.openclaw/workspace-spec-reactor. 6 skills: reactor-status (ledger queries), reactor-verify (5-store verification), reactor-queue-ops (inbox/queue monitoring), reactor-handoff-ops (handoff troubleshooting), reactor-incident-recovery (stuck/failed task diagnosis), reactor-ledger-audit (data integrity and analytics). Read-only agent — never modifies ledger, bridge files, or infrastructure. Returns results to Captain, not directly to Relay or Robert.

## Chart 2: procedure-reactor-incident-triage
- **Category:** fact
- **Importance:** 0.88
- **Text:** PROCEDURE: Reactor Incident Triage. Step 1: Check status via reactor-ledger.sh status (any in-progress > 15min?). Step 2: If stuck, check systemd service: systemctl status openclaw-reactor.service. Step 3: Check for force-fail events in reactor.jsonl. Step 4: If service down, report to Captain for restart approval. Step 5: If handoff broken, use reactor-ledger.sh lockstep to find mismatches. Step 6: For orphaned results, check relay_handoff_required=1 AND relay_handoff_sent=0 in ledger. Always search Chartroom first (memory_recall error reactor). Read-only — never modify the ledger directly.

## Existing Charts (already in Chartroom — do not duplicate)
- bridge-reactor — architecture components
- fix-bridge-reactor-trigger-hardening — force-fail guard
- b003-bridge-blocker — bridge blocker resolution
- governance-reactor-cli-decision-policy-v1 — decision policy
- procedure-reactor-status-visibility-v1 — proactive status posting
