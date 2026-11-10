---
title: "Ejercicio 3: Probar la función con el servidor MCP de Playwright"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Modo de agente][previous-lesson] · [Siguiente lección: Agentes personalizados →][next-lesson]

Acabas de crear la función de filtrado con el modo de agente. Antes de abrir una solicitud de incorporación de cambios, confirma que funciona en el navegador. En lugar de probar la aplicación manualmente, conectarás el **servidor MCP de Playwright** y pedirás a Copilot que controle un navegador para comprobar la función. Después publicarás la rama y abrirás la solicitud.

En este ejercicio:

- aprenderás qué es Model Context Protocol (MCP) y cómo los servidores MCP amplían Copilot con nuevas herramientas;
- añadirás el servidor MCP de Playwright al espacio de trabajo;
- pedirás a Copilot que lo use para probar manualmente la función de filtrado en el navegador;
- publicarás la rama y abrirás una solicitud de incorporación de cambios para el trabajo de filtrado.

## ¿Qué es Model Context Protocol (MCP)?

El modo de agente es mucho más potente cuando puede acceder a recursos externos al editor. Copilot utiliza Model Context Protocol (MCP) para comunicarse con herramientas y servicios externos mediante un estándar común.

![Diagrama del funcionamiento interno del modo de agente y su interacción con el contexto, el LLM y las herramientas, incluidas las que proporcionan los servidores MCP y las extensiones de VS Code](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) permite que los agentes de IA se comuniquen con herramientas y servicios externos en tiempo real. Así pueden acceder a información actualizada mediante recursos y realizar acciones en tu nombre mediante herramientas.

Se accede a estas herramientas y recursos a través de un servidor MCP, que sirve de puente entre el agente de IA y los servicios externos, como las API existentes o las herramientas locales (por ejemplo, paquetes de NPM). Cada servidor MCP ofrece al agente un conjunto distinto de herramientas y recursos.

Algunos servidores MCP populares:

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: proporciona acceso a API para gestionar repositorios de GitHub. Permite, por ejemplo, crear repositorios, actualizar los existentes y gestionar incidencias y solicitudes de incorporación de cambios.
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: proporciona funciones de automatización del navegador mediante Playwright. Permite, por ejemplo, abrir páginas web, rellenar formularios y seleccionar botones.

