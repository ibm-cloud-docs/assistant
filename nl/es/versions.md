---

copyright:
  years: 2015, 2019
lastupdated: "2019-08-05"

subcollection: assistant

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:deprecated: .deprecated}
{:important: .important}
{:note: .note}
{:tip: .tip}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:swift: .ph data-hd-programlang='swift'}

# Creación de versiones de conocimientos
{: #versions}

Las versiones le ayudan a gestionar el flujo de trabajo de un proyecto de desarrollo de conocimientos de diálogo.
{: shortdesc}

Cree una versión de conocimiento para capturar una instantánea de los datos de entrenamiento (intenciones y entidades) y un diálogo en el conocimiento en puntos clave durante el proceso de desarrollo. El hecho de poder guardar un conocimiento en curso en un determinado punto en el tiempo resulta especialmente útil si empieza a ajustar el asistente. A menudo es necesario realizar un cambio y ver el impacto del cambio en tiempo real para poder determinar si el cambio mejora la efectividad del asistente o al contrario. En función de los resultados en un despliegue de entorno de prueba, puede tomar una decisión informada sobre si debe desplegar un cambio determinado en un asistente que se ha desplegado en un entorno de producción.

Si tiene un plan gratuito (Lite), no puede crear versiones de conocimientos.
{: note}

En este vídeo de 2 minutos y medio se describe cómo le puede ayudar el uso de versiones.

<iframe class="embed-responsive-item" id="youtubeplayer" title="Creación de versiones de un conocimiento" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/FDolnBxvcZ8" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

Para obtener más información sobre cómo las versiones pueden mejorar el flujo de trabajo que utiliza para crear un asistente,
[lea esta publicación de blog ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://medium.com/ibm-watson/watson-assistant-versions-announcement-d60869b1f5f){: new_window}.

## Creación de una versión
{: #versions-create}

Puede editar solo una versión del conocimiento de diálogo a la vez. La versión en curso se denomina la versión de *desarrollo*.

Cuando se guarda una versión, también se guardan los valores del conocimiento que ha aplicado a la versión de desarrollo.

Para crear una versión de conocimiento de diálogo, siga estos pasos:

1.  Desde la cabecera del conocimiento, pulse **Guardar versión nueva** y, a continuación, describa el estado actual del conocimiento.

    El hecho de añadir una buena descripción le ayudará a distinguir posteriormente varias versiones entre sí.

1.  Pulse **Guardar**.

Se toma una instantánea del conocimiento actual y se guarda como una nueva versión. El usuario permanece en la versión de desarrollo del conocimiento. Los cambios que realice se siguen aplicando a la versión de desarrollo, no a la versión que ha guardado. Para acceder a la versión que ha guardado, vaya a la página **Versiones**.

## Despliegue de una versión de conocimiento
{: #versions-deploy}

1.  En la cabecera del conocimiento, pulse el separador **Versiones**.
1.  Pulse el icono ![Pulsar para ver acciones](images/kebab-react.png) de la versión que desea desplegar y luego seleccione **Asignar a asistente**.

    Se muestra una lista de los asistentes a los que puede enlazar esta versión. La lista se limita a aquellos asistentes que no tienen ningún conocimiento asociado o que están asociados con una versión diferente de este conocimiento.
1.  Pulse el recuadro de selección correspondiente a uno o varios de los asistentes y luego pulse **Asignar**.

Realice un seguimiento de cuándo se ha desplegado esta versión en un asistente y durante cuánto tiempo. Es probable que desee analizar las conversaciones `de usuario que se producen entre los usuarios y esta versión específica del conocimiento. Puede obtener esta información en la página **Análisis**. Sin embargo, cuando se elige un origen de datos, las versiones no aparecen en la lista. Debe elegir el nombre del asistente en el que ha desplegado esta versión. Luego puede filtrar los datos de métricas para mostrar sólo las conversaciones que se han producido entre las fechas de inicio y de finalización del intervalo de tiempo durante el que se ha desplegado esta versión del conocimiento en el asistente.
{: important}

## Límites de versiones de un conocimiento
{: #versions-limits}

El número de versiones que puede crear para un conocimiento depende del plan de {{site.data.keyword.conversationshort}}.

| Plan de servicio     | Versiones por conocimiento |
|------------------|-------------------:|
| Premium          |                 50 |
| Plus             |                 10 |
| Estándar         |                 10 |
| Plus Trial       |                 10 |
| Lite             |                  0 |
{: caption="Detalles del plan de servicio" caption-side="top"}

## Intercambio de conocimientos
{: #versions-swap-skills}

Si cree que una versión anterior de un conocimiento reconocía y respondía mejor a los requisitos de los clientes que una versión posterior, puede cambiar el conocimiento que está enlazado con el asistente para utilizar la versión anterior.

Siga los mismos pasos que se utilizan para [desplegar una versión de un conocimiento](#versions-deploy) para cambiar la versión que está enlazada a un asistente.

## Acceso a una versión guardada
{: #versions-view}

La única forma de ver una versión guardada consiste en sobrescribir la versión de desarrollo en curso del conocimiento con la versión guardada. (Pero no antes de haber guardado el trabajo que haya realizado en la versión de desarrollo actual).

No se puede editar una versión guardada. Para alcanzar el mismo objetivo, puede utilizar una versión guardada como base para una nueva versión en la que incorporar los cambios que desee realizar. Para comenzar el trabajo de desarrollo desde una versión guardada, sobrescriba la versión de desarrollo en curso del conocimiento con la versión guardada.

1.  Guarde los cambios que ha realizado en el conocimiento desde la última vez que creó una versión.

    Guardar ahora una versión del conocimiento. De lo contrario, su trabajo se perderá cuando siga estos pasos.
    {: important}

1.  Desde la versión que desea editar, pulse el icono **Acciones de conocimiento** ![Acciones de conocimiento](images/kebab-react.png) y luego seleccione **Revertir a esta versión** y confirme la acción.

    La página se renueva para volver al estado en el que se encontraba el conocimiento cuando se creó la versión.

Si desea guardar los cambios que realice en esta versión, debe guardar el conocimiento como una nueva versión. No se pueden aplicar cambios a una versión que ya esté guardada.

Cuando se abre un conocimiento pulsando el mosaico del conocimiento en la página de asistente, se muestra la versión de desarrollo del conocimiento. Aunque haya asociado una versión posterior con el asistente, cuando accede al conocimiento se abre su versión de desarrollo.
{: important}
