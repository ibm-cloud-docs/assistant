---

copyright:
  years: 2015, 2019
lastupdated: "2019-02-21"

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

# Acceso a espacios de trabajo
{: #edit-convo-workspace}

Si ha creado un espacio de trabajo con una versión anterior del servicio (incluso de cuando se llamaba {{site.data.keyword.watson}} Conversation), puede seguir utilizando el espacio de trabajo.
{: shortdesc}

## Cambio del espacio de trabajo de conversación
{: #edit-convo-workspace-task}

El espacio de trabajo sigue estando disponible; solo que ahora se denomina *conocimiento*. Para realizar cambios en un espacio de trabajo antiguo, siga los pasos siguientes:

1.  En la página de inicio de {{site.data.keyword.conversationshort}}, pulse **Conocimientos**.

    Los espacios de trabajo que ha creado con la versión disponible a nivel general del servicio {{site.data.keyword.conversationshort}}, se visualizan como mosaicos de conocimiento de diálogo.
1.  Pulse el conocimiento de diálogo que representa el espacio de trabajo que desea editar.
1.  Realice los cambios o adiciones que desee en los datos de entrenamiento o en el diálogo.
1.  Guarde los cambios.

Una vez que se haya completado el entrenamiento, las actualizaciones estarán disponibles para la aplicación personalizada desde la que ha llamado al servicio. Consulte [Visión general de la API de Watson Assistant](/docs/services/assistant?topic=assistant-api-overview) para obtener más información.

## Limitaciones
{: #edit-convo-workspace-cons}

Con la versión más reciente del servicio, puede seguir haciendo todo lo que hacía con el servicio antiguo, pero con más flexibilidad. ¿Ha creado un espacio de trabajo con una versión anterior del servicio y lo está llamando desde una aplicación existente que no desea sustituir? Ya gestiona el estado y realiza funciones útiles sobre el diálogo individual que desea seguir controlando. Todavía puede organizar las llamadas con la última versión de la API /message. La ventaja es que no tiene que hacerlo. En la versión más reciente, puede dar soporte a más de un canal de integración a la vez con el mismo conocimiento de diálogo subyacente.

Si elige omitir el paso de crear un asistente y añadir al mismo su conocimiento de diálogo, perderá la simplicidad que proporcionan los asistentes. Concretamente, **no podrá** utilizar una sola conversación para interactuar con los clientes a través de varios canales de integración a la vez y rápidamente expandir o cambiar a nuevos canales muy utilizados entre los usuarios.

## Conocimientos y espacios de trabajo
{: #edit-convo-workspace-names}

Lo que se presenta en la herramienta como un conocimiento de diálogo es efectivamente un derivador de un espacio de trabajo V1. Aunque actualmente no hay métodos de API para la creación de conocimientos con la API V2, puede seguir utilizando la API V1 para crear espacios de trabajo.

- Cuando abra la herramienta, los espacios de trabajo creados antes del 9 de noviembre se muestran como conocimientos. El nombre del conocimiento se toma del nombre del espacio de trabajo. Sin embargo, si posteriormente cambia el nombre o la descripción del conocimiento, estos cambios no afectan al espacio de trabajo. Del mismo modo, si utiliza las API para editar el nombre o la descripción del espacio de trabajo, estos cambios no afectan al conocimiento.
- En la herramienta, los detalles de la API correspondientes al conocimiento muestran tanto el ID del conocimiento como el ID del espacio de trabajo.
