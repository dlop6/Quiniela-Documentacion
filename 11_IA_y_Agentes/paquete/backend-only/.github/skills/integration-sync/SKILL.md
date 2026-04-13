---
name: integration-sync
description: Reusable workflow for validating or designing Quiniela external result synchronization, fallback, override, and reconciliation behavior.
scope: "integration sync workflows"
---

# Integration Sync

## Purpose

Use this skill to validate or design result synchronization workflows that involve external providers, fallback behavior, manual overrides, reconciliation, and contingency-sensitive authority handling.

This skill exists to make sure provider behavior never becomes undocumented business truth.

## Use This Skill When

Use this skill when:

- the task affects external result ingestion;
- provider mapping or adaptation is changing;
- fallback or override behavior is being reviewed;
- reconciliation logic or contingency flow is involved;
- repeated or stale external events are relevant.

## Do Not Use This Skill When

Do not use this skill when:

- the task is only competitive-rule interpretation with no provider or sync behavior;
- the task is generic backend work with no integration risk;
- the task is pure schema evolution unrelated to result authority;
- the required authority or fallback behavior is still undocumented.

## Required Inputs

Before using this skill, identify:

- the provider-facing behavior under review;
- the documented authority path for official results;
- whether fallback, manual override, or reconciliation is affected;
- whether the task changes repeat-event handling or contingency behavior.

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

Also read `01_Producto_y_Reglas` when synced results can affect official competitive outcomes.

## Workflow

1. Identify the exact sync or authority concern.
2. Read the integration and operational docs first.
3. Confirm the documented authority path:
   - provider input;
   - domain adaptation;
   - fallback or manual override;
   - reconciliation;
   - downstream recalculation or state effect.
4. Check whether the task changes:
   - provider trust assumptions;
   - fallback behavior;
   - override precedence;
   - reconciliation traceability;
   - repeated or stale event handling;
   - contingency behavior.
5. Reject any design that lets provider output skip documented authority boundaries.
6. Report authority, fallback, reconciliation, and validation impact.

## Quality Checks

- Provider contracts are adapted before domain use.
- Fallback and override behavior remain documented and traceable.
- Repeated events do not silently redefine official state.
- Reconciliation logic stays explainable.
- Contingency-sensitive behavior is treated as first-class risk.

## Expected Output

Always include:

1. `Summary`
2. `Docs consulted`
3. `Constraints applied`
4. `Risks`
5. `Open questions or blocked decisions`
6. `Validation or tests needed`
7. `Authority and fallback impact`

Also include:

- `Authority path reviewed`
- `Reconciliation concerns`

## Stop Conditions

Stop and report a blocker if:

- authority or fallback behavior is undocumented;
- the proposal lets provider data bypass documented domain adaptation;
- contingency behavior required by the task is missing or contradictory;
- reconciliation semantics cannot be explained from the docs.

## Non-Negotiable Rules

- Do not treat provider output as automatic business truth.
- Do not invent undocumented precedence or override behavior.
- Do not weaken reconciliation traceability.
- Do not ignore stale-event, repeated-event, or contingency risk.
