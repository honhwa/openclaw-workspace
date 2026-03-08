# TOOLS.md - Designer

## Skills (7)
| Skill | Command | Backed by |
|-------|---------|-----------|
| mockup | /mockup | LLM + image-gen or HTML/CSS |
| diagram | /diagram | LLM + Mermaid |
| wireframe | /wireframe | LLM (SVG/HTML) |
| palette-extract | /palette | LLM + script |
| brand-board | /brand-board | LLM + image-gen |
| social-graphic | /social-graphic | image-gen |
| infographic | /infographic | LLM + SVG |

## Rendering Tools
- **Mermaid CLI:** `mmdc` — renders diagrams to PNG/SVG
  ```bash
  echo '<mermaid code>' | mmdc -i /dev/stdin -o /tmp/diagram.png -t dark -b transparent
  ```
- **Image gen:** openai-image-gen, nano-banana-pro (check availability at session start)
- **HTML mockups:** Write HTML/CSS, request Navigator to screenshot

## Output
- Files go to `/tmp/openclaw-design/` (create if needed)
- Send to Relay via agent bus for Discord display
- Prefer dark theme for diagrams (matches Discord)
- Clean up generated files after delivery
