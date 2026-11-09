---
title: "Lección 8 - Crear y combinar la PR de funcionalidad"
description: "Revisa el filtrado, las instrucciones, la habilidad, el perfil QA y las pruebas; después, crea una PR y utiliza Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

La implementación del filtrado, las instrucciones, la habilidad, el perfil de control de calidad (QA) y las pruebas están guardados en una rama. Ahora revisarás los cambios en conjunto y abrirás una solicitud de incorporación de cambios (PR). La PR de valoraciones por estrellas la combinaste manualmente; esta vez dejarás que **Agent Merge** gestione el proceso.

> [!NOTE]
> En un flujo habitual, separarías la funcionalidad, las instrucciones, la habilidad y el agente QA en varias PR. Para simplificar el taller, has mantenido todo el trabajo de filtrado y calidad en una conversación y una rama.

En esta lección:

- aprenderás qué es Agent Merge y cómo automatiza el ciclo de una PR.
- inspeccionarás el cambio completo y las pruebas de verificación.
- crearás la PR de filtrado.
- activarás Agent Merge solo después de revisar los cambios y confirmarás que la PR se ha combinado.

## Escenario

Has utilizado Copilot para planificar, implementar y verificar una funcionalidad. Tailspin Toys quiere automatizar el trabajo pendiente de la PR, pero mantener bajo el control del desarrollador la autorización para combinarla.

## Acerca de Agent Merge

**Agent Merge** automatiza el trabajo restante para incorporar una PR. Cuando lo activas, Copilot resuelve los bloqueos de la PR, como las comprobaciones de integración continua (CI) fallidas, los comentarios de revisión y las necesidades de rebase. Después, habilita la combinación automática de GitHub si la configuración del repositorio lo permite.

Hasta ahora has seleccionado **Merge pull request** manualmente. Agent Merge puede encargarse de ese proceso, pero debes revisar el trabajo y decidir que está listo antes de activarlo.

## Usar Agent Merge para gestionar la PR

Revisa el código, crea la PR y, solo entonces, deja que Agent Merge gestione el resto.

1. Vuelve a tu Codespace.
2. Abre el diálogo de agentes con `/agent`.
3. Selecciona **Default** y pulsa <kbd>Enter</kbd>.
4. Crea una PR con `/pr create`.
5. Activa Agent Merge con `/pr agentmerge`.
6. Copilot supervisará la integración continua de la PR y, cuando todo se supere, la combinará.
7. Confirma que Copilot muestra un mensaje similar a «PR #14 was squash-merged successfully».

> [!IMPORTANT]
> Agent Merge no omite las aprobaciones obligatorias, la protección de ramas, las colas de combinación, la configuración del repositorio ni los permisos que falten. Si el proceso se bloquea, revisa el motivo indicado y completa manualmente la combinación revisada cuando el repositorio lo permita.

## Resumen y pasos siguientes

Has automatizado partes del desarrollo, desde la generación y verificación del código hasta la gestión de la PR. En esta lección:

- has aprendido qué es Agent Merge y cómo automatiza el ciclo de una PR.
- has inspeccionado el cambio completo y las pruebas de verificación.
- has creado la PR de filtrado.
- has activado Agent Merge después de revisar los cambios y has confirmado la combinación.

A continuación, [explorarás comandos de barra útiles de Copilot CLI][next-lesson] para gestionar el contexto, los modelos, el uso compartido y la delegación opcional a la nube.

## Recursos

- [Gestionar solicitudes de incorporación de cambios con Copilot CLI][manage-prs]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
