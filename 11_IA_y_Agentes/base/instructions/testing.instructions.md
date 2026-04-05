---
scope: "testing tasks"
---

# Testing Instructions

## Purpose

These instructions define the always-on testing guardrails for Quiniela work.

They exist to keep tests tied to documented behavior, focused on risk, and sensitive to backend enforcement and competitive correctness.

## When It Applies

Use these instructions for:

- creating tests;
- updating tests;
- reviewing test coverage;
- validating regression risk;
- deciding what should be tested for a change.

## Primary Guardrails

- Map tests to documented rules, flows, or critical scenarios.
- Prefer risk-based coverage over volume-based coverage.
- Do not let UI-only tests hide backend enforcement failures.
- Treat locks, scoring, results sync, admin actions, and permissions as high-regression areas when affected.
- Keep test intent traceable to the documented behavior it protects.
- Do not invent expected outcomes that are not documented.

## Required Documentation

Read the minimum relevant document families from `dlop6/Quiniela-Documentacion`:

Canonical URL:

- `https://github.com/dlop6/Quiniela-Documentacion`

- `08_Testing`

Also read the affected area docs such as:

- `01_Producto_y_Reglas`
- `03_Backend`
- `04_Frontend`
- `05_Datos`
- `06_Integraciones`
- `07_Seguridad_y_Operacion`

## Task-Specific Constraints

- When a change affects a critical domain flow, treat regression coverage as mandatory.
- When a test checks visible UI behavior, confirm whether backend enforcement also needs coverage.
- When a task touches scoring, locks, sync, override, admin actions, or account state, read the owning docs before proposing expected outcomes.
- Do not accept brittle tests that merely mirror implementation detail instead of documented behavior.
- Keep failure expectations aligned with documented rules, not convenience assumptions.

## Stop Conditions

Stop and report a blocker if:

- no documented behavior can be tied to the proposed test;
- the expected result depends on an unresolved decision;
- the change affects a critical flow but the owning documentation is unclear;
- the task would require inventing test acceptance criteria not backed by documentation.

## Non-Negotiable Rules

- Do not write tests for guessed behavior.
- Do not treat frontend-only validation as sufficient for protected backend rules.
- Do not ignore regression sensitivity in competitive or administrative flows.
- Do not claim meaningful coverage when the critical documented behavior remains untested.
