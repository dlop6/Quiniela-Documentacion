# Sprint 7 Jira Ticket Drafts (No Ambiguedades)

## Reality Snapshot

Fecha: 2026-05-14
- Backend baseline: `QuinielaBackend/main` (`92f79e3`)
- Frontend baseline: `quinielahoras/main` (`0a1009e`)
- Docs baseline: `Quiniela-Documentacion/main` (`0fcdf8b`)

Supuesto explicitamente autorizado para planning:
- Tratar migracion Supabase como absorbida/mergeada para Sprint 7.

Scope Sprint 7 (roadmap):
- seguridad minima v1
- estabilizacion de flujos criticos
- cierre de huecos de accesibilidad/estados visibles
- refactor comprobantes hacia Google Forms

## Boundary

Incluye:
- contrato y flujo backend/frontend para comprobantes via Google Forms
- hardening atomico de seguridad (auth/payments)
- estabilizacion activacion end-to-end
- estados visibles y baseline de accesibilidad en activacion

No incluye:
- nuevas features de scoring/leaderboard
- cambios de reglas de negocio fuera de activacion
- creacion de tickets en Jira (solo drafts)

## Dependency Map (alto nivel)

- `QH-XX01` bloquea `QH-XX02` y `QH-XX04`
- `QH-XX02` bloquea `QH-XX03`
- `QH-XX04` bloquea `QH-XX05`
- `QH-XX03` y `QH-XX05` bloquean `QH-XX12`
- `QH-XX13` depende de `QH-XX12`
- `QH-XX14` puede iniciar en paralelo
- `QH-XX15` depende de `QH-XX14`
- seguridad atomica (`QH-XX06` a `QH-XX11`) corre en paralelo

---

## QH-XX01 - Definir contrato backend para recibir comprobantes desde Google Forms

### Objetivo
Definir un contrato backend unico para ingestar respuestas de Google Forms y transformarlas al contrato interno vigente de `PaymentProof`.

### Incluye
- `backend`: definir `POST /payments/proofs/forms-ingest` como endpoint administrativo interno.
- `backend`: definir payload de entrada de Forms y reglas de normalizacion.
- `backend`: definir transformacion deterministica hacia `PaymentProofSubmitRequest` vigente.
- `testing`: pruebas unitarias de validacion y transformacion.

### Criterios tecnicos
- Endpoint: `POST /payments/proofs/forms-ingest`.
- Autorizacion: `ADMIN` o `SUPER_ADMIN` (mismo criterio que revision de comprobantes).
- Payload obligatorio de Forms:
  - `referenceCode: string` (trim, uppercase, 1..64)
  - `registeredEmail: string` (trim, lowercase, email valido)
  - `depositDate: string` (`YYYY-MM-DD`)
  - `amount: string` (decimal positivo con max 2 decimales)
  - `paymentIdentifier: string` (trim, 1..80)
  - `evidenceUrl: string` (URL valida)
  - `submittedAt: string` (datetime ISO-8601)
- Mapeo cerrado hacia contrato interno existente:
  - `acceptedTerms = true`
  - `termsVersion = "forms-v1"`
  - `storageKey = "forms/" + referenceCode + "/" + sha256(evidenceUrl)`
  - `fileName = paymentIdentifier + ".url"`
  - `mimeType = "application/pdf"` (valor tecnico fijo para mantener compatibilidad schema actual)
  - `fileSizeBytes = 1` (sentinela tecnico fijo)
  - `fileHash = sha256(referenceCode + "|" + registeredEmail + "|" + amount + "|" + depositDate + "|" + paymentIdentifier + "|" + evidenceUrl)`
- Error de formato/campo faltante: `400 validation_error`.
- Error sin permisos: `403 forbidden`.
- No crea ni cambia estado funcional en este ticket.

### Fuentes documentales
- `03_Backend/tex/03_02_Procesos_de_Negocio.tex`
- `03_Backend/tex/03_01_Modulos_y_Responsabilidades.tex`
- `QuinielaBackend/src/modules/payments/http/schemas.ts`
- `QuinielaBackend/prisma/schema.prisma`

### No incluye
- persistencia de `PaymentProof` final
- resolucion admin
- cambios de schema Prisma

### Criterios de aceptacion
- Existe contrato tecnico cerrado para `/payments/proofs/forms-ingest`.
- Payload incompleto o invalido devuelve `400 validation_error`.
- Transformacion produce exactamente los 7 campos del `PaymentProofSubmitRequest` vigente.
- Pruebas cubren al menos 1 caso valido y 3 invalidos.

