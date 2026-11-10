---
title: "Lección 10: Repaso y siguientes pasos"
description: "Repasa el flujo de trabajo de Copilot CLI, las dos solicitudes de incorporación de cambios, las personalizaciones reutilizables y otros recursos."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Has usado GitHub Copilot CLI en un flujo de trabajo continuo para Tailspin Toys. En concreto:

- preparaste un codespace, instalaste Copilot CLI, exploraste el proyecto y encontraste la incidencia de filtrado ya creada;
- añadiste valoraciones con estrellas, revisaste el resultado en un navegador reenviado y fusionaste manualmente tu primera solicitud de incorporación de cambios (PR);
- partiste de la incidencia de filtrado, definiste el enfoque con el modo Plan, lo implementaste con Autopilot y lo revisaste en el modo Interactive;
- orientaste al agente mediante instrucciones personalizadas, personalizaste la skill existente `quality-checks` y la usaste para ejecutar pruebas unitarias, lint y comprobaciones de tipos;
- añadiste el servidor Model Context Protocol (MCP) de Playwright y lo usaste para explorar el filtrado en un navegador real;
- creaste y seleccionaste un agente personalizado de control de calidad (QA) para evaluar los requisitos, la cobertura, los resultados de la skill y las pruebas del navegador;
- revisaste el cambio completo de filtrado y autorizaste Agent Merge para la solicitud correspondiente;
- exploraste comandos slash para gestionar el contexto y los modelos, compartir sesiones y delegar tareas opcionalmente al agente en la nube.

## Cambios que has publicado

El taller incluye dos hitos de solicitudes de incorporación de cambios, cada uno en su propia rama basada en `main` actualizada:

1. **Valoraciones con estrellas:** mostrar el valor existente de `starRating` y un estado explícito para los juegos sin valorar en las tarjetas.
2. **Filtrado y flujo de calidad:** implementar el filtrado, actualizar las instrucciones y aplicarlas a la función, personalizar el informe de `quality-checks`, crear un perfil de QA e incluir las pruebas relacionadas.

Desde la planificación del filtrado hasta la creación de su solicitud, usaste la misma conversación y rama. Agrupamos ese trabajo en una solicitud para simplificar el taller.

## Distintos tipos de verificación

Comprobaste el código de varias formas: pruebas automatizadas, exploración manual en el navegador y navegación automatizada de Copilot mediante MCP. La skill `quality-checks` ejecutó pruebas unitarias, lint y comprobaciones de tipos, y presentó los resultados con el nuevo formato. QA combinó esos resultados con una revisión de los requisitos y la cobertura antes de crear la solicitud.

Las pruebas nuevas deben cubrir carencias reales; si QA no necesita añadir pruebas, también puede ser correcto. Antes de autorizar una fusión, revisa el código y las pruebas, y vuelve a generar las pruebas afectadas después de hacer cambios.

## Prácticas recomendadas

El contexto y las herramientas que proporcionas influyen en el trabajo de Copilot. En este taller actualizaste instrucciones, personalizaste una skill, creaste un perfil de QA y configuraste un servidor MCP. Reutiliza estas personalizaciones entre conversaciones y ajústalas a las necesidades del equipo. Las instrucciones establecen estándares, las skills describen tareas repetibles, los agentes personalizados definen roles especializados y los servidores MCP conectan herramientas externas. Revisa los cambios y los resultados de las herramientas; no te limites al resumen del agente.

Elige el **modo y el modelo** adecuados para la tarea. Usa **Plan** para decidir el enfoque antes de empezar, **Interactive** para seguir de cerca los cambios específicos y **Autopilot** para las tareas bien delimitadas. Elige un modelo más rápido para ediciones rutinarias y uno más avanzado para tareas complejas.

El contexto sigue siendo tan importante como la infraestructura. Explicar con claridad *qué* quieres crear, *por qué* y *cómo* influye de manera significativa en el resultado.

## Más funciones para explorar

Has completado el flujo de trabajo principal. Puedes explorar otras funciones de CLI:

- `/review` para pedir al agente de revisión de código que analice los cambios;
- `/rubber-duck` para explicar un problema y obtener otra perspectiva;
- `/fleet` para coordinar tareas independientes en paralelo;
- `/worktree` para aislar una tarea distinta;
- `/delegate` para enviar una tarea al agente en la nube de Copilot.

## Siguientes pasos

La mejor forma de mejorar con cualquier herramienta es seguir usándola. Úsala con código de producción, en proyectos personales o para crear esa pequeña aplicación que llevas años imaginando. Comparte lo que aprendas con el equipo y aprende de sus experiencias. Y, como siempre, explora la documentación.

Si quieres ampliar Tailspin Toys desde el terminal, continúa con la serie opcional [Foundry Backer Concierge][foundry]. Para comparar otros entornos, explora el [taller de VS Code][vscode], el [taller de la aplicación GitHub Copilot][app] o el [taller del agente en la nube de Copilot][cloud].

## Recursos

- [Acerca de GitHub Copilot CLI][about-cli]
- [Referencia de comandos de Copilot CLI][cli-reference]
- [Personalizar Copilot CLI][customize-cli]
- [Gestionar solicitudes de incorporación de cambios con Copilot CLI][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
