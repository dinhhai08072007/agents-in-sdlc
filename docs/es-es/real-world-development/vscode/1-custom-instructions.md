---
title: "Ejercicio 1: Instrucciones personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Requisitos previos][previous-lesson] · [Siguiente lección: Modo de agente →][next-lesson]

El contexto es fundamental cuando se trabaja con IA generativa. Si una tarea debe realizarse de una forma concreta o Copilot necesita información previa, es importante que ese contexto esté disponible. Hay varias formas de compartirlo con Copilot. Una de las principales son los [archivos de instrucciones][instruction-files], que permiten orientar la generación de código.

En este ejercicio:

- explorarás cómo llegan a Copilot el contexto específico del proyecto, las pautas de programación y los estándares de documentación mediante instrucciones personalizadas del repositorio y archivos de instrucciones aplicables a rutas concretas;
- generarás la primera parte de la capa de datos para el filtrado (un helper de editoriales) con las instrucciones actuales;
- añadirás un estándar general al archivo `.github/copilot-instructions.md`;
- volverás a enviar el mismo mensaje y observarás cómo el código generado incorpora el nuevo estándar;
- confirmarás los cambios de instrucciones y filtrado en `main` para que Copilot use las nuevas indicaciones en el siguiente ejercicio.

> [!CAUTION]
> El código generado puede no cumplir algunos de los estándares que hayas definido. Copilot no es determinista. El objetivo del ejercicio es observar la tendencia del cambio después de actualizar las instrucciones, no obtener exactamente el mismo resultado carácter por carácter.

## Archivos de instrucciones

### Escenario

Como cualquier empresa de desarrollo, Tailspin Toys tiene pautas y requisitos para sus prácticas de programación. Entre ellos:

- La capa de datos siempre necesita pruebas unitarias.
- La interfaz debe usar el tema oscuro y tener un aspecto moderno.
- El código debe incluir documentación en forma de comentarios TSDoc.
- Al principio de cada archivo debe haber un bloque de comentarios que explique su propósito.

Con los archivos de instrucciones, proporcionarás a Copilot la información necesaria para que realice las tareas de acuerdo con estas prácticas.

### Instrucciones personalizadas

Las instrucciones personalizadas proporcionan contexto y preferencias a Copilot para que entienda mejor tu estilo y tus requisitos de programación. Esta función permite orientar las sugerencias y los fragmentos de código. Puedes especificar las convenciones de programación, las bibliotecas y los comentarios que prefieres. Puedes crear instrucciones para todo el repositorio o para tipos concretos de archivos y tareas.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md`: un único archivo que se envía a Copilot en **todas** las solicitudes del repositorio. Debe contener información general del proyecto que sea pertinente para la mayoría de las solicitudes de chat o CLI, como la pila tecnológica, una descripción de lo que se está creando, prácticas recomendadas y directrices globales.
- `.github/instructions/*.instructions.md`: archivos que puedes crear para tareas o tipos de archivo concretos. Por ejemplo, puedes definir pautas para TypeScript o Astro, o para tareas como crear un componente de interfaz o una nueva prueba unitaria.

> [!NOTE]
> En el IDE, los archivos de instrucciones solo se usan para generar código en Copilot Chat, no para las finalizaciones de código ni las sugerencias de siguiente edición.
>
> Copilot Chat, Copilot CLI y el agente en la nube de Copilot usan tanto las instrucciones del repositorio como los archivos `*.instructions.md` con frontmatter `applyTo` cuando generan código.
>
> Copilot también [admite archivos de instrucciones en otros formatos][custom-instructions-support], incluidos los archivos AGENTS.md y CLAUDE.md.

### Prácticas recomendadas para gestionar archivos de instrucciones

Este taller no abarca en detalle la creación de archivos de instrucciones, pero los ejemplos del proyecto muestran un enfoque representativo. En general:

- Mantén `.github/copilot-instructions.md` centrado en las directrices del proyecto, como su descripción, estructura y estándares globales de programación.
- Usa `*.instructions.md` para instrucciones específicas de tipos de archivo (pruebas unitarias, componentes Astro o la capa de datos) o de tareas concretas.
- Escribe en lenguaje natural. Explica las pautas con claridad e incluye ejemplos de cómo debería ser el código y de cómo no debería ser.

No hay una única forma de crear archivos de instrucciones, igual que no hay una única forma de usar IA. La experimentación te permitirá encontrar lo que mejor se adapte al proyecto.

> [!TIP]
> Todos los proyectos que usan GitHub Copilot deberían tener un conjunto sólido de archivos de instrucciones. Al explorar los archivos de este proyecto, verás que hay instrucciones para distintos tipos de tareas, incluidas las [actualizaciones de interfaz][ui-instructions] y [Astro][astro-instructions].
>
> Copilot también puede ayudarte a generar archivos de instrucciones. Cada entorno ofrece esta opción de una forma distinta; por ejemplo, **Configure Chat → Generate Agent Instructions** en VS Code o `/init` en Copilot CLI. La lección correspondiente indicará cuándo utilizarla.
>
> ¿Buscas plantillas o un punto de partida? Explora [awesome-copilot][awesome-copilot], un repositorio con archivos de instrucciones, agentes personalizados y otros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explora los archivos de instrucciones personalizadas del proyecto

Dedica un momento a leer los archivos de instrucciones incluidos en este repositorio: hay un archivo principal `copilot-instructions.md` y varios archivos `*.instructions.md` para distintas tareas. Ábrelos en el editor o en la interfaz web de GitHub.

1. Abre `.github/copilot-instructions.md`.
2. Examina el archivo y fíjate en la breve descripción del proyecto y en secciones como **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. Dentro de **Code standards**, observa las directrices anidadas para **GitHub Actions Workflows**. Se aplican a cualquier interacción con Copilot.
3. Abre la carpeta `.github/instructions` y explórala. Verás instrucciones para archivos Astro, la capa de datos Drizzle, las pruebas y otros elementos.
4. Abre `.github/instructions/unit-tests.instructions.md`. Observa el campo `applyTo` del principio, que establece un patrón glob (relativo a la raíz del repositorio) para determinar a qué archivos se aplican las instrucciones. En este caso, coincide con cualquier archivo de pruebas TypeScript, por ejemplo, uno que cumpla `**/*.test.ts`.
5. Fíjate en las instrucciones específicas para crear pruebas unitarias en este proyecto.
6. Por último, abre `.github/instructions/drizzle.instructions.md` y ve al final. Observa los enlaces a otros archivos de instrucciones, como `unit-tests.instructions.md`, y a archivos existentes del proyecto. Las rutas son relativas al archivo de instrucciones, no a la raíz del repositorio. Así puedes dividir conjuntos de instrucciones grandes en archivos más pequeños y reutilizables, y señalar ejemplos que Copilot puede seguir.

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` describe los estándares de programación del proyecto, pero todavía no exige documentación dentro del código. En los siguientes pasos añadirás reglas para los comentarios TSDoc y los encabezados de comentarios de los archivos.

