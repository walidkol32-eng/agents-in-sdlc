---
title: "Ejercicio 5: Iterar sobre el trabajo de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Supervisar y gestionar agentes][previous-lesson] |
|:--|

## Revisar el trabajo

En este taller has pedido a GitHub Copilot que documente el código, añada juegos relacionados y mejore la accesibilidad con modos de alto contraste y claro. Ahora revisarás las solicitudes de incorporación de cambios (PR) y, si hace falta, pedirás mejoras antes de integrar el trabajo.

### Escenario

Los principios de diseño de software y desarrollo y operaciones (DevOps) siguen siendo importantes al utilizar inteligencia artificial generativa. Revisa el código que genera Copilot y sigue el proceso habitual de DevOps. Antes de habilitar la revisión para el resto del equipo, examina sus propuestas para la documentación, los juegos relacionados y las funciones de accesibilidad.

## Seguridad y agente en la nube de GitHub Copilot

Como el agente en la nube trabaja de forma asíncrona y sin supervisión, se aplican algunas restricciones de seguridad:

- Copilot solo tiene acceso de lectura al repositorio y acceso de escritura únicamente a la rama que utiliza.
- El agente se ejecuta en GitHub Actions, donde crea un entorno efímero e independiente para trabajar.
- Una persona debe aprobar los flujos de trabajo de GitHub Actions antes de que se ejecuten.
- [El acceso a recursos externos está limitado de forma predeterminada][agent-firewall], incluidos los servidores del Model Context Protocol (MCP).

## Revisar la documentación generada

Empieza por revisar la primera PR que generó el agente en la nube de GitHub Copilot: la que añade documentación al código. Utilizarás la interfaz habitual de PR de GitHub.com.

> [!NOTE]
> Es posible que la PR muestre un aviso de que el firewall bloquea a GitHub Copilot. Es el comportamiento previsto: el acceso a recursos externos está limitado de forma predeterminada, incluidas las llamadas a servidores MCP externos. Si quieres, puedes [configurar o desactivar el firewall del agente en la nube][agent-firewall].

1. Vuelve al repositorio en GitHub.com.
2. Selecciona **Pull Requests** para abrir la lista de PR.
3. Abre la PR cuyo título sea parecido a **Add missing documentation**.
4. Si Copilot sigue trabajando, la PR incluirá la etiqueta **[WIP]**. Espera a que termine; puede tardar varios minutos.
5. Cuando la PR esté lista, selecciona la pestaña **Files changed** y revisa los cambios.

   ![Pestaña Files changed](../../../_images/shared-pr-files-changed.png)

6. Explora el código actualizado, que incluirá comentarios de documentación TSDoc y otros cambios. El contenido exacto puede variar.
7. Mientras revisas, busca comentarios de documentación TSDoc, convenciones de TypeScript y encabezados de comentarios. Estas mejoras proceden de los archivos de instrucciones que revisaste al principio del ejercicio 2.
8. Cuando termines la revisión y los cambios sean adecuados, vuelve a **Conversation** y desplázate hacia abajo.
9. Si hay flujos de trabajo pendientes de aprobación, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

10. Comprueba que los flujos de trabajo se hayan puesto en cola en la sección de comprobaciones de la PR. Si todo va bien, las comprobaciones del proyecto para la aplicación Astro se completarán correctamente; pueden tardar varios minutos.

## Pedir cambios a GitHub Copilot

La colaboración con Copilot no termina en una sola dirección. Puedes mencionarlo en comentarios de la PR o en comentarios insertados en el código, igual que harías con otras personas del equipo. Copilot verá el comentario e iniciará otra sesión para resolverlo. Como los resultados no son deterministas, no hay una indicación única que debas utilizar. Algunas ideas para pedir cambios son:

- Añadir encabezados de comentarios al principio de cada archivo de código con una breve descripción de su propósito.
- Añadir comentarios de documentación TSDoc a los archivos TypeScript y Astro.
- Crear un archivo README que describa la estructura de la aplicación Astro.

1. Añade un comentario que mencione a **@copilot**, como mencionarías a cualquier otra persona, y pide un cambio en la documentación. Puedes utilizar una de las ideas anteriores u otra que te interese.
2. Selecciona **View Session** para observar cómo Copilot inicia una sesión nueva y realiza el trabajo.
3. Selecciona **Back to pull request** para volver a la PR.
4. Cuando Copilot termine, comprueba que haya una confirmación nueva en la PR.
5. Selecciona **Files changed** y revisa los cambios.

Puedes seguir iterando hasta que estés conforme. Después, convierte la PR en lista para revisión y combínala.

![Convertir una PR en lista para revisión](../../../_images/ex7-ready-for-review.png)

## Revisar la funcionalidad de juegos relacionados

Vuelve a la PR que Copilot generó para resolver la incidencia de juegos relacionados en la página de detalles.

1. Vuelve al repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Selecciona la PR cuyo título sea parecido a **Show related games on the game details page**.
4. Selecciona la pestaña **Files changed** para revisar el código generado.
5. Cuando termines la revisión y los cambios sean adecuados, vuelve a **Conversation** y desplázate hacia abajo.
6. Si hay flujos de trabajo pendientes de aprobación, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

