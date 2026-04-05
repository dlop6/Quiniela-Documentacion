---
name: generate-tests
description: Task template for generating or proposing Quiniela tests from documented behavior and risk-sensitive flows.
scope: "testing tasks"
---

# Generate Tests

## Purpose

Use this prompt to generate or propose tests tied to documented Quiniela behavior and documented risk.

This prompt is not for inventing expected behavior. It is for translating documented behavior into useful test coverage.

## Use This Prompt When

Use this prompt when:

- the task is to propose or write tests;
- the behavior under test is already documented;
- regression risk or coverage gaps need to be addressed;
- test scope and level need to be clarified.

## Do Not Use This Prompt When

Do not use this prompt when:

- expected behavior is undocumented or unresolved;
- the real task is feature implementation rather than testing;
- the task requires policy invention about what the product should do;
- the prompt would have to guess failure behavior or acceptance criteria.

## Required Inputs

Before acting, identify:

- the documented behavior being tested;
- the area affected;
- the risk level;
- the most appropriate test level;
- whether the task touches critical flows such as locks, scoring, sync, admin actions, or permissions.

## Documentation to Review First

Canonical documentation repository:

- `https://github.com/dlop6/Quiniela-Documentacion`

Read first:

- `08_Testing`

Then read the affected area docs.

## Prompt Instructions

1. Identify the documented behavior under test.
2. Determine the best test level or mix of levels.
3. Name the regression-sensitive flows that must be protected.
4. Generate or propose tests that map directly to documented expectations.
5. Call out where frontend-only tests are insufficient because backend enforcement also needs coverage.
6. Report any coverage gaps or blockers before claiming the tests are complete.

## Expected Output

Always include:

1. `Summary`
2. `Docs consulted`
3. `Constraints applied`
4. `Risks`
5. `Open questions or blocked decisions`
6. `Validation or tests needed`
7. `Coverage and gaps`

## Stop Conditions

Stop and report a blocker if:

- no documented expected behavior can be identified;
- the test would require guessing acceptance criteria;
- the behavior is unresolved or contradictory across docs;
- the task requires pretending coverage is sufficient when critical cases remain unclear.

## Non-Negotiable Rules

- Do not generate tests for guessed behavior.
- Do not hide missing backend enforcement behind UI-only tests.
- Do not confuse test volume with risk coverage.
- Do not overstate coverage when documented critical behavior remains unprotected.
