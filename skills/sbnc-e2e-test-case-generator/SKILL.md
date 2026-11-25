---
name: sbnc-e2e-test-case-generator
description: "Generate structured, traceable E2E test case proposals from product requirements, PRDs, feature descriptions, user stories, and acceptance criteria. Covers positive and negative scenarios, priorities, preconditions, test steps, and expected results. Use when translating business requirements into reviewable test coverage."
---

# SBNC E2E Test Case Generator

## Purpose and use

Use this Skill when a user provides a PRD, feature description, Jira story, or acceptance criteria and wants test cases. This is an instruction-only Skill: work only from information supplied in the conversation. Do not require a bootstrap step, local workspace, manifest, node index, or external tool.

## Inputs

- **Required:** requirement text, such as a PRD, Feature Description, user story, or Acceptance Criteria.
- **Optional:** requirement IDs, the project's priority convention, existing test cases, known reusable node/action IDs with descriptions, and constraints about manual or external steps.

If a missing detail prevents a meaningful expected result, ask a concise clarifying question. Otherwise proceed and label the assumption or unresolved point; do not invent product behavior.

## Processing rules

### 1. Trace requirements to coverage

- Extract actors, preconditions, triggers, data constraints, actions, state changes, observable outcomes, and stated error or conditional behavior.
- Cover each explicit Acceptance Criterion with one or more cases. Call out uncovered, ambiguous, or conflicting requirements rather than silently resolving them.
- Keep the scope anchored to the supplied requirements; do not add speculative features or business rules.

### 2. Design positive and negative scenarios

- Include positive scenarios for the supported primary flow and other explicitly required outcomes.
- Include negative scenarios for stated invalid input, validation, permission, failure, or rejection behavior. Where a requirement does not define the negative outcome, identify it as an open question instead of making one up.
- When an outcome depends on a condition (for example, a setting or permission), represent the condition as a precondition and verify each required branch separately. Keep the action's always-true completion result separate from conditional side effects.
- Preserve manual and external work (such as email, SMS, or a file download) in the case and mark it as manual/external; do not silently omit it or imply it is automated.

### 3. Make steps and expected results testable

- Write ordered, concrete user actions and observable expected results. Include setup/preconditions where needed.
- Keep each action focused on one UI state transition. If an action navigates to a new view and then operates on it, split those into separate steps when the intermediate state may be reused independently.
- Give navigation, dialog/state transitions, and the action's final postcondition meaningful observable results. For a simple fill/type step with no independent outcome, use `No independent assertion` rather than inventing an assertion.
- Treat a conditional side effect as a separate verification step or case, not as an unconditional result of the action.

### 4. Reuse actions and use safe test data

- Reuse an existing node/action only when the user supplied its ID and description and it matches the intended action with greater than 90% confidence. Otherwise label it `Candidate action — existence unverified`; never claim a repository node exists based on inference.
- Describe new actions in plain language. Do not generate facade names, selectors, or DOM assumptions for test-case planning.
- Use unique per-test data for values created and then looked up, matched, or deleted in the same case. Do not treat email addresses, numeric references, credentials, or environment-bound values as generated unique text; use supplied fixtures or mark the value as needing confirmation.

### 5. Assign a proposed priority

- Follow a priority convention supplied by the user.
- If none is supplied, use `High`, `Medium`, or `Low` as a proposed priority based only on the stated business impact, and mark inferred priorities as proposed. If impact cannot be inferred, use `Unassigned` and say why.

## Output requirements

Return Markdown in the conversation; do not write files or produce a framework manifest. Start with:

> **Status: Proposed / Not Executed**

For every test case include:

- **Test Case ID:** sequential IDs such as `TC-001`.
- **Test Case Title:** concise and scenario-specific.
- **Priority:** user-defined or proposed as described above.
- **Scenario:** `Positive` or `Negative`.
- **Requirement / Acceptance Criteria:** source IDs or a short traceability reference, when available.
- **Preconditions / Test Data:** only what is needed and supported by the input.
- **Test Steps and Expected Results:** an ordered table with `Step`, `Action`, and `Expected Result`; pair each meaningful action with its observable result.

Finish with a short coverage summary and any assumptions, open questions, or candidate reusable actions. Keep all test cases marked **Proposed / Not Executed**. This Skill does not run a validator: do not claim Pydantic Schema Validation Passed or any other schema-validation or execution result.
