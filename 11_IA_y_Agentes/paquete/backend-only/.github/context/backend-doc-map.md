# Backend Documentation Map

Use this map to decide which documentation families to read before acting.
Read only the minimum relevant families required by the task.

Official source:

- `https://github.com/dlop6/Quiniela-Documentacion`

## Backend Architecture And Module Ownership

Read:

- `02_Arquitectura`
- `03_Backend`
- `05_Datos`

Use for:

- module boundaries
- modular monolith decisions
- endpoint ownership
- controller/application/domain/infrastructure separation

## Auth, Roles, Activation, And Security

Read:

- `01_Producto_y_Reglas`
- `03_Backend`
- `07_Seguridad_y_Operacion`
- `08_Testing`

Use for:

- auth and session behavior
- role and permission changes
- activation flows
- protected admin behavior
- security-sensitive review

## Picks, Locks, And Special Predictions

Read:

- `01_Producto_y_Reglas`
- `03_Backend`
- `05_Datos`
- `08_Testing`

Use for:

- match picks
- lock-sensitive behavior
- `SIN_PICK`
- special predictions
- competitive correctness boundaries

## Results, Integrations, And Manual Override

Read:

- `03_Backend`
- `06_Integraciones`
- `07_Seguridad_y_Operacion`
- `08_Testing`

Use for:

- provider adaptation
- result synchronization
- manual override precedence
- reconciliation
- contingency and fallback behavior

## Scoring, Recalculation, And Leaderboard

Read:

- `01_Producto_y_Reglas`
- `03_Backend`
- `05_Datos`
- `08_Testing`

Use for:

- scoring logic
- recalculation
- leaderboard derivation
- tie-break-sensitive behavior

## Data, Schema, And Invariants

Read:

- `03_Backend`
- `05_Datos`
- `08_Testing`

Use for:

- schema evolution
- physical modeling
- invariants
- persistence review
- migration-sensitive work
