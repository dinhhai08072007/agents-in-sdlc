---
title: "Ejercicio 2: Agente en la nube de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Instrucciones personalizadas][previous-lesson] |
|:--|

Es probable que casi todas las organizaciones tengan problemas de deuda técnica. Puede tratarse de problemas de seguridad sin resolver, código heredado que necesita actualizarse o solicitudes de funciones que llevan mucho tiempo pendientes porque no había tiempo para implementarlas. El agente en la nube de GitHub Copilot está diseñado para realizar tareas como actualizar código y añadir funciones de forma autónoma. Cuando termina, genera un borrador de solicitud de incorporación de cambios para que lo revise una persona. Así puedes delegar tareas tediosas, acelerar el desarrollo y dedicar más tiempo a asuntos de mayor alcance.

Con el agente en la nube de Copilot aprenderás a:

- personalizar el entorno de generación de código;
- garantizar que las operaciones se realicen de forma segura;
- crear incidencias con un alcance claro;
- asignar incidencias a Copilot.

## Escenarios

Tailspin Toys quiere resolver parte de su deuda técnica. Los contratistas que crearon la primera versión del sitio dejaron la documentación en mal estado: comprobarás que falta por completo. Como primer paso, el equipo quiere añadir comentarios TSDoc a todas las funciones exportadas de la aplicación.

Además, el equipo de diseño quiere facilitar el descubrimiento de juegos. Quiere mostrar en la página de detalles de cada juego otros títulos de la misma categoría para que la gente pueda seguir explorando. Por ahora no necesita una implementación pulida; solo algo que permita hacer pruebas de aceptación de la experiencia. Este trabajo está bloqueado, pero hay otras incidencias con prioridad más alta.

Son tareas que pueden quedar relegadas con facilidad y que se pueden asignar al agente en la nube de Copilot. Mientras el agente trabaja de forma asíncrona, el desarrollador puede ocuparse de otras tareas y volver después para revisar los resultados.

## Presentación del agente en la nube de GitHub Copilot

