---
title: "Lección 6 - Validar la funcionalidad con Playwright MCP"
description: "Configura Playwright MCP en Copilot CLI y úsalo para explorar en un navegador la funcionalidad de filtrado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Los servidores Model Context Protocol (MCP) amplían Copilot CLI con herramientas y servicios externos. En esta lección configurarás el servidor MCP de Playwright si aún no está disponible y pedirás al agente que explore en el navegador la funcionalidad de filtrado.

En esta lección:

- aprenderás qué es Model Context Protocol (MCP) y cómo lo utiliza Copilot CLI.
- añadirás el servidor MCP de Playwright si aún no está disponible.
- pedirás al agente que controle un navegador y explore el filtrado.

## Escenario

Las pruebas unitarias y de extremo a extremo son importantes, pero también conviene interactuar con la interfaz como lo haría una persona. Playwright MCP permite a Copilot explorar el sitio en un navegador.

## ¿Qué es Model Context Protocol (MCP)?

[Model Context Protocol (MCP)][mcp-blog-post] permite que los agentes de IA se comuniquen con herramientas y servicios externos. Un servidor MCP actúa como puente y proporciona acceso a sus herramientas y recursos.

- **[Servidor MCP de GitHub][github-mcp]**: permite acceder a API para gestionar repositorios, incidencias y solicitudes de incorporación de cambios.
- **[Servidor MCP de Playwright][playwright-mcp-server]**: ofrece funciones de automatización del navegador mediante Playwright.

GitHub mantiene un [registro de MCP][mcp-registry] para facilitar la búsqueda y las contribuciones.

> [!CAUTION]
> Trata los servidores MCP como cualquier otra dependencia. Antes de utilizarlos, revisa el código fuente, verifica quién los publica y considera sus implicaciones de seguridad.

## Añadir el servidor MCP de Playwright

Si el servidor ya está configurado en Copilot CLI, omite estos pasos y continúa con la sección siguiente.

1. Vuelve a tu Codespace.
2. En Copilot CLI, introduce:

   ```plaintext
   /mcp add
   ```

3. Escribe `playwright` como nombre y pulsa <kbd>Tab</kbd>.
4. Confirma el tipo **STDIO** con <kbd>Enter</kbd> y pulsa <kbd>Tab</kbd>.
5. En el cuadro **Command**, pega:

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. Pulsa <kbd>Control</kbd>+<kbd>S</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) para guardar el servidor MCP.
7. Pulsa <kbd>Esc</kbd> para salir del cuadro de diálogo de MCP.

## Pedir a Copilot que explore la funcionalidad con Playwright

Pide a Copilot que inicie el sitio, compruebe el filtrado con Playwright MCP y detenga el servidor que haya iniciado.

1. Envía este prompt:

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

2. Observa los pasos que Copilot realiza en el navegador para comprobar la funcionalidad.
3. Lee el informe y comprueba que el comportamiento sea el esperado.

Copilot iniciará el servidor, interactuará con el sitio mediante Playwright, detendrá el servidor y te entregará un informe.

## Resumen y pasos siguientes

Has utilizado el servidor MCP de Playwright para explorar la funcionalidad en un navegador real desde Copilot CLI. En esta lección:

- has aprendido qué es MCP y cómo lo utiliza Copilot CLI.
- has añadido el servidor MCP de Playwright si aún no estaba disponible.
- has pedido al agente que controle el navegador y explore el filtrado.

A continuación, [crearás un agente QA personalizado][next-lesson] que combinará la habilidad y las herramientas del navegador.

## Recursos

- [Qué es MCP y por qué se habla de ello][mcp-blog-post]
- [Servidor MCP de Microsoft Playwright][playwright-mcp-server]
- [Añadir servidores MCP a Copilot CLI][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