---

## QH-XX02 - Guardar respuestas validas de Google Forms como comprobantes revisables

### Objetivo
Persistir respuestas validas de Google Forms como `PaymentProof` en estado `PENDIENTE` reutilizando el flujo de `submitPaymentProof` actual.

### Incluye
- `backend`: ejecutar transformacion definida en `QH-XX01`.
- `backend`: llamar el caso de uso existente `submitPaymentProof` con actor administrativo designado.
- `backend`: persistir en `PaymentProof` con modelo actual (`storageKey`, `fileName`, `mimeType`, `fileSizeBytes`, `fileHash`).
- `audit`: registrar evento tecnico de ingest Forms.
- `testing`: pruebas de create y conflicto por pendiente existente.

### Criterios tecnicos
- El usuario target se resuelve por `registeredEmail` (normalizado).
- Si usuario no existe: `404 not_found`.
- Si usuario existe pero `status != PENDIENTE`: `400 invalid_state` (se hereda regla de `submitPaymentProof`).
- Si ya existe comprobante pendiente para el usuario: `409 conflict`.
- Se mantiene regla actual: solo un comprobante `PENDIENTE` por usuario.
- Evento de auditoria obligatorio: `PAYMENT_PROOF_FORMS_INGESTED` con `entityType = PaymentProof` y `entityId = paymentProof.id`.
- No cambia estado funcional del usuario.

### Fuentes documentales
- `QuinielaBackend/src/modules/payments/application/payments-service.ts`
- `QuinielaBackend/src/modules/payments/application/payment-proof-repository.ts`
- `QuinielaBackend/src/modules/users/application/user-account-repository.ts`
- `05_Datos/tex/05_04_Auditoria_y_Trazabilidad.tex`

### No incluye
- aprobacion/rechazo
- notificaciones
- cambios de contrato de review

### Criterios de aceptacion
- Ingest valido crea `PaymentProof` con `status = PENDIENTE`.
- Usuario inexistente responde `404 not_found`.
- Usuario con pendiente existente responde `409 conflict`.
- Se registra auditoria `PAYMENT_PROOF_FORMS_INGESTED`.

---

## QH-XX03 - Permitir que admin apruebe o rechace comprobantes de Google Forms y actualice estado del usuario

### Objetivo
Habilitar revision administrativa de comprobantes ingeridos desde Google Forms con actualizacion de estado funcional del usuario.

### Incluye
- `backend`: resolucion `APROBADO`/`RECHAZADO` (enums vigentes).
- `backend`: sincronizar `PaymentProof.status` con estado funcional de usuario.
- `audit`: evento de resolucion admin.
- `testing`: aprobar, rechazar y doble resolucion.

### Criterios tecnicos
- Solo `ADMIN`/`SUPER_ADMIN` pueden resolver.
- Payload de review se mantiene como contrato actual:
  - `resolution: "APROBADO" | "RECHAZADO"`
  - `reviewReason?: string` (obligatorio si `RECHAZADO`, max 500)
- `APROBADO` => usuario `ACTIVO`.
- `RECHAZADO` => usuario `RECHAZADO`.
- Resolver evidencia ya resuelta => `409 conflict`.
- Si usuario target no esta `PENDIENTE` al resolver => `409 conflict`.
- Eventos auditoria obligatorios existentes:
  - `PAYMENT_PROOF_APPROVED`
  - `PAYMENT_PROOF_REJECTED`

### Fuentes documentales
- `QuinielaBackend/src/modules/payments/http/schemas.ts`
- `QuinielaBackend/src/modules/payments/application/payments-service.ts`
- `01_Producto_y_Reglas/tex/01_02_Estados_y_Flujos.tex`

### No incluye
- reglas antifraude avanzadas
- autoaprobacion
- frontend

### Criterios de aceptacion
- Admin puede aprobar y activar (`APROBADO -> ACTIVO`).
- Admin puede rechazar (`RECHAZADO -> RECHAZADO` usuario).
- `RECHAZADO` sin `reviewReason` devuelve `400 validation_error`.
- Segunda resolucion de mismo comprobante devuelve `409 conflict`.

---

## QH-XX04 - Cambiar frontend de activacion para usar Google Forms en lugar de subir archivo en la app

### Objetivo
Reemplazar el flujo de upload interno por flujo de formulario externo oficial manteniendo plataforma como punto principal de estado.

