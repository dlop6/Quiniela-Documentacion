---
name: validate-scoring
description: Task template for validating Quiniela scoring, picks, locks, tie-breaks, special predictions, and leaderboard-sensitive behavior.
scope: "competitive validation tasks"
---

# Validate Scoring

## Purpose

Use this prompt to validate a scoring-sensitive change against official Quiniela rules.

This prompt is for structured competitive-rule review, not for inventing scoring policy.

## Use This Prompt When

Use this prompt when:

- a change affects picks, locks, scoring, tie-breaks, special predictions, or leaderboard outcomes;
- competitive correctness must be validated;
- a regression-sensitive competitive rule needs review.

## Do Not Use This Prompt When

Do not use this prompt when:

- the task is generic backend or frontend work without competitive impact;
- the task is provider sync behavior rather than competitive-rule behavior;
- the relevant competitive rule is unresolved.

## Required Inputs

Before acting, identify:

- the rule family affected;
- the implementation surface affected;
- whether timing, precedence, rounding, authority, or visibility assumptions are involved;
- whether the task is implementation support, review, or test derivation.

## Documentation to Review First

Canonical documentation repository:

- `https://github.com/dlop6/Quiniela-Documentacion`

Read first:

- `01_Producto_y_Reglas`
- `05_Datos`
- `08_Testing`

Then read the affected implementation docs.

## Prompt Instructions

1. Identify the exact competitive rule family.
2. Load the official product rules first.
3. Check whether the task changes:
   - timing;
   - precedence;
   - rounding;
   - pick validity;
   - lock enforcement;
   - special prediction behavior;
   - leaderboard semantics.
4. Reject undocumented assumptions before continuing.
5. Report competitive impact and required regression validation.

## Expected Output

Always include:

1. `Summary`
2. `Docs consulted`
3. `Constraints applied`
4. `Risks`
5. `Open questions or blocked decisions`
6. `Validation or tests needed`
7. `Competitive impact`

## Stop Conditions

Stop and report a blocker if:

- the relevant rule is missing or ambiguous;
- the task depends on undocumented timing, rounding, precedence, or visibility behavior;
- the change could alter competitive outcomes without a documented basis.

## Non-Negotiable Rules

- Do not guess scoring, picks, locks, or tie-break behavior.
- Do not reinterpret official rules for convenience.
- Do not ignore regression risk for competitive flows.
- Do not let implementation shortcuts redefine competitive outcomes.
