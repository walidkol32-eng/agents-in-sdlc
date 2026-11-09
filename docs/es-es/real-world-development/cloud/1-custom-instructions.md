---
title: "Ejercicio 1 - Instrucciones personalizadas (agente en la nube)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

El contexto es importante cuando trabajas con inteligencia artificial generativa. Si una tarea debe realizarse de una forma concreta o Copilot necesita información previa, los [archivos de instrucciones][instruction-files] proporcionan ese contexto: ayudan a Copilot a entender no solo qué debe hacer, sino también cómo.

En este ejercicio:

- explorarás cómo llegan a Copilot el contexto del proyecto, las directrices de código y los estándares de documentación mediante instrucciones del repositorio y de rutas específicas.
- añadirás un estándar nuevo para todo el repositorio en `.github/copilot-instructions.md`.

> [!NOTE]
> A diferencia de los recorridos de VS Code y CLI, aquí no compararás prompts antes y después. El agente en la nube trabaja de forma asíncrona en incidencias de GitHub, por lo que su efecto es más difícil de mostrar en tiempo real. Verás cómo influyen las instrucciones al revisar las solicitudes de incorporación de cambios (PR) que genere más adelante.

## Archivos de instrucciones

### Escenario

Como cualquier buen equipo de desarrollo, Tailspin Toys tiene directrices y requisitos para sus prácticas de desarrollo. Entre ellos se incluyen:

- La capa de datos siempre necesita pruebas unitarias.
- La interfaz debe tener un tema oscuro y un aspecto moderno.
- El código debe incluir comentarios de documentación TSDoc.
- Cada archivo debe empezar con un bloque de comentarios que describa su propósito.

Con los archivos de instrucciones, proporcionarás a Copilot la información necesaria para que realice las tareas de acuerdo con estas prácticas.

### Instrucciones personalizadas

