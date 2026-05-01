# Sprint Planning Playbook (No Assumptions)

## Purpose
Standardize sprint planning so any agent can produce consistent, non-ambiguous backlog proposals based on real evidence from:
- backend repository
- frontend repository
- documentation repository
- Jira state

This playbook is mandatory for Sprint planning from Sprint 5 onward.

## Non-Negotiable Rules
1. Do not assume implementation status from ticket names.
2. Do not assume implementation quality from `Done` status.
3. Do not assume missing code means missing requirement.
4. Classify by capability, not by ticket title.
5. Separate `carry-over` from `new sprint objective`.
6. Keep sprint goal stable unless product explicitly changes it.
7. Record decisions as `closed` or `open`; never invent closure.

## Required Inputs (Before Any Planning Output)
1. Backend repo `main` snapshot.
2. Frontend repo `main` snapshot.
3. Documentation repo `main` snapshot.
4. Jira sprint scope and issue status snapshot.
5. Closed sprint decisions that govern current planning.

If any input is inaccessible, planning must continue only with accessible inputs and must explicitly mark the gap as `Unverified`.

## Capability-First Planning Flow

### Step 1: Baseline Freeze
Build a one-page baseline with:
- what backend already has
- what frontend already has
- what docs declare as target
- what Jira marks as done/in progress/open

Output artifact: `Reality Snapshot`.

### Step 2: Capability Audit
Define capability groups first, then inspect each group in code and docs.

Status vocabulary (mandatory):
- `not present`
- `partial`
- `functionally present`
- `present but risky`
- `present but outside original sprint boundary`

Output artifact: `Backend Capability Audit` and `Frontend Capability Audit`.

### Step 3: Cross-Repo Absorption Matrix
Create one matrix with these columns:
- `capability`
- `roadmap sprint owner`
- `backend status`
- `frontend status`
- `jira status`
- `documentation status`
- `absorbed early (yes/no)`
- `needs hardening (yes/no)`
- `recommended action`

Output artifact: `Absorption Matrix` (single source for backlog decisions).

### Step 4: Classification for Ticketing
Each capability must be classified as one and only one:
- `carry-over sprint N`
- `new sprint N+1`
- `already absorbed`
- `hardening`
- `out of sprint`

Classification rules:
1. If roadmap owner is previous sprint and still missing: `carry-over`.
2. If roadmap owner is current sprint and missing: `new`.
3. If implemented but incomplete quality/contract: `hardening`.
4. If implemented and aligned end-to-end: `already absorbed` (no new feature ticket).
5. If belongs to future sprint objective: `out of sprint`.

### Step 5: Boundary Lock
Write a short boundary statement:
- what sprint includes
- what sprint excludes
- what is explicitly deferred

No ticket creation is allowed before boundary is written.

### Step 6: Backlog Construction
Build backlog in 5 buckets:
1. Backend build
2. Backend hardening
3. Frontend build
4. Frontend hardening
5. Cross-repo contract validation

Each candidate ticket must map to one capability row in the matrix.

### Step 7: Dependency Mapping
For each ticket define:
- `blocks`
- `is blocked by`

Rule: no dependency edge without a concrete technical reason (contract, endpoint availability, schema, admin flow, etc.).

### Step 8: Ticket Drafting Quality Gate
Before creating Jira issues, validate every draft against `Jira_Ticket_Execution_Standard.md`.

### Step 9: Jira Creation Protocol
1. Confirm project, epic, sprint availability.
2. Create tickets in dependency-safe order.
3. Do not set assignee unless explicitly requested.
4. If sprint assignment fails, create in backlog and mark `Sprint assignment pending`.
5. Create links after all tickets exist.
6. Run final verification query/checklist.

## Planning Deliverables (Required Order)
1. Reality Snapshot
2. Capability Audits (backend/frontend)
3. Absorption Matrix
4. Boundary Statement
5. Backlog Candidate (grouped by bucket)
6. Dependency Map
7. Ticket Drafts
8. Jira Creation Log
9. Final Validation Checklist

## Final Validation Checklist (Planning)
- Baseline comes from repos + docs + Jira, not assumptions.
- Carry-over items are separated from current sprint objective.
- No duplicate tickets for already absorbed capability.
- Hardening is separated from new feature build.
- Contract dependencies are explicit.
- Scope exclusions are explicit.
- All open product decisions are listed as open.

## Reusable Prompt (Planning Execution)
Use this prompt as-is when delegating sprint planning:

```md
Plan sprint backlog using only real evidence from backend repo, frontend repo, documentation repo, and Jira.
Do not assume implementation or completion from ticket titles/status.
Classify every capability into exactly one: carry-over, new, absorbed, hardening, out-of-sprint.
Produce in order: reality snapshot, backend/frontend capability audits, absorption matrix, sprint boundary statement, backlog candidates grouped by bucket, dependency map, and ticket drafts.
Use explicit scope exclusions and list open decisions separately.
No ticket proposal is allowed without capability-to-ticket traceability.
```

