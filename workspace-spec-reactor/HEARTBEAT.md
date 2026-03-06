# HEARTBEAT.md — Reactor Manager

# Heartbeat tasks:
# - Check for stuck jobs (in-progress > 15min with no events)
# - Check for orphaned handoffs (relay_handoff_required=1, relay_handoff_sent=0, age > 5min)
