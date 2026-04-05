---
name: create-endpoint
description: Task template for creating or reviewing a Quiniela backend endpoint from documented behavior, ownership boundaries, and backend authority rules.
scope: "backend endpoint tasks"
---

# Create Endpoint

## Purpose

Use this prompt to create, review, or refine a backend endpoint only when the intended behavior can be tied to the official Quiniela documentation.

This prompt is a task accelerator. It does not replace the Quiniela documentation repository, the backend instructions, the backend specialist agent, or the implementation skill.

## Use This Prompt When

Use this prompt when:

- the task is about a backend endpoint, route, operation, or controller boundary;
- the expected behavior is already documented or can be traced to a documented rule;
- the main work is endpoint-focused rather than a large workflow redesign;
- you need a structured way to gather ownership, contracts, validation, authority, and testing impact before implementing.

## Do Not Use This Prompt When

Do not use this prompt when:

- the intended behavior is still open, disputed, or undocumented;
- the task is primarily about scoring, picks, locks, or leaderboard behavior and needs scoring-specialist review first;
- the task is primarily about external results sync, manual override, fallback, or reconciliation and needs integration-specialist review first;
- the task is mainly schema evolution, invariants, or persistence modeling;
- the task is a broad feature workflow that should use the implementation skill instead.

## Required Inputs

Before acting, gather:

- a short task summary;
- the expected endpoint purpose;
- the likely owning backend module;
- any known request and response boundary expectations;
- whether the endpoint touches permissions, scoring, locks, results authority, admin actions, or user state changes;
- any existing code or contract context if the endpoint already exists.

## Documentation to Review First

Canonical documentation repository:

- `https://github.com/dlop6/Quiniela-Documentacion`

Read the minimum relevant document families first:

- `03_Backend`
- `02_Arquitectura`
- `05_Datos`
- `07_Seguridad_y_Operacion`
- `08_Testing`

Also read:

- `01_Producto_y_Reglas` when the endpoint changes official product behavior, eligibility, picks, locks, scoring, results, leaderboard behavior, or user-visible state transitions.

## Prompt Instructions

1. Identify the owning backend module and state why it owns the endpoint.
2. List the documentation families consulted before proposing any implementation shape.
3. Define the endpoint objective in one short sentence tied to documented behavior.
4. Describe the contract boundary:
   - caller or client;
   - input shape or validation concerns;
   - output shape or error surface;
   - whether the endpoint mutates official state.
5. State the backend authority assumptions explicitly:
   - permissions;
   - locks;
   - official results;
   - scoring effects;
   - auditability requirements.
6. Identify which concerns belong in:
   - boundary validation;
   - application orchestration;
   - domain logic;
   - infrastructure adaptation.
7. Call out required audit, idempotency, testing, and error-handling impact.
8. If anything is undocumented or blocked by an open decision, stop and report it instead of inventing endpoint behavior.

## Expected Output

Always return:

1. `Summary`
2. `Docs consulted`
3. `Constraints applied`
4. `Risks`
5. `Open questions or blocked decisions`
6. `Validation or tests needed`
7. `Affected module(s)`
8. `Contract boundary`

If proposing implementation, also state whether the endpoint stays fully inside documented behavior.

## Stop Conditions

Stop and report a blocker if:

- the owning backend module cannot be identified;
- the endpoint would require undocumented behavior, states, or transitions;
- request validation rules are materially unclear;
- authority rules for permissions, locks, scoring, or results are missing or contradictory;
- the task really belongs to scoring, integrations, or data evolution and that specialist context is not loaded.

## Non-Negotiable Rules

- Do not invent undocumented endpoints, states, or workflow branches.
- Do not put official business rules in controller code for convenience.
- Do not bypass backend authority because another layer appears to validate first.
- Do not ignore auditability, idempotency, error handling, or regression impact on critical endpoints.
