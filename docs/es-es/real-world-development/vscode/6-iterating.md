---
title: "Ejercicio 6: Iterar sobre el trabajo de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
next: false
---

## Revisar el trabajo

En este taller has utilizado el modo agente para añadir filtrado, el servidor MCP de Playwright para probarlo y un agente personalizado para implementar los modos de alto contraste y claro. Ahora publicarás el trabajo local y lo revisarás como lo haría el equipo.

### Escenario

Los fundamentos del diseño de software y DevOps no cambian al añadir IA generativa. Publicarás los cambios de accesibilidad desde tu Codespace y revisarás la solicitud de incorporación de cambios antes de compartirla con el equipo.

## Publicar las funciones de accesibilidad

Confirma que los cambios de alto contraste y modo claro están guardados en la rama y publica la solicitud de incorporación de cambios (PR).

1. Vuelve a tu Codespace y abre **Source Control** en VS Code.
2. Confirma que los cambios de accesibilidad están confirmados. Si quedan cambios sin confirmar, prepáralos y confírmalos con un mensaje descriptivo.
3. Selecciona **Publish Branch** o utiliza el menú **...** y selecciona **Push**.
4. Abre la rama en GitHub.com y crea una solicitud de incorporación de cambios.
5. Escribe un título y una descripción que expliquen el cambio y su propósito.
6. Selecciona **Create pull request**.
7. Abre la pestaña **Files changed** y revisa los controles, el almacenamiento local, los estilos y los atributos de accesibilidad.
8. Vuelve a **Conversation** y selecciona **Approve and run workflows** si hay flujos de trabajo pendientes.
9. Espera a que finalicen las comprobaciones.

> [!TIP]
> Para pedir otra opinión sobre la accesibilidad, menciona a `@copilot` en un comentario de la PR y solicita que revise posibles problemas de las pautas de accesibilidad para el contenido web (WCAG) o de navegación con teclado.

## Ejercicio opcional: seguir explorando en local

Puedes continuar trabajando con un agente desde VS Code. Por ejemplo, añade un formulario de interés para patrocinadores, implementa paginación, valida las entradas de los ayudantes de `src/lib/` o amplía la revisión de accesibilidad al orden del foco del teclado.

## Resumen

Has completado el recorrido de VS Code. En estos ejercicios:

- has utilizado Playwright MCP para probar manualmente la funcionalidad.
- has utilizado el modo agente para coordinar cambios en varias capas.
- has seleccionado un agente personalizado de accesibilidad y has revisado su trabajo.
- has supervisado y dirigido una sesión del agente.
- has publicado el trabajo local y revisado la PR.

## Repaso y pasos siguientes

Has completado el recorrido requerido de VS Code. También puedes explorar escenarios relacionados en el [recorrido de CLI][cli] o el [recorrido del agente en la nube][cloud], o adaptar instrucciones, agentes y habilidades de [awesome-copilot][awesome-copilot].

Como extensión opcional, [Incorporar Foundry][exercise-7] utiliza VS Code y Microsoft Foundry Toolkit para preparar un modelo, implementar Backer Concierge y conectarlo al sitio.

## Recursos

- [GitHub Copilot][github-copilot]
- [Copilot Chat en VS Code][copilot-chat-vscode]
- [Usar el modo agente][agent-mode]

[previous-lesson]: ../5-managing-agents/
[exercise-4]: ../4-custom-agents/
[exercise-5]: ../5-managing-agents/
[exercise-7]: ../7-foundry-toolkit/
[github-copilot]: https://github.com/features/copilot
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
[awesome-copilot]: https://github.com/github/awesome-copilot
[cli]: ../../cli/
[cloud]: ../../cloud/
