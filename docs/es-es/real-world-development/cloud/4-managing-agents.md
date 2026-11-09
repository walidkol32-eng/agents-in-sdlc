---
title: "Ejercicio 4 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

El agente en la nube trabaja de forma asíncrona y autónoma, pero sigue siendo importante supervisar sus tareas. La página de agentes de GitHub.com muestra las tareas con solicitudes de incorporación de cambios (PR) abiertas, las operaciones realizadas y las sesiones que aún puedes orientar.

En este ejercicio:

- explorarás la página de agentes para supervisar tareas del agente en la nube.
- dirigirás una sesión activa para pedir una funcionalidad adicional.

## Escenario

Después de pedir al agente que implemente el modo de alto contraste, el equipo ha decidido añadir también un modo claro. Como ya se están actualizando los estilos y se está añadiendo un control de alternancia, pedirás al agente que amplíe su trabajo.

## Revisar las tareas del agente en la nube

Consulta el estado de las tareas que has asignado al agente.

1. Abre la [página de agentes de Copilot][agents-page].
2. Revisa las tareas del panel principal y del panel izquierdo, incluidas las de documentación, API para modificar juegos y modo de alto contraste.
3. Selecciona una tarea activa y revisa las operaciones realizadas, como obtener el código, preparar el entorno, configurar servidores del Model Context Protocol (MCP) y completar la tarea.
4. Revisa el panel de solicitud de incorporación de cambios, que muestra la PR y los archivos modificados.

> [!NOTE]
> Los pasos exactos varían según el estado del trabajo de Copilot y el enfoque que haya seguido.

## Dirigir el agente en la nube

Envía una indicación durante la sesión activa para que el agente añada el modo claro junto al modo de alto contraste.

1. Selecciona la sesión relacionada con el modo de alto contraste.

   ![Sesión de accesibilidad en el panel de agentes](../../../_images/ex6-accessibility-session.png)

2. Espera hasta que la sesión indique que ha terminado de preparar el entorno y ha empezado a trabajar.
3. En el cuadro **Steer active session while Copilot is working**, introduce:

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![Sesión del agente en la nube y cuadro para dirigir el trabajo activo](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pulsa <kbd>Enter</kbd> para enviar el prompt.
5. Comprueba que Copilot lo reconoce y lo incorpora al trabajo.

Copilot incorporará la solicitud después de terminar el paso que estaba ejecutando cuando enviaste el mensaje.

## Resumen y pasos siguientes

Has explorado la página de agentes de Copilot, el espacio central para supervisar y dirigir las tareas del agente en la nube. En este ejercicio:

- has explorado el panel de agentes para supervisar las tareas del agente en la nube.
- has redirigido una sesión activa para pedir una funcionalidad adicional.

Mientras Copilot termina el trabajo de accesibilidad, [iterarás sobre las solicitudes de incorporación de cambios que ha creado][next-lesson].

## Recursos

- [Página de agentes de Copilot][agents-page]
- [Acerca de los agentes personalizados][custom-agents]

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
