---
title: "Ejercicio 0 - Requisitos previos"
description: "Crea tu copia de Tailspin Toys y prepara un Codespace para el taller de Copilot CLI."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Antes de empezar los ejercicios de Copilot CLI, prepara una copia del repositorio Tailspin Toys y crea un [Codespace][codespaces]. En el siguiente ejercicio utilizarás su terminal integrado para instalar y ejecutar Copilot CLI.

En este ejercicio:

- crearás tu copia del proyecto Tailspin Toys a partir de la plantilla.
- crearás un Codespace y confirmarás que el proyecto está listo.

## Configurar el repositorio del taller

Crea tu copia del proyecto Tailspin Toys a partir del [repositorio de plantilla][tailspin-template]. Contendrá todos los archivos necesarios para el taller.

1. En una nueva ventana del navegador, abre la [plantilla de Tailspin Toys][tailspin-template].
2. Selecciona **Use this template** y, después, **Create a new repository**.
3. Si participas en un evento organizado por GitHub o Microsoft, sigue las indicaciones de los mentores. En caso contrario, crea el repositorio en una organización donde tengas acceso a GitHub Copilot.
4. Anota la ruta del repositorio (**organization-or-user-name/repository-name**), que necesitarás más adelante.

> [!NOTE]
> Al crear el repositorio a partir de la plantilla, se crea automáticamente un backlog de incidencias de GitHub que utilizarás durante el taller.

Utiliza una copia nueva de la plantilla. Incluye las instrucciones del repositorio, el código de la aplicación, las pruebas, la habilidad `quality-checks` y el backlog. Si utilizas una copia anterior, comprueba con quien imparte el taller que contiene los archivos necesarios.

## Crear un Codespace

Crea un Codespace para completar el taller. El entorno de desarrollo en la nube permite escribir, ejecutar y depurar código directamente en el navegador.

1. Abre el nuevo repositorio.
2. Selecciona **Code**.
3. Selecciona la pestaña **Codespaces** y, después, **Create codespace on main**.
4. Espera a que termine la configuración. La plantilla instala las dependencias del proyecto y Playwright Chromium, y prepara la base de datos local.
5. Si aparece el mensaje **Do you trust the authors of the files in this folder?**, selecciona **Trust Folder & Continue**.
6. Abre un terminal en la raíz del repositorio e inicia la aplicación:

   ```bash
   npm run dev
   ```

7. Cuando Codespaces indique que el puerto `4321` está disponible, selecciona **Open in Browser** y confirma que se carga el sitio de Tailspin Toys.
8. Vuelve al terminal y detén el servidor de desarrollo con <kbd>Ctrl</kbd>+<kbd>C</kbd>.

## Resumen y pasos siguientes

Ya tienes el entorno preparado. En este ejercicio:

- has creado tu copia del proyecto Tailspin Toys a partir de la plantilla.
- has creado un Codespace y confirmado que el proyecto está listo.

A continuación, [instalarás GitHub Copilot CLI][next-lesson] en tu Codespace y te autenticarás con tu cuenta de GitHub.

## Recursos

- [Información general sobre GitHub Codespaces][codespaces]
- [Crear un repositorio a partir de una plantilla][template-repository]
- [Inicio rápido de Codespaces][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[next-lesson]: ../1-install-copilot-cli/
[codespaces]: https://github.com/features/codespaces
