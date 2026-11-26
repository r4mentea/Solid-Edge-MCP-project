# High-Priority Design-Work Flaws in the Solid Edge MCP Server

> **Primary management review — P1 High and P0 Critical only**
>
> This report is deliberately limited to MCP capability gaps that materially prevent or compromise actual CAD design work: creating, modifying, inspecting, or verifying model geometry and properties.
>
> Document setup, opening/closing files, connection management, window activation, diagnostics, and general client-integration issues are outside this report's scope. They remain in the project's test records where appropriate.

## Executive summary

**One high-priority design-work limitation has been confirmed in the tests completed so far.**

The tested MCP tools can change a component's appearance, but they could not retrieve the reference component's colour in a usable form. This prevents the AI from reliably matching one component's colour to another without manual inspection by the user.

This is a meaningful design-work gap: the AI can perform the write operation, but it lacks the information needed to determine the correct value and verify an exact match.

No Critical (P0) design-work flaw has been established by the current test evidence.

## High-priority findings

### FLAW-001 — Cannot read a reference component's colour to perform an exact colour match

**Priority: P1 — High**

**Status:** Confirmed capability gap in the tested MCP tool workflow; underlying implementation cause not yet established.

**Affected design work:** Assembly component appearance / colour matching.

**Related records:** TC-010, TC-011; ERR-004; SG-005.

#### What the user wanted

Make the tube component in the assembly exactly the same colour as the reference component `oilsprt.asm:1`.

#### What the MCP server could do

In TC-010:
- The server identified the tube component, `Tube 1.par:1`.
- The `set_component_appearance` tool successfully changed its colour to yellow (RGB 255, 255, 0).
- The tool returned the updated appearance style, and a follow-up query returned `MCP FFFF00`.
- The user visually confirmed that the tube appeared yellow.

This demonstrates that the tested workflow can set a component's appearance.

#### What the MCP server could not do

In TC-011, the AI attempted to inspect the reference component's appearance before applying it to the tube.

The tested `query_component` options did not provide usable colour information:
- `style` returned an empty value.
- `face_style` returned null.
- `bodies` returned zero bodies for the queried occurrence.
- The attempted `info`, `tree`, and `sub_occurrences` queries did not expose the required colour value.

A gold colour (RGB 255, 215, 0) was tried as an estimate. It was not measured from the reference component and was not established as an exact match.

#### Why this is a serious design-work limitation

An exact colour-matching task requires the AI to complete a read-compare-write-verify sequence:

1. Read the reference component's existing colour.
2. Obtain an unambiguous value, such as RGB or a style identifier.
3. Apply that same value to the target component.
4. Read back the target appearance and verify that it matches.

The tested MCP workflow supports changing appearance, but it did not provide the information needed to complete the first step for the reference component.

As a result:
- The AI has to guess the colour or ask the user to inspect Solid Edge.
- Exact matching cannot be completed independently.
- The AI cannot reliably verify that the target matches the reference.
- Similar read-before-write gaps could affect other appearance-driven design tasks if the relevant properties are also not exposed.

The last point is a risk to investigate, not a confirmed limitation beyond colour.

#### MCP capability needed

Add a read-only appearance query for assembly occurrences/components and, where supported, their underlying parts, bodies, and faces.

The query should return:
- RGB values or another unambiguous colour representation;
- appearance/style name or identifier, where available;
- where the appearance is assigned (occurrence, part, body, or face);
- effective appearance information, including inherited or overridden values where the API makes these available.

The server should also support reading the appearance back after a change so the AI can verify the result.

A possible tool name is `get_component_appearance`. This is a recommendation, not an existing MCP tool.

#### Current workaround

The user manually inspects the reference component in Solid Edge, supplies its RGB value, and asks the AI to apply that value to the target. The user then visually checks the result.

This workaround requires human intervention and does not provide an independent automated match.

#### Evidence

- [TC-010 and TC-011 — Test Results](Testing/Test_Results.md)
- [ERR-004 — Error Log](Testing/Error_Log.md)
- [SG-005 — Suggestions](Testing/Suggestions.md)

**Cause assessment:** The missing read capability is confirmed for the tool/query paths tested. The exact reason has not yet been confirmed through source-code inspection. It may be an omitted MCP getter, an incomplete query implementation, or complexity in how Solid Edge assigns and inherits appearance properties.

---

## Critical-priority findings

**None confirmed at this stage.**

A P0 finding should be added only when test evidence demonstrates a critical design-work impact, such as serious model corruption, data loss, or another issue that makes continued operation unsafe.

## High-priority issues to investigate next

These are investigation areas, not confirmed flaws. They must be tested before being added as findings.

| Investigation area | What to test | Why it matters to design work |
|---|---|---|
| Geometry/property read-back | After creating or modifying a feature, can the MCP server retrieve its resulting dimensions and properties? | The AI needs to verify that a design change produced the intended model. |
| Feature editability | Can the AI identify and edit an existing feature and its parameters, rather than only create new geometry? | Real design work involves modifying existing models, not just creating new ones. |
| Face/edge identification | Can the AI reliably identify the intended face or edge for operations such as holes, fillets, and chamfers? | Incorrect selection can modify the wrong geometry or block the operation. |
| Assembly component properties | Can the AI read and compare relevant component properties and appearances? | Assembly-level design often requires matching or checking existing components. |
| Model-state verification | Can the AI inspect the resulting geometry after an operation and detect whether it is correct? | A successful tool response alone does not prove the CAD model is correct. |

These investigations should receive P1 status only if testing demonstrates that the gap materially blocks or compromises a design workflow.

## Priority rules for this report

- **P0 — Critical:** A confirmed issue causes severe design-work impact, such as model corruption, data loss, or an unsafe result requiring testing to stop.
- **P1 — High:** A confirmed MCP capability gap prevents an important design task from being completed reliably or independently, or prevents meaningful verification of the result.
- **P2 and below:** Excluded from this management report. Keep these in the detailed testing records instead.

## Maintenance rules

- Include only P0 and P1 design-work findings in this file.
- Do not include document-opening, setup, connection, window-management, or diagnostic-only problems.
- Every finding must link to test evidence and relevant error/suggestion records.
- Clearly distinguish confirmed behaviour from suspected implementation causes.
- Do not turn an investigation idea into a confirmed flaw before testing it.
- When a limitation is fixed, record the server version/commit and a successful retest before marking it resolved.
- Keep detailed test steps and raw evidence in `Testing/Test_Results.md`.
- This document is the concise, high-priority management view of design-work limitations.
