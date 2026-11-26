# Solid Edge MCP Project

A workspace for evaluating the capabilities, limitations, reliability, and potential improvements of the [SolidEdge-MCP server](https://github.com/tylerwagler/SolidEdge-MCP).

## Main priority for management review

**[High-Priority Design-Work Flaws](FLAWS_AND_MCP_LIMITATIONS.md)** — a focused report of P1 High and P0 Critical MCP capability gaps that materially affect CAD design work: creating, modifying, inspecting, and verifying models.

The current confirmed high-priority finding is the inability of the tested tool workflow to read a reference component's colour for an exact colour match (TC-011 / ERR-004). No Critical (P0) design-work flaw has been confirmed yet. Setup, document-opening, connection, window-management, and diagnostic issues are intentionally excluded from this report and remain in the detailed test records.

## Project purpose

This repository records structured tests of AI-assisted operations in Siemens Solid Edge: what works, under which conditions, what fails, and what could be improved.

## Repository structure
- **FLAWS_AND_MCP_LIMITATIONS.md** — Management-facing report limited to high-priority design-work flaws (P1/P0).
- **Testing/** — Test plan, results, error log, and suggestions.
- **Documentation/** — Setup notes, architecture, and known limitations.
- **Future_Development/** — Improvement proposals.

## Testing principles
1. Give every test a unique ID.
2. Preserve exact error messages and evidence.
3. Distinguish expected limitations from unexpected failures.
4. Verify the resulting CAD model in Solid Edge; do not rely only on a success response.
5. Use dedicated test files or copies to protect important models.
6. Do not modify the upstream server repository as part of routine testing.

## Current status
- Local development environment: Complete
- Upstream repository cloned and dependencies installed: Complete
- Python environment and pytest discovery checks: Passed (TC-001)
- Claude Desktop-to-MCP connection: Passed (TC-002)
- MCP-to-Solid-Edge connection: Passed (TC-003)
- Tests executed: 11 (7 passed, 1 failed, 1 blocked, 2 inconclusive)
- Errors recorded: 4 (all low severity)

See [Testing/Test_Plan.md](Testing/Test_Plan.md), [Testing/Test_Results.md](Testing/Test_Results.md), and the [high-priority design-work flaws report](FLAWS_AND_MCP_LIMITATIONS.md).

## Upstream implementation

The implementation selected for evaluation is [tylerwagler/SolidEdge-MCP](https://github.com/tylerwagler/SolidEdge-MCP). This repository remains separate so test evidence and recommendations are not mixed with upstream source code.
