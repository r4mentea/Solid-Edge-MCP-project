# FORFH — Collaborator Guide

## 1. Welcome

Welcome to the Solid Edge MCP Project!

This repository is a shared workspace for evaluating the capabilities, limitations, reliability, and potential improvements of the [SolidEdge-MCP server](https://github.com/tylerwagler/SolidEdge-MCP).

Our objective is to conduct structured and repeatable tests of AI-assisted operations in Siemens Solid Edge, document what works and what does not, and identify opportunities for improvement.

This repository is for testing documentation, evidence, findings, and recommendations. It is separate from the upstream MCP server source code.

## 2. Understanding the Repository

Before contributing, familiarise yourself with the repository structure:

| Folder / File | Purpose |
|---|---|
| `README.md` | Project overview and current status |
| `Testing/Test_Plan.md` | Planned tests, test phases and testing principles |
| `Testing/Test_Results.md` | Record of completed tests and their outcomes |
| `Testing/Error_Log.md` | Record of errors and unexpected behaviour |
| `Testing/Suggestions.md` | Workarounds and improvement suggestions |
| `Documentation/Setup_Guide.md` | Local environment setup instructions |
| `Documentation/MCP_Architecture.md` | Explanation of the MCP architecture |
| `Documentation/Known_Limitations.md` | Upstream-documented and independently observed limitations |
| `Future_Development/Improvement_Proposals.md` | Longer-term improvement proposals |

## 3. Getting Started

Before performing tests:

1. Read this guide and the main `README.md`.
2. Review `Documentation/Setup_Guide.md`.
3. Check the current testing status in `Testing/Test_Plan.md` and `Testing/Test_Results.md`.
4. Confirm that the required software, dependencies, and Solid Edge environment are available.
5. Confirm that you have the necessary authorisation to use the MCP client and local integrations.

The currently documented test environment uses Windows 11, Siemens Solid Edge 2026, Python 3.11.9, Git, Visual Studio Code, and uv.

Refer to the Setup Guide for the verified installation commands and environment details. Do not assume that a collaborator's environment is identical.

## 4. Testing Workflow

Every test should be recorded in a consistent and repeatable way.

### Before testing

- Check the test plan for an existing test covering the same operation.
- Assign a unique Test ID, such as `TC-002`.
- Record the required preconditions and test model.
- Identify the expected result.
- Use a dedicated test document or a copy of an existing model.

### During testing

- Record the exact user request and MCP tool names, where available.
- Observe both the MCP response and the resulting Solid Edge state.
- Preserve exact error messages.
- Capture screenshots, logs, or other useful evidence.
- Avoid changing several variables at once when investigating a failure.

### After testing

- Record the actual result in `Testing/Test_Results.md`.
- Assign an outcome: PASS, FAIL, BLOCKED, or INCONCLUSIVE.
- Link any related error using its Error ID.
- Add workarounds or improvement ideas to `Testing/Suggestions.md`.
- Update the relevant documentation when a finding changes what the team knows.

A successful MCP response alone does not prove that a CAD operation succeeded. The resulting model must be inspected in Solid Edge.

## 5. Recording Test Results

Use the following outcome definitions:

- **PASS:** The operation completed and the resulting CAD state was verified.
- **FAIL:** The operation did not meet the expected result.
- **BLOCKED:** Testing could not proceed because of a prerequisite, access, connection, or environment issue.
- **INCONCLUSIVE:** The result could not be verified or the available evidence was insufficient.

Every test should have a unique ID and enough information for another collaborator to understand and reproduce it.

Follow the test entry template in `Testing/Test_Results.md`.

## 6. Reporting Errors

Record observed errors in `Testing/Error_Log.md`.

Each error should include:

- A unique Error ID, such as `ERR-001`.
- The related Test ID.
- The environment and relevant software versions.
- The MCP tool involved.
- The exact original error message.
- Steps to reproduce the issue.
- Expected and actual behaviour.
- Error category and severity.
- Supporting evidence.
- Any workaround or suggested improvement.

Do not paraphrase an error message when the original text is available.

An intentionally refused or unsupported operation is not automatically a software defect. Check the upstream documentation before classifying it as an unexpected failure.

## 7. Suggesting Improvements

Record improvement ideas and workarounds in `Testing/Suggestions.md`.

A useful suggestion should explain:

1. What problem or opportunity was observed?
2. Which test or error revealed it?
3. What improvement is proposed?
4. What benefit could it provide?
5. Are there any possible risks or trade-offs?
6. Has the proposed workaround or improvement actually been tested?

Keep suggestions separate from confirmed defects. An idea should not be described as a verified fix until it has been tested.

## 8. Collaboration and GitHub Contributions

To keep the repository organised:

- Read the relevant documentation before making changes.
- Make focused changes rather than rewriting unrelated sections.
- Preserve existing test IDs, Error IDs, and historical results.
- Check the latest version of a file before editing it.
- Avoid overwriting another collaborator's changes.
- Use clear commit messages that describe what changed.
- Explain the reason for significant documentation changes.
- Ask for clarification before making changes that could affect the project's structure or testing methodology.

When submitting a change, include a short explanation of the file changed and the reason for the change.

## 9. Important Project Restrictions

### Protect the upstream repository

The upstream implementation is maintained separately at [tylerwagler/SolidEdge-MCP](https://github.com/tylerwagler/SolidEdge-MCP).

This evaluation project does not authorise collaborators to modify the upstream server.

Do not modify upstream source code, configuration, or behaviour without explicit authorisation from the project owner.

### Protect CAD files

- Use dedicated test documents or copies.
- Do not begin tests using important production models.
- Avoid broad or destructive tests until basic behaviour and recovery procedures are understood.
- Verify the resulting model after modifying operations.
- Preserve evidence if an operation causes unexpected application or model behaviour.

### Follow organisational security requirements

- Use only AI clients, MCP integrations, and software connections approved for the project.
- Do not upload confidential CAD files, internal company information, credentials, or proprietary data to unapproved external services.
- Do not bypass organisational security controls or expose the local MCP server through an unauthorised public connection.

## 10. General Testing Principles

All collaborators should follow these principles:

1. Be accurate: record what actually happened, not what was expected to happen.
2. Be reproducible: provide enough detail for another person to repeat the test.
3. Be transparent: preserve exact errors and supporting evidence.
4. Be cautious: protect important CAD documents and the local environment.
5. Be objective: distinguish expected limitations from unexpected failures.
6. Be collaborative: keep documentation clear, organised, and up to date.

## 11. Final Note

The value of this project depends on the quality of its testing evidence and documentation.

Every completed test, reproducible error, useful workaround, and well-supported suggestion helps build a clearer understanding of the Solid Edge MCP server.

If you are unsure how to classify a result or where to record a finding, ask before making assumptions.
