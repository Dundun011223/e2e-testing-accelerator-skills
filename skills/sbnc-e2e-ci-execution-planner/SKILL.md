---
name: sbnc-e2e-ci-execution-planner
description: "Guidance for integrating Playwright E2E tests into CI pipelines. Covers execution stages, dependencies, environment configuration, secret handling, and test report archiving. Use when planning remote or CI-based test execution. Provides a pipeline design without creating pipelines or triggering remote tests."
---

# SBNC E2E CI Execution Planner

## Purpose and use

Use this Skill to recommend how an existing Playwright E2E project could run in CI. Keep the design pipeline-agnostic unless the user names a CI provider. The host team owns its pipeline configuration; this Skill provides a plan, not an implementation.

## Inputs

Use only information supplied by the user. Helpful inputs include:

- Test project structure, Playwright runner, test/spec scope, and documented project command.
- Node.js and package-manager versions, lockfile availability, browser requirements, and existing test/report dependencies.
- CI provider and whether this is a standalone test stage or an additional stage in a host pipeline.
- Names or paths of required environment values (for example, `buyer.password` or `base_url`)—never the secret values.
- Existing report format, artifact policy, and whether another suite already produces Allure results.

If details are missing, provide a vendor-neutral proposal and list assumptions or confirmation items. Do not inspect a repository or CI system.

## Planning rules

### 1. Propose a clear pipeline sequence

Describe the stages appropriate to the supplied project. A typical plan is:

1. Check out the test project and select the requested feature/spec scope.
2. Set up the project's supported Node.js version (the Accelerator bootstrap template declares `>=18.0.0`).
3. Install locked dependencies from the committed lockfile (the Accelerator template uses `npm ci`).
4. Provide the required Playwright browser and operating-system dependencies, preferably from a compatible runner image or cache; otherwise include a browser-install stage.
5. Inject required environment variables from the CI provider's secret store and fail early when required values are missing.
6. Run the project's documented Playwright test command for the requested scope.
7. Archive test results even when the test stage fails; optionally publish or merge a human-readable report.

Treat this as a design sequence, not an executed pipeline or a claim that the project supports every step.

### 2. Identify dependencies without installing them

- Prefer the versions already declared in the project's manifest and lockfile. Do not prescribe upgrades without evidence.
- The Accelerator template baseline includes Node.js `>=18.0.0`, `@playwright/test`, TypeScript tooling, and the Playwright browser required by the configured projects. The bootstrap template also declares `yaml` and `@playwright/cli`; include them only as project dependencies where the existing framework uses them.
- `allure-playwright` is needed only if the project config uses the Allure reporter. An Allure CLI or report-generation stage is optional and should be listed only if the host wants a generated/merged HTML report.
- Clearly separate required project dependencies from optional report or runner-image components. Do not install anything or claim dependency compatibility was tested.

### 3. Design environment-variable and secret handling

- Never ask for, display, or embed secret values. Use variable names and descriptions only.
- Keep credentials out of committed `config/environments.yaml` and pipeline files. Recommend storing secrets in the CI provider's protected secret store and injecting them at runtime.
- Use fail-fast checks for required variables. When mapping framework dot-paths to environment names, use uppercase names with dots/dashes converted to underscores (for example, `buyer.password` → `E2E_BUYER_PASSWORD`; `base_url` → `E2E_BASE_URL`). Mark inferred mappings as proposed.
- If the application needs an environment-config override such as `E2E_ENVIRONMENTS_FILE` or `E2E_ACTIVE_ENV`, state that the project must already support it or the owner must implement and validate that support. Do not claim it is configured.
- If the project must materialize environment configuration at runtime, recommend an ephemeral, access-restricted file with cleanup; never propose committing generated secret-bearing configuration.

### 4. Define reports and artifacts

- For the Accelerator pattern, recommend `reports/allure-results/` as the machine-readable Allure artifact path when the project is configured to produce it.
- Archive results with an unconditional/always-run artifact step so failures still retain diagnostics. Include retention and access restrictions according to the user's policy.
- If a host pipeline already produces Allure results, keep suites in separate subdirectories (for example, `wdio/` and `playwright/`) and merge them only if the host's Allure tooling supports it.
- Do not promise that a report exists or that any tests ran.

### 5. Keep ownership and scope explicit

- The host team owns provider-specific pipeline files and secret configuration. If a provider is named, describe where the proposed stages, secret bindings, and artifact upload would fit, but do not generate or modify a complete pipeline file.
- A conceptual single test-run step and stable report-artifact path can make integration portable; do not require a generated `ci/` bundle or a particular entry-point script.
- Never access CI credentials, create pipelines, trigger jobs, run remote tests, or claim a remote run succeeded.

## Output requirements

Return a Markdown design. Start with:

> **CI Execution Plan / Not Executed**

Include:

1. A stage table with purpose, required inputs/dependencies, and success/failure handling.
2. A dependency list separating project requirements from optional browser/reporting components.
3. An environment-variable contract table with variable name, purpose, source (CI secret/config), and whether the mapping is confirmed or proposed. Never include values.
4. A report and artifact-archive plan, including collection on test failure and any optional Allure merge.
5. Provider-specific integration notes only when the user supplied a CI provider, plus assumptions and items the project owner must verify.

Do not access a CI system, use credentials, create pipeline configuration, trigger or execute a remote test, or report an execution result. Keep the plan marked **CI Execution Plan / Not Executed**.
