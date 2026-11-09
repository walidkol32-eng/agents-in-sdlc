---
title: "Lección 7 - Crear y utilizar un agente QA"
description: "Crea un agente personalizado QA que combine los requisitos de la incidencia, la habilidad quality-checks y Playwright MCP."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Has utilizado la habilidad `quality-checks` para ejecutar comprobaciones y Playwright MCP para observar el filtrado en un navegador. Ahora combinarás esas capacidades en un agente personalizado con un proceso de control de calidad (QA) definido.

En esta lección:

- explorarás cómo un agente personalizado utiliza instrucciones, habilidades y herramientas MCP.
- crearás y revisarás un perfil reutilizable de calidad.
- seleccionarás el agente QA y revisarás sus conclusiones respecto a la incidencia.

## Escenario

Antes de abrir una solicitud de incorporación de cambios (PR), Tailspin Toys quiere revisar de forma coherente los requisitos, la calidad del código, las comprobaciones automatizadas, la cobertura de pruebas y el comportamiento en el navegador.

## ¿Qué es un agente personalizado?

Un agente personalizado es una versión especializada de Copilot definida en un perfil Markdown. El perfil describe su propósito, instrucciones y herramientas disponibles. En este taller crearás el rol QA en `.github/agents/qa.agent.md` y lo seleccionarás en Copilot CLI.

Las instrucciones del repositorio describen los estándares del equipo; la habilidad `quality-checks` reúne comprobaciones repetibles; Playwright MCP proporciona herramientas de navegador; y el perfil QA indica a Copilot cómo utilizar esas capacidades. El perfil no reemplaza las otras personalizaciones ni requiere otra conversación.

## Crear el perfil QA

Pide a Copilot que cree un perfil reutilizable que defina las comprobaciones y sus límites.

1. Vuelve a tu Codespace y confirma que la conversación de filtrado está en modo Interactive.
2. Pide al agente predeterminado que cree el perfil:

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## Revisar el perfil

Inspecciona el perfil antes de utilizarlo para confirmar que incluye el proceso y los límites previstos.

1. Introduce `/diff` y abre `.github/agents/qa.agent.md`.
2. Lee el frontmatter. `description` es obligatorio; `name` es opcional, pero proporciona un nombre visible al agente.
3. Confirma que el perfil parte de los requisitos, sigue las instrucciones del repositorio, ejecuta `quality-checks` y utiliza Playwright MCP.
4. Confirma que el informe incluye pruebas, que el agente pide permiso antes de cambiar el código de implementación y que no confirma cambios ni abre PR.
5. Si falta alguna responsabilidad o límite, pide al agente predeterminado que revise el perfil antes de continuar.

## Ejecutar QA respecto a la incidencia

Copilot CLI carga los agentes del proyecto al iniciar una conversación. Reanuda la conversación de filtrado para conservar el contexto de la incidencia y el plan.

1. Activa el agente con:

   ```plaintext
   /agent QA
   ```

2. Si el agente aún no aparece en la lista, introduce el comando igualmente para activarlo.
3. Pídele que revise la funcionalidad:

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

4. Lee el informe cuando termine.

## Resumen y pasos siguientes

Has añadido un rol especializado reutilizable y has revisado su trabajo. En esta lección:

- has explorado cómo un agente personalizado utiliza instrucciones, habilidades y herramientas MCP.
- has creado y revisado un perfil reutilizable de control de calidad.
- has seleccionado el agente QA y revisado sus conclusiones respecto a la incidencia.

La implementación, la actualización de la habilidad, el perfil QA, las pruebas y el informe de verificación están listos para revisión. A continuación, [los reunirás en una PR de funcionalidad y utilizarás Agent Merge][next-lesson].

## Recursos

- [Crear agentes personalizados para Copilot CLI][create-agents]
- [Configuración de agentes personalizados][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
