---
title: "Lección 12 - Publicar el sitio con GitHub Pages"
description: "Usa la aplicación GitHub Copilot para configurar un flujo de trabajo de Pages, revisar su solicitud de incorporación de cambios y comprobar el Space Quiz publicado."
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

Publica el Space Quiz con GitHub Pages para que cualquiera pueda visitarlo. Copilot puede preparar el flujo de trabajo y guiar la publicación, pero debes revisar los cambios y confirmar que el sitio está disponible.

En esta lección:

- pedirás a Copilot que configure la publicación en GitHub Pages.
- revisarás el flujo de trabajo y la solicitud de incorporación de cambios antes de combinarla.
- seguirás la publicación y comprobarás el sitio publicado.

## Escenario

El Space Quiz ya está en un repositorio de GitHub, pero hace falta una forma sencilla de probarlo. Publicar el sitio, que consta de un único archivo, con GitHub Pages lo hace accesible mediante una URL pública sin añadir un servidor ni dependencias de ejecución.

## Pedir a Copilot que publique el sitio

En la sesión de la aplicación Copilot para `space-quiz`, envía el prompt siguiente para pedir que se configure Pages y se publique el sitio:

1. Envía este prompt:

   ```plaintext
   Enable GitHub Pages for this repository. Create a GitHub Actions workflow that builds the website if needed and deploys it to GitHub Pages. Add a README describing the app and how to run it locally if one does not already exist. Create a pull request with these changes and merge it after required checks pass. Monitor the deployment until it succeeds, then report the final URL of the live website. If you lack permission to enable Pages or merge, tell me exactly what I need to approve or do; do not report success until the site is live.
   ```

![Diagrama de tres etapas: una solicitud de incorporación de cambios de space-quiz combinada; una ejecución correcta de Deploy website en GitHub Actions con los pasos Build website, Upload artifact y Deploy to Pages; y el Space Quiz publicado en yourname.github.io/space-quiz/.](../../../_images/first-steps-app-pages-deploy.svg)

> [!WARNING]
> Este prompt pide al agente que cree y combine una solicitud de incorporación de cambios y que cambie la configuración de Pages del repositorio. Revisa las diferencias, los permisos de publicación y las solicitudes de aprobación de GitHub antes de continuar.

## Revisar el flujo de trabajo y comprobar la publicación

Comprueba los cambios y sigue la ejecución de publicación para confirmar que el sitio está disponible, en vez de confiar únicamente en el informe del agente.

1. Confirma que el archivo README se añade solo si el repositorio aún no tiene uno.
2. Revisa los cambios de la solicitud de incorporación de cambios, incluidos los pasos de compilación y publicación del flujo de trabajo.
3. Combina la solicitud únicamente cuando se hayan superado las comprobaciones obligatorias.
4. Sigue la ejecución de publicación de GitHub Actions hasta que finalice correctamente.
5. Abre la URL indicada y confirma que se carga el Space Quiz.

Si el agente no puede habilitar Pages o combinar la solicitud, sigue las instrucciones concretas que te dé. No des la tarea por terminada hasta que la publicación haya finalizado correctamente y el sitio se cargue.

## Resumen y pasos siguientes

El Space Quiz ya está publicado y has comprobado la publicación, en lugar de depender del informe del agente. En esta lección:

- has pedido a Copilot que configure la publicación en GitHub Pages.
- has revisado el flujo de trabajo y la solicitud de incorporación de cambios antes de combinarla.
- has seguido la publicación y comprobado el sitio publicado.

Continúa con [la lección 13: Repaso y pasos siguientes][next-lesson].

[next-lesson]: ../13-review/
