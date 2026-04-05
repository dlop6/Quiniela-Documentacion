---
name: review-change
description: Task template for reviewing a Quiniela change set against documented behavior, architecture, testing, and risk expectations.
scope: "general review tasks"
---

# Review Change

## Purpose

Use this prompt to review a change set against Quiniela documentation with structured, findings-first output.

This prompt is for grounded review, not preference-driven commentary.

## Use This Prompt When

Use this prompt when:

- reviewing a change set or pull request;
- checking alignment with documentation;
- identifying defects, drift, or gaps;
- evaluating risk before merge.

## Do Not Use This Prompt When

Do not use this prompt when:

- the task is implementation rather than review;
- there is no documented behavior to review against;
- the task is actually operational release governance across many areas;
- the review would require inventing expected behavior.

## Required Inputs

Before acting, identify:

- what changed;
- which areas are affected;
- which docs own those behaviors;
- whether the review is general or focused on a specific risk area.

## Documentation to Review First

Canonical documentation repository:

- `https://github.com/dlop6/Quiniela-Documentacion`

Read the affected area docs first.

Also read:

- `07_Seguridad_y_Operacion` when permissions, admin behavior, secrets, or traceability matter;
- `08_Testing` when regression or coverage matters.

## Prompt Instructions

1. Identify the affected areas and source-of-truth docs.
2. Review the change against documented expectations, not local preference.
3. Look for:
   - incorrect behavior;
   - architecture drift;
   - missing validation;
   - missing auditability or traceability;
   - missing tests;
   - unresolved-decision leakage.
4. Classify findings by severity.
5. Report residual risks and ambiguities separately from confirmed defects.

## Expected Output

Always include:

1. `Findings by severity`
2. `Docs consulted`
3. `Gaps or missing tests`
4. `Residual risks`
5. `Open questions or blocked decisions`

If there are no findings, say so explicitly and still mention any residual risk.

## Stop Conditions

Stop and report a blocker if:

- the affected behavior has no identifiable source-of-truth document;
- the docs are contradictory in a way that changes the review outcome;
- the review would require inventing intended behavior;
- the task is really a product clarification request instead of a review.

## Non-Negotiable Rules

- Do not review against undocumented personal preference.
- Do not bury serious findings under generic summary text.
- Do not pretend missing docs imply correctness.
- Do not claim confidence when the review basis is unclear.
