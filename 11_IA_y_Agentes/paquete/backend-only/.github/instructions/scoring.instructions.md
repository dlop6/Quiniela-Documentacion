---
scope: "scoring, picks, and lock-sensitive tasks"
---

# Scoring Instructions

## Purpose

These instructions define the always-on guardrails for Quiniela scoring-sensitive work, including picks, locks, special predictions, tie-breaks, and leaderboard-affecting behavior.

They exist to protect competitive correctness and prevent convenience-driven reinterpretation of official rules.

## When It Applies

Use these instructions for:

- scoring changes;
- picks or lock behavior changes;
- leaderboard-affecting changes;
- tie-break logic review;
- special prediction behavior review;
- test or review work for competitive rules.

## Primary Guardrails

- Use only officially documented rules for picks, locks, scoring, tie-breaks, and related visibility behavior.
- Do not reinterpret competitive rules for implementation convenience.
- Treat scoring-sensitive changes as regression-sensitive by default.
- Do not invent silent rounding, precedence, or fallback behavior.
- Verify temporal and authority assumptions before changing lock or scoring behavior.
- Treat leaderboard behavior as derived from documented rules, not as an independent rule source.

## Required Documentation

Read the minimum relevant document families from `dlop6/Quiniela-Documentacion`:

Canonical URL:

- `https://github.com/dlop6/Quiniela-Documentacion`

- `01_Producto_y_Reglas`
- `05_Datos`
- `08_Testing`

Also read `03_Backend` and `04_Frontend` when the task affects implementation ownership or visible presentation of scoring-sensitive behavior.

## Task-Specific Constraints

- Do not implement or review scoring logic from memory alone.
- When a task affects picks, special predictions, lock windows, result authority, or leaderboard behavior, verify the relevant documents before proposing any change.
- Treat exactness as more important than convenience in competitive behavior.
- If a visible scoring behavior depends on backend authority, do not let UI assumptions redefine it.
- Treat special predictions as a separate visible flow from regular match picks.
- Treat `Campeon` and `Goleador` as hidden from other users until a later reveal rule is formally closed.
- Assume regression risk whenever the task touches scoring-sensitive flows, even if the requested change appears small.

## Stop Conditions

Stop and report a blocker if:

- the relevant competitive rule is missing or ambiguous;
- the task depends on undocumented rounding, precedence, or lock behavior;
- the proposed change would alter competitive outcomes without a documented basis;
- the change affects leaderboard semantics but the owning rules cannot be identified.

## Non-Negotiable Rules

- Do not guess scoring, picks, lock, or tie-break behavior.
- Do not reinterpret official rules for engineering convenience.
- Do not ignore regression impact on competitive fairness.
- Do not let presentation logic redefine official competitive behavior.
