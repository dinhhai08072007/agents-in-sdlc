---
title: "Ejercicio 5: Iterar sobre el trabajo de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Supervisar y gestionar agentes][previous-lesson] |
|:--|

## Revisa el trabajo

Durante este taller has trabajado con GitHub Copilot en varias tareas para mejorar la experiencia de usuario y añadir funciones. Le pediste que añadiera documentación al código, creara una función de juegos relacionados para que el equipo de diseño pudiera iterar y añadiera funciones de accesibilidad, incluidos controles para los modos de alto contraste y claro. Revisemos los cambios y, si hace falta, demos indicaciones a Copilot para mejorarlos.

### Escenario

Como ya hemos señalado, los fundamentos del diseño de software y DevOps no cambian por usar IA generativa. Siempre debemos revisar el código generado y seguir el proceso habitual de DevOps. Revisaremos las propuestas de GitHub Copilot para la documentación, la función de juegos relacionados y las mejoras de accesibilidad antes de habilitar la revisión para el resto del equipo.

## Seguridad y agente en la nube de GitHub Copilot

Como el agente en la nube de Copilot realiza tareas de forma asíncrona y sin supervisión, se han establecido ciertas restricciones de seguridad:

- Copilot solo tiene acceso de lectura al repositorio y acceso de escritura **únicamente** a la rama que utilizará para el código.
- El agente en la nube se ejecuta en GitHub Actions, que crea un entorno independiente y efímero para el trabajo.
- Una persona debe aprobar cualquier flujo de trabajo de GitHub Actions antes de que se ejecute.
- [El acceso a recursos externos está limitado de forma predeterminada](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluidos los servidores MCP.

## Revisa la documentación generada

Empecemos por la primera solicitud de incorporación de cambios (PR) que generó el agente en la nube de GitHub Copilot: la que añade documentación al código. Usarás la interfaz habitual de PR de GitHub.com.

> [!NOTE]
> Al revisar la solicitud, puede que veas un aviso de que el cortafuegos ha bloqueado GitHub Copilot. **Es normal**: Copilot tiene acceso limitado a recursos externos de forma predeterminada, incluidas las llamadas a servidores MCP externos. Si quieres, puedes [personalizar o deshabilitar el cortafuegos del agente en la nube de Copilot][agent-firewall].

1. Vuelve al repositorio en github.com.
2. Selecciona **Pull Requests** para abrir la lista de solicitudes.
3. Abre la solicitud cuyo título sea parecido a **Add missing documentation**.

> [!NOTE]
> Si Copilot sigue trabajando, la solicitud tendrá la marca **[WIP]**. Espera a que termine. Puede tardar unos minutos; puedes descansar o repasar lo que has aprendido.

4. Cuando la solicitud esté lista, selecciona la pestaña **Files changed** y revisa los cambios.

    ![Pestaña Files changed](../../../_images/shared-pr-files-changed.png)

5. Explora el código actualizado, que ahora incluye comentarios TSDoc y otra documentación. Los cambios concretos pueden variar.

   Al revisarlos, busca comentarios TSDoc, convenciones de TypeScript y encabezados de comentarios. Estas indicaciones proceden de los archivos de instrucciones personalizadas que revisaste al principio del Ejercicio 2.

6. Cuando hayas revisado las actualizaciones y estés conforme, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Deberías ver un indicador de que hay flujos de trabajo pendientes de aprobación.
8. Si hay flujos de trabajo pendientes, selecciona **Approve and run workflows**.

    ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Los flujos de trabajo deberían aparecer en la sección de comprobaciones de la solicitud. Si todo va bien, las comprobaciones del proyecto para la aplicación Astro se superarán. El proceso puede tardar unos minutos.

## Pide cambios a GitHub Copilot

Trabajar con Copilot en una solicitud de incorporación de cambios no es una interacción unidireccional. Puedes mencionarlo en comentarios de la solicitud o en comentarios en línea del código, igual que harías con otra persona del equipo. Copilot verá esos comentarios e iniciará otra sesión para resolverlos. Como los resultados no son deterministas, no podemos darte un texto exacto. Algunas ideas de cambios que puedes pedir:

- Añadir encabezados de comentarios al principio de cada archivo de código con una breve descripción de su contenido.
- Añadir comentarios TSDoc a los archivos TypeScript y Astro.
- Crear un README que describa la estructura de la aplicación Astro.

1. Añade un comentario para pedir un cambio en la documentación generada y menciona a **@copilot**, como harías con cualquier otra persona. Usa una de las ideas anteriores u otra sugerencia sobre la documentación del código.
2. Selecciona **View Session** para observar el trabajo de Copilot. Fíjate en que inicia una sesión nueva para realizar las actualizaciones.
3. Puedes seleccionar **Back to pull request** para volver a la solicitud.

    ![Volver a la solicitud de incorporación de cambios](../../../_images/ex7-back-to-pr.png)

4. Cuando Copilot termine, debería aparecer una confirmación nueva en la solicitud.
5. Selecciona la pestaña **Files changed** para revisar los cambios.

Puedes seguir iterando hasta que estés conforme. Después, convierte el borrador en una solicitud lista para revisión y fusiónala con la rama principal.

![Convertir una solicitud en lista para revisión](../../../_images/ex7-ready-for-review.png)

## Revisa la función de juegos relacionados

Volvamos a la solicitud que Copilot generó para resolver la incidencia sobre la sección de juegos relacionados en la página de detalles.

1. Vuelve al repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Selecciona la solicitud cuyo título sea parecido a **Show related games on the game details page**.
4. Selecciona la pestaña **Files changed** para revisar el código generado.
5. Cuando hayas revisado las actualizaciones y estés conforme, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
6. Deberías ver un indicador de que hay flujos de trabajo pendientes de aprobación.
7. Si los hay, selecciona **Approve and run workflows**.

    ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

8. Los flujos de trabajo deberían aparecer en la sección de comprobaciones de la solicitud. Si todo va bien, las comprobaciones del proyecto para la aplicación Astro se superarán. El proceso puede tardar unos minutos.
9. **Opcional:** puedes cambiar a esta rama en el codespace para probar manualmente la función de juegos relacionados. Ve al codespace, abre el terminal y ejecuta estos comandos. Sustituye `<branch-name>` por el nombre de la rama creada por Copilot, por ejemplo, **copilot/fix-8**:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

¡Copilot ya ha creado la función de juegos relacionados! Como antes, puedes iterar con el agente en la nube para pedir cambios. Por ejemplo, puedes modificar el número de juegos relacionados que se muestran o pedir que se añadan encabezados de comentarios y comentarios TSDoc. Recuerda que esta tarea se asignó **antes** de actualizar las instrucciones personalizadas. Para pedir cambios, añade un comentario en la pestaña **Conversation**; Copilot lo verá e iniciará una sesión nueva.

## Revisa las funciones de accesibilidad

Por último, revisemos las funciones de accesibilidad implementadas por el agente personalizado. La solicitud debería incluir tanto el modo de alto contraste que pediste en el Ejercicio 3 como el modo claro que solicitaste desde Mission Control en el Ejercicio 4.

1. Vuelve al repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Selecciona la solicitud cuyo título sea parecido a **Add high contrast mode to website**.

> [!NOTE]
> Si Copilot sigue trabajando, la solicitud tendrá la marca **[WIP]**. Espera a que termine; puede tardar unos minutos.

4. Selecciona la pestaña **Files changed** para revisar el código generado.
5. Revisa la implementación y presta atención a:
   - los controles de interfaz para cambiar entre los modos;
   - el uso del almacenamiento local para conservar las preferencias;
   - los cambios de CSS o de estilos para los modos de alto contraste y claro;
   - los atributos de accesibilidad (etiquetas ARIA, navegación con teclado, etc.);
   - el código JavaScript/TypeScript que gestiona los cambios de modo.

6. Cuando hayas revisado las actualizaciones y estés conforme, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Deberías ver un indicador de que hay flujos de trabajo pendientes de aprobación.
8. Si los hay, selecciona **Approve and run workflows**.

    ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Los flujos de trabajo deberían aparecer en la sección de comprobaciones de la solicitud. Si todo va bien, las comprobaciones del proyecto para la aplicación Astro se superarán. El proceso puede tardar unos minutos.
10. **Opcional:** puedes cambiar a esta rama en el codespace para probar manualmente las funciones de accesibilidad. Abre el terminal y ejecuta estos comandos, sustituyendo `<branch-name>` por el nombre de la rama creada por Copilot:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    Después, inicia la aplicación y prueba los controles de los modos de alto contraste y claro en el navegador. Comprueba que funcionan y que la configuración se conserva al volver a cargar la página.

El agente personalizado de accesibilidad ha ayudado a Copilot a implementar estas funciones siguiendo prácticas recomendadas de accesibilidad. Si detectas algún problema o una posible mejora, puedes mencionar a **@copilot** en un comentario y pedirle que lo actualice, como hiciste con las solicitudes anteriores.

## Ejercicio opcional: sigue delegando

El agente en la nube resulta útil cuando puedes asignarle tareas reales de la lista de trabajo y ocuparte de otra cosa mientras tanto. Para practicarlo, crea algunas incidencias más en el repositorio y asígnaselas a Copilot. Por ejemplo:

- Crear un formulario para mostrar el interés de los patrocinadores en la página de detalles del juego.
- Implementar paginación en la página de la lista de juegos.
- Añadir validación de entradas y gestión de errores a los helpers de acceso a datos.

## Resumen

Has completado el itinerario del agente en la nube. En estas lecciones:

- **Has revisado los archivos de instrucciones personalizadas del repositorio** para observar más adelante su efecto en el trabajo del agente.
- **Has asignado incidencias al agente en la nube de Copilot** y has visto cómo configura el entorno, planifica y trabaja de forma asíncrona.
- **Has creado y utilizado un agente personalizado** para implementar funciones de accesibilidad, incluidos los modos de alto contraste y claro.
- **Has usado la página de agentes de Copilot como Mission Control** para supervisar y orientar la sesión de accesibilidad mientras estaba en curso.
- **Has revisado y actualizado las solicitudes de incorporación de cambios del agente en la nube**, mencionando a `@copilot` para pedir cambios y aprobando flujos de trabajo.

## Repaso y siguientes pasos

Has completado el itinerario del agente en la nube. Si quieres seguir explorando, los otros itinerarios complementan lo que has practicado:

- 🖥️ **[Itinerario de VS Code](../../vscode/)**: explora el modo de agente de Copilot Chat y la integración de MCP desde el IDE.
- 💻 **[Itinerario de CLI](../../cli/)**: completa dos cambios revisados desde el terminal con los modos Plan y Autopilot, skills de agente, un agente de QA personalizado, Playwright MCP, Agent Merge y un puente opcional `/delegate` al agente en la nube.

En tu propio repositorio, prueba estas ideas:

- Asigna al agente una incidencia de refactorización o cobertura de pruebas.
- Crea un agente personalizado para otro ámbito.
- Configura `copilot-setup-steps.yml` para otra pila tecnológica.

## Recursos

- [GitHub Copilot][github-copilot]
- [Acerca de los agentes de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para configurar el flujo de trabajo del agente en la nube de Copilot][cloud-agent-best-practices]
- [Configurar el cortafuegos del agente en la nube de Copilot][agent-firewall]

---

| [← Lección anterior: Supervisar y gestionar agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
