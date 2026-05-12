# Sprint 6 Jira Ticket Drafts (Local Only)

## Contexto

Estos drafts fueron preparados localmente y **no** han sido creados en Jira.

Baseline usado para redactarlos:
- `QuinielaBackend/main` en `2798835`
- `quinielahoras/main` en `0a1009e`
- `Quiniela-Documentacion/main` con decisiones cerradas de Sprint 6 ya integradas localmente

Límite de verificación:
- la app de Atlassian/Rovo no estuvo disponible en esta sesión, así que estos drafts no se deduplicaron contra un posible backlog ya existente de Sprint 6 dentro de Jira;
- por eso, antes de crear issues, conviene contrastar estos drafts contra el estado real del board.

## Convenciones de metadata

- `Epic`: `E3 Scoring and Leaderboard`
- `Sprint target`: `QH: Sprint 6`
- `Assignee`: vacío

---

## Ticket 1

**Summary**  
`Alinear consumo frontend de results con el contrato backend real`

**Type**  
Task

**Labels**  
`frontend`, `results`, `contract`, `hardening`, `sprint-6`

**Dependencies**  
None

### Objetivo

Dejar el consumo frontend de resultados oficiales alineado al contrato runtime real de backend para que la UI no dependa de campos o rutas no garantizados.

### Incluye

- `frontend`: ajustar el acceso a datos de `results` en `matches`.
- `frontend`: eliminar consumo de campos no expuestos formalmente por backend en `GET /matches/{matchId}/result`.
- `frontend`: reconciliar hooks, adapters y tipos de `results` con el contrato mínimo vigente.
- `testing`: actualizar o agregar cobertura mínima sobre adaptación de contrato en frontend.

### Criterios tecnicos

- La fuente de verdad funcional es el contrato backend ya cerrado para `GET /matches/{matchId}/result`.
- El frontend no debe asumir `isCorrected`, `hasImpact` ni metadata interna no formalizada.
- Las adaptaciones deben vivir en la capa de acceso a datos, no dispersas en componentes visuales.
- La cobertura mínima debe verificar respuesta con `result != null` y `result = null`.
- Si aparece un vacío contractual real, debe escalarse; no se debe asumir ni improvisar.

### Fuentes documentales

- `03_Backend/tex/03_04_Endpoints_Conceptuales.tex`
- `04_Frontend/tex/04_03_Estado_Validacion_y_Datos.tex`
- `12_Decisiones_por_Sprint/tex/12_05_Sprint_05_contenido.tex`

### No incluye

- Nuevos endpoints backend de `results`.
- Cambios a scoring o leaderboard.
- UX final de leaderboard.

### Criterios de aceptacion

- El frontend consume `GET /matches/{matchId}/result` sin depender de `isCorrected` ni `hasImpact`.
- Los tipos y adapters de `results` reflejan el contrato backend real vigente.
- Las consultas de `results` siguen funcionando con `result: null` y con resultado visible.
- No queda lógica de contrato paralela en componentes visuales.
- Si falta un campo para UX futura, el ticket lo deja reportado y no lo inventa.

---

## Ticket 2

**Summary**  
`Cerrar drift documental y funcional de results que impacta scoring`

**Type**  
Task

**Labels**  
`backend`, `results`, `documentation`, `hardening`, `sprint-6`

**Dependencies**  
None

### Objetivo

Dejar una sola verdad operativa para `results` en todo lo que afecta a scoring, evitando que backend, frontend y documentación trabajen sobre reglas contradictorias.

### Incluye

- `documentation`: identificar y ajustar drift vigente de `results` que afecte scoring o consumo competitivo.
- `backend`: verificar reglas runtime de `results` que impactan scoring y recálculo.
- `testing`: dejar evidencia mínima de la regla vigente cuando aplique.

### Criterios tecnicos

- La fuente de verdad funcional para Sprint 6 es la documentación vigente más reciente ya cerrada, junto con el runtime real del backend.
- El ticket debe enfocarse solo en inconsistencias que afecten scoring, leaderboard o consumo competitivo.
- No debe crear una tercera variante de contrato o regla.
- Si una diferencia no está cerrada documentalmente, debe escalarse antes de imponerla en código o docs.

### Fuentes documentales

