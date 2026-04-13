---
scope: "security-sensitive tasks"
---

# Security Instructions

## Purpose

These instructions define the always-on security guardrails for Quiniela implementation and review work.

They exist to keep changes aligned with documented security expectations without forcing undocumented implementation choices.

## When It Applies

Use these instructions for:

- authentication or authorization changes;
- input validation changes;
- secret or credential handling;
- admin or privileged flows;
- security review of any risky feature;
- changes that affect sensitive data or critical actions.

## Primary Guardrails

- Trust no external input without validation.
- Treat authentication, authorization, and traceability as separate concerns.
- Follow least privilege for any sensitive action or role-sensitive behavior.
- Do not expose secrets, internal diagnostics, or unsafe error detail.
- Require traceability for critical state changes, overrides, or administrative actions.
- Use the documented auth baseline for v1: access JWT plus refresh token, with no social login.

## Required Documentation

Read the minimum relevant document families from `dlop6/Quiniela-Documentacion`:

Canonical URL:

- `https://github.com/dlop6/Quiniela-Documentacion`

- `07_Seguridad_y_Operacion`
- `08_Testing`

Also read the affected area docs such as `03_Backend`, `04_Frontend`, `05_Datos`, or `06_Integraciones` depending on where the risky change lives.

## Task-Specific Constraints

- Do not treat UI hiding as a permission model.
- Do not override the documented authentication or session mechanism by convenience.
- Do not accept undocumented tradeoffs that weaken validation, auditability, or error safety.
- When the task changes admin actions, manual overrides, access states, or critical mutations, verify traceability requirements before proceeding.
- Prefer behavior-level security alignment over tool-specific implementation guesses.

## Stop Conditions

Stop and report a blocker if:

- the task depends on an undocumented security control or auth decision;
- a critical action cannot be tied to an audit or traceability expectation;
- the change would expose hidden internals, weaken permission boundaries, or bypass validation;
- the task requires choosing a security strategy that has not been documented;
- the documentation required to understand the risk surface is missing or contradictory.

## Non-Negotiable Rules

- Do not trust client-side enforcement for protected behavior.
- Do not expose secrets, tokens, or internal diagnostics in outputs or implementations.
- Do not weaken traceability for critical actions.
- Do not guess undocumented authentication, authorization, or secret-handling behavior.
