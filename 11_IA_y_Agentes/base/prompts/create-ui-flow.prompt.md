---
name: create-ui-flow
description: Task template for creating or reviewing a Quiniela frontend flow from documented visible behavior, feature ownership, and backend-aligned rules.
scope: "frontend flow tasks"
---

# Create UI Flow

## Purpose

Use this prompt to create, review, or refine a frontend flow only when the visible behavior can be tied to the official Quiniela documentation.

This prompt helps the model gather the right UI constraints before implementation. It does not turn the frontend into the source of truth for business rules.

## Use This Prompt When

Use this prompt when:

- the task is about a screen flow, feature interaction, form, state transition, or visible user journey;
- the expected behavior is documented or can be traced to documented feature behavior;
- the main problem is frontend flow design or implementation rather than deep architectural redesign;
- the task needs explicit alignment between visible states and backend behavior.

## Do Not Use This Prompt When

Do not use this prompt when:

- the visible behavior depends on unresolved product decisions;
- the task is mainly backend, data, or integration work;
- the main risk is scoring, picks, locks, or permission-sensitive behavior and specialist context is required first;
- the task is only a styling or microcopy pass with no flow implications;
- the work is a large multi-step implementation that should run through the implementation skill instead.

## Required Inputs

Before acting, gather:

- a short task summary;
- the feature or page being changed;
- the visible states involved;
- the relevant backend dependency or contract if known;
- any user role, activation, scoring, or permission assumptions that may affect the flow;
- any current screen or component context if the flow already exists.

## Documentation to Review First

Canonical documentation repository:

- `https://github.com/dlop6/Quiniela-Documentacion`

Read the minimum relevant document families first:

- `04_Frontend`
- `02_Arquitectura`
- `09_UI_UX`
- `08_Testing`

Also read:

- `01_Producto_y_Reglas` when the flow exposes or depends on official domain behavior such as activation, picks, special predictions, locks, results, leaderboard behavior, or role-sensitive actions.

## Prompt Instructions

1. Identify the affected feature and the visible flow being changed.
2. List the documentation families consulted before proposing UI behavior.
3. Describe the visible states the user can encounter:
   - entry state;
   - success states;
   - loading or pending states;
   - blocked or empty states;
   - error states.
4. State which behaviors come from backend authority and must not be redefined in UI.
5. Separate:
   - presentation concerns;
   - local UI state;
   - remote or server state;
   - rule-sensitive behavior that must stay aligned with the backend.
6. Identify hidden assumptions about permissions, activation, scoring, locks, or visibility and verify whether they are documented.
7. Call out testing and regression-sensitive visible behavior.
8. If any visible state or rule dependency is not documented, stop and report the blocker instead of inventing UI behavior.

## Expected Output

Always return:

1. `Summary`
2. `Docs consulted`
3. `Constraints applied`
4. `Risks`
5. `Open questions or blocked decisions`
6. `Validation or tests needed`
7. `Affected feature(s) or visible states`

If proposing implementation, also state whether any part of the flow depends on backend-confirmed behavior.

## Stop Conditions

Stop and report a blocker if:

- the owning feature or visible flow cannot be identified;
- the task would require undocumented screen states or transitions;
- backend authority for the visible behavior is unclear;
- the flow depends on unresolved rules for roles, activation, picks, scoring, or locks;
- the task really belongs to backend, scoring validation, or security review first.

## Non-Negotiable Rules

- Do not invent undocumented screen states or UI-only rule branches.
- Do not move official domain enforcement into the frontend.
- Do not assume the UI decides permissions, locks, scoring, or official results.
- Do not ignore visible-state consistency, backend contracts, or regression impact.
