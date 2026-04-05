---
scope: "backend tasks"
---

# Backend Instructions

## Purpose

These instructions define the always-on backend guardrails for Quiniela implementation work.

They exist to keep backend changes aligned with the documented modular architecture, domain boundaries, validation discipline, auditability expectations, and backend enforcement rules.

## When It Applies

Use these instructions for:

- backend implementation;
- backend refactoring;
- backend review;
- API or endpoint changes;
- module scaffolding or restructuring;
- backend process or validation changes.

## Primary Guardrails

- Respect the documented modular monolith and ownership by module.
- Keep business logic out of controllers and other boundary-only components.
- Validate all external input at the boundary before domain execution.
- Treat backend as the final authority for permissions, locks, scoring, and official behavior.
- Do not invent undocumented modules, states, endpoints, or domain behaviors.
- Account for auditability, error handling, and idempotency when the task affects critical flows.

## Required Documentation

Read the minimum relevant document families from `dlop6/Quiniela-Documentacion`:

Canonical URL:

- `https://github.com/dlop6/Quiniela-Documentacion`

- `02_Arquitectura`
- `03_Backend`
- `05_Datos`
- `07_Seguridad_y_Operacion`
- `08_Testing`

Also read `01_Producto_y_Reglas` when the backend change touches domain rules such as activation, picks, locks, scoring, results, or leaderboard behavior.

## Task-Specific Constraints

- Keep ownership explicit when a task affects more than one backend module.
- Do not let infrastructure contracts redefine domain language or business behavior.
- Do not place validation, permissions, or scoring rules in transport-layer helpers as a hidden shortcut.
- Do not treat admin flows as a parallel source of business truth.
- If a change affects audit events, manual overrides, recalculation, or lock behavior, assume regression sensitivity and read the related docs first.

## Stop Conditions

Stop and report a blocker if:

- the responsible backend module cannot be identified;
- a change would require an undocumented endpoint or undocumented state transition;
- a backend behavior conflicts with documented domain or architecture rules;
- a critical backend flow depends on an open product or data decision;
- the implementation would require guessing validation, error, or authority boundaries.

## Non-Negotiable Rules

- Do not move official business rules into controllers.
- Do not bypass backend enforcement because the frontend already validates something.
- Do not introduce undocumented backend states, module ownership changes, or hidden workflow shortcuts.
- Do not ignore auditability, security, or regression impact on critical flows.