- `12_Decisiones_por_Sprint/tex/12_05_Sprint_05_contenido.tex`
- `12_Decisiones_por_Sprint/tex/12_06_Sprint_06_contenido.tex`
- `03_Backend/tex/03_02_Procesos_de_Negocio.tex`
- `03_Backend/tex/03_04_Endpoints_Conceptuales.tex`

### No incluye

- Refactor completo del módulo `results`.
- Nuevas features de sync.
- Cambios de UX no relacionados con scoring o ranking.

### Criterios de aceptacion

- Las reglas de `results` que afectan scoring quedan consistentes entre documentación y runtime.
- No quedan contradicciones activas sobre consumo competitivo de `results`.
- Cualquier gap no resuelto queda explicitado como decisión abierta, no asumido.
- El ticket no amplía scope hacia proveedor, cron ni nuevas capacidades de `results`.

---

## Ticket 3

**Summary**  
`Implementar scoring oficial por partido desde resultado oficial vigente`

**Type**  
Task

**Labels**  
`backend`, `scoring`, `domain`, `sprint-6`

**Dependencies**  
Blocked by: `Cerrar drift documental y funcional de results que impacta scoring`

### Objetivo

Permitir que backend calcule el puntaje oficial de un pick por partido usando el resultado oficial vigente y las reglas de scoring ya cerradas.

### Incluye

- `backend`: implementar un caso de uso explícito en `scoring` para calcular el puntaje de un pick por partido.
- `backend`: consumir como input únicamente:
  - pick persistido del usuario
  - resultado oficial vigente del partido
  - fase oficial del partido
- `backend`: calcular outcome correcto, marcador exacto y multiplicador por fase según reglas ya cerradas.
- `backend`: producir un resultado de cálculo reproducible y consumible por la capa de persistencia posterior.
- `testing`: agregar cobertura unitaria e integración mínima para el cálculo por partido.

### Criterios tecnicos

- La fuente de verdad funcional es `01_04_Scoring_y_Desempates` y la verdad oficial vigente entregada por `results`.
- El ticket debe operar sobre la verdad oficial vigente ya resuelta por `results`; no debe volver a decidir precedencia entre `EXTERNAL` y `MANUAL_OVERRIDE`.
- La unidad de cálculo es `un usuario + un partido + un resultado oficial vigente`.
- El cálculo no debe redondear internamente el valor oficial.
- El cálculo debe diferenciar explícitamente:
  - sin pick válido
  - pick con outcome correcto
  - pick con marcador exacto
  - partido cancelado sin resultado utilizable
- Debe interpretar correctamente `SIN_PICK` como ausencia sin puntaje.
- La salida del caso de uso debe dejar claro, como mínimo:
  - `userId`
  - `matchId`
  - `points`
  - `reason` o desglose suficiente para persistencia posterior
- No debe recalcular leaderboard ni persistir ranking en este ticket.
- La cobertura mínima debe incluir:
  - outcome correcto sin exact score
  - exact score
  - multiplicador por fase
  - `SIN_PICK`
  - partido sin resultado utilizable
- Si aparece un vacío real en el shape del resultado interno, debe escalarse; no se debe asumir ni improvisar.

### Fuentes documentales

- `01_Producto_y_Reglas/tex/01_04_Scoring_y_Desempates.tex`
- `03_Backend/tex/03_02_Procesos_de_Negocio.tex`
- `12_Decisiones_por_Sprint/tex/12_06_Sprint_06_contenido.tex`

### No incluye

- Persistencia de `ScoreEntry`.
- Scoring de predicciones especiales.
- Recalculo.
- Endpoints HTTP públicos de scoring.
- Construcción de ranking agregado.

### Criterios de aceptacion

- Backend puede calcular el puntaje oficial de un pick por partido desde un resultado oficial vigente.
- El cálculo aplica 3 puntos por outcome, 2 por exact score y multiplicador por fase según documento dueño.
- La unidad de cálculo queda desacoplada de HTTP y de persistencia final.
- El valor oficial no se redondea en backend.
- Un partido sin pick válido no genera puntaje.
- Un partido cancelado sin resultado oficial utilizable no genera puntaje.
- Las pruebas cubren al menos outcome, exact score, multiplicador y `SIN_PICK`.

---

## Ticket 4

**Summary**  
`Persistir y actualizar ScoreEntry para picks por partido`

**Type**  
Task

**Labels**  
`backend`, `scoring`, `data`, `sprint-6`

**Dependencies**  
Blocked by: `Implementar scoring oficial por partido desde resultado oficial vigente`

