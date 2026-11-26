# High-Priority Design-Work Flaws

Only confirmed P1 (High) and P0 (Critical) issues affecting CAD design work are listed here. Setup and connection issues remain in the test records.

## Summary
- **P1 High: 1 confirmed**
- **P0 Critical: 0 confirmed**

## FLAW-001 — Cannot read a reference component's colour

**Priority:** P1 High  
**Evidence:** TC-010, TC-011; ERR-004; SG-005

The server successfully changed the tube colour to yellow in TC-010. In TC-011, the tested query options could not read the colour of reference component `oilsprt.asm:1`. The AI therefore could not match the tube's colour exactly. Gold was tried as an estimate, not a verified match.

**Impact:** The AI can set a colour but cannot reliably read, copy or verify a reference colour.

**Needed:** A read-only appearance query returning the effective colour (such as RGB), style and assignment level, plus a way to read back the result.

**Workaround:** The user reads the RGB value in Solid Edge and supplies it. Visual verification is still required.

The missing read capability is confirmed for the query paths tested. The underlying source-code cause has not been established.

## Critical issues
None confirmed.

## Next areas to test
These are unverified investigation areas, not confirmed flaws:
- Read back feature dimensions and properties after edits.
- Edit existing features and parameters.
- Identify intended faces and edges for holes, fillets and chamfers.
- Read assembly component properties.
- Verify resulting model geometry after changes.

Add a finding only when test evidence shows a material design-work impact.
