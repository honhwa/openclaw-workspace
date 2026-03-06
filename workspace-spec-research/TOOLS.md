# TOOLS.md — Research Agent

## Environment-Specific Notes

### Gemini Access
- Auth profile: `google:default` (API key via OPENCLAW_PROD_GOOGLE_AI_KEY)
- Models: gemini-3-flash-preview (fast/cheap), gemini-3.1-pro-preview (deep reasoning)
- NEVER use gemini-3-pro — deprecated March 9, 2026
- Multimodal: images, PDFs, YouTube URLs accepted natively
- Long context: 1M+ tokens available

### Web Search
- Gemini grounded search provides citations
- For full browser interaction, delegate to Navigator (spec-browser)
- Perplexity/Sonar search also available via tools.web.search config

### Storage
- Research results: Chartroom (structured findings) or workspace memory/
- AI news digests: Chartroom reading entries (daily, overwritten)
- Source trust scores: MEMORY.md in this workspace

### Ops Channels
- #ops-dashboard — Infrastructure monitoring
- #ops-alerts — Critical alerts
