---

copyright:
  years: 2015, 2019
lastupdated: "2019-02-21"

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
{:deprecated: .deprecated}
{:important: .important}
{:note: .note}
{:tip: .tip}

# Visión general de la API de {{site.data.keyword.conversationshort}}
{: #api-overview}

Puede utilizar las API REST de {{site.data.keyword.conversationshort}} y los SDK correspondientes para desarrollar aplicaciones que interactúen con el servicio. Existen dos versiones de la API de {{site.data.keyword.conversationshort}}, cada una de las cuales da soporte a un conjunto diferente de funciones: v1 y v2. La API que debe utilizar depende del tipo de métodos que requiera su aplicación:

- **Métodos de tiempo de ejecución**: métodos que permiten a una aplicación cliente interactuar con un asistente o un conocimiento existentes, pero no modificarlos. Puede utilizar estos métodos para desarrollar un cliente de cara al usuario que se pueda desplegar en un entorno de producción, una aplicación que actúe como intermediaria en la comunicación entre un asistente y otro servicio (como un servicio de chat o un sistema de fondo) o una aplicación de prueba.

  La API v2 de {{site.data.keyword.conversationshort}} ofrece acceso a los métodos que puede utilizar para interactuar con un asistente en tiempo de ejecución (como por ejemplo `/message`). Es la API preferida para desarrollar nuevas aplicaciones cliente. Para ver más información sobre la API v2, revise la {{site.data.keyword.conversationshort}} [Consulta de API v2 ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://{DomainName}/apidocs/assistant-v2){: new_window}.

  La API v1 de {{site.data.keyword.conversationshort}} incluye un método `/message` que envía la entrada de usuario directamente al espacio de trabajo que utiliza un conocimiento de diálogo, pasando por alto el asistente. La API v1 de tiempo de ejecución recibe soporte principalmente a efectos de compatibilidad con versiones anteriores. Si utiliza el método `/message` de v1, la app no puede aprovechar las posibilidades de orquestación y de gestión de estado de un asistente. Para obtener más información, consulte [Utilización de la API de tiempo de ejecución v1](/docs/services/assistant?topic=assistant-api-client#v1-api).

- **Métodos de creación**: métodos que permiten a una aplicación crear o modificar conocimientos de diálogo, como alternativa a la creación de un conocimiento de forma gráfica mediante la herramienta {{site.data.keyword.conversationshort}}. Una aplicación de creación utiliza varios métodos para crear y modificar conocimientos, intenciones, entidades, nodos de diálogo y otros artefactos que componen un conocimiento de diálogo.

  Para crear una aplicación de creación, utilice la API v1. Para obtener más información, vea la [Consulta de API v1 ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://{DomainName}/apidocs/assistant){: new_window}.

  **Nota:** los métodos de creación de v1 interactúan con espacios de trabajo en lugar de con conocimientos. Un espacio de trabajo es un contenedor para el diálogo y los datos de entrenamiento (por ejemplo, intenciones y entidades) dentro de un conocimiento de diálogo. En la mayoría de los casos, cuando utilice la API puede considerar que los espacios de trabajo y los conocimientos de diálogo son intercambiables; por ejemplo, si crea un nuevo espacio de trabajo mediante la API, aparecerá como un nuevo conocimiento de diálogo en la herramienta {{site.data.keyword.conversationshort}}.

Para ver una lista de los métodos de API disponibles, consulte [Resumen de métodos de API](/docs/services/assistant?topic=assistant-api-methods).
