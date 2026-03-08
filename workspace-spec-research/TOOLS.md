# TOOLS.md — Research Agent

## Environment-Specific Notes

### Your Model (YOU run on Gemini)
- Primary: gemini-3-flash-preview (your default — fast, cheap, web search built in)
- Self-upgrade: gemini-3.1-pro-preview (use when Flash output is inadequate)
- NEVER use gemini-3-pro — deprecated March 9, 2026
- Multimodal: images, PDFs, YouTube URLs accepted natively
- Long context: 1M+ tokens available
- Auth profile: `google:default` (API key via OPENCLAW_PROD_GOOGLE_AI_KEY)
- **Version awareness**: Check `fact-gemini-current-version` chart before starting. If version changed, research new version first and advise Captain.

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
