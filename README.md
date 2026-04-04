# Quiniela Documentacion

Repositorio exclusivo para la documentacion tecnica del proyecto `Quiniela`.

## Objetivo

Este repositorio es la fuente de verdad documental del proyecto. Su funcion es:

- formalizar reglas de negocio;
- definir el dominio;
- describir arquitectura, limites y flujos;
- documentar criterios operativos y de seguridad;
- servir como base de trabajo para frontend y backend.

No se utiliza para implementar backend ni frontend.

## Jerarquia de fuentes

Cuando exista contradiccion entre documentos o fuentes, manda lo siguiente:

1. La decision mas reciente del responsable del proyecto.
2. El registro documental de este repositorio.
3. El PRD.
4. El backlog/UI-UX como apoyo de detalle.

## Estructura del repositorio

Cada carpeta principal sigue la misma convencion:

- `tex/` contiene las fuentes editables en LaTeX.
- el PDF final de la seccion vive en la raiz de la carpeta.

Estructura general:

- `00_Overview`
- `01_Producto_y_Reglas`
- `02_Arquitectura`
- `03_Backend`
- `04_Frontend`
- `05_Datos`
- `06_Integraciones`
- `07_Seguridad_y_Operacion`
- `08_Testing`
- `09_UI_UX`
- `10_Planeacion`
- `tex/` para el documento maestro y la plantilla compartida

## Convenciones documentales

- Idioma oficial: espanol.
- Formato de pagina: `Letter`.
- Engine oficial: `xelatex`.
- Orquestador oficial: `latexmk`.
- En `02_Arquitectura` se priorizan diagramas y trees estructurales.
- En `03_Backend` y `04_Frontend` se deben incluir al menos trees de modulos o carpetas, y diagramas solo cuando aporten claridad adicional real.
- Un diagrama debe usarse cuando un tree no alcance para explicar limites, dependencias, flujos o relaciones entre componentes.
- Cada documento debe incluir:
  - descripcion;
  - alcance;
  - definiciones;
  - reglas;
  - casos borde;
  - invariantes;
  - preguntas abiertas;
  - decisiones pendientes, si aplica.

Categorias de seguimiento:

- `Decision pendiente`
- `Bloqueante`
- `Riesgo`

Criticidad:

- `Bloquea backend`
- `Bloquea frontend`
- `Bloquea ambos`
- `No bloqueante`

## Convenciones visuales para arquitectura

Objetivo:

- hacer visible la estructura del sistema sin depender solo de texto corrido;
- facilitar lectura rapida de limites, ownership y relaciones entre modulos.

Reglas:

- `02_Arquitectura` debe incluir diagramas de alto nivel y trees complementarios.
- `03_Backend` debe incluir trees de modulos y puede incluir diagramas simples de procesos o dependencias internas cuando el texto no sea suficiente.
- `04_Frontend` debe incluir trees por features y puede incluir diagramas de flujo o composicion de vistas cuando sea util.
- los trees deben mostrarse en bloques monoespaciados para conservar jerarquia visual.
- los diagramas deben ser sobrios, tecnicos y faciles de mantener; no deben introducir detalle decorativo.
- si un diagrama resulta costoso de mantener y no agrega informacion real, se prefiere un tree claro antes que un diagrama innecesario.

Formato recomendado:

- trees en bloques `verbatim` o equivalentes dentro de LaTeX;
- diagramas simples directamente en LaTeX cuando sea razonable;
- si un diagrama complejo se genera fuera de LaTeX, debe integrarse al documento final sin romper trazabilidad ni versionado.

## Flujo de trabajo

Cada documento se trabaja asi:

1. diagnostico de fuentes relevantes;
2. extraccion de reglas definidas;
3. deteccion de ambiguedades, inconsistencias y vacios;
4. clasificacion de pendientes por criticidad;
5. redaccion en LaTeX;
6. compilacion a PDF;
7. validacion de warnings relevantes;
8. actualizacion de `CHANGELOG.md`.

## Compilacion en WSL

Los comandos oficiales se ejecutan desde `Ubuntu` en WSL, ubicandose en la raiz del repositorio.

Compilar el PDF maestro:

```bash
mkdir -p .build/master
latexmk -xelatex -interaction=nonstopmode -file-line-error -outdir=.build/master tex/master.tex
cp .build/master/master.pdf Quiniela_Documentacion_Maestra.pdf
```

Compilar una seccion individual:

```bash
mkdir -p .build/01_Producto_y_Reglas
latexmk -xelatex -interaction=nonstopmode -file-line-error -outdir=.build/01_Producto_y_Reglas 01_Producto_y_Reglas/tex/01_Producto_y_Reglas.tex
cp .build/01_Producto_y_Reglas/01_Producto_y_Reglas.pdf 01_Producto_y_Reglas/01_Producto_y_Reglas.pdf
```

La misma estructura aplica a cada carpeta principal, cambiando el nombre de la seccion.

## Estado actual

- Fase 1 iniciada.
- Nucleo documental redactado para `00_Overview`, `01_Producto_y_Reglas`, `02_Arquitectura` y `05_Datos`.
- Resto de secciones creadas con estructura base y alcance declarado para trabajo iterativo.
