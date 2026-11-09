---
title: "Ejercicio 1 - Instalar GitHub Copilot CLI"
description: "Instala y autentica Copilot CLI en tu Codespace, familiarízate con la herramienta y localiza la incidencia de filtrado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

[GitHub Copilot CLI][about-copilot-cli] es un asistente de programación con agentes que se ejecuta en el terminal. Permite explorar bases de código, generar código, ejecutar comandos e interactuar con herramientas externas desde la línea de comandos. Instalarás la herramienta, te autenticarás y la prepararás para el resto del taller.

En este ejercicio:

- instalarás GitHub Copilot CLI mediante npm.
- te autenticarás con tu cuenta de GitHub.
- confiarás en el repositorio del taller y probarás una conversación.
- localizarás la incidencia de filtrado con el servidor MCP de GitHub integrado.

## Escenario

Tu equipo está empezando a utilizar agentes de IA para abordar un backlog creciente. Copilot CLI incorpora esa capacidad al terminal, donde ya trabajan muchos desarrolladores.

## Instalar Copilot CLI

Puedes instalar Copilot CLI con [npm][install-cli], WinGet o Homebrew. Como GitHub Codespaces incluye Node.js, utilizarás npm.

1. Vuelve a tu Codespace y abre un terminal.
2. Comprueba que Node.js esté instalado y cumpla el requisito de versión:

   ```bash
   node --version
   ```

   La versión debe ser 24 o posterior.

3. Instala Copilot CLI de forma global:

   ```bash
   npm install -g @github/copilot
   ```

4. Comprueba la instalación:

   ```bash
   copilot --version
   ```

## Autenticarse con GitHub

La primera vez que inicies Copilot CLI, tendrás que autenticarte con tu cuenta de GitHub.

1. Inicia Copilot CLI:

   ```bash
   copilot
   ```

2. Si se te solicita, sigue las instrucciones del código de dispositivo para autenticarte y autorizar Copilot CLI.
3. Comprueba que el prompt muestra este mensaje:

   ```plaintext
   Copilot can read files in this folder and, with your permission, edit them or run code and shell commands. It will remember your permissions for the rest of this session.

   Do you trust the files in this folder?
   ```

4. Confirma que la ruta corresponde a tu repositorio Tailspin Toys y selecciona **Yes, and remember this folder for future sessions**.

> [!NOTE]
> En Codespaces, puede que ya estés autenticado mediante tu sesión de GitHub. Si Copilot CLI se inicia sin pedir autenticación, ya puedes continuar.

## Familiarizarse con la herramienta

Los comandos introducidos en el prompt habitual del shell se ejecutan directamente en Codespaces. Cuando inicias Copilot CLI, el lenguaje natural se envía al agente y los comandos de barra controlan la conversación.

1. Introduce `/model`, utiliza las teclas de flecha para seleccionar **Auto**, pulsa <kbd>Enter</kbd> y vuelve a pulsar <kbd>Enter</kbd> para confirmar.
2. Introduce `/help` para ver los comandos disponibles en tu versión y pulsa <kbd>Esc</kbd> para cerrar la ayuda.
3. Haz una pregunta sencilla:

   ```plaintext
   What are the key files in this project?
   ```

4. Lee la respuesta y observa cómo Copilot explora el repositorio antes de responder.
5. Introduce `/mcp list` y confirma que está disponible el servidor MCP de GitHub integrado.
6. Pide a Copilot que encuentre la incidencia de filtrado:

   ```plaintext
   Using GitHub MCP, find the issue in this repository titled "Allow users to filter games by category and publisher." Give me its URL and a short summary. Don't change anything.
   ```

7. Abre la URL y lee la incidencia. La utilizarás después de completar un primer cambio rápido.

> [!TIP]
> Una sesión normal de Copilot CLI utiliza la rama que está activa en el terminal y no crea automáticamente un árbol de trabajo. Crearás una rama de funcionalidad antes de cada cambio.

## Utilizar el acceso directo del taller

Copilot CLI suele pedir permiso antes de utilizar herramientas fuera de los permisos establecidos. En este taller, lo iniciarás con `--yolo` en el Codespace para evitar estas confirmaciones y centrarte en los ejercicios.

> [!CAUTION]
> `--yolo` habilita los permisos automáticos completos (`--allow-all-tools`, `--allow-all-paths` y `--allow-all-urls`). Úsalo solo en un entorno aislado, como un Codespace o una máquina virtual, y nunca lo configures como alias predeterminado para el desarrollo diario. Consulta [Permitir y denegar el uso de herramientas][allow-all-warning] para obtener más información.

`--enable-all-github-mcp-tools` habilita las herramientas de lectura y escritura de GitHub MCP que se utilizarán en lecciones posteriores para trabajar con incidencias y solicitudes de incorporación de cambios. Aunque Codespaces limita el acceso al equipo local, los recursos de GitHub autenticados siguen siendo reales. Revisa los cambios antes de publicarlos o combinarlos.

1. Sal de Copilot CLI con `/exit`.
2. Vuelve a iniciarlo desde la raíz del repositorio:

   ```bash
   copilot --yolo --enable-all-github-mcp-tools
   ```

3. Haz otra pregunta breve sobre el proyecto para confirmar que la conversación funciona y sal con `/exit`.

Copilot guarda las conversaciones automáticamente. Más adelante, cuando cambies las instrucciones o añadas un agente, utilizarás `copilot --resume` para volver a la misma conversación y rama.

## Resumen y pasos siguientes

Has instalado y autenticado GitHub Copilot CLI. En este ejercicio:

- has instalado Copilot CLI mediante npm.
- te has autenticado con tu cuenta de GitHub.
- has confiado en el repositorio del taller y probado una conversación.
- has localizado la incidencia de filtrado con el servidor MCP de GitHub integrado.

A continuación, [iniciarás tu primer cambio específico][next-lesson] y utilizarás Copilot CLI para mostrar las valoraciones por estrellas en las tarjetas de juegos.

## Recursos

- [Instalar GitHub Copilot CLI][install-cli]
- [Acerca de GitHub Copilot CLI][about-copilot-cli]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-add-star-rating/
[install-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/set-up-copilot-cli/install-copilot-cli
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[allow-all-warning]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli#allowing-and-denying-tool-use
