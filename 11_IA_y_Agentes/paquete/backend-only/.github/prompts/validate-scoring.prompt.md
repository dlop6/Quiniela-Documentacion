---
name: validate-scoring
description: Task template for validating Quiniela scoring, picks, locks, special predictions, tie-breaks, and leaderboard-sensitive behavior against official rules.
scope: "competitive-rule review tasks"
---

# Validate Scoring

## Purpose

Use this prompt to validate any change that affects competitive behavior such as picks, locks, scoring, tie-breaks, special predictions, or leaderboard-sensitive visibility.

This prompt treats competitive logic as high-regression and requires exact alignment with documented rules. It does not allow policy invention for convenience.

## Use This Prompt When

Use this prompt when:

- the task touches picks, locks, score calculation, special predictions, tie-breaks, or leaderboard-sensitive behavior;
- the task could change who earns points, when a pick becomes immutable, or how official outcomes are interpreted;
- the task needs rule validation before implementation or review can be trusted.

## Do Not Use This Prompt When

Do not use this prompt when:

- the task is unrelated to competitive logic;
- the scoring-relevant behavior is undocumented or still open;
- the main issue is provider sync or manual override authority and integration review should come first;
- the task is purely structural and does not affect competitive outcomes.

## Required Inputs

Before acting, gather:

- a short task summary;
- the scoring-sensitive behavior being changed;
- whether the change affects picks, locks, special predictions, score calculation, visibility, or tie-breaks;
- the implementation surface:
  - backend;
  - frontend;
  - both;
- any known timing, authority, or visibility assumptions.

## Documentation to Review First

Canonical documentation repository:

- `https://github.com/dlop6/Quiniela-Documentacion`

Always read:

- `01_Producto_y_Reglas`
- `05_Datos`
- `08_Testing`

Also read:

- `03_Backend` when backend logic or enforcement is touched;
- `04_Frontend` when visible scoring states or competitive UI are touched.

## Prompt Instructions

1. Identify exactly which competitive rule family is affected.
2. List the documentation families consulted.
3. State the authority path for the behavior:
   - who decides;
   - when it becomes official;
   - what data it depends on;
   - what user-visible effects it creates.
4. Check timing and precedence assumptions:
   - lock timing;
   - result authority;
   - scoring calculation order;
   - tie-break precedence;
   - visibility timing.
5. Call out regression risk explicitly and explain what could break competitively if the interpretation is wrong.
6. Identify the tests or validation evidence needed before the change can be trusted.
7. If any part of the rule depends on undocumented rounding, precedence, timing, or visibility behavior, stop and report the blocker.

## Expected Output

Always return:

1. `Summary`
2. `Docs consulted`
3. `Constraints applied`
4. `Risks`
5. `Open questions or blocked decisions`
6. `Validation or tests needed`
7. `Competitive impact`

If reviewing a proposal, state clearly whether the proposal stays inside the documented competitive rules.

## Stop Conditions

Stop and report a blocker if:

- the relevant scoring rule family cannot be identified;
- timing, precedence, or authority is undocumented or contradictory;
- the change would require guessed lock, tie-break, scoring, or visibility behavior;
- the regression risk is high but the necessary documentation or testing basis is missing.

## Non-Negotiable Rules

- Do not invent competitive policy for convenience.
- Do not assume undocumented rounding, timing, precedence, or visibility rules.
- Do not treat frontend display logic as authority over official competitive outcomes.
- Do not minimize regression risk when points, locks, or leaderboard behavior can change.
