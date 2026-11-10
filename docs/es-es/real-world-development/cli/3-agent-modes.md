---
title: "Lección 3: Modos de agente, Plan y Autopilot"
description: "Usa Plan para acordar un enfoque, Autopilot para crear filtros a partir de una incidencia e Interactive para revisar el resultado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Empezamos por añadir una función pequeña al proyecto, pero los cambios más amplios necesitan un proceso más sólido. Por suerte, GitHub Copilot CLI está diseñado para adaptarse al flujo de trabajo de una organización y ayudarnos a crear lo que necesitamos de la forma adecuada. Esta es la primera de varias lecciones en las que seguirás un proceso de desarrollo habitual con agentes: partirás de una incidencia para crear una función, comprobarás el código, validarás el comportamiento y, finalmente, fusionarás el cambio en el proyecto.

> [!NOTE]
> A medida que desarrolles la función, seguirás usando la misma conversación y la misma rama. Normalmente se usarían ramas o solicitudes de incorporación de cambios diferentes para distintos tipos de archivos, pero simplificaremos el proceso para centrarnos en los conceptos principales.

En esta lección:

- iniciarás una conversación de Copilot a partir de una incidencia de GitHub;
- definirás los requisitos en el modo Plan;
- implementarás la función con el modo Autopilot;
- revisarás el código;
- validarás manualmente la función en un navegador reenviado.

Mientras sigues trabajando en esta función, actualizarás las instrucciones del repositorio, personalizarás la skill `quality-checks`, añadirás validación con MCP, crearás un agente de QA y abrirás la solicitud de incorporación de cambios.

## Escenario

El catálogo de Tailspin Toys crece y los visitantes necesitan filtrar los juegos por categoría y editorial. La incidencia de la lista de tareas describe la función, pero antes de escribir código hay que acordar detalles como la combinación de categorías. Usarás el modo Plan para decidir esos detalles y, después, autorizarás una implementación acotada con Autopilot.

## Contexto

Incorporar agentes de programación con IA al flujo de trabajo no cambia los fundamentos del desarrollo; de hecho, los hace aún más importantes. La mayoría de los desarrolladores siguen un proceso parecido a este:

1. Partir de una incidencia que describa el trabajo.
2. Crear un plan de implementación.
3. Crear y revisar el código.
4. Ejecutar pruebas para validar el código.
5. Validar manualmente la nueva función.
6. Crear una solicitud de incorporación de cambios (PR).
7. Cuando se haya revisado el código y se hayan superado las comprobaciones de integración continua, fusionarlo.

> [!NOTE]
> Los detalles concretos varían según el equipo y la organización, pero la mayoría de los procesos siguen alguna variante de estos pasos.

Al mantener este enfoque, te aseguras de que el código generado por IA cumpla los requisitos establecidos y pase por el mismo proceso de revisión que el código escrito manualmente.

## Modos de conversación

El **modo de conversación** controla la autonomía del agente. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para cambiar de modo:

- **Interactive**: trabajas con el agente. Propone cambios y espera tu respuesta antes de continuar.
- **Plan**: primero crea un plan y no puede editar archivos del proyecto.
- **Autopilot**: trabaja de forma autónoma: escribe código, ejecuta pruebas e itera hasta completar la tarea.

Empieza en el modo Plan, revisa el plan y, después, usa Autopilot para implementarlo.

## Empieza por la incidencia

Antes de empezar a trabajar en los filtros, vuelve al codespace y comprueba que el repositorio y el terminal estén preparados.

1. Vuelve al codespace. Si está detenido, reinícialo.
2. Confirma que se ha fusionado la solicitud de valoración con estrellas.
3. Si el terminal no está abierto, pulsa <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
4. Actualiza `main` y crea una rama para el trabajo de filtrado:

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Inicia Copilot CLI:

   ```bash
   copilot --yolo
   ```

6. Pulsa <kbd>Tab</kbd> dos veces para abrir la pestaña **Issues**.
7. Pulsa <kbd>A</kbd> para mostrar todas las incidencias.
8. Usa las flechas para resaltar la incidencia **Allow users to filter games by category and publisher**.
9. Pulsa <kbd>C</kbd> para añadir la incidencia al mensaje y volver a la pestaña **Session**.

