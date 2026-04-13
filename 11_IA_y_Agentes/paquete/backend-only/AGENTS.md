# AGENTS

## Purpose

This file is the routing and governance entrypoint for the Quiniela backend AI kit.
Its job is to help any LLM work correctly inside the backend implementation repository without copying the full Quiniela documentation into that repo.

This file does not redefine business rules, architecture, or product behavior.
It tells the model:

- where the source of truth lives;
- what to read first;
- which kit artifact to use for a task;
- when to stop instead of guessing;
- what output shape is expected.

## Scope

This file is intended for the backend implementation repository that uses the Quiniela AI kit.

It applies to AI-assisted:

- implementation;
- refactoring;
- review;
- testing;
- security analysis;
- operational readiness checks.

It does not replace the official Quiniela documentation repository and it must not be treated as a second source of truth.

## Source of Truth

The official source of truth is the documentation repository:

- `dlop6/Quiniela-Documentacion`
- `https://github.com/dlop6/Quiniela-Documentacion`

Operational hierarchy:

1. The official Quiniela documentation repository wins.
2. More recent sprint decisions win over older kit wording if both cannot be true at the same time.
3. This AI kit is an execution layer and must follow the docs repository plus closed sprint decisions.
4. Open decisions must block implementation. They must not be guessed or silently closed by the model.

If this kit conflicts with the documentation repository or with a newer sprint decision, follow the newer documented source and report the mismatch so the kit or docs can be synchronized.

## Required Read Order

Use this package in the following order:

1. Read this file first.
2. Read `.github/context/current-sprint-scope.md`.
3. Read the relevant thematic file in `.github/context/` or `.github/instructions/`.
4. Use the matching `agent`, `skill`, or `prompt` only after the above context is loaded.

Do not skip the sprint scope file.
The backend must not quietly implement behavior that belongs to a later sprint.

## Documentation Access

Preferred access path:

1. Read the official documentation repository through GitHub MCP.
2. Read only the minimum documents required for the current task.
3. Keep implementation repositories lightweight and do not copy the full documentation corpus into them.

Fallback access path when GitHub MCP is unavailable:

1. Use stable read-only links to the documentation repository.
2. Use an approved read-only mirror if one exists.
3. If no reliable documentation access exists, stop and report the blocker.

Documentation lookup rule:

- never read the entire documentation repository by default;
- read only the document families required by the task type;
- if the task expands in scope, expand the document lookup accordingly.

## How to Use This Kit

Use this kit in layers:

- `.github/instructions` for persistent guardrails;
- `.github/agents` for task specialization;
- `.github/skills` for step-by-step workflows;
- `.github/prompts` for reusable task templates;
- `.github/context` for current sprint and closed backend decisions.

Recommended order:

1. Read this file first.
2. Read `.github/context/current-sprint-scope.md`.
3. Identify the task type.
4. Load the relevant `.github/instructions`.
5. Load the matching `.github/context` file if the task needs project-specific closed decisions.
6. Use the matching `agent` if the task needs role-specific guidance.
7. Use the matching `skill` if the task is workflow-driven.
8. Use a `prompt` only as a task accelerator, never as a replacement for documentation.

## Task Routing Rules

Use the following routing rules as the default entrypoint:

| Task type | Required instructions | Recommended agent | Recommended skill | Typical prompt |
| --- | --- | --- | --- | --- |
| Backend implementation or refactor | `backend.instructions.md` | `backend-architect.agent.md` | `feature-implementation` | `create-endpoint.prompt.md` or `scaffold-module.prompt.md` |
| Scoring, picks, locks, tie-break logic | `scoring.instructions.md` | `picks-scoring-specialist.agent.md` | `scoring-validation` | `validate-scoring.prompt.md` |
| Results sync, fallback, override workflows | `integrations.instructions.md` | `results-integration-specialist.agent.md` | `integration-sync` | `review-integration-sync.prompt.md` |
| Data model, schema, invariants, migrations | `data.instructions.md` | `data-model-guardian.agent.md` | `data-evolution` | `scaffold-module.prompt.md` |
| Security-sensitive implementation or review | `security.instructions.md` | `security-auditor.agent.md` | `review-pr` | `harden-security.prompt.md` |
| Test design, coverage, regression checks | `testing.instructions.md` | `qa-test-engineer.agent.md` | `review-pr` | `generate-tests.prompt.md` |
| Release readiness, maintenance, governance | `security.instructions.md`, `testing.instructions.md` | `operations-maintainer.agent.md` | `release-governance` | `review-change.prompt.md` |

