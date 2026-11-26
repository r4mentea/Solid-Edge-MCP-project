# MCP Server Flaws, Gaps and Limitations

> **Primary management review document**
>
> This document summarises capability gaps and problematic behaviours found while evaluating the SolidEdge-MCP server. It is intended to help prioritise improvements that affect real CAD workflows.
>
> **Scope:** Findings from this project's recorded tests only. A missing capability in the exposed MCP tools is described as a *capability gap*. It is not automatically proof of a bug in the underlying Solid Edge API or a defect in a specific line of server code.

## Executive summary

The most important finding so far is that the server can **change a component's appearance but the tested tool set could not read the reference component's colour**. This prevents an AI agent from reliably matching one component's colour to another without asking a person to inspect Solid Edge and provide the colour value.

Other observations concern diagnostic behaviour, window activation feedback, and access to document-list resources. Some may be server-side design gaps; others may depend on Windows or the MCP client. They are kept separate below so that the project does not attribute an issue to the server without sufficient evidence.

### Priority overview

| Finding | Priority | Evidence | Current assessment |
|---|---|---|---|
| FLAW-001 — Component colour cannot be read for matching | **P1 — High** | TC-010, TC-011; ERR-004; SG-005 | Confirmed exposed-tool capability gap; implementation cause not yet inspected |
| FLAW-002 — Diagnostics require an active document | P2 — Medium | TC-004, TC-007; ERR-001; SG-001 | Confirmed behaviour; likely avoidable diagnostic design limitation |
| FLAW-003 — Window activation can report success without foreground focus | P2 — Medium | TC-005; ERR-002; SG-004 | Reproduced observation; server-versus-Windows cause not established |
| FLAW-004 — Open-document information is not available through a tool in the tested client workflow | P2 — Medium | TC-009; SG-003 | Interoperability/access gap; resource support in other clients not assessed |
| FLAW-005 — No automated visual evidence was available in this test workflow | P3 — Low | TC-010; SG-006 | Verification limitation; absence of a server screenshot tool should be confirmed against the full tool inventory |

**Priority meaning:** P1 affects the ability to complete a meaningful CAD task reliably. P2 creates workflow friction or unreliable feedback. P3 reduces verification quality but does not by itself prevent the operation.

## Detailed findings

### FLAW-001 — Component colour cannot be read for matching

**P1 — High | Confirmed exposed-tool capability gap**

**What was requested**

Change the tube in the assembly so that it has exactly the same colour as the reference component `oilsprt.asm:1`.

**What happened**

- TC-010 demonstrated that the server can set the tube component's appearance: `set_component_appearance` changed it to yellow (RGB 255, 255, 0), and the user visually confirmed the result.
- In TC-011, the tested `query_component` options (`style`, `face_style`, `bodies`, `info`, `tree` and `sub_occurrences`) did not return the reference component's colour.
- The style query returned an empty value, the face-style query returned null, and the body query returned zero bodies for the queried occurrence.
- A gold colour was tried as an estimate. It was not a measured match and the user had not confirmed it as an exact match.

**Why this is a problem**

The exposed tools support writing an appearance value, but the tested query path does not expose the reference appearance in a usable form. An AI agent therefore cannot reliably perform a read-compare-write workflow:

1. Read the reference component's colour.
2. Convert or identify the colour as RGB / style information.
3. Apply that exact value to the target component.
4. Read the target colour back and verify the match.

Without the read step, the agent must guess or ask the user to inspect Solid Edge manually. This makes an apparently simple instruction dependent on human intervention and prevents programmatic verification.

**Likely capability missing**

A read-only appearance query for an occurrence/component, body and face, returning at least:
- RGB values (or another unambiguous colour representation);
- the assigned style / appearance name, where available;
- the level at which the appearance is assigned (occurrence, part, body or face);
- enough information to identify inherited or overridden appearance values.

A corresponding read-back after setting appearance would also allow verification.

**Recommended improvement**

