# LIMITS.md — Fleet Operating Constraints (2026-03-21)

## Critical Blockers

### 1. Permissions Lockdown
- **What**: `/home/node/.openclaw` and all child files/directories are owned by `root`.
- **Impact**: Agents cannot write to their own workspaces (SOUL.md, memory, charts, transcripts).
- **Why**: Likely side-effect of manual config edits or container mounting.
- **FIX**: Run on the **host machine**: `sudo chown -R node:node /home/node/.openclaw`
- **Verified**: 2026-03-21. Diagnosed via `ls -ld /home/node/.openclaw` and failed `write` attempts.

### 2. Discord Handoff Failure
- **What**: Relay bot returns "No groups found" in `directory groups list`. Logs show `Unknown Channel` errors.
- **Impact**: Discord message delivery fails (handoffs to user `187662930794381312` silently dropped).
- **Why**: Missing Discord Developer Portal intents: **Server Members** and **Message Content**.
- **FIX**:
  1. Visit [https://discord.com/developers/applications/](https://discord.com/developers/applications/)
  2. Enable "Server Members Intent" + "Message Content Intent"
  3. Restart Gateway.
- **Verified**: 2026-03-21. Confirmed via `openclaw logs` and Discord Guild audit.

### 3. Memory Pressure
- **What**: System scope at **91%** (critical threshold).
- **Impact**: Slowness, hangs, degraded chart search performance.
- **Why**: 24 stale charts pending hygiene; workspace bootstrap files near size limits.
- **FIX**:
  1. Resolve permissions lockdown (blocking Quartermaster run).
  2. Clear 24 stale charts via `chart stale` and targeted deletions.
  3. Review `AGENTS.md` for bootstrap file optimization.

### 4. Helm Routing Timeout
- **What**: Connection timeout to `172.20.0.1:18791` (Helm proxy).
- **Impact**: `sessions_send` tool times out; inter-agent messaging degraded.
- **Why**: Local gateway unreachable from container; possible proxy crash or network misconfiguration.
- **FIX**:
  1. Check Helm process: `ps aux | grep "openclaw-helm\|mcp"`
  2. Verify port binding: `ss -tuln | grep 18791` on host.
  3. Restart gateway (`openclaw gateway restart`).

## Degraded Operations

### DGX Spark Migration Frozen
- **Status**: Planning paused.
- **Reason**: Requires chart updates, model registry changes, and permissions to update `ARCHITECTURE.md`.
- **Readiness**: 30% (outline only).

### Key False Positive
- **Alert**: Ops Officer nightly reported **"0/9 keys"**.
- **Actual**: All 9 keys (`NVIDIA_NIM_API_KEY`, `TELEGRAM_BOT_TOKEN`, etc.) are active in gateway process environment.
- **Root Cause**: Incorrect `.env` file path check ("/root/.openclaw/.env" not found).

---

**Action Required**: Reactor (Robert) must resolve permissions and Discord intents **first**. Fleet operations will resume **only after** `chown` remediation.

INTENT: Observable [I13].
PURPOSE: System Health [P04].
VERIFIED: 2026-03-21.