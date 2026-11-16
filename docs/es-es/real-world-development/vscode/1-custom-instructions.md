---
title: "Ejercicio 1 - Instrucciones personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Requisitos previos][previous-lesson] · [Siguiente lección: Modo agente →][next-lesson]

El contexto es clave cuando se trabaja con IA generativa. Si una tarea debe hacerse de una forma concreta, o hay información de fondo que Copilot debería conocer, te interesa asegurarte de que ese contexto esté disponible. Hay varias formas de compartir contexto específico con Copilot. Entre ellas destacan los [archivos de instrucciones][instruction-files], con los que proporcionas esa guía para la generación de código.

En este ejercicio:

- explorarás cómo el contexto específico del proyecto, las directrices de código y los estándares de documentación llegan a Copilot a través de las instrucciones personalizadas del repositorio y de los archivos de instrucciones con ámbito de ruta,
- generarás el primer bloque de datos para el filtrado (un helper de editoriales) con las instrucciones *actuales*,
- añadirás un nuevo estándar general del repositorio a `.github/copilot-instructions.md`,
- volverás a ejecutar la misma indicación y verás cómo el código generado adopta el nuevo estándar,
- confirmarás en `main` la actualización de las instrucciones y el bloque de filtrado para que Copilot pueda usar la guía actualizada en el ejercicio siguiente.

> [!CAUTION]
> El código generado puede diferir de algunos de los estándares que establezcas. Copilot no es determinista. El objetivo de este ejercicio es ver la *tendencia* de cambio del comportamiento tras actualizar las instrucciones, no igualar la salida carácter por carácter.

## Archivos de instrucciones

### Escenario

Como cualquier buen equipo de desarrollo, Tailspin Toys tiene un conjunto de directrices y requisitos para sus prácticas de desarrollo. Entre ellos:

- La capa de datos siempre necesita pruebas unitarias.
- La interfaz debe estar en modo oscuro y tener un aspecto moderno.
- Hay que añadir documentación al código en forma de comentarios de documentación TSDoc.
- Hay que añadir un bloque de comentarios al principio de cada archivo que describa qué hace.

Mediante los archivos de instrucciones te asegurarás de que Copilot dispone de la información adecuada para realizar las tareas conforme a las prácticas indicadas.

### Instrucciones personalizadas

