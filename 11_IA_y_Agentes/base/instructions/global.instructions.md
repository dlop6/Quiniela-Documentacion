---
scope: "all tasks"
---

# Global Instructions

## Purpose

These instructions define the minimum always-on guardrails for any AI-assisted work in Quiniela implementation repositories.

They are intentionally lightweight.
They do not restate the full Quiniela documentation.
They exist to make sure the model reads the right source, stays within scope, and stops when the work would otherwise require guessing.

## Source of Truth Rule

- Treat `dlop6/Quiniela-Documentacion` as the source of truth.
- Canonical URL: `https://github.com/dlop6/Quiniela-Documentacion`
- Use GitHub MCP as the preferred way to read the documentation repository.
- If GitHub MCP is not available, use an approved read-only fallback.
- If documentation access is missing or unreliable, stop and report the blocker.

## Core Behavioral Rules

- Do not assume undocumented behavior.
- Do not silently close open decisions.
- Do not contradict documented product rules, architecture, data invariants, security constraints, or testing expectations.
- Do not duplicate the full documentation corpus inside implementation repositories.
- Read only the minimum document families required by the task.
- Escalate document lookup only when the task expands in scope.

## Task Discipline

- Identify the task type before choosing a workflow.
- Load the relevant scoped instructions first.
- Use the matching agent when the task clearly belongs to a specialist area.
- Use a skill for multi-step workflows.
- Use prompts as accelerators, not as replacements for documentation lookup.

## Required Documentation Behavior

Before implementing, reviewing, or refactoring:

1. Identify the primary document family that owns the behavior.
2. Confirm whether the task touches architecture, data, security, testing, or operations.
3. Read the minimum related documents needed to avoid guessing.
4. If a contradiction appears, stop and report it.

## Output Expectations

Unless the environment requires a different format, responses should include:

1. `Summary`
2. `Docs consulted`
3. `Constraints applied`
4. `Risks`
5. `Open questions` or `Blocked decisions`
6. `Validation or tests needed`

For implementation work, also include the affected module, feature, or workflow owner when it can be identified.

## Stop Conditions

Stop and report a blocker if:

- no primary document can be identified;
- a critical document is missing;
- the task requires an unresolved decision;
- two documents conflict in a way that changes implementation behavior;
- the requested change depends on undocumented behavior.

## Non-Negotiable Rules

- Do not move official business rules into UI for convenience.
- Do not bypass backend enforcement for permissions, locks, scoring, or auditability.
- Do not ignore security, testing, or traceability impact.
- Do not treat this kit as a substitute for the official documentation repository.
