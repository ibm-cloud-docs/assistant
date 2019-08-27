---

copyright:
  years: 2015, 2019
lastupdated: "2019-07-16"

subcollection: assistant


---

{:curl: #curl .ph data-hd-programlang='curl'}
{:javascript: #javascript .ph data-hd-programlang='javascript'}
{:java: #java .ph data-hd-programlang='java'}
{:python: #python .ph data-hd-programlang='python'}
{:swift: data-hd-programlang='swift'}
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:codeblock: .codeblock}
{:download: .download}
{:deprecated: .deprecated}
{:important: .important}
{:note: .note}
{:tip: .tip}

# Visión general de la API de {{site.data.keyword.conversationshort}}
{: #api-overview}

Puede utilizar las API REST de {{site.data.keyword.conversationshort}} y los SDK correspondientes para desarrollar aplicaciones que interactúen con el servicio.

## Aplicaciones clientes

Para crear un asistente virtual u otra aplicación cliente que se comunique con un asistente en tiempo de ejecución, utilice la nueva API v2. Con esta API, puede desarrollar un cliente de cara al usuario que se pueda desplegar en un entorno de producción, una aplicación que actúe como intermediaria en la comunicación entre un asistente y otro servicio (como un servicio de chat o un sistema de fondo) o una aplicación de prueba.

Al utilizar la API de tiempo de ejecución v2 para comunicarse con el asistente, la aplicación puede beneficiarse de las siguientes características:

- **Gestión automática de estado.** La API de tiempo de ejecución v2 gestiona cada sesión con un usuario final, almacenando y manteniendo todos los datos de contexto que su asistente necesita para una conversación completa.

- **Facilidad de despliegue utilizando asistentes.** Además de dar soporte a clientes personalizados, un asistente se puede desplegar fácilmente en canales de mensajería populares como Slack y Facebook Messenger.

- **Control de versiones.** Con el mantenimiento de versiones de conocimiento de diálogo, puede guardar una instantánea de su conocimiento y enlazar su asistente con esa versión específica. Luego puede continuar actualizando la versión de desarrollo sin que afecte al asistente de producción.

- **Capacidades de búsqueda.** La API de tiempo de ejecución v2 se puede utilizar para recibir respuestas de conocimientos tanto de diálogo como de búsqueda. Cuando se envía una consulta que su conocimiento de diálogo no puede responder, el asistente puede utilizar un conocimiento de búsqueda para encontrar la mejor respuesta en los orígenes de datos configurados. (Los conocimientos de búsqueda son una función beta disponible sólo para los usuarios del plan Plus o Premium).

Para ver más información sobre la API v2, revise la {{site.data.keyword.conversationshort}} [Consulta de API v2 ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://{DomainName}/apidocs/assistant-v2){: new_window}.

**Nota**: La API v1 de {{site.data.keyword.conversationshort}} admite el método `/message` heredado que envía la entrada de usuario directamente al espacio de trabajo que utiliza un conocimiento de diálogo. La API v1 de tiempo de ejecución recibe soporte principalmente a efectos de compatibilidad con versiones anteriores. Si utiliza el método v1 `/message`, debe implementar su propia gestión de estado, y no puede aprovechar el mantenimiento de versiones ni ninguna de las otras características de un asistente.

## Creación de aplicaciones

La API v1 proporciona métodos que permiten a una aplicación crear o modificar conocimientos de diálogo, como alternativa a la creación de un conocimiento de forma gráfica mediante la interfaz de usuario de {{site.data.keyword.conversationshort}}. Una aplicación de creación utiliza la API para crear y modificar conocimientos, intenciones, entidades, nodos de diálogo y otros artefactos que componen un conocimiento de diálogo. Para obtener más información, consulte la [Referencia de API v1 ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://{DomainName}/apidocs/assistant){: new_window}.

  **Nota:** los métodos de creación de v1 crean y modifican espacios de trabajo en lugar de conocimientos. Un espacio de trabajo es un contenedor para el diálogo y los datos de entrenamiento (por ejemplo, intenciones y entidades) dentro de un conocimiento de diálogo. Si crea un espacio de trabajo nuevo utilizando la API, aparecerá como un nuevo conocimiento de diálogo en la interfaz de usuario de {{site.data.keyword.conversationshort}}.

Para ver una lista de los métodos de API disponibles, consulte [Resumen de métodos de API](/docs/services/assistant?topic=assistant-api-methods).
