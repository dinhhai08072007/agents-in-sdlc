---
title: "Ejercicio 3: Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## ¿Qué son los agentes personalizados?

Los [agentes personalizados][custom-agents-concept] de GitHub Copilot permiten crear asistentes de IA especializados para tareas o ámbitos concretos del flujo de trabajo de desarrollo. Al definir agentes mediante archivos Markdown en la carpeta `.github/agents` del repositorio, puedes proporcionar a Copilot instrucciones específicas, prácticas recomendadas, patrones de programación y conocimientos del dominio para que realice determinados tipos de trabajo con mayor eficacia. Los equipos pueden plasmar su experiencia en agentes reutilizables: por ejemplo, un agente de accesibilidad que aplique el cumplimiento de [WCAG][wcag], uno de seguridad que siga prácticas de programación segura o uno de pruebas que mantenga patrones coherentes.

Los agentes personalizados se definen en archivos Markdown de la carpeta `.github/agents` del proyecto o, de forma global, en `~/.copilot/agents`. Cada archivo tiene frontmatter YAML con, al menos, los campos `name` y `description`, seguido de una instrucción Markdown que define el comportamiento, la experiencia y las indicaciones del agente.

### Diferencias entre los agentes personalizados y las habilidades de agente

Los agentes personalizados y las [habilidades de agente][agent-skills-concept] tienen algunos aspectos en común. Ambos se definen principalmente mediante archivos Markdown y explican a una IA cómo realizar determinadas operaciones. La forma más sencilla de distinguirlos es esta: un **agente personalizado** es quien realiza el trabajo y las **habilidades** son las herramientas.

Los agentes personalizados tienen su propia ventana de contexto y están diseñados para coordinar habilidades (e incluso otros agentes) durante el trabajo. En este ejercicio, el agente personalizado de accesibilidad revisará y actualizará el sitio según las pautas de accesibilidad. Como parte de ese trabajo, podría utilizar habilidades como una para gestionar el flujo de trabajo de una solicitud de incorporación de cambios o para ejecutar y gestionar pruebas.

> [!NOTE]
> No hay una única forma «correcta» de crear un agente personalizado. Como con cualquier herramienta de IA, prueba distintas opciones e itera para encontrar la que mejor se adapte a tu entorno y a tus situaciones.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

En este ejercicio aprenderás a:

- definir agentes personalizados.
- asignar una tarea a un agente personalizado.

## Escenario

Tailspin Toys se compromete a que todas las personas puedan usar su plataforma de financiación colectiva, con independencia de sus capacidades visuales o preferencias. Comentarios recientes de usuarios indican que algunas personas tienen dificultades para leer el tema oscuro porque el contraste entre el texto y el fondo es insuficiente. Para resolver este problema de accesibilidad, el equipo de diseño ha solicitado un modo de alto contraste que se pueda activar y desactivar.

Como la accesibilidad es fundamental, quieres implementar esta función cuanto antes. Para generarla, utilizarás un agente personalizado.

## Revisa el agente personalizado de accesibilidad

Ya se ha creado un agente personalizado de accesibilidad. Revisemos su contenido para entender cómo guiará a Copilot.

Vuelve al codespace y revisa el archivo del agente personalizado de accesibilidad:

1. Abre `.github/agents/accessibility.md`.
2. Observa el frontmatter YAML, que contiene los campos `name` y `description`.

> [!CAUTION]
> Los campos `name` y `description` son obligatorios en el frontmatter de los agentes personalizados.

3. Lee las secciones siguientes, que describen:
   - las responsabilidades principales al generar código para un sitio web accesible.
   - las prácticas recomendadas de accesibilidad.
   - ejemplos de código HTML, CSS y JavaScript.
   - errores y problemas frecuentes.

## Crea y asigna una incidencia

Mission control es el lugar central para trabajar con todos los agentes del entorno. Puedes asignar tareas al agente en la nube de Copilot, supervisarlas y redirigirlas o darles más indicaciones. Empecemos por asignar a Copilot la tarea de crear el modo de alto contraste.

1. Ve al repositorio.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de una incidencia nueva.
4. Selecciona **Blank issue** para crear una incidencia en blanco.
5. Escribe `Add high contrast mode to website` en **Title**.
6. Escribe lo siguiente en **Description**:

    ```plaintext
    We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
    ```

7. Selecciona **Create** para crear la incidencia.
8. A la derecha, selecciona **Assign to Copilot** para abrir el cuadro de asignación.
9. Selecciona **Accessibility agent** en la lista de agentes personalizados.

    ![Captura de pantalla de la asignación del agente en la nube, con los campos de agente personalizado y de accesibilidad resaltados](../../../_images/ex5-select-custom-agent.png)

10. Selecciona **Assign**.
11. ¡Copilot empezará a trabajar en segundo plano!

## Resumen y siguientes pasos

En esta lección has explorado los [agentes personalizados][custom-agents] de GitHub Copilot, asistentes especializados en tareas y ámbitos concretos. Los agentes personalizados permiten plasmar los conocimientos y estándares del equipo en agentes reutilizables que guían el trabajo de Copilot.

Has aprendido a:

- definir agentes personalizados.
- asignar una tarea a un agente personalizado.

Mientras Copilot implementa el modo de alto contraste, puedes [supervisar y orientar la sesión del agente][next-lesson] desde Mission Control.

## Recursos

- [Acerca de los agentes personalizados][custom-agents]
- [Prepararse para usar agentes personalizados en una organización][org-custom-agents]
- [Prepararse para usar agentes personalizados en una empresa][enterprise-custom-agents]

---

| [← Lección anterior: Agente en la nube de GitHub Copilot][previous-lesson] | [Siguiente lección: Supervisar y gestionar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
