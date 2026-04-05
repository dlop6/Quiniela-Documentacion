---
name: scaffold-module
description: Task template for scaffolding a Quiniela backend module or frontend feature boundary from documented architecture and ownership rules.
scope: "scaffolding tasks"
---

# Scaffold Module

## Purpose

Use this prompt to scaffold a backend module or frontend feature structure from documented architecture, ownership, and boundary rules.

This prompt is for structural setup, not for inventing new architecture.

## Use This Prompt When

Use this prompt when:

- the task is to scaffold a documented module or feature;
- ownership and boundaries are already defined;
- the goal is structural setup rather than full implementation;
- the task requires preserving architecture discipline from the start.

## Do Not Use This Prompt When

Do not use this prompt when:

- the architecture or ownership is undocumented;
- the task would require inventing new module responsibilities;
- the real need is implementation of behavior rather than structural setup;
- the task is really a data-modeling or competitive-rule decision.

## Required Inputs

Before acting, identify:

- whether the scaffolding target is backend or frontend;
- the intended owner area;
- the documented boundaries;
- the in-scope responsibilities;
- any data or contract boundaries that the scaffold must respect.

## Documentation to Review First

Canonical documentation repository:

- `https://github.com/dlop6/Quiniela-Documentacion`

Read first:

- `02_Arquitectura`

Then read the affected technical docs such as:

- `03_Backend`
- `04_Frontend`
- `05_Datos` when data boundaries matter

## Prompt Instructions

1. Identify the intended owner and the architecture boundary.
2. Read the minimum architecture and technical docs needed.
3. Describe the scaffold in terms of:
   - ownership;
   - responsibility;
   - boundaries;
   - allowed neighbors;
   - what the scaffold should not absorb.
4. Ensure the scaffold does not imply undocumented behavior.
5. If the module or feature boundary is not documented clearly, stop and report the blocker.

## Expected Output

Always include:

1. `Summary`
2. `Docs consulted`
3. `Constraints applied`
4. `Risks`
5. `Open questions or blocked decisions`
6. `Validation or tests needed`
7. `Affected owner area`

## Stop Conditions

Stop and report a blocker if:

- the owner area is unclear;
- the scaffold would require inventing undocumented responsibilities or boundaries;
- architecture docs are missing or contradictory;
- data or contract boundaries are required but not documented.

## Non-Negotiable Rules

- Do not invent undocumented architecture.
- Do not let scaffolding imply undocumented business behavior.
- Do not blur ownership for convenience.
- Do not skip documentation lookup just because the task is “only structural.”
