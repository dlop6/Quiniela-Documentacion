---
name: bugfix-workflow
description: Reusable workflow for fixing Quiniela bugs by reproducing them, mapping them to documented behavior, and applying the smallest safe correction.
scope: "bugfix workflows"
---

# Bugfix Workflow

## Purpose

Use this skill to fix a Quiniela bug without redefining intended behavior.

This skill exists to prevent “fixes” that silently introduce new rules, undocumented edge handling, or convenience-driven changes.

## Use This Skill When

Use this skill when:

- the current behavior appears wrong;
- there is enough documentation to define what should happen instead;
- the task is about correcting behavior rather than designing a new feature;
- regression risk matters.

## Do Not Use This Skill When

Do not use this skill when:

- the behavior is undocumented and the real issue is product ambiguity;
- the task is feature implementation rather than defect correction;
- the task requires choosing new official behavior;
- the report is only a symptom and no documented expected behavior can be identified.

## Required Inputs

Before using this skill, identify:

- the observed bad behavior;
- the expected documented behavior;
- the affected area or module;
- whether the bug touches high-risk flows such as scoring, locks, sync, admin actions, or permissions.

## Documentation to Review First

Canonical documentation repository:

- `https://github.com/dlop6/Quiniela-Documentacion`

Read the affected area docs first, then:

- `08_Testing`

Also read `01_Producto_y_Reglas`, `07_Seguridad_y_Operacion`, or `06_Integraciones` when the bug touches those concerns.

## Workflow

1. Reproduce or clearly restate the observed failure.
2. Identify the documented expected behavior.
3. Confirm that the issue is a real bug and not an unresolved product decision.
4. Trace the problem to its likely root cause area.
5. Apply the smallest safe correction that restores documented behavior.
6. Check whether the fix affects adjacent critical flows.
7. Report the root cause, the correction, and the regression checks needed.

## Quality Checks

- The fix restores documented behavior rather than inventing new behavior.
- The root cause is identified at the correct layer.
- The change is no broader than necessary.
- Regression-sensitive areas are checked explicitly.
- Any ambiguity is surfaced instead of patched over.

## Expected Output

Always include:

1. `Summary`
2. `Docs consulted`
3. `Constraints applied`
4. `Risks`
5. `Open questions or blocked decisions`
6. `Validation or tests needed`

Also include:

- `Observed behavior`
- `Expected behavior`
- `Root cause hypothesis or confirmed cause`

## Stop Conditions

Stop and report a blocker if:

- no documented expected behavior can be identified;
- the “bug” is actually an unresolved decision;
- the proposed fix would require inventing new official behavior;
- the root cause cannot be investigated without first closing a documentation gap.

## Non-Negotiable Rules

- Do not “fix” a bug by inventing new intended behavior.
- Do not hide a product ambiguity behind an implementation patch.
- Do not overcorrect with a broad refactor when a narrow fix is sufficient.
- Do not ignore regression risk in critical flows.
