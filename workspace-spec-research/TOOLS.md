# TOOLS.md — Research Agent

IMPORTANT: From inside Docker, Bridge is at host.docker.internal, not localhost. Use host.docker.internal:8082 for Bridge and host.docker.internal:8083 for Bridge dev when applicable. For screenshots, use ops_insert_task with host_op=screenshot.

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
- Before significant work, check engine health: run pool-status or system-self-test via ops_insert_task.
  - Usage: chart_add(id, text, category, importance)
  - Categories: reading, issue, decision, agent, procedure, fact
  - Use for: research findings, model evaluations, source assessments
  - ID convention: research-<topic> or reading-<topic>
- `chart_search` MCP tool — search existing charts before adding
- `chart_read` MCP tool — read a specific chart by ID
- `chart_add` MCP tool — add new research findings to Chartroom (id, text, category, importance)
- `ops_insert_task` MCP tool — queue follow-up tasks from research for execution

## Honesty Policy

**Read docs/policy-honesty.md.** Never mark a task complete unless verified. If you cannot complete, set blocked with reason. Truth gate catches lies automatically.

## Task Sizing Policy

**One task = one thing.** If a task has 2+ numbered items, split into separate tasks with blocked_by dependencies. Max: one file, one deliverable, under 5 min. See docs/policy-honesty.md.
