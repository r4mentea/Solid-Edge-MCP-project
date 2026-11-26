# Setup — Verified Local Environment

## Environment
Windows 11 · Solid Edge 2026 (226.00.08.04) · Python 3.11.9 · Git 2.56.1 · VS Code 1.147.0 · uv 0.13.10 · Claude Desktop 6.28899.6.

Upstream server: [tylerwagler/SolidEdge-MCP](https://github.com/tylerwagler/SolidEdge-MCP)  
Local clone: `%USERPROFILE%\GitHub\SolidEdge-MCP`

## Install and verify
Run in PowerShell:

```powershell
py -3.11 --version
git --version
uv --version

New-Item -ItemType Directory -Force "$HOME\GitHub"
cd "$HOME\GitHub"
git clone https://github.com/tylerwagler/SolidEdge-MCP.git
cd "$HOME\GitHub\SolidEdge-MCP"

uv sync --all-extras
uv run python --version
uv run pytest --collect-only -q
```

The pytest command checks test discovery only; it does not run the tests.

## Claude Desktop configuration
1. Open Settings → Desktop app → Extensions → Local MCP servers → Edit config.
2. Find uv's full path with `where.exe uv`.
3. Add the server entry to `claude_desktop_config.json`, adjusting the Windows username and paths:

```json
"mcpServers": {
  "solidedge": {
    "command": "C:/Users/<username>/.local/bin/uv.exe",
    "args": ["--directory", "C:/Users/<username>/GitHub/SolidEdge-MCP", "run", "solidedge-mcp"],
    "env": { "SOLIDEDGE_MCP_LOG_LEVEL": "WARNING" }
  }
},
```

4. Fully quit Claude Desktop from the system tray, then restart it.
5. Confirm `solidedge` appears as running under Local MCP servers.

In this environment, Edit config opened a folder under `%LOCALAPPDATA%\Claude-Data\`. A full quit and restart was needed before the server appeared (ERR-003).

## Verified connection
With Solid Edge already running, `manage_connection` using `action: connect` and `start_if_needed: false` connected to build 226.00.08.04 (TC-003).

## Notes
- Use the upstream repository as a separate local clone. Do not modify it without approval.
- Each MCP call required manual “Allow once” approval in the tested Claude Desktop setup.
- Keep confidential CAD files and company data within approved systems.
