# CHAMELEON — RETIRED (2026-03-03)

The CHAMELEON emoji protocol was retired. It was documentation-only with zero runtime implementation.

**What replaced it:** OpenClaw's built-in status-reactions system, which was already in the codebase but silently disabled by a config gate. Now enabled globally via `ackReactionScope: "all"` in openclaw.json.

**What you see on Discord messages:**
- Agent emoji (who picked it up) → 🤔 thinking → 👨‍💻/⚡/🔥 tools → ✅ done or ❌ error
- Stall detection: ⏳ at 10s, ⚠️ at 30s
- Reactions auto-clear after reply

**Do not reference CHAMELEON protocol, emoji domains, or RACP tags.** These were never implemented and no longer apply.