7. Comprueba que los flujos de trabajo se hayan puesto en cola en la sección de comprobaciones de la PR. Si todo va bien, las comprobaciones del proyecto para la aplicación Astro se completarán correctamente; pueden tardar varios minutos.
8. **Opcional:** cambia a esta rama en tu Codespace para probar manualmente la funcionalidad de juegos relacionados. Abre el terminal y ejecuta estos comandos; sustituye `<branch-name>` por el nombre de la rama creada por Copilot, por ejemplo, **copilot/fix-8**:

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

Copilot ha creado la funcionalidad de juegos relacionados. Puedes seguir trabajando con el agente en la nube de forma iterativa y pedir cambios, por ejemplo, para ajustar cuántos juegos relacionados se muestran o para añadir encabezados de comentarios y comentarios de documentación TSDoc. Recuerda que esta tarea se asignó antes de actualizar las instrucciones personalizadas. Para pedir estos cambios, añade otro comentario en **Conversation**; Copilot lo verá e iniciará una sesión nueva.

## Revisar las funciones de accesibilidad

Por último, revisa las funciones de accesibilidad implementadas con el agente personalizado. La PR debe incluir tanto el modo de alto contraste que asignaste en el ejercicio anterior como el modo claro que pediste durante la supervisión de la sesión.

1. Vuelve al repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Selecciona la PR cuyo título sea parecido a **Add high contrast mode to website**.
4. Si Copilot sigue trabajando, la PR incluirá la etiqueta **[WIP]**. Espera a que termine; puede tardar varios minutos.
5. Selecciona la pestaña **Files changed** para revisar el código generado.
6. Presta atención a estos aspectos de la implementación:
   - Los controles de alternancia entre los modos.
   - El uso del almacenamiento local para conservar las preferencias.
   - Los cambios en las hojas de estilo en cascada (CSS) para los modos de alto contraste y claro.
   - Los atributos de accesibilidad, como las etiquetas de Accessible Rich Internet Applications (ARIA) y la navegación con teclado.
   - El código JavaScript o TypeScript que gestiona el cambio de modo.
7. Cuando termines la revisión y los cambios sean adecuados, vuelve a **Conversation** y desplázate hacia abajo.
8. Si hay flujos de trabajo pendientes de aprobación, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Comprueba que los flujos de trabajo se hayan puesto en cola en la sección de comprobaciones de la PR. Si todo va bien, las comprobaciones del proyecto para la aplicación Astro se completarán correctamente; pueden tardar varios minutos.
10. **Opcional:** cambia a esta rama en tu Codespace para probar manualmente las funciones de accesibilidad. Abre el terminal y ejecuta estos comandos; sustituye `<branch-name>` por el nombre de la rama creada por Copilot:

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

11. Si has cambiado a la rama de Copilot, inicia la aplicación y prueba los controles de los modos de alto contraste y claro en el navegador. Comprueba que funcionen y que las preferencias se conserven al recargar la página.

Si encuentras problemas de accesibilidad, menciona a **@copilot** en un comentario para solicitar una actualización.

## Ejercicio opcional: seguir delegando

El agente en la nube funciona mejor cuando puede ocuparse de elementos reales del backlog mientras tú haces otras tareas. Para crear ese hábito, abre algunas incidencias nuevas en el repositorio y asígnaselas a Copilot. Por ejemplo:

- Crear un formulario de interés para quienes apoyan los juegos en la página de detalles.
- Implementar la paginación en la lista de juegos.
- Añadir validación de entradas y gestión de errores a los ayudantes de acceso a datos.

## Resumen

Has completado el recorrido del agente en la nube. En estos ejercicios:

- has inspeccionado las instrucciones personalizadas del repositorio.
- has asignado incidencias al agente y observado su trabajo asíncrono.
- has creado y utilizado un agente personalizado de accesibilidad para añadir los modos de alto contraste y claro.
- has utilizado la página de agentes para supervisar y dirigir la sesión.
- has revisado e iterado sobre las PR del agente y aprobado los flujos de trabajo.

## Repaso y pasos siguientes

Has completado el recorrido del agente en la nube. Si quieres seguir explorando, los otros entornos complementan lo que has practicado:

- [Recorrido de VS Code][vscode]: explora el modo Agente de Copilot Chat y la integración con MCP directamente desde el IDE.
- [Recorrido de CLI][cli]: implementa desde el terminal dos cambios revisados con los modos Plan y Autopilot, las habilidades de agente, un agente de calidad personalizado, Playwright MCP, Agent Merge y, de forma opcional, el puente `/delegate` hacia el agente en la nube.

En tu propio repositorio, prueba estas ideas:

- Asigna al agente en la nube una incidencia de refactorización o cobertura de pruebas.
- Crea un agente personalizado para otro ámbito.
- Configura `copilot-setup-steps.yml` para otra pila tecnológica.

## Recursos

- [GitHub Copilot][github-copilot]
- [Acerca de los agentes de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para configurar el agente en la nube][cloud-agent-best-practices]
- [Configurar el firewall del agente en la nube][agent-firewall]

[previous-lesson]: ../4-managing-agents/
[github-copilot]: https://github.com/features/copilot
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall
[vscode]: ../../vscode/
[cli]: ../../cli/