Observa que el mensaje ahora empieza por `#7` (o un número parecido). El carácter `#` permite incluir en el contexto una incidencia o solicitud de incorporación de cambios (PR) de GitHub.

## Planifica la función de filtrado

La planificación permite decidir el enfoque para implementar una función o realizar una tarea antes de delegarla a Copilot. En una tarea compleja, siempre es recomendable dedicar tiempo a planificar. Cambiemos al modo Plan y pidamos a Copilot que cree el plan.

1. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para cambiar al modo Plan. Confirma que el indicador debajo del mensaje muestra **Plan**.
2. Después de la referencia a la incidencia que añadiste en el paso anterior, escribe:

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot empezará a elaborar el plan. Primero explorará el proyecto y, después, determinará el mejor enfoque.

3. Durante el proceso, Copilot puede preguntarte cómo debería funcionar el filtrado. Responde según tus preferencias; no hay respuestas incorrectas.
4. Cuando termine el plan, pulsa <kbd>Control</kbd>+<kbd>E</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) para expandirlo.
5. Desplázate por el plan y revísalo.
6. Pide a Copilot que revise cualquier parte que no refleje tus decisiones.

## Aprueba Autopilot

Ya tienes un plan escrito y revisado. Ahora es el momento de implementarlo. Deja que Copilot trabaje en la tarea con Autopilot.

Con Autopilot, Copilot puede iterar hasta considerar que ha completado el trabajo.

1. Selecciona **Accept plan and build on autopilot (recommended)** o la opción equivalente de tu versión.
2. Confirma que el indicador debajo del mensaje muestra **Autopilot**.
3. Observa cómo Copilot sigue el plan, genera código y ejecuta pruebas.

> [!NOTE]
> Al aprobar el plan, la implementación puede empezar inmediatamente, así que revísalo antes. Si Copilot informa de dependencias ausentes o de un conflicto de puertos, resuelve el problema de configuración antes de dar por completas las comprobaciones.

## Revisa y comprueba la implementación

Como con cualquier otro código, debes revisar el código generado antes de fusionarlo. Revisemos el código y ejecutemos el sitio para comprobar que todo funciona correctamente.

1. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para cambiar al modo Interactive. Confirma que el indicador de modo ya no muestra **Plan** ni **Autopilot**.
2. Escribe `/diff` y revisa la implementación de los filtros y las pruebas.
3. Compara el resultado con la incidencia y las decisiones que tomaste al planificar.
4. Cuando termines de revisar el código, pulsa <kbd>Esc</kbd> para salir de la vista de diferencias.
5. Revisa los resultados de las comprobaciones del proyecto y pide a Copilot que resuelva los errores que haya.

## Explora la nueva función

El código parece correcto, pero ¿se ejecuta? Inicia la aplicación como antes y ábrela en el navegador mediante el puerto reenviado de Codespaces.

1. Pide a Copilot que inicie la aplicación:

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Cuando Codespaces indique que el puerto `4321` está disponible, selecciona **Open in Browser**.
3. Prueba los filtros de categoría y editorial, así como las combinaciones que acordaste en el plan.
4. Confirma que las opciones para restablecer los filtros y los resultados vacíos coinciden con la incidencia y con tus decisiones.
5. Vuelve al codespace y pide a Copilot que detenga el servidor de desarrollo que inició.

## Resumen y siguientes pasos

Has usado distintos modos de conversación para crear y revisar una función. En esta lección:

- iniciaste una conversación de Copilot a partir de una incidencia de GitHub;
- definiste los requisitos en el modo Plan;
- implementaste la función con el modo Autopilot;
- revisaste el código;
- validaste manualmente la función en un navegador reenviado.

A continuación, profundizarás en cómo se genera el código y cómo seguir las prácticas documentadas mediante [instrucciones personalizadas][next-lesson].

## Recursos

- [Autopilot en GitHub Copilot CLI][autopilot]
- [Referencia de comandos de Copilot CLI][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
