---
name: scaffold-module
description: Task template for scaffolding a Quiniela backend module or frontend feature structure from documented architecture boundaries and ownership rules.
scope: "scaffolding tasks"
---

# Scaffold Module

## Purpose

Use this prompt to scaffold a backend module or frontend feature structure only when the intended ownership, scope, and boundaries can be tied to the official Quiniela documentation.

This prompt is for structure and setup. It is not permission to invent undocumented responsibilities or hidden architectural layers.

## Use This Prompt When

Use this prompt when:

- the task is to create a new module, feature folder, base structure, or implementation skeleton;
- the scope is architectural scaffolding rather than deep business logic;
- the intended owner area is documented;
- the main value is making the repository structure align with Quiniela boundaries.

## Do Not Use This Prompt When

Do not use this prompt when:

- the module or feature ownership is still unclear;
- the task is mainly about full implementation rather than structural setup;
- the architecture boundary depends on unresolved design decisions;
- the task would require inventing new layers, responsibilities, or cross-module ownership rules.

## Required Inputs

Before acting, gather:

- a short task summary;
- whether the scaffold is backend or frontend;
- the intended owner area;
- the in-scope responsibilities;
- any known no-go assumptions or excluded responsibilities;
- any data or integration boundaries that the structure must respect.

## Documentation to Review First

Canonical documentation repository:

- `https://github.com/dlop6/Quiniela-Documentacion`

Always read:

- `02_Arquitectura`

Also read the affected technical families such as:

- `03_Backend`
- `04_Frontend`
- `05_Datos` when data boundaries or invariants matter
- `06_Integraciones` when external provider structure matters

## Prompt Instructions

1. Identify the owning module, feature, or layer and state why it owns the scaffold.
2. List the documentation families consulted.
3. Define the intended scope and the out-of-scope responsibilities.
4. Describe the structural boundaries that must be preserved:
   - module ownership;
   - feature boundaries;
   - shared versus local responsibilities;
   - domain versus infrastructure responsibilities.
5. Propose the minimum scaffold needed to support documented behavior later.
6. Call out dependencies on data, integrations, security, or testing if the scaffold must reserve space for them.
7. If ownership or boundaries are unclear, stop and report the blocker instead of inventing architecture.

## Expected Output

Always return:

1. `Summary`
2. `Docs consulted`
3. `Constraints applied`
4. `Risks`
5. `Open questions or blocked decisions`
6. `Validation or tests needed`
7. `Proposed structure`

If scaffolding code or files, state clearly what is structural placeholder work versus documented implemented behavior.

## Stop Conditions

Stop and report a blocker if:

- the owner area cannot be identified;
- the scaffold would require undocumented module responsibilities or layering;
- the requested structure conflicts with documented architecture boundaries;
- the task needs a product or architecture decision that has not been closed.

## Non-Negotiable Rules

- Do not invent undocumented architecture.
- Do not hide ownership ambiguity behind generic shared folders.
- Do not mix structural scaffolding with undocumented business behavior.
- Do not ignore data, integration, security, or testing boundaries when they materially shape the scaffold.
