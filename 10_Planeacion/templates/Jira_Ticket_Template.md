# Jira Ticket Template

Documentation source of truth:
- https://github.com/dlop6/Quiniela-Documentacion/tree/main

## Objetivo
Una frase corta con el outcome funcional del ticket.

## Incluye
- Qué sí construye, ajusta o valida este ticket.
- Superficie principal involucrada: `backend`, `frontend`, `admin`, `data`, `integrations`, `testing`, `documentation`.
- Módulo, flujo o contrato afectado cuando aplique.
- Dependencia relevante solo si cambia el orden de implementación.

## Criterios tecnicos
- Dónde vive la verdad funcional.
- Qué validación o enforcement debe respetarse.
- Qué trazabilidad, auditoría o seguridad aplica si corresponde.
- Qué nivel mínimo de prueba se espera.
- No asumir decisiones abiertas; mencionar solo las que realmente bloquean este ticket.

## Fuentes documentales
Usar solo de 2 a 5 referencias que gobiernan el ticket.

- `01_Producto_y_Reglas/...`
- `02_Arquitectura/...`
- `03_Backend/...`
- `04_Frontend/...`
- `05_Datos/...`
- `06_Integraciones/...`
- `07_Seguridad_y_Operacion/...`
- `08_Testing/...`
- `09_UI_UX/...`
- `10_Planeacion/...`

## No incluye
- Qué no debe resolver este ticket.
- Qué parte pertenece a otro ticket, otro sprint o a hardening posterior.

## Criterios de aceptacion
- Escribir de 3 a 5 condiciones observables y verificables.
- Preferir resultados concretos, no formulaciones vagas.