## Crea una rama para los cambios

Sigamos algunas prácticas recomendadas y creemos una rama para los cambios.

1. Vuelve al codespace del ejercicio anterior.
2. Abre un terminal nuevo pulsando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crea una rama y cámbiate a ella:

   ```bash
   git checkout -b custom-instructions
   ```

## Usa Copilot Chat *antes* de actualizar las instrucciones

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar los ejercicios siguientes, vuelve al codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo varían según el ejercicio; cada paso lo indicará cuando sea necesario.

Para observar el efecto de las instrucciones personalizadas, empieza enviando un mensaje con el archivo de instrucciones actual. Más adelante, lo actualizarás y volverás a enviar el mismo mensaje para comparar el resultado.

1. Cierra las pestañas abiertas de ejercicios anteriores para que Copilot use solo el contexto que necesitas.
2. Abre `src/lib/publishers.ts` para indicar a Copilot dónde debe ubicarse el helper.
3. En el menú desplegable de agentes de la vista Chat, selecciona **Agent** para que Copilot pueda aplicar cambios a los archivos.

   ![Captura de pantalla del selector de agentes en la vista Chat.](../../../_images/shared-chat-mode-selector.png)

4. Envía este mensaje:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot explorará el proyecto y aplicará los cambios, que a menudo abarcarán el archivo helper y sus pruebas.
6. Observa que el helper propuesto es una función tipada que recibe un cliente `db` como primer argumento y devuelve una matriz tipada de editoriales. Esto se debe a las convenciones de la capa de datos definidas en `.github/instructions/drizzle.instructions.md`, que se aplican a `src/lib/*.ts`.
7. Observa que al código propuesto **le faltan** los comentarios TSDoc y el encabezado de comentario del archivo.

> [!CAUTION]
> Como Copilot es probabilístico, puede que añada comentarios de documentación aunque no se lo pidas. No pasa nada: el objetivo sigue siendo observar la mejora de coherencia después de actualizar las instrucciones.

## Añade un estándar al repositorio

Como hemos visto, `.github/copilot-instructions.md` proporciona información general del proyecto a Copilot. Añadamos un estándar de programación para mejorar las sugerencias.

1. Vuelve a abrir `.github/copilot-instructions.md`.
2. Busca la sección **Code formatting requirements**, que debería estar cerca de la línea 27. Describe los estándares de programación del proyecto, pero todavía no exige documentación en el código; por eso el helper generado no tenía comentarios de documentación.
3. Añade las líneas Markdown siguientes justo después de los estándares existentes. Así indicarás a Copilot que añada encabezados de comentarios a los archivos y comentarios TSDoc:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Guarda `copilot-instructions.md`.