Las instrucciones personalizadas proporcionan contexto y preferencias a Copilot para que entienda mejor el estilo y los requisitos de código. Puedes especificar convenciones, bibliotecas y los tipos de comentarios que quieres incluir. Las instrucciones pueden aplicarse a todo el repositorio o a determinados tipos de archivo y tareas.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md` es un único archivo de instrucciones que se envía a Copilot con cada solicitud del repositorio. Debe contener información general del proyecto que sea pertinente para la mayoría de las solicitudes de chat o de la interfaz de línea de comandos (CLI), como la pila tecnológica, una descripción de lo que se está creando, las prácticas recomendadas y la orientación general.
- `.github/instructions/*.instructions.md` contiene directrices para tipos de archivo o tareas concretos. Puedes crear instrucciones para lenguajes como TypeScript o Astro, o para tareas como crear un componente de interfaz o una batería de pruebas unitarias.

> [!NOTE]
> En el entorno de desarrollo integrado (IDE), los archivos de instrucciones solo se usan para generar código en Copilot Chat, no para completar código ni para las sugerencias de siguiente edición.
>
> Copilot Chat, Copilot CLI y el agente en la nube de Copilot usan las instrucciones del repositorio y los archivos `*.instructions.md` que tengan metadatos `applyTo`.
>
> Copilot también admite archivos de instrucciones en otros formatos, como AGENTS.md y CLAUDE.md [y otros estándares][custom-instructions-support].

### Prácticas recomendadas para administrar archivos de instrucciones

El proyecto de ejemplo muestra un enfoque representativo para crear archivos de instrucciones:

- Mantén `.github/copilot-instructions.md` centrado en la orientación general del proyecto, como su descripción, estructura y estándares globales.
- Usa `*.instructions.md` para instrucciones específicas de tipos de archivo, como pruebas unitarias, componentes de Astro o la capa de datos, o para tareas concretas.
- Escribe en lenguaje natural y presenta directrices claras con ejemplos de cómo debería ser el código y cómo no debería ser.

No hay una única forma de crear archivos de instrucciones ni de usar la inteligencia artificial. La experimentación te ayudará a encontrar el enfoque adecuado para tu proyecto.

> [!TIP]
> Todos los proyectos que usan GitHub Copilot deberían tener un conjunto sólido de archivos de instrucciones. Al explorar los archivos de este proyecto, puede que veas instrucciones para varias tareas, incluidas las [actualizaciones de la interfaz][ui-instructions] y [Astro][astro-instructions].
>
> Copilot también puede ayudarte a generar archivos de instrucciones. Cada entorno ofrece esta opción de una forma distinta; por ejemplo, **Configure Chat → Generate Agent Instructions** en VS Code o `/init` en Copilot CLI. La lección correspondiente explica cuándo usarla.
>
> Si buscas plantillas o un punto de partida, explora [Awesome Copilot][awesome-copilot], un repositorio con archivos de instrucciones, agentes personalizados y otros recursos.

## Explorar los archivos de instrucciones

Dedica un momento a revisar los archivos de instrucciones incluidos en el repositorio. Hay un archivo principal `copilot-instructions.md` y varios archivos `*.instructions.md` para distintas tareas.

1. Abre `.github/copilot-instructions.md`.
2. Explora el archivo y fíjate en la breve descripción del proyecto y en secciones como **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. En **Code standards**, revisa la guía anidada **GitHub Actions Workflows**. Estas instrucciones se aplican a cualquier interacción con Copilot.
3. Abre la carpeta `.github/instructions` y explora sus archivos para Astro, la capa de datos Drizzle, las pruebas y otros ámbitos.
4. Abre `.github/instructions/unit-tests.instructions.md` y fíjate en el campo `applyTo` de la parte superior. Este campo define un patrón relativo a la raíz del repositorio que determina a qué archivos se aplican las instrucciones. En este caso, coincide con cualquier archivo de pruebas TypeScript, por ejemplo, uno que cumpla el patrón `**/*.test.ts`.
5. Revisa las instrucciones específicas para crear pruebas unitarias en este proyecto.
6. Abre `.github/instructions/drizzle.instructions.md` y desplázate hasta el final. Fíjate en los enlaces a otros archivos de instrucciones, como `unit-tests.instructions.md`, y a archivos existentes del proyecto. Las rutas se expresan en relación con el archivo de instrucciones, no con la raíz del repositorio.

La sección **Code formatting requirements** del archivo `copilot-instructions.md` describe los estándares de código del proyecto, pero todavía no exige documentación dentro del código. En los pasos siguientes, añadirás reglas para los comentarios de documentación TSDoc y los encabezados de archivo.

## Añadir un estándar para el repositorio

El agente en la nube lee los archivos de instrucciones de la rama a la que apunta la incidencia. Antes de editar, prepara una rama para trabajar. Aunque el agente leerá las instrucciones de la rama de destino de la incidencia, combinarás tus cambios para que el agente pueda usarlos más adelante.

1. En el terminal de tu Codespace, crea una rama:

   ```bash
   git checkout -b custom-instructions-cloud
   ```

1. Vuelve a abrir `.github/copilot-instructions.md`.
2. Localiza la sección **Code formatting requirements**, que debería estar cerca de la línea 27. Revisa los estándares de código documentados: aún no incluyen comentarios de documentación, por lo que el asistente generado no tenía comentarios TSDoc.
3. Añade estas líneas debajo de los estándares existentes:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Guarda `copilot-instructions.md`.

> [!TIP]
> Las instrucciones del repositorio son adecuadas para estándares de todo el proyecto, como la regla de comentarios de documentación. Los archivos `*.instructions.md` sirven para ámbitos más específicos.

## Confirmar, subir y combinar la actualización

Confirma la actualización y combínala en `main`, la rama que el agente en la nube utilizará al abordar las incidencias posteriores.

1. Prepara y confirma el archivo:

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. Sube la rama:

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. En GitHub.com, abre una solicitud de incorporación de cambios desde `custom-instructions-cloud` a `main` y combínala.

> [!TIP]
> También puedes trabajar directamente en `main` durante este taller. El paso de la rama reproduce el flujo habitual de un proyecto.

## Resumen y pasos siguientes

Has explorado cómo Copilot obtiene el contexto de los archivos de instrucciones y has añadido un estándar para todo el repositorio a `.github/copilot-instructions.md`. Verás cómo influye en las solicitudes de incorporación de cambios que genere el agente en la nube.

A continuación, [asignarás tu primera incidencia al agente en la nube de Copilot][next-lesson].

## Recursos

- [Instrucciones personalizadas para GitHub Copilot][instruction-files]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Cinco consejos para escribir mejores instrucciones personalizadas][copilot-instructions-five-tips]
- [Awesome Copilot: instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
[awesome-copilot]: https://github.com/github/awesome-copilot
[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
