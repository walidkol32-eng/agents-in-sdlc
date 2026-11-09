---
title: "Lección 4 - Guiar a Copilot con instrucciones personalizadas"
description: "Explora las instrucciones del repositorio, añade un estándar de documentación y aplícalo al código de filtrado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

El contexto es importante cuando trabajas con inteligencia artificial generativa. Los archivos de instrucciones explican a Copilot no solo qué código quieres, sino también cómo debe estructurarse. Ahora que ya has creado el filtrado, explorarás las instrucciones utilizadas por Copilot, añadirás un estándar de documentación y lo aplicarás al código.

En esta lección:

- explorarás cómo llegan al agente las instrucciones del repositorio y las instrucciones específicas de rutas.
- actualizarás el archivo de instrucciones para que se respeten los estándares de código.
- observarás el efecto de los archivos de instrucciones en el código.

## Escenario

Tailspin Toys tiene directrices para las prácticas de desarrollo. Entre ellas, los comentarios deben explicar la intención y las decisiones que no sean evidentes; las funciones exportadas de `db/` y `src/lib/` deben documentar su propósito, parámetros y valores devueltos mediante TSDoc/JSDoc; los componentes Astro reutilizables deben documentar sus contratos `Props`; y deben respetarse las indicaciones existentes de formato y lint.

## Archivos de instrucciones

Los archivos de instrucciones aportan contexto y preferencias a Copilot. Puedes mantener instrucciones para todo el repositorio o para tipos de archivos y tareas concretos.

- `.github/copilot-instructions.md` contiene instrucciones del proyecto que Copilot recibe para cada solicitud.
- `.github/instructions/*.instructions.md` contiene directrices para determinados lenguajes, tipos de archivo o tareas.

> [!NOTE]
> Los formatos de instrucciones compatibles varían según el entorno. Consulta la [referencia de compatibilidad de instrucciones personalizadas][custom-instructions-support] antes de depender de un formato concreto.

## Explorar las instrucciones existentes

Revisa los archivos incluidos en el proyecto para entender qué orientación recibe Copilot antes de modificarlos.

1. Vuelve a tu Codespace.
2. En el editor de Codespaces, abre `.github/copilot-instructions.md`.
3. Revisa la descripción breve del proyecto y las directrices de programación.
4. Explora los archivos de `.github/instructions`, incluidos los de Astro, la capa de datos Drizzle y las pruebas.
5. Abre `.github/instructions/unit-tests.instructions.md` y fíjate en el campo `applyTo`, que determina los archivos a los que se aplican las instrucciones.
6. Abre `.github/instructions/drizzle.instructions.md` y revisa sus enlaces a otros archivos de instrucciones y ejemplos del proyecto.

## Actualizar las instrucciones

Añade al archivo principal un estándar para documentar el nuevo código TypeScript.

1. En `.github/copilot-instructions.md`, localiza la sección **Code formatting guidance**.
2. Añade este elemento al final de la sección:

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

3. Guarda los cambios.

## Aplicar las instrucciones actualizadas

Copilot CLI carga las instrucciones del repositorio cuando comienza una conversación. Cierra y reanuda la conversación de filtrado para que Copilot las lea sin perder el contexto de la funcionalidad.

1. Si la conversación de filtrado sigue abierta, introduce `/exit`.
2. Desde la raíz del repositorio en la rama `game-filters-cli`, reanuda la conversación:

   ```bash
   copilot --resume
   ```

3. Si se te solicita, selecciona la conversación de filtrado y confirma que está activa la rama `game-filters-cli`.
4. Pide a Copilot que aplique las instrucciones actualizadas:

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

5. Introduce `/diff` y revisa los archivos TypeScript modificados. Confirma que los comentarios TSDoc describen el código con precisión.

## Resumen y pasos siguientes

Has explorado cómo Copilot CLI obtiene el contexto de los archivos de instrucciones y has aplicado un nuevo estándar a la funcionalidad. En esta lección:

- has explorado cómo llegan al agente las instrucciones del repositorio y las instrucciones específicas de rutas.
- has actualizado el archivo de instrucciones para que se respeten los estándares de código.
- has observado el efecto de los archivos de instrucciones en el código.

A continuación, [personalizarás y ejecutarás la habilidad reutilizable `quality-checks`][next-lesson] para ejecutar las pruebas y el lint de forma coherente.

## Recursos

- [Añadir instrucciones personalizadas para Copilot CLI][instruction-files]
- [Compatibilidad con instrucciones personalizadas][custom-instructions-support]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Awesome Copilot: colección de instrucciones y otros recursos][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