### Objetivo

Persistir el puntaje derivado por partido en `ScoreEntry` para que el sistema pueda construir acumulados y leaderboard oficiales.

### Incluye

- `backend`: generar una sola fila `ScoreEntry` por combinación `userId + matchId`.
- `backend`: actualizar `ScoreEntry` cuando cambie el valor derivado de scoring por partido.
- `backend`: poblar como mínimo en `ScoreEntry`:
  - `userId`
  - `matchId`
  - `points`
  - `reason`
- `data`: usar el modelo `ScoreEntry` ya existente como almacenamiento oficial de puntaje derivado.
- `testing`: cubrir create, update e idempotencia de `ScoreEntry` por partido.

### Criterios tecnicos

- La fuente de verdad funcional es el cálculo oficial del módulo `scoring`.
- La persistencia debe respetar la unicidad por `userId + matchId`.
- La persistencia debe usar el modelo actual de `ScoreEntry` sin abrir una tabla paralela de acumulados en este ticket.
- El campo `reason` no debe quedar vacío; debe conservar contexto suficiente para trazabilidad funcional del origen del puntaje derivado.
- El ticket no debe duplicar puntos ni crear varias entradas para el mismo usuario/partido.
- Si ya existe una entrada para `userId + matchId`, el comportamiento es `update`, no `create`.
- Repetir una persistencia con el mismo valor no debe generar duplicación ni estados divergentes.
- La cobertura mínima debe probar:
  - create inicial
  - update sobre cambio de resultado oficial
  - repetición idempotente sin cambio

### Fuentes documentales

- `01_Producto_y_Reglas/tex/01_04_Scoring_y_Desempates.tex`
- `03_Backend/tex/03_02_Procesos_de_Negocio.tex`
- `05_Datos`

### No incluye

- Ranking agregado.
- Predicciones especiales.
- Recalculo global o selectivo.
- Agregados por usuario fuera de `ScoreEntry`.

### Criterios de aceptacion

- Se crea un `ScoreEntry` por pick de partido cuando existe score oficial derivable.
- Un cambio posterior en score del mismo partido actualiza la misma entrada en vez de duplicarla.
- La persistencia mantiene unicidad por usuario y partido.
- `reason` queda persistido con cada entrada creada o actualizada.
- Las pruebas cubren creación, actualización e idempotencia de `ScoreEntry`.

---

## Ticket 5

**Summary**  
`Implementar scoring oficial de predicciones especiales`

**Type**  
Task

**Labels**  
`backend`, `scoring`, `special-predictions`, `sprint-6`

**Dependencies**  
Can run in parallel with: `Persistir y actualizar ScoreEntry para picks por partido`

### Objetivo

Permitir que backend calcule y persista el puntaje oficial de `Campeon` y `Goleador` como parte del total competitivo del torneo.

### Incluye

- `backend`: implementar scoring de `Campeon` y `Goleador`.
- `backend`: persistir `ScoreEntry` asociado a `specialPredictionId`.
- `testing`: cubrir acierto y no acierto de predicciones especiales.

### Criterios tecnicos

- La fuente de verdad funcional es `01_04_Scoring_y_Desempates`.
- Los valores cerrados son 20 puntos para `Campeon` y 15 para `Goleador`.
- El ticket no redefine visibilidad social de esas predicciones.
- La cobertura mínima debe probar ambos tipos de predicción con caso positivo y negativo.

### Fuentes documentales

- `01_Producto_y_Reglas/tex/01_04_Scoring_y_Desempates.tex`
- `01_Producto_y_Reglas/tex/01_03_Picks_y_Locks.tex`
- `03_Backend/tex/03_02_Procesos_de_Negocio.tex`

### No incluye

- Exposición UI de breakdown detallado.
- Ranking filtrado.
- Regla de revelación social.

### Criterios de aceptacion

- Backend asigna 20 puntos a `Campeon` correcto y 15 puntos a `Goleador` correcto.
- Un fallo en predicción especial no genera puntaje.
- El puntaje de predicción especial se persiste en `ScoreEntry` usando `specialPredictionId`.
- Las pruebas cubren acierto y no acierto para ambos tipos.

---

## Ticket 6

**Summary**  
`Implementar recálculo selectivo e idempotente tras cambio de resultado oficial`

**Type**  
Task

**Labels**  
`backend`, `scoring`, `recalculation`, `sprint-6`

