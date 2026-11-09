---
title: "Ejercicio 3 - Probar la funcionalidad con el servidor MCP de Playwright"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Modo agente][previous-lesson] · [Siguiente lección: Agentes personalizados →][next-lesson] |

Has creado la funcionalidad de filtrado con el modo agente. Antes de abrir una solicitud de incorporación de cambios (PR), comprobarás que funciona en el navegador. Configurarás el servidor MCP de Playwright para que Copilot pueda controlar un navegador y probar la funcionalidad; después, publicarás la rama y abrirás la PR.

En este ejercicio:

- aprenderás qué es Model Context Protocol (MCP) y cómo sus servidores amplían Copilot con herramientas.
- añadirás el servidor MCP de Playwright al espacio de trabajo.
- pedirás a Copilot que pruebe manualmente el filtrado en un navegador.
- publicarás la rama y abrirás una solicitud de incorporación de cambios.

## ¿Qué es Model Context Protocol (MCP)?

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) permite que los agentes de IA se comuniquen con herramientas y servicios externos. Los servidores MCP actúan como puente entre el agente y esas herramientas.

![Diagrama que muestra cómo el modo agente interactúa con el contexto, el modelo de lenguaje y las herramientas, incluidas las que proporcionan los servidores MCP y las extensiones de VS Code](../../../_images/ex1-mcp-diagram.png)

MCP permite a los agentes de IA comunicarse con herramientas y servicios externos en tiempo real. Así pueden acceder a información actualizada mediante recursos y realizar acciones en nombre del usuario mediante herramientas. Un servidor MCP administra la comunicación entre el agente y las herramientas externas, como las API existentes o las herramientas locales, incluidas las de paquetes de NPM. Cada servidor ofrece un conjunto distinto de herramientas y recursos.

- **[Servidor MCP de GitHub](https://github.com/github/github-mcp-server)**: proporciona acceso a interfaces de programación de aplicaciones (API) para gestionar repositorios, incidencias y solicitudes de incorporación de cambios.
- **[Servidor MCP de Playwright](https://github.com/microsoft/playwright-mcp)**: ofrece automatización del navegador con Playwright.

GitHub mantiene un [registro MCP](https://github.com/mcp) para facilitar la búsqueda y las contribuciones.

> [!CAUTION]
> Trata los servidores MCP como cualquier otra dependencia. Revisa su código fuente, comprueba quién los publica y considera las implicaciones de seguridad antes de usarlos.

## Revisar la configuración de MCP

El archivo `.vscode/mcp.json` configura los servidores MCP disponibles en el espacio de trabajo de VS Code. El proyecto ya incluye un servidor `github` remoto que permite a Copilot consultar el backlog.

1. Abre `.vscode/mcp.json` en tu Codespace.
2. Comprueba que ya haya un servidor `github` configurado:

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       }
     }
   }
   ```

La entrada `github` viene con la plantilla del proyecto y por eso Copilot pudo leer el backlog en el ejercicio anterior. Utiliza el [servidor MCP remoto de GitHub][remote-github-mcp-server], así que no hace falta instalarlo en local. VS Code se conecta por HTTP y te autenticas con GitHub la primera vez que Copilot utiliza una de sus herramientas.

## Añadir el servidor MCP de Playwright

Añade un segundo servidor para que Copilot pueda controlar un navegador.

1. En `.vscode/mcp.json`, añade la entrada `playwright` junto a `github`:

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       },
       "playwright": {
         "command": "npx",
         "args": ["@playwright/mcp@latest", "--headless"]
       }
     }
   }
   ```

2. Guarda el archivo.

## Iniciar y confiar en el servidor

VS Code inicia los servidores MCP cuando se necesitan: la primera vez que Copilot requiere las herramientas de un servidor, VS Code lo inicia y te pide que confirmes que confías en él. Puedes iniciarlo ahora para comprobar que la configuración es correcta.

1. En `.vscode/mcp.json`, selecciona la acción **Start** que aparece encima de la entrada `playwright`.
2. Si VS Code te pide que confirmes que confías en el servidor, revisa la configuración y selecciona la opción para confiar en él.

