---
title: "Ejercicio 2 - Añadir funcionalidad con el modo agente de Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Instrucciones personalizadas][previous-lesson] · [Siguiente lección: Servidor MCP de Playwright →][next-lesson] |

Incluso una actualización sencilla suele requerir cambios en varios archivos y tareas, como ejecutar pruebas. El flujo de trabajo de desarrollo puede implicar buscar los archivos pertinentes, hacer cambios, ejecutar pruebas, depurar, identificar un archivo que se ha pasado por alto y volver a modificarlo.

El modo agente de Copilot puede ayudar con ese trabajo. En el entorno de desarrollo integrado (IDE), explora la estructura del proyecto, realiza los cambios necesarios, ejecuta tareas como las pruebas y corrige automáticamente los errores que detecta. En este ejercicio, lo utilizarás para añadir una funcionalidad al sitio.

> [!NOTE]
> Aunque sus nombres son parecidos, el modo agente y el agente en la nube ofrecen experiencias distintas. El modo agente trabaja en el IDE y permite ciclos de respuesta rápidos e interacción directa. El agente en la nube funciona de manera asíncrona, como otro miembro del equipo, y se comunica contigo mediante incidencias y solicitudes de incorporación de cambios.

En este ejercicio aprenderás a:

- utilizar el modo agente para explorar el proyecto, localizar archivos y coordinar cambios.
- implementar funcionalidades en la interfaz y la capa de datos.
- revisar los cambios y las pruebas antes de integrarlos en el código base.

## Escenario

El catálogo de Tailspin Toys crece y quieres que los usuarios puedan filtrar por categoría y editor. En el ejercicio anterior añadiste un ayudante para consultar los editores. Ahora completarás la capa de datos, la interfaz y las pruebas con el modo agente de Copilot.

## Ejecutar el sitio web de Tailspin Toys

Antes de cambiar el código, explora el sitio para entender su comportamiento actual.

El sitio web es una plataforma de financiación colectiva de juegos de mesa con una temática de desarrollo. Permite consultar juegos y ver sus páginas de detalles. Es una única aplicación Astro que genera páginas estáticas en lenguaje de marcado de hipertexto (HTML) durante la compilación. Las páginas consultan directamente una base de datos SQLite mediante Drizzle, un mapeador objeto-relacional (ORM), desde el frontmatter; no hay una interfaz de programación de aplicaciones (API) de backend independiente ni un framework de interfaz del lado del cliente. Los ayudantes reutilizables para acceder a los datos están en `src/lib/`, y la interactividad se implementa con un pequeño elemento `<script>` de Astro que usa las API estándar del modelo de objetos del documento (DOM).

### Iniciar el sitio web

Para facilitar el inicio del sitio, el proyecto incluye la tarea `npm run dev`, que ejecuta el único servidor de desarrollo de Astro. Inicia el sitio en tu Codespace y prepara una rama para el trabajo de filtrado.

1. Vuelve a tu Codespace y confirma que estás en `main`.
2. Abre un terminal con <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Actualiza `main` y crea una rama:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Inicia el sitio:

   ```bash
   npm run dev
   ```

   Cuando el servidor de desarrollo Astro esté listo, mostrará un aviso con la dirección, similar a este:

   ```text
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

5. Si se abre un cuadro de diálogo para preguntarte si quieres abrir `http://localhost:4321` en un navegador, ciérralo seleccionando **x**.
6. Mantén pulsado <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y selecciona la dirección del servidor Astro `http://localhost:4321` en el terminal.

En Codespaces, al seleccionar el enlace a la dirección local en el terminal se te redirigirá automáticamente a `https://<your-codespace-name>-4321.app.github.dev/`. Se trata de un túnel privado a tu Codespace, que ahora aloja el servidor web.

### Explorar el sitio

Las principales funciones del sitio son:

- **Página de inicio**: muestra juegos de mesa con sus títulos, imágenes y descripciones.
- **Página de detalles del juego**: al seleccionar un juego, se abre su página de detalles con más información, como el título, la descripción, la editorial y la categoría.

## Explorar el backlog con Copilot

Pide a Copilot que muestre las incidencias del proyecto y te ayude a priorizarlas.

> [!TIP]
> Antes de empezar, vuelve a tu Codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación limpia. El modo y el modelo varían según el ejercicio; se indicarán cuando sea necesario.

La implementación inicial del sitio es funcional, pero quieres añadir nuevas capacidades. Al crear el proyecto a partir de la plantilla se generó automáticamente un backlog de incidencias. Pide a GitHub Copilot que te muestre esas tareas.

1. Selecciona **Agent** en el menú desplegable de agentes de la vista Chat. El agente puede planificar y aplicar cambios de forma autónoma en varios archivos, ejecutar comandos de terminal y utilizar herramientas.

   ![Selector de agente en la vista Chat](../../../_images/shared-chat-mode-selector.png)

2. Selecciona **Claude Sonnet 4.5** en la lista de modelos disponibles.

> [!CAUTION]
> Los autores del taller no prefieren un modelo concreto. Durante su desarrollo se utilizó Claude Sonnet 4.5 para que las sugerencias fueran relativamente coherentes y ofrecer una experiencia fluida. Los modelos de lenguaje de gran tamaño (LLM) son probabilísticos, así que las sugerencias pueden diferir de los ejemplos; es normal.

