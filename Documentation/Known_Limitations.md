# Known Limitations

This file separates limitations documented by the upstream project from limitations independently observed during our tests.

## Upstream-documented information
The selected upstream project, [tylerwagler/SolidEdge-MCP](https://github.com/tylerwagler/SolidEdge-MCP), documents that some operations are intentionally refused because of known Solid Edge API defects, crash risks, or UI-only selection requirements.

The upstream project also describes verification coverage for its create operations. That coverage is not a guarantee that every operation works for every document, geometry, or application state.

Consult the upstream project's current documentation and verification-status reference for the latest details. This file does not replace upstream documentation.

## Independently observed limitations
None recorded yet. Testing has not started.

## Limitation record template
### LIM-XXX — Short limitation title
- **Source:** Upstream documentation / Observed during testing
- **Related test ID:**
- **Affected operation / tool:**
- **Conditions:**
- **Documented or observed behaviour:**
- **User impact:**
- **Workaround:**
- **Potential alternative workflow:**
- **Evidence:**
- **Status:** Unverified / Confirmed / Workaround available

## Important distinction
An expected refusal of a documented unsupported operation is not automatically a defect. We should still assess whether the refusal is safe, understandable, and sufficiently informative.
