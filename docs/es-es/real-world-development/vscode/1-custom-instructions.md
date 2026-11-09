---
title: "Ejercicio 1 - Instrucciones personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Requisitos previos][previous-lesson] · [Siguiente lección: Modo agente →][next-lesson] |

El contexto es importante cuando trabajas con inteligencia artificial generativa. Los archivos de instrucciones explican a Copilot no solo qué código quieres, sino también cómo debe estructurarse.

En este ejercicio:

- explorarás cómo llegan a Copilot el contexto del proyecto, las directrices de código y los estándares de documentación.
- generarás la primera parte de los datos para el filtrado con las instrucciones actuales.
- añadirás un estándar de todo el repositorio a `.github/copilot-instructions.md`.
- repetirás el prompt y observarás cómo cambia el código generado.
- confirmarás los cambios en `main` para utilizarlos en el ejercicio siguiente.

> [!CAUTION]
> El código generado puede no seguir todos los estándares. Copilot no es determinista; observa la tendencia después de actualizar las instrucciones, no una salida idéntica a un ejemplo.

## Archivos de instrucciones

### Escenario

Como cualquier buen equipo de desarrollo, Tailspin Toys tiene directrices y requisitos para sus prácticas de desarrollo. Entre ellos se incluyen:

- La capa de datos siempre necesita pruebas unitarias.
- La interfaz debe tener un tema oscuro y un aspecto moderno.
- El código debe incluir comentarios de documentación de TypeScript (TSDoc).
- Cada archivo debe empezar con un bloque de comentarios que describa su propósito.

Con los archivos de instrucciones, proporcionarás a Copilot el contexto necesario para que realice las tareas de acuerdo con estas prácticas.

### Instrucciones personalizadas