El servidor remoto `github` se conecta mediante HTTP, mientras que VS Code inicia el servidor local `playwright` ejecutando `npx @playwright/mcp@latest`. La opción `--headless` permite ejecutar el navegador sin una ventana visible, necesaria en Codespaces porque no hay un escritorio que mostrar.

Iniciar el servidor manualmente es opcional. Si omites este paso, VS Code lo iniciará y te pedirá que confíes en él la primera vez que Copilot necesite sus herramientas.

> [!NOTE]
> Tailspin Toys ya utiliza Playwright para las pruebas de extremo a extremo. Si Copilot indica que falta un navegador, pídele que ejecute `npx playwright install chromium` y vuelve a intentarlo.

## Probar la funcionalidad de filtrado

Con el servidor conectado y la aplicación en ejecución, pide a Copilot que pruebe los filtros mediante el navegador.

1. Envía este prompt:

   ```text
   Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

   1. Go to the games page and note how many games are listed.
   2. Apply a category filter and confirm the list updates to only show games in that category.
   3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
   4. Combine a category and a publisher filter and confirm the results respect both.

   Report what you observe at each step, and call out anything that does not behave as expected.
   ```

2. Compara el informe con los criterios de aceptación de la incidencia. Si algo no funciona, pide aclaraciones o solicita que Copilot corrija el código antes de abrir una PR.

El servidor MCP de Playwright utiliza el árbol de accesibilidad de la página en lugar de capturas de pantalla. El agente trabaja con elementos estructurados y etiquetados, como botones, enlaces y elementos de lista, igual que las tecnologías de asistencia. Por eso, una comprobación funcional también sirve como verificación básica de accesibilidad.

> [!NOTE]
> La aplicación debe estar disponible en `http://localhost:4321` para realizar la prueba. Si detuviste el servidor de desarrollo, vuelve a iniciarlo antes de enviar el prompt. La primera vez que Copilot use el servidor MCP de Playwright, puede que tenga que descargar un navegador. Si indica que falta uno, pídele que ejecute `npx playwright install chromium` y vuelve a intentarlo.

El proyecto Tailspin Toys ya utiliza Playwright para las pruebas de extremo a extremo, así que el navegador suele estar instalado.

## Publicar la rama y crear una solicitud de incorporación de cambios

Después de comprobar la funcionalidad, publica la rama `filtering-vscode` y abre una solicitud de incorporación de cambios (PR).

1. Abre el panel **Source Control** y revisa los cambios de Copilot.
2. Prepara los cambios con el icono **+**.
3. Genera un mensaje de confirmación con el botón **Sparkle**.
4. Selecciona **Publish** para subir la rama.

## Crear la solicitud

Pide a Copilot que encuentre la incidencia adecuada y cree una PR vinculada.

1. En Copilot Chat, selecciona **New Chat** y confirma que está seleccionado **Agent**.
2. Envía este prompt:

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

3. Selecciona **Continue** cuando sea necesario para permitir que Copilot recopile información y realice acciones. Si se te solicita, autentícate con GitHub.
4. Abre el enlace de la PR y revisa los cambios, pero no la combines todavía.

## Resumen y pasos siguientes

En este ejercicio:

- has aprendido qué es MCP y cómo sus servidores amplían Copilot con herramientas.
- has añadido el servidor MCP de Playwright al espacio de trabajo.
- lo has utilizado para probar el filtrado en un navegador.
- has publicado la rama y abierto una PR para el trabajo de filtrado.

A continuación, [crearás un agente personalizado][next-lesson] para agilizar tareas específicas, como las revisiones de accesibilidad.

## Recursos

- [Qué es MCP y por qué se habla de ello][mcp-blog-post]
- [Servidor MCP de Microsoft Playwright][playwright-mcp-server]
- [Servidor MCP de GitHub][github-mcp-server]
- [Registro MCP de GitHub][mcp-registry]
- [Servidores MCP en VS Code][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
