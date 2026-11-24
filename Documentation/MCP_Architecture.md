# MCP Architecture

## Intended evaluation architecture
1. The user describes a CAD task in ChatGPT.
2. ChatGPT selects an available MCP tool and supplies its arguments.
3. The MCP connection transports the tool call to the local server.
4. The Solid Edge MCP server uses Windows COM automation to communicate with Solid Edge.
5. Solid Edge performs the requested operation.
6. The server returns a result or error to ChatGPT.
7. The resulting CAD state is inspected and recorded.

## Conceptual diagram
```text
User
  |
  v
ChatGPT client
  |
  v
MCP connection / transport
  |
  v
Solid Edge MCP server (local Windows machine)
  |
  v
Windows COM automation
  |
  v
Siemens Solid Edge
```

## Important distinction
The upstream server's documented transport and the connection method supported by the selected ChatGPT plan/client must both be considered. A local STDIO MCP server is not automatically reachable by ChatGPT over the internet. The actual connection design must be verified before setup is considered complete.

## Components to verify
- ChatGPT plan and MCP feature availability.
- ChatGPT client (desktop or web) and supported connection type.
- Any required remote endpoint, tunnel, or bridge.
- Local MCP server process and dependencies.
- Windows COM connection to Solid Edge.
- Solid Edge document state and operation prerequisites.

## Verification status
This is a conceptual architecture. The end-to-end connection has not yet been established or tested.
