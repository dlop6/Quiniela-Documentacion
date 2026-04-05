---
name: harden-security
description: Task template for reviewing Quiniela changes against documented security expectations and identifying actionable risks.
scope: "security review tasks"
---

# Harden Security

## Purpose

Use this prompt to review a change against documented Quiniela security expectations.

This prompt is for grounded security review, not for inventing a new security architecture.

## Use This Prompt When

Use this prompt when:

- a change may affect trust boundaries;
- permissions or admin actions are involved;
- input handling is risky;
- secrets, credentials, or unsafe error exposure are relevant;
- traceability of critical actions matters.

## Do Not Use This Prompt When

Do not use this prompt when:

- the task is generic implementation with no meaningful security risk;
- the required security behavior is undocumented and the request is really asking for policy invention;
- the task is primarily about non-security architecture or UI composition.

## Required Inputs

Before acting, identify:

- the risky change surface;
- the relevant trust boundary;
- the documented security expectation involved;
- whether the task touches auth, permissions, traceability, input validation, or secret handling.

## Documentation to Review First

Canonical documentation repository:

- `https://github.com/dlop6/Quiniela-Documentacion`

Read first:

- `07_Seguridad_y_Operacion`
- `08_Testing`

Then read the affected area docs.

## Prompt Instructions

1. Identify the security-sensitive surface of the task.
2. Read the relevant security docs first, then the changed-area docs.
3. Evaluate the change for:
   - trust-boundary violations;
   - weak permission assumptions;
   - unsafe input handling;
   - secret leakage;
   - unsafe error exposure;
   - missing traceability for critical actions.
4. Report findings in a structured way.
5. If review depends on undocumented security policy, stop and report the blocker.

## Expected Output

Always include:

1. `Summary`
2. `Docs consulted`
3. `Constraints applied`
4. `Risks`
5. `Open questions or blocked decisions`
6. `Validation or tests needed`
7. `Findings by severity`

## Stop Conditions

Stop and report a blocker if:

- the task requires choosing undocumented auth or permission policy;
- the security expectation needed for review is missing or contradictory;
- the review would require inventing secret-handling or trust-boundary behavior.

## Non-Negotiable Rules

- Do not trust client-side protection for protected behavior.
- Do not expose secrets or unsafe diagnostics.
- Do not approve undocumented security tradeoffs.
- Do not use this prompt as a substitute for the security docs.