### Incluye
- `frontend`: remover upload interno en activacion.
- `frontend`: mostrar referencia canonica, instrucciones de deposito y CTA al formulario oficial.
- `frontend`: mantener estados visibles oficiales.
- `testing`: pruebas de render por estado.

### Criterios tecnicos
- Mensaje obligatorio: enviar formulario no activa automaticamente la cuenta.
- CTA unico hacia formulario oficial.
- No inventar estado funcional nuevo.

### Fuentes documentales
- `04_Frontend/tex/04_02_Flujos_y_Paginas.tex`
- `04_Frontend/tex/04_03_Estado_Validacion_y_Datos.tex`
- `10_Planeacion/tex/10_01_Roadmap_y_Fases.tex`

### No incluye
- backend ingest
- marketing copy externo
- scoring

### Criterios de aceptacion
- No existe upload interno en activacion.
- Existe CTA a formulario oficial.
- Texto de no-activacion automatica visible.

---

## QH-XX05 - Actualizar estado visible de activacion despues de enviar el formulario externo

### Objetivo
Asegurar que frontend reconcilie estado remoto de activacion tras envio de formulario externo.

### Incluye
- `frontend`: invalidacion/refetch de estado.
- `frontend`: mensajes de espera/error consistentes.
- `testing`: prueba de transicion y error.

### Criterios tecnicos
- Sin senal backend extra, no crear estado funcional nuevo.
- Si fallo de consulta: mostrar feedback recuperable y opcion de reintento.

### Fuentes documentales
- `04_Frontend/tex/04_03_Estado_Validacion_y_Datos.tex`
- `01_Producto_y_Reglas/tex/01_02_Estados_y_Flujos.tex`

### No incluye
- nuevos endpoints
- cambios de roles/permisos

### Criterios de aceptacion
- Estado visible se actualiza sin recarga manual obligatoria.
- Errores de sincronizacion son visibles y recuperables.

---

## QH-XX06 - Estandarizar errores publicos en endpoints sensibles de auth

### Objetivo
Unificar taxonomia de errores publicos en endpoints de auth para salida v1.

### Incluye
- `backend`: mapa de errores publico para superficie auth vigente.
- `testing`: pruebas negativas por codigo esperado en rutas protegidas por middleware auth.

### Criterios tecnicos
- Superficie minima obligatoria: `GET /auth/me` y middlewares `requireAuthenticatedUser`, `requirePlayableUser`, `requireAdministrativeUser`.
- Codigos publicos cerrados en auth:
  - `authentication_required`: credencial ausente, bearer invalido o token no verificable.
  - `forbidden`: rol/estado no autorizado para superficie solicitada.
  - `validation_error`: formato de entrada invalido cuando aplique en rutas auth relacionadas.
  - `invalid_state`: estado funcional incompatible cuando aplique por regla de negocio.
  - `conflict`: solo si existe colision de estado en flujo auth relacionado.
- No exponer stack trace ni detalles internos.

### Fuentes documentales
- `07_Seguridad_y_Operacion`
- `03_Backend/tex/03_04_Endpoints_Conceptuales.tex`

### No incluye
- cambio de proveedor auth
- rediseño de auth

### Criterios de aceptacion
- Endpoints auth retornan codigos publicos canonicos.
- Pruebas negativas validan al menos `authentication_required` y `forbidden` en `/auth/me`/middlewares.
- Cuando aplique en auth relacionado, `invalid_state` y `conflict` usan codigo publico correcto.

---

## QH-XX07 - Estandarizar errores publicos en endpoints sensibles de payments

### Objetivo
Unificar taxonomia de errores publicos en endpoints de payments para flujo de comprobantes.

### Incluye
- `backend`: mapa de errores publico en payments.
- `testing`: pruebas negativas por codigo esperado.

### Criterios tecnicos
- Superficie minima obligatoria: `POST /payments/proofs`, `PATCH /payments/proofs/{paymentProofId}/review`, y `POST /payments/proofs/forms-ingest` cuando exista.
- Codigos publicos cerrados en payments:
  - `validation_error`
  - `forbidden`
  - `invalid_state`
  - `conflict`
  - `not_found`
  - `internal_error`
- `conflict`: doble resolucion o regla idempotente violada.
- Sin detalle sensible de evidencia en respuesta publica.

### Fuentes documentales
- `03_Backend/tex/03_04_Endpoints_Conceptuales.tex`
- `05_Datos/tex/05_04_Auditoria_y_Trazabilidad.tex`

### No incluye
- cambios de UI
- cambios de modelo de dominio

