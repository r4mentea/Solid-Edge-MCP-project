# MCP Architecture

## Evaluation architecture
1. The user describes a CAD task in Claude Desktop.
2. Claude selects an available MCP tool and supplies its arguments.
3. Claude Desktop starts the Solid Edge MCP server as a local process and sends the tool call to it over STDIO.
4. The server uses Windows COM automation to communicate with Solid Edge.
5. Solid Edge performs the requested operation.
6. The server returns a result or error to Claude.
7. The resulting CAD state is inspected and recorded.

## Conceptual diagram
```text
User
  |
  v
Claude Desktop (MCP client)
  |
  v
STDIO (local pipes on the same PC)
  |
  v
Solid Edge MCP server (launched by Claude Desktop)
  |
  v
Windows COM automation
  |
  v
Siemens Solid Edge
```

## Important distinction
The server runs on the same Windows machine as Claude Desktop and Solid Edge. Claude Desktop launches it from an entry in `claude_desktop_config.json`, so no remote endpoint, tunnel or bridge is needed. The project originally planned to use ChatGPT, which was replaced by Claude Desktop because this design needs a client that can start a local STDIO server.

## Components and status
- Claude Desktop and its local MCP servers section: working (TC-002).
- Local MCP server process and dependencies: working; the server shows a running status.
- Windows COM connection to Solid Edge: working (TC-003).
- Tool, prompt and resource visibility in the client: partly verified (TC-009).
- Solid Edge document state and operation prerequisites: being tested from phase 3 onward.

## Verification status
The end-to-end path from Claude Desktop to Solid Edge has been established and exercised with a blank part (TC-006 to TC-008). CAD modelling operations have not yet been tested.
