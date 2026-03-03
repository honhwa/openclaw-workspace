RESILIENCE.md

Phase 1 Guardrails:
- Guardrails and guard values for prompt, tool use, and memory usage are established.

Phase 2 Guardrails:
- Shadow-context hardening: ensure that assistant audio-visual prompts are treated as shadow-context and do not influence core reasoning or self-model; logs are scrubbed and separate.
- Seasoning safeguards: limit system prompts and memory of internal tools; implement a rolling hash chain for critical actions; scrub sensitive data from logs; apply differential privacy where feasible.
- Health metrics: define KPIs for resilience like audit pass rate, token savings, latency, error rates, and MTTR; track weekly.

Integration with RACP-based workflow:
- RACP flow triggers resilience checks before actions; audit outcomes feed resilience metrics; escalation path for failures; automated reports.

Audit plan:
- Feed RESILIENCE.md with ongoing audit outcomes and success ratios.
