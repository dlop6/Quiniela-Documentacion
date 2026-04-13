# Backend Decisions Index

This file summarizes closed backend decisions that the implementation repository must treat as non-negotiable unless a newer official decision explicitly changes them.

## Technology Baseline

- New implementation is written in `TypeScript`.
- Boundary validation uses `Zod`.
- The approved backend stack is:
  - `Node.js`
  - `Express`
  - `TypeScript`
  - `Zod`
  - `Prisma`
  - `Helmet`
  - `Pino`
  - `OpenTelemetry`
  - `Vitest`

## Auth And Session

- Auth is real in v1 and is not a scaffold-only area.
- The session strategy is access JWT plus refresh token.
- Social login is out of scope for v1.

## User Activation And States

- Activation is global in v1, not per tournament.
- Only `ACTIVO` users can play.
- Official user states are:
  - `PENDIENTE`
  - `ACTIVO`
  - `RECHAZADO`
  - `BLOQUEADO`
- Official state transitions are:
  - registration -> `PENDIENTE`
  - `PENDIENTE` -> `ACTIVO`
  - `PENDIENTE` -> `RECHAZADO`
  - `ACTIVO` -> `BLOQUEADO`

## Picks And Special Predictions

- `SIN_PICK` is a derived domain outcome, not a fabricated pick record.
- `Campeon` and `Goleador` are real v1 functionality, not structural placeholders.
- Special predictions live in a flow separate from regular match picks.
- `Campeon` and `Goleador` stay hidden from other users until a later reveal rule is formally closed.

## Persistence And Modeling

- The backend repository is separate from frontend.
- The backend architecture is a modular monolith by domain.
- Prisma physical modeling is allowed, but only with minimum real schema needed by the current slice.
- Real domain migrations are allowed.

## Admin And Audit

- Minimum admin surface includes:
  - users and payment proofs
  - matches and results
  - scoring, recalculation, and audit
- Critical admin actions require auditability with at least:
  - `timestamp`
  - actor
  - action
  - affected entity
  - previous value
  - new value
  - justification when applicable

## Error Taxonomy

The minimum backend error taxonomy includes:

- `validation_error`
- `authentication_required`
- `forbidden`
- `invalid_state`
- `lock_closed`
- `not_found`
- `conflict`
- `external_dependency_error`
- `internal_error`

## Testing Bar

Critical backend flows require meaningful validation before merge.
At minimum, this includes:

- activation
- picks lock behavior
- `SIN_PICK`
- special predictions
- scoring-sensitive behavior
- corrected results
- recalculation
- admin permissions
- archive-sensitive operations

## Source References

- Documentation repository: `https://github.com/dlop6/Quiniela-Documentacion`
- Sprint decision records in the documentation repository:
  - `12_Decisiones_por_Sprint` for Sprint 1 and Sprint 3 decisions
- Core backend-owning families:
  - `01_Producto_y_Reglas`
  - `03_Backend`
  - `07_Seguridad_y_Operacion`
  - `08_Testing`
