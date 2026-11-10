---
title: "Lección 5: Personalizar y usar una skill de comprobaciones de calidad"
description: "Explora la skill quality-checks existente, personaliza el formato del informe y úsala para validar los filtros."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Escribir código implica mucho más que programarlo. Ya hemos comprobado manualmente que funciona y hemos usado archivos de instrucciones para garantizar que cumple los estándares. Pero ¿qué ocurre con las pruebas, el lint y las demás partes de la integración continua (CI)?

Las **skills de agente** son adecuadas para estas tareas. Ayudan a Copilot a entender cómo ejecutar operaciones como estas.

En esta lección:

- explorarás la skill `quality-checks` existente;
- personalizarás el formato de los resultados;
- volverás a cargar la skill y la ejecutarás.

## Escenario

Tailspin Toys usa la skill `quality-checks` para ejecutar pruebas unitarias, lint y comprobaciones de tipos. El equipo quiere mejorar el informe para que los resultados sean más fáciles de leer.

## Instrucciones, scripts y recursos

Las skills de agente agrupan instrucciones reutilizables para tareas, scripts ejecutables y recursos complementarios que el agente carga cuando los necesita. En esencia, son una carpeta con el nombre de la skill que contiene un archivo Markdown llamado `SKILL.md`. El archivo tiene frontmatter con los campos `name` y `description`, una descripción general de la función de la skill e indicaciones sobre cuándo debe usarse. También puede contener subcarpetas con scripts y otros recursos.

> [!NOTE]
> Las carpetas y los archivos adicionales son opcionales. La skill `quality-checks` de Tailspin Toys solo contiene `SKILL.md` porque usa los comandos existentes del proyecto.

Las skills pueden estar en `.github/skills` del proyecto para compartirlas y reutilizarlas en el equipo, o en la carpeta de skills del usuario `~/.copilot/skills`.

## Explora la skill

Exploremos la skill `quality-checks` que creó el equipo de Tailspin Toys para ejecutar pruebas unitarias, lint y comprobaciones de tipos.

1. Vuelve al codespace. En el editor de Codespaces, abre `.github/skills/quality-checks/SKILL.md`.
2. Lee los campos `name` y `description` del principio. La descripción ayuda a Copilot a determinar cuándo debe usar la skill.
3. Lee las instrucciones y observa cómo guían a Copilot durante las pruebas y el lint.
4. Fíjate en que la skill todavía no tiene una sección **Results output formatting**.

## Ejecuta la skill antes de modificarla

Puedes invocar las skills directamente desde Copilot CLI o mediante lenguaje natural. Pidamos a Copilot que ejecute las tres comprobaciones de la skill.

1. Vuelve a la conversación de filtrado en modo Interactive.
2. Envía este mensaje:

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. Observa el informe final.

## Personaliza el informe

Queremos que el informe indique qué se ejecutó, si terminó correctamente y qué resultados comunicaron las herramientas. Actualicemos la skill para generar ese informe.

1. Vuelve a `.github/skills/quality-checks/SKILL.md`.
2. Añade al final del archivo esta sección:

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. El archivo se guardará automáticamente.

## Ejecuta la skill actualizada

Ya hemos realizado el cambio. Veamos cómo funciona. Copilot CLI puede volver a cargar las skills editadas sin reiniciar la conversación.

1. Escribe:

   ```plaintext
   /skills reload
   ```

2. Envía el mismo mensaje que antes:

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. Observa el informe final y compáralo con el primero.

## Resumen y siguientes pasos

Has personalizado y usado una skill de agente existente. En esta lección:

- exploraste la skill `quality-checks` para pruebas unitarias, lint y comprobaciones de tipos;
- personalizaste el formato de los resultados;
- volviste a cargar y ejecutaste la skill.

El cambio acompañará a los filtros en la solicitud de incorporación de cambios. A continuación, permitirás que Copilot interactúe directamente con el sitio [mediante el servidor MCP de Playwright][next-lesson].

## Más ejemplos de skills

Estos ejemplos de la comunidad sirven de referencia y no son tareas adicionales:

- [Especificación de Agent Skills][skill-spec]
- [Flujo de trabajo para contribuir: `make-repo-contribution`][contribution-example]
- [Documentos de requisitos: `prd`][prd-example]
- [Diagramas y script de exportación incluido: `drawio`][drawio-example]
- [Pruebas del navegador: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
