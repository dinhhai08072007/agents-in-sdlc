---
title: "Lección 7: Crear y usar un agente de QA"
description: "Crea un agente de QA personalizado que combine los requisitos de la incidencia, la skill quality-checks y Playwright MCP."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Has usado la skill `quality-checks` para ejecutar comprobaciones automatizadas y Playwright MCP para observar en el navegador la experiencia de filtrado. Ahora combinarás esas capacidades en un agente personalizado con un proceso de QA bien definido.

En esta lección:

- explorarás cómo trabaja un agente personalizado con instrucciones, skills y herramientas MCP;
- crearás y revisarás un perfil de control de calidad reutilizable;
- seleccionarás el agente de QA y revisarás sus conclusiones respecto a la incidencia de filtrado.

## Escenario

Antes de abrir una solicitud de incorporación de cambios (PR), Tailspin Toys quiere revisar de forma coherente los requisitos, la calidad del código, las comprobaciones automatizadas, la cobertura de pruebas y el comportamiento en el navegador. Un agente personalizado puede coordinar el proceso de QA y generar un informe reutilizable.

## ¿Qué es un agente personalizado?

Un agente personalizado es una versión especializada de Copilot definida mediante un perfil Markdown. El perfil describe el propósito, las instrucciones y las herramientas disponibles para el agente. En este taller, definirás el rol de QA en `.github/agents/qa.agent.md` y lo seleccionarás desde Copilot CLI.

Las personalizaciones que has utilizado tienen funciones distintas. Las instrucciones del repositorio describen los estándares del equipo. La skill `quality-checks` agrupa comprobaciones repetibles. Playwright MCP proporciona herramientas para el navegador. El perfil de QA indica a Copilot cómo utilizar esas capacidades para evaluar los requisitos y comunicar los resultados. No sustituye a las demás herramientas ni requiere iniciar otra conversación.

## Crea el perfil de QA

Antes de abrir la solicitud de la función, pide a Copilot que cree un perfil de QA reutilizable. El perfil definirá las comprobaciones que hará el agente y los límites que debe respetar.

1. Vuelve al codespace y confirma que la conversación de filtrado está en modo Interactive.
2. Pide al agente predeterminado que cree el agente personalizado:

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## Inspecciona el perfil

Antes de usar el agente nuevo, revisa su perfil para confirmar que Copilot ha incluido el proceso y los límites de QA que querías. Así evitarás que un agente incompleto o demasiado amplio modifique una función que solo querías comprobar.

1. Escribe `/diff` y abre `.github/agents/qa.agent.md`.
2. Lee el frontmatter. `description` es obligatorio; `name` es opcional, pero incluirlo proporciona un nombre claro en la interfaz.
3. Lee las instrucciones del perfil y confirma que QA empieza por los requisitos, sigue las instrucciones del repositorio, ejecuta la skill `quality-checks` y usa Playwright MCP.
4. Confirma que el agente presenta pruebas que respaldan sus resultados, pide permiso antes de cambiar código de implementación y no confirma cambios ni abre solicitudes de incorporación de cambios.
5. Si faltan responsabilidades o límites, pide al agente predeterminado que revise el perfil antes de continuar.

## Ejecuta QA con la incidencia

Copilot CLI carga los agentes del proyecto al iniciar una conversación. Después de crear el perfil, retoma la misma conversación de filtrado y selecciona QA para que tenga disponibles la incidencia y las decisiones de planificación.

1. Activa el agente con este mensaje:

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> Como acabas de crear el agente, puede que todavía no aparezca en la lista de agentes disponibles. El comando anterior lo activará igualmente.

2. Pide al agente de QA que revise la función:

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. El agente de QA empezará a trabajar.
4. Cuando termine, lee el informe.

## Resumen y siguientes pasos

Has añadido un rol especializado reutilizable al flujo de trabajo y has revisado su trabajo. En esta lección:

- exploraste cómo trabaja un agente personalizado con instrucciones, skills y herramientas MCP;
- creaste e inspeccionaste un perfil de control de calidad reutilizable;
- seleccionaste el agente de QA y revisaste sus conclusiones respecto a la incidencia de filtrado.

La implementación, la actualización de la skill, el perfil de QA, las pruebas y el informe de verificación ya están listos para revisarlos. A continuación, [los reunirás en una solicitud de incorporación de cambios y usarás Agent Merge][next-lesson].

## Recursos

- [Crear agentes personalizados para Copilot CLI][create-agents]
- [Configuración de agentes personalizados][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
