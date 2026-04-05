---
name: create-ui-flow
description: Task template for creating or reviewing a Quiniela frontend flow from documented visible behavior only.
scope: "frontend flow tasks"
---

# Create UI Flow

## Purpose

Use this prompt to create or review a frontend flow only from documented Quiniela visible behavior.

This prompt is a task accelerator. It does not replace the official documentation, frontend instructions, or the frontend agent.

## Use This Prompt When

Use this prompt when:

- the task is to create or review a user-facing flow;
- the visible behavior is already documented;
- the affected feature or screen area can be identified;
- the flow depends on backend contracts or visible states that already exist conceptually.

## Do Not Use This Prompt When

Do not use this prompt when:

- the visible behavior is undocumented;
- the task would require inventing screen states or UX outcomes;
- the main issue is competitive rule interpretation rather than UI flow execution;
- the task is actually backend-only.

## Required Inputs

Before acting, identify:

- the affected feature or screen;
- the intended visible outcome;
- the key UI states involved;
- the backend capability the flow depends on;
- whether the flow touches permissions, activation, picks, scoring visibility, or corrected-result messaging.

## Documentation to Review First

Canonical documentation repository:

- `https://github.com/dlop6/Quiniela-Documentacion`

Read first:

- `04_Frontend`
- `02_Arquitectura`
- `09_UI_UX`
- `08_Testing`

Also read `01_Producto_y_Reglas` when rule-visible behavior is affected.

## Prompt Instructions

1. Identify the affected feature and visible states.
2. Read the minimum frontend, UX, and rule-owning docs needed for the flow.
3. Describe the documented flow in terms of:
   - entry point;
   - visible states;
   - backend dependencies;
   - failure or blocked states;
   - user-facing constraints.
4. Check whether the flow asks the UI to decide permissions, locks, scoring, or other official rules.
5. If the flow is implementable, propose the narrowest frontend change consistent with the documented behavior.
6. If documentation is missing or contradictory, stop and report the blocker.

## Expected Output

Always include:

1. `Summary`
2. `Docs consulted`
3. `Constraints applied`
4. `Risks`
5. `Open questions or blocked decisions`
6. `Validation or tests needed`
7. `Affected feature(s) or visible states`

## Stop Conditions

Stop and report a blocker if:

- the visible flow is not documented;
- the task requires inventing undocumented screen states or user outcomes;
- the frontend behavior depends on undocumented backend behavior;
- the proposed flow would shift official rule ownership into the UI.

## Non-Negotiable Rules

- Do not invent undocumented visible states or UX outcomes.
- Do not move official domain rules into the UI.
- Do not assume backend behavior that the docs do not support.
- Do not use this prompt without first identifying the owning docs.
