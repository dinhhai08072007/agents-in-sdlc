---
title: "Ejercicio 4: Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En los últimos ejercicios has pedido al agente en la nube de Copilot que se ocupe de tres tareas diferentes para mejorar la experiencia de usuario y añadir funciones. Aunque el agente en la nube está diseñado para trabajar de forma autónoma y asíncrona, sigue siendo importante poder supervisar esas tareas.

Hay varias herramientas para gestionar las tareas asignadas al agente en la nube, incluida [la página de agentes][agents-page] de GitHub.com. Desde este centro de control puedes ver todas las tareas de los agentes que tienen solicitudes de incorporación de cambios abiertas. Puedes examinar las operaciones realizadas e incluso orientar una sesión que esté en curso.

En esta lección:

- explorarás la página de agentes para supervisar sus tareas en la nube.
- orientarás una sesión en curso para pedir que incluya otra función.

## Escenario

Después de asignar al agente la implementación de un modo de alto contraste, el equipo decidió que también sería buen momento para añadir un modo claro. Como ya se estaba actualizando el estilo del sitio y añadiendo controles de modo, parecía lógico incluir esta función. Orientarás el trabajo del agente para que añada ambos modos.

## Revisa las tareas del agente en la nube de Copilot

Veamos el estado actual de las tareas asignadas al agente en la nube de Copilot.

1. Ve a la [página de agentes de Copilot](https://github.com/copilot/agents).
2. Observa la lista de tareas en el panel principal y en el panel izquierdo. Deberías ver las tareas que has asignado a Copilot, entre ellas:
   - actualizar la documentación del código;
   - generar API para modificar juegos;
   - añadir un modo de alto contraste al sitio web.
3. Selecciona una de las tareas en curso. Revisa las operaciones que ha realizado Copilot, por ejemplo:
   - obtener el código del repositorio;
   - crear el entorno de trabajo;
   - configurar servidores MCP;
   - realizar distintas acciones para completar la tarea asignada.

> [!NOTE]
> Los pasos concretos de la lista variarán según el estado del trabajo de Copilot y el enfoque que haya seguido.

4. Observa también el panel de solicitudes de incorporación de cambios que aparece a la derecha. Permite consultar la solicitud y los archivos modificados para seguir el trabajo.

## Orienta al agente en la nube

Ahora que has visto las tareas activas, pídele a Copilot que añada el modo claro mientras trabaja en el modo de alto contraste.

1. Selecciona la sesión relacionada con el modo de alto contraste. El título exacto dependerá del nombre que haya elegido Copilot y del estado del trabajo.

    ![Sesión de accesibilidad en Mission Control](../../../_images/ex6-accessibility-session.png)

2. Observa la sesión durante unos minutos hasta que indique que ha terminado la configuración y ha empezado a trabajar. Lo notarás cuando aparezcan mensajes parecidos a los siguientes.
3. En el cuadro de diálogo **Steer active session while Copilot is working**, escribe este mensaje:

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Captura de pantalla de la tarea del agente en la nube en la página de agentes, con el cuadro de diálogo para orientar la sesión activa resaltado](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pulsa <kbd>Enter</kbd> para enviar el mensaje.
5. Observa cómo Copilot lo confirma y lo incorpora a su flujo de trabajo.

## Deja que Copilot trabaje

Como antes, Copilot se ocupará de la tarea actualizada. Incorporará la nueva solicitud a su flujo de trabajo cuando termine el paso que estaba ejecutando al recibir el mensaje.

Esto también tardará varios minutos; puedes aprovechar para repasar todo lo que has aprendido y explorado hasta ahora.

## Resumen y siguientes pasos

En esta lección has explorado la página de agentes de Copilot, el centro donde puedes supervisar y orientar las tareas del agente en la nube de GitHub Copilot. Desde ahí puedes seguir las tareas activas y completadas, revisar el trabajo y redirigir las sesiones en curso para ajustar su alcance o proporcionar más indicaciones.

Has aprendido a:

- explorar Mission Control y la página de agentes para supervisar tareas del agente en la nube.
- redirigir una sesión en curso para pedir otra función.

Mientras Copilot termina de trabajar en las funciones de accesibilidad, puedes [iterar sobre las solicitudes de incorporación de cambios que ha creado][next-lesson].

## Recursos

- [Página de agentes de Copilot][agents-page]
- [Acerca de los agentes personalizados][custom-agents]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