### Criterios de aceptacion
- Endpoints payments responden codigos cerrados y consistentes.
- Pruebas negativas validan al menos `validation_error`, `invalid_state`, `conflict` y `forbidden`.

---

## QH-XX08 - Evitar exposicion de tokens y datos sensibles en logs de auth

### Objetivo
Eliminar fugas de secretos en logging de auth.

### Incluye
- `backend`: redaccion de logs en auth.
- `testing`: verificacion de no-fuga en pruebas/log snapshots.

### Criterios tecnicos
- No loggear bearer token completo.
- No loggear claims sensibles completos.
- Permitido: identificadores tecnicos truncados/no reversibles.

### Fuentes documentales
- `07_Seguridad_y_Operacion/tex/07_03_Logging_Observabilidad_y_Auditoria.tex`

### No incluye
- SIEM externo
- observabilidad avanzada

### Criterios de aceptacion
- Logs auth no contienen tokens completos.
- Verificacion automatizada de no-fuga pasa.

---

## QH-XX09 - Evitar exposicion de datos sensibles de comprobantes en logs de payments

### Objetivo
Eliminar fuga de datos sensibles de comprobantes en logs operativos.

### Incluye
- `backend`: redaccion de metadata de comprobantes.
- `testing`: verificacion de no-fuga.

### Criterios tecnicos
- No loggear evidencia completa, no base64, no URLs firmadas completas.
- No loggear datos bancarios completos.
- Permitido: ids internos y hash/fragmento corto.

### Fuentes documentales
- `05_Datos/tex/05_04_Auditoria_y_Trazabilidad.tex`
- `07_Seguridad_y_Operacion`

### No incluye
- cifrado de storage externo
- politicas legales externas

### Criterios de aceptacion
- Logs payments enmascaran datos sensibles.
- Pruebas/validaciones de no-fuga pasan.

---

## QH-XX10 - Pruebas negativas minimas de autorizacion en auth

### Objetivo
Cubrir escenarios de autorizacion fallida en auth con resultados esperados y estables.

### Incluye
- `testing`: casos negativos cerrados sobre `GET /auth/me` y middlewares auth reutilizados por rutas protegidas.

### Criterios tecnicos
- Casos minimos obligatorios:
  1. sin header `Authorization` => `401 authentication_required`
  2. bearer invalido => `401 authentication_required`
  3. usuario autenticado sin permiso admin en ruta admin protegida => `403 forbidden`
  4. usuario autenticado no `ACTIVO` en ruta jugable protegida => `403 forbidden`
- Cada caso valida codigo publico exacto.
- Debe correr en CI sin dependencia manual.

### Fuentes documentales
- `03_Backend/tex/03_04_Endpoints_Conceptuales.tex`
- `07_Seguridad_y_Operacion`

### No incluye
- e2e browser

### Criterios de aceptacion
- Suite negativa auth en verde.
- Cubre los 4 casos minimos obligatorios definidos arriba.

---

## QH-XX11 - Pruebas negativas minimas de validacion en payments

### Objetivo
Cubrir escenarios de payload invalido y conflicto en payments para flujo de comprobantes.

### Incluye
- `testing`: payload faltante, formato invalido, doble resolucion, acceso sin permiso.

### Criterios tecnicos
- Casos minimos obligatorios:
  1. `POST /payments/proofs` con payload invalido => `400 validation_error`
  2. `POST /payments/proofs` para usuario no `PENDIENTE` => `400 invalid_state`
  3. `POST /payments/proofs` con pendiente existente => `409 conflict`
  4. `PATCH /payments/proofs/{paymentProofId}/review` con `RECHAZADO` sin `reviewReason` => `400 validation_error`
  5. `PATCH /payments/proofs/{paymentProofId}/review` sobre comprobante ya resuelto => `409 conflict`
- Si `forms-ingest` ya existe en la rama del ticket, agregar:
  6. payload invalido en ingest => `400 validation_error`
  7. colision idempotente definida por ingest => codigo cerrado segun `QH-XX02`
- Cada caso valida codigo publico exacto.

### Fuentes documentales
- `03_Backend/tex/03_04_Endpoints_Conceptuales.tex`
- `05_Datos/tex/05_04_Auditoria_y_Trazabilidad.tex`

### No incluye
- pruebas de carga

### Criterios de aceptacion
- Suite negativa payments en verde.
- Cubre 5 casos minimos obligatorios; si existe `forms-ingest`, cubre tambien los 2 casos adicionales.

---

## QH-XX12 - Validacion end-to-end de activacion: registro, evidencia, revision admin y acceso

