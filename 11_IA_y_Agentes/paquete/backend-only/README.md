# Backend-Only Quiniela AI Kit

This package is a portable AI governance layer for the Quiniela backend repository.

It is designed to be copied into the root of the backend implementation repo and used by different LLMs, including Claude and Codex, without depending on one specific tool.

## What This Package Is

This package gives an AI model:

- the routing entrypoint;
- persistent backend guardrails;
- specialist agents;
- reusable workflow skills;
- task prompts;
- lightweight project context for current sprint scope and closed backend decisions.

It is not a copy of the full Quiniela documentation repository.
The official source of truth remains:

- `https://github.com/dlop6/Quiniela-Documentacion`

## Recommended Read Order

Always use this package in the following order:

1. `AGENTS.md`
2. `.github/context/current-sprint-scope.md`
3. The relevant thematic file in `.github/context/` or `.github/instructions/`
4. The relevant `.github/agents/`, `.github/skills/`, or `.github/prompts/` artifact

## Package Layout

- `AGENTS.md`: routing and governance entrypoint
- `.github/instructions/`: always-on backend guardrails
- `.github/agents/`: specialist role guidance
- `.github/skills/`: reusable workflows
- `.github/prompts/`: task accelerators
- `.github/context/`: current sprint and closed-decision context

## Operating Rules

- Read only the minimum relevant documentation families required by the task.
- Stay within the current sprint scope unless a newer approved scope explicitly expands it.
- If a kit file conflicts with a newer sprint decision, follow the newer sprint decision and report the mismatch for synchronization.
- Stop when implementation would require guessing a still-open decision.

## Copy Target

This folder is intended to be copied into the backend repository root as-is.
It should remain lightweight and should not be expanded into a local clone of the full documentation repository.
