# Test Results

Running record of executed scenarios. Keep Test IDs unique.

## Summary
| Metric | Count |
|---|---:|
| Tests executed | 9 |
| Passed | 6 |
| Failed | 1 |
| Blocked | 0 |
| Inconclusive | 2 |
| Errors logged | 3 |

## Test register
| Test ID | Date | Scenario | MCP tool(s) | Outcome | Error reference |
|---|---|---|---|---|---|
| TC-001 | 2026-11-25 | Local Python environment and test-discovery verification | None (local commands) | PASS | None |
| TC-002 | 2026-11-25 | MCP client connection (Claude Desktop to solidedge server) | manage_connection | PASS | ERR-003 |
| TC-003 | 2026-11-25 | Solid Edge COM connection without launching a new instance | manage_connection (connect) | PASS | None |
| TC-004 | 2026-11-25 | diagnose_api with no document open | diagnose_api | FAIL | ERR-001 |
| TC-005 | 2026-11-25 | Bring Solid Edge window to the front | manage_connection (activate) | INCONCLUSIVE | ERR-002 |
| TC-006 | 2026-11-25 | Create a blank part document | create_document | PASS | None |
| TC-007 | 2026-11-25 | diagnose_api with a blank part open (retest of TC-004) | diagnose_api | PASS | None |
| TC-008 | 2026-11-25 | Close the test part without saving | close_document | PASS | None |
| TC-009 | 2026-11-25 | Tools, prompts and resources visible in Claude Desktop | None (client menus) | INCONCLUSIVE | None |

## Test entry template
### TC-XXX — Short scenario title
- **Date:**
- **Test area:**
- **Preconditions / test model:**
- **User request:**
- **MCP tool(s):**
- **Expected result:**
- **Actual result:**
- **Outcome:** PASS / FAIL / BLOCKED / INCONCLUSIVE
- **Error reference:** None / ERR-XXX
- **Evidence:** Screenshot, log, or supporting file
- **Notes / suggestion:**

---

## TC-000 — Initial environment and MCP connection checkpoint
- **Date:** 2026-11-24
- **Test area:** Environment and connection
- **Expected result:** Required software is installed; ChatGPT can reach the MCP server; the server can communicate with Solid Edge.
- **Actual result:** Local development tools and dependencies are now installed and verified. ChatGPT-to-MCP and MCP-to-Solid-Edge connections have not yet been tested.
- **Outcome:** BLOCKED — connection setup remains outstanding.
- **Error reference:** None.
- **Notes:** Tracking checkpoint only, not a failed operation test. ChatGPT Free plan compatibility and the available connection route must be established before operation testing.
- **Update (2026-11-25):** Superseded by TC-002 and TC-003, both PASS. The project now uses Claude Desktop as the MCP client instead of ChatGPT.

## TC-001 — Local Python environment and test-discovery verification
- **Date:** 2026-11-25
- **Test area:** Environment / setup
- **Preconditions / test model:** Windows 11; upstream SolidEdge-MCP repository cloned locally; dependency installation completed with `uv sync --all-extras`.
- **User request:** Verify that the project environment uses the expected Python version and that its test suite can be discovered.
- **MCP tool(s):** None. Local PowerShell commands only.
- **Expected result:** The project runs under Python 3.11.9 and pytest can collect the project's tests without executing them.
- **Actual result:** `uv run python --version` returned Python 3.11.9. `uv run pytest --collect-only -q` completed successfully, according to the user. The user also confirmed that `.venv` exists and `uv sync --all-extras` completed successfully.
- **Outcome:** PASS
- **Error reference:** None. No errors reported.
- **Evidence:** User-confirmed command results; exact pytest collection output was not retained.
- **Notes / suggestion:** Environment is ready for the next stage: investigate the available MCP client connection route, then verify Solid Edge connectivity. Test collection does not mean the test suite itself has been executed.

---

