# MEMORY.md — Research Agent

## Source Trust Scores
Format: source | trust 1-10 | reason | last updated

### High Trust (8-10)
- Official Anthropic blog | 10 | Primary source, always accurate | initial
- Official OpenAI blog | 10 | Primary source | initial
- Google AI blog | 10 | Primary source | initial
- arXiv papers | 9 | Peer review, check for retractions | initial
- Ars Technica | 8 | Strong technical journalism | initial

### Medium Trust (5-7)
- The Verge | 7 | Generally accurate, verify technical claims | initial
- TechCrunch | 7 | Good coverage, occasional hype | initial

### Tracking (needs 5+ evaluations to rank)
(Add sources here as encountered)

## Model Providers Watch List
| Provider | Models We Use | Cost Model | Watch Priority |
|----------|--------------|------------|----------------|
| Anthropic | Opus 4.6 (Reactor), Sonnet 4.6, Haiku 4.5 | Flat (Max) / per-token (API) | HIGH |
| OpenAI | GPT-5.3 Codex | Flat ($20/mo ChatGPT Plus) | HIGH |
| Google | Gemini 3 Flash, Gemini 3.1 Pro | Per-token | HIGH |
| OpenRouter | Auto (catch-all) | Per-token (varies) | MEDIUM |
| Meta | Llama (via OpenRouter) | Per-token | LOW |

When you detect a change: update model-profile-* in Chartroom. If it affects cost or capability, flag for Captain.

## Gemini Token Spend Log
(Track daily. Format: date | task | model | est cost | actual cost)

## Research Patterns Learned
(Populate as you discover what works)