**Dependencies**  
Blocked by: `Persistir y actualizar ScoreEntry para picks por partido`

### Objetivo

Permitir que un cambio efectivo en resultado oficial dispare un recálculo selectivo del scoring afectado sin duplicar ni corromper puntajes.

### Incluye

- `backend`: identificar picks y participantes afectados por un partido cuyo resultado oficial cambió.
- `backend`: recomputar únicamente los `ScoreEntry` asociados a ese `matchId`.
- `backend`: dejar trazabilidad mínima del recálculo disparado por cambio de resultado.
- `backend`: exponer una unidad interna de recálculo reutilizable por `results` cuando haya cambio efectivo de valor oficial.
- `testing`: cubrir idempotencia y convergencia del recálculo selectivo.

### Criterios tecnicos

- La fuente de verdad funcional es `03_02_Procesos_de_Negocio` y la decisión cerrada de Sprint 6 sobre recálculo selectivo.
- El trigger del recálculo en este ticket es `cambio efectivo en resultado oficial vigente`, no simple lectura ni sync sin cambio.
- La unidad de recálculo es `un partido afectado` y su conjunto derivado de picks/score entries relacionados.
- El ticket no implementa recálculo global del torneo.
- El proceso debe ser idempotente frente a múltiples disparos sobre el mismo cambio efectivo.
- El proceso no debe reescribir score entries de partidos no afectados.
- La cobertura mínima debe probar:
  - cambio real de resultado con actualización de score entries
  - repetición sin duplicación
  - ausencia de impacto sobre partidos no afectados

### Fuentes documentales

- `03_Backend/tex/03_02_Procesos_de_Negocio.tex`
- `12_Decisiones_por_Sprint/tex/12_06_Sprint_06_contenido.tex`
- `01_Producto_y_Reglas/tex/01_04_Scoring_y_Desempates.tex`

### No incluye

- Endpoint global de recálculo admin.
- Leaderboard final.
- UX de notificación.
- Recalculo por torneo completo.

### Criterios de aceptacion

- Un cambio efectivo de resultado oficial recalcula solo el subconjunto afectado.
- El recálculo no duplica `ScoreEntry` ni deja estados divergentes.
- El recálculo no altera `ScoreEntry` de partidos no relacionados.
- Repetir el mismo disparo converge al mismo estado final.
- Existe trazabilidad mínima del evento de recálculo.

---

## Ticket 7

**Summary**  
`Exponer contrato backend de leaderboard general`

**Type**  
Task

**Labels**  
`backend`, `leaderboard`, `contract`, `sprint-6`

**Dependencies**  
Blocked by: `Implementar recálculo selectivo e idempotente tras cambio de resultado oficial`

### Objetivo

Exponer una lectura oficial del leaderboard general para que frontend consuma ranking y puntaje acumulado sin reinterpretar scoring.

### Incluye

- `backend`: definir y exponer contrato de leaderboard general.
- `backend`: entregar, por fila, como mínimo:
  - posición
  - identificador del participante
  - nombre visible del participante
  - puntaje oficial acumulado
- `backend`: construir el acumulado a partir de `ScoreEntry`, no de cálculo on-the-fly en frontend.
- `testing`: cubrir consulta de leaderboard general con más de un participante.

### Criterios tecnicos

- La fuente de verdad funcional es el acumulado derivado desde `ScoreEntry`.
- El contrato debe responder con ranking oficial, no con datos parciales sin contexto.
- La posición debe venir resuelta desde backend y ser consistente con el orden entregado.
- Este ticket cierra solo el contrato de leaderboard general; no mezcla filtros por fase o jornada.
- El ticket no debe delegar orden o acumulación al frontend.
- La cobertura mínima debe probar:
  - más de un participante
  - orden por puntaje acumulado
  - posición consistente con el orden de salida
- Si algún campo de identidad visible del participante no está disponible de forma estable, debe escalarse antes de asumirlo.

### Fuentes documentales

- `01_Producto_y_Reglas/tex/01_04_Scoring_y_Desempates.tex`
- `03_Backend/tex/03_04_Endpoints_Conceptuales.tex`
- `12_Decisiones_por_Sprint/tex/12_06_Sprint_06_contenido.tex`

### No incluye

- Filtros por fase o jornada.
- Desempates finales complejos fuera del orden base.
- UI frontend.
- Breakdown detallado por fila.

