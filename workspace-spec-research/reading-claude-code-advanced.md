ID: reading-claude-code-advanced
Audience: Reactor, Dev
Verified from official docs/changelog: 2026-03-07

Worktrees:
claude --worktree <name>
claude -w <name>
Creates isolated git worktree session.

Plan mode:
claude --permission-mode plan
claude --permission-mode plan -p "<prompt>"
Use /plan in-session.

Tasks + scheduling:
/tasks
/loop [interval] <prompt-or-slash-command>
(Changelog 2.1.71: /loop and recurring cron scheduling tools)

Subagents / agent tool:
/agents
claude agents
claude --agent <name>
claude --agents '<json>'

Agent teams:
teammateMode: auto|in-process|tmux
(Experimental gating documented in env/settings/changelog)

Memory:
/memory
autoMemoryEnabled setting
Env kill-switch: CLAUDE_CODE_DISABLE_AUTO_MEMORY=1
Storage: ~/.claude/projects/<project>/memory/

Remote workflows:
claude --remote "<task>"
claude --teleport
claude remote-control

UNVERIFIED:
- Any feature advertised only by community guides without official doc parity.

Sources:
- https://code.claude.com/docs/en/common-workflows
- https://code.claude.com/docs/en/memory
- https://code.claude.com/docs/en/settings
- https://code.claude.com/docs/en/changelog
