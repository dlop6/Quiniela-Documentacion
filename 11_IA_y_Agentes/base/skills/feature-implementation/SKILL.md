---
name: feature-implementation
description: Reusable workflow for implementing documented Quiniela behavior safely, incrementally, and without improvising open decisions.
scope: "implementation workflows"
---

# Feature Implementation

## Purpose

Use this skill to implement a documented Quiniela feature or change in a way that stays aligned with the official documentation repository.

This skill is for execution flow, not role specialization. It tells the model how to implement safely once the task type has already been identified.

## Use This Skill When

Use this skill when:

- the task is implementation work rather than abstract planning;
- the intended behavior is already documented;
- the task has a clear owner area such as backend, frontend, integrations, data, or testing support;
- the work can proceed without closing an unresolved product or architecture decision first.

## Do Not Use This Skill When

Do not use this skill when:

- the desired behavior is still open or ambiguous;
- the task is mainly bug triage rather than implementation;
- the task is mainly PR review or release readiness review;
- the task depends on undocumented behavior;
- the main question is “what should the system do?” instead of “how do we implement the documented behavior?”

## Required Inputs

Before using this skill, identify:

- the task summary;
- the affected repository area;
- the responsible module, feature, or workflow if known;
- the primary documentation families that own the behavior;
- whether the task affects security, testing, data, scoring, or integrations.

## Documentation to Review First

Canonical documentation repository:

- `https://github.com/dlop6/Quiniela-Documentacion`

Read the affected area docs first.

Always read the owning documents for the change, and also read:

- `01_Producto_y_Reglas` when behavior is domain-sensitive;
- `07_Seguridad_y_Operacion` when critical actions, permissions, or traceability are affected;
- `08_Testing` when regression-sensitive flows are touched.

## Workflow

1. Identify the task owner:
   - backend;
   - frontend;
   - data;
   - integrations;
   - cross-cutting.
2. Read the minimum documentation needed to understand the intended behavior.
3. Write down what is in scope and what is out of scope.
4. Confirm that no unresolved decision is required to implement the task.
5. Identify the implementation surface:
   - module;
   - feature;
   - contract boundary;
   - persistence impact;
   - visible state impact.
6. Apply the narrowest change that satisfies the documented behavior.
7. Check whether the change affects:
   - permissions;
   - auditability;
   - scoring or locks;
   - results authority;
   - testing expectations.
8. Summarize what was implemented, what remains blocked, and what must be validated.

## Quality Checks

- The behavior is backed by documentation, not by interpretation.
- The change stays inside the documented ownership boundaries.
- No hidden assumptions were introduced about permissions, states, or authority.
- The implementation surface matches the documented responsibility.
- Regression-sensitive areas are called out explicitly.

## Expected Output

Always include:

1. `Summary`
2. `Docs consulted`
3. `Constraints applied`
4. `Risks`
5. `Open questions or blocked decisions`
6. `Validation or tests needed`

Also include:

- `In scope`
- `Out of scope`
- `Affected owner area`

## Stop Conditions

Stop and report a blocker if:

- the intended behavior cannot be tied to a primary document;
- the task requires closing an unresolved decision;
- the implementation would require undocumented fallback, precedence, state, or authority behavior;
- the task owner cannot be identified with enough confidence.

## Non-Negotiable Rules

- Do not implement guessed behavior.
- Do not silently expand scope.
- Do not treat convenience as a valid replacement for documented behavior.
- Do not ignore testing, security, or traceability impact on critical flows.