Add a dedicated read-only tool such as `get_component_appearance` (name illustrative, not an existing tool). It should return the effective appearance and, where possible, distinguish an occurrence override from a part/body/face appearance. The current setter should remain available.

**Workaround**

Ask the tester to inspect the reference colour in Solid Edge and supply its RGB value, then set that value on the target and visually verify it. This is a manual workaround, not an automated match.

**Evidence and references:** TC-010, TC-011 in [Test Results](Testing/Test_Results.md); ERR-004 in [Error Log](Testing/Error_Log.md); SG-005 in [Suggestions](Testing/Suggestions.md).

**Cause confidence:** High that the tested MCP tool workflow lacks a usable colour-read capability. The precise implementation cause (for example, an omitted server tool, incomplete query implementation, or Solid Edge API/appearance inheritance complexity) has not yet been established by source-code inspection.

---

### FLAW-002 — Diagnostics fail when no document is open

**P2 — Medium | Behaviour confirmed**

**What happened**

In TC-004, calling `diagnose_api` while connected to Solid Edge with no active document returned:

```json
{"error":"No active document"}
```

In TC-007, the same diagnostic call returned data after a blank Part document was opened.

**Why this is a problem**

Connection diagnostics are especially useful before opening or creating a model. Requiring an active document means the tool cannot provide its full diagnostic output during an initial connection check. The current behaviour conflates two separate states: whether the server is connected to Solid Edge, and whether a document is active.

**Likely capability/design gap**

The diagnostic operation appears to depend on an active document, rather than reporting application-level connection information independently from document-level information.

**Recommended improvement**

Make `diagnose_api` report connection and application details even when no document is open. Return an explicit document state such as `active_document: null` and a clear message that document-specific inspection is unavailable until a document is opened.

**Workaround**

Open a document before calling `diagnose_api`.

**Evidence and references:** TC-004 and TC-007 in [Test Results](Testing/Test_Results.md); ERR-001 in [Error Log](Testing/Error_Log.md); SG-001 in [Suggestions](Testing/Suggestions.md).

**Cause confidence:** The behaviour is confirmed. The exact code-level cause has not been inspected.

---

### FLAW-003 — Window activation reports success without confirmed foreground focus

**P2 — Medium | Under investigation**

**What happened**

In TC-005, `manage_connection` with `action: activate` returned:

```json
{"status":"activated"}
```

The user observed only a taskbar flash; the Solid Edge window did not come to the foreground. The same observation occurred twice.

**Why this is a problem**

The returned status suggests that the requested action succeeded, but the visible application state did not match that claim. An AI agent may continue under the incorrect assumption that Solid Edge is now active and visible.

**Possible cause / gap**

The server may be reporting that an activation request was issued rather than verifying that Windows actually granted foreground focus. However, Windows foreground-window restrictions may also explain the behaviour. The current evidence does not prove which layer is responsible.

**Recommended improvement**

Where technically possible, check the foreground window after requesting activation and return separate outcomes, for example:
- activation request sent;
- Solid Edge confirmed as foreground;
- activation not confirmed (possibly blocked by Windows).

Do not return an unqualified success when foreground state has not been verified.

**Workaround**

Manually click the Solid Edge taskbar icon.

**Evidence and references:** TC-005 in [Test Results](Testing/Test_Results.md); ERR-002 in [Error Log](Testing/Error_Log.md); SG-004 in [Suggestions](Testing/Suggestions.md).

**Cause confidence:** The mismatch between the response and the user's observation is recorded. Whether it is a server defect or a Windows focus restriction remains unconfirmed.

---

### FLAW-004 — Open-document information is not available as a tool in the tested client workflow

**P2 — Medium | Interoperability gap**

**What happened**

TC-009 found that the upstream project defines document-list information as MCP resources, including:
- `solidedge://document/list`
- `solidedge://document/count`
- `solidedge://document/active-type`

A resource-reading tool was not available to the assistant in the tested chat workflow, so the assistant could not retrieve this information through the tools it could call.

**Why this is a problem**

An AI client that can invoke tools but cannot read MCP resources may be unable to discover which documents are open or which document is active. This can lead to unnecessary user questions, incorrect assumptions about the active model, or blocked workflows.

