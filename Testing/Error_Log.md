# Error Log

Record every observed error from setup, connection, tool execution, or model operations. Preserve the exact original message wherever possible.

## Summary
No errors have been observed during Solid Edge MCP testing yet. Operation testing has not started.

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
| — | — | — | — | No errors recorded yet | — |

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
