---
name: review-change
description: Task template for reviewing Quiniela changes against documented behavior, architecture, testing, and risk, using findings-first output.
scope: "general review tasks"
---

# Review Change

## Purpose

Use this prompt to review a change set against the official Quiniela documentation when the goal is to identify defects, risks, contradictions, and testing gaps.

This prompt is for disciplined review. It is not a replacement for specialist agents when the main risk is clearly scoring, integrations, data, or security.

## Use This Prompt When

Use this prompt when:

- the task is to review a diff, PR, proposal, or implementation plan;
- the change spans one or more technical areas and needs structured review;
- the expected output should be findings-first rather than an implementation plan;
- the task needs explicit separation between defects, risks, and ambiguities.

## Do Not Use This Prompt When

Do not use this prompt when:

- the task is purely implementation with no review component;
- the change belongs primarily to a specialist area that should be reviewed with a narrower prompt first;
- the source-of-truth documents are unavailable;
- the task asks for personal preference or style feedback without documented justification.

## Required Inputs

Before acting, gather:

- a short summary of the change;
- the affected areas or modules;
- the code, diff, proposal, or artifact to review;
- any known high-risk domains touched by the change;
- whether the review should focus on correctness, architecture, security, testing, operations, or all of them.

## Documentation to Review First

Canonical documentation repository:

- `https://github.com/dlop6/Quiniela-Documentacion`

Read the affected area docs first.

Also read:

- `07_Seguridad_y_Operacion` when the change touches permissions, admin actions, sensitive flows, traceability, or external inputs;
- `08_Testing` when the change touches critical behavior, regression-sensitive flows, or expected validation coverage.

## Prompt Instructions

1. Identify the owning documentation families for the change.
2. Review the change against documented behavior and architecture, not personal preference.
3. Prioritize findings by severity and user or system impact.
4. Separate:
   - confirmed defects;
   - likely risks;
   - ambiguities caused by missing or conflicting documentation.
5. Call out gaps in:
   - correctness;
   - ownership boundaries;
   - data invariants;
   - security expectations;
   - testing coverage;
   - operational traceability.
6. If the review enters a specialist area, say so clearly and note whether a specialist review should follow.
7. If the relevant documentation is missing or contradictory, stop and report that the review cannot be completed safely.

## Expected Output

Always return:

1. `Summary`
2. `Docs consulted`
3. `Constraints applied`
4. `Risks`
5. `Open questions or blocked decisions`
6. `Validation or tests needed`
7. `Findings`

For review work, present findings first and order them by severity.

## Stop Conditions

Stop and report a blocker if:

- the owning documentation cannot be identified;
- the requested review would rely on undocumented expected behavior;
- conflicting documents materially change the outcome of the review;
- the change needs specialist analysis that has not been loaded and the outcome would otherwise be guesswork.

## Non-Negotiable Rules

- Do not review from undocumented personal preference.
- Do not hide correctness risks behind stylistic commentary.
- Do not treat missing tests as minor when the change touches critical behavior.
- Do not continue as if contradictions in the source documents were harmless.
