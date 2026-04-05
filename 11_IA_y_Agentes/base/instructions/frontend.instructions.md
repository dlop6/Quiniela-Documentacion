---
scope: "frontend tasks"
---

# Frontend Instructions

## Purpose

These instructions define the always-on frontend guardrails for Quiniela implementation work.

They exist to keep frontend changes aligned with the documented feature organization, visible-state behavior, backend contract boundaries, and UI responsibilities.

## When It Applies

Use these instructions for:

- frontend implementation;
- frontend refactoring;
- screen or flow changes;
- component composition;
- frontend review;
- state handling or data-fetching changes.

## Primary Guardrails

- Organize frontend work by feature and keep shared code genuinely neutral.
- Separate UI concerns from domain enforcement and server-state concerns.
- Do not implement official business rules in the UI as the source of truth.
- Respect backend contracts and treat backend responses as the final authority for protected behavior.
- Keep visible states consistent with documented product and UX behavior.
- Do not invent undocumented screens, states, permissions, or flow outcomes.

## Required Documentation

Read the minimum relevant document families from `dlop6/Quiniela-Documentacion`:

Canonical URL:

- `https://github.com/dlop6/Quiniela-Documentacion`

- `02_Arquitectura`
- `04_Frontend`
- `09_UI_UX`
- `08_Testing`

Also read `01_Producto_y_Reglas` when the flow touches activation, picks, special predictions, locks, scoring visibility, permissions, or leaderboard behavior.

## Task-Specific Constraints

- Do not move domain decisions into components, hooks, or client-side state managers.
- Use UI validation to improve usability, but never treat it as official enforcement.
- Keep role- and state-sensitive UI aligned with the documented backend authority model.
- When a flow touches scoring, activation, picks, roles, or result corrections, verify both product rules and frontend behavior documents before changing UI.
- Do not let local convenience override documented visible-state consistency.

## Stop Conditions

Stop and report a blocker if:

- the task requires an undocumented screen state or flow branch;
- a UI behavior would need to assume backend behavior that is not documented;
- the change depends on an unresolved product or UX decision;
- the frontend contract required by the change cannot be tied to a documented backend capability;
- the implementation would force official rule enforcement into the client.

## Non-Negotiable Rules

- Do not treat UI as the source of truth for permissions, locks, scoring, or official states.
- Do not expand `shared` with feature-specific domain logic.
- Do not invent undocumented visible states, success paths, or fallback behavior.
- Do not ignore testing impact for critical user journeys.
