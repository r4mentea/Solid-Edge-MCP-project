# Error Log

## Summary
4 errors recorded. All are currently rated Low. No crash, data loss or model corruption was observed.

| ID | Test | Issue | Severity | Status |
|---|---|---|---|---|
| ERR-001 | TC-004 | diagnose_api requires an open document | Low | Expected limitation |
| ERR-002 | TC-005 | activate did not bring window forward | Low | Investigating |
| ERR-003 | TC-002 | Server absent after first restart | Low | Workaround found |
| ERR-004 | TC-011 | Reference component colour could not be read | Low* | Investigating |

*The error log's operational severity is Low; the related design-work impact is assessed as P1 High in [the flaws report](../FLAWS_AND_MCP_LIMITATIONS.md).

## ERR-001 — No active document
**Tool:** `diagnose_api`  
**Response:** `{"error":"No active document"}`  
**Cause:** No document was open. With a blank part open, the tool returned data (TC-007).  
**Workaround:** Open a document first.

## ERR-002 — Window activation
**Tool:** `manage_connection`, action `activate`  
**Response:** `{"status":"activated"}`  
**Observed:** Solid Edge taskbar icon flashed, but the window did not come forward (twice).  
**Workaround:** Click the taskbar icon. Windows focus rules are a possible cause, not confirmed.

## ERR-003 — Server missing after restart
**Environment:** Claude Desktop 6.28899.6, Windows 11  
**Observed:** Local MCP servers list was empty after the first restart, although the config entry was present.  
**Workaround:** Fully quit Claude Desktop from the tray and restart it. Server then appeared as running. Cause not isolated.

## ERR-004 — Cannot read component colour
**Tool:** `query_component`  
**Model:** `seaabtb_yc.asm`, reference `oilsprt.asm:1`  
**Observed:** `style` returned empty; `face_style` returned null; `bodies` returned zero bodies. Other tested query paths exposed no colour value.  
**Impact:** TC-011 could not match or verify the reference colour.  
**Workaround:** User reads RGB in Solid Edge and supplies it.  
**Cause:** Missing read capability confirmed for tested query paths; source-level cause unknown.
