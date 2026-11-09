---
title: "Ejercicio 0: Requisitos previos"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Antes de empezar los ejercicios de VS Code, prepara una copia del repositorio Tailspin Toys, crea un [Codespace][codespaces] y confirma que GitHub Copilot Chat funciona en el editor.

## Configurar el repositorio del taller

Crea tu copia a partir de la plantilla del repositorio, que contiene los archivos necesarios para el taller.

1. En una nueva ventana del navegador, abre la [plantilla de Tailspin Toys][tailspin-template].
2. Selecciona **Use this template** y, después, **Create a new repository**.

   ![Botón Use this template](../../../_images/ex0-use-template.png)

3. Si participas en un evento organizado por GitHub o Microsoft, sigue las indicaciones de los mentores. En caso contrario, crea el repositorio en una organización donde tengas acceso a GitHub Copilot.

   ![Configuración del repositorio a partir de una plantilla](../../../_images/ex0-repository-settings.png)

4. Anota la ruta del repositorio (**organization-or-user-name/repository-name**), que necesitarás más adelante.

> [!NOTE]
> **El backlog está preparado**
>
> Al crear el repositorio a partir de la plantilla, se crea automáticamente un backlog de incidencias de GitHub que utilizarás durante el taller.

## Crear un Codespace

Utilizarás un Codespace para completar los ejercicios.

1. Abre el nuevo repositorio.
2. Selecciona el botón verde **Code**.

   ![Botón Code](../../../_images/ex0-code-button.png)

3. Selecciona la pestaña **Codespaces** y, después, el botón **+** para crear un Codespace.

   ![Crear un Codespace](../../../_images/ex0-create-codespace.png)

La creación puede tardar varios minutos. Deja abierto el Codespace en una pestaña del navegador para volver a él en un ejercicio posterior.

> [!NOTE]
> El taller está preparado para ejecutarse en Codespaces o en un [contenedor de desarrollo local][dev-containers]. Ambos incluyen los requisitos previos necesarios. Si prefieres trabajar en local, abre el repositorio clonado en VS Code y selecciona **Reopen in Container** cuando aparezca la opción. VS Code creará el mismo contenedor de desarrollo que usa Codespaces.

## Usar GitHub Copilot Chat y el modo agente

Para usar el modo agente de GitHub Copilot Chat, necesitas tener instalada la extensión GitHub Copilot Chat en el entorno de desarrollo integrado (IDE). Si trabajas en Codespaces, ya debería estar instalada.

> [!TIP]
> Si no tienes instalada la extensión GitHub Copilot Chat, [instálala desde Visual Studio Code Marketplace][copilot-chat-extension]. También puedes abrir la vista Extensions de Visual Studio Code, buscar **GitHub Copilot Chat** y seleccionar **Install**.

Después de instalar la extensión, puede que tengas que autenticarte con tu cuenta de GitHub para habilitarla.

1. Vuelve a tu Codespace.
2. Si Copilot Chat no aparece a la derecha del editor, selecciona el icono **Copilot Chat** en la parte superior del Codespace.
3. Escribe un mensaje como `Hello world` en la ventana de Copilot Chat y pulsa <kbd>Enter</kbd>. Esto debería activar Copilot Chat.
4. Si aún no te has autenticado, se te pedirá que inicies sesión con tu cuenta de GitHub. Sigue las instrucciones para autenticarte.

   ![Ejemplo de la solicitud de autenticación de Copilot Chat](../../../_images/ex1-copilot-authentication.png)

5. Después de autenticarte, debería aparecer la ventana de Copilot Chat.

## Resumen

Has creado una copia del repositorio y has comenzado a crear el Codespace que utilizarás al escribir código.

## Paso siguiente

Continúa con el [ejercicio 1: Instrucciones personalizadas][next-lesson] para enseñar a Copilot las convenciones del proyecto.

## Recursos

- [Información general sobre GitHub Codespaces][codespaces]
- [Crear un repositorio a partir de una plantilla][template-repository]
- [Inicio rápido de Codespaces][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces]: https://github.com/features/codespaces
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers
[copilot-chat-extension]: https://marketplace.visualstudio.com/items?itemName=GitHub.copilot-chat
[next-lesson]: ../1-custom-instructions/
