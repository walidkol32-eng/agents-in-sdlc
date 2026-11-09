---
title: "Ejercicio 4 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Probar la funcionalidad con el servidor del Model Context Protocol (MCP) de Playwright][previous-lesson] | [Siguiente lección: Supervisar y gestionar agentes →][next-lesson] |
|:--|--:|

## ¿Qué son los agentes personalizados?

Los [agentes personalizados][custom-agents-concept] de GitHub Copilot permiten crear asistentes de inteligencia artificial especializados en tareas o ámbitos concretos del flujo de desarrollo. Se definen mediante archivos Markdown en `.github/agents` y proporcionan a Copilot instrucciones específicas, prácticas recomendadas, patrones de código y conocimientos del dominio para que realice determinados tipos de trabajo de forma más eficaz. Los equipos pueden plasmar su experiencia en agentes reutilizables: por ejemplo, un agente de accesibilidad que compruebe el cumplimiento de las pautas de accesibilidad para el contenido web ([WCAG][wcag]), uno de seguridad que siga prácticas de programación segura o uno de pruebas que mantenga patrones coherentes.

Los agentes personalizados se definen en archivos Markdown dentro de `.github/agents` del proyecto o, de forma global, en `~/.copilot/agents`. Cada archivo tiene frontmatter YAML con, como mínimo, los campos `name` y `description`, seguido de una indicación en Markdown que define el comportamiento, la experiencia y las instrucciones del agente.

### Agentes personalizados y habilidades de agente

Los agentes personalizados y las [habilidades de agente][agent-skills-concept] se solapan en algunos aspectos: ambos se definen principalmente mediante archivos Markdown e indican a una inteligencia artificial cómo realizar determinadas operaciones. La forma más sencilla de distinguirlos es pensar que el **agente personalizado** es quien realiza el trabajo y las **habilidades** son las herramientas.

Los agentes personalizados tienen su propia ventana de contexto y están diseñados para coordinar habilidades e incluso otros agentes. En este ejercicio, el agente de accesibilidad revisará y actualizará el sitio según las pautas de accesibilidad. Como parte de ese trabajo, podría utilizar habilidades como una para gestionar el flujo de solicitudes de incorporación de cambios o una que ejecute y administre pruebas.

> [!NOTE]
> No hay una única forma correcta de crear agentes personalizados. Pruébalos e itera para encontrar lo que funciona en tu entorno.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

En este ejercicio:

- revisarás un agente personalizado de accesibilidad existente.
- utilizarás el agente de accesibilidad en Copilot Chat para implementar un modo de alto contraste.

## Escenario

Tailspin Toys se compromete a que todas las personas puedan utilizar su plataforma de financiación colectiva, independientemente de sus capacidades o preferencias visuales. Algunos comentarios recientes indican que a ciertas personas les cuesta leer el tema oscuro porque el contraste entre el texto y el fondo es insuficiente. Para resolver este problema de accesibilidad, el equipo de diseño ha solicitado un modo de alto contraste que se pueda activar y desactivar. Como la accesibilidad es fundamental, quieres implementar esta funcionalidad cuanto antes y utilizarás un agente personalizado para generarla.

## Revisar el agente de accesibilidad

Revisa el perfil existente para saber cómo orientará a Copilot.

Vuelve a tu Codespace, abre un terminal y prepara una rama nueva a partir de `main` para trabajar en la accesibilidad. Así mantendrás separada esta tarea de la solicitud de incorporación de cambios (PR) de filtrado del ejercicio anterior.

1. Ejecuta los comandos siguientes:

   ```bash
   git checkout main
   git pull
   git checkout -b accessibility-vscode
   ```

2. Abre `.github/agents/accessibility.md`.
3. Revisa los campos `name` y `description` del frontmatter.
4. Examina las secciones siguientes, incluidas:
   - Las responsabilidades principales al generar código para un sitio accesible.
   - Las prácticas recomendadas de accesibilidad.
   - Ejemplos de lenguaje de marcado de hipertexto (HTML), hojas de estilo en cascada (CSS) y JavaScript.
   - Una lista de errores frecuentes.

> [!CAUTION]
> El frontmatter con `name` y `description` es obligatorio para los agentes personalizados.

## Utilizar el agente personalizado en Copilot Chat

VS Code muestra todos los agentes personalizados definidos en `.github/agents` en el selector de agentes situado en la parte inferior de la vista Copilot Chat. Puedes seleccionar un agente personalizado para limitar la sesión de chat a sus instrucciones y herramientas.

> [!TIP]
> Antes de empezar, vuelve a tu Codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación limpia. El modo y el modelo varían según el ejercicio; se indicarán cuando sea necesario.

1. En Copilot Chat, selecciona **Agent** si aún no está seleccionado.

   ![Selector de agente en la vista Chat](../../../_images/shared-chat-mode-selector.png)

2. Abre el selector de agentes situado en la parte inferior de la vista de chat; muestra el agente activo, que de forma predeterminada es **default**.
3. Selecciona **Accessibility agent** en la lista de agentes disponibles.
4. Envía este prompt al agente de accesibilidad:

   ```plaintext
   Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
   ```

5. Espera mientras Copilot explora el código base, propone cambios y edita los archivos del proyecto. Cada edición aparecerá en el chat con la ruta del archivo y una diferencia que podrás revisar.
6. Observa cómo se actualiza el indicador **Files changed** para mostrar los archivos que ha modificado el agente.

> [!NOTE]
> El proceso puede tardar varios minutos. Copilot realizará cambios reales en el repositorio: editará archivos existentes, como componentes de Astro, estilos CSS y pruebas relacionadas.

Revisarás y dirigirás el trabajo en curso en la siguiente lección.

## Resumen y pasos siguientes

Has explorado los agentes personalizados de GitHub Copilot y has utilizado el agente de accesibilidad en Copilot Chat. En este ejercicio:

- has revisado un agente personalizado de accesibilidad existente.
- has utilizado el agente de accesibilidad en Copilot Chat para implementar un modo de alto contraste.

A continuación, [supervisarás y dirigirás el trabajo del agente][next-lesson], revisarás los cambios y ampliarás la misma sesión para añadir un modo claro.

## Recursos

- [Acerca de los agentes personalizados][custom-agents]
- [Crear agentes personalizados en el IDE][creating-custom-agents-ide]
- [Agentes personalizados en VS Code][custom-agents-vscode]
- [Configuración de agentes personalizados][custom-agents-config]
- [Agentes personalizados en awesome-copilot][awesome-copilot-agents]

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents

---

| [← Lección anterior: Probar la funcionalidad con el servidor MCP de Playwright][previous-lesson] | [Siguiente lección: Supervisar y gestionar agentes →][next-lesson] |
|:--|--:|