### Criterios de aceptacion

- Backend expone una lectura oficial de leaderboard general.
- La respuesta incluye al menos posición, participante identificable y puntaje acumulado oficial por fila.
- El ranking no depende de cálculo local en frontend.
- La posición entregada es consistente con el orden de respuesta.
- Las pruebas cubren un caso con múltiples participantes ordenados por score acumulado.

---

## Ticket 8

**Summary**  
`Aplicar desempates oficiales en el orden de leaderboard`

**Type**  
Task

**Labels**  
`backend`, `leaderboard`, `tie-break`, `sprint-6`

**Dependencies**  
Blocked by: `Exponer contrato backend de leaderboard general`

### Objetivo

Garantizar que backend resuelva el orden del leaderboard usando el algoritmo oficial de desempates ya cerrado por producto.

### Incluye

- `backend`: implementar orden de desempates oficial sobre ranking.
- `backend`: usar exact scores, outcomes, puntaje KO y fecha de registro según jerarquía cerrada.
- `testing`: cubrir al menos un empate resuelto por cada criterio principal.

### Criterios tecnicos

- La fuente de verdad funcional es `01_04_Scoring_y_Desempates`.
- El algoritmo debe ser determinista y reproducible.
- El ticket no redefine la fórmula de scoring.
- La cobertura mínima debe incluir escenarios de empate con resolución consistente.

### Fuentes documentales

- `01_Producto_y_Reglas/tex/01_04_Scoring_y_Desempates.tex`
- `03_Backend/tex/03_02_Procesos_de_Negocio.tex`

### No incluye

- Filtros de leaderboard.
- Cambios de UI.
- Desglose visual de criterios.

### Criterios de aceptacion

- Backend ordena el leaderboard aplicando la jerarquía oficial de desempates.
- Dos participantes empatados en puntaje no quedan en orden arbitrario.
- El orden es estable y reproducible ante la misma entrada.
- Las pruebas cubren al menos un caso resuelto por criterio de desempate.

---

## Ticket 9

**Summary**  
`Exponer contrato backend de leaderboard por fase y jornada`

**Type**  
Task

**Labels**  
`backend`, `leaderboard`, `filters`, `sprint-6`

**Dependencies**  
Blocked by: `Aplicar desempates oficiales en el orden de leaderboard`

### Objetivo

Exponer lecturas oficiales de leaderboard filtrado por fase y jornada usando como valor principal solo el puntaje atribuible al scope consultado.

### Incluye

- `backend`: soportar cortes de leaderboard por fase y por jornada.
- `backend`: calcular el puntaje principal de cada fila usando únicamente score entries atribuibles al filtro activo.
- `backend`: excluir del valor principal filtrado el puntaje de predicciones especiales.
- `backend`: mantener el mismo modelo base de fila del leaderboard general en la medida que el contrato lo permita.
- `testing`: cubrir al menos un filtro por fase y uno por jornada.

### Criterios tecnicos

- La fuente de verdad funcional es la decisión cerrada de Sprint 6 sobre leaderboard filtrado.
- El valor principal visible del ranking filtrado debe corresponder solo al scope consultado.
- La unidad de filtro debe salir de datos oficiales del dominio, no de agrupaciones inventadas en frontend.
- El ticket debe soportar dos tipos de corte distintos:
  - por fase
  - por jornada
- El ticket no debe mezclar total general con puntaje principal filtrado.
- El ticket no debe mezclar el contrato del ranking general con un valor principal filtrado ambiguo.
- La cobertura mínima debe verificar:
  - exclusión de picks especiales del valor principal
  - un caso por fase
  - un caso por jornada

### Fuentes documentales

- `01_Producto_y_Reglas/tex/01_04_Scoring_y_Desempates.tex`
- `03_Backend/tex/03_04_Endpoints_Conceptuales.tex`
- `12_Decisiones_por_Sprint/tex/12_06_Sprint_06_contenido.tex`

### No incluye

- UI de filtros.
- Breakdown visual frontend.
- Reglas nuevas de scoring.
- Vista general del total acumulado como dato principal de esta respuesta.

### Criterios de aceptacion

- Backend expone leaderboard filtrado por fase y por jornada.
- El valor principal del ranking filtrado excluye puntos de `Campeon` y `Goleador`.
- El ranking filtrado sigue siendo derivado oficial de backend.
- El contrato filtrado mantiene filas identificables para consumo frontend sin recomposición local.
- Las pruebas cubren al menos un filtro por fase y uno por jornada.

