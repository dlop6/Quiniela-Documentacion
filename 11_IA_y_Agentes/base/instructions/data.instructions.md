---
scope: "data and schema tasks"
---

# Data Instructions

## Purpose

These instructions define the always-on guardrails for Quiniela data, schema, and invariant-sensitive work.

They exist to keep data changes aligned with documented entities, relationships, invariants, and traceability expectations.

## When It Applies

Use these instructions for:

- schema changes;
- data model review;
- migration planning;
- invariant-sensitive refactors;
- persistence-layer review;
- data access changes that can affect documented behavior.

## Primary Guardrails

- Treat documented invariants and entity boundaries as more important than implementation convenience.
- Do not introduce undocumented entities, states, or relationships.
- Consider auditability and traceability impact for any structural data change.
- Do not let storage convenience redefine domain behavior.
- Handle schema evolution cautiously and avoid undocumented assumptions about physical modeling.
- Prefer explicit consistency over premature optimization.

## Required Documentation

Read the minimum relevant document families from `dlop6/Quiniela-Documentacion`:

Canonical URL:

- `https://github.com/dlop6/Quiniela-Documentacion`

- `05_Datos`
- `03_Backend`
- `08_Testing`

Also read `01_Producto_y_Reglas` when the change affects official domain behavior such as picks, scoring, status transitions, or leaderboard semantics.

## Task-Specific Constraints

- If a data change touches state transitions, scoring, result authority, or audit events, verify the owning documents before proposing structure.
- Do not assume a physical schema policy that the docs do not define.
- When schema evolution is involved, account for rollback and consistency risk without inventing undocumented migration rules.
- Keep derived data, snapshots, and reproducible data concerns aligned with documented ownership.
- If a change risks weakening auditability or reproducibility, treat it as high-risk.

## Stop Conditions

Stop and report a blocker if:

- the affected invariant cannot be identified;
- the change would require an undocumented entity, field meaning, or state;
- the schema proposal changes domain behavior without a documented basis;
- the migration path depends on assumptions not supported by the docs.

## Non-Negotiable Rules

- Do not trade away documented invariants for implementation convenience.
- Do not invent undocumented entity relationships or state models.
- Do not ignore auditability or reproducibility impact.
- Do not let a schema shortcut redefine the documented domain.
