# Fleet Sync Bulletin — 2026-03-28

Source session update (Claude Code):
- fix-executor-crash-loop-2026-03-28
- fix-bridge-restart-buttons-2026-03-28
- fix-bridge-workshop-hardening-2026-03-28
- decision-model-routing-2026-03-27
- reading-google-labs-creative-stack

Mandatory workspace updates:
1) Ops Officer:
   - Add daemon-watchdog skill.
   - stability-monitor includes checks 8-13.
   - alert dedup window = 30 minutes.
2) All agents:
   - Routing baseline: 11 Codex / 7 Mistral.
3) Terminology migration:
   - Workshop stage label "Spark" renamed to "Intake" everywhere.
4) Bridge hardening assumptions:
   - atomic writes enabled.
   - validation allowlists enabled.
   - per-user telegram_chat_id support.

Notes:
- If workspace is root-owned and not writable, record BLOCKED with path + owner.
- Use bearings_ask for ambiguity, do not guess.
