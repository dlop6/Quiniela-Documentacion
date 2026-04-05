# Jira Ticket Template

Use this template for implementation tickets that must stay aligned with the documentation repository.

Documentation source of truth:
- https://github.com/dlop6/Quiniela-Documentacion/tree/main

## Objetivo
Describe the functional outcome expected from the ticket.

## Alcance
- Describe what the ticket includes.
- Mention the main surfaces involved: `backend`, `frontend`, `data`, `admin`, `integrations`, `testing`, or `documentation`.

## Criterios tecnicos
- Mention architecture or implementation constraints that must be respected.
- Clarify where enforcement should live when relevant, for example:
  - backend is the source of truth
  - frontend reflects state but does not define business rules
  - validation happens at the boundary
  - critical actions require traceability or auditability
- Mention testing expectations when relevant.

## Fuentes documentales
Link only the documentation that governs the ticket. Prefer 2 to 5 references.

Suggested format:
- Product rules: `01_Producto_y_Reglas/...`
- Architecture: `02_Arquitectura/...`
- Backend: `03_Backend/...`
- Frontend: `04_Frontend/...`
- Data: `05_Datos/...`
- Integrations: `06_Integraciones/...`
- Security and operations: `07_Seguridad_y_Operacion/...`
- Testing: `08_Testing/...`
- UI and UX: `09_UI_UX/...`
- Planning: `10_Planeacion/...`

Example links:
- https://github.com/dlop6/Quiniela-Documentacion/blob/main/01_Producto_y_Reglas/tex/01_02_Estados_y_Flujos.tex
- https://github.com/dlop6/Quiniela-Documentacion/blob/main/03_Backend/tex/03_02_Procesos_de_Negocio.tex
- https://github.com/dlop6/Quiniela-Documentacion/blob/main/08_Testing/tex/08_02_Casos_Criticos_de_Prueba.tex

## Notas de implementacion
- Mention the responsible module, feature, or area when known.
- Mention dependencies on other tickets or flows.
- Call out open decisions that must not be assumed.
- Mention relevant technology or library constraints only when they materially affect the ticket.

## Fuera de alcance
- State what this ticket should not solve.
- Point to work that belongs in another ticket or sprint.

## Criterios de aceptacion
- Write 3 to 5 observable and testable conditions.
- Prefer concrete outcomes over vague statements.

Example:
- An authorized admin can approve a valid payment proof.
- A participant in `PENDIENTE` cannot execute gameplay actions.
- The UI reflects the updated functional account state after administrative review.