Las instrucciones personalizadas proporcionan contexto y preferencias a Copilot para que entienda mejor el estilo y los requisitos de código. Puedes especificar convenciones, bibliotecas y los tipos de comentarios que quieres incluir. Las instrucciones pueden aplicarse a todo el repositorio o a determinados tipos de archivo y tareas.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md` es un único archivo que se envía a Copilot con cada solicitud del repositorio. Debe contener información general del proyecto pertinente para la mayoría de las solicitudes de chat o de la interfaz de línea de comandos (CLI), como la pila tecnológica, una descripción de lo que se está creando, las prácticas recomendadas y la orientación general.
- `.github/instructions/*.instructions.md` contiene directrices para tipos de archivo o tareas concretos. Puedes crear instrucciones para lenguajes como TypeScript o Astro, o para tareas como crear un componente de interfaz o una batería de pruebas unitarias.

> [!NOTE]
> En el entorno de desarrollo integrado (IDE), los archivos de instrucciones solo se usan para generar código en Copilot Chat, no para completar código ni para las sugerencias de siguiente edición.
>
> Copilot Chat, Copilot CLI y el agente en la nube de Copilot usan las instrucciones del repositorio y los archivos `*.instructions.md` que tengan metadatos `applyTo` cuando generan código.
>
> Copilot también admite archivos de instrucciones en otros formatos, como AGENTS.md y CLAUDE.md.

### Prácticas recomendadas para administrar archivos de instrucciones

El proyecto de ejemplo muestra un enfoque representativo para crear archivos de instrucciones:

- Mantén `copilot-instructions.md` centrado en la orientación general del proyecto, como su descripción, estructura y estándares globales.
- Usa `*.instructions.md` para instrucciones específicas de tipos de archivo, como pruebas unitarias, componentes de Astro o la capa de datos, o para tareas concretas.
- Escribe en lenguaje natural y presenta directrices claras con ejemplos de cómo debería ser el código y cómo no debería ser.

No hay una única forma de crear archivos de instrucciones ni de usar la inteligencia artificial. La experimentación te ayudará a encontrar el enfoque adecuado para tu proyecto.

> [!TIP]
> Todos los proyectos que usan GitHub Copilot deberían tener un conjunto sólido de archivos de instrucciones. Al explorar los archivos de este proyecto, puede que veas instrucciones para varias tareas, incluidas las [actualizaciones de la interfaz][ui-instructions] y [Astro][astro-instructions].
>
> Copilot también puede ayudarte a generar archivos de instrucciones. Cada entorno ofrece esta opción de una forma distinta; por ejemplo, **Configure Chat → Generate Agent Instructions** en VS Code o `/init` en Copilot CLI. La lección correspondiente explica cuándo usarla.
>
> Si buscas plantillas o un punto de partida, explora [Awesome Copilot][awesome-copilot], un repositorio con archivos de instrucciones, agentes personalizados y otros recursos.

## Explorar las instrucciones existentes

Revisa los archivos que se incluyen en el repositorio para entender el contexto disponible para Copilot.

1. Abre `.github/copilot-instructions.md`.
2. Explora el archivo y fíjate en la breve descripción del proyecto y en secciones como **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. En **Code standards**, revisa la guía anidada **GitHub Actions Workflows**. Estas instrucciones se aplican a cualquier interacción con Copilot.
3. Abre la carpeta `.github/instructions` y explora sus archivos para Astro, la capa de datos Drizzle, las pruebas y otros ámbitos.
4. Abre `.github/instructions/unit-tests.instructions.md` y fíjate en el campo `applyTo` de la parte superior. Este campo define un patrón relativo a la raíz del repositorio que determina a qué archivos se aplican las instrucciones. En este caso, coincide con cualquier archivo de pruebas TypeScript, por ejemplo, uno que cumpla el patrón `**/*.test.ts`.
5. Revisa las instrucciones específicas para crear pruebas unitarias en este proyecto.
6. Abre `.github/instructions/drizzle.instructions.md` y desplázate hasta el final. Fíjate en los enlaces a otros archivos de instrucciones, como `unit-tests.instructions.md`, y a archivos existentes del proyecto. Las rutas se expresan en relación con el archivo de instrucciones, no con la raíz del repositorio.

La sección **Code formatting requirements** del archivo `copilot-instructions.md` describe los estándares del proyecto, pero todavía no exige documentación dentro del código. En los pasos siguientes, añadirás reglas para los comentarios TSDoc y los encabezados de archivo.

## Crear una rama

Separa el trabajo de este ejercicio en una rama nueva.

1. Vuelve a tu Codespace y abre un terminal con <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
2. Crea y cambia a la rama:

   ```bash
   git checkout -b custom-instructions
   ```

## Usar Copilot Chat antes de cambiar las instrucciones

> [!TIP]
> Antes de empezar, vuelve a tu Codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación limpia. El modo y el modelo varían según el ejercicio; se indicarán cuando sea necesario.

Para observar el efecto de las instrucciones personalizadas, empieza enviando un prompt con las instrucciones actuales. Más adelante, actualizarás el archivo y volverás a enviar el mismo prompt para comparar los resultados.

1. Abre Copilot Chat y selecciona **New Chat**.
2. Cierra las pestañas abiertas de ejercicios anteriores.
3. Abre `src/lib/publishers.ts` para indicar dónde debe ubicarse el ayudante.
4. Selecciona **Agent** en el selector de agentes.

   ![Selector de agente en la vista Chat](../../../_images/shared-chat-mode-selector.png)

5. Envía este prompt:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

6. Observa cómo Copilot explora el proyecto y aplica cambios, a menudo tanto al archivo del ayudante como a sus pruebas.
7. Revisa los cambios propuestos. El ayudante debería ser una función tipada que reciba un cliente `db` como primer argumento y devuelva una matriz tipada de editoriales. Esto procede de las convenciones de la capa de datos definidas en `.github/instructions/drizzle.instructions.md`, que se aplica a `src/lib/*.ts`.
8. Comprueba que faltan los comentarios TSDoc y el encabezado de comentario del archivo.

> [!CAUTION]
> Copilot puede añadir comentarios aunque no se le soliciten. El objetivo es observar la mejora de coherencia tras actualizar las instrucciones.

## Añadir un estándar para el repositorio

Añade las directrices de documentación al archivo principal de instrucciones.

Como ya se ha indicado, `.github/copilot-instructions.md` proporciona información general del proyecto a Copilot. Añade un estándar de documentación al código para mejorar las sugerencias.

1. Vuelve a abrir `.github/copilot-instructions.md`.
2. Localiza la sección **Code formatting requirements**, que debería estar cerca de la línea 27. Revisa los estándares de código documentados: aún no incluyen comentarios de documentación, por lo que el ayudante generado no tenía comentarios TSDoc.
3. Añade estas líneas debajo de los estándares existentes para pedir a Copilot que añada encabezados de comentario a los archivos y comentarios TSDoc:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Guarda `copilot-instructions.md`.

> [!TIP]
> Como has visto en la lección anterior, las instrucciones del proyecto se pueden definir en `.github/copilot-instructions.md` para todo el repositorio o en archivos `*.instructions.md` para lenguajes, tipos de archivo o tareas concretas. El archivo principal es el lugar adecuado para estándares globales, como la regla sobre los comentarios de documentación.

## Repetir el prompt y observar el cambio

Repite el mismo prompt en una conversación limpia y compara el resultado con el primer intento.

1. En Copilot Chat, selecciona **New Chat**.
2. Abre `src/lib/publishers.ts`.
3. Envía exactamente el mismo prompt utilizado antes:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. Comprueba que el archivo propuesto ahora empieza con un bloque de comentarios parecido a este:

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. Comprueba que la función propuesta incluye un comentario TSDoc parecido a este:

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

Has orientado a Copilot para que siga un estándar nuevo y lo aplique a código real que servirá de base para el ejercicio siguiente.

## Confirmar, subir y combinar la actualización

Confirma las instrucciones y el ayudante para que el siguiente ejercicio pueda aprovechar ese trabajo.

> [!NOTE]
> Este es el primer ejercicio en el que Copilot actuará en GitHub. La plantilla del proyecto ya conecta las herramientas de GitHub con tu espacio de trabajo, así que no tienes que configurar nada. La primera vez que Copilot utilice una herramienta, VS Code puede pedirte que inicies sesión en GitHub; sigue las indicaciones para permitirlo. En un ejercicio posterior aprenderás cómo funciona esta conexión mediante Model Context Protocol (MCP).

1. Abre un terminal con <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
2. Ejecuta `git status` para comprobar los cambios.
3. Prepara y confirma las instrucciones y los cambios del ayudante:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. Sube la rama:

   ```bash
   git push -u origin custom-instructions
   ```

5. Abre Copilot Chat en el Codespace.
6. Pulsa <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) para abrir la vista Copilot Chat y asegúrate de que **Agent** esté seleccionado.
7. Pide a Copilot que encuentre la incidencia relacionada con la actualización de los archivos de instrucciones y cree una solicitud de incorporación de cambios (PR) desde la rama actual. Pídele que indique que la PR cierra esa incidencia e incluya que también se ha añadido la base del ayudante de editoriales para el próximo trabajo de filtrado:

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

8. Copilot buscará la incidencia y creará la PR.
9. Cuando Copilot te pida permiso para realizar acciones de GitHub, revisa el comando y selecciona **Allow** según corresponda.
10. Cuando se cree la PR, pide a Copilot que la combine en `main` y devuelva la rama local a `main` con los últimos cambios:

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

11. Cuando Copilot te pida permiso para realizar acciones de GitHub y ejecutar comandos de shell, revisa el comando y selecciona **Allow** según corresponda.

Ya has creado y combinado una PR con la ayuda de GitHub Copilot.

## Resumen y pasos siguientes

Has explorado cómo Copilot utiliza los archivos de instrucciones y has aplicado un estándar nuevo al código. En este ejercicio:

- has enviado un prompt de generación de código y observado el resultado con las instrucciones existentes.
- has añadido un estándar para todo el repositorio a `.github/copilot-instructions.md`.
- has repetido el prompt y observado el efecto de las instrucciones actualizadas.
- has confirmado los cambios en `main` para que el siguiente ejercicio pueda utilizarlos.

A continuación, aplicarás las instrucciones en [modo agente][next-lesson] para añadir una funcionalidad en el proyecto.

## Recursos

- [Instrucciones personalizadas para GitHub Copilot][instruction-files]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Cinco consejos para escribir mejores instrucciones personalizadas][copilot-instructions-five-tips]
- [Instrucciones personales para GitHub Copilot][personal-instructions]
- [Awesome Copilot: instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
[awesome-copilot]: https://github.com/github/awesome-copilot
[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
