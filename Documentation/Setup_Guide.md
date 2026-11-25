# Setup Guide

This document captures setup steps verified for the local test environment.

## Target environment
- Operating system: Windows 11
- CAD application: Siemens Solid Edge 2026
- MCP implementation under evaluation: [tylerwagler/SolidEdge-MCP](https://github.com/tylerwagler/SolidEdge-MCP)
- Python requirement declared upstream: Python 3.11+
- Dependency manager used upstream: uv
- Connection method: Local STDIO server launched by Claude Desktop from an entry in `claude_desktop_config.json`.

## Installed tools
| Item | Verified version / status | Notes |
|---|---|---|
| Python | 3.11.9 | Verified with `py -3.11 --version` and `uv run python --version` |
| Git for Windows | 2.56.1.windows.1 | Verified with `git --version` |
| Visual Studio Code | 1.147.0 | Installed; the `code` command was initially not recognised in the existing terminal, but `code --version` later worked |
| uv | 0.13.10 | Verified with `uv --version` |
| Solid Edge 2026 | Installed | Build 226.00.08.04 (Help > About); MCP connection verified in TC-003 |
| Upstream MCP project | Cloned locally | Located at `%USERPROFILE%\GitHub\SolidEdge-MCP` |
| Python virtual environment | Created | `.venv` inside the upstream repository |
| Project dependencies | Installed | `uv sync --all-extras` completed successfully, per user confirmation |
| Python environment check | Passed | `uv run python --version` returned Python 3.11.9 |
| Pytest discovery check | Passed | `uv run pytest --collect-only -q` completed successfully; tests were collected, not executed |
| Claude Desktop | 6.28899.6 | Installed; local MCP servers are managed under Settings > Desktop app > Extensions |
| solidedge server in Claude Desktop | Running | Configured in `claude_desktop_config.json`; verified in TC-002 |
| End-to-end connection | Connected | Solid Edge reached through the server in TC-003 |

## Verified procedure

### 1. Install Python
Install Python 3.11 (64-bit) for Windows and enable the option to add Python to PATH.
Verify:
```powershell
py -3.11 --version
```
Expected: Python 3.11.9 in this environment.

### 2. Install Git
Install Git for Windows using the official installer.
Verify:
```powershell
git --version
```

### 3. Install Visual Studio Code
Install VS Code for Windows. If the `code` command is not recognised immediately, open a new PowerShell window and try again. VS Code can also be launched from the Windows Start menu.
Verify:
```powershell
code --version
```

### 4. Install uv
Use the official uv installation instructions for Windows. In this environment, the PowerShell installer command was:
```powershell
powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
```
Open a new PowerShell window and verify:
```powershell
uv --version
```

### 5. Clone the upstream MCP repository
Create a workspace folder and clone the upstream repository:
```powershell
New-Item -ItemType Directory -Force "$HOME\GitHub"
cd "$HOME\GitHub"
git clone https://github.com/tylerwagler/SolidEdge-MCP.git
cd "$HOME\GitHub\SolidEdge-MCP"
```

### 6. Install project dependencies
From the upstream repository directory, run:
```powershell
uv sync --all-extras
```
This creates the project virtual environment (normally `.venv`) and installs the declared project and optional development dependencies. Use `dir -Force` to see the hidden `.venv` folder.

### 7. Verify the project environment
Run:
```powershell
uv run python --version
uv run pytest --collect-only -q
```
The first command should report the project's Python 3.11 interpreter. The second checks test discovery only; it does not execute the test suite.

### 8. Add the server to Claude Desktop
1. Open Claude Desktop, then Settings, then Desktop app, then Extensions. The **Local MCP servers** section has an **Edit config** button. (This install had no Developer tab.)
2. Find the full path to uv:
```powershell
where.exe uv
```
3. Fully quit Claude Desktop first (tray icon, Quit), then edit `claude_desktop_config.json` in the folder that Edit config opens. Add this block directly under the first `{`, replacing `<your-windows-username>` with your own and keeping the comma after the closing `}`:
```json
  "mcpServers": {
    "solidedge": {
      "command": "C:/Users/<your-windows-username>/.local/bin/uv.exe",
      "args": ["--directory", "C:/Users/<your-windows-username>/GitHub/SolidEdge-MCP", "run", "solidedge-mcp"],
      "env": { "SOLIDEDGE_MCP_LOG_LEVEL": "WARNING" }
    }
  },
```
4. Check that the file is valid JSON (adjust the folder name if yours differs):
```powershell
Get-Content "$env:LOCALAPPDATA\Claude-Data\claude_pzs8sxrjxfjjc\claude_desktop_config.json" -Raw | ConvertFrom-Json | Out-Null; "JSON OK"
```

### 9. Restart and verify
1. Fully quit Claude Desktop (tray icon, Quit) and start it again. Closing the window is not enough.
2. Under Settings, Desktop app, Extensions, Local MCP servers, `solidedge` should be listed with a running status. In this environment it was not listed after the first restart and appeared after a full quit and restart (see ERR-003).
3. Open Solid Edge, then run a safe first test in a new chat: connect without starting Solid Edge, change nothing, and report the version (TC-003).

Notes from this environment:
- The config folder opened by Edit config was under `%LOCALAPPDATA%\Claude-Data\`, not the `%APPDATA%\Claude` path shown in public MCP documentation.
- Running the server by hand (`uv --directory "%USERPROFILE%\GitHub\SolidEdge-MCP" run solidedge-mcp`) prints a FastMCP banner that mentions available updates and then waits silently. Do not update FastMCP; the upstream project pins its own version range. Press Ctrl+C to stop it.
- Claude Desktop asks for permission on each tool call. "Allow once" keeps each call under manual control.

## Setup principles
1. Keep the upstream server in a separate local folder.
2. Do not copy or modify upstream source files unless explicitly authorised.
3. Follow upstream installation instructions and pinned Python version.
4. Capture installation commands, versions, warnings, and exact errors.
5. Verify each prerequisite before proceeding.

## Open questions
- Why was the server not listed after the first restart? (ERR-003)
- Does Claude Desktop show all 119 tools, 53 resources and 4 prompts? (TC-009)
- Which operations that modify CAD models work reliably? (Testing phases 3 onward.)
