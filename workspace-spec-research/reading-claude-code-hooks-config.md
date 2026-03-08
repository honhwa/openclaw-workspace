ID: reading-claude-code-hooks-config
Audience: all agents
Verified from official docs: 2026-03-07

Hooks config:
- settings JSON key: `hooks`
- scopes: managed/user/project/local

Hook events (documented):
SessionStart
InstructionsLoaded
UserPromptSubmit
PreToolUse
PermissionRequest
PostToolUse
PostToolUseFailure
Notification
SubagentStart
SubagentStop
Stop
TeammateIdle
TaskCompleted
ConfigChange
WorktreeCreate
WorktreeRemove
PreCompact
SessionEnd

Hook governance keys:
disableAllHooks
allowManagedHooksOnly
allowedHttpHookUrls
httpHookAllowedEnvVars

Keybindings:
File: ~/.claude/keybindings.json
Supports context-specific mappings and null unbind.

CLAUDE.md hierarchy:
Managed: /etc/claude-code/CLAUDE.md
Project: ./CLAUDE.md or ./.claude/CLAUDE.md
User: ~/.claude/CLAUDE.md
Local personal: ./CLAUDE.local.md

Rules:
.claude/rules/**/*.md
Optional `paths:` frontmatter for conditional loading.
`@path` imports supported.

Settings files:
~/.claude/settings.json
./.claude/settings.json
./.claude/settings.local.json
managed-settings.json (managed deployments)

Sources:
- https://code.claude.com/docs/en/hooks
- https://code.claude.com/docs/en/keybindings
- https://code.claude.com/docs/en/memory
- https://code.claude.com/docs/en/settings