Las instrucciones personalizadas te permiten proporcionar a Copilot contexto y preferencias para que comprenda mejor tu estilo de programación y tus requisitos. Es una función potente que ayuda a orientar a Copilot para obtener sugerencias y fragmentos de código más relevantes. Puedes especificar tus convenciones de código preferidas, las bibliotecas e incluso el tipo de comentarios que quieres incluir en el código. Puedes crear instrucciones para todo el repositorio o para tipos de archivos concretos, para aportar contexto a nivel de tarea.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md`, un único archivo de instrucciones que se envía a Copilot en **todas** las solicitudes del repositorio. Debe contener información de nivel de proyecto: contexto relevante para la mayoría de las solicitudes de chat o de CLI enviadas a Copilot. Puede incluir la pila tecnológica utilizada, una descripción general de lo que se está construyendo, prácticas recomendadas y otras directrices globales.
- Los archivos `.github/instructions/*.instructions.md` se pueden crear para tareas o tipos de archivo concretos. Puedes usarlos para proporcionar directrices para lenguajes concretos (como TypeScript o Astro) o para tareas como crear un componente de interfaz o un nuevo conjunto de pruebas unitarias.

> [!NOTE]
> Cuando trabajas en tu IDE, los archivos de instrucciones solo se usan para la generación de código en Copilot Chat, no para las finalizaciones de código ni para las sugerencias de edición siguiente.
>
> Copilot Chat, Copilot CLI y el agente en la nube de Copilot usan tanto los archivos de nivel de repositorio como los archivos `*.instructions.md` (con el frontmatter `applyTo`) al generar código.
>
> Por último, Copilot [admite archivos de instrucciones con otros estándares][custom-instructions-support], como los archivos AGENTS.md y CLAUDE.md.

### Prácticas recomendadas para gestionar los archivos de instrucciones

Una conversación completa sobre cómo crear archivos de instrucciones excede el alcance del taller. Sin embargo, los ejemplos del proyecto de muestra reflejan un enfoque representativo. A grandes rasgos:

- Mantén las instrucciones de `copilot-instructions.md` centradas en la guía de nivel de proyecto, como la descripción de lo que se está construyendo, la estructura del proyecto y los estándares de código globales.
- Usa archivos `*.instructions.md` para dar instrucciones específicas para tipos de archivo (pruebas unitarias, componentes de Astro, la capa de datos) o para tareas concretas.
- Usa lenguaje natural. Mantén la guía clara. Incluye ejemplos de cómo debe (y cómo no debe) verse el código.

No existe una única forma de crear archivos de instrucciones, igual que no existe una única forma de usar la IA. Descubrirás con la experimentación lo que mejor funciona en tu proyecto.

> [!TIP]
> Todo proyecto que use GitHub Copilot debería tener una colección sólida de archivos de instrucciones. Al explorar los de este proyecto, verás que hay archivos para numerosos tipos de tareas, entre ellos las [actualizaciones de la interfaz][ui-instructions] y [Astro][astro-instructions].
>
> Copilot también puede ayudarte a generar archivos de instrucciones. Cada superficie lo expone de forma distinta (por ejemplo, **Configure Chat → Generate Agent Instructions** en VS Code o `/init` en Copilot CLI); la lección correspondiente a la superficie que uses lo indicará donde sea relevante.
>
> Si buscas plantillas o un punto de partida, explora [awesome-copilot][awesome-copilot], un repositorio lleno de archivos de instrucciones, agentes personalizados y otros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explorar los archivos de instrucciones personalizadas de este proyecto

Tómate un momento para leer los archivos de instrucciones que incluye este repositorio: hay un archivo principal `copilot-instructions.md` y una colección de archivos `*.instructions.md` para distintas tareas. Ábrelos en tu editor o en la interfaz web de GitHub.

1. Abre `.github/copilot-instructions.md`.
2. Explora el archivo y fíjate en la breve descripción del proyecto y en secciones como **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. En **Code standards**, observa la guía anidada de **GitHub Actions Workflows**. Todo esto se aplica a cualquier interacción que tengas con Copilot.
3. Abre la carpeta `.github/instructions` y echa un vistazo. Observa que hay instrucciones para archivos de Astro, la capa de datos de Drizzle, las pruebas y más.
4. Abre `.github/instructions/unit-tests.instructions.md`. Fíjate en el campo `applyTo` de la parte superior: define un patrón glob (relativo a la raíz del repositorio) que determina a qué archivos se aplican las instrucciones. Aquí coincidirá cualquier archivo de pruebas de TypeScript (por ejemplo, uno que coincida con `**/*.test.ts`).
5. Fíjate en las instrucciones específicas para crear pruebas unitarias en este proyecto.
6. Por último, abre `.github/instructions/drizzle.instructions.md` y desplázate hasta el final. Observa los vínculos a otros archivos de instrucciones (como `unit-tests.instructions.md`) y a archivos existentes del proyecto. Así puedes dividir conjuntos de instrucciones grandes en archivos más pequeños y reutilizables, y dirigir a Copilot hacia ejemplos que seguir al generar código. (Las rutas que aparecen allí son relativas al archivo de instrucciones, no a la raíz del repositorio).

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` documenta los estándares de código del proyecto, pero todavía no exige documentación en el código. En los pasos siguientes añadirás reglas para los comentarios de documentación TSDoc y las cabeceras de comentarios de archivo.

## Crear una rama para los cambios

