---
name: sbnc-e2e-project-setup
description: "Guidance for initializing E2E Testing Accelerator projects, including workspace structure, dependencies, configuration, and feature scaffolding. Use when starting a new Playwright E2E project or planning additions to an existing one. Provides an initialization plan without modifying files."
---

# SBNC E2E Project Setup

## Purpose and use

Use this Skill when a user wants to start an E2E Testing Accelerator project or add a feature scaffold to an existing project. Produce a plan from information supplied in the conversation only. This Skill does not inspect or modify a workspace and does not execute setup or verification commands.

## Inputs

Ask for only the information needed to form the plan:

- Application name.
- Project mode: `Standalone` or `Integrated` in an existing repository. For `Integrated`, the user may provide the relative target path.
- Whether the project is fresh or already has an Accelerator workspace. Do not infer this from the filesystem.
- Feature group and feature name to plan, if a feature scaffold is requested.
- Relevant constraints, such as existing package manager, Node.js version, or project conventions.

If details are missing, ask concise follow-up questions. Treat existing groups/features and workspace contents as unknown unless the user supplies them.

## Planning rules

### 1. Validate identifiers and scope

- Application name, feature group, and feature name must use kebab-case matching `^[a-z0-9]+(-[a-z0-9]+)*$`.
- A feature name must be unique within its feature group. If the user has not provided the existing names, mark uniqueness as needing confirmation.
- For an integrated project, show the supplied relative target path; do not assume a default workspace path.

### 2. Preserve the E2E Testing Accelerator structure

Recommend a structure consistent with its Node–Facade–DAG architecture. Present it as a plan, not as an assertion that these paths already exist:

```text
<workspace>/
├── config/
│   ├── app.config.yaml
│   └── environments.yaml
├── features/
│   └── <featureGroup>/<featureName>/
│       ├── manifest.yaml
│       ├── README.md
│       └── <featureName>.spec.ts       # generated from the manifest
├── shared/
│   ├── facades/<FacadeName>/            # implementation.ts + facade.yaml
│   ├── nodes/<nodeId>/                  # implementation.ts + node.yaml + optional node-tests.ts
│   ├── utils/
│   └── index.yaml                       # lightweight index: id + description
├── snapshots/
├── tools/
├── reporters/
├── package.json
├── playwright.config.ts
└── tsconfig.json
```

Keep executable Facade and Node code under `shared/`. Feature directories contain definitions, generated specs, and documentation; spec files are generated from manifests rather than hand-written. Each Facade is a pair (`implementation.ts`, `facade.yaml`); each Node has `implementation.ts`, `node.yaml`, and optionally `node-tests.ts`. The index is for discovery only; full contracts belong in their individual YAML files.

For a fresh project, plan the root configuration and shared structure before the feature scaffold. For an existing project, plan only the requested additions and identify compatibility checks. The source bootstrap refreshes bundled tools and overwrites `shared/utils/playwright.ts` and `playwright.config.ts`; call this out as a review/diff risk before the user applies any update, especially where customizations exist.

### 3. List the template dependency baseline

Use the versions declared by the bundled bootstrap template as a reference, not as a claim that they are installed or compatible with the user's project:

| Group | Package | Template version |
|---|---|---|
| Development | `@playwright/test` | `^1.48.0` |
| Development | `@types/node` | `^22.10.2` |
| Development | `tsx` | `^4.19.2` |
| Development | `typescript` | `^5.7.2` |
| Runtime | `@playwright/cli` | `^0.1.8` |
| Runtime | `yaml` | `^2.8.3` |

The template declares Node.js `>=18.0.0`. Flag this baseline for confirmation against the user's existing runtime and dependency policy. Do not install packages, download browser binaries, or claim compatibility was tested.

### 4. Describe configuration without secrets

When useful, show a small illustrative `app.config.yaml` shape using the supplied application name, `framework: playwright`, and a timestamp placeholder. For `environments.yaml`, describe the purpose (environment URLs and credentials) but never request or reproduce passwords, tokens, or other secrets in the plan. Use placeholders only.

## Output requirements

Return a Markdown plan in the conversation. Start with:

> **Initialization Plan / Not Executed**

Include:

1. The user-provided inputs, mode, and any assumptions or items needing confirmation.
2. Recommended initialization phases for a fresh or existing project, as applicable.
3. The conceptual directory tree and the Facade / Node / Spec responsibilities.
4. The dependency baseline and Node.js requirement above, clearly marked as template recommendations.
5. Risks or compatibility checks, including existing feature-name conflicts and files the source bootstrap overwrites.

Do not create directories or files, copy templates, run `setup.sh`, install dependencies, install Playwright browsers, run TypeScript checks, or claim initialization succeeded. Do not say files were inspected unless the user supplied their contents.
