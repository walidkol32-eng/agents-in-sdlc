---
title: "Lección 9 - Comandos de barra en GitHub Copilot CLI"
description: "Explora comandos de barra para gestionar el contexto, elegir modelos, compartir sesiones y delegar tareas opcionalmente."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

GitHub Copilot CLI incluye comandos de barra para acceder a funciones avanzadas, consultar información y cambiar la configuración. Ya has utilizado `/diff`, `/mcp`, `/skills`, `/agent` y `/pr`. Explorarás otros comandos útiles.

En esta lección:

- utilizarás `/context` y `/compact` para explorar cómo gestiona Copilot el contexto de la conversación.
- utilizarás `/model` para explorar los modelos disponibles.
- aprenderás cómo `/share` puede exportar o compartir una sesión.
- explorarás comandos opcionales para trabajar en paralelo, usar árboles de trabajo y delegar en el agente en la nube.

## Escenario

Has terminado el flujo principal de la CLI. Ahora revisarás otras posibilidades: administrar el contexto, cambiar de modelo, compartir sesiones y delegar tareas opcionalmente al [agente en la nube de Copilot][about-cloud-agent].

## Explorar el contexto de Copilot CLI

Copilot CLI compacta automáticamente la conversación cuando se llena la ventana de contexto. Puedes consultar el uso y compactarla manualmente.

1. Vuelve a tu Codespace e inicia Copilot CLI desde la raíz del repositorio si aún no está abierto.
2. Introduce:

   ```plaintext
   /context
   ```

3. Observa el modelo, el uso actual de tokens y la distribución del contexto entre instrucciones del sistema, herramientas, mensajes y espacio libre.
4. Compacta la conversación:

   ```plaintext
   /compact
   ```

5. Vuelve a introducir `/context` y compara los resultados. Si la conversación ya es pequeña, puede que apenas haya cambios.

> [!NOTE]
> Copilot CLI compacta el contexto automáticamente a medida que se llena la ventana. Usa `/compact` para elegir cuándo hacerlo. Usa `/clear` o `/new` al pasar a una tarea no relacionada y querer una conversación nueva.

## Elegir un modelo

Los modelos tienen distintas capacidades y preferencias, y Copilot CLI permite consultar y seleccionar el que quieras utilizar.

1. Introduce:

   ```plaintext
   /model
   ```

2. Explora los modelos disponibles y la información de uso.
3. Mantén el modelo actual, selecciona otro o pulsa <kbd>Esc</kbd> para cerrar la lista.

## Compartir una sesión

El comando `/share` puede exportar una sesión a un archivo Markdown o HTML, crear un enlace para compartir o publicar un gist de GitHub. Compartir la sesión es opcional.

1. Introduce `/help` y revisa las opciones de `/share` disponibles en tu versión.
2. Si quieres compartir la sesión, elige el destino adecuado, como `/share file` para exportar un archivo Markdown local.
3. Revisa el contenido exportado antes de enviarlo o publicarlo. Las exportaciones pueden incluir prompts, respuestas y detalles del proyecto.

No publiques contenido del repositorio o de la conversación que el equipo no tenga previsto compartir.

## Opcional: ampliar o delegar el trabajo

El flujo principal del taller ha terminado. Copilot CLI también ofrece comandos para tareas mayores:

- `/fleet` puede dividir tareas independientes entre subagentes y ejecutarlas en paralelo.
- `/worktree` puede crear un árbol de trabajo Git aislado para otra tarea.
- `/delegate` puede enviar una tarea al agente en la nube de Copilot, que trabaja de forma asíncrona y puede abrir una PR.

Estos comandos son opcionales porque pueden crear árboles de trabajo o trabajo remoto. Empieza con una tarea nueva y bien definida y revisa los resultados mediante el flujo habitual. Para profundizar en el trabajo asíncrono, continúa con el [taller del agente en la nube][cloud-workshop].

## Resumen y pasos siguientes

Has utilizado comandos de barra para configurar Copilot CLI, compartir sesiones y consultar información interna. En esta lección:

- has utilizado `/context` y `/compact` para explorar la gestión del contexto.
- has utilizado `/model` para explorar los modelos disponibles.
- has aprendido cómo `/share` puede exportar o compartir sesiones.
- has explorado comandos opcionales para el trabajo en paralelo, los árboles de trabajo y la delegación al agente en la nube.

Hay más comandos de barra que puedes explorar. Para cerrar el recorrido, [repasarás lo aprendido y los pasos siguientes][next-lesson].

## Recursos

- [Referencia de comandos de Copilot CLI][cli-reference]
- [Gestión del contexto en Copilot CLI][context-management]
- [Acerca del agente en la nube de Copilot][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
