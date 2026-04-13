---
name: harden-security
description: Task template for reviewing Quiniela changes against documented security expectations, trust boundaries, and critical-action traceability.
scope: "security-focused review tasks"
---

# Harden Security

## Purpose

Use this prompt to review a change against Quiniela security expectations when the main concern is trust boundaries, permissions, secret handling, safe error exposure, or traceability.

This prompt is for security-focused analysis, not for inventing a new security strategy.

## Use This Prompt When

Use this prompt when:

- the task touches authentication, authorization, roles, admin actions, or sensitive data;
- the task exposes inputs from untrusted clients or external providers;
- the task could leak secrets, internal errors, or privileged behavior;
- the task changes critical mutation flows that require traceability.

## Do Not Use This Prompt When

Do not use this prompt when:

- the task is not meaningfully security-sensitive;
- the requested change requires choosing an undocumented auth strategy or policy;
- the main work is generic implementation and not risk review;
- the task belongs primarily to scoring or integrations and security is only a secondary concern.

## Required Inputs

Before acting, gather:

- a short task summary;
- the affected area or module;
- any known trust boundaries involved;
- whether the change affects roles, permissions, activation, admin actions, external inputs, or error surfaces;
- any existing implementation or review context.

## Documentation to Review First

Canonical documentation repository:

- `https://github.com/dlop6/Quiniela-Documentacion`

Always read:

- `07_Seguridad_y_Operacion`
- `08_Testing`

Also read the affected area docs such as:

- `02_Arquitectura`
- `03_Backend`
- `04_Frontend`
- `06_Integraciones`
- `05_Datos`

## Prompt Instructions

1. Identify the trust boundary or critical action affected by the change.
2. List the documentation families consulted.
3. State the main security questions:
   - who is allowed;
   - who provides the input;
   - what is trusted;
   - what must be traced;
   - what must never be exposed.
4. Review whether the change weakens or bypasses:
   - permission enforcement;
   - backend authority;
   - input validation;
   - safe error handling;
   - secret handling;
   - auditability.
5. Classify findings by severity if issues are found.
6. Separate confirmed defects from open policy questions.
7. If the review depends on undocumented security policy, stop and report the blocker instead of choosing a strategy.

## Expected Output

Always return:

1. `Summary`
2. `Docs consulted`
3. `Constraints applied`
4. `Risks`
5. `Open questions or blocked decisions`
6. `Validation or tests needed`
7. `Findings by severity`

If no findings are found, say so explicitly and mention any residual risk or testing gap.

## Stop Conditions

Stop and report a blocker if:

- the task requires choosing an undocumented auth, session, or security policy;
- the trust boundary cannot be identified;
- the permission or traceability requirements are materially unclear;
- the requested review would depend on assumptions about secret storage, admin authority, or external trust that are not documented.

## Non-Negotiable Rules

- Do not trust client or provider input by default.
- Do not choose undocumented security strategy details on your own.
- Do not expose internal details through convenience error handling.
- Do not ignore traceability for critical mutations or privileged actions.
