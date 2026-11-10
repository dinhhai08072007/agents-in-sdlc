---
title: "Lección 4: Orientar a Copilot con instrucciones personalizadas"
description: "Explora las instrucciones del repositorio, añade un estándar de documentación y aplícalo al código de filtrado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

El contexto es fundamental cuando se trabaja con IA generativa. Si una tarea debe realizarse de una forma concreta, proporciona a Copilot las instrucciones necesarias. Los [archivos de instrucciones][instruction-files] describen no solo *qué* código quieres, sino también *cómo* debe estructurarse. Ahora que has creado los filtros, explorarás las instrucciones que ha usado Copilot, añadirás un estándar de documentación y lo aplicarás al código.

En esta lección:

- explorarás cómo llegan al agente las instrucciones del repositorio y las instrucciones específicas de determinadas rutas;
- actualizarás el archivo de instrucciones para que se cumplan los estándares de programación;
- observarás el efecto de los archivos de instrucciones en el código.

## Escenario

Tailspin Toys tiene pautas y requisitos para sus prácticas de programación. Entre ellos:

- Los comentarios deben explicar la intención y las decisiones que no sean obvias, no repetir lo que hace el código.
- Las funciones exportadas en `db/` y `src/lib/` deben documentar su propósito, parámetros y valores devueltos mediante TSDoc/JSDoc; también deben documentar el argumento `db` inyectable cuando exista.
- Los componentes Astro reutilizables deben documentar sus contratos `Props`, y los comentarios deben mantenerse actualizados cuando cambie el código relacionado.
- Deben conservarse las directrices de formato y lint existentes.

Con los archivos de instrucciones, proporcionarás a Copilot la información necesaria para que realice las tareas de acuerdo con estas prácticas.

## Archivos de instrucciones

Las instrucciones personalizadas proporcionan contexto y preferencias a Copilot para que entienda mejor tu estilo y tus requisitos de programación. Esta función permite orientar las sugerencias y los fragmentos de código. Puedes especificar las convenciones de programación, las bibliotecas y los comentarios que prefieres. Puedes crear instrucciones para todo el repositorio o para tipos concretos de archivos y tareas.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md`: un único archivo que se envía a Copilot en **todas** las solicitudes del repositorio. Debe contener información general del proyecto pertinente para la mayoría de las solicitudes.
- `.github/instructions/*.instructions.md`: archivos que proporcionan directrices para determinados lenguajes, tipos de archivo o tareas.

> [!NOTE]
> Los demás formatos de instrucciones y su compatibilidad varían según el entorno. Consulta la [referencia sobre compatibilidad con instrucciones personalizadas][custom-instructions-support] antes de depender de un formato concreto.

## Explora los archivos de instrucciones personalizadas del proyecto

El proyecto inicial ya incluye varios archivos de instrucciones. Explorémoslos antes de hacer cambios para observar el efecto.

1. Vuelve al codespace.
2. En el editor de Codespaces (no en el terminal), abre `.github/copilot-instructions.md`.
3. Explora el archivo y fíjate en la breve descripción del proyecto y las pautas de programación. Estas instrucciones se aplican a todas las interacciones con Copilot en este repositorio.
4. Abre la carpeta `.github/instructions` y explora sus archivos. Verás instrucciones para archivos Astro, la capa de datos Drizzle, las pruebas y otros elementos.
5. Abre `.github/instructions/unit-tests.instructions.md`. Observa el campo `applyTo` del principio, que establece un patrón glob para determinar a qué archivos se aplican las instrucciones.
6. Abre `.github/instructions/drizzle.instructions.md` y observa sus enlaces a otros archivos de instrucciones y a archivos existentes del proyecto. Así puedes dividir conjuntos de instrucciones grandes en archivos más pequeños y reutilizables, y señalar ejemplos que Copilot puede seguir.

## Actualiza los archivos de instrucciones según las pautas del equipo

Los archivos existentes son un buen punto de partida, pero falta una directriz. Modifiquemos el archivo principal `copilot-instructions.md` para garantizar que el nuevo código TypeScript incluya comentarios TSDoc.

1. En `.github/copilot-instructions.md`, busca la sección **Code formatting guidance**, que debería estar cerca de la línea 35.
2. Añade el siguiente elemento al final de la lista:

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

El archivo se guardará automáticamente.

## Aplica las instrucciones actualizadas

Copilot CLI carga las instrucciones del repositorio al iniciar una conversación. Después de guardar las instrucciones nuevas, sal de la conversación de filtrado y retómala para que Copilot las cargue sin perder el contexto de la función.

1. Si la conversación de filtrado sigue abierta, escribe `/exit` para cerrarla. Copilot CLI guarda automáticamente las conversaciones.
2. Desde la raíz del repositorio, en la rama `game-filters-cli`, retoma la conversación guardada:

   ```bash
   copilot --resume
   ```

   Si se te solicita, selecciona la conversación de filtrado y confirma que está seleccionada la rama `game-filters-cli`.
3. Pide a Copilot que aplique las instrucciones actualizadas:

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. Escribe `/diff` y revisa los archivos TypeScript modificados. Observa los comentarios TSDoc nuevos y confirma que describen el código correctamente.

## Resumen y siguientes pasos

Has explorado cómo Copilot CLI obtiene contexto de los archivos de instrucciones y has aplicado un estándar nuevo a la función. En concreto:

- exploraste cómo llegan al agente las instrucciones del repositorio y las instrucciones específicas de determinadas rutas;
- actualizaste el archivo de instrucciones para que se cumplan los estándares de programación;
- observaste el efecto de los archivos de instrucciones en el código.

A continuación, [personalizarás y ejecutarás la skill reutilizable `quality-checks`][next-lesson] para que el lint y las pruebas se ejecuten de forma coherente.

## Recursos

- [Añadir instrucciones personalizadas a Copilot CLI][instruction-files]
- [Compatibilidad con instrucciones personalizadas][custom-instructions-support]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Awesome Copilot: archivos de instrucciones y otros recursos][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
