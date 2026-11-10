---
title: "Lección 8: Crear y fusionar la solicitud de la función"
description: "Revisa los filtros, las instrucciones, la actualización de la skill, el perfil de QA y las pruebas; después crea una solicitud y usa Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

La implementación del filtrado, las instrucciones actualizadas, la modificación de la skill, el perfil de control de calidad (QA) y las pruebas están guardados en una rama. Es el momento de revisarlo todo y abrir una solicitud de incorporación de cambios. Fusionaste manualmente la solicitud de valoración con estrellas; esta vez dejarás que **Agent Merge** gestione el proceso.

> [!NOTE]
> Normalmente separaríamos la función, las actualizaciones de instrucciones, el cambio de la skill y el agente de QA en varias solicitudes. Para simplificar el taller, has mantenido todo el trabajo de filtrado y calidad en una misma conversación y rama, y lo incluirás en esta solicitud.

En esta lección:

- aprenderás qué es Agent Merge y cómo automatiza el ciclo de vida de una solicitud;
- revisarás el cambio completo de la función y las pruebas de verificación;
- crearás la solicitud de filtrado;
- habilitarás Agent Merge solo después de revisar el trabajo y confirmarás que la solicitud se ha fusionado.

## Escenario

Durante el proceso de filtrado has usado Copilot para planificar, implementar y verificar una función. Ahora Tailspin Toys quiere automatizar el trabajo restante de la solicitud, pero mantener bajo el control del desarrollador la autorización para fusionarla.

## Presentación de Agent Merge

**Agent Merge** automatiza el trabajo restante para incorporar una solicitud. Al habilitarlo, Copilot resuelve los bloqueos: corrige las comprobaciones de integración continua (CI) que fallan, responde a comentarios de revisión y vuelve a aplicar la base cuando hace falta. Después habilita la fusión automática de GitHub si el repositorio lo permite.

Hasta ahora seleccionabas **Merge pull request**. Agent Merge puede encargarse de esa tarea. Revisa el trabajo y confirma que está listo antes de habilitarlo.

## Deja que Agent Merge gestione la solicitud

Ya está creado el código. Revisémoslo, creemos la solicitud y dejemos que Agent Merge se ocupe del resto.

1. Vuelve al codespace.
2. Abre el cuadro de agente con `/agent`.
3. Selecciona **Default** en la lista y pulsa <kbd>Enter</kbd>.
4. Crea una solicitud con el comando `/pr create`.
5. Activa Agent Merge con `/pr agentmerge`.
6. Copilot supervisará el proceso de integración continua de la solicitud. Cuando todo se complete correctamente, la fusionará.
7. Confirma que Copilot muestra un mensaje parecido a «PR #14 was squash-merged successfully».

> [!IMPORTANT]
> Agent Merge no omite las aprobaciones obligatorias, la protección de ramas, las colas de fusión, la configuración del repositorio ni los permisos que falten. Si se bloquea, revisa el motivo indicado y completa manualmente la fusión revisada cuando el repositorio lo permita.

## Resumen y siguientes pasos

Has automatizado varias partes del proceso de desarrollo: generar código, probarlo y validarlo, y gestionar la solicitud de incorporación de cambios. En esta lección:

- aprendiste qué es Agent Merge y cómo automatiza el ciclo de vida de una solicitud;
- revisaste el cambio completo de la función y las pruebas de verificación;
- creaste la solicitud de filtrado;
- habilitaste Agent Merge después de revisar el trabajo y confirmaste que la solicitud se fusionó.

A continuación, [explorarás otros comandos slash útiles de Copilot CLI][next-lesson] para gestionar el contexto, los modelos, el uso compartido y la delegación opcional al agente en la nube.

## Recursos

- [Gestionar solicitudes de incorporación de cambios con Copilot CLI][manage-prs]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
