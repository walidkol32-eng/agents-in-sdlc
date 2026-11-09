---
title: "Ejercicio 5 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En el modo agente, GitHub Copilot explora el código, propone cambios, edita archivos y ejecuta comandos. Copilot Chat muestra las operaciones y permite revisar cada diferencia, aceptar o rechazar cambios y orientar la conversación.

En este ejercicio:

- supervisarás en tiempo real el trabajo del agente de accesibilidad.
- revisarás y aceptarás las ediciones propuestas.
- pedirás al agente que añada un modo claro junto al modo de alto contraste.

## Escenario

Mientras el agente de accesibilidad implementa el modo de alto contraste, el equipo de diseño solicita también un modo claro. En lugar de empezar una conversación nueva, ampliarás el trabajo en la sesión existente.

## Supervisar el progreso de Copilot

Revisa las llamadas a herramientas y las ediciones que propone el agente.

1. Vuelve a Copilot Chat desde el ejercicio anterior.
2. Recorre la conversación para ver las lecturas de archivos, las ediciones y los comandos del terminal.
3. En cada diferencia, selecciona **Keep** para aceptar el cambio o **Undo** para revertirlo.
4. Abre **Files changed** para revisar el conjunto completo de archivos modificados.
5. Espera a que el agente termine el modo de alto contraste.

> [!NOTE]
> Si una operación requiere permiso, Copilot mostrará **Allow**. Aprueba únicamente los comandos que quieras permitir.

## Dirigir al agente para añadir el modo claro

Continúa la misma conversación para que el agente conserve el contexto de los cambios y el patrón de almacenamiento local.

1. Envía este prompt en el cuadro de Copilot Chat:

   ```plaintext
   Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
   ```

2. Revisa las nuevas ediciones con **Keep** o **Undo**.
3. Si el agente sigue un enfoque que no quieres, selecciona **Stop** y envía una indicación para aclarar lo que necesitas.

> [!TIP]
> Las indicaciones de seguimiento permiten corregir el rumbo o ampliar el trabajo de una sesión sin empezar de cero.

## Revisar el conjunto de cambios

Antes de confirmar el trabajo, revisa todos los archivos modificados por el agente.

1. Abre **Source Control** en VS Code.
2. Revisa la lista completa de archivos y las diferencias.
3. Confirma que el trabajo mantiene los patrones de accesibilidad y que las preferencias se guardan en el almacenamiento local.
4. Cuando estés conforme, prepara y confirma los cambios desde el panel **Source Control**. Publicarás la rama en el siguiente ejercicio.

## Resumen y pasos siguientes

Has supervisado y dirigido una sesión activa de Copilot Chat, revisando las ediciones a medida que se proponían y ampliando el trabajo mediante indicaciones de seguimiento. En este ejercicio:

- has supervisado el trabajo de un agente en tiempo real.
- has aceptado y rechazado ediciones de archivos individuales.
- has dirigido una sesión con una indicación de seguimiento.

A continuación, [publicarás el trabajo de accesibilidad en una solicitud de incorporación de cambios][next-lesson] y revisarás los cambios del agente.

## Recursos

- [Copilot Chat en VS Code][copilot-chat-vscode]
- [Usar el modo agente][agent-mode]

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
