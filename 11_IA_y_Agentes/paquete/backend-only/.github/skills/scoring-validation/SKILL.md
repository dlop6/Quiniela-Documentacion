---
name: scoring-validation
description: Reusable workflow for validating Quiniela picks, locks, scoring, tie-breaks, and leaderboard-affecting changes against official rules.
scope: "competitive-rule validation workflows"
---

# Scoring Validation

## Purpose

Use this skill to validate any change that can affect Quiniela competitive behavior, especially picks, locks, scoring, tie-breaks, special predictions, or leaderboard outcomes.

This skill exists because competitive logic is high-regression and cannot be reviewed safely from memory, intuition, or implementation convenience.

## Use This Skill When

Use this skill when:

- a task changes picks, lock behavior, scoring, tie-breaks, or special predictions;
- a change may affect leaderboard outcomes;
- a review must confirm competitive correctness;
- a regression-sensitive competitive flow needs structured validation.

## Do Not Use This Skill When

Do not use this skill when:

- the task is generic backend or frontend work with no competitive impact;
- the task is provider sync behavior rather than competitive-rule interpretation;
- the task is mainly schema evolution with no competitive semantics risk;
- the official competitive rule is still unresolved.

## Required Inputs

Before using this skill, identify:

- the competitive rule family involved;
- the affected implementation surface;
- whether the change touches timing, authority, precedence, rounding, or user-visible competitive output;
- whether the task is implementation, review, or test validation.

## Documentation to Review First

Canonical documentation repository:

- `https://github.com/dlop6/Quiniela-Documentacion`

Read first:

- `01_Producto_y_Reglas`
- `05_Datos`
- `08_Testing`

Then read the affected implementation docs:

- `03_Backend`
- `04_Frontend`

## Workflow

1. Identify the exact competitive rule family.
2. Load the official product rules first.
3. Confirm the task does not depend on an unresolved competitive decision.
4. Check whether the change affects:
   - pick validity;
   - lock timing or enforcement;
   - special prediction handling;
   - scoring calculation;
   - tie-break ordering;
   - leaderboard semantics;
   - corrected-result competitive impact.
5. Look explicitly for undocumented assumptions about:
   - timing;
   - precedence;
   - rounding;
   - fallback behavior;
   - visibility side effects.
6. Assess regression risk and required validation.
7. Report whether competitive correctness is preserved.

## Quality Checks

- The review is grounded in official rules, not memory.
- No undocumented timing or precedence assumption is accepted.
- The implementation surface stays subordinate to official competitive rules.
- Regression sensitivity is treated as mandatory, not optional.
- Any ambiguity is surfaced as a blocker rather than patched over.

## Expected Output

Always include:

1. `Summary`
2. `Docs consulted`
3. `Constraints applied`
4. `Risks`
5. `Open questions or blocked decisions`
6. `Validation or tests needed`
7. `Competitive impact`

Also include:

- `Rule family reviewed`
- `Regression-sensitive areas`

## Stop Conditions

Stop and report a blocker if:

- the relevant rule is missing or ambiguous;
- the task depends on undocumented rounding, timing, precedence, or lock behavior;
- the change could alter competitive outcomes without a documented basis;
- the visible or backend behavior implies a competitive rule that the docs do not define.

## Non-Negotiable Rules

- Do not guess scoring, pick, lock, tie-break, or leaderboard behavior.
- Do not reinterpret official rules for implementation convenience.
- Do not ignore regression impact on competitive fairness.
- Do not let UI or provider behavior redefine official competitive outcomes.
