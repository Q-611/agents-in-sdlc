---
title: "Ejercicio 4 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En los últimos ejercicios has pedido al agente de Copilot en la nube que realice tres tareas distintas para mejorar la experiencia de usuario y añadir funcionalidades. Aunque trabaja de forma asíncrona y autónoma, sigue siendo importante poder supervisar esas tareas.

Hay varias herramientas para gestionar las tareas asignadas al agente, entre ellas la [página de agentes][agents-page] de GitHub.com. Desde este centro de control puedes ver las tareas de los agentes que tienen pull requests abiertas, explorar las operaciones realizadas y orientar las sesiones que siguen activas.

En esta lección:

- explorarás la página de agentes para supervisar las tareas del agente en la nube.
- dirigirás una sesión activa para solicitar una funcionalidad adicional.

## Escenario

Después de asignar al agente la tarea de crear un modo de alto contraste, el equipo ha decidido que también convendría añadir un modo claro. Como ya se está actualizando el estilo del sitio y añadiendo controles de alternancia, parece lógico incluir esta funcionalidad. Orientarás al agente para que añada el modo claro además del modo de alto contraste.

## Revisa las tareas del agente de Copilot en la nube

Vamos a consultar el estado de las tareas asignadas al agente de Copilot en la nube.

1. Ve a la [página de agentes de Copilot](https://github.com/copilot/agents).
2. Observa la lista de tareas en el panel principal y en el panel izquierdo. Deberías ver las tareas que has asignado a Copilot, entre ellas:
    - actualizar la documentación del código.
    - generar API para modificar juegos.
    - añadir un modo de alto contraste al sitio web.
3. Selecciona una de las tareas en curso. Revisa las operaciones realizadas por Copilot, que pueden incluir:
    - obtener el código del repositorio.
    - crear el entorno de trabajo de Copilot.
    - configurar servidores MCP.
    - realizar varias operaciones para completar la tarea.

> [!NOTE]
> Los pasos concretos variarán según el estado del trabajo de Copilot y el método que haya seguido.

4. Observa también el panel de pull requests que aparece a la derecha. Desde allí puedes consultar la PR y los archivos modificados para seguir el trabajo.

## Dirige el agente en la nube

Ahora que has visto las tareas activas, pide a Copilot que añada el modo claro mientras trabaja en el modo de alto contraste.

1. Selecciona la sesión relacionada con el modo de alto contraste. El título exacto dependerá del nombre que haya elegido Copilot y del estado del trabajo.

    ![Sesión de accesibilidad en mission control](../../../_images/ex6-accessibility-session.png)

2. Observa la sesión durante unos minutos hasta que indique que ha terminado de configurar el entorno y ha empezado a trabajar. Lo sabrás cuando empieces a ver mensajes similares a los siguientes.
3. En el cuadro de diálogo **Steer active session while Copilot is working**, escribe este prompt:

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Captura de la tarea del agente en la nube en la página de agentes, con el cuadro de diálogo para dirigir una sesión activa resaltado](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pulsa <kbd>Enter</kbd> para enviar el prompt.
5. Observa cómo Copilot confirma que lo ha recibido y lo incorpora al flujo de trabajo.

## Deja que Copilot trabaje

Copilot comenzará a trabajar en la tarea actualizada. Incorporará la nueva solicitud al flujo de trabajo cuando haya terminado el paso que estaba realizando en el momento en que enviaste el mensaje.

Como antes, el proceso tardará varios minutos. Puedes aprovechar el tiempo para repasar lo que has aprendido y explorado hasta ahora.

## Resumen y siguientes pasos

En esta lección has explorado la página de agentes de Copilot, el centro desde el que se supervisan y dirigen las tareas del agente en la nube de GitHub Copilot. Puedes seguir las tareas activas y completadas, revisar el trabajo y redirigir las sesiones en curso para ajustar su alcance o proporcionar más indicaciones.

Has explorado estos conceptos:

- usar mission control y la página de agentes para supervisar las tareas del agente en la nube.
- redirigir una sesión activa para solicitar más funcionalidades.

Mientras Copilot termina de trabajar en las funciones de accesibilidad, puedes [iterar sobre las pull requests que ha creado][next-lesson].

## Recursos

- [Página de agentes de Copilot][agents-page]
- [Acerca de los agentes personalizados][custom-agents]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
