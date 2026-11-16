---
title: "Lección 8 - Crear y combinar la PR de la funcionalidad"
description: "Revisa juntos el filtrado, las instrucciones, la actualización de la habilidad, el perfil de QA y las pruebas; después crea una PR y usa Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

La implementación del filtrado, las actualizaciones de las instrucciones, la actualización de la habilidad, el perfil de control de calidad (QA) y las pruebas están guardados en una sola rama. Es el momento de revisarlos juntos y abrir una solicitud de incorporación de cambios (PR). La PR de las valoraciones por estrellas la combinaste tú mismo; esta vez dejarás que **Agent Merge** gestione el proceso.

> [!NOTE]
> Lo normal sería dividir la funcionalidad, las actualizaciones de las instrucciones, la actualización de la habilidad y el agente de QA en varias PR independientes. Para agilizar el taller, has mantenido todo el flujo de filtrado y calidad en una sola conversación y una sola rama, y todo ese trabajo se incluirá en esta PR.

En esta lección:

- aprenderás qué es Agent Merge y cómo automatiza el ciclo de vida de la combinación.
- inspeccionarás el cambio completo de la funcionalidad y las pruebas de verificación.
- crearás la PR de filtrado.
- habilitarás Agent Merge solo después de la revisión y confirmarás que la PR se ha combinado.

## Escenario

A lo largo del flujo de filtrado has usado Copilot para planificar, implementar y verificar una funcionalidad. Tailspin Toys quiere ahora automatizar el resto del trabajo de la PR sin que la autorización de la combinación deje de estar en manos del desarrollador.

## Presentación de Agent Merge

**Agent Merge** automatiza el trabajo restante necesario para incorporar una solicitud de incorporación de cambios. Cuando lo habilitas, Copilot resuelve lo que bloquea la PR (corregir las comprobaciones de integración continua (CI) que fallan, responder a los comentarios de revisión y volver a basar la rama cuando haga falta) y después habilita la combinación automática de GitHub si el repositorio lo permite.

Hasta ahora has seleccionado **Merge pull request** tú mismo. Agent Merge puede asumir esa responsabilidad. Revisa el trabajo y decide que está listo antes de habilitar Agent Merge.

## Usar Agent Merge para gestionar la PR

Con todo el código creado, vamos a revisarlo en conjunto, crear la PR y dejar que Agent Merge gestione el resto del proceso.

1. Vuelve a tu Codespace.
2. Abre el cuadro de diálogo de agentes escribiendo `/agent`.
3. Selecciona **Default** en la lista de opciones y pulsa <kbd>Enter</kbd>.
4. Crea una PR nueva con el comando `/pr create`.
5. Activa Agent Merge con `/pr agentmerge`.
6. Copilot supervisará el proceso de integración continua de la PR. Cuando todo termine correctamente, realizará la combinación.
7. Comprueba que aparece un mensaje de Copilot similar a "PR #14 was squash-merged successfully."

> [!IMPORTANT]
> Agent Merge no omite las aprobaciones necesarias, la protección de ramas, las colas de combinación, la configuración del repositorio ni la falta de permisos. Si se bloquea, lee el motivo indicado y completa manualmente la combinación revisada cuando tu repositorio lo permita.

## Resumen y pasos siguientes

Has automatizado varias partes del proceso de desarrollo, como la generación de código, las pruebas y la validación del código y, ahora, el proceso de la solicitud de incorporación de cambios. En concreto:

- has aprendido qué es Agent Merge y cómo automatiza el ciclo de vida de la combinación.
- has inspeccionado el cambio completo de la funcionalidad y las pruebas de verificación.
- has creado la PR de filtrado.
- has habilitado Agent Merge solo después de la revisión y has confirmado que la PR se ha combinado.

A continuación, [explorarás más comandos de barra útiles de Copilot CLI][next-lesson] para el contexto, los modelos, el uso compartido y la delegación opcional en la nube.

## Recursos

- [Gestionar solicitudes de incorporación de cambios con Copilot CLI][manage-prs]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
