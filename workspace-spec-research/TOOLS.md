# TOOLS.md — Research Agent

## Skills (6)
`ai-news`, `gemini-check`, `research-estimate`, `research`, `transcript-query`, `youtube-ingest`


## Environment-Specific Notes

### Your Model (YOU run on Gemini)
- Primary: gemini-3-flash-preview (your default — fast, cheap, web search built in)
- Self-upgrade: gemini-3.1-pro-preview (use when Flash output is inadequate)
- NEVER use gemini-3-pro — deprecated March 9, 2026
- Multimodal: images, PDFs, YouTube URLs accepted natively
- Long context: 1M+ tokens available
- Auth profile: `google:default` (API key via OPENCLAW_PROD_GOOGLE_AI_KEY)
- **Version awareness**: Check `fact-gemini-current-version` chart before starting. If version changed, research new version first and advise Captain.

### Fallback: Gemini CLI (host)
When your gateway Gemini model is rate-limited or unavailable, use the `engine_dispatch` MCP tool:
- `engine_dispatch` with engine: `gemini` and your prompt
- This calls `gemini-task` on the host — same Gemini Flash, different auth path
- **This is your FIRST fallback** — try this before any non-Gemini model
- No extra cost, same capabilities, bypasses gateway rate limits

### Web Search
- Gemini grounded search provides citations
- For full browser interaction, delegate to Navigator (spec-browser)
- Perplexity/Sonar search also available via tools.web.search config

### Storage
- Research results: Chartroom (structured findings) or workspace memory/
- AI news digests: Chartroom reading entries (daily, overwritten)
- Source trust scores: MEMORY.md in this workspace
- Video transcripts: `/root/.openclaw/transcripts.db` (SQLite)

### YouTube Transcript Database
- DB path: `/root/.openclaw/transcripts.db`
- Ingest: `python3 ~/.openclaw/scripts/youtube-ingest.py <urls or --channel @handle>`
- Query: `python3 ~/.openclaw/scripts/transcript-query.py <keywords>`
- Schema: videos(video_id, title, publish_date, url, description, transcript, summary, key_insights, source, trust_level, trusted_by, channel)
- Current: 29 videos from @NateBJones (Feb 7 - Mar 7, 2026), 872K chars
- All entries: source=external, trust_level=0.9, trusted_by=Robert
- Uses Gemini API for ingestion (native YouTube video processing)

### Ops Channels
- #ops-dashboard — Infrastructure monitoring
- #ops-alerts — Critical alerts

### Chartroom Write Access
- `chart_add` MCP tool — chart discoveries inline during research
  - Usage: chart_add(id, text, category, importance)
  - Categories: reading, issue, decision, agent, procedure, fact
  - Use for: research findings, model evaluations, source assessments
  - ID convention: research-<topic> or reading-<topic>
- `chart_search` MCP tool — search existing charts before adding
- `chart_read` MCP tool — read a specific chart by ID
