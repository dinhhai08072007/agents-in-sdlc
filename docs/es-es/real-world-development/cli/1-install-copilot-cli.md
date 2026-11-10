---
title: "Lección 1: Instalar GitHub Copilot CLI"
description: "Instala y autentica Copilot CLI en tu codespace, conoce la interfaz y encuentra la incidencia de filtrado creada."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

[GitHub Copilot CLI][about-copilot-cli] es un potente asistente de programación con agentes que se ejecuta en el terminal y te permite explorar bases de código, generar código, ejecutar comandos e interactuar con herramientas externas desde la línea de comandos. Puedes delegar tareas, pedir cambios y mantener la concentración. El primer paso, como imaginarás, es instalar la herramienta. Por suerte, puedes hacerlo con herramientas que ya conoces.

En esta lección:

- instalarás GitHub Copilot CLI mediante npm;
- te autenticarás con tu cuenta de GitHub;
- confiarás en el repositorio del taller y probarás una conversación rápida;
- buscarás la incidencia de filtrado mediante el servidor MCP integrado de GitHub.

## Escenario

Tu equipo está empezando a usar agentes de IA para abordar una lista de trabajo pendiente cada vez mayor. Copilot CLI lleva esa capacidad a la terminal, donde muchos desarrolladores ya trabajan habitualmente. En esta lección lo instalarás, autenticarás y dejarás listo para usarlo durante el resto del taller.

## Instala Copilot CLI

Puedes instalar Copilot CLI mediante [npm][install-cli], WinGet o Homebrew. Como GitHub Codespaces incluye Node.js, usarás npm.

1. Vuelve al codespace y abre un terminal.
2. Comprueba que Node.js esté instalado y que cumpla el requisito de versión:

   ```bash
   node --version
   ```

   Deberías ver la versión 24 o posterior.

3. Instala Copilot CLI globalmente:

   ```bash
   npm install -g @github/copilot
   ```

4. Comprueba la instalación:

   ```bash
   copilot --version
   ```

   Debería aparecer el número de versión.

## Autentícate con GitHub

La primera vez que inicies Copilot CLI, te pedirá que te autentiques con tu cuenta de GitHub.

1. Inicia Copilot CLI:

   ```bash
   copilot
   ```

2. Si se te solicita, sigue las instrucciones del código de dispositivo para autenticarte y autorizar Copilot CLI.
3. Copilot CLI mostrará este mensaje:

   ```plaintext
   Copilot can read files in this folder and, with your permission, edit them or run code and shell commands. It will remember your permissions for the rest of this session.

   Do you trust the files in this folder?
   ```

4. Comprueba que la ruta corresponde al repositorio Tailspin Toys y, después, selecciona **Yes, and remember this folder for future sessions**.

> [!NOTE]
> En un codespace, es posible que ya estés autenticado mediante tu sesión de GitHub. Si Copilot CLI se inicia sin pedir autenticación, ya está todo listo.

## Familiarízate con la interfaz

Los comandos que escribes en el prompt habitual del shell se ejecutan directamente en el codespace. Cuando se inicia Copilot CLI, los mensajes en lenguaje natural se envían al agente y los comandos slash controlan la conversación.

1. Escribe `/model`, usa las flechas para seleccionar **Auto**, pulsa <kbd>Enter</kbd> y vuelve a pulsar <kbd>Enter</kbd> para confirmar.
2. Escribe `/help` para ver los comandos de tu versión y pulsa <kbd>Esc</kbd> para cerrar la ayuda.
3. Haz una pregunta sencilla para comprobar que todo funciona:

   ```plaintext
   What are the key files in this project?
   ```

4. Lee la respuesta y observa cómo Copilot explora el repositorio antes de responder.
5. Escribe `/mcp list` y confirma que está disponible el servidor MCP integrado de GitHub.
6. Pide a Copilot que encuentre la incidencia de filtrado:

   ```plaintext
   Using GitHub MCP, find the issue in this repository titled "Allow users to filter games by category and publisher." Give me its URL and a short summary. Don't change anything.
   ```

7. Abre la URL y lee la incidencia. La usarás después de completar un primer cambio rápido.

> [!TIP]
> Una sesión normal de Copilot CLI trabaja en la rama que esté seleccionada en el terminal; no crea automáticamente un worktree. Crearás una rama de función antes de cada cambio.

## Usa el acceso directo del taller

Normalmente, Copilot CLI pide permiso antes de usar herramientas fuera de los permisos establecidos. Para este taller, lo reiniciarás con `--yolo`, un acceso directo aprobado por ti que elimina esas solicitudes de permiso dentro del codespace para que puedas centrarte en los ejercicios.

> [!CAUTION]
> `--yolo` concede permisos automáticos completos (`--allow-all-tools`, `--allow-all-paths` y `--allow-all-urls`). Úsalo solo en un entorno aislado, como un codespace o una máquina virtual; nunca lo configures como opción predeterminada para el desarrollo diario. Consulta [Permitir y denegar el uso de herramientas][allow-all-warning] para obtener más información.

En este taller, `--enable-all-github-mcp-tools` habilita las herramientas de lectura y escritura de GitHub MCP que se usarán más adelante para trabajar con incidencias y solicitudes de incorporación de cambios. El codespace limita el acceso a tu equipo local, pero los recursos de GitHub autenticados siguen siendo reales. Revisa los cambios antes de publicarlos o fusionarlos.

1. Sal de Copilot CLI con `/exit`.
2. Vuelve a iniciarlo desde la raíz del repositorio:

   ```bash
   copilot --yolo --enable-all-github-mcp-tools
   ```

3. Haz otra pregunta breve sobre el proyecto para confirmar que la conversación funciona y, después, sal con `/exit`.

Copilot guarda las conversaciones automáticamente. Más adelante, cuando cambies las instrucciones o añadas un agente, utilizarás `copilot --resume` para volver a la misma conversación y rama de la función.

## Resumen y siguientes pasos

¡Enhorabuena! En esta lección:

- instalaste GitHub Copilot CLI mediante npm;
- te autenticaste con tu cuenta de GitHub;
- confiaste en el repositorio del taller y probaste una conversación rápida;
- encontraste la incidencia de filtrado mediante el servidor MCP integrado de GitHub.

A continuación, [empezarás un primer cambio específico][next-lesson] y utilizarás Copilot CLI para mostrar una valoración por estrellas en las tarjetas de juegos.

## Recursos

- [Instalar GitHub Copilot CLI][install-cli]
- [Acerca de GitHub Copilot CLI][about-copilot-cli]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-add-star-rating/
[install-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/set-up-copilot-cli/install-copilot-cli
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[allow-all-warning]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli#allowing-and-denying-tool-use
