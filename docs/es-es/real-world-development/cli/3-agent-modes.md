---
title: "Lección 3 - Modos de agente: Plan y Autopilot"
description: "Usa Plan para acordar un enfoque, Autopilot para implementar el filtrado desde una incidencia e Interactive para revisar el resultado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

El primer cambio era pequeño, pero las modificaciones mayores requieren un proceso más completo. En este flujo continuo partirás de una incidencia para planificar, implementar y revisar una funcionalidad antes de combinarla.

En esta lección:

- iniciarás una conversación de Copilot desde una incidencia de GitHub.
- definirás los requisitos en modo Plan.
- implementarás la funcionalidad con el modo Autopilot.
- revisarás el código.
- comprobarás manualmente la funcionalidad en un navegador reenviado.

En las siguientes lecciones ampliarás el flujo con instrucciones del repositorio, una habilidad `quality-checks`, validación con MCP, un agente QA y una solicitud de incorporación de cambios.

## Escenario

El catálogo de Tailspin Toys crece y los visitantes necesitan filtrar los juegos por categoría y editor. La incidencia del backlog describe la funcionalidad, pero antes de programar hay que acordar decisiones como la combinación de categorías. Usarás Plan para aclararlas y, después, autorizarás una implementación acotada con Autopilot.

## Modos de conversación

El modo de conversación determina cuánta autonomía tiene el agente. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para cambiar de modo.

- **Interactive**: colaboras con el agente, que espera tus indicaciones antes de continuar.
- **Plan**: el agente crea un plan y no puede editar los archivos del proyecto.
- **Autopilot**: el agente trabaja de forma autónoma, escribe código, ejecuta pruebas e itera hasta completar la tarea.

Empieza en Plan, revisa el plan y luego usa Autopilot para implementarlo.

## Partir de la incidencia

Prepara el repositorio y crea una rama para el trabajo de filtrado.

1. Vuelve a tu Codespace. Si está detenido, reinícialo.
2. Confirma que la PR de valoraciones por estrellas se ha combinado.
3. Si el terminal no está abierto, pulsa <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
4. Actualiza `main` y crea una rama:

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Inicia Copilot CLI:

   ```bash
   copilot --yolo
   ```

6. Pulsa <kbd>Tab</kbd> dos veces para abrir la pestaña **Issues**.
7. Pulsa <kbd>A</kbd> para mostrar todas las incidencias.
8. Usa las teclas de flecha para resaltar **Allow users to filter games by category and publisher**.
9. Pulsa <kbd>C</kbd> para añadir la incidencia al prompt y volver a la pestaña **Session**.

## Planificar el filtrado

Planificar permite acordar el enfoque antes de implementar una tarea compleja. Cambia al modo Plan y pide a Copilot que prepare una propuesta.

1. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para cambiar al modo Plan y confirma que el indicador bajo el prompt muestra **Plan**.
2. Después de la referencia a la incidencia, escribe:

   ```plaintext
   Create a plan for implementing this feature.
   ```

3. Responde a las preguntas de Copilot sobre el comportamiento del filtrado según tus preferencias.
4. Cuando termine el plan, pulsa <kbd>Control</kbd>+<kbd>E</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) para ampliarlo.
5. Recorre el plan y pide a Copilot que corrija cualquier parte que no refleje tus decisiones.

## Aprobar Autopilot

Una vez revisado el plan, autoriza la implementación y supervisa su avance.

1. Selecciona **Accept plan and build on autopilot (recommended)** o la opción equivalente en tu versión.
2. Confirma que el indicador muestra **Autopilot**.
3. Observa cómo Copilot implementa el plan y ejecuta las pruebas.

> [!NOTE]
> Al aprobar el plan, la implementación puede empezar de inmediato. Revísalo antes de aprobarlo. Si Copilot informa de dependencias ausentes o de un conflicto de puerto, resuelve el problema antes de considerar completas las comprobaciones.

## Revisar y comprobar la implementación

Revisa el código y los resultados de las comprobaciones antes de validar el sitio manualmente.

1. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para cambiar al modo Interactive y confirma que el indicador ya no muestra **Plan** ni **Autopilot**.
2. Introduce `/diff` e inspecciona la implementación del filtrado y sus pruebas.
3. Compara el resultado con la incidencia y las decisiones acordadas durante la planificación.
4. Pulsa <kbd>Esc</kbd> para salir de la vista de diferencias.
5. Revisa las comprobaciones del proyecto y pide a Copilot que resuelva los errores.

## Explorar la funcionalidad

Comprueba el filtrado en el sitio que se ejecuta en el puerto reenviado de Codespaces.

1. Pide a Copilot que inicie la aplicación:

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Cuando Codespaces indique que el puerto `4321` está disponible, selecciona **Open in Browser**.
3. Prueba los filtros por categoría y editor, así como las combinaciones acordadas en el plan.
4. Confirma que el reinicio de filtros y el comportamiento sin resultados coinciden con la incidencia y tus decisiones.
5. Vuelve a tu Codespace y pide a Copilot que detenga el servidor de desarrollo que inició.

## Resumen y pasos siguientes

Has utilizado distintos modos de conversación para desarrollar y revisar una funcionalidad. En esta lección:

- has iniciado una conversación de Copilot desde una incidencia de GitHub.
- has definido los requisitos en modo Plan.
- has implementado la funcionalidad con Autopilot.
- has revisado el código.
- has comprobado manualmente la funcionalidad en un navegador reenviado.

A continuación, [guiarás a Copilot con instrucciones personalizadas][next-lesson] para que el código siga las prácticas documentadas.

## Recursos

- [Autopilot en GitHub Copilot CLI][autopilot]
- [Referencia de comandos de Copilot CLI][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
