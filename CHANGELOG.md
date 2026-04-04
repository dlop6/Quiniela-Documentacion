# Changelog

## 2026-04-03

- Se adopta la estructura documental por secciones con subcarpetas `tex/`.
- Se incorpora plantilla compartida en LaTeX para documentos seccionales y PDF maestro.
- Se redacta el nucleo documental inicial de:
  - `00_Overview`
  - `01_Producto_y_Reglas`
  - `02_Arquitectura`
  - `05_Datos`
- Se crean los documentos base del resto de secciones con alcance y preguntas abiertas para iteracion posterior.
- Se oficializa el flujo de compilacion en `WSL Ubuntu` usando `latexmk` y `xelatex`.
- Se cierra en `01_02_Estados_y_Flujos` que `EN_REVISION` no es un estado de usuario y que la revision del comprobante pertenece al flujo administrativo de activacion.
- Se sincroniza `01_01_Reglamento_V1` con las decisiones ya cerradas sobre predicciones especiales y se eliminan preguntas abiertas resueltas.
- Se consolida en `10_03_Decisiones_Pendientes` el registro priorizado de pendientes activos de `01_Producto_y_Reglas`, con criticidad y documento duenio.
- Se alinea `05_Datos` con las reglas ya cerradas del dominio: predicciones especiales dentro de `Picks`, separacion entre estado de usuario y revision de pago, unicidad de resultado vigente y ausencia de pick sin valor implicito.
- Se formaliza la convencion visual del repo para `02_Arquitectura`, `03_Backend` y `04_Frontend`: usar diagramas cuando aporten claridad y al menos trees estructurales en backend y frontend.
- Se itera `02_01_Vision_Arquitectonica` con una vista logica de ejecucion, tree conceptual de repositorios y limites de responsabilidad entre frontend, backend, datos, integraciones y documentacion.
- Se itera `02_02_Arquitectura_Frontend` con capas funcionales, tree arquitectonico sugerido, criterios de uso de `shared` y limites entre presentacion, features y acceso a datos.
- Se itera `02_03_Arquitectura_Backend` con modulos de dominio esperados, capas internas, tree del modular monolith y limites entre dominio, aplicacion, infraestructura y servicios transversales.
- Se itera `02_04_Contratos_y_Limites` con matriz de dependencias permitidas, reglas de adaptacion en frontera y limites internos esperados para frontend y backend.
- Se itera `03_01_Modulos_y_Responsabilidades` con tree modular, ownership por modulo, entradas y salidas esperadas y reglas de colaboracion entre modulos backend.
- Se itera `03_02_Procesos_de_Negocio` con procesos backend de activacion, lock, sincronizacion de resultados, scoring, recalculo y archivado, incluyendo triggers, validaciones y resultados esperados.
- Se itera `03_03_Validacion_y_Errores` con validacion de frontera, taxonomia funcional vs tecnica, reglas de idempotencia y relacion entre error, log y auditoria.
- Se itera `03_04_Endpoints_Conceptuales` con operaciones publicas, autenticadas y administrativas agrupadas por modulo, con entrada y salida esperada a nivel conceptual.
