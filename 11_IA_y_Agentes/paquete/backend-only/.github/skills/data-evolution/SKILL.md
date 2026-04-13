---
name: data-evolution
description: Reusable workflow for evaluating or planning Quiniela data and schema changes without violating documented invariants or semantics.
scope: "data evolution workflows"
---

# Data Evolution

## Purpose

Use this skill to evaluate or plan data and schema changes without violating documented invariants, entity boundaries, auditability expectations, or reproducibility concerns.

This skill exists to prevent schema work from silently redefining domain meaning.

## Use This Skill When

Use this skill when:

- schema evolution is proposed;
- a persistence change may affect semantics;
- a migration-sensitive change is under review;
- derived data, snapshots, or reproducibility concerns are involved;
- a data review must assess invariant safety.

## Do Not Use This Skill When

Do not use this skill when:

- the task is generic backend implementation with no meaningful data-model risk;
- the task is pure frontend work;
- the task is competitive-rule interpretation rather than data semantics;
- the required invariant or entity meaning is still undocumented.

## Required Inputs

Before using this skill, identify:

- the affected entities or relationships;
- the invariant or semantic concern involved;
- whether the task affects migration, rollback, derived data, or auditability;
- whether domain semantics would change if the proposal is accepted.

## Documentation to Review First

Canonical documentation repository:

- `https://github.com/dlop6/Quiniela-Documentacion`

Read first:

- `05_Datos`
- `03_Backend`
- `08_Testing`

Also read `01_Producto_y_Reglas` when the change affects official domain semantics such as scoring, picks, states, results, or leaderboard meaning.

## Workflow

1. Identify the affected entities, relationships, or invariants.
2. Read the data docs first, then the backend and testing docs.
3. Confirm whether the change is:
   - purely physical;
   - semantically meaningful;
   - migration-sensitive;
   - auditability-sensitive;
   - reproducibility-sensitive.
4. Map the proposal against documented invariants and entity boundaries.
5. Check whether rollback, migration, or derived data behavior depends on undocumented assumptions.
6. Report semantic impact, migration risk, and invariant safety.

## Quality Checks

- The proposal preserves documented invariants.
- Physical convenience does not override conceptual meaning.
- Auditability and reproducibility concerns are explicitly considered.
- Migration-sensitive changes are framed with rollback caution.
- The skill does not invent new entities, states, or relationships.

## Expected Output

Always include:

1. `Summary`
2. `Docs consulted`
3. `Constraints applied`
4. `Risks`
5. `Open questions or blocked decisions`
6. `Validation or tests needed`
7. `Invariant impact`

Also include:

- `Semantic impact`
- `Migration or rollback concerns`

## Stop Conditions

Stop and report a blocker if:

- the affected invariant cannot be identified;
- the proposal requires undocumented entities, relationships, or state semantics;
- rollback or migration reasoning depends on unsupported assumptions;
- the change alters domain meaning without explicit documentation support.

## Non-Negotiable Rules

- Do not invent undocumented entities, relationships, or state semantics.
- Do not trade documented invariants for convenience.
- Do not weaken auditability or reproducibility without explicit support.
- Do not let schema evolution redefine official domain meaning.
