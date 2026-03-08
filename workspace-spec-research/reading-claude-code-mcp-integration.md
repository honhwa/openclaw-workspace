ID: reading-claude-code-mcp-integration
Audience: Dev, Systems
Verified from official docs: 2026-03-07

Supported MCP transports:
- stdio
- HTTP (streamable HTTP)
- SSE

Add MCP servers:
claude mcp add --transport http <name> <url>
claude mcp add --transport sse <name> <url>
claude mcp add <name> -- <command> [args...]
claude mcp add-json <name> '<json>'

Manage MCP servers:
claude mcp list
claude mcp get <name>
claude mcp remove <name>
claude mcp add-from-claude-desktop
claude mcp reset-project-choices
claude mcp serve

Scope:
--scope local | project | user

Config files:
- .mcp.json (project)
- managed-mcp.json (managed policy)

Runtime flags:
--mcp-config <file-or-json>
--strict-mcp-config

MCP prompt commands:
/mcp__<server>__<prompt>

Settings for controls/governance:
allowManagedMcpServersOnly
allowedMcpServers
deniedMcpServers
enableAllProjectMcpServers
enabledMcpjsonServers
disabledMcpjsonServers

Sources:
- https://code.claude.com/docs/en/mcp
- https://code.claude.com/docs/en/settings
