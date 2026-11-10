---
title: "Lección 6: Validar funciones con Playwright MCP"
description: "Inspecciona o configura Playwright MCP en Copilot CLI y úsalo para explorar en el navegador la función de filtrado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como ya hemos señalado, escribir código implica mucho más que programarlo. Hay que trabajar con datos y servicios externos, y permitir que Copilot use automatizaciones adicionales. Para ello sirven los servidores MCP: amplían las herramientas y los servicios integrados en Copilot CLI.

En esta lección:

- aprenderás qué es Model Context Protocol (MCP) y cómo lo usa Copilot CLI;
- añadirás el servidor MCP de Playwright si todavía no está disponible;
- pedirás al agente que controle un navegador y explore la función de filtrado.

## Escenario

Las pruebas unitarias y de extremo a extremo son importantes, pero para validar cambios en la interfaz hay que interactuar con ella. Quieres que Copilot use el sitio web como lo haría una persona y automatice más comprobaciones, para tener mayor confianza en que los cambios funcionan como esperas.

## ¿Qué es Model Context Protocol (MCP)?

[Model Context Protocol (MCP)][mcp-blog-post] permite que los agentes de IA se comuniquen con herramientas y servicios externos. Gracias a MCP, pueden acceder a información actualizada y realizar acciones en tu nombre.

El acceso a estas herramientas y recursos se hace mediante un servidor MCP, que sirve de puente entre el agente de IA y los servicios externos. Cada servidor ofrece al agente un conjunto distinto de herramientas y recursos.

Algunos servidores MCP populares:

- **[GitHub MCP Server][github-mcp]**: proporciona acceso a API para gestionar repositorios, incidencias y solicitudes de incorporación de cambios de GitHub.
- **[Playwright MCP Server][playwright-mcp-server]**: permite automatizar el navegador mediante Playwright.

Hay muchos otros servidores MCP disponibles. GitHub aloja un [registro de MCP][mcp-registry] para facilitar su descubrimiento y las contribuciones.

> [!CAUTION]
> Trata los servidores MCP como cualquier otra dependencia del proyecto. Antes de usar uno, revisa su código fuente, comprueba quién lo publica y considera las implicaciones de seguridad.

## Añade el servidor MCP de Playwright

Si el servidor MCP de Playwright todavía no está disponible, añádelo para que Copilot pueda interactuar con el sitio como lo haría una persona.

> [!NOTE]
> Si Playwright MCP ya está configurado en Copilot CLI, omite estos pasos y continúa con la sección siguiente.

1. Vuelve al codespace.
2. En Copilot CLI, abre el cuadro de diálogo para añadir un servidor MCP:

   ```plaintext
   /mcp add
   ```

3. Escribe `playwright` como nombre y pulsa <kbd>Tab</kbd>.
4. Confirma el tipo de servidor **STDIO** pulsando <kbd>Enter</kbd> y, después, pulsa <kbd>Tab</kbd>.
5. Pega lo siguiente en el cuadro de diálogo **Command**:

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. Pulsa <kbd>Control</kbd>+<kbd>S</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) para guardar el servidor MCP.
7. Pulsa <kbd>Esc</kbd> para salir del cuadro de diálogo MCP.

## Pide a Copilot que explore la función con Playwright

Antes comprobaste manualmente que la función se comporta como esperas. Ahora hagamos lo mismo con el servidor MCP de Playwright disponible en Copilot.

1. Envía este mensaje para pedir a Copilot que valide la función con el servidor MCP de Playwright:

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> No es imprescindible pedir a Copilot que use el servidor MCP; normalmente lo averiguará por su cuenta. Pero como sabes qué herramienta debe usar, indicárselo puede dar resultados más coherentes y ahorrar algunos tokens.

2. Observa los pasos que ejecuta Copilot en el navegador para comprobar que la función funciona.
3. Lee el informe y confirma que todo se comporta como esperas.

Copilot iniciará el servidor, interactuará con el sitio mediante Playwright, detendrá el servidor y te entregará un informe.

## Resumen y siguientes pasos

¡Enhorabuena! Has usado el servidor MCP de Playwright para explorar la función en un navegador real desde Copilot CLI. En resumen:

- aprendiste qué es Model Context Protocol (MCP) y cómo lo usa Copilot CLI;
- añadiste el servidor MCP de Playwright si aún no estaba disponible;
- pediste al agente que controlara un navegador y explorara la función de filtrado.

A continuación, [crearás un agente personalizado de QA][next-lesson] que combinará la skill y las herramientas del navegador en un rol especializado.

## Recursos

- [Qué es MCP y por qué todo el mundo habla de ello][mcp-blog-post]
- [Servidor MCP de Microsoft Playwright][playwright-mcp-server]
- [Añadir servidores MCP a Copilot CLI][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
