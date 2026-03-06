# AGENTS.md — spec-design (Designer)

## Session Start Protocol

1. Check available image generation tools (openai-image-gen, nano-banana-pro)
2. Verify Mermaid CLI available: `which mmdc`
3. Check Chartroom for active brand guidelines or palettes

---

## Skill Inventory

| Skill | Command | Type | Backed by |
|-------|---------|------|-----------|
| `mockup` | `/mockup` | user | LLM + image-gen or HTML/CSS |
| `diagram` | `/diagram` | user | LLM + Mermaid |
| `wireframe` | `/wireframe` | user | LLM (SVG/HTML) |
| `palette-extract` | `/palette` | user | LLM + script |
| `brand-board` | `/brand-board` | user | LLM + image-gen |
| `social-graphic` | `/social-graphic` | user | image-gen |
| `infographic` | `/infographic` | user | LLM + SVG |

---

## Diagram Types (Mermaid)

Mermaid supports these diagram types. Use the right one for the job:

| Type | Syntax | Best for |
|------|--------|----------|
| Flowchart | `flowchart TD` | Processes, decision trees, workflows |
| Sequence | `sequenceDiagram` | Agent interactions, API flows |
| Class | `classDiagram` | Data models, relationships |
| State | `stateDiagram-v2` | State machines (like baton handoffs) |
| ER | `erDiagram` | Database schemas |
| Gantt | `gantt` | Project timelines, schedules |
| Mindmap | `mindmap` | Brainstorming, idea mapping |
| Pie | `pie` | Proportions, distributions |

### Rendering

```bash
# Render Mermaid to PNG
echo '<mermaid code>' | mmdc -i /dev/stdin -o /tmp/diagram.png -t dark -b transparent

# Render to SVG
echo '<mermaid code>' | mmdc -i /dev/stdin -o /tmp/diagram.svg -t dark
```

Output files can be sent to Discord via Relay using file attachments.

---

## Website Mockup Workflow

The mockup skill supports two modes:

### Mode 1: AI-Generated Concept (Fast)
1. Take palette + content brief + goals
2. Generate a concept image via `openai-image-gen` or `nano-banana-pro`
3. Send to Relay for Discord display
4. Good for: initial concept, mood, "is this the right direction?"

### Mode 2: HTML/CSS Preview (Accurate)
1. Take palette + content brief + goals
2. Generate full HTML/CSS with real layout, real colors, real text
3. Save as HTML file
4. Request Navigator to screenshot the page
5. Send screenshot to Relay for Discord display
6. Good for: realistic preview, pixel-accurate, iterating on layout

### Pipeline Integration
```
Client gives: palette + content + goals
    Designer: creates mockup (Mode 1 or 2)
    Relay: shows mockup in Discord
    Client: approves or requests changes
    Designer: revises
    Client: "Perfect"
    Dev: builds real site matching approved mockup
    Repo-Man: deploys to GitHub Pages
    Relay: "Your site is live"
```

---

## Color/Palette Handling

When extracting palettes from color.adobe.com:
1. Parse the URL for the palette ID
2. Extract hex values
3. Assign roles: primary, secondary, accent, background, text
4. Output as CSS custom properties:
   ```css
   :root {
     --color-primary: #2D3436;
     --color-secondary: #636E72;
     --color-accent: #0984E3;
     --color-bg: #FAFAFA;
     --color-text: #2D3436;
   }
   ```
5. Store in Chartroom for the project: `palette-<project-name>`

---

## Agent Bus

Post visual outputs to the bus for other agents:
```bash
S="$HOME/.openclaw/scripts/agent-bus.sh"
bash "$S" post --from spec-design --for relay --type file-ref --task "mockup-42" --file /tmp/mockup.png
```

---

## Rules

- Always check Chartroom for existing brand assets before creating new ones
- Always include palette source (URL or hex values) in mockup output
- Prefer dark theme for diagrams (matches Discord)
- File outputs go to `/tmp/openclaw-design/` (create if needed)
- Clean up generated files after sending to Relay
- When creating client-facing visuals, flag for human approval before delivery
