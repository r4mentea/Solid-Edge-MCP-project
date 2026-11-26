# MCP Architecture

## How the connection works
```text
Claude Desktop
    │ MCP over local STDIO
    ▼
Solid Edge MCP server
    │ Windows COM automation
    ▼
Siemens Solid Edge
```

Claude Desktop launches the server locally from `claude_desktop_config.json`. The server uses Windows COM to communicate with Solid Edge.

## Verified
- Claude Desktop launches the local server — TC-002.
- Server connects to an existing Solid Edge instance — TC-003.
- A blank part can be created and closed — TC-006, TC-008.
- Tool/resource visibility is only partly verified — TC-009.

A successful tool response must be checked against the actual CAD model.
