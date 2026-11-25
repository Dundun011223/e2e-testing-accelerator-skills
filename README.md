# SBNC E2E Testing Accelerator Skills

A small collection of instruction-only skills for planning and supporting Playwright E2E work with the SBNC E2E Testing Accelerator.

## About

These skills help an AI assistant turn user-provided requirements and test information into plans, proposals, script drafts, execution guidance, and analysis of user-provided results. They are plain Markdown instructions, not executable tooling.

## Available Skills

| Skill | Description | Instructions |
|-------|-------------|--------------|
| `sbnc-e2e-project-setup` | Plan project structure, dependencies, configuration, and feature scaffolding. | [SKILL.md](skills/sbnc-e2e-project-setup/SKILL.md) |
| `sbnc-e2e-test-case-generator` | Create traceable test case proposals from requirements and acceptance criteria. | [SKILL.md](skills/sbnc-e2e-test-case-generator/SKILL.md) |
| `sbnc-e2e-playwright-script-designer` | Draft Playwright tests using the Facade, Node, and Spec architecture. | [SKILL.md](skills/sbnc-e2e-playwright-script-designer/SKILL.md) |
| `sbnc-e2e-local-test-guide` | Guide user-run local tests and analyze supplied results. | [SKILL.md](skills/sbnc-e2e-local-test-guide/SKILL.md) |
| `sbnc-e2e-ci-execution-planner` | Plan CI test execution, environment handling, and report archiving. | [SKILL.md](skills/sbnc-e2e-ci-execution-planner/SKILL.md) |

## Installation

Install all skills from the repository:

```bash
npx skills add Dundun011223/e2e-testing-accelerator-skills
```

Install only the test case generator:

```bash
npx skills add Dundun011223/e2e-testing-accelerator-skills --skill sbnc-e2e-test-case-generator
```

The public repository is owned by `Dundun011223`.

## Usage

Ask your AI assistant in natural language. For example:

- **Project setup:** “Plan a new Playwright E2E project for [application] with [constraints]. Do not create files.”
- **Test cases:** “Turn this PRD and its acceptance criteria into prioritized, traceable positive and negative E2E test cases: [requirements].”
- **Script design:** “Design an unvalidated Playwright draft for these test cases using Facade, Node, and Spec layers; use placeholders for unknown locators: [test cases].”
- **Local testing:** “Given this run command and these user-provided test results, suggest local execution steps and analyze the failures. Do not run the tests: [command and results].”
- **CI planning:** “Plan CI execution for [provider and test scope], including dependencies, secret variable names, and report archiving. Do not create or trigger a pipeline.”

## Limitations

These are instruction-only skills. They provide plans, proposals, unvalidated script drafts, execution guidance, and analysis based on information supplied by the user. They do not independently execute tests, inspect or modify workspaces, access applications or CI systems, or require an MCP connector. Users remain responsible for carrying out and validating any suggested actions. No SAP certification or endorsement is claimed.

## Author / Maintainer

**Public author / maintainer:** `Dundun011223`

## License

Apache License, Version 2.0. See [LICENSE](LICENSE).