Sigamos algunas prácticas recomendadas y creemos una rama para nuestros cambios.

1. Vuelve a tu codespace del ejercicio anterior.
2. Abre un nuevo terminal pulsando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crea una rama nueva y cámbiate a ella:

   ```bash
   git checkout -b custom-instructions
   ```

## Usar Copilot Chat *antes* de actualizar las instrucciones

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar con los ejercicios siguientes, vuelve a tu codespace, abre el panel de Copilot Chat y selecciona **New Chat** para iniciar una conversación limpia. La selección de modo y de modelo varía en cada ejercicio; cada paso lo indica cuando importa.

Para ver el efecto de las instrucciones personalizadas, empieza enviando una indicación con el archivo de instrucciones actual. Más adelante lo actualizarás y volverás a enviar la misma indicación para ver la diferencia.

1. Cierra las pestañas del editor abiertas de ejercicios anteriores para que Copilot tome solo el contexto que quieres.
2. Abre `src/lib/publishers.ts` para que Copilot sepa dónde debe ubicarse el helper.
3. Selecciona **Agent** en la lista desplegable de agentes de la vista de chat para que Copilot pueda aplicar cambios en los archivos.

   ![Captura de pantalla del selector de agentes en la vista de chat.](../../../_images/shared-chat-mode-selector.png)

4. Envía la indicación siguiente:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot explora el proyecto y aplica actualizaciones de código, que a menudo abarcan el archivo del helper y sus pruebas.
6. Observa que el helper propuesto es una función tipada que recibe un cliente `db` como primer argumento y devuelve una matriz tipada de editoriales; esto procede de las convenciones de la capa de datos de `.github/instructions/drizzle.instructions.md` (que se aplica a `src/lib/*.ts`).
7. Observa que al código propuesto **le faltan** los comentarios de documentación TSDoc y una cabecera de comentarios a nivel de archivo.

> [!CAUTION]
> Como Copilot es probabilístico, es posible que añada comentarios de documentación aunque no se lo hayas indicado. Si ocurre, no pasa nada: lo importante sigue siendo la mejora de *coherencia* tras actualizar las instrucciones.

## Añadir un nuevo estándar del repositorio

Como se ha señalado antes, `.github/copilot-instructions.md` está pensado para proporcionar a Copilot información de nivel de proyecto. Asegurémonos de que los estándares de código del repositorio estén documentados para mejorar las sugerencias de código.

1. Vuelve a abrir `.github/copilot-instructions.md`.
2. Busca la sección **Code formatting requirements**, que debería estar cerca de la línea 27. Observa cómo documenta los estándares de código del proyecto, pero aún no incluye ninguna regla sobre documentación en el código, que es la razón por la que el helper generado no tenía comentarios de documentación.
3. Añade las líneas de Markdown siguientes justo debajo de los estándares existentes para indicar a Copilot que añada cabeceras de comentarios de archivo y comentarios de documentación TSDoc:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Guarda `copilot-instructions.md`.

> [!TIP]
> Como viste en la lección anterior, los archivos de instrucciones pueden crearse a nivel de repositorio (`.github/copilot-instructions.md`) para una guía global, o como archivos `*.instructions.md` para lenguajes, tipos de archivo o tareas concretos. El archivo a nivel de repositorio es el lugar adecuado para estándares que afectan a todo el proyecto, como la regla de comentarios de documentación que acabas de añadir.

## Volver a ejecutar la indicación y observar el cambio

1. Vuelve a Copilot Chat y selecciona **New Chat** para borrar el búfer.
2. Haz clic de nuevo en `src/lib/publishers.ts` para que Copilot se centre en el archivo correcto.
3. Envía **la misma indicación** que antes:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. Observa que el archivo propuesto empieza ahora con un bloque de comentarios similar a este:

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. Observa que la función propuesta incluye ahora un comentario TSDoc similar a este:

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

