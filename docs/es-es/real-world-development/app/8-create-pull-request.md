---
title: "Lección 8 - Crear y combinar la PR de la funcionalidad"
description: "Revisa conjuntamente el filtrado, las instrucciones, la actualización de la habilidad, el perfil QA y las pruebas; después, crea una PR y utiliza Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

La implementación del filtrado, las actualizaciones de instrucciones y de la habilidad, el perfil de control de calidad (QA) y las pruebas están guardados en una sola rama. Es hora de revisarlos juntos y abrir una solicitud de incorporación de cambios. Ya has combinado personalmente la solicitud de incorporación de cambios (PR) de valoraciones por estrellas; esta vez permitirás que **Agent Merge** gestione el proceso.

> [!NOTE]
> Normalmente separaríamos la funcionalidad, las actualizaciones de instrucciones, la modificación de la habilidad y el agente QA en varias PR. Para agilizar el taller, has mantenido todo el flujo de filtrado y calidad en una sesión y una rama, y todo ese trabajo se incluirá en esta PR.

En esta lección:

- aprenderás qué es Agent Merge y cómo automatiza el ciclo de vida de una combinación.
- examinarás la PR completa de la funcionalidad y las pruebas de verificación.
- autorizarás Agent Merge solo después de la revisión y confirmarás que la PR está combinada.

## Escenario

A lo largo del flujo de filtrado, has utilizado Copilot para planificar, implementar y verificar una funcionalidad. Ahora Tailspin Toys quiere automatizar el trabajo restante de la PR y mantener la autorización para combinar bajo el control del desarrollador.

## Introducción a Agent Merge

**Agent Merge** automatiza el trabajo restante para incorporar una solicitud de cambios en la aplicación GitHub Copilot. Al habilitarlo, la sesión de la aplicación lee la solicitud y resuelve lo que la bloquea: corrige comprobaciones de integración continua (CI) que fallan, responde a comentarios de revisión y vuelve a aplicar la base cuando hace falta. Después la fusiona en cuanto GitHub lo permite. Se ejecuta en segundo plano, continúa después de reiniciar la aplicación y se desactiva cuando se fusiona la solicitud.

Hasta ahora has seleccionado **Merge pull request** personalmente. Agent Merge puede encargarse de esa tarea, pero su capacidad para editar código y fusionar sigue necesitando tu autorización explícita. Revisa las acciones que tiene permitidas y el trabajo antes de concederle permiso para fusionar.

## Utilizar Agent Merge para gestionar la PR

Ya has creado y revisado todo el código. Revisémoslo en conjunto, creemos la PR y dejemos que Agent Merge gestione el proceso.

1. Utiliza el selector de agentes para seleccionar **Default agent**.
2. Selecciona el menú desplegable junto a **Create PR**.
3. Selecciona **Agent Merge**. El botón cambiará a **Agent Merge**.
4. Selecciona el botón **Agent Merge** para iniciar el proceso. Elegir la opción en el menú desplegable no lo inicia por sí solo.

Se iniciará el proceso de Agent Merge. El agente:

- creará la solicitud de incorporación de cambios con un título y una descripción;
- si iniciaste la sesión desde una incidencia, la mencionará en el cuerpo de la descripción;
- volverá a aplicar la base o resolverá posibles conflictos de fusión con la rama de destino;
- supervisará el proceso de CI para comprobar que todas las comprobaciones se superen;
- revisará los comentarios de otros desarrolladores y de la revisión de código de Copilot, y hará cambios para resolverlos;
- opcionalmente, fusionará la solicitud cuando todo se complete correctamente.

Pidamos a Agent Merge que fusione también la solicitud cuando todo se supere.

5. Revisa las diferencias de la solicitud y las pruebas de verificación antes de conceder permiso para fusionarla.
6. Selecciona el menú desplegable junto a **Agent Merge**.
7. Comprueba que **Merge pull request** está marcado.
8. Pide explícitamente al agente que fusione la solicitud:

    ```plaintext
    Merge the PR once all required checks and approvals pass. Do not bypass repository protections.
    ```

9. Supervisa el progreso y resuelve los bloqueos que indique. Antes de continuar, confirma que el estado de la solicitud sea **Merged**. Tener habilitado el permiso de fusión o un agente trabajando no confirma que se haya completado.

> [!IMPORTANT]
> Agent Merge no elude las protecciones del repositorio ni los permisos ausentes. Resuelve esos bloqueos antes de continuar.

## Resumen y pasos siguientes

Has automatizado varias partes del proceso de desarrollo, como la generación, las pruebas y la validación de código, y ahora también el proceso de solicitud de incorporación de cambios. En concreto:

- has aprendido qué es Agent Merge y cómo automatiza el ciclo de vida de una combinación.
- has examinado la PR completa de la funcionalidad y las pruebas de verificación.
- has autorizado Agent Merge solo después de la revisión y has confirmado que la PR estaba combinada.

A continuación, [utilizarás un lienzo existente y crearás uno de clasificación][next-lesson] para explorar una forma más completa de examinar, planificar y visualizar el trabajo con el agente.

## Recursos

- [Gestionar incidencias y solicitudes de incorporación de cambios con la aplicación GitHub Copilot][managing-issues-prs]
- [Acerca de la aplicación GitHub Copilot][about-copilot-app]

[next-lesson]: ../9-canvases/
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app