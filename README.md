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

