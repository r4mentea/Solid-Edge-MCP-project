# Known Limitations

## Upstream-documented
The upstream [SolidEdge-MCP project](https://github.com/tylerwagler/SolidEdge-MCP) identifies operations it refuses because of Solid Edge API defects, crash risks or UI-only selection requirements. Check upstream documentation for the current list.

## Observed in this evaluation
- **Component colour read-back:** Tested query paths did not return the reference component's colour. This blocked exact colour matching in TC-011. See ERR-004 and [FLAW-001](../FLAWS_AND_MCP_LIMITATIONS.md).

Other design-work areas remain untested. See [Test Plan](../Testing/Test_Plan.md).
