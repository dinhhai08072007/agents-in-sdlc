---
title: "Ejercicio 0: Requisitos previos"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Antes de empezar los ejercicios del agente en la nube de Copilot, prepara todo lo necesario. Crearás una copia propia del repositorio Tailspin Toys e iniciarás un [codespace][codespaces] que usarás para editar archivos de instrucciones y revisar el trabajo del agente en la nube.

## Configura el repositorio del taller

Para crear una copia del repositorio con el código que desarrollarás, crearás una instancia a partir de la [plantilla][template-repository]. La nueva instancia incluirá todos los archivos necesarios para el taller y la usarás durante los ejercicios.

1. En una nueva ventana del navegador, ve a la [plantilla de Tailspin Toys][tailspin-template].
2. Crea una copia del repositorio seleccionando el botón **Use this template** en la página del repositorio del taller. Después, selecciona **Create a new repository**.

    ![Botón Use this template](../../../_images/ex0-use-template.png)

3. Si participas en un evento organizado por GitHub o Microsoft, sigue las instrucciones de los mentores. De lo contrario, puedes crear el repositorio en una organización donde tengas acceso a GitHub Copilot.

    ![Configuración del repositorio a partir de una plantilla](../../../_images/ex0-repository-settings.png)

4. Anota la ruta del repositorio que has creado (**organization-or-user-name/repository-name**), porque la necesitarás más adelante en el taller.

## Crea un codespace

A continuación, usarás un codespace para completar los ejercicios.

[GitHub Codespaces][codespaces] es un entorno de desarrollo en la nube que te permite escribir, ejecutar y depurar código directamente en el navegador. Proporciona un IDE completo con compatibilidad con varios lenguajes de programación, extensiones y herramientas.

1. Ve al repositorio que acabas de crear.
2. Selecciona el botón verde **Code**.

    ![Selecciona el botón Code](../../../_images/ex0-code-button.png)

3. Selecciona la pestaña **Codespaces** y, después, el botón **+** para crear un codespace.

    ![Crea un codespace](../../../_images/ex0-create-codespace.png)

El codespace tardará varios minutos en crearse, aunque será mucho más rápido que instalar todos los servicios manualmente. Mientras tanto, puedes explorar otras funciones de GitHub Copilot, que veremos a continuación.

> [!CAUTION]
> Volverás al codespace en un ejercicio posterior. Por ahora, déjalo abierto en una pestaña del navegador.

> [!NOTE]
> Este taller está diseñado para ejecutarse en un codespace o en un [contenedor de desarrollo][dev-containers] local. Ambos incluyen los requisitos previos necesarios para facilitar el trabajo. Si prefieres trabajar localmente, abre el repositorio clonado en VS Code y selecciona **Reopen in Container** cuando aparezca el aviso. VS Code creará el mismo contenedor de desarrollo que se usa en el codespace.

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## Resumen

¡Enhorabuena! Has creado una copia del repositorio del taller y has iniciado la creación del codespace que usarás mientras trabajas con el agente en la nube de Copilot.

## Siguiente paso

Añadamos instrucciones personalizadas que seguirá el agente en la nube. Continúa con el [Ejercicio 1: Instrucciones personalizadas][next-lesson].

## Recursos

- [Introducción a GitHub Codespaces][codespaces]
- [Crear un repositorio a partir de una plantilla][template-repository]
- [Introducción a Codespaces][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[next-lesson]: ../1-custom-instructions/
