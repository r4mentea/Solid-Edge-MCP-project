# Test Results

## Summary
| Outcome | Count |
|---|---:|
| PASS | 7 |
| FAIL | 1 |
| BLOCKED | 1 |
| INCONCLUSIVE | 2 |
| **Total** | **11** |

## Test register
| ID | Date | Test | Result | Error |
|---|---|---|---|---|
| TC-001 | 2026-11-25 | Python environment and pytest discovery | PASS | — |
| TC-002 | 2026-11-25 | Claude Desktop to MCP server | PASS | ERR-003 |
| TC-003 | 2026-11-25 | MCP connection to running Solid Edge | PASS | — |
| TC-004 | 2026-11-25 | diagnose_api with no document open | FAIL | ERR-001 |
| TC-005 | 2026-11-25 | Bring Solid Edge window to foreground | INCONCLUSIVE | ERR-002 |
| TC-006 | 2026-11-25 | Create blank part | PASS | — |
| TC-007 | 2026-11-25 | diagnose_api with blank part open | PASS | — |
| TC-008 | 2026-11-25 | Close blank part without saving | PASS | — |
| TC-009 | 2026-11-25 | Tools, prompts and resources visibility | INCONCLUSIVE | — |
| TC-010 | 2026-11-26 | Change assembly tube colour to yellow | PASS | — |
| TC-011 | 2026-11-26 | Match tube colour to reference component | BLOCKED | ERR-004 |

## Test details

### TC-001 — Environment check
Windows 11 environment prepared. `uv sync --all-extras` completed; Python 3.11.9 verified; `uv run pytest --collect-only -q` completed. Tests were collected, not executed. Evidence: user-confirmed command results.

### TC-002 — MCP client connection
Claude Desktop launched the local `solidedge` server. It appeared as running after a full quit and restart. First restart did not show it. See ERR-003.

### TC-003 — Solid Edge connection
Connected to the running Solid Edge 2026 instance without launching another instance or changing documents. Response reported build `226.00.08.04`; user confirmed the version.

### TC-004 — diagnose_api without a document
Returned `{"error":"No active document"}`. Retested successfully with a blank part in TC-007. See ERR-001.

### TC-005 — Window activation
Tool returned `{"status":"activated"}`, but the user saw only a taskbar flash; the window did not come forward. Result remains inconclusive. See ERR-002.

### TC-006 — Create blank part
`create_document` returned Part1. User confirmed the blank part opened and no other documents were affected.

### TC-007 — diagnose_api with a document
Returned diagnostic data with a blank part open. Output included verbose Python internals and `CDispatch`.

### TC-008 — Close blank part
`close_document` returned `{"status":"closed","document":"Part1","saved":false}`. User confirmed Part1 closed.

### TC-009 — Client capability visibility
Tool search listed 119 tools; the client menu showed about 44 items; upstream source at commit `49384d0` defined 4 prompts. Resource visibility was not fully verified.

### TC-010 — Change tube colour
In `seaabtb_yc.asm`, `Tube 1.par:1` was identified at index 2. `set_component_appearance` applied RGB 255,255,0 and returned `MCP FFFF00`. Follow-up query and user visual check confirmed yellow. Assembly was not saved.

### TC-011 — Match reference colour
The reference `oilsprt.asm:1` was at index 1. Tested queries returned empty style, null face_style, zero bodies, and no colour through info/tree/sub-occurrences. Gold (255,215,0) was tried but not verified as a match. Blocked because the reference colour could not be read. See ERR-004.

## Test environment
TC-002 to TC-009: Windows 11, Solid Edge 2026 build 226.00.08.04, server commit `49384d0`, Python 3.11.9, Claude Desktop 6.28899.6.  
TC-010 and TC-011 used the same environment and the Solid Edge training assembly. Neither test saved changes to the assembly file.

TC-000 was an initial setup checkpoint. It was superseded by TC-002 and TC-003 and is not included in the test count.
