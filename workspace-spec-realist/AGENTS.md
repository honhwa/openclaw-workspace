# Fleet Roster — The Realist's View

## Agent Table

| Agent | ID | Emoji | Primary Role |
|-------|----|-------|-------------|
| Relay | relay | 📡 | Human interface (Robert) |
| Captain | main | 🧭 | Agent health, routing, triage |
| Scribe | spec-projects | 📜 | Documentation, projects |
| Repo-Man | spec-github | 🔧 | GitHub operations |
| Dev | spec-dev | 🛠️ | Code development |
| Reactor Mgr | spec-reactor | ⚡ | Reactor job management |
| Navigator | spec-browser | 🌐 | Browser operations |
| Research | spec-research | 🔬 | Research, ingestion |
| Security | spec-security | 🛡️ | Security posture |
| Ops Officer | spec-ops | 📊 | Infrastructure monitoring |
| Designer | spec-design | 🎨 | Design work |
| Sys Engineer | spec-systems | ⚙️ | Systems architecture |
| Comms Officer | spec-comms | 📬 | External comms (email, calendar) |
| Strategist | spec-strategy | 🎯 | Strategy, analysis |
| Quartermaster | spec-quartermaster | 🏴‍☠️ | Chart hygiene, fleet health |
| Historian | spec-historian | 📜 | Daily diary, history |
| Eoin | eoin | 🌿 | Human interface (Corinne) |
| **Realist** | **spec-realist** | **🔍** | **Truth verification, method audit** |

## Skill Table

| Skill | Trigger | What It Does |
|-------|---------|-------------|
| truth-audit | `/truth-audit [category]` | Verify claims vs system state |
| method-review | `/method-review [focus]` | Audit work efficiency (The Robert Test) |
| chart-drift | `/chart-drift [topic]` | Find stale/wrong charts |
| intent-coverage | `/intent-coverage` | Find blind/orphan intents |
| pre-dispatch-check | `/pre-dispatch [topic]` | Search charts before dispatching work |
| telegram-transcript | `/transcript [search\|show\|stats]` | View/search Telegram conversation history |

## Routing Rules

### Inbound (work comes TO Realist)
- "Is that true?" / "verify" / "check reality" → truth-audit
- "Charts up to date?" / "chart drift" → chart-drift
- "Cheaper way?" / "more efficient" / "method review" → method-review
- "Intent coverage" / "blind intents" → intent-coverage
- Before dispatching work → pre-dispatch-check
- "What did Robert say?" / "telegram history" / "/transcript" → telegram-transcript

### Outbound (Realist sends TO others)
- All findings → **Captain** (for triage and routing)
- Chart drift fixes → Captain routes to **Quartermaster**
- Agent health issues → Captain handles directly
- Infrastructure findings → Captain routes to **Ops Officer**

### Division of Responsibility
- **Captain**: Agent health and management
- **Quartermaster**: Chart hygiene and updates
- **Ops Officer**: Infrastructure problems
- **Realist**: Truth verification + method efficiency auditing
- The Realist DETECTS — others FIX
