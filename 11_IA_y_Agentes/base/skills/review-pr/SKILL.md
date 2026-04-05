---
name: review-pr
description: Reusable workflow for reviewing Quiniela changes against documented behavior, architecture, security, testing, and unresolved-decision risk.
scope: "review workflows"
---

# Review PR

## Purpose

Use this skill to review a Quiniela change set against the official documentation repository and report findings in a disciplined, risk-aware way.

This skill exists to keep reviews grounded in documented expectations rather than taste, habit, or undocumented local conventions.

## Use This Skill When

Use this skill when:

- reviewing a pull request or change set;
- checking alignment with docs before merge;
- assessing regression risk;
- looking for bugs, architecture drift, missing tests, or security concerns.

## Do Not Use This Skill When

Do not use this skill when:

- the task is implementation rather than review;
- there is no documented behavior to compare against;
- the task is really a release-readiness assessment across many areas;
- the task is a pure product decision rather than a code review.

## Required Inputs

Before using this skill, identify:

- what changed;
- which areas are affected;
- which docs should own the changed behavior;
- whether the review is general or focused on a high-risk area.

## Documentation to Review First

Canonical documentation repository:

- `https://github.com/dlop6/Quiniela-Documentacion`

Read the affected area docs first.

Also read:

- `08_Testing` when regression risk matters;
- `07_Seguridad_y_Operacion` when permissions, secrets, admin actions, or critical traces are involved.

## Workflow

1. Identify the affected areas and load the owning docs.
2. Review the change against documented behavior, not against taste.
3. Check for:
   - incorrect behavior;
   - architecture drift;
   - missing validation;
   - missing traceability;
   - missing or weak tests;
   - unresolved-decision leakage.
4. Classify findings by severity.
5. Report gaps, residual risks, and unclear areas explicitly.

## Quality Checks

- Findings are grounded in documentation, not preference.
- Findings are ordered by severity.
- Missing tests are called out when risk justifies them.
- Ambiguity is reported as ambiguity, not as a false defect.
- Non-issues are not inflated into noise.

## Expected Output

Always include:

1. `Findings by severity`
2. `Docs consulted`
3. `Gaps or missing tests`
4. `Residual risks`
5. `Open questions or blocked decisions`

If no findings are discovered, state that explicitly and note any remaining risk or testing gaps.

## Stop Conditions

Stop and report a blocker if:

- the affected behavior has no identifiable source-of-truth document;
- the review would require inventing expected behavior;
- the docs are contradictory in a way that changes the review outcome;
- the task is actually a product clarification request rather than a review.

## Non-Negotiable Rules

- Do not review against undocumented personal preference.
- Do not bury the main findings under summary text.
- Do not treat missing documentation as evidence that the implementation is correct.
- Do not claim confidence when the source of truth is missing or contradictory.