Acabas de orientar a Copilot para que siga un nuevo estándar del proyecto y lo aplique a código real sobre el que se apoyará el ejercicio siguiente.

## Confirmar las instrucciones y enviar la rama

Los archivos de instrucciones son como cualquier otro recurso del repositorio, por lo que se gestionan con el mismo control de código fuente que usarías con cualquier otro elemento. Vamos a confirmar los cambios y a enviar la rama a nuestro repositorio.

1. Abre una nueva ventana de terminal pulsando <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
2. Desde el terminal, confirma que la actualización de las instrucciones y los cambios del helper están presentes ejecutando:

   ```bash
   git status
   ```

3. Desde el terminal, prepara y confirma la actualización de las instrucciones y todos los archivos que Copilot haya cambiado para la base del helper:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. Desde el terminal, envía la rama al repositorio:

   ```bash
   git push -u origin custom-instructions
   ```

## Crear y combinar una solicitud de incorporación de cambios

Con la rama enviada, debemos crear una solicitud de incorporación de cambios y vincularla a la incidencia del estándar de documentación de tu backlog. Podríamos hacerlo manualmente, pero Copilot puede hacerlo por nosotros con las herramientas de GitHub que ya están conectadas a tu proyecto. Pidamos a Copilot que encuentre la incidencia, cree la PR que la cierre y después la combine.

> [!NOTE]
> Este es el primer ejercicio en el que Copilot actúa en GitHub por ti. La plantilla del proyecto ya conecta las herramientas de GitHub a tu espacio de trabajo, así que no hay nada que configurar; la primera vez que Copilot use una, es posible que VS Code te pida iniciar sesión en GitHub. Sigue las indicaciones para permitirlo. Aprenderás cómo funciona esta conexión (Model Context Protocol) en un ejercicio posterior.

1. Abre Copilot Chat dentro de tu codespace.
2. Pulsa <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) para abrir la vista de Copilot Chat y asegúrate de que **Agent** está seleccionado en el selector de agentes.
3. Pide a Copilot que busque la incidencia relacionada con la actualización de los archivos de instrucciones y que cree una PR a partir de la rama actual que describa tanto las actualizaciones de las instrucciones como la nueva base del helper de editoriales:

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot empezará a buscar la incidencia y a crear la PR.
5. Cuando se te solicite **Allow** para que Copilot realice acciones de GitHub en tu nombre, revisa el comando y selecciona **Allow** según corresponda.
6. Cuando se haya creado la PR, pide a Copilot que la combine y que devuelva tu rama a main con la indicación siguiente:

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Cuando se te solicite **Allow** para que Copilot realice acciones de GitHub y ejecute comandos de shell en tu nombre, revisa el comando y selecciona **Allow** según corresponda.

Ya has creado y combinado una solicitud de incorporación de cambios con la ayuda de GitHub Copilot.

## Resumen y pasos siguientes

Has explorado cómo Copilot toma el contexto de los archivos de instrucciones de este proyecto y has usado Copilot Chat en VS Code para:

- enviar una indicación de generación de código y observar lo que produce Copilot con las instrucciones *existentes*,
- añadir un nuevo estándar general del repositorio a `.github/copilot-instructions.md`,
- volver a ejecutar la misma indicación y ver cómo el código propuesto adopta el nuevo estándar,
- confirmar en `main` la actualización de las instrucciones y la base del helper para que el ejercicio siguiente pueda apoyarse en ellas.

A continuación, pondrás esas instrucciones en práctica en el [modo agente][next-lesson], mientras Copilot añade una nueva funcionalidad en toda la base de código.

## Recursos

- [Archivos de instrucciones para la personalización de GitHub Copilot][instruction-files]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [5 consejos para escribir mejores instrucciones personalizadas para Copilot][copilot-instructions-five-tips]
- [Instrucciones personalizadas personales para GitHub Copilot][personal-instructions]
- [Awesome Copilot: una colección de archivos de instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