---

## Ticket 10

**Summary**  
`Construir pantalla de leaderboard en /leaderboard consumiendo ranking oficial de backend`

**Type**  
Task

**Labels**  
`frontend`, `leaderboard`, `ux`, `sprint-6`

**Dependencies**  
Blocked by: `Exponer contrato backend de leaderboard general`

### Objetivo

Permitir que el usuario autenticado consulte el leaderboard oficial desde la ruta `/leaderboard`, usando únicamente datos y orden entregados por backend.

### Incluye

- `frontend`: reemplazar el placeholder actual de `/leaderboard`.
- `frontend`: construir una sola pantalla de ranking con tres modos de consulta:
  - general
  - por fase
  - por jornada
- `frontend`: agregar controles visibles para cambiar entre esos modos sin navegar a rutas distintas.
- `frontend`: consumir el contrato backend oficial de leaderboard y adaptarlo en la capa de acceso a datos.
- `frontend`: renderizar estados mínimos obligatorios:
  - carga
  - vacío
  - error recuperable
  - lista con datos
- `frontend`: mostrar en cada fila, como mínimo:
  - posición
  - nombre visible del participante
  - puntaje oficial del scope consultado
- `testing`: cubrir render inicial, cambio de filtro, estado vacío y error recuperable.

### Criterios tecnicos

- La fuente de verdad funcional es el contrato backend oficial de leaderboard.
- La pantalla debe seguir la decisión cerrada de una sola pantalla con filtros para general, fase y jornada.
- La ruta de entrada del feature es `/leaderboard`; no deben crearse rutas separadas para general, fase y jornada en este ticket.
- El cambio entre filtros debe actualizar la misma pantalla, no montar páginas distintas.
- El frontend no puede ordenar ni recalcular ranking por su cuenta.
- Las adaptaciones de contrato deben vivir en la capa de servicios/hooks, no dentro de componentes visuales.
- Si backend todavía no expone breakdown, este ticket no lo inventa ni lo deriva localmente.
- La cobertura mínima debe incluir estados visibles mínimos: carga, vacío y error.
- Si aparece un vacío contractual real, debe escalarse; no se debe asumir ni improvisar.

### Fuentes documentales

- `04_Frontend/tex/04_02_Flujos_y_Paginas.tex`
- `04_Frontend/tex/04_03_Estado_Validacion_y_Datos.tex`
- `12_Decisiones_por_Sprint/tex/12_06_Sprint_06_contenido.tex`

### No incluye

- Desglose expandido de rendimiento.
- Nuevos contratos backend.
- Analytics o reportes.
- Nuevas rutas de leaderboard fuera de `/leaderboard`.

### Criterios de aceptacion

- La ruta `/leaderboard` deja de ser placeholder y muestra ranking oficial consumido desde backend.
- El usuario puede alternar entre general, fase y jornada desde la misma pantalla.
- Cada fila muestra al menos posición, participante y puntaje oficial del scope activo.
- La pantalla cubre carga, vacío y error recuperable.
- El orden mostrado coincide con el orden recibido desde backend, sin reordenamiento local.

---

## Ticket 11

**Summary**  
`Mostrar score visible y desglose compacto en leaderboard con el formato acordado`

**Type**  
Task

**Labels**  
`frontend`, `leaderboard`, `scoring`, `ux`, `sprint-6`

**Dependencies**  
Blocked by: `Construir la superficie única de leaderboard consumiendo backend oficial`

### Objetivo

Mostrar el puntaje del ranking con el formato visual cerrado para Sprint 6 y, cuando el contrato lo permita, un desglose compacto que no compita con el valor principal.

### Incluye

- `frontend`: renderizar el puntaje principal de cada fila del leaderboard con estas reglas:
  - si el valor es entero, mostrarlo sin decimales
  - si el valor tiene fracción, mostrar exactamente dos decimales
- `frontend`: mostrar un desglose compacto secundario solo si backend expone datos suficientes para ello.
- `frontend`: usar como dato principal:
  - en leaderboard general: total acumulado del torneo
  - en leaderboard por fase/jornada: puntaje oficial del filtro activo
- `frontend`: asegurar que el desglose, si existe, se muestre como texto secundario o bloque auxiliar, no como dato dominante de la fila.
- `testing`: cubrir formateo de enteros y fraccionarios, y presencia/ausencia controlada del desglose.

