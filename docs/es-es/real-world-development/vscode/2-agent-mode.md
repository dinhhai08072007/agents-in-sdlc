---
title: "Ejercicio 2: Añadir funciones con el modo de agente de Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Incluso las actualizaciones más sencillas de una aplicación suelen requerir cambios en varios archivos y operaciones como ejecutar pruebas. Como desarrollador, normalmente tienes que localizar todos los archivos necesarios, hacer los cambios, ejecutar las pruebas, depurar, averiguar qué archivo te has dejado y volver a modificarlo. Y así sucesivamente.

El modo de agente de Copilot te ayuda con este proceso.

El modo de agente de Copilot está diseñado para trabajar de forma más autónoma en el IDE. Se comporta de manera parecida a un desarrollador: primero explora la estructura del proyecto, realiza los cambios necesarios, ejecuta tareas como las pruebas y corrige automáticamente los errores que encuentra. Veamos cómo usarlo para añadir funciones al sitio.

> [!NOTE]
> Aunque los nombres se parecen, el modo de agente y el agente en la nube ofrecen experiencias distintas. El modo de agente trabaja en el IDE para facilitar ciclos rápidos de comentarios e interacción. El agente en la nube actúa como un compañero que trabaja de forma asíncrona con incidencias y solicitudes de incorporación de cambios.

En este ejercicio aprenderás a:

- hacer que el modo de agente de Copilot explore el proyecto e identifique archivos relacionados;
- usar el modo de agente de GitHub Copilot para implementar funciones en la interfaz y la capa de datos;
- revisar los cambios y las pruebas generados por el modo de agente antes de incorporarlos al código.

## Escenario

A medida que aumenta la lista de juegos, quieres permitir que los usuarios filtren por categoría y editorial. En el ejercicio anterior ya añadiste un helper de editoriales. Ahora completarás con el modo de agente de Copilot el resto del trabajo de la capa de datos, la interfaz y las pruebas.

## Ejecuta el sitio web de Tailspin Toys

Antes de hacer cambios, exploremos el sitio web de Tailspin Toys para conocer sus funciones actuales.

El sitio es una plataforma de financiación colectiva de juegos de mesa con temática de desarrollo. Permite mostrar juegos y sus detalles. Toda la aplicación Astro genera páginas HTML estáticas durante la compilación. Las páginas consultan directamente una base de datos SQLite local mediante Drizzle ORM en el frontmatter; no hay una API de backend independiente ni un framework de interfaz del lado del cliente. Los helpers de acceso a datos reutilizables están en `src/lib/`, y la interactividad se implementa con pequeños elementos `<script>` de Astro que usan las API DOM estándar.

### Inicia el sitio web

Para facilitar la ejecución del sitio, se ha creado una tarea `npm run dev` que inicia el único servidor de desarrollo de Astro. Para ejecutarlo en GitHub Codespaces:

1. Vuelve al codespace. Deberías estar en `main` después del ejercicio anterior.
2. Abre un terminal nuevo en el codespace pulsando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crea una rama nueva para este trabajo de filtrado:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Ejecuta este comando para iniciar el sitio:

   ```bash
   npm run dev
   ```

   Cuando se inicie el servidor de desarrollo de Astro, verás un mensaje con la URL, parecido a este:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Si aparece un cuadro de diálogo para abrir una ventana del navegador en `http://localhost:4321`, ciérralo seleccionando **x**.

5. Abre el sitio manteniendo pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y seleccionando en el terminal la dirección del servidor Astro: `http://localhost:4321`.

> [!NOTE]
> En un codespace, al seleccionar el enlace localhost en el terminal, se redirige automáticamente a `https://<your-codespace-name>-4321.app.github.dev/`. Es un túnel privado al codespace, que ahora aloja el servidor web.

### Explora el sitio web

Con el sitio en ejecución, puedes explorar sus funciones principales:

- **Página de inicio**: muestra juegos de mesa con sus títulos, imágenes y descripciones.
- **Página de detalles del juego**: al seleccionar un juego, se abre una página con más información, como el título, la descripción, la editorial y la categoría.

## Explora la lista de tareas con Copilot

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar los ejercicios siguientes, vuelve al codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo varían según el ejercicio; se indicarán cuando corresponda.

La implementación inicial del sitio funciona, pero queremos añadir capacidades. Empecemos por revisar la lista de trabajo. Al crear el proyecto desde la plantilla, se generaron automáticamente varias incidencias. Pide a GitHub Copilot que te las muestre.

1. En el menú desplegable de agentes de la vista Chat, selecciona **Agent**. El agente planifica e implementa cambios en varios archivos de forma autónoma, ejecuta comandos de terminal y llama a herramientas.

   ![Captura de pantalla del selector de agentes en la vista Chat.](../../../_images/shared-chat-mode-selector.png)

2. Selecciona **Claude Sonnet 4.5** en la lista de modelos disponibles.

> [!CAUTION]
> Los autores de este taller no están recomendando un modelo por encima de otro. Al crear el taller, usamos Claude Sonnet 4.5 y lo indicamos para que las sugerencias de código sean relativamente coherentes y la experiencia resulte satisfactoria. Sin embargo, como los modelos de lenguaje grandes son probabilísticos, es posible que las sugerencias difieran de las indicadas en el taller. Es normal y esperable.

> [!NOTE]
> Debido a la naturaleza probabilística de los modelos de lenguaje grandes, Copilot puede utilizar un comando MCP diferente, pero debería completar la tarea.

