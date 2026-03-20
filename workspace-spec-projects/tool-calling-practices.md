# Tool Calling Best Practices — Big 3 Models

## The #1 Fix: Forced Tool Calling Mode

All three providers support forcing tool calls at the API level:
- **Gemini**: `function_calling_config: { mode: "ANY" }`
- **Claude**: `tool_choice: { type: "any" }`
- **OpenAI**: `tool_choice: "required"`

This is more reliable than any prompt engineering. When you NEED a tool call, force it.

## Universal Prompt Pattern (works across all three)

> You have access to the following tools. When [condition], you MUST use the appropriate tool. Do not describe what you would do — call the tool directly.

### What works across all models:
1. **Describe WHEN to use each tool, not HOW** — the tool schema handles HOW
2. **3-4 sentence tool descriptions minimum** — name, purpose, when to use, what it returns
3. **Negative constraints help**: "Do NOT output tool parameters as text"
4. **Clear conditional language**: "Use X when Y. Do NOT use X for Z."

### What doesn't work:
- Showing JSON examples of tool parameters in the system prompt — models copy-paste them as text
- Vague tool descriptions — models default to text responses
- Competing formats — if system prompt and tool schema disagree, model gets confused

## Model-Specific Notes

### Gemini Flash / Flash Lite
- **Least reliable in AUTO mode** — needs forced calling or very explicit descriptions
- Flash Lite less reliable than full Flash at complex multi-tool scenarios
- Detailed descriptions in function declarations are critical
- Include examples in parameter descriptions
- Known issue: ANY mode can cause infinite tool call loops

### Claude (Sonnet/Haiku/Opus)
- **Most reliable at tool calling** — rarely outputs params as text
- Tool descriptions are the single most important factor
- `tool_choice: any` skips natural language explanation, goes straight to call
- Supports `input_examples` in tool definitions (unique to Claude)

### OpenAI GPT/Codex
- Enable `strict: true` for guaranteed schema adherence
- Can return BOTH `content` (text) AND `tool_calls` simultaneously — handle both
- `tool_choice: "required"` forces at least one tool call
- "Before you call a tool, explain why" pattern improves accuracy

## Application to Scribe Menu System

The `/scribe` menu bug (Flash Lite outputting button JSON as text) is caused by:
1. Gemini in AUTO mode choosing text over tool call
2. SOUL.md showing JSON examples that Flash Lite copy-pastes
3. Gateway system prompt describing tool params in text that looks like inline syntax

Fix priority:
1. Check if gateway supports per-agent tool_choice config (force ANY for menu callbacks)
2. Remove all JSON examples from workspace files — describe intent only
3. If no API-level fix, use the universal prompt pattern with strong negative constraints