Environment for TC-002 to TC-009: Windows 11; Solid Edge 2026 build 226.00.08.04; server `master` at commit 49384d0 (merge of upstream PR #4); Python 3.11.9; client Claude Desktop 6.28899.6. Each tool call was approved with "Allow once".

## TC-002: MCP client connection (Claude Desktop to the solidedge server)
- **Date:** 2026-11-25
- **Test area:** Connection
- **Preconditions / test model:** Server first run standalone with `uv --directory "%USERPROFILE%\GitHub\SolidEdge-MCP" run solidedge-mcp` (started, FastMCP banner shown, `git status --short` empty before and after). A `solidedge` entry was then added to `claude_desktop_config.json` and Claude Desktop was fully quit and restarted.
- **User request:** Have Claude Desktop launch the local server and make a tool call through it.
- **MCP tool(s):** manage_connection (see TC-003)
- **Expected result:** The server appears under Local MCP servers with a running status and its tools can be called.
- **Actual result:** After the first restart the server was not listed. After re-checking the saved file and doing a full quit and restart, `solidedge` was listed with status "running" (user report) and tool calls succeeded.
- **Outcome:** PASS
- **Error reference:** ERR-003 (setup issue on the way to this result)
- **Evidence:** User report of the "running" status; successful tool calls in TC-003 and later tests; the assistant's tool search listed 119 solidedge tools.
- **Notes / suggestion:** This Claude Desktop install had no Developer tab. The Local MCP servers section (with an Edit config button) is on the Extensions page under Desktop app. Edit config opened a folder under `%LOCALAPPDATA%\Claude-Data\`, not the `%APPDATA%\Claude` path shown in public MCP documentation. A full path to `uv.exe` was used in the config.

## TC-003: Solid Edge COM connection without launching a new instance
- **Date:** 2026-11-25
- **Test area:** Connection
- **Preconditions / test model:** Solid Edge 2026 already running, no documents open.
- **User request:** Connect to Solid Edge, do not start it if not running, change nothing, report the version.
- **MCP tool(s):** manage_connection with `action: connect`, `start_if_needed: false`
- **Expected result:** Connection to the running instance and a version report, with no document changes.
- **Actual result:** `{"status":"connected","version":"226.00.08.04","visible":true,"caption":"Solid Edge 2026"}`
- **Outcome:** PASS
- **Error reference:** None.
- **Evidence:** Exact response above. The user confirmed in Solid Edge Help > About that the version matches 226.00.08.04.
- **Notes / suggestion:** The upstream documentation reports its own testing on build 226.00.01.04, so this build differs from the author's. Keep this in mind when comparing results.

## TC-004: diagnose_api with no document open
- **Date:** 2026-11-25
- **Test area:** Connection / diagnostics
- **Preconditions / test model:** Connected to Solid Edge; no document open.
- **User request:** Inspect the COM connection and active document.
- **MCP tool(s):** diagnose_api
- **Expected result:** Diagnostic information about the connection and the active document.
- **Actual result:** `{"error":"No active document"}`
- **Outcome:** FAIL
- **Error reference:** ERR-001
- **Evidence:** Exact response above.
- **Notes / suggestion:** Classified as Model dependency (a document is a prerequisite). Retested with a document open in TC-007. See SG-001.

## TC-005: Bring Solid Edge window to the front
- **Date:** 2026-11-25
- **Test area:** Connection / window control
- **Preconditions / test model:** Solid Edge running behind the Claude Desktop window; no documents open.
- **User request:** Activate the Solid Edge window.
- **MCP tool(s):** manage_connection with `action: activate`, `start_if_needed: false`
- **Expected result:** The Solid Edge window comes to the front.
- **Actual result:** `{"status":"activated"}` (run twice, identical). The user saw only a red flash on the Solid Edge taskbar icon; the window did not come to the front.
- **Outcome:** INCONCLUSIVE
- **Error reference:** ERR-002
- **Evidence:** Exact response above; user observation of the taskbar flash.
- **Notes / suggestion:** Possibly Windows focus rules, not confirmed. Workaround: click the taskbar icon. See SG-004.

## TC-006: Create a blank part document
- **Date:** 2026-11-25
- **Test area:** Document lifecycle
- **Preconditions / test model:** Connected; no documents open. Permission to create one blank test part was given by the user.
- **User request:** Create one blank test part, without saving.
- **MCP tool(s):** create_document with `type: part`
- **Expected result:** A new empty part document opens and becomes active.
- **Actual result:** `{"status":"created","type":"Part","name":"Part1","path":"Part1"}`
- **Outcome:** PASS
- **Error reference:** None.
- **Evidence:** Exact response above. The user verified a blank Part1 opened, no pop-ups appeared (other than the Claude "Allow once" permission prompt), and no other files were open.
- **Notes / suggestion:** None.

## TC-007: diagnose_api with a blank part open (retest of TC-004)
- **Date:** 2026-11-25
- **Test area:** Connection / diagnostics
- **Preconditions / test model:** Blank Part1 open and active (from TC-006).
- **User request:** Inspect the COM connection and active document.
- **MCP tool(s):** diagnose_api
- **Expected result:** Diagnostic information about the active document.
- **Actual result:** Returned data instead of an error. It began `{"document_type":"CDispatch","available_collections":["Models","ProfileSets","Constructions","RefPlanes","Sketches"],...` followed by long lists of method names, including Python internals such as `__getattr__`.
- **Outcome:** PASS
- **Error reference:** None.
- **Evidence:** The full output is in the Claude Desktop chat and was not copied here because it is very long.
- **Notes / suggestion:** The output is verbose and `CDispatch` is not a meaningful document type for a user. See SG-002.

## TC-008: Close the test part without saving
- **Date:** 2026-11-25
- **Test area:** Document lifecycle
- **Preconditions / test model:** Blank Part1 open; it was the only document open.
- **User request:** Close the active document without saving.
- **MCP tool(s):** close_document with `save: false`, `scope: active`
- **Expected result:** Part1 closes with no save prompt and nothing is written to disk.
- **Actual result:** `{"status":"closed","document":"Part1","saved":false}`
- **Outcome:** PASS
- **Error reference:** None.
- **Evidence:** Exact response above; user confirmation that Part1 is no longer open.
- **Notes / suggestion:** None.

## TC-009: Tools, prompts and resources visible in Claude Desktop
- **Date:** 2026-11-25
- **Test area:** Connection / capability discovery
- **Preconditions / test model:** Server running in Claude Desktop (TC-002).
- **User request:** Check which tools, prompts and resources Claude Desktop offers.
- **MCP tool(s):** None (client menus and the assistant's tool search).
- **Expected result:** About 119 tools, 53 resources and 4 prompts, as described in the upstream README.
- **Actual result:** The assistant's tool search listed 119 solidedge tools. In the menu opened from the `>` arrow beside the Solid Edge panel, the user sees about 44 items, including items named "new part", "design review" and "sketch info". The user's earlier report of about 44 tools may refer to this same list; tools were not recounted separately. The upstream code at commit 49384d0 defines 4 prompts: new_part, design_review, manufacturability_check, troubleshoot_feature.
- **Outcome:** INCONCLUSIVE
- **Error reference:** None.
- **Evidence:** User observation; upstream source at commit 49384d0.
- **Notes / suggestion:** The open-document list is offered as MCP resources (`solidedge://document/list`, `solidedge://document/count`, `solidedge://document/active-type`), not as a tool, and no resource-reading tool was available to the assistant in this chat. An AI that only uses tools would miss this information. See SG-003.
