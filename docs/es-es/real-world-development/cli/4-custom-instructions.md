---
title: "Lección 4 - Guiar a Copilot con instrucciones personalizadas"
description: "Explora las instrucciones del repositorio, añade un estándar de documentación y aplícalo al código de filtrado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

El contexto es clave cuando se trabaja con IA generativa. Si una tarea debe hacerse de una forma concreta, te interesa que Copilot disponga de esa guía. Los [archivos de instrucciones][instruction-files] describen no solo *qué* código quieres, sino también *cómo* debe estructurarse. Ahora que has creado el filtrado, explorarás las instrucciones que usó Copilot, añadirás un estándar de documentación y lo aplicarás a tu código.

En esta lección:

- explorarás cómo llegan al agente las instrucciones del repositorio y los archivos de instrucciones con ámbito de ruta.
- actualizarás el archivo de instrucciones para asegurarte de que se siguen los estándares de código.
- verás el efecto de los archivos de instrucciones en el código.

## Escenario

Como cualquier buen equipo de desarrollo, Tailspin Toys tiene un conjunto de directrices y requisitos para sus prácticas de desarrollo. Entre ellos:

- Los comentarios deben explicar la intención y las decisiones poco evidentes en lugar de repetir el código.
- Las funciones exportadas de `db/` y `src/lib/` deben documentar su propósito, parámetros y valores devueltos con TSDoc/JSDoc, incluido un argumento `db` inyectable cuando exista.
- Los componentes reutilizables de Astro deben documentar sus contratos de `Props`, y los comentarios deben mantenerse al día cuando cambie el código relacionado.
- Debe conservarse la guía de formato y de lint existente.

Mediante los archivos de instrucciones te asegurarás de que Copilot dispone de la información adecuada para realizar las tareas conforme a las prácticas indicadas.

## Archivos de instrucciones

Las instrucciones personalizadas te permiten proporcionar a Copilot contexto y preferencias para que comprenda mejor tu estilo de programación y tus requisitos. Es una función potente que ayuda a orientar a Copilot para obtener sugerencias y fragmentos de código más relevantes. Puedes especificar tus convenciones de código preferidas, las bibliotecas e incluso el tipo de comentarios que quieres incluir en el código. Puedes crear instrucciones para todo el repositorio o para tipos de archivos concretos, para aportar contexto a nivel de tarea.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md`, un único archivo de instrucciones que se envía a Copilot en **todas** las solicitudes del repositorio. Debe contener información de nivel de proyecto relevante para la mayoría de las solicitudes.
- Los archivos `.github/instructions/*.instructions.md`, que proporcionan directrices para lenguajes, tipos de archivo o tareas concretos.

> [!NOTE]
> Otros formatos de instrucciones y su compatibilidad varían según el entorno. Consulta la [referencia de compatibilidad de instrucciones personalizadas][custom-instructions-support] antes de depender de un formato concreto.

## Explorar los archivos de instrucciones personalizadas de este proyecto

Para facilitar los primeros pasos, el proyecto de partida ya incluye un conjunto de archivos de instrucciones. Vamos a explorar lo que ya hay antes de hacer un cambio, para ver después su efecto.

1. Vuelve a tu Codespace.
2. En el editor de Codespaces (no en el terminal), abre `.github/copilot-instructions.md`.
3. Explora el archivo y fíjate en la breve descripción del proyecto y en su guía de programación. Estas instrucciones se aplican a todas las interacciones con Copilot en este repositorio.
4. Abre la carpeta `.github/instructions` y explora los archivos. Observa que hay instrucciones para archivos de Astro, la capa de datos de Drizzle, las pruebas y más.
5. Abre `.github/instructions/unit-tests.instructions.md`. Fíjate en el campo `applyTo` de la parte superior: define un patrón glob que determina a qué archivos se aplican las instrucciones.
6. Abre `.github/instructions/drizzle.instructions.md` y observa sus vínculos a otros archivos de instrucciones y a archivos existentes del proyecto. Así puedes dividir conjuntos de instrucciones grandes en archivos más pequeños y reutilizables, y dirigir a Copilot hacia ejemplos que seguir.

## Actualizar los archivos de instrucciones según la guía del equipo

Aunque los archivos ya creados son un buen comienzo, todavía falta algo. Vamos a modificar el archivo principal `copilot-instructions.md` para asegurarnos de que se añaden comentarios TSDoc al código TypeScript nuevo.

1. En `.github/copilot-instructions.md`, busca la sección titulada **Code formatting guidance**, que debería estar cerca de la línea 35.
2. Añade lo siguiente como último elemento de la lista:

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

El archivo se guardará automáticamente.

## Usar la guía actualizada

Copilot CLI carga las instrucciones del repositorio al iniciar una conversación. Tras guardar las instrucciones actualizadas, sal de la conversación de filtrado y reanúdala para que Copilot parta de la nueva guía sin perder el contexto de la funcionalidad.

1. Si la conversación de filtrado sigue abierta, escribe `/exit` para cerrarla. Copilot CLI guarda las conversaciones automáticamente.
2. Desde la raíz del repositorio, en la rama `game-filters-cli`, reanuda la conversación guardada:

   ```bash
   copilot --resume
   ```

   Si se te solicita, selecciona la conversación de filtrado y confirma que la rama `game-filters-cli` está activa.
3. Pide a Copilot que aplique la guía actualizada:

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. Escribe `/diff` y lee los archivos TypeScript modificados. Fíjate en los comentarios TSDoc recién generados y comprueba que explican el código con exactitud.

## Resumen y pasos siguientes

Has explorado cómo Copilot CLI toma el contexto de los archivos de instrucciones y has aplicado un nuevo estándar a tu funcionalidad. En concreto:

- has explorado cómo llegan al agente las instrucciones del repositorio y los archivos de instrucciones con ámbito de ruta.
- has actualizado el archivo de instrucciones para asegurarte de que se siguen los estándares de código.
- has visto el efecto de los archivos de instrucciones en el código.

A continuación, [personalizarás y ejecutarás la habilidad reutilizable quality-checks][next-lesson] para garantizar que el lint y las pruebas se ejecutan de forma coherente.

## Recursos

- [Añadir instrucciones personalizadas para Copilot CLI][instruction-files]
- [Compatibilidad de instrucciones personalizadas][custom-instructions-support]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Awesome Copilot: una colección de archivos de instrucciones y otros recursos][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
