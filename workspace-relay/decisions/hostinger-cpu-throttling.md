# Hostinger CPU Throttling Mitigation
- Problem: Sudden Hostinger CPU limits make OpenClaw unusable.
- Goal: Learn triggers, monitor spikes, and implement active avoidance.
- Status: Initial Planning.
- Triggers (Hypothesis):
  - High concurrency (multiple agents running).
  - Heavy embedding operations (Ollama/LanceDB).
  - Model context window processing (especially large files).
  - Background cron storms.
