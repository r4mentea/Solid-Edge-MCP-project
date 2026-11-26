# Suggestions and Workarounds

Record improvement ideas separately from confirmed defects. A suggestion is not a verified fix until it has been tested.

## Suggestions register
| Suggestion ID | Related test / error | Area | Proposal | Evidence / rationale | Status |
|---|---|---|---|---|---|
| SG-001 | ERR-001 / TC-004 | diagnose_api | Return connection information even when no document is open | diagnose_api returned only an error with no document | Proposed |
| SG-002 | TC-007 | diagnose_api | Hide Python internals and show a meaningful document type | Output included names like `__getattr__` and `CDispatch` | Proposed |
| SG-003 | TC-009 | Resources / tools | Offer a tool that lists open documents, since the list exists only as a resource | Tool-only clients cannot read resources | Proposed |
| SG-004 | ERR-002 / TC-005 | manage_connection | Report whether the window actually reached the foreground | activate returned success while the window only flashed | Proposed |
| SG-005 | ERR-004 / TC-011 | query_component / appearance | Add a read-only color query returning RGB and style name for a component, body or face | The color of oilsprt.asm could not be read, which blocked TC-011 | Proposed |
| SG-006 | TC-010 | Visual verification | Add a screenshot or viewport capture tool, or inline image return | The assistant has no view of the Solid Edge window, so visual results depend on the tester | Proposed |

## Suggestion entry template
### SG-XXX — Short proposal
- **Related test / error:**
- **Area:**
- **Observed problem or opportunity:**
- **Suggested improvement:**
- **Potential benefit:**
- **Possible risk or trade-off:**
- **Workaround, if applicable:**
- **Confidence:** Idea / Plausible / Reproduced / Verified
- **Status:** Proposed / Under review / Accepted / Rejected / Implemented elsewhere

## Ground rules
- Keep suggestions separate from confirmed errors.
- Prefer specific, testable recommendations.
- Do not change the upstream MCP server without explicit authorisation.
- Record whether a workaround was actually tested.

---

## SG-001: Return connection information when no document is open
- **Related test / error:** TC-004, ERR-001
- **Area:** diagnose_api
- **Observed problem or opportunity:** With no document open, the tool returned `{"error":"No active document"}` and no connection details.
- **Suggested improvement:** Report the connection and application state, and say clearly that no document is open.
- **Potential benefit:** Useful for first-step troubleshooting.
- **Possible risk or trade-off:** Slightly different output shape for callers that expect an error.
- **Workaround, if applicable:** Open a document first. Not tested beyond TC-007.
- **Confidence:** Plausible
- **Status:** Proposed

## SG-002: Reduce noise in diagnose_api output
- **Related test / error:** TC-007
- **Area:** diagnose_api
- **Observed problem or opportunity:** The output listed Python internal names and used `CDispatch` as the document type.
- **Suggested improvement:** Filter names starting with an underscore and return the document type as Solid Edge names it (for example Part).
- **Potential benefit:** Shorter, easier-to-read diagnostics.
- **Possible risk or trade-off:** Some users may want the raw lists.
- **Workaround, if applicable:** None needed.
- **Confidence:** Plausible
- **Status:** Proposed

## SG-003: Offer a tool for listing open documents
- **Related test / error:** TC-009
- **Area:** Tools and resources
- **Observed problem or opportunity:** The open-document list is offered only as resources. No resource-reading tool was available to the assistant in this chat.
- **Suggested improvement:** Add a read-only tool that returns the same information.
- **Potential benefit:** Works with clients that use tools but not resources.
- **Possible risk or trade-off:** Duplicates information already offered as resources.
- **Workaround, if applicable:** The user can attach the resource manually if the client supports it. Not tested.
- **Confidence:** Idea
- **Status:** Proposed

## SG-004: Report real foreground state in activate
- **Related test / error:** TC-005, ERR-002
- **Area:** manage_connection
- **Observed problem or opportunity:** activate returned success while the window only flashed in the taskbar.
- **Suggested improvement:** Return whether the window actually reached the foreground, or add a note that Windows may block focus changes.
- **Potential benefit:** Avoids a false sense of success.
- **Possible risk or trade-off:** Detecting foreground state may need extra Windows API calls.
- **Workaround, if applicable:** Click the taskbar icon. Tested manually by the user only in the sense of the flash; not otherwise tested.
- **Confidence:** Idea
- **Status:** Proposed

## SG-005: Add a read-only color query
- **Related test / error:** TC-011, ERR-004
- **Area:** query_component / appearance
- **Observed problem or opportunity:** Color can be set (TC-010) but not read. Every color query tried returned empty or null, which is the root of the TC-011 block.
- **Suggested improvement:** Add a read-only query that returns RGB and style name for a component, body or face.
- **Potential benefit:** Colors could be matched between components and verified after a change.
- **Possible risk or trade-off:** Not assessed.
- **Workaround, if applicable:** The tester reads the RGB value in Solid Edge and supplies it.
- **Confidence:** Plausible
- **Status:** Proposed

## SG-006: Add a screenshot or viewport capture tool
- **Related test / error:** TC-010
- **Area:** Visual verification
- **Observed problem or opportunity:** The assistant cannot see the Solid Edge window, so visual results such as the yellow tube had to be confirmed by the tester.
- **Suggested improvement:** Add a screenshot or viewport capture tool, or return an image inline.
- **Potential benefit:** Visual results could be checked without asking the tester.
- **Possible risk or trade-off:** Not assessed.
- **Workaround, if applicable:** Ask the tester to confirm visually.
- **Confidence:** Idea
- **Status:** Proposed