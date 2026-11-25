---
name: sbnc-e2e-local-test-guide
description: "Guidance for running Playwright E2E tests locally and analyzing user-provided test results. Covers execution command templates, prerequisites, failure classification, and troubleshooting. Use when preparing a local test run or investigating test failures. Does not execute tests or access the local workspace."
---

# SBNC E2E Local Test Guide

## Purpose and use

Use this Skill when a user wants guidance for running Accelerator Playwright tests locally or wants help interpreting test results. Work only from test cases, scripts, commands, logs, and artifacts the user supplies. Do not access the user's workspace or execute tests.

## Inputs

Useful inputs include:

- The test case ID, feature/spec scope, or test script.
- The project's Playwright runner and any user-provided run command.
- The user's intended mode: one test, one spec/feature, or the full suite; headed or headless.
- For failure analysis, the relevant error/report entry and any supplied `run-context.json`, screenshot description, or trace findings.

If a path, command, project convention, or result is missing, ask for it or use an explicit placeholder. Never assume a particular workspace, URL, environment, or that a test has run.

## Local execution guidance

Before suggesting a command, confirm that the supplied project uses Playwright Test and that the requested scope is clear. You may provide command templates for the user to run in their own environment; label them **User-run only — not executed by this Skill** and replace unknown paths/IDs with placeholders:

```text
# One test case
npx playwright test <spec-path> --grep "<test-case-id>" --headed --trace on --reporter=list

# One spec / feature
npx playwright test <spec-path> --headed --trace on --reporter=list

# Full suite
npx playwright test --headed --trace on --reporter=list
```

Use headless mode only when the user requests it; for these templates, that means omitting `--headed`. The user should confirm the actual spec path and project configuration. Do not run these commands, install packages or browsers, or inspect local files. If the project does not use the standard Playwright Test CLI, ask the user to provide its documented command instead of guessing.

When useful, remind the user to check their own prerequisites: Node.js version (the Accelerator template declares `>=18`), dependencies and browser availability, app URL/environment configuration, and test data. Do not ask them to paste secrets.

## Failure analysis rules

Analyze only evidence included by the user. If an artifact is mentioned but its relevant contents were not supplied, ask for the relevant excerpt; do not claim to have opened a local file or trace archive.

Use these categories as evidence-based hypotheses, not definitive diagnoses:

| Category | Typical supplied evidence | Guidance |
|---|---|---|
| **A — Facade / locator drift** | `locator.waitFor` timeout, element not found, or strict-mode violation; supplied trace evidence shows a changed locator target | Identify the affected Facade method and the specific evidence. Suggest a locator update only when the supplied DOM/trace unambiguously supports a mechanical change. |
| **B — Node logic** | `expect()` mismatch, `waitForURL` timeout, `TypeError`, or null dereference | Identify the failed step/assertion and likely Node responsibility. Recommend review; do not rewrite Node logic. |
| **C — Application behavior** | HTTP 4xx/5xx, unexpected redirect, or a feature outcome that appears broken independently of the test | Summarize the reproduction evidence and recommend application investigation/escalation. |

For triage, first use supplied report details and static evidence: failed case/step/node, error text, URLs, console errors, failed network requests, and the failure screenshot. If that does not resolve the cause, use only trace details the user provides, such as the failing call, DOM snapshot, or network event. Treat `status >= 400` network events as possible application evidence and console errors as supporting evidence, not proof by themselves.

Do not modify Facade, Node, Spec, or configuration files. Do not recommend a code fix without concrete supporting evidence. Stop and report when the element is absent, multiple matches are ambiguous, a state precondition may have failed, or a proposed change would alter Node business logic, navigation, or data flow. If more evidence is required, ask the user to provide it or arrange a separate, explicitly approved inspection; this Skill never accesses the application.

## Result status and output

- If no actual result/log was supplied, start with **Tests Not Run**. You may still provide a user-run command template or review a proposed script.
- If the user supplied results, start with **Analysis of User-Provided Results / Not Independently Executed**. Attribute counts and pass/fail claims to the supplied report (for example, “The provided report records…”).
- Never imply that this Skill executed or independently verified a test.

For a result analysis, report only fields supported by the supplied evidence: feature/spec, mode, passed/failed/skipped counts, and for each failure the category, evidence, analysis, and recommended next step. Call out missing evidence and uncertainty. Do not invent counts, file paths, or line numbers.
