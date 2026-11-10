---
title: "Lección 2: Añadir valoraciones con estrellas, una mejora rápida"
description: "Usa Copilot CLI para hacer un pequeño cambio en las tarjetas de juegos, revísalo en un navegador reenviado y fusiónalo en tu primera solicitud de incorporación de cambios."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Ya has instalado Copilot CLI y probado una conversación. Ahora harás tu primer cambio en el proyecto. Será pequeño: los datos de los juegos ya incluyen una valoración con estrellas, pero todavía no aparece en las tarjetas de la página principal. Pedirás al agente que la muestre, revisarás el cambio y lo fusionarás como tu primera solicitud de incorporación de cambios.

En esta lección:

- iniciarás una conversación de Copilot centrada en una rama de función;
- pedirás al agente un pequeño cambio en el proyecto;
- revisarás el cambio con `/diff`;
- ejecutarás la aplicación y comprobarás el resultado en un navegador reenviado;
- abrirás y fusionarás tu primera solicitud de incorporación de cambios.

## Escenario

Cada juego de Tailspin Toys puede tener una valoración con estrellas, que ya aparece en su página de detalles. Sin embargo, las tarjetas de juegos de la página principal solo muestran el título, la categoría, la editorial y la descripción. Como ejercicio inicial, pedirás al agente que muestre en cada tarjeta la valoración existente. Es un cambio pequeño y autónomo, ideal para tu primera sesión.

## Anatomía de una conversación

Una **conversación** es el espacio donde trabajas con Copilot CLI en una tarea. A diferencia de la aplicación Copilot, una conversación normal de CLI usa el repositorio y la rama de Git que están abiertos en el terminal, en lugar de crear un worktree específico. Las conversaciones guardadas te permiten volver a la misma conversación, mientras que los archivos y la rama siguen siendo el estado habitual de Git en el disco.

En una conversación verás tres elementos: tus mensajes y las respuestas del agente, la actividad de las herramientas que usa para explorar y editar archivos, y los cambios que puedes examinar con `/diff`.

## Inicia una conversación y pide el cambio

Empecemos una conversación nueva para implementar la función.

1. Vuelve al codespace.
2. Si el terminal no está abierto, pulsa <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Si Copilot no está abierto, inícialo con este comando:

   ```bash
   copilot --yolo
   ```

4. Inicia una sesión nueva con el comando slash `/new` y pulsa <kbd>Enter</kbd>.
5. Envía este mensaje para pedir el cambio:

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

Copilot explorará el proyecto, localizará los archivos que muestran los detalles de los juegos y creará el código necesario. ¡Ya has añadido una función con Copilot CLI!

## Revisa las diferencias

Debes revisar todos los cambios generados por IA antes de fusionarlos, aunque sean pequeños. Examinemos los cambios directamente en Copilot CLI.

1. Escribe `/diff` y revisa todos los archivos modificados.
2. Confirma que las tarjetas muestran la valoración numérica cuando existe y `No rating yet` cuando `starRating` es `null`.
3. Confirma que las pruebas cubren ambos casos.
4. Revisa los resultados de las comprobaciones que haya ejecutado Copilot y pídele que corrija los errores.
5. Cuando termines la revisión, pulsa <kbd>Esc</kbd> para salir de la vista de diferencias.

> [!NOTE]
> Como todas las herramientas de IA generativa, Copilot es probabilístico, no determinista; el código exacto puede variar. Revisa el comportamiento en lugar de esperar una implementación idéntica.

## Comprueba los cambios

No basta con leer el código y dar por hecho que funciona. Pidamos a Copilot que inicie el sitio para que puedas examinar la interfaz actualizada en el navegador reenviado de Codespaces.

1. Pide a Copilot que inicie la aplicación:

   ```plaintext
   Start the app so I can inspect the star-rating change in my browser. Tell me the URL and leave the server running.
   ```

2. Cuando Codespaces indique que el puerto `4321` está disponible, selecciona **Open in Browser**.
3. Confirma que las tarjetas de juegos muestran su valoración sobre cinco.
4. Vuelve a Copilot y pídele que detenga el servidor:

   ```plaintext
   Stop the development server you started.
   ```

## Abre y fusiona tu primera solicitud de incorporación de cambios

Ya has creado la función. Ahora abre una solicitud de incorporación de cambios (PR) para fusionar el nuevo código en el proyecto.

1. Pide al agente predeterminado que confirme el cambio:

   ```plaintext
   Commit the reviewed star-rating changes with an appropriate commit message.
   ```

2. Escribe `/pr create`. Copilot CLI publicará la confirmación existente al crear la solicitud y mostrará su URL.
3. Abre la solicitud manteniendo pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y seleccionando la URL que muestre Copilot CLI.
4. Revisa los archivos modificados y las comprobaciones.
5. Cuando esté todo listo, selecciona **Merge pull request** y confirma la fusión.
6. Vuelve al codespace y sal de Copilot CLI con `/exit`.
7. Actualiza tu rama local `main`:

   ```bash
   git checkout main
   git pull
   ```

## Resumen y siguientes pasos

¡Enhorabuena! Has publicado tu primer cambio con GitHub Copilot CLI. En concreto:

- iniciaste una conversación centrada en una rama de función;
- pediste al agente un pequeño cambio en las tarjetas de juegos;
- revisaste el cambio con `/diff`;
- ejecutaste la aplicación y comprobaste la valoración en un navegador reenviado;
- abriste y fusionaste tu primera solicitud de incorporación de cambios.

A continuación, [partirás de la incidencia de filtrado y usarás los modos Plan y Autopilot][next-lesson] para crear una función más compleja.

## Recursos

- [Acerca de GitHub Copilot CLI][about-copilot-cli]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