### Criterios tecnicos

- La fuente de verdad funcional es la decisión cerrada de Sprint 6 sobre precisión visual y desglose.
- La fuente de verdad del valor mostrado es backend; frontend no calcula score oficial.
- El frontend no debe redondear o truncar de modo que altere la lectura del valor oficial.
- El desglose no puede reemplazar al valor principal del ranking.
- Si backend no expone breakdown suficiente, la fila debe mostrar solo el puntaje principal y omitir el desglose sin fallback inventado.
- En vistas filtradas por fase o jornada, el puntaje principal no debe incluir picks especiales.
- Este ticket no debe usar helpers locales de scoring como fuente oficial de datos visibles.

### Fuentes documentales

- `01_Producto_y_Reglas/tex/01_04_Scoring_y_Desempates.tex`
- `04_Frontend/tex/04_02_Flujos_y_Paginas.tex`
- `04_Frontend/tex/04_03_Estado_Validacion_y_Datos.tex`
- `12_Decisiones_por_Sprint/tex/12_06_Sprint_06_contenido.tex`

### No incluye

- Reglas nuevas de scoring.
- Desglose expandido por partido o por predicción especial.
- Vista de perfil o detalle de rendimiento.
- Cambios a orden backend del leaderboard.
- Nuevos campos backend de breakdown si todavía no existen.

### Criterios de aceptacion

- Los puntajes enteros se muestran sin decimales visibles.
- Los puntajes fraccionarios se muestran con exactamente dos decimales visibles.
- En cada vista, el puntaje principal corresponde al scope activo del leaderboard.
- Si existe desglose visible, aparece como información secundaria y no reemplaza el puntaje principal.
- Si backend no entrega breakdown, la pantalla sigue funcionando sin inventar datos derivados en frontend.

---

## Ticket 12

**Summary**  
`Ajustar feedback visual tras corrección de resultado con impacto competitivo`

**Type**  
Task

**Labels**  
`frontend`, `results`, `scoring`, `ux`, `sprint-6`

**Dependencies**  
Blocked by: `Implementar recálculo selectivo e idempotente tras cambio de resultado oficial`

### Objetivo

Informar al usuario, de forma visible y consistente, cuando una corrección de resultado afecte el estado competitivo mostrado en calendario y leaderboard.

### Incluye

- `frontend`: mantener badge persistente en el partido corregido dentro de la vista de partidos/calendario.
- `frontend`: mostrar un aviso breve cuando una corrección cambie:
  - el puntaje visible del usuario
  - o su posición/ranking visible
- `frontend`: invalidar y refrescar, como mínimo:
  - resultado del partido afectado
  - lista/calendario donde aparece ese partido
  - score visible del usuario si ya existe en pantalla
  - leaderboard activo
- `frontend`: reconciliar la UI con los datos refrescados sin depender de recarga manual.
- `testing`: cubrir badge persistente, refresco de datos y aparición del aviso breve ante cambio competitivo efectivo.

### Criterios tecnicos

- La fuente de verdad funcional es la decisión cerrada de Sprint 6 sobre resultado corregido.
- La fuente de verdad del cambio competitivo es backend y el refresco posterior de datos oficiales.
- Este ticket no debe asumir `hasImpact`, `isCorrected` ni otros campos no formalizados si backend no los expone.
- El badge persistente pertenece al partido corregido; el aviso breve pertenece al evento de cambio competitivo visible.
- Debe invalidar al menos resultado del partido, calendario afectado, score visible y leaderboard activo.
- Si no existe una señal backend suficiente para disparar el aviso de forma determinista, el ticket debe degradarse de forma controlada y reportar el gap.
- La lógica de invalidación debe vivir en hooks/estado remoto, no dispersa en componentes visuales.
- La cobertura mínima debe probar reconciliación posterior a una corrección con cambio efectivo.

### Fuentes documentales

- `04_Frontend/tex/04_02_Flujos_y_Paginas.tex`
- `04_Frontend/tex/04_03_Estado_Validacion_y_Datos.tex`
- `12_Decisiones_por_Sprint/tex/12_06_Sprint_06_contenido.tex`

### No incluye

- Auditoría detallada visible.
- Diferenciar visualmente si la corrección vino de API o de override manual.
- Nuevas capacidades backend de `results`.
- Rediseñar la pantalla de calendario o leaderboard fuera de este feedback.

