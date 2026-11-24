# Test Plan

## Objective
Evaluate the SolidEdge-MCP server in a controlled and repeatable way. Identify supported capabilities, operating conditions, expected limitations, unexpected failures, and improvement opportunities.

## Test record fields
| Field | Description |
|---|---|
| Test ID | Unique identifier, e.g. TC-001 |
| Date | Date run |
| Test area | Connection, document, sketch, feature, assembly, etc. |
| Scenario | User request and operation |
| Preconditions | Required document, geometry, selections, and application state |
| MCP tool(s) | Tool names invoked, where known |
| Expected result | Intended behaviour |
| Actual result | What Solid Edge and the MCP server did |
| Outcome | PASS, FAIL, BLOCKED, or INCONCLUSIVE |
| Error reference | Related entry in Error_Log.md |
| Evidence | Exact error text, screenshots, logs, or model observations |
| Notes / suggestion | Workaround or improvement idea |

## Outcome definitions
- **PASS** — Operation completed and resulting CAD state was verified.
- **FAIL** — Operation did not meet the expected result.
- **BLOCKED** — Testing could not proceed due to a prerequisite, access, connection, or environment issue.
- **INCONCLUSIVE** — Result could not be verified or evidence was insufficient.

A success response alone is insufficient; inspect the model to verify the result.

## Failure classification
- Expected limitation — Server intentionally refuses a documented unsupported operation.
- Functional failure — A supported operation fails unexpectedly.
- Input validation — Parameters are missing, invalid, or outside accepted bounds.
- Model dependency — Model or document does not meet operation prerequisites.
- COM / Solid Edge API — Automation or application-level failure.
- MCP connection — ChatGPT/client cannot reach or communicate with the server.
- Unexpected behaviour — Operation completes but produces an incorrect result.
- Environment / setup — Python, dependencies, permissions, configuration, or installation problem.

## Planned test phases
1. Environment setup and dependency verification.
2. MCP connection and Solid Edge connection.
3. Document lifecycle: create, open, save, close.
4. Sketch creation and geometry.
5. Part features: extrude, cut, hole, fillet, chamfer, patterns, and other available operations.
6. Sheet-metal workflows.
7. Assembly workflows.
8. Drafting and export workflows.
9. Limit tests: invalid inputs, missing references, unusual dimensions, repeated calls, and unsupported cases.
10. Final capability and reliability assessment.

This is a plan, not a claim that every listed operation is supported.

## Safety and repeatability
- Use dedicated test files or copies; do not begin with important production models.
- Record Solid Edge version, server version/commit, and environment details.
- Change one main variable at a time when investigating a failure.
- Preserve exact error messages and logs.
- Inspect the model after modifying operations.
- Avoid broad or destructive tests until basic connection and recovery are understood.

## Current status
No Solid Edge MCP operation tests have been executed. Setup and connection checks are pending.
