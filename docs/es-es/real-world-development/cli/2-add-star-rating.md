---
title: "Lección 2 - Añadir valoraciones por estrellas: una mejora rápida"
description: "Usa Copilot CLI para mostrar las valoraciones de los juegos, revisar los cambios en el navegador y combinar tu primera solicitud de incorporación de cambios."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Ahora que has instalado Copilot CLI y probado una conversación, harás un primer cambio pequeño. Los datos de los juegos ya incluyen la valoración por estrellas, pero las tarjetas de la página principal no la muestran. Pedirás al agente que la muestre, revisarás los cambios y los combinarás en tu primera solicitud de incorporación de cambios.

En esta lección:

- iniciarás una conversación específica de Copilot en una rama de funcionalidad.
- pedirás al agente que realice un cambio pequeño en el proyecto.
- revisarás las diferencias con `/diff`.
- ejecutarás la aplicación y comprobarás el cambio en el navegador reenviado.
- abrirás y combinarás tu primera solicitud de incorporación de cambios.

## Escenario

Cada juego de Tailspin Toys puede tener una valoración por estrellas, que ya aparece en la página de detalles. Sin embargo, las tarjetas solo muestran el título, la categoría, el editor y la descripción. Como primer ejercicio, pedirás al agente que muestre en cada tarjeta la valoración existente.

## Iniciar una conversación y pedir el cambio

La conversación de Copilot CLI utiliza el repositorio y la rama que tienes abiertos en el terminal. Regresa a Codespaces y empieza una sesión nueva para implementar el cambio.

1. Vuelve a tu Codespace.
2. Si el terminal no está abierto, pulsa <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Si Copilot CLI no está iniciado, ejecuta:

   ```bash
   copilot --yolo
   ```

4. Inicia una sesión nueva con `/new` y pulsa <kbd>Enter</kbd>.
5. Envía este prompt:

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

## Revisar las diferencias

Revisa los cambios generados y las comprobaciones antes de abrir la solicitud.

1. Introduce `/diff` e inspecciona todos los archivos modificados.
2. Confirma que la tarjeta muestra la valoración numérica cuando existe y `No rating yet` cuando `starRating` es `null`.
3. Confirma que las pruebas cubren ambos casos.
4. Revisa los resultados de las comprobaciones y pide a Copilot que corrija los errores.
5. Cuando termines la revisión, pulsa <kbd>Esc</kbd> para salir de la vista de diferencias.

> [!NOTE]
> Copilot es probabilístico, por lo que el código exacto puede variar. Revisa el comportamiento y no esperes una implementación idéntica a un ejemplo.

## Comprobar el cambio

Ejecuta la aplicación para comprobar en el navegador reenviado por Codespaces que las tarjetas muestran las valoraciones.

1. Pide a Copilot que inicie la aplicación:

   ```plaintext
   Start the app so I can inspect the star-rating change in my browser. Tell me the URL and leave the server running.
   ```

2. Cuando Codespaces indique que el puerto `4321` está disponible, selecciona **Open in Browser**.
3. Comprueba que las tarjetas muestran las valoraciones sobre cinco.
4. Vuelve a Copilot y pídele que detenga el servidor que inició:

   ```plaintext
   Stop the development server you started.
   ```

## Abrir y combinar tu primera solicitud

Después de revisar y comprobar el cambio, crea una solicitud de incorporación de cambios (PR) y combínala.

1. Pide al agente predeterminado que confirme el cambio:

   ```plaintext
   Commit the reviewed star-rating changes with an appropriate commit message.
   ```

2. Introduce `/pr create`. Copilot CLI sube la confirmación existente al crear la PR y muestra su URL.
3. Abre la PR manteniendo pulsado <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y seleccionando la URL que muestra Copilot CLI.
4. Revisa los archivos modificados y las comprobaciones.
5. Cuando esté todo listo, selecciona **Merge pull request** y confirma la combinación.
6. Vuelve a tu Codespace y sal de Copilot CLI con `/exit`.
7. Actualiza tu `main` local:

   ```bash
   git checkout main
   git pull
   ```

## Resumen y pasos siguientes

Has enviado tu primer cambio con GitHub Copilot CLI. En esta lección:

- has iniciado una conversación de Copilot específica en una rama de funcionalidad.
- has pedido al agente que realice un cambio pequeño en las tarjetas de juegos.
- has revisado los cambios con `/diff`.
- has ejecutado la aplicación y comprobado las valoraciones en un navegador reenviado.
- has abierto y combinado tu primera solicitud de incorporación de cambios.

A continuación, [partirás de la incidencia de filtrado y usarás los modos Plan y Autopilot][next-lesson] para desarrollar una funcionalidad mayor.

## Recursos

- [Acerca de GitHub Copilot CLI][about-copilot-cli]
- [Referencia de comandos de Copilot CLI][cli-reference]

[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
