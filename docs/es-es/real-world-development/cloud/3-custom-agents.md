---
title: "Ejercicio 3 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## ¿Qué son los agentes personalizados?

Los [agentes personalizados][custom-agents-concept] de GitHub Copilot permiten crear asistentes de inteligencia artificial especializados en tareas o ámbitos concretos del flujo de desarrollo. Se definen mediante archivos Markdown en la carpeta `.github/agents` del repositorio y proporcionan a Copilot instrucciones específicas, prácticas recomendadas, patrones de código y conocimientos del dominio para que realice determinados tipos de trabajo de forma más eficaz. Los equipos pueden plasmar su experiencia en agentes reutilizables: por ejemplo, un agente de accesibilidad que compruebe el cumplimiento de las pautas de accesibilidad para el contenido web ([WCAG][wcag]), uno de seguridad que siga prácticas de programación segura o uno de pruebas que mantenga patrones coherentes.

Los agentes personalizados se definen en archivos Markdown dentro de `.github/agents` en el proyecto o de forma global en `~/.copilot/agents`. Cada archivo tiene frontmatter YAML con, como mínimo, los campos `name` y `description`, seguido de una indicación en Markdown que define el comportamiento, la experiencia y las instrucciones del agente.

### Comparar agentes personalizados y habilidades de agente

Los agentes personalizados y las [habilidades de agente][agent-skills-concept] se solapan en algunos aspectos: ambos se definen principalmente mediante archivos Markdown e indican a una inteligencia artificial cómo realizar determinadas operaciones. La forma más sencilla de distinguirlos es pensar que el **agente personalizado** es quien realiza el trabajo y las **habilidades** son las herramientas.

Los agentes personalizados tienen su propia ventana de contexto y están diseñados para coordinar habilidades e incluso otros agentes. En este ejercicio, el agente de accesibilidad revisará y actualizará el sitio según las pautas de accesibilidad. Como parte de ese trabajo, podría utilizar habilidades como una para gestionar el flujo de solicitudes de incorporación de cambios o una que ejecute y administre pruebas.

> [!NOTE]
> No hay una única forma correcta de crear un agente personalizado. Pruébalo e itera para adaptarlo al entorno y a las tareas.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

En este ejercicio:

- explorarás cómo se definen los agentes personalizados.
- asignarás una tarea a un agente personalizado.

## Escenario

Tailspin Toys se compromete a que todas las personas puedan utilizar su plataforma de financiación colectiva, independientemente de sus capacidades o preferencias visuales. Algunos comentarios recientes indican que a ciertas personas les cuesta leer el tema oscuro porque el contraste entre el texto y el fondo es insuficiente. Para resolver este problema de accesibilidad, el equipo de diseño ha solicitado un modo de alto contraste que se pueda activar y desactivar. Como la accesibilidad es fundamental, quieres implementar esta funcionalidad cuanto antes y utilizarás un agente personalizado para generarla.

## Revisar el agente de accesibilidad

El repositorio incluye un agente personalizado de accesibilidad. Revisa sus instrucciones antes de asignarle una tarea.

1. Abre `.github/agents/accessibility.md`.
2. Revisa los campos `name` y `description` del frontmatter YAML.
3. Examina sus responsabilidades, prácticas recomendadas, ejemplos de lenguaje de marcado de hipertexto (HTML), hojas de estilo en cascada (CSS) y JavaScript, y errores frecuentes.

> [!CAUTION]
> El frontmatter con `name` y `description` es obligatorio para los agentes personalizados.

## Crear y asignar una incidencia

El panel de agentes es el lugar central para trabajar con todos los agentes del entorno. Permite asignar tareas al agente en la nube, supervisar su progreso y redirigirlo o proporcionarle indicaciones adicionales. Asigna al agente de accesibilidad una incidencia para implementar el modo de alto contraste.

1. Abre el repositorio en GitHub.com y selecciona **Issues**.
2. Selecciona **New issue** y, después, **Blank issue**.
3. En **Title**, escribe `Add high contrast mode to website`.
4. En **Description**, introduce:

   ```plaintext
   We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
   ```

5. Selecciona **Create** y, en el panel derecho, selecciona **Assign to Copilot**.
6. Selecciona **Accessibility agent** en la lista de agentes personalizados.

   ![Asignación del agente en la nube con el agente personalizado Accessibility seleccionado](../../../_images/ex5-select-custom-agent.png)

7. Selecciona **Assign**.

Copilot comenzará a trabajar en segundo plano.

## Resumen y pasos siguientes

Has explorado los agentes personalizados de GitHub Copilot, asistentes especializados en tareas y ámbitos concretos. En este ejercicio:

- has explorado cómo se definen los agentes personalizados.
- has asignado una tarea a un agente personalizado.

Mientras Copilot implementa el modo de alto contraste, [supervisarás y dirigirás la sesión del agente][next-lesson] desde el panel de agentes.

## Recursos

- [Acerca de los agentes personalizados][custom-agents]
- [Preparar la organización para utilizar agentes personalizados][org-custom-agents]
- [Preparar la empresa para utilizar agentes personalizados][enterprise-custom-agents]

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents

---

| [← Lección anterior: Agente en la nube de GitHub Copilot][previous-lesson] | [Siguiente lección: Supervisar y administrar agentes →][next-lesson] |
|:--|--:|
