---
name: review-integration-sync
description: Task template for reviewing Quiniela result synchronization, provider adaptation, fallback, manual override, and reconciliation behavior.
scope: "integration sync review tasks"
---

# Review Integration Sync

## Purpose

Use this prompt to review or design result synchronization behavior when the task involves external providers, provider adaptation, fallback paths, manual override, or reconciliation logic.

This prompt assumes external data is untrusted until it is processed through documented authority rules.

## Use This Prompt When

Use this prompt when:

- the task touches external result ingestion or synchronization;
- the task affects manual override, fallback behavior, reconciliation, or provider adaptation;
- the task could change how official results are sourced, confirmed, or corrected;
- the review must verify authority and contingency behavior before implementation can be trusted.

## Do Not Use This Prompt When

Do not use this prompt when:

- the task is unrelated to integrations or official result authority;
- the provider or authority behavior is still open and undocumented;
- the task is primarily scoring validation rather than sync or source authority;
- the task is generic backend structure work with no integration risk.

## Required Inputs

Before acting, gather:

- a short task summary;
- the external provider or source involved if known;
- whether the change touches sync, adaptation, fallback, manual override, reconciliation, or correction paths;
- any known authority assumptions;
- any existing implementation or flow context.

## Documentation to Review First

Canonical documentation repository:

- `https://github.com/dlop6/Quiniela-Documentacion`

Always read:

- `06_Integraciones`
- `07_Seguridad_y_Operacion`
- `08_Testing`

Also read:

- `03_Backend`
- `05_Datos`

## Prompt Instructions

1. Identify the authority path for the result or external data flow.
2. List the documentation families consulted.
3. Separate:
   - provider data;
   - adapted internal representation;
   - official result authority;
   - fallback or manual override behavior;
   - reconciliation or correction behavior.
4. Check whether the proposal respects documented precedence between automatic sync and manual authority.
5. Identify traceability requirements for overrides, corrections, and contingency paths.
6. Call out failure modes:
   - missing provider data;
   - late data;
   - conflicting data;
   - manual correction;
   - downstream scoring or visibility impact.
7. If any precedence, authority, or contingency behavior is undocumented, stop and report the blocker instead of inventing it.

## Expected Output

Always return:

1. `Summary`
2. `Docs consulted`
3. `Constraints applied`
4. `Risks`
5. `Open questions or blocked decisions`
6. `Validation or tests needed`
7. `Authority and fallback impact`

If reviewing a proposal, say clearly whether provider data is being treated correctly as an input rather than as unqualified business truth.

## Stop Conditions

Stop and report a blocker if:

- the authority path cannot be identified;
- precedence between provider sync and manual override is unclear;
- contingency or reconciliation behavior is undocumented;
- the change would redefine official behavior based on provider convenience;
- downstream scoring or auditability impact cannot be evaluated from the docs.

## Non-Negotiable Rules

- Do not treat provider data as business truth by default.
- Do not invent undocumented fallback, override, or reconciliation precedence.
- Do not ignore traceability for manual correction or contingency paths.
- Do not separate integration behavior from its downstream effect on official state.
