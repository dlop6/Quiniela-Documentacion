---
name: release-governance
description: Reusable workflow for assessing Quiniela release readiness, continuity risk, and governance quality for high-impact changes.
scope: "release and governance workflows"
---

# Release Governance

## Purpose

Use this skill to determine whether a high-impact Quiniela change is operationally ready enough to be treated as safe for release or high-confidence maintenance.

This skill exists to prevent changes from being treated as operationally safe when documentation, traceability, testing, or continuity expectations are still incomplete.

## Use This Skill When

Use this skill when:

- a change is high-impact or multi-area;
- release readiness is being assessed;
- governance-sensitive review is needed;
- continuity or operational integrity is the main concern;
- a go or no-go style judgment is required.

## Do Not Use This Skill When

Do not use this skill when:

- the task is ordinary implementation;
- the task is a standard PR review with no operational-readiness focus;
- the required release policy is undocumented and the request is really asking for policy invention;
- the change is narrow and low-risk enough that operational readiness is not the relevant question.

## Required Inputs

Before using this skill, identify:

- what changed;
- which areas are affected;
- what makes the change operationally sensitive;
- whether unresolved decisions, weak coverage, or traceability gaps are already known.

## Documentation to Review First

Canonical documentation repository:

- `https://github.com/dlop6/Quiniela-Documentacion`

Read first:

- `07_Seguridad_y_Operacion`
- `08_Testing`
- `10_Planeacion`

Then read the changed-area docs that matter.

## Workflow

1. Identify the operational question and why the change is high-impact.
2. Read operational, testing, and planning docs first.
3. Read the changed-area docs needed to understand risk and ownership.
4. Check for:
   - unresolved decisions;
   - weak or missing coverage;
   - weak traceability;
   - unclear fallback or continuity handling;
   - unclear ownership across areas;
   - security-sensitive unresolved behavior.
5. Decide whether the change can be framed as operationally safe, partially blocked, or not ready.
6. Report readiness and unresolved risk explicitly.

## Quality Checks

- The assessment is grounded in documentation rather than intuition.
- The output distinguishes implementation correctness from operational readiness.
- Missing documentation or missing validation is treated as real risk.
- High-impact areas are reviewed across boundaries, not in isolation.
- The skill does not invent release policy that the docs do not define.

## Expected Output

Always include:

1. `Summary`
2. `Docs consulted`
3. `Constraints applied`
4. `Risks`
5. `Open questions or blocked decisions`
6. `Validation or tests needed`
7. `Release or continuity risk`

Also include:

- `Readiness signals`
- `Blocking gaps`

## Stop Conditions

Stop and report a blocker if:

- release readiness cannot be assessed from the available documentation;
- the task depends on undocumented governance, continuity, or traceability policy;
- changed areas cannot be mapped to responsible docs or owners;
- a go or no-go judgment would require policy invention.

## Non-Negotiable Rules

- Do not approve operational readiness when critical documentation, traceability, or coverage is missing.
- Do not turn undocumented assumptions into release decisions.
- Do not confuse implementation progress with readiness.
- Do not ignore fallback, continuity, or cross-area risk for high-impact changes.
