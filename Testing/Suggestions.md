# Suggestions and Workarounds

| ID | Related test | Suggestion | Benefit | Status |
|---|---|---|---|---|
| SG-001 | TC-004 | Return connection status even when no document is open | Clearer diagnostics | Proposed |
| SG-002 | TC-007 | Remove Python internals from diagnostic output | Easier to read | Proposed |
| SG-003 | TC-009 | Add a tool to list open documents | Works for tool-only clients | Proposed |
| SG-004 | TC-005 | Report whether the window actually reached the foreground | Avoids misleading success | Proposed |
| SG-005 | TC-011 | Add a read-only component/body/face colour query | Enables colour matching and verification | Proposed |
| SG-006 | TC-010 | Add screenshot or viewport capture | Enables visual checks | Proposed |

## Workarounds
- **SG-001:** Open a document before running `diagnose_api`.
- **SG-004:** Click the Solid Edge taskbar icon.
- **SG-005:** Read the RGB value in Solid Edge and provide it to the AI.
- **SG-006:** Ask the user to confirm the visual result.

Suggestions are proposals, not implemented fixes. Upstream code changes require separate approval.
