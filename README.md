# Solid Edge MCP Evaluation

This project tests how reliably the [SolidEdge-MCP server](https://github.com/tylerwagler/SolidEdge-MCP) can perform CAD work in Siemens Solid Edge.

## Current status
- 11 tests: **7 PASS, 1 FAIL, 1 BLOCKED, 2 INCONCLUSIVE**
- 4 errors recorded; all currently rated Low in the test log.
- 1 confirmed high-priority design-work gap: component colour cannot be read through the tested tools.
- No Critical (P0) design-work flaw confirmed.

## Main findings
- [High-priority design-work flaws](FLAWS_AND_MCP_LIMITATIONS.md)
- [Test results](Testing/Test_Results.md)
- [Errors](Testing/Error_Log.md)
- [Improvement suggestions](Testing/Suggestions.md)

## Project files
- `Testing/` — test plan, results, errors and suggestions.
- `Documentation/` — setup, architecture and known limitations.
- `Future_Development/` — future proposals.

The evaluation records evidence, not assumptions. A successful tool response does not prove the CAD model is correct; verify the result in Solid Edge.

The upstream server repository is separate. Do not modify it without explicit approval.