### Objetivo
Validar flujo critico completo de activacion con evidencia por Google Forms y resolucion administrativa.

### Incluye
- `backend`: validacion operativa del ciclo completo.
- `frontend`: verificacion de consumo y estados visibles.
- `testing`: evidencia reproducible (script/checklist ejecutable).

### Criterios tecnicos
- Camino obligatorio: usuario `PENDIENTE` -> ingest evidencia -> review admin -> usuario `ACTIVO` -> acceso permitido.
- Sin bypass de activacion.

### Fuentes documentales
- `03_Backend/tex/03_02_Procesos_de_Negocio.tex`
- `04_Frontend/tex/04_02_Flujos_y_Paginas.tex`
- `10_Planeacion/tex/10_01_Roadmap_y_Fases.tex`

### No incluye
- nuevas features

### Criterios de aceptacion
- Flujo completo ejecutado con evidencia reproducible.
- Se valida cambio de estado y acceso final.

---

## QH-XX13 - Corregir inconsistencias de estado funcional entre backend y frontend en activacion

### Objetivo
Eliminar diferencias de interpretacion de estado funcional entre backend y frontend dentro del flujo de activacion.

### Incluye
- `backend`: confirmar y exponer semantica unica de estados.
- `frontend`: alinear rendering y guardias a esa semantica.
- `testing`: pruebas de consistencia de estados.

### Criterios tecnicos
- Estados oficiales cerrados: `PENDIENTE`, `ACTIVO`, `RECHAZADO`, `BLOQUEADO`.
- Prohibido introducir `EN_REVISION` como estado funcional.
- Mismo estado debe producir misma restriccion en toda la app.

### Fuentes documentales
- `01_Producto_y_Reglas/tex/01_02_Estados_y_Flujos.tex`
- `04_Frontend/tex/04_03_Estado_Validacion_y_Datos.tex`

### No incluye
- nuevos estados

### Criterios de aceptacion
- No hay drift backend/frontend en estados funcionales.
- Pruebas de estados quedan en verde.

---

## QH-XX14 - Unificar mensajes y estados visibles en pantallas de activacion

### Objetivo
Estandarizar mensajes visibles de activacion para reducir ambiguedad del usuario.

### Incluye
- `frontend`: unificar copy de pendiente, rechazo, bloqueo y activo.
- `frontend`: definir mensaje canonico de “formulario enviado no activa automaticamente”.
- `testing`: validacion de presencia de copy por estado.

### Criterios tecnicos
- Mensajes deben mapear 1:1 a estados oficiales.
- Debe existir CTA claro por estado.

### Fuentes documentales
- `04_Frontend/tex/04_02_Flujos_y_Paginas.tex`
- `01_Producto_y_Reglas/tex/01_02_Estados_y_Flujos.tex`

### No incluye
- rediseño visual completo

### Criterios de aceptacion
- Mensajes de activacion son consistentes en todas las pantallas del flujo.
- CTA por estado es claro y no contradictorio.

---

## QH-XX15 - Aplicar baseline minimo de accesibilidad en pantallas criticas de activacion

### Objetivo
Aplicar controles minimos de accesibilidad en pantallas de activacion antes de release candidate.

### Incluye
- `frontend`: contraste minimo legible en mensajes y CTAs.
- `frontend`: foco visible y orden de tab en controles clave.
- `frontend`: etiquetas accesibles en elementos interactivos.
- `testing`: checklist a11y minimo ejecutable.

### Criterios tecnicos
- Controles clave navegables por teclado.
- Feedback de error legible para lector/usuario visual.
- Checklist minimo definido y versionado en repo.

### Fuentes documentales
- `09_UI_UX`
- `04_Frontend/tex/04_02_Flujos_y_Paginas.tex`

### No incluye
- auditoria WCAG completa
- rediseño total de UI

### Criterios de aceptacion
- Checklist a11y minimo pasa en pantallas criticas de activacion.
- Navegacion por teclado y foco visible verificados.

---

## Trazabilidad Capacidad -> Ticket

- Google Forms backend contrato/ingest/review: `QH-XX01`, `QH-XX02`, `QH-XX03`
- Google Forms frontend flujo/estado: `QH-XX04`, `QH-XX05`
- Seguridad minima v1 atomica: `QH-XX06` a `QH-XX11`
- Estabilizacion flujo critico: `QH-XX12`, `QH-XX13`
- Estados visibles y accesibilidad: `QH-XX14`, `QH-XX15`