**Likely capability gap**

The server exposes useful state through resources, but the tested client/tool workflow did not provide the assistant with a way to read those resources. This may be a client integration limitation rather than a defect in the server itself.

**Recommended improvement**

Consider adding a read-only tool that returns the same open-document information, or ensure that the intended MCP clients can discover and read the resources. Avoid duplicating interfaces unless client compatibility requires it.

**Workaround**

Use a client that supports MCP resource reading, or have the user provide the open-document information manually.

**Evidence and references:** TC-009 in [Test Results](Testing/Test_Results.md); SG-003 in [Suggestions](Testing/Suggestions.md).

**Cause confidence:** The resource definitions and limitation in the tested workflow are recorded. Other MCP clients have not been tested for this behaviour.

---

### FLAW-005 — Visual verification was dependent on the human tester

**P3 — Low | Needs capability-inventory check**

**What happened**

In TC-010, the tool response and style read-back supported the colour change, but the user still had to look at the Solid Edge window and confirm that the tube appeared yellow. The assistant could not independently inspect the CAD viewport in this workflow.

**Why this is a problem**

A successful tool response or property read-back may not prove that the final visible result is correct. Without visual evidence, the AI cannot independently catch display-level issues, unexpected appearance overrides, or a mismatch between stored properties and what the user sees.

**Possible capability gap**

A screenshot or viewport-capture capability may be missing from the server/client workflow. This has not yet been established as a server defect; the full available tool inventory and any client-side image support should be checked.

**Recommended improvement**

Investigate whether the server or client can safely provide a screenshot / viewport image as evidence. If not, keep explicit user visual confirmation as a required test step. Any capture feature should avoid unintentionally exposing unrelated or confidential on-screen information.

**Workaround**

Ask the user to inspect the model in Solid Edge and record the visual confirmation.

**Evidence and references:** TC-010 in [Test Results](Testing/Test_Results.md); SG-006 in [Suggestions](Testing/Suggestions.md).

**Cause confidence:** Human verification was required in the recorded test. The absence of a suitable capture capability in the server itself remains to be confirmed.

---

## Issues not currently attributed to the MCP server

These are recorded in the test history but should not be presented as confirmed server flaws:

| Issue | Why it is excluded from confirmed server flaws |
|---|---|
| ERR-003 — Claude Desktop did not list the server after the first restart | A full quit/restart resolved it. The cause was not isolated and appears related to client configuration/start-up rather than a demonstrated server operation defect. |
| TC-009 tool-count / menu-count discrepancy | The assistant's tool search and the client menu showed different counts/representations. The test was inconclusive and does not establish missing server functionality. |
| TC-011 estimated gold colour | This was a manual estimate, not a measured match. It is evidence of the missing read capability, not evidence that the colour setter itself is inaccurate. |

## Recommended improvement order

1. **P1 — Add colour read-back / query capability.** This directly unblocks exact component colour matching and verification.
2. **P2 — Separate application diagnostics from active-document diagnostics.** This improves connection troubleshooting and initial workflow reliability.
3. **P2 — Make activation feedback truthful and verifiable.** Distinguish a requested activation from confirmed foreground focus.
4. **P2 — Improve document-state access for tool-oriented clients.** Expose document information through a compatible tool where resource access is unavailable.
5. **P3 — Investigate visual evidence capture.** Confirm whether this belongs in the server, client, or tester workflow before proposing implementation.

## How this document is maintained

- Add a finding only when supported by a test, reproducible observation, or verified source-code inspection.
- Link each finding to its Test ID, Error ID and Suggestion ID where applicable.
- Separate observed behaviour from suspected cause. Do not label a suspected cause as confirmed.
- Update the priority when business impact or new evidence changes.
- When a gap is fixed, record the relevant code version and a retest before marking it resolved.
- Keep detailed test steps and raw evidence in `Testing/Test_Results.md`; this file is the concise management-facing view.
- Do not modify the upstream MCP server as part of this documentation update.
