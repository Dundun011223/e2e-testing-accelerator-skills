---
name: sbnc-e2e-playwright-script-designer
description: "Design Playwright E2E test scripts using the E2E Testing Accelerator's Facade, Node, and Spec architecture. Covers test structure, reusable components, locator placeholders, and assertion candidates. Use when converting test cases or test manifests into implementation-ready designs and unvalidated script drafts."
---

# SBNC E2E Playwright Script Designer

## Purpose and use

Use this Skill when a user provides a Test Manifest or existing test cases and wants a Playwright design and script draft. This is a design-only Skill: use only the conversation and material explicitly supplied by the user. Do not access an application, inspect a workspace, or perform implementation or execution work.

## Inputs

- **Required:** a Test Manifest or test cases with steps and expected results.
- **Optional:** existing Facade/Node IDs and descriptions, framework conventions, test data, and user-supplied application evidence such as confirmed accessible names, test IDs, or DOM snippets.

Do not require a live URL, a bootstrap step, a particular directory, or access to an existing workspace. If critical information is missing, ask a concise question or leave an explicit `TODO` and list what must be confirmed.

## Processing rules

### 1. Preserve the test intent

- Keep Test Case IDs, priorities, step order, and expected outcomes traceable to the supplied cases. Do not expand product scope.
- Map each specified expected outcome to a candidate assertion after the corresponding action. If there is no supplied evidence for an assertion or locator, mark it for validation instead of guessing.
- For conditional outcomes, keep the condition as setup and the side-effect verification separate from the action's unconditional exit condition.

### 2. Keep the Facade / Node / Spec boundaries

- **Facade:** owns locators and UI element methods. Group Facades by logical UI surface, not by screen position. Do not put business logic, navigation, or assertions in a Facade.
- **Node:** owns business logic, navigation, and assertions; follows the framework's state-in/state-out pattern and calls Facade methods rather than using direct locators. Do not put CSS selectors or direct Playwright locator calls in a Node. Avoid fixed sleeps such as `waitForTimeout()`.
- **Spec:** wires test paths, Nodes, and data bindings; it should not contain low-level implementation details.
- Keep each Node to one UI state transition. Split navigation from acting on the newly opened view when that intermediate state is independently useful or reusable.
- Group proposed Facade methods around supplied interactions. Match a supplied existing Facade or Node only when its provided description covers the need with greater than 90% confidence. If no inventory was supplied, label proposed components `existence unverified`; do not imply they exist in a repository.

### 3. Use only evidenced locators and assertions

- Never visit the application, inspect its DOM, run `playwright-cli`, or infer real selectors, accessible names, routes, or UI text.
- Use locator evidence only when it was explicitly supplied. Otherwise put a clear placeholder in the draft and list the evidence needed to replace it. When actual locator evidence is later available, prefer stable semantic locators (`getByRole`, `getByLabel`, `getByTestId`) over CSS; justify any CSS fallback.
- Derive assertion candidates from the test case's Expected Results. Identify the exact observable evidence that still needs confirmation; do not present an unverified assertion as a verified one.
- Use unique test data for values created and then looked up or deleted in the same case. Do not invent credentials, environment values, base URLs, or test fixtures.

### 4. Draft only; do not execute

- Present code inline as **Draft / Not Validated**. Use explicit `TODO` placeholders for unknown imports, project APIs, configuration, locators, and assertions.
- Do not write or copy files, install Playwright or other dependencies, run a compiler, execute tests, or access a live/local application.
- Never claim the draft compiled, ran, passed, or was validated. Keep proposed names and structure clearly distinct from existing project assets.

## Output requirements

Return the following sections in Markdown. Any directory tree is conceptual only and must not imply that the directory exists:

1. **Status:** `Draft / Not Validated`.
2. **Suggested Playwright Test Structure:** a conceptual Spec/Node/Facade layout and a mapping from Test Case IDs to proposed specs/nodes.
3. **Facade / Node Design:** each proposed component's responsibility, relevant interactions, and whether its existence is confirmed or unknown.
4. **Key Test Steps:** a concise mapping from the provided steps to Node calls and expected outcomes.
5. **Example Playwright Script Draft:** a clearly labeled code block with placeholders wherever project APIs or application details are unknown. Keep locator ownership in Facades, behavior/assertions in Nodes, and path wiring in Specs.
6. **Needs Real Application Validation:** a table of each unresolved locator and assertion, the related Test Case/step, and the concrete evidence needed to confirm it.

State assumptions and missing inputs explicitly. Do not claim compilation, execution, test success, or application validation.