3. Pide a Copilot que muestre las incidencias de la lista con este mensaje:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecciona **Continue** para ejecutar el comando que enumera todas las incidencias.
5. Revisa la lista generada.

Fíjate en que Copilot también ha priorizado las tareas según las que considera más útiles para los usuarios.

## Revisa los archivos de instrucciones

Antes de pedir al agente que genere código, revisa el archivo de instrucciones que proporcionará contexto a Copilot. Usarás el archivo de interfaz de [usuario (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que explica cómo añadir funciones al sitio.

1. En el codespace, abre `.github/instructions/ui.instructions.md`.
2. Lee las pautas generales para añadir funciones. Incluyen:
   - una descripción de la arquitectura;
   - principios de diseño de componentes, capacidad de prueba y accesibilidad;
   - enlaces a archivos de instrucciones específicos para Astro, Tailwind CSS y Drizzle.

> [!TIP]
> Los archivos de instrucciones pueden incluir referencias a otros archivos de instrucciones y a archivos del proyecto. Las rutas son relativas a la ubicación del archivo de instrucciones. Así puedes reutilizar indicaciones, dividir instrucciones extensas en archivos más manejables y señalar ejemplos y plantillas.

## Implementa el filtrado

Para completar el filtrado hay que hacer al menos tres cambios independientes en la aplicación:

- añadir o ajustar la lógica restante de filtrado en la capa de datos (`src/lib/`);
- añadir o actualizar pruebas del comportamiento de filtrado;
- actualizar la página con la lista de juegos para incluir la interfaz de filtrado.

Además, las pruebas deben ejecutarse correctamente antes de incorporar los cambios. El modo de agente de Copilot puede encargarse de estas tareas. Añadamos la función.

1. Continúa la conversación actual con Copilot o selecciona **New Chat** para iniciar otra.
2. Selecciona **Add Context**, **Instructions** y **ui** como archivo de instrucciones.

   ![Captura de pantalla que muestra cómo seleccionar el archivo de instrucciones de interfaz](../../../_images/ex3-select-instructions-file.png)

3. Confirma que **Agent** sigue seleccionado en el menú desplegable de agentes de la vista Chat.

   ![Captura de pantalla del selector de agentes en la vista Chat.](../../../_images/shared-chat-mode-selector.png)

4. Confirma que **Claude Sonnet 4.5** sigue seleccionado como modelo.
5. Pide a Copilot que implemente la función según la incidencia relacionada de la lista:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Observa cómo Copilot explora el proyecto y localiza los archivos relacionados con la función: los helpers de la capa de datos, la interfaz y las pruebas. Después, modifica los archivos y ejecuta las pruebas.

   ![Captura de pantalla de Copilot mientras explora los archivos del proyecto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot realizará varias tareas, como explorar el proyecto, modificar archivos y ejecutar pruebas. El proceso puede tardar unos minutos según la complejidad de la tarea y del código. Mientras trabaja, puede que aparezcan los botones **Keep** y **Undo** en el editor. Al terminar, podrás aceptar o deshacer todos los cambios; no tienes que seleccionar estas opciones mientras siga trabajando.

7. Cuando Copilot te lo pida, selecciona **Continue** para ejecutar las pruebas.

   ![Captura de pantalla de un cuadro de diálogo en Copilot Chat que pide permiso para ejecutar pruebas](../../../_images/ex3-agent-mode-run-tests.png)

8. Puede que Copilot se detenga varias veces y que algunas pruebas fallen durante el proceso. No pasa nada: alternará entre generar código y ejecutar pruebas hasta completar la tarea y no detectar errores.

   ![Captura de pantalla de una sesión completa con el modo de agente de Copilot](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Examina el código generado para detectar posibles problemas.

> [!CAUTION]
> Recuerda que siempre es importante revisar el código generado por Copilot o cualquier otra herramienta de IA.

10. Vuelve al navegador con el sitio en ejecución y explora las nuevas funciones.
11. Cuando hayas confirmado que todo funciona y revisado el código, selecciona **Keep** en la ventana Copilot Chat.

## Resumen y siguientes pasos

¡Enhorabuena! En este ejercicio has aprendido a usar el modo de agente de GitHub Copilot para añadir funciones al sitio de Tailspin Toys. Has aprendido a:

- implementar funciones en la interfaz y en la capa de datos con el modo de agente de GitHub Copilot;
- hacer que el modo de agente explore el proyecto, identifique archivos relacionados y coordine los cambios;
- revisar los cambios y las pruebas generados por el modo de agente antes de incorporarlos al código.

Ahora [probarás la función con el servidor MCP de Playwright][next-lesson] y abrirás una solicitud de incorporación de cambios.

### Ejercicio de exploración adicional: implementar paginación

A medida que aumente la lista de juegos, será necesario habilitar la paginación. Usa lo que has aprendido para pedir a Copilot que la implemente. Por ejemplo:

- sigue las prácticas recomendadas y utiliza los archivos de instrucciones existentes;
- decide cómo quieres implementar la paginación y si quieres permitir que el usuario elija el tamaño de página o prefieres fijarlo;
- incluye en el mensaje las indicaciones que Copilot necesita para crear la implementación deseada;
- puede que tengas que iterar con GitHub Copilot, pedir cambios y proporcionar contexto; es lo habitual al trabajar con Copilot.

## Recursos

- [Modos ask, edit y agent de Copilot: qué hacen y cuándo usarlos][choose-mode]
- [Modo de agente en VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
