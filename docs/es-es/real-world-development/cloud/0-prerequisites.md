---
title: "Ejercicio 0: Requisitos previos"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Antes de empezar los ejercicios del agente en la nube de Copilot, prepara el entorno. Crearás una copia propia del repositorio Tailspin Toys y un [Codespace][codespaces] para editar archivos de instrucciones y revisar el trabajo que produzca el agente en la nube.

## Configurar el repositorio del taller

Para obtener el código que usarás en los ejercicios, crea una instancia propia a partir de la [plantilla del repositorio][template-repository]. Incluirá todos los archivos necesarios para el taller y trabajarás en esa copia durante los ejercicios.

1. En una nueva ventana del navegador, abre la [plantilla de Tailspin Toys][tailspin-template].
2. Selecciona **Use this template** y, después, **Create a new repository**.

   ![Botón Use this template](../../../_images/ex0-use-template.png)

3. Si participas en un evento organizado por GitHub o Microsoft, sigue las instrucciones de los mentores. En caso contrario, crea el repositorio en una organización donde tengas acceso a GitHub Copilot.

   ![Configuración del repositorio a partir de una plantilla](../../../_images/ex0-repository-settings.png)

4. Anota la ruta del repositorio que has creado (**organization-or-user-name/repository-name**).

## Crear un Codespace

[GitHub Codespaces][codespaces] es un entorno de desarrollo en la nube que permite escribir, ejecutar y depurar código directamente en el navegador. Ofrece un entorno de desarrollo integrado (IDE) completo con compatibilidad con varios lenguajes de programación, extensiones y herramientas.

1. Abre el nuevo repositorio.
2. Selecciona el botón verde **Code**.

   ![Botón Code](../../../_images/ex0-code-button.png)

3. Selecciona la pestaña **Codespaces** y, después, el botón **+** para crear un Codespace.

   ![Crear un Codespace](../../../_images/ex0-create-codespace.png)
4. Deja el Codespace abierto en una pestaña del navegador. Volverás a él en un ejercicio posterior.

La creación del Codespace tardará varios minutos. Mientras esperas, puedes explorar otras funciones de GitHub Copilot que se presentan en los siguientes ejercicios.

> [!NOTE]
> El taller está preparado para ejecutarse en un Codespace o en un [contenedor de desarrollo local][dev-containers]. Ambos incluyen los requisitos previos necesarios. Si prefieres trabajar en local, abre el repositorio clonado en VS Code y selecciona **Reopen in Container** cuando aparezca la opción. VS Code creará el mismo contenedor de desarrollo que usa Codespaces.

## Resumen

Has creado una copia del repositorio y has iniciado la creación del Codespace que utilizarás junto al agente en la nube de Copilot.

## Paso siguiente

A continuación, añadirás las instrucciones personalizadas que seguirá el agente en la nube en el [ejercicio 1: Instrucciones personalizadas][next-lesson].

## Recursos

- [Información general sobre GitHub Codespaces][codespaces]
- [Crear un repositorio a partir de una plantilla][template-repository]
- [Inicio rápido de Codespaces][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[next-lesson]: ../1-custom-instructions/
