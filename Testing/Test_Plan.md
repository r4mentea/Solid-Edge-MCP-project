# Test Plan

## Objective
Test whether SolidEdge-MCP can create, modify, inspect and verify CAD models reliably.

## Test phases
1. Environment and dependencies — complete.
2. MCP and Solid Edge connections — complete.
3. Document lifecycle — started; create and close tested.
4. Sketches and geometry.
5. Part features.
6. Sheet metal.
7. Assemblies.
8. Drafting and export.
9. Invalid inputs, limits and recovery.
10. Overall capability assessment.

Planned operations are not claims of support.

## Test outcomes
- **PASS** — Result achieved and verified in Solid Edge.
- **FAIL** — Operation attempted but expected result not achieved.
- **BLOCKED** — A prerequisite prevented the test.
- **INCONCLUSIVE** — Evidence was insufficient to decide.

## Test requirements
Use test files or copies. Record the Solid Edge and server versions, tool responses, actual model result and relevant evidence. Preserve existing test IDs. Never treat a success response alone as proof of a correct model.

## Current next steps
Continue with document save/open tests, then sketch and part-feature operations. Revisit TC-011 when the reference colour can be read or the user supplies its RGB value.
