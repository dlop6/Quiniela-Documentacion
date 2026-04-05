---
scope: "integration and external sync tasks"
---

# Integrations Instructions

## Purpose

These instructions define the always-on guardrails for Quiniela integration work, especially around external results synchronization, fallback, reconciliation, and provider adaptation.

They exist to keep external systems from becoming accidental sources of truth and to preserve traceability under failure or contingency.

## When It Applies

Use these instructions for:

- external results sync changes;
- provider contract mapping;
- fallback or override workflows;
- reconciliation logic;
- integration review;
- contingency-sensitive integration behavior.

## Primary Guardrails

- Treat all external data as untrusted until adapted to Quiniela domain expectations.
- Adapt provider contracts before domain logic consumes them.
- Respect documented fallback and manual override behavior.
- Preserve reconciliation traceability for effective changes.
- Do not let provider behavior redefine official game rules or authority boundaries.
- Treat integration failures and repeated sync events as regression-sensitive.

## Required Documentation

Read the minimum relevant document families from `dlop6/Quiniela-Documentacion`:

Canonical URL:

- `https://github.com/dlop6/Quiniela-Documentacion`

- `06_Integraciones`
- `07_Seguridad_y_Operacion`
- `08_Testing`

Also read `03_Backend` and `05_Datos` when the task affects persistence, backend module ownership, recalculation behavior, or auditability.

## Task-Specific Constraints

- Do not assume external values are immediately fit for domain use.
- Do not collapse sync behavior, fallback behavior, and manual override behavior into a single undocumented shortcut.
- When a task can affect result authority, recalculation, or contingency flow, verify the documented precedence and traceability expectations first.
- Treat repeated events, stale provider data, and manual correction paths as first-class concerns.
- Keep integration logic subordinate to documented domain ownership.

## Stop Conditions

Stop and report a blocker if:

- the task requires undocumented provider precedence or sync behavior;
- the change would let external data bypass domain adaptation;
- the integration proposal weakens fallback, override, or reconciliation traceability;
- the task depends on undocumented contingency handling.

## Non-Negotiable Rules

- Do not treat provider output as automatic business truth.
- Do not overwrite documented manual authority by assumption.
- Do not ignore reconciliation, traceability, or repeat-event risk.
- Do not invent undocumented sync, fallback, or override behavior.