El [agente en la nube de GitHub Copilot](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) puede realizar tareas en segundo plano de forma similar a un desarrollador. Igual que con el trabajo de otra persona, hay varias formas de asignarle tareas; por ejemplo, [asignándole una incidencia de GitHub a Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Cuando se le asigna una incidencia, Copilot crea un borrador de solicitud de incorporación de cambios para registrar el progreso, configura un entorno y empieza a trabajar. Puedes consultar la sesión mientras está en curso o una vez completada. Cuando la solución propuesta esté lista para revisarla, Copilot te mencionará en la solicitud.

## La importancia de definir bien las tareas

GitHub Copilot no hace magia: no puedes dejar de pensar en la tarea. Incluso las operaciones que parecen sencillas pueden volverse complejas al examinarlas con detalle, así que [define cuidadosamente el alcance de las tareas para el agente en la nube de Copilot][cloud-agent-best-practices]. Trátalo como a un compañero de programación basado en IA: trabaja por etapas, aprende, experimenta y adapta el enfoque. Los fundamentos del desarrollo de software no cambian por usar IA generativa.

## Instrucciones personalizadas de este repositorio

En ejercicios anteriores has visto cómo las instrucciones personalizadas orientan a Copilot. Si ya completaste un ejercicio práctico sobre instrucciones personalizadas, habrás visto cómo se usa `.github/copilot-instructions.md`. Si no, este es un buen momento para revisarlo. Antes de asignar trabajo al agente en la nube de Copilot, examina en modo de solo lectura los archivos de instrucciones que ya incluye este repositorio. Así podrás observar su efecto más adelante.

Abre estos archivos en la interfaz web de GitHub del repositorio o en un codespace si ya tienes uno abierto:

- `.github/copilot-instructions.md`: revisa la sección **Code standards**, especialmente las convenciones de TypeScript y los comentarios TSDoc.
- `.github/instructions/unit-tests.instructions.md`: observa el frontmatter `applyTo`, que aplica estas instrucciones a los archivos `**/*.test.ts`.

Cuando asignes al agente en la nube la incidencia `Code lacks documentation` en la sección siguiente, busca en la solicitud resultante comentarios TSDoc, convenciones de TypeScript y encabezados de comentarios. Estas indicaciones proceden de los archivos de instrucciones; volveremos a señalarlas al revisar las solicitudes en un ejercicio posterior.

## Configura el entorno de desarrollo del agente en la nube de Copilot

Crear código suele requerir un entorno específico y la ejecución de scripts de configuración para garantizar que todo esté preparado, independientemente de quién realice el trabajo. Lo mismo ocurre cuando asignas tareas a Copilot, que trabaja de forma parecida a un ingeniero de software.

El agente en la nube usa [GitHub Actions][github-actions] como entorno de trabajo. Puedes personalizarlo con un [flujo de trabajo de configuración especial][setup-workflow], definido en `.github/workflows/copilot-setup-steps.yml`, que se ejecuta antes de que el agente empiece. Así tendrá acceso a las herramientas y dependencias necesarias. Este flujo de trabajo ya está configurado para facilitar el taller y permitirte explorar esta función. Proporciona a Copilot Node.js, las dependencias del proyecto, los navegadores necesarios para las pruebas de extremo a extremo y la base de datos SQLite migrada y con datos iniciales de la aplicación Astro:

```yaml
name: "Copilot Setup Steps"

# Te permite probar los pasos de configuración en la pestaña "Actions" del repositorio
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Asigna solo los permisos mínimos necesarios para *tus pasos*. Copilot recibirá su propio token para sus operaciones.
    permissions:
      # Si quieres clonar el repositorio como parte de la configuración, por ejemplo, para instalar dependencias, necesitarás el permiso `contents: read`. Si no lo clonas en tus pasos, Copilot lo hará automáticamente cuando estos terminen.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuración de frontend y aplicación: Node.js (ahora toda la aplicación usa Astro y Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migra y carga la base de datos SQLite local para que las compilaciones y pruebas tengan datos.
      - name: Set up the database
        run: npm run db:setup
```

El archivo se parece a cualquier otro flujo de trabajo de GitHub, pero hay algunos puntos importantes:

- Contiene un único trabajo llamado `copilot-setup-steps`. GitHub Actions lo ejecuta antes de que Copilot empiece a trabajar en la solicitud de incorporación de cambios.
- El activador `workflow_dispatch` permite ejecutar manualmente el flujo de trabajo desde la pestaña **Actions** del repositorio. Así puedes comprobar que funciona correctamente sin esperar a que Copilot lo ejecute.

## Añade documentación

Aunque todo el mundo reconoce la importancia de la documentación, muchos proyectos tienen información obsoleta o carecen de ella. Esta deuda técnica suele quedar desatendida, lo que reduce la productividad, dificulta el mantenimiento del código y complica la incorporación de nuevos desarrolladores. Copilot destaca en la creación de documentación, así que esta es una buena tarea para asignar al agente en la nube. Trabajará en segundo plano para generar la documentación necesaria. En un ejercicio posterior volverás a revisar los resultados.

1. En una nueva pestaña del navegador, ve al repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de una incidencia nueva.
4. Selecciona **Blank issue** para crear la incidencia.
5. Escribe `Code lacks documentation` en **Title**.
6. Escribe lo siguiente en **Description**:

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. Selecciona **Create** para crear la incidencia.
8. A la derecha, selecciona **Assign to Copilot** para abrir el cuadro de asignación.

    ![Asignación de una incidencia a Copilot](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.

    ![Detalles de la asignación a Copilot](../../../_images/ex4-assign-copilot-details.png)

10. Selecciona la pestaña **Pull Requests**.
11. Abre la solicitud de incorporación de cambios (PR) que se acaba de generar. Su título será parecido a `[WIP]: Code lacks documentation`. Si todavía no aparece, espera un momento y actualiza la ventana del navegador.
12. Después de unos minutos, Copilot debería haber creado una lista de tareas.

> [!NOTE]
> La lista de tareas puede tardar varios minutos en aparecer en la solicitud. Copilot está creando su entorno (ejecutando el flujo de trabajo anterior), analizando el proyecto y determinando cómo abordar la tarea.

13. Revisa la lista y las tareas que se van a realizar.
14. Desplázate hacia abajo por la cronología de la solicitud. Deberías ver un aviso de que Copilot ha empezado a trabajar en la incidencia.
15. Selecciona el botón **View session**.

    ![Vista de la sesión de Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Puede que tengas que actualizar la ventana para ver el indicador actualizado.

16. Desplázate por la sesión en directo y observa cómo Copilot resuelve el problema: explora el código, analiza la situación, se detiene a pensar y decide cómo proceder, y crea código.

El proceso puede tardar varios minutos. Uno de los objetivos principales del agente en la nube de Copilot es trabajar de forma asíncrona para que puedas ocuparte de otras tareas. Aprovecharemos esa función: asignaremos otra tarea al agente y, mientras trabaja, nos centraremos en añadir funciones a la aplicación.

## Añade una sección de juegos relacionados a la página de detalles

Una de las ventajas del agente en la nube de GitHub Copilot es que permite repartir el trabajo. Puedes ocuparte de una tarea mientras él se encarga de otra. Añadir una sección de juegos relacionados para el equipo de diseño quizá no lleve mucho tiempo, pero podrías dedicar ese tiempo a otras tareas. Asignemos esta tarea al agente en la nube de Copilot.

1. Vuelve al repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de una incidencia nueva.
4. Selecciona **Blank issue** para usar la plantilla en blanco.
5. Escribe `Show related games on the game details page` en **Title**.
6. Escribe lo siguiente en **Description**:

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. Selecciona **Create** para crear la incidencia.
8. A la derecha, selecciona **Assign to Copilot** para abrir el cuadro de asignación.

    ![Asignación de una incidencia a Copilot](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.

Al poco tiempo, aparecerán varios 👀 en el primer comentario de la incidencia: indican que Copilot ya está trabajando.

![Copilot usa el emoji de ojos para indicar que está trabajando en la incidencia](../../../_images/ex4-issue-eyes-emoji.png)

Copilot ya está trabajando en la segunda solicitud. El agente en la nube funciona de manera parecida a un ingeniero de software: no hace falta supervisarlo continuamente, pero sí revisar el resultado cuando termine. Ahora veremos cómo crear y usar agentes personalizados.

## Resumen y siguientes pasos

En esta lección has explorado el [agente en la nube de GitHub Copilot][copilot-agents]. Puedes asignarle incidencias para que trabaje de forma asíncrona. Copilot puede ayudar a resolver deuda técnica, crear funciones o migrar código de un marco de trabajo a otro.

Has aprendido a:

- personalizar el entorno de generación de código;
- garantizar que las operaciones se realicen de forma segura;
- crear incidencias con un alcance claro;
- asignar incidencias a Copilot.

Mientras el agente en la nube trabaja, puedes crear y usar agentes personalizados. [El agente en la nube también puede usar servidores MCP][cloud-agent-mcp] y recibe instrucciones personalizadas, que ya exploramos en módulos anteriores.

## Recursos

- [Acerca del agente en la nube de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para configurar el flujo de trabajo del agente en la nube de Copilot][cloud-agent-best-practices]

| [Siguiente lección: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
