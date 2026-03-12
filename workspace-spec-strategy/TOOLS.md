# TOOLS.md — Strategist

## Skills (9)
`educate`, `idea-propose`, `opportunity-scan`, `source-brief`, `strategy-brief`, `strategy-digest`, `transcript-analyze`, `validate-idea`, `video-analyze`


## Environment-Specific Notes

### Your Model (YOU run on Codex)
- Primary: openai-codex/gpt-5.3-codex (flat-rate via ChatGPT Plus)
- Fallback: google/gemini-3-flash-preview (per-token)
- Auth profile: `openai-codex:default`

### Python Toolkit: strategist-tools.py
Location: `/root/.openclaw/scripts/strategist-tools.py` (also `/usr/local/bin/strategist`)
Your primary toolbelt. All subcommands:

| Command | What it does |
|---------|-------------|
| `strategist idea-propose <file.json>` | Load scored ideas into pipeline |
| `strategist idea-list [--status X]` | List ideas by status |
| `strategist idea-top [N]` | Top N proposed ideas by score |
| `strategist idea-stats` | Pipeline statistics |
| `strategist transcript-search <keywords>` | Search transcripts, return excerpts |
| `strategist transcript-themes` | Theme coverage across library |
| `strategist transcript-stats` | DB status (video count, dates, backfill) |
| `strategist source-brief [--themes X]` | Generate source brief, optionally filtered |
| `strategist digest` | Weekly strategy digest |
| `strategist validate-idea <idea_id>` | Pre-check idea feasibility |

### Idea Pipeline: idea-manager.py
Location: `/root/.openclaw/scripts/idea-manager.py` (also `/usr/local/bin/idea`)
Robert's review interface. Statuses: proposed → approved/rejected/parked → in-progress → done.

| Command | What it does |
|---------|-------------|
| `idea top [N]` | Top N proposed by score |
| `idea list [--status X]` | List by status |
| `idea view <id>` | Full detail on one idea |
| `idea approve <id>` | Robert approves (only Robert does this) |
| `idea reject <id> <reason>` | Robert rejects with reason |
| `idea park <id>` | Defer for later |
| `idea stats` | Pipeline counts |

### Transcript Database
- DB path: `/root/.openclaw/transcripts.db`
- Tables: `videos` (transcripts + analysis), `ideas` (pipeline), `sources` (provenance registry)
- Videos schema: video_id, title, publish_date, url, description, transcript, summary, key_insights, description_insights, source, trust_level, trusted_by, channel, source_id (FK→sources)
- Ideas schema: idea_id, title, description, category, status, source, score_impact/effort/urgency, score_total (computed), owner_agent, proposed_by, approved_by, rejection_reason, source_id (FK→sources)
- Sources schema: source_id, name, type (youtube-channel/website/api/manual), url, trust_level (0-1), trusted_by, trust_reason, data_class (external-reference/internal/user-provided/generated), active, auto_ingest, ingest_schedule, created_at, updated_at
- Current: 29 videos from @NateBJones, 872K chars, all with summaries + key_insights + description_insights
- Query: `strategist transcript-search <keywords>` or `transcript-query.py`
- Themes: ai-market, ai-coding, model-evaluation, cost-optimization, career-impact, prompt-engineering, agent-architecture, security, openclaw, leadership, compute-economics, harness, strategy

### Data Classification (MANDATORY)
Every piece of data you use has a class. You MUST cite sources.
| Class | Meaning | Example |
|-------|---------|---------|
| `internal` | Charts, agent memory, system state — this is US | Chartroom entries, MEMORY.md |
| `external-reference` | Transcripts, web research, API data — this is OTHERS | Nate B Jones videos, web scraping |
| `user-provided` | Robert said it directly — highest trust | Chat messages, direct instructions |
| `generated` | You or another agent produced it — label it | Strategy briefs, analysis outputs |

Citation format: `[Source: Name, "Title", YYYY-MM-DD, data_class]`
Example: `[Source: Nate B Jones, "AI Agents Are Taking Over", 2026-02-15, external-reference]`

### Transcript Backfill Pipeline
- Backfill: `transcript-backfill.py` — routes through YOU (Strategist/Codex) to analyze transcripts
- Auto-ingest: `transcript-auto-ingest.sh` — daily cron 8am UTC, checks @NateBJones for new videos
- You ARE the analysis engine for the transcript pipeline

### Chartroom (MCP Tools)
Use these MCP tools directly — do NOT use chart-handler.sh CLI:
- `chart_search` — search by keywords
- `chart_read` — read a specific chart by ID
- `chart_add` — add a new chart
- Key categories: identity, vision, decision, reading, architecture, cost, governance, agent, procedure
- Key charts: `identity-*` (self-knowledge), `agent-strategy`, `procedure-idea-pipeline`

### Built-in OpenClaw Tools
- `agentToAgent` — delegate to any agent (Research, Navigator, Dev, etc.)
- `web.search` — web search via configured providers
- `sessions` — conversation memory

### Delegation Protocol
| Need | Delegate to | Via |
|------|------------|-----|
| Fresh web research | Research (spec-research) | agentToAgent |
| New transcript ingestion | Research (spec-research) | agentToAgent |
| Live browsing / scraping | Navigator (spec-browser) | agentToAgent |
| Code implementation | Dev (spec-dev) | agentToAgent |
| Project tracking | Scribe (spec-projects) | agentToAgent |
| System health data | Ops Officer (spec-ops) | agentToAgent |

### Storage
- Strategy outputs: Chartroom (category: strategy or reading)
- Ideas: `ideas` table in transcripts.db (linked to sources via source_id)
- Sources registry: `sources` table in transcripts.db
- Source briefs: `workspace/memory/source-brief.md`
- Working notes: `workspace/memory/`

### Ops Channels
- #ops-dashboard — Infrastructure monitoring
- #ops-alerts — Critical alerts

### Critical Constraint
Robert has a full-time day job. 9 hours work + 4 hours OpenClaw + weekends. No spare time.
ALL ideas must be UNMANNED — can this make money while Robert sleeps?
No consulting, no client calls, no manual delivery per transaction.
