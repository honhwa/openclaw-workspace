ID: reading-claude-code-commands
Audience: operators
Verified from official docs: 2026-03-07

Slash commands (documented):
/add-dir, /agents, /chrome, /clear, /compact, /config, /context, /copy, /cost, /diff, /doctor, /exit, /export, /fast, /feedback, /fork, /help, /hooks, /ide, /init, /insights, /keybindings, /login, /logout, /mcp, /memory, /model, /output-style, /passes, /permissions, /plan, /plugin, /pr-comments, /privacy-settings, /reload-plugins, /remote-control, /remote-env, /rename, /resume, /review, /rewind, /sandbox, /security-review, /skills, /stats, /status, /statusline, /tasks, /terminal-setup, /theme, /upgrade, /usage, /vim.

Core CLI syntax:
claude
claude "<prompt>"
claude -p "<prompt>"
cat file | claude -p "<prompt>"
claude -c
claude -r "<session-id-or-name>" "<prompt>"
claude update
claude auth login|status|logout
claude agents
claude mcp
claude remote-control

Key flags (documented):
--model, --fallback-model, --permission-mode, --allowedTools, --disallowedTools, --max-turns, --output-format, --input-format, --json-schema, --session-id, --continue/-c, --resume/-r, --worktree/-w, --agent, --agents, --mcp-config, --strict-mcp-config, --add-dir, --settings, --setting-sources, --append-system-prompt, --append-system-prompt-file, --system-prompt, --system-prompt-file, --fork-session, --from-pr, --tools, --verbose, --print/-p.

UNVERIFIED:
- Local host `claude --help` output (web-only constraint).

Sources:
- https://code.claude.com/docs/en/cli-reference
- https://code.claude.com/docs/en/interactive-mode
