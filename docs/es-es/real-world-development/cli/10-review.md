---
title: "Lección 10 - Repaso y pasos siguientes"
description: "Repasa el flujo de Copilot CLI, las dos solicitudes de incorporación de cambios, las personalizaciones reutilizables y otros recursos."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Has utilizado GitHub Copilot CLI en un flujo continuo de Tailspin Toys. En este taller:

- has preparado un Codespace, instalado Copilot CLI, explorado el proyecto y encontrado la incidencia de filtrado del backlog.
- has añadido las valoraciones por estrellas, revisado el resultado en un navegador reenviado y combinado manualmente tu primera solicitud de incorporación de cambios (PR).
- has partido de la incidencia de filtrado, definido el enfoque en modo Plan, implementado con Autopilot y revisado con Interactive.
- has guiado al agente con instrucciones personalizadas, adaptado la habilidad `quality-checks` y utilizado esta para ejecutar pruebas unitarias, lint y comprobaciones de tipos.
- has añadido el servidor Model Context Protocol (MCP) de Playwright y lo has utilizado para explorar el filtrado en un navegador.
- has creado y seleccionado un agente personalizado de control de calidad (QA) para revisar los requisitos, la cobertura, los resultados de la habilidad y las pruebas del navegador.
- has revisado el cambio completo de filtrado y autorizado Agent Merge para esa PR.
- has explorado comandos de barra para el contexto, los modelos, el uso compartido y la delegación opcional a la nube.

## Cambios realizados

El taller incluye dos hitos de PR, cada uno en su propia rama basada en `main` actualizado:

1. **Valoraciones por estrellas:** muestra `starRating` y un estado explícito para los juegos sin valorar.
2. **Filtrado y flujo de calidad:** implementa el filtrado, actualiza las instrucciones y las aplica a la funcionalidad, personaliza el informe de `quality-checks`, crea un perfil QA e incluye las pruebas correspondientes.

Desde la planificación del filtrado hasta la creación de su PR, has utilizado la misma conversación y rama. Hemos reunido ese trabajo en una PR para simplificar el taller.

## Distintos tipos de verificación

Has comprobado el código con pruebas automatizadas, en el navegador y mediante la exploración de Copilot a través de MCP. La habilidad `quality-checks` ejecutó las pruebas unitarias, lint y las comprobaciones de tipos, e informó de los resultados con el nuevo formato. QA reunió esos resultados con la revisión de los requisitos y la cobertura antes de abrir la PR.

Las pruebas nuevas deben cubrir carencias reales; una ejecución de QA que no necesite añadir pruebas también puede ser correcta. Revisa el código y las pruebas antes de autorizar la combinación, y actualiza las pruebas de verificación afectadas después de cualquier cambio.

## Prácticas recomendadas

El contexto y las herramientas que proporcionas influyen en el trabajo de Copilot. En este taller, has actualizado instrucciones, personalizado una habilidad, creado un perfil QA y configurado un servidor MCP. Reutiliza estas personalizaciones entre conversaciones y adáptalas a las necesidades del equipo. Las instrucciones establecen estándares, las habilidades describen tareas repetibles, los agentes personalizados definen roles especializados y los servidores MCP conectan herramientas externas. Revisa los cambios y los resultados de las herramientas, no solo el resumen del agente.

Elige el **modo y el modelo** adecuados para la tarea. Utiliza **Plan** para definir un enfoque antes de construir, **Interactive** para seguir de cerca los cambios acotados y **Autopilot** para tareas bien delimitadas. Elige un modelo más rápido para ediciones rutinarias y uno más capaz para tareas complejas.

El contexto sigue siendo tan importante como la infraestructura. Explicar claramente qué quieres construir, por qué y cómo influye de forma significativa en el resultado.

## Más posibilidades

Has completado el flujo principal. También puedes explorar estas funciones de Copilot CLI:

- `/review` para pedir al agente de revisión de código que analice los cambios.
- `/rubber-duck` para conversar sobre un problema y obtener otra perspectiva.
- `/fleet` para coordinar tareas independientes en paralelo.
- `/worktree` para aislar otra tarea.
- `/delegate` para enviar una tarea al agente en la nube de Copilot.

## Pasos siguientes

La mejor forma de mejorar con una herramienta es seguir utilizándola, ya sea en código de producción, en proyectos personales o en una aplicación que tengas pendiente. Comparte lo que aprendas con el equipo y explora la documentación.

Si quieres ampliar Tailspin Toys desde el terminal, continúa con la serie opcional [Foundry Backer Concierge][foundry]. También puedes explorar el [taller de VS Code][vscode], el de la [aplicación GitHub Copilot][app] o el del [agente en la nube de Copilot][cloud].

## Recursos

- [Acerca de GitHub Copilot CLI][about-cli]
- [Referencia de comandos de Copilot CLI][cli-reference]
- [Personalizar Copilot CLI][customize-cli]
- [Gestionar solicitudes de incorporación de cambios con Copilot CLI][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
