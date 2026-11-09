---
title: "Lección 5 - Personalizar y utilizar una habilidad quality-checks"
description: "Explora la habilidad quality-checks, personaliza el formato del informe y úsala para comprobar el filtrado."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Escribir código implica más que implementarlo. Ya has comprobado el código manualmente y has utilizado archivos de instrucciones para aplicar los estándares. Las habilidades de agente sirven para tareas repetibles como ejecutar pruebas, lint y otras comprobaciones de integración continua (CI).

En esta lección:

- explorarás la habilidad existente `quality-checks`.
- personalizarás el formato de los resultados.
- volverás a cargar la habilidad y la ejecutarás.

## Escenario

Tailspin Toys utiliza la habilidad `quality-checks` para ejecutar pruebas unitarias, lint y comprobaciones de tipos. El equipo quiere mejorar el informe para que los resultados sean más fáciles de leer.

## Instrucciones, scripts y recursos

Las habilidades de agente agrupan instrucciones reutilizables, scripts ejecutables y recursos auxiliares que el agente carga cuando los necesita. Una habilidad se guarda en una carpeta con un archivo Markdown `SKILL.md`, cuyo frontmatter define el nombre y la descripción. La carpeta también puede incluir scripts y otros recursos.

> [!NOTE]
> Una habilidad no necesita archivos ni carpetas adicionales. La habilidad `quality-checks` de Tailspin Toys solo contiene `SKILL.md` porque utiliza los comandos existentes del proyecto.

Las habilidades pueden estar en `.github/skills` para compartirlas en el repositorio o en la carpeta de habilidades del usuario, `~/.copilot/skills`.

## Explorar la habilidad

Revisa la habilidad del equipo antes de ejecutarla o modificar su informe.

1. Vuelve a tu Codespace y abre `.github/skills/quality-checks/SKILL.md` en el editor de Codespaces.
2. Lee el `name` y la `description` de la parte superior. La descripción ayuda a Copilot a decidir cuándo utilizar la habilidad.
3. Revisa cómo las instrucciones guían las pruebas y el lint.
4. Comprueba que aún no contiene una sección **Results output formatting**.

## Ejecutar la habilidad antes de cambiarla

Pide a Copilot que ejecute las tres comprobaciones y toma nota del informe actual.

1. Vuelve a la conversación de filtrado en modo Interactive.
2. Envía este prompt:

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. Revisa el informe final.

## Personalizar el informe

Añade instrucciones para que el informe indique qué se ejecutó y qué resultados se obtuvieron.

1. Vuelve a `.github/skills/quality-checks/SKILL.md`.
2. Añade esta sección al final del archivo:

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. Guarda el archivo.

## Ejecutar la habilidad actualizada

Copilot CLI puede volver a cargar las habilidades editadas sin reiniciar la conversación. Vuelve a cargarla y repite el mismo prompt para comparar los informes.

1. Introduce:

   ```plaintext
   /skills reload
   ```

2. Envía de nuevo el mismo prompt:

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. Revisa el informe y compáralo con el primero.

## Resumen y pasos siguientes

Has personalizado y utilizado una habilidad de agente existente. En esta lección:

- has explorado la habilidad `quality-checks` para las pruebas unitarias, lint y comprobaciones de tipos.
- has personalizado el formato de los resultados.
- has vuelto a cargar y ejecutado la habilidad.

El cambio acompañará al filtrado en la solicitud de incorporación de cambios. A continuación, permitirás que Copilot interactúe con el sitio mediante [el servidor MCP de Playwright][next-lesson].

## Más ejemplos de habilidades

Estos ejemplos de la comunidad son referencias, no tareas adicionales:

- [Especificación de Agent Skills][skill-spec]
- [Flujo de contribución: `make-repo-contribution`][contribution-example]
- [Documentos de requisitos: `prd`][prd-example]
- [Diagramas y script de exportación: `drawio`][drawio-example]
- [Pruebas de navegador: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