> [!TIP]
> Como viste en la lección anterior, puedes crear archivos de instrucciones en el repositorio (`.github/copilot-instructions.md`) para definir directrices globales o como archivos `*.instructions.md` para determinados lenguajes, tipos de archivo o tareas. Las reglas generales del proyecto, como la de los comentarios de documentación, corresponden al archivo del repositorio.

## Vuelve a enviar el mensaje y observa el cambio

1. Vuelve a Copilot Chat y selecciona **New Chat** para borrar el contexto anterior.
2. Selecciona `src/lib/publishers.ts` para que Copilot se centre en el archivo correcto.
3. Envía **el mismo mensaje** que antes:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. Observa que el archivo propuesto ahora empieza con un bloque de comentarios parecido a este:

   ```typescript
   /**
    * Helpers de acceso a datos de editoriales para la plataforma de financiación colectiva de Tailspin Toys.
    * Proporciona funciones para recuperar información de editoriales de la base de datos.
    */
   ```

5. Observa que la función propuesta ahora incluye un comentario TSDoc parecido a este:

   ```typescript
   /**
    * Devuelve una lista de todas las editoriales con su id y nombre.
    *
    * @param db - Cliente de base de datos de Drizzle.
    * @returns Promesa que se resuelve en una matriz de objetos de editorial.
    */
   ```

Has orientado a Copilot para que siga un estándar nuevo y lo aplique a código real que utilizarás en el siguiente ejercicio.

## Confirma las instrucciones y publica la rama

Los archivos de instrucciones son recursos del repositorio y se gestionan mediante el mismo control de código fuente que los demás archivos. Confirmemos los cambios y publiquemos la rama.

1. Abre un terminal nuevo pulsando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
2. En el terminal, confirma que están presentes las instrucciones actualizadas y los cambios del helper ejecutando:

   ```bash
   git status
   ```

3. Prepara y confirma los cambios en las instrucciones y todos los archivos que Copilot modificó para crear el helper:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # Si git status muestra otros cambios relacionados (por ejemplo, pruebas), añade también esos archivos.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. Publica la rama en el repositorio:

   ```bash
   git push -u origin custom-instructions
   ```

## Crea y fusiona una solicitud de incorporación de cambios

Con la rama publicada, crea una solicitud de incorporación de cambios y asóciala a la incidencia de tu lista de tareas sobre la actualización de las instrucciones. Podrías hacerlo manualmente, pero pidamos a Copilot que encuentre la incidencia, cree una solicitud que la cierre y la fusione.

> [!NOTE]
> En este ejercicio Copilot actuará por primera vez en GitHub. La plantilla del proyecto ya configura las herramientas de GitHub, así que no hay que preparar nada. La primera vez que Copilot use una herramienta, VS Code podría pedirte que inicies sesión en GitHub. Sigue las instrucciones para autorizarlo. En un ejercicio posterior aprenderás cómo funciona esta conexión mediante Model Context Protocol.

1. Abre Copilot Chat en el codespace.
2. Pulsa <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) para abrir la vista Copilot Chat y confirma que **Agent** está seleccionado en el selector de agentes.
3. Pide a Copilot que busque la incidencia relacionada con la actualización de los archivos de instrucciones y cree una solicitud de incorporación de cambios desde la rama actual. Indícale que describa tanto las actualizaciones de instrucciones como el helper de editoriales:

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot empezará a buscar la incidencia y a crear la solicitud.
5. Cuando Copilot te pida **Allow** para realizar acciones de GitHub en tu nombre, revisa el comando y selecciona **Allow** si corresponde.
6. Cuando se haya creado la solicitud, pide a Copilot que la fusione con `main` y que vuelva a esa rama:

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Cuando Copilot te pida **Allow** para realizar acciones de GitHub y ejecutar comandos de shell en tu nombre, revisa el comando y selecciona **Allow** si corresponde.

¡Has creado y fusionado una solicitud de incorporación de cambios con ayuda de GitHub Copilot!

## Resumen y siguientes pasos

Has explorado cómo Copilot utiliza los archivos de instrucciones. Después, con Copilot Chat en VS Code:

- enviaste un mensaje para generar código y observaste el resultado con las instrucciones existentes;
- añadiste un estándar general a `.github/copilot-instructions.md`;
- volviste a enviar el mismo mensaje y observaste cómo el código propuesto incorporaba el nuevo estándar;
- confirmaste las instrucciones y el helper en `main` para que el siguiente ejercicio pudiera partir de ellos.

A continuación, aplicarás esas instrucciones en el [modo de agente][next-lesson] para que Copilot añada una función en varias partes del código.

## Recursos

- [Modos ask, edit y agent de Copilot: qué hacen y cuándo usarlos][choose-mode]
- [Modo de agente en VS Code][vs-code-agent-mode]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Cinco consejos para escribir mejores instrucciones personalizadas para Copilot][copilot-instructions-five-tips]
- [Instrucciones personales personalizadas para GitHub Copilot][personal-instructions]
- [Awesome Copilot: archivos de instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
[awesome-copilot]: https://github.com/github/awesome-copilot
