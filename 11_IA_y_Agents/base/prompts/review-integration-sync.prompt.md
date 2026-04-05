---
name: review-integration-sync
description: Task template for reviewing or designing Quiniela external result synchronization, fallback, override, and reconciliation behavior.
scope: "integration review tasks"
---

# Review Integration Sync

## Purpose

Use this prompt to review or design Quiniela external result synchronization and related authority behavior from documented sources only.

This prompt is for structured integration review, not for inventing provider authority or undocumented fallback policy.

## Use This Prompt When

Use this prompt when:

- a task touches external results sync;
- provider mapping or adaptation is involved;
- fallback or manual override behavior is under review;
- reconciliation or contingency behavior matters.

## Do Not Use This Prompt When

Do not use this prompt when:

- the task is generic backend work with no integration risk;
- the task is competitive-rule interpretation without provider authority concerns;
- the required authority or fallback behavior is not documented.

## Required Inputs

Before acting, identify:

- the provider-facing behavior involved;
- the authority path for official results;
- whether fallback, override, reconciliation, or contingency behavior is affected;
- whether repeat-event or stale-event handling matters.

## Documentation to Review First

Canonical documentation repository:

- `https://github.com/dlop6/Quiniela-Documentacion`

Read first:

- `06_Integraciones`
- `07_Seguridad_y_Operacion`
- `08_Testing`

Then read:

- `03_Backend`
- `05_Datos`

## Prompt Instructions

1. Identify the exact integration or authority concern.
2. Read the integration and operational docs first.
3. Describe the documented authority path:
   - provider input;
   - domain adaptation;
   - fallback or manual override;
   - reconciliation;
   - downstream effect.
4. Check whether the change introduces undocumented provider authority, precedence, or contingency behavior.
5. Report authority impact, fallback impact, and validation needs.

## Expected Output

Always include:

1. `Summary`
2. `Docs consulted`
3. `Constraints applied`
4. `Risks`
5. `Open questions or blocked decisions`
6. `Validation or tests needed`
7. `Authority and fallback impact`

## Stop Conditions

Stop and report a blocker if:

- authority or fallback behavior is undocumented;
- provider data would bypass documented domain adaptation;
- reconciliation or contingency behavior cannot be explained from the docs;
- the review would require inventing precedence rules.

## Non-Negotiable Rules

- Do not treat provider output as automatic business truth.
- Do not invent undocumented fallback, override, or precedence behavior.
- Do not weaken reconciliation traceability.
- Do not ignore contingency risk in sync-sensitive flows.