Hay muchos otros servidores MCP disponibles. GitHub aloja un [registro de MCP](https://github.com/mcp) para facilitar su descubrimiento y las contribuciones.

> [!CAUTION]
> En materia de seguridad, trata los servidores MCP como cualquier otra dependencia del proyecto. Antes de usar uno, revisa su código fuente, comprueba quién lo publica y considera las implicaciones de seguridad. Usa únicamente servidores de confianza y ten cuidado al conceder acceso a recursos u operaciones sensibles.

## Revisa la configuración de MCP

El archivo `.vscode/mcp.json` configura los servidores MCP disponibles en este espacio de trabajo de VS Code.

1. Abre `.vscode/mcp.json` en el codespace.
2. Verás que ya hay un servidor `github` configurado:

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

La entrada `github` viene con la plantilla del proyecto; por eso Copilot pudo leer la lista de incidencias en el ejercicio anterior. Utiliza el [servidor MCP remoto de GitHub][remote-github-mcp-server], así que no hay que instalarlo localmente. VS Code se conecta mediante HTTP y te autenticas con GitHub la primera vez que Copilot usa una de sus herramientas.

## Añade el servidor MCP de Playwright

Ahora añadirás un segundo servidor. El [servidor MCP de Playwright][playwright-mcp-server] proporciona a Copilot un navegador que puede controlar, justo lo que necesitas para probar la función.

1. En `.vscode/mcp.json`, añade una entrada `playwright` junto a `github` para que el archivo quede así:

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. Guarda el archivo.

A diferencia del servidor remoto `github`, `playwright` es local: VS Code lo inicia en tu equipo mediante `npx @playwright/mcp@latest`. La opción `--headless` hace que Playwright ejecute el navegador sin mostrar una ventana, algo necesario en un codespace, donde no hay un escritorio que pueda mostrarla.

> [!NOTE]
> El proyecto Tailspin Toys ya usa Playwright para las pruebas de extremo a extremo, así que normalmente el navegador necesario ya está instalado. Si Copilot informa más adelante de que falta, pídele que ejecute `npx playwright install chromium` y vuelva a intentarlo.

## Inicia el servidor y confía en él

VS Code inicia los servidores MCP **cuando se necesitan**: la primera vez que Copilot necesita las herramientas de un servidor, VS Code lo inicia y te pregunta si confías en él.

1. En `.vscode/mcp.json`, aparece la opción **Start** encima de la entrada `playwright`. Selecciónala para iniciar el servidor y confirmar que la configuración es correcta.
2. Si VS Code te pregunta si confías en el servidor, revisa la configuración y confirma que confías en él para iniciarlo.

Iniciarlo manualmente es opcional. Si no lo haces, VS Code lo iniciará y pedirá que confirmes si confías en él la primera vez que Copilot necesite sus herramientas.

## Prueba la función de filtrado

El [servidor MCP de Playwright][playwright-mcp-server] proporciona a Copilot un navegador real. En vez de probar la aplicación tú, el agente puede abrir una página, navegar por ella, aplicar filtros y leer los resultados para después resumirlos. Así puedes comprobar rápidamente si una función se comporta como esperas sin salir de la conversación.

Internamente, el servidor MCP de Playwright trabaja con el [árbol de accesibilidad][playwright-mcp-server] de la página en vez de con capturas de pantalla. El agente interpreta elementos estructurados y etiquetados (botones, enlaces y elementos de lista), igual que lo haría una tecnología de asistencia. Por eso, una comprobación funcional rápida también sirve para una comprobación básica de accesibilidad.

Con el servidor conectado y la aplicación en ejecución, pide a Copilot que pruebe la función de filtrado que acabas de crear:

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot abrirá un navegador mediante el servidor MCP de Playwright, seguirá los pasos y te comunicará lo que encuentre. Compara el informe con los criterios de aceptación de la incidencia. Si algo no funciona, haz preguntas de seguimiento o pide que corrija el código antes de abrir la solicitud.

> [!NOTE]
> La aplicación debe estar en ejecución en `http://localhost:4321` para esta prueba. Si detuviste el servidor de desarrollo, vuelve a iniciarlo antes de enviar el mensaje. La primera vez que Copilot use el servidor MCP de Playwright, quizá tenga que descargar un navegador. Si indica que falta, pídele que ejecute `npx playwright install chromium` y vuelva a intentarlo.

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## Publica la rama y crea una solicitud de incorporación de cambios

Ya has confirmado que la función funciona. Ahora puedes abrir una solicitud de incorporación de cambios (PR) para que el equipo la revise. Primero publicarás la rama `filtering-vscode`.

1. Ve al panel **Source Control** del codespace y revisa los cambios de Copilot.
2. Selecciona el icono **+** para preparar los cambios.
3. Genera un mensaje de confirmación mediante el botón **Sparkle**.

   ![Captura de pantalla del panel Source Control con los cambios realizados](../../../_images/ex3-source-control-changes.png)

4. Selecciona **Publish** para enviar la rama al repositorio.

## Crea la solicitud de incorporación de cambios

Hay varias formas de crear una solicitud, como github.com o la interfaz de línea de comandos (CLI) de GitHub. Como ya estás trabajando con GitHub Copilot, pídele que la cree. Puede buscar la incidencia correspondiente y asociarla a la solicitud.

1. Abre el panel Copilot Chat y selecciona **New Chat** para iniciar una sesión nueva.
2. Confirma que **Agent** está seleccionado en el menú desplegable de agentes para que Copilot pueda usar las herramientas de GitHub.
3. Pídele a Copilot que cree una solicitud:

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. Si hace falta, selecciona **Continue** para permitir que Copilot recopile información y realice las operaciones necesarias. La primera vez que use una herramienta de GitHub, puede que tengas que autenticarte; sigue las instrucciones.
5. Observa cómo Copilot busca entre las incidencias, encuentra la correcta y crea la solicitud.
6. Selecciona el enlace que genere Copilot para revisar la solicitud, pero **no la fusiones todavía**.

## Resumen y siguientes pasos

¡Enhorabuena! En este ejercicio:

- aprendiste qué es Model Context Protocol (MCP) y cómo los servidores MCP amplían Copilot con nuevas herramientas;
- añadiste el servidor MCP de Playwright al espacio de trabajo;
- lo usaste para probar manualmente la función en un navegador antes de publicarla;
- publicaste la rama y abriste una solicitud de incorporación de cambios para el trabajo de filtrado.

Has utilizado un servidor MCP para *probar* una función, pero MCP solo es una de las formas de ampliar Copilot. A continuación, [crearás un agente personalizado][next-lesson] para agilizar tareas específicas, como las revisiones de accesibilidad.

## Recursos

- [Qué es MCP y por qué todo el mundo habla de ello][mcp-blog-post]
- [Servidor MCP de Microsoft Playwright][playwright-mcp-server]
- [Servidor MCP de GitHub][github-mcp-server]
- [Registro de MCP de GitHub][mcp-registry]
- [Servidores MCP en VS Code][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