If a task spans multiple areas, load the relevant instruction families first and then prefer the narrowest specialist agent or skill that matches the highest-risk part of the task.

## Required Documentation by Task Type

Read the minimum relevant document families from `dlop6/Quiniela-Documentacion`.

| Task type | Required document families |
| --- | --- |
| Backend | `02_Arquitectura`, `03_Backend`, `05_Datos`, `07_Seguridad_y_Operacion`, `08_Testing` |
| Scoring or picks | `01_Producto_y_Reglas`, `03_Backend`, `05_Datos`, `08_Testing` |
| Results sync or fallback | `03_Backend`, `06_Integraciones`, `07_Seguridad_y_Operacion`, `08_Testing` |
| Data or schema | `03_Backend`, `05_Datos`, `08_Testing` |
| Security | `03_Backend`, `07_Seguridad_y_Operacion`, `08_Testing`, plus the affected area docs |
| Testing | `08_Testing`, plus the affected area docs |
| Release or governance | `07_Seguridad_y_Operacion`, `08_Testing`, `10_Planeacion`, and the latest sprint decisions |

Escalate document lookup only when the task expands or when a cross-domain dependency becomes relevant.

## Project-Specific Closed Constraints

Treat the following as already closed unless the documentation repository or a newer sprint decision explicitly changes them:

- the backend repository is separate from frontend;
- the backend is a modular monolith by domain;
- the approved backend stack is `Node.js`, `Express`, `TypeScript`, `Zod`, `Prisma`, `Helmet`, `Pino`, `OpenTelemetry`, and `Vitest`;
- the backend is the final authority for permissions, locks, scoring, manual override precedence, and auditability;
- auth is real in v1, with access JWT plus refresh token, and no social login in v1;
- activation is global and only `ACTIVO` users can play;
- `SIN_PICK` is a derived domain outcome, not a fabricated pick row;
- Sprint 3 is limited to `matches + picks base`, not full lock, final `SIN_PICK` UX, results, scoring, recalculation, or leaderboard.

Use `.github/context/backend-decisions-index.md` for the compact version of these rules.

## Stop Conditions

Stop and report a blocker if any of the following is true:

- a primary document cannot be identified;
- a critical document is unavailable;
- two relevant documents conflict in a way that changes implementation behavior;
- the task requires closing an open product, architecture, data, security, or testing decision;
- completing the task would require undocumented behavior;
- the model cannot identify the responsible module, feature, or workflow owner;
- the task would cross sprint boundaries in a way not already approved by the sprint scope file.

When stopping, report:

- what is blocked;
- which document or decision is missing;
- why implementation would require guessing.

## Expected Output Format

Unless the tool or environment requires something else, the model should return work in this structure:

1. `Summary`
2. `Docs consulted`
3. `Constraints applied`
4. `Risks`
5. `Open questions` or `Blocked decisions`
6. `Validation or tests needed`

For implementation tasks, also include:

- affected area or responsible module;
- whether the task stayed within documented behavior;
- whether any partial blocker was found.

## Non-Negotiable Rules

- Do not assume undocumented behavior.
- Do not restate or fork the Quiniela documentation repository locally unless explicitly required for a specific adapter artifact.
- Do not bypass backend enforcement for permissions, locks, scoring, auditability, or admin authority.
- Do not ignore testing, security, data invariants, or operational traceability impact.
- Do not treat prompts as a replacement for documentation.
- Do not continue implementation when an open decision materially changes behavior.
- Do not quietly implement future-sprint behavior because the current change appears adjacent to it.
