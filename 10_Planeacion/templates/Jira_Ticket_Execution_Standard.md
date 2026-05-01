# Jira Ticket Execution Standard (Anti-Ambiguity)

## Purpose
Define exactly how tickets must be written so execution is clear, testable, and low-ambiguity across backend and frontend.

This standard is mandatory when creating or editing tickets.

## Required Template (Exact Sections)
Every ticket must include these sections in this order:
1. `Objetivo`
2. `Incluye`
3. `Criterios tecnicos`
4. `Fuentes documentales`
5. `No incluye`
6. `Criterios de aceptacion`

If a section is missing, the ticket is not ready.

## Writing Rules (Non-Negotiable)
1. One ticket = one primary responsibility.
2. Write observable behavior, not vague intent.
3. Use explicit domain terms already closed by product decisions.
4. Separate scope by `Incluye` and `No incluye` with hard boundaries.
5. Add only relevant dependencies that affect execution order.
6. Acceptance criteria must be verifiable (3 to 5 items).
7. Do not include unresolved product decisions as if they were closed.
8. Do not silently extend scope into future sprint goals.

## Section-by-Section Specification

### 1) Objetivo
- One sentence only.
- Must describe functional outcome visible to a consumer (user, admin, service, or integration).

### 2) Incluye
- Bullet list of what this ticket does implement.
- Must mention surface: `backend`, `frontend`, `admin`, `data`, `integrations`, `testing`, or `documentation`.
- Must mention module/flow/contract affected.
- Include dependency note only when it changes implementation order.

### 3) Criterios tecnicos
- State source of functional truth.
- State required validations/enforcement.
- State required traceability/audit/security if applicable.
- State minimum testing expectation.
- List open decisions only if they block implementation.

### 4) Fuentes documentales
- 2 to 5 references maximum.
- Use repository document families (01..12) and concrete files when available.
- No noisy references.

### 5) No incluye
- Explicitly list out-of-scope work.
- Mention what belongs to other tickets/sprints/hardening.

### 6) Criterios de aceptacion
- 3 to 5 bullet points.
- Each criterion must be:
  - observable
  - testable
  - bounded
- Avoid words like "correctly", "properly", "adequately" unless quantified.

## Quality Checklist Before Creating Ticket
- Title is specific and understandable without opening the description.
- Responsibility is singular (no hidden second project).
- Includes/Excludes are non-overlapping.
- Criteria técnicos reference real architecture/contracts.
- Acceptance criteria are executable as QA checks.
- Ticket has correct labels, epic, and dependency metadata.

## Dependency Rules
- Use `blocks/is blocked by` only for real blockers:
  - contract not available
  - endpoint/provider not available
  - schema/event model not available
  - admin workflow requires backend capability first
- Avoid artificial dependency chains.
- If two concerns can run in parallel, do not link them as blockers.

## Minimal Metadata Standard
Each ticket must include:
- clear summary
- labels (domain + sprint + type)
- epic link
- no assignee by default (unless explicitly requested)
- sprint target, or backlog fallback with note

## Reusable Prompt (Ticket Drafting)
Use this prompt as-is when drafting Jira tickets:

```md
Draft Jira tickets using the exact section template:
Objetivo, Incluye, Criterios tecnicos, Fuentes documentales, No incluye, Criterios de aceptacion.
Enforce one primary responsibility per ticket, explicit scope boundaries, and 3-5 testable acceptance criteria.
Use only closed domain terms and do not assume unresolved decisions.
Keep references to 2-5 authoritative documentation sources.
Add dependency metadata only when technically blocking.
```