### Criterios de aceptacion

- Un partido corregido muestra badge persistente en la vista de partidos/calendario.
- Cuando una corrección cambia puntaje o ranking visible, el usuario recibe un aviso breve.
- Tras una corrección con cambio efectivo, calendario, resultado y leaderboard dejan de mostrar datos obsoletos.
- El flujo no depende de campos no garantizados por contrato backend.
- Si backend no expone señal suficiente para el aviso, el ticket deja el gap explícito y no lo resuelve por inferencia local.

---

## Ticket 13

**Summary**  
`Verificar contrato backend de leaderboard antes del consumo final frontend`

**Type**  
Task

**Labels**  
`backend`, `frontend`, `leaderboard`, `contract`, `hardening`, `sprint-6`

**Dependencies**  
Blocked by: `Exponer contrato backend de leaderboard por fase y jornada`

### Objetivo

Verificar y dejar alineado el contrato de leaderboard entre backend y frontend antes del cierre visual final de Sprint 6.

### Incluye

- `backend`: validar shape y consistencia del contrato expuesto por leaderboard.
- `frontend`: validar adaptación del contrato consumido por la UI.
- `testing`: dejar evidencia reproducible o automatizada del contrato acordado.

### Criterios tecnicos

- La fuente de verdad funcional es el contrato backend realmente expuesto.
- El ticket debe detectar drift antes de que la UI final se apoye en supuestos no cerrados.
- Si falta un campo crítico, debe escalarse explícitamente en vez de improvisarse en cliente.
- La validación mínima debe cubrir leaderboard general y al menos un corte filtrado.

### Fuentes documentales

- `03_Backend/tex/03_04_Endpoints_Conceptuales.tex`
- `04_Frontend/tex/04_03_Estado_Validacion_y_Datos.tex`
- `12_Decisiones_por_Sprint/tex/12_06_Sprint_06_contenido.tex`

### No incluye

- Rediseño visual de la pantalla.
- Nuevas reglas de scoring.
- Cambios de producto no cerrados.

### Criterios de aceptacion

- Backend y frontend usan el mismo contrato de leaderboard general.
- Backend y frontend usan el mismo contrato de al menos un leaderboard filtrado.
- No quedan campos asumidos solo por frontend para cerrar la experiencia.
- Cualquier gap no resuelto queda documentado y no se implementa por inferencia.

---

## Ticket 14

**Summary**  
`Verificar flujo end-to-end de resultado oficial a leaderboard visible`

**Type**  
Task

**Labels**  
`backend`, `frontend`, `e2e`, `validation`, `sprint-6`

**Dependencies**  
Blocked by: `Ajustar feedback competitivo tras resultado corregido`

### Objetivo

Validar que el ciclo competitivo completo funcione de punta a punta desde resultado oficial vigente hasta score y leaderboard visibles.

### Incluye

- `backend`: verificar integración entre `results`, `scoring` y `leaderboard`.
- `frontend`: verificar consumo visible de score y ranking oficiales.
- `testing`: ejecutar validación end-to-end o integración de alto nivel sobre el flujo competitivo completo.

### Criterios tecnicos

- La fuente de verdad funcional es el ciclo documentado de Sprint 6.
- La validación debe cubrir al menos: resultado oficial, recálculo derivado, score actualizado y leaderboard coherente.
- Debe incluir evidencia sobre corrección de resultado con consecuencia competitiva.
- Si el flujo depende de mocks o pasos manuales no cerrados, el ticket debe reportarlo explícitamente.

### Fuentes documentales

- `10_Planeacion/tex/10_01_Roadmap_y_Fases.tex`
- `03_Backend/tex/03_02_Procesos_de_Negocio.tex`
- `04_Frontend/tex/04_02_Flujos_y_Paginas.tex`
- `12_Decisiones_por_Sprint/tex/12_06_Sprint_06_contenido.tex`

### No incluye

- Nuevas features de scoring o leaderboard.
- Hardening general de release.
- Refactor de activación o Supabase.

### Criterios de aceptacion

- Existe evidencia de que un resultado oficial vigente impacta score derivado y ranking visible.
- Una corrección de resultado con cambio efectivo actualiza el estado competitivo mostrado.
- El flujo no depende de cálculo oficial en frontend.
- Los hallazgos de integración quedan cerrados o reportados explícitamente.
