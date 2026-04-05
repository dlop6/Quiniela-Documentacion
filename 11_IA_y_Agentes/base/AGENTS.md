# AGENTS

## Purpose

This file is the routing and governance entrypoint for the Quiniela AI kit.
Its job is to help any LLM work correctly inside implementation repositories without copying the full Quiniela documentation into those repos.

This file does not redefine business rules, architecture, or product behavior.
It tells the model:

- where the source of truth lives;
- how to access it;
- which kit artifact to use for a task;
- when to stop instead of guessing;
- what output shape is expected.

## Scope

This file is intended for backend and frontend implementation repositories that use the Quiniela AI kit.

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
2. This AI kit is an execution layer and must follow the docs repository.
3. Open decisions must block implementation. They must not be guessed or silently closed by the model.

If this kit conflicts with the documentation repository, follow the documentation repository and report the mismatch.

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

- `instructions` for persistent guardrails;
- `agents` for task specialization;
- `skills` for step-by-step workflows;
- `prompts` for reusable task templates.

Recommended order:

1. Read this file first.
2. Identify the task type.
3. Load the relevant `instructions`.
4. Use the matching `agent` if the task needs role-specific guidance.
5. Use the matching `skill` if the task is workflow-driven.
6. Use a `prompt` only as a task accelerator, never as a replacement for documentation.

## When to Use Instructions

Use `instructions` when you need persistent, always-on guardrails for a class of work.

Instructions should:

- stay lightweight;
- define non-negotiable constraints;
- point to the documentation families that matter;
- avoid restating the full domain.

Use instructions first for:

- backend work;
- frontend work;
- security-sensitive work;
- testing work;
- data or schema work;
- integrations work;
- scoring or picks work.

## When to Use Agents

Use `agents` when the task needs role-based specialization.

Agents should define:

- when they apply;
- when they do not apply;
- what documentation must be reviewed;
- what output the model must produce;
- when the model must stop.

Use an agent when the task clearly belongs to a specialist area such as:

- backend architecture;
- frontend UI and feature composition;
- testing strategy or test design;
- security review;
- data modeling;
- scoring and picks validation;
- result synchronization and fallback;
- maintenance and release governance.

## When to Use Skills

Use `skills` when the task requires a reusable workflow rather than a role.

Skills are the most portable layer across tools and should be preferred for multi-step work such as:

- feature implementation;
- bug fixing;
- PR review;
- scoring validation;
- integration sync validation;
- data evolution;
- release governance.

Use a skill when the model needs:

- a repeatable sequence of steps;
- explicit checkpoints;
- a defined output shape;
- a clear stopping rule.

## When to Use Prompts

Use `prompts` when you need a reusable task template for a specific action.

Prompts should:

- accelerate execution;
- stay tied to documented behavior;
- require documentation lookup before implementation;
- never become the only source of guidance for a risky task.

Typical uses:

- create an endpoint;
- create a UI flow;
- generate tests;
- review a change;
- scaffold a module;
- validate scoring behavior;
- review results synchronization.

## Task Routing Rules

Use the following routing rules as the default entrypoint:

| Task type | Required instructions | Recommended agent | Recommended skill | Typical prompt |
| --- | --- | --- | --- | --- |
| Backend implementation or refactor | `backend.instructions.md` | `backend-architect.agent.md` | `feature-implementation` | `create-endpoint.prompt.md` or `scaffold-module.prompt.md` |
| Frontend implementation or refactor | `frontend.instructions.md` | `frontend-ui-builder.agent.md` | `feature-implementation` | `create-ui-flow.prompt.md` |
| Scoring, picks, locks, tie-break logic | `scoring.instructions.md` | `picks-scoring-specialist.agent.md` | `scoring-validation` | `validate-scoring.prompt.md` |
| Results sync, fallback, override workflows | `integrations.instructions.md` | `results-integration-specialist.agent.md` | `integration-sync` | `review-integration-sync.prompt.md` |
| Data model, schema, invariants, migrations | `data.instructions.md` | `data-model-guardian.agent.md` | `data-evolution` | `scaffold-module.prompt.md` or a data-specific prompt |
| Security-sensitive implementation or review | `security.instructions.md` | `security-auditor.agent.md` | `review-pr` or a security-specific workflow | `harden-security.prompt.md` |
| Test design, coverage, regression checks | `testing.instructions.md` | `qa-test-engineer.agent.md` | `review-pr` or test workflow | `generate-tests.prompt.md` |
| Release readiness, maintenance, governance | `security.instructions.md`, `testing.instructions.md` | `operations-maintainer.agent.md` | `release-governance` | `review-change.prompt.md` |

If a task spans multiple areas, load the relevant instruction families first and then prefer the narrowest specialist agent or skill that matches the highest-risk part of the task.

## Required Documentation by Task Type

Read the minimum relevant document families from `dlop6/Quiniela-Documentacion`.

| Task type | Required document families |
| --- | --- |
| Backend | `02_Arquitectura`, `03_Backend`, `05_Datos`, `07_Seguridad_y_Operacion`, `08_Testing` |
| Frontend | `02_Arquitectura`, `04_Frontend`, `09_UI_UX`, `08_Testing` |
| Scoring or picks | `01_Producto_y_Reglas`, `05_Datos`, `08_Testing` |
| Results sync or fallback | `06_Integraciones`, `07_Seguridad_y_Operacion`, `08_Testing` |
| Data or schema | `05_Datos`, `03_Backend`, `08_Testing` |
| Security | `07_Seguridad_y_Operacion`, `08_Testing`, plus the affected area docs |
| Testing | `08_Testing`, plus the affected area docs |
| Release or governance | `07_Seguridad_y_Operacion`, `08_Testing`, `10_Planeacion` |

Escalate document lookup only when the task expands or when a cross-domain dependency becomes relevant.

## Stop Conditions

Stop and report a blocker if any of the following is true:

- a primary document cannot be identified;
- a critical document is unavailable;
- two relevant documents conflict in a way that changes implementation behavior;
- the task requires closing an open product, architecture, data, security, or testing decision;
- completing the task would require undocumented behavior;
- the model cannot identify the responsible module, feature, or workflow owner.

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
- Do not move official business rules into UI for convenience.
- Do not bypass backend enforcement for permissions, locks, scoring, or auditability.
- Do not ignore testing, security, data invariants, or operational traceability impact.
- Do not treat prompts as a replacement for documentation.
- Do not continue implementation when an open decision materially changes behavior.

## Open Questions

- Confirm whether every target implementation repository will always have GitHub MCP access to `dlop6/Quiniela-Documentacion`.
- Confirm whether adapter-specific files will be generated from this base layer or maintained manually.
