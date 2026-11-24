# Setup Guide

This document will capture verified setup steps for the test environment.

## Target environment
- Operating system: Windows 11
- CAD application: Siemens Solid Edge 2026
- MCP implementation under evaluation: [tylerwagler/SolidEdge-MCP](https://github.com/tylerwagler/SolidEdge-MCP)
- Python requirement declared upstream: Python 3.11+
- Dependency manager used upstream: uv
- Connection method: To be confirmed for the selected ChatGPT plan and client.

## Setup status
| Item | Status | Notes |
|---|---|---|
| Python 3.11 | Pending verification | Upstream project pins Python 3.11 |
| Git for Windows | Pending | Used to obtain upstream project |
| uv | Pending | Manages Python environment and dependencies |
| Solid Edge 2026 | Installed | Confirm version during testing |
| Upstream MCP project | Not cloned yet | Keep separate from this repository |
| ChatGPT MCP access | Pending | Free-plan capabilities and connection route need confirmation |
| End-to-end connection | Not tested | Requires all prerequisites |

## Setup principles
1. Keep the upstream server in a separate local folder.
2. Do not copy or modify upstream source files unless explicitly authorised.
3. Follow upstream installation instructions and pinned Python version.
4. Capture installation commands, versions, warnings, and exact errors.
5. Verify each prerequisite before proceeding.

## Verified procedure
No end-to-end setup procedure has been verified yet. Add commands and screenshots only after they have been tested successfully.

## Open questions
- Which MCP connection route is available to this ChatGPT account?
- Does that route support tool calls that modify CAD models?
- Is a local-to-remote tunnel or another bridge required?
