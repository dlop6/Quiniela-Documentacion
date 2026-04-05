---
name: generate-tests
description: Task template for deriving or reviewing Quiniela tests from documented behavior, risk, and regression sensitivity instead of guessed expectations.
scope: "testing tasks"
---

# Generate Tests

## Purpose

Use this prompt to generate, propose, or review tests only when the expected behavior can be tied to the official Quiniela documentation.

This prompt is for turning documented behavior into test intent. It does not authorize the model to invent expected outcomes.

## Use This Prompt When

Use this prompt when:

- the task is to create or refine tests;
- the task is to identify missing coverage for a change;
- the task is to map documented behavior to test cases;
- the task involves regression-sensitive logic that needs explicit validation expectations.

## Do Not Use This Prompt When

Do not use this prompt when:

- the expected behavior is not documented;
- the task is mainly implementation rather than testing;
- the task is purely exploratory debugging without a confirmed expected outcome;
- the task requires choosing behavior rather than validating it.

## Required Inputs

Before acting, gather:

- a short task summary;
- the affected area or module;
- the documented behavior or rule being validated;
- the likely test level:
  - unit;
  - integration;
  - end-to-end;
  - acceptance or regression review;
- any known high-risk flows affected by the change.

## Documentation to Review First

Canonical documentation repository:

- `https://github.com/dlop6/Quiniela-Documentacion`

Always read:

- `08_Testing`

Also read the affected area docs, such as:

- `03_Backend`
- `04_Frontend`
- `05_Datos`
- `06_Integraciones`
- `07_Seguridad_y_Operacion`
- `01_Producto_y_Reglas`

## Prompt Instructions

1. Identify the documented behavior that the tests must validate.
2. State which documentation families were consulted.
3. Choose the correct test level for the risk being covered.
4. List the primary cases to validate:
   - expected behavior;
   - failure behavior;
   - boundary behavior;
   - regression-sensitive behavior.
5. Call out which flows are especially sensitive:
   - permissions;
   - locks;
   - scoring;
   - result authority;
   - admin actions;
   - data invariants;
   - sync fallback or override behavior.
6. Identify coverage gaps if the current change does not validate the documented behavior well enough.
7. If the expected behavior cannot be traced to the documentation, stop and report that the test expectation is blocked.

## Expected Output

Always return:

1. `Summary`
2. `Docs consulted`
3. `Constraints applied`
4. `Risks`
5. `Open questions or blocked decisions`
6. `Validation or tests needed`
7. `Coverage and gaps`

If proposing test cases, group them by the documented behavior they validate.

## Stop Conditions

Stop and report a blocker if:

- the expected behavior is undocumented or contradictory;
- the correct test level cannot be determined without guessing architecture or ownership;
- the change touches high-risk behavior but the relevant source documents were not reviewed;
- the requested tests would validate invented behavior rather than documented behavior.

## Non-Negotiable Rules

- Do not generate tests for guessed behavior.
- Do not treat test convenience as proof of intended behavior.
- Do not ignore regression-sensitive flows when the change touches critical logic.
- Do not separate testing recommendations from the documented rule they are meant to protect.
