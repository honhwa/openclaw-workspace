# MEMORY.md — Research Agent

## Source Trust Scores
(Populate as you evaluate sources. Format: source | trust 1-10 | reason | last updated)

### High Trust (8-10)
- Official Anthropic blog | 10 | Primary source, always accurate | initial
- Official OpenAI blog | 10 | Primary source | initial
- Google AI blog | 10 | Primary source | initial
- arXiv papers | 9 | Peer review, but check for retractions | initial

### Medium Trust (5-7)
- The Verge | 7 | Generally accurate, verify technical claims | initial
- TechCrunch | 7 | Good coverage, occasional hype | initial
- Ars Technica | 8 | Strong technical journalism | initial

### Tracking (needs more data)
(Add sources here as you encounter them, move up or down after 5+ evaluations)

## Gemini Token Spend Log
(Track daily spend to optimize budget)

## Model Providers — Active Watch List
Search Chartroom for `model-profile-*` for full profiles. Your job: keep these profiles current.

| Provider | Models We Use | Access | Cost Model | Watch Priority |
|----------|--------------|--------|------------|----------------|
| Anthropic | Opus 4.6 (Reactor), Sonnet 4.6 (reserved), Haiku 4.5 | Max plan + API | Flat rate (Max) / per-token (API) | HIGH — Opus IS our Reactor |
| OpenAI | GPT-5.3 Codex | ChatGPT Plus OAuth | Flat rate ($20/mo) | HIGH — all 8 agents depend on this |
| Google | Gemini 3 Flash, Gemini 3.1 Pro | API key | Per-token (see profile) | HIGH — your primary tool |
| OpenRouter | Auto (catch-all) | API key | Per-token (varies by model) | MEDIUM — White Room design pending |
| Meta | Llama (via OpenRouter) | OpenRouter | Per-token | LOW — future potential |

**When you detect a change**: Update the `model-profile-*` Chartroom entry immediately. If it affects cost or capability, flag for Captain to evaluate routing changes. If a model is deprecated, URGENT flag — we got burned by gemini-3-pro.

## Research Patterns Learned
(Populate as you discover what works and what doesn't)
