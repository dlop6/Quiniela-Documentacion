# Current Sprint Scope

## Snapshot

- Current sprint: `Sprint 2`
- Next planned sprint: `Sprint 3`
- Planning source: `10_Planeacion` plus closed decisions recorded for Sprint 1 and Sprint 3

## Current Execution Context

Sprint 2 is the active implementation sprint.
Its work is centered on activation, payment proof handling, administrative review, and state-based access restrictions.

Do not redefine Sprint 2 behavior from this file.
Use it only to avoid leaking Sprint 3 work into the wrong implementation slice.

## Sprint 3 Goal

Sprint 3 is intended to leave the functional base ready for:

- consulting matches;
- exposing official kickoff and visible match status;
- preparing the picks flow;
- preparing the technical base that Sprint 4 will use to close lock-sensitive behavior.

## In Scope For Backend In Sprint 3

- match queries for participant calendar or listing surfaces;
- exposure of official kickoff and visible match status;
- pick contract definition per match;
- create or update picks while the window is open;
- score range validation from `0` to `9`;
- `ACTIVO` user gating for pick operations;
- technical preparation for lock-sensitive validation;
- technical preparation for derived `SIN_PICK` handling.

## Out Of Scope For Sprint 3

- fully finished lock enforcement as a completed end-user experience;
- final `SIN_PICK` user experience;
- results ingestion and operational sync;
- scoring calculation;
- recalculation workflows;
- leaderboard behavior.

If a requested change would finish one of the above areas, stop and report the sprint-scope mismatch.

## Backend Constraints Derived From Closed Sprint Decisions

- Special predictions belong to the picks module, but they live in a separate visible flow from regular match picks.
- `Campeon` and `Goleador` remain hidden from other users until a later reveal rule is formally closed.
- Only `ACTIVO` users can create or update valid picks.
- `SIN_PICK` is derived from the absence of a valid pick at close, not from a fabricated pick entity.

## Boundary With Sprint 4

Sprint 3 may prepare lock-sensitive and absence-sensitive backend behavior, but it must not fully close:

- automatic lock behavior;
- finished post-lock editing rejection UX;
- final `SIN_PICK` presentation semantics.

Those belong to the next sprint boundary unless a newer planning decision explicitly changes that allocation.