> [!NOTE]
> Debido al carácter probabilístico de los LLM, Copilot puede utilizar un comando del Model Context Protocol (MCP) diferente, pero debería poder completar la tarea.

3. Envía este prompt:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecciona **Continue** para ejecutar el comando que enumera las incidencias.
5. Revisa la lista y las prioridades propuestas.

Copilot puede priorizar las incidencias según las que considere más útiles para los usuarios.

## Revisar los archivos de instrucciones

Antes de pedir a Copilot que genere código, revisa el archivo de instrucciones que le proporcionará contexto para su trabajo. Usarás el archivo de instrucciones de interfaz de usuario (UI), que explica cómo añadir funcionalidad al sitio web.

1. En tu Codespace, abre `.github/instructions/ui.instructions.md`.
2. Revisa las directrices generales para añadir funcionalidad, entre ellas:
   - Un resumen de la arquitectura.
   - Principios de diseño de componentes, capacidad de prueba y accesibilidad.
   - Enlaces a instrucciones específicas para distintos tipos de archivo, como Astro, Tailwind CSS y Drizzle.

   ![Ejemplo de selección del archivo de instrucciones de interfaz](../../../_images/ex3-select-instructions-file.png)

> [!TIP]
> Los archivos de instrucciones pueden incluir referencias a otros archivos de instrucciones y a archivos del proyecto. Las rutas son relativas a la ubicación del archivo de instrucciones. Así puedes reutilizar instrucciones, dividir conjuntos complejos en partes más manejables y proporcionar ejemplos y plantillas.

## Implementar el filtrado

El filtrado requiere actualizar la lógica de datos en `src/lib/`, añadir o ajustar pruebas y modificar la página de listado de juegos. Pide a Copilot que complete el trabajo según la incidencia.

1. Continúa en la conversación actual o selecciona **New Chat**.
2. Selecciona **Add Context**, **Instructions** y **ui**.
3. Confirma que está seleccionado **Agent** y que el modelo es **Claude Sonnet 4.5**.

   ![Selector de agente en la vista Chat](../../../_images/shared-chat-mode-selector.png)
4. Envía este prompt:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

5. Observa cómo Copilot explora el proyecto, localiza los archivos relacionados con la funcionalidad —incluidos los ayudantes de la capa de datos, la interfaz y las pruebas— y empieza a modificarlos y ejecutar pruebas.

   ![Copilot explora los archivos del proyecto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot puede realizar varias tareas, como explorar el proyecto, modificar archivos y ejecutar pruebas. El proceso puede tardar unos minutos según la complejidad de la tarea y del código base. Mientras trabaja, pueden aparecer los botones **Keep** y **Undo** en el editor. Cuando termine, tendrás la opción de conservar o deshacer todos los cambios; no es necesario seleccionarlos mientras Copilot sigue trabajando.

6. Cuando Copilot lo solicite, selecciona **Continue** para ejecutar las pruebas.

   ![Copilot Chat solicita permiso para ejecutar pruebas](../../../_images/ex3-agent-mode-run-tests.png)

7. Ten en cuenta que el proceso puede detenerse y mostrar pruebas fallidas. Copilot alterna entre generar código y ejecutar pruebas hasta completar la tarea y no detectar errores.

   ![Sesión completa de Copilot en modo agente](../../../_images/ex3-agent-mode-proposed-changes.png)

8. Revisa el código generado para detectar posibles problemas.
9. Vuelve al navegador con el sitio en ejecución y explora la nueva funcionalidad.
10. Cuando hayas comprobado que funciona y revisado el código, selecciona **Keep** en la ventana de Copilot Chat.

> [!CAUTION]
> Revisa siempre el código generado por Copilot u otras herramientas de inteligencia artificial antes de aceptarlo.

## Resumen y pasos siguientes

Has utilizado el modo agente de Copilot para añadir funcionalidad al sitio Tailspin Toys. En este ejercicio:

- has utilizado el modo agente para explorar el proyecto, localizar archivos y coordinar cambios.
- has implementado filtrado en la interfaz y la capa de datos.
- has revisado los cambios y las pruebas antes de incorporarlos.

A continuación, [probarás la funcionalidad con el servidor MCP de Playwright][next-lesson].

### Ejercicio de exploración adicional: implementar paginación

Al crecer la lista de juegos, será necesario habilitar la paginación. Aplica lo aprendido en este ejercicio y pide a Copilot que la implemente. Ten en cuenta estas decisiones:

- Sigue las prácticas recomendadas existentes, incluidos los archivos de instrucciones.
- Decide cómo quieres implementar la paginación, por ejemplo, si los usuarios podrán elegir el tamaño de página o si será fijo.
- En el prompt, proporciona a Copilot las directrices necesarias para crear la implementación que necesitas.
- Puede que tengas que iterar con Copilot, pedir cambios y proporcionar contexto. Es el flujo de trabajo habitual.

## Recursos

- [Modos ask, edit y agent de Copilot][choose-mode]
- [Modo agente en VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
