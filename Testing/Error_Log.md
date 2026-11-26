# Error Log

Record every observed error from setup, connection, tool execution, or model operations. Preserve the exact original message wherever possible.

## Summary
Four entries recorded so far (ERR-001 to ERR-004). All are LOW severity. None involved a crash, data loss, or model change.

## Error categories
- Expected limitation / unsupported operation
- Functional failure
- Input validation
- Model dependency
- COM / Solid Edge API
- MCP connection
- Unexpected behaviour
- Environment / setup

## Severity guide
- **Low:** Minor inconvenience; straightforward workaround.
- **Medium:** Feature unavailable or significant manual recovery required.
- **High:** Major workflow blocked, or model/application needs recovery.
- **Critical:** Data loss, model corruption, or serious application/system impact. Stop testing and preserve evidence.

## Error register
| Error ID | Related test | Category | Severity | Short description | Status |
|---|---|---|---|---|---|
| ERR-001 | TC-004 | Model dependency | Low | diagnose_api returns an error when no document is open | Expected limitation |
| ERR-002 | TC-005 | Unexpected behaviour | Low | activate reports success but the window only flashes in the taskbar | Investigating |
| ERR-003 | TC-002 | Environment / setup | Low | Server not listed in Local MCP servers after first restart | Workaround found |
| ERR-004 | TC-011 | Expected limitation | Low | No tool can read the color of a component, body or face | Investigating |

## Error entry template
### ERR-XXX — Short error title
- **Date:**
- **Related test ID:**
- **Environment:** Solid Edge version, server version/commit, Python version, client
- **MCP tool:**
- **Exact error message:** Paste verbatim; do not paraphrase.
- **Steps to reproduce:**
- **Expected behaviour:**
- **Actual behaviour:**
- **Category:**
- **Severity:**
- **Model / document context:**
- **Evidence:** Screenshot, log, traceback, or other material
- **Workaround:**
- **Suggested improvement:**
- **Status:** Open / Investigating / Workaround found / Resolved / Expected limitation

## Recording guidance
An intentionally unsupported operation that is correctly refused is an expected limitation, not automatically a software defect. We should still assess whether the refusal message is clear and useful.

If the same underlying error appears in multiple tests, link each test to the relevant error record and note whether it appears to share a root cause.

---

## ERR-001: diagnose_api fails when no document is open
- **Date:** 2026-11-25
- **Related test ID:** TC-004 (retested in TC-007)
- **Environment:** Solid Edge 2026 build 226.00.08.04; server master commit 49384d0; Python 3.11.9; Claude Desktop 6.28899.6
- **MCP tool:** diagnose_api
- **Exact error message:** `{"error":"No active document"}`
- **Steps to reproduce:** Connect to Solid Edge with no document open, then call diagnose_api.
- **Expected behaviour:** Diagnostic information about the connection and active document.
- **Actual behaviour:** An error and no connection information.
- **Category:** Model dependency
- **Severity:** Low
- **Model / document context:** No document open.
- **Evidence:** Exact response above.
- **Workaround:** Create or open a document first. With Part1 open, diagnose_api returned data (TC-007).
- **Suggested improvement:** See SG-001.
- **Status:** Expected limitation

## ERR-002: activate reports success but the window does not come to the front
- **Date:** 2026-11-25
- **Related test ID:** TC-005
- **Environment:** Solid Edge 2026 build 226.00.08.04; server master commit 49384d0; Python 3.11.9; Claude Desktop 6.28899.6
- **MCP tool:** manage_connection with `action: activate`
- **Exact error message:** None. The response was `{"status":"activated"}`.
- **Steps to reproduce:** With Solid Edge behind the Claude Desktop window, call manage_connection with `action: activate`.
- **Expected behaviour:** The Solid Edge window comes to the front.
- **Actual behaviour:** Only a red flash appeared on the Solid Edge taskbar icon (observed twice).
- **Category:** Unexpected behaviour (possibly Windows focus rules, not confirmed)
- **Severity:** Low
- **Model / document context:** No document open.
- **Evidence:** User observation.
- **Workaround:** Click the Solid Edge taskbar icon.
- **Suggested improvement:** See SG-004.
- **Status:** Investigating

## ERR-003: Server not listed after the first restart of Claude Desktop
- **Date:** 2026-11-25
- **Related test ID:** TC-002
- **Environment:** Claude Desktop 6.28899.6; Windows 11
- **MCP tool:** None (client configuration).
- **Exact error message:** None shown. The Local MCP servers section stayed empty.
- **Steps to reproduce:** Add a `solidedge` entry to `claude_desktop_config.json`, restart Claude Desktop, open Settings > Desktop app > Extensions > Local MCP servers.
- **Expected behaviour:** `solidedge` is listed.
- **Actual behaviour:** The list was empty after the first restart. The saved file was confirmed to contain the entry.
- **Category:** Environment / setup
- **Severity:** Low
- **Model / document context:** Not applicable.
- **Evidence:** User report and the first lines of the saved config file.
- **Workaround:** After re-checking the file and doing a full quit and restart of Claude Desktop, the server was listed with status "running". The exact cause was not isolated.
- **Suggested improvement:** In setup documentation, tell users to fully quit Claude Desktop (tray icon, Quit) before restarting.
- **Status:** Workaround found

## ERR-004: No tool can read the color of a component, body or face
- **Date:** 2026-11-26
- **Related test ID:** TC-011
- **Environment:** As for ERR-001 to ERR-003.
- **MCP tool:** query_component
- **Exact error message:** None. The responses were: `style` returned `""` for index 1; `face_style` returned `null` for index 1 and for the tube before any changes; `bodies` returned `{"component_index":1,"body_count":0,"bodies":[]}`.
- **Steps to reproduce:** Open `seaabtb_yc.asm`, then call `query_component` on index 1 (`oilsprt.asm:1`) with `style`, `face_style` and `bodies`.
- **Expected behaviour:** Color information (RGB or style name) for the component so that it can be matched.
- **Actual behaviour:** No color data was returned by any of the query options tried.
- **Category:** Expected limitation (missing read capability; cause not determined)
- **Severity:** Low
- **Model / document context:** Training assembly `seaabtb_yc.asm`, component `oilsprt.asm:1` (9 child parts).
- **Evidence:** The tool responses above.
- **Workaround:** The tester reads the RGB value in Solid Edge and supplies it.
- **Suggested improvement:** See SG-005.
- **Status:** Investigating. The color is likely set on the child parts, and the server appears to expose setters only, with no getter. This is not confirmed.