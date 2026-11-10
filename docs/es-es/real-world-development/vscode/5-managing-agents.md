---
title: "Ejercicio 5: Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Agentes personalizados][previous-lesson] |
|:--|

Cuando utilizas el modo de agente de GitHub Copilot, este trabaja de forma autónoma: explora el código, propone cambios, edita archivos y ejecuta comandos. Como trabaja en tu equipo en tiempo real, Copilot Chat en VS Code te muestra cada llamada a una herramienta y cada edición a medida que se producen. Puedes revisar las diferencias en línea, aceptar o rechazar cambios concretos y orientar la conversación con mensajes de seguimiento para ajustar o ampliar el trabajo sin salir del editor.

En este ejercicio:

- supervisarás en tiempo real el trabajo del agente de accesibilidad.
- revisarás y aceptarás las ediciones de archivos propuestas.
- orientarás al agente durante la sesión para que añada un modo claro junto al modo de alto contraste.

## Escenario

El agente personalizado de accesibilidad que usaste en el [Ejercicio 4][previous-lesson] está implementando el modo de alto contraste. Mientras trabaja, el equipo de diseño solicita además un modo claro para las personas a quienes les resulta demasiado oscuro el tema predeterminado. En lugar de empezar una conversación nueva, orientarás la sesión existente para ampliar el trabajo.

## Supervisa el progreso de Copilot

La vista Copilot Chat muestra cada paso del agente: los archivos que explora, los comandos que ejecuta y las ediciones que propone. Cada edición aparece en el chat con sus diferencias y los controles **Keep** y **Undo**.

1. Vuelve a la vista Copilot Chat del Ejercicio 4.
2. Mientras trabaja el agente, desplázate por el chat para ver cada llamada a una herramienta. Verás lecturas y ediciones de archivos, además de los comandos de terminal que solicite ejecutar.
3. Cuando el agente proponga una edición, las diferencias aparecerán en línea:
   - Selecciona **Keep** para aceptar el cambio.
   - Selecciona **Undo** para revertir el cambio y permitir que el agente vuelva a intentarlo.
4. También puedes abrir el indicador **Files changed** situado en la parte superior del chat para ver todos los archivos modificados durante esta sesión.

> [!NOTE]
> Si una llamada a una herramienta solicita permiso (por ejemplo, para ejecutar un comando de terminal), el agente se detendrá y mostrará una solicitud **Allow**. Aprueba solo los comandos que consideres seguros.

5. Espera a que el agente termine de implementar el modo de alto contraste. Deberías ver un mensaje final que resuma los cambios.

## Pide al agente que añada el modo claro

Ahora que ya está disponible el modo de alto contraste, ampliarás la misma conversación para añadir un modo claro. Como continuarás en la misma sesión de chat, el agente conservará el contexto del trabajo anterior, incluidos los componentes que modificó, el enfoque de estilos y el uso de almacenamiento local.

1. Envía este mensaje de seguimiento en el cuadro de Copilot Chat:

    ```plaintext
    Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
    ```

2. El agente retomará el trabajo, revisará los cambios que ya hizo y propondrá cómo añadir el modo claro.
3. Revisa cada nueva edición a medida que aparezca, igual que hiciste con el modo de alto contraste. Usa **Keep** o **Undo** en cada diferencia.
4. Si el agente sigue un camino que no te convence —por ejemplo, si intenta reorganizar los controles de una forma que no te gusta—, detenlo con el botón **Stop** junto al cuadro de chat y envíale un mensaje de seguimiento para aclarar lo que necesitas.

> [!TIP]
> Orientar una sesión en curso es más rápido que empezar de nuevo. Usa mensajes de seguimiento para pedir ajustes («simplifica el CSS; utiliza variables CSS en lugar de duplicar selectores»), corregir el rumbo («no añadas un tercer control; integra el modo claro en el panel de configuración existente») o ampliar el alcance («añade también métodos abreviados de teclado para ambos controles»).

## Revisa los cambios

Antes de confirmar el trabajo, revisa rápidamente todo lo que ha modificado el agente.

1. Abre la vista **Source Control** en VS Code.
2. Revisa la lista completa de archivos modificados. Deberías ver cambios en los componentes Astro, los estilos y las pruebas relacionadas.
3. Abre algunos archivos modificados y examina las diferencias. Confirma que se aplican las pautas de accesibilidad del agente personalizado: atributos ARIA, navegación con teclado, HTML semántico y persistencia mediante almacenamiento local.
4. Cuando estés conforme, prepara y confirma los cambios desde el panel **Source Control**. Los publicarás en la [siguiente lección][next-lesson].

## Resumen y siguientes pasos

Has usado el modo de agente de Copilot Chat para supervisar y orientar una sesión en curso: revisaste cada edición, aceptaste o rechazaste cambios y enviaste mensajes de seguimiento para ampliar el trabajo sin perder el contexto.

En esta lección has aprendido a:

- supervisar el trabajo de un agente en tiempo real.
- aceptar y rechazar ediciones individuales.
- orientar una sesión en curso mediante mensajes de seguimiento.

En la [siguiente lección][next-lesson] publicarás el trabajo de accesibilidad en una solicitud de incorporación de cambios y revisarás los cambios que ha generado el agente personalizado local durante el taller.

## Recursos

- [Copilot Chat en VS Code][copilot-chat-vscode]
- [Usar el modo de agente][agent-mode]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
