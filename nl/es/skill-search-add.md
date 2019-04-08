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

# Creación de un conocimiento de búsqueda
{: #skill-search-add}

Un asistente utiliza un *conocimiento de búsqueda* para direccionar las consultas de cliente complejas al servicio {{site.data.keyword.discoveryfull}}. {{site.data.keyword.discoveryshort}} trata la entrada de usuario como una consulta de búsqueda. Localiza la información relevante para la consulta de un origen de datos externo y la devuelve al asistente.
{: shortdesc}

Esta característica solo está disponible para los participantes en el programa beta. Para obtener información sobre cómo solicitar acceso, consulte [Participe en el programa beta](/docs/services/assistant?topic=assistant-feedback#feedback-beta).

![Beta](images/beta.png) IBM proporciona servicios, características y soporte de idioma nacional para su evaluación que se clasifican como versiones beta. Estas características pueden ser inestables, pueden cambiar con frecuencia y pueden dejar de recibir soporte tras un aviso con poca antelación. Las características beta también pueden no proporcionar el mismo nivel de rendimiento o compatibilidad que proporcionan las características con disponibilidad general, y no están pensadas para su uso en un entorno de producción.

Puede añadir un conocimiento de búsqueda a un asistente. Consulte [Límites de conocimientos](/docs/services/assistant?topic=assistant-skill-add#skill-add-limits) para obtener información sobre los límites por plan.

El conocimiento de búsqueda busca información de una recopilación de datos que crea el usuario mediante el servicio {{site.data.keyword.discoveryshort}}. {{site.data.keyword.discoveryshort}} es un servicio que rastrea, convierte y normaliza los datos no estructurados. El servicio aplica el análisis de datos y la intuición cognitiva para enriquecer los datos de forma que luego pueda encontrar y recuperar fácilmente la información significativa. Para obtener más información sobre {{site.data.keyword.discoveryshort}}, consulte la [documentación del producto ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](/docs/services/discovery?topic=discovery-about).

El servicio {{site.data.keyword.discoveryfull}} se activa de las formas siguientes:

- **Nodo de cualquier otra cosa**: busca en un origen de datos externo una respuesta relevante cuando ninguno de los nodos de diálogo puede responder a la consulta del usuario. En lugar de mostrar un mensaje estándar como, por ejemplo, `No sé cómo ayudarle con este asunto.`, el asistente puede decir `Quizás esta información le ayude:`, seguido del texto que devuelve la búsqueda. Si hay un conocimiento de búsqueda enlazado al asistente, siempre que se activa el nodo `anything_else`, en lugar de mostrar la respuesta del nodo se realiza una búsqueda. El asistente pasa la entrada de usuario como consulta al conocimiento de búsqueda y devuelve los resultados de la búsqueda como respuesta.
- **Tipo de respuesta de búsqueda**: si añade un tipo de respuesta de búsqueda a un nodo de diálogo, el servicio recupera texto de un origen de datos externo y lo devuelve como respuesta a una pregunta concreta. Este tipo de búsqueda se produce únicamente cuando se procesa el nodo de diálogo individual. Este método resulta útil si desea limitar una consulta de usuario antes de realizar una búsqueda. Por ejemplo, la rama de diálogo podría recopilar información sobre el tipo de dispositivo que el cliente desea comprar. Si conoce la versión y el modelo, puede enviar una palabra clave de modelo en la consulta que se envía al conocimiento de búsqueda y obtener mejores resultados.
- **Solo conocimiento de búsqueda**: si solo hay un conocimiento de búsqueda asociado a un asistente y no hay ningún conocimiento de diálogo enlazado al mismo, la consulta de búsqueda se envía al servicio {{site.data.keyword.discoveryshort}} cuando se recibe una entrada de usuario de uno de los canales de integración del asistente.

## Creación de un conocimiento de búsqueda
{: #skill-search-add-task}

Si no lo ha hecho, siga los pasos de requisito previo de la [guía de aprendizaje de iniciación](/docs/services/assistant?topic=assistant-getting-started#getting-started-prerequisites) para crear una instancia del servicio {{site.data.keyword.conversationshort}} e iniciar la herramienta {{site.data.keyword.conversationshort}}.

Utilice la herramienta {{site.data.keyword.conversationshort}} para crear conocimientos. Siga estos pasos para crear un conocimiento:

1.  Pulse el separador **Conocimientos**.

1.  Pulse **Crear nuevo**.

1.  Pulse **Añadir** para crear un *conocimiento de búsqueda*.

1.  Especifique los detalles del nuevo conocimiento:
    - **Nombre**: un nombre no puede contener más de 100 caracteres. El nombre es obligatorio.
    - **Descripción**: una descripción no puede contener más de 200 caracteres.

1.  Pulse **Crear**.

Los pasos restantes difieren en función de si tiene acceso a una instancia del servicio {{site.data.keyword.discoveryshort}} existente con recopilaciones creadas o no. Siga el procedimiento adecuado para su situación:

- [Conéctese a una instancia existente de Watson Discovery](#skill-search-add-connect-discovery)
- [Cree una instancia de Watson Discovery](#skill-search-add-create-discovery)

## Conexión a una instancia existente del servicio Watson Discovery
{: #skill-search-add-connect-discovery}

1.  Elija la instancia del servicio {{site.data.keyword.discoveryshort}} de la que desea extraer información.
{: #choose-d-instance}

    En la lista se muestran las instancias del servicio {{site.data.keyword.discoveryshort}} a las que tiene acceso.

    Si ve un aviso que indica que algunas de las instancias del servicio {{site.data.keyword.discoveryshort}} no tienen credenciales definidas, significa que tiene acceso al menos a una instancia que nunca ha abierto directamente desde el panel de control de {{site.data.keyword.cloud_notm}}. Debe acceder a una instancia de servicio para que se creen credenciales para la misma. Las credenciales deben existir para que {{site.data.keyword.conversationshort}} pueda establecer una conexión con la instancia del servicio {{site.data.keyword.discoveryshort}} en su nombre. Si cree que debe aparecer en la lista una instancia del servicio {{site.data.keyword.discoveryshort}} que no aparece, abra la instancia directamente desde el panel de control de {{site.data.keyword.cloud_notm}} para generar credenciales para la misma.

1.  Indique la recopilación de datos que se va a utilizar, realizando una de las siguientes acciones:
{: #pick-data-collection}

    - Elija una recopilación de datos existente.

      Pulse el enlace *Abrir en Discovery* para revisar la configuración de una recopilación de datos antes de decidir la que va a utilizar.

      Vaya a [Configurar la búsqueda](#beta-search-skill-add-configure).

    - Si no tiene una recopilación o no desea utilizar ninguna de las recopilaciones de datos que aparecen en la lista, pulse **Crear una nueva recopilación** para añadir una. Siga el procedimiento del apartado sobre [Creación de una recopilación de datos](#beta-search-skill-add-create-discovery-collection).

      El botón **Crear una nueva recopilación** no se visualiza si ha alcanzado el límite de recopilaciones que tiene permitido crear en función del plan de servicio de {{site.data.keyword.discoveryshort}}. Consulte los [planes de precios de {{site.data.keyword.discoveryshort}} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](/docs/services/discovery/discovery-about?topic=discovery-discovery-pricing-plans) para ver información sobre los límites de cada plan.

## Creación de una instancia de servicio de Watson Discovery
{: #skill-search-add-create-discovery}

1.  Para crear una instancia de servicio de {{site.data.keyword.discoveryshort}}, pulse **Crear nueva recopilación**.

    Se crea automáticamente una instancia del servicio {{site.data.keyword.discoveryshort}} y se abre una página de configuración de la nueva instancia de servicio de {{site.data.keyword.discoveryshort}}.

    Se proporciona una instancia del plan Lite del servicio en {{site.data.keyword.Bluemix_notm}}, independientemente del plan de servicio de {{site.data.keyword.conversationshort}} que utilice. Si desea crear la instancia de servicio de {{site.data.keyword.discoveryshort}} como parte de otro plan, deténgase aquí. Cree la instancia de servicio directamente desde el [catálogo de {{site.data.keyword.Bluemix_notm}} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://cloud.ibm.com/catalog/services/discovery) y siga el procedimiento de [Conexión con una instancia de servicio de {{site.data.keyword.discoveryshort}} existente](#skill-search-add-connect-discovery).
    {: important}

1.  Revise los términos y condiciones de uso de la instancia y pulse **Aceptar** para continuar.

1.  [Cree una recopilación de datos](#skill-search-add-create-discovery-collection).

## Creación de una recopilación de datos
{: #skill-search-add-create-discovery-collection}

No intente añadir el origen de datos completos denominado *Watson Discovery News* a su instancia. No es un tipo de datos en el que se puedan realizar búsquedas desde {{site.data.keyword.conversationshort}}.
{: important}

1.  Para crear una recopilación de {{site.data.keyword.discoveryshort}}, realice una de las acciones siguientes:

      - Para crear una recopilación a partir de datos almacenados en un tipo de origen de datos para el que {{site.data.keyword.discoveryshort}} proporciona soporte incorporado, pulse **Conectar con origen de datos**.

        1.  Elija un tipo de origen de datos.
        1.  Especifique la información solicitada para el origen de datos que seleccione y pulse **Conectar**.

            Consulte [Conexión con orígenes de datos ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](/docs/services/discovery?topic=discovery-sources) para ver más detalles.
        1.  Indique la frecuencia con la que desea que los datos del origen de datos se sincronicen con la recopilación que está creando en {{site.data.keyword.discoveryshort}}.
        1.  Especifique la información que desea extraer del origen de datos e incluir en la recopilación de {{site.data.keyword.discoveryshort}}.

            Las opciones que se muestran difieren en función del tipo de origen de datos.

            - Para un origen de datos de Salesforce, seleccione los tipos de objeto que desea extraer de los documentos de origen. Puede seleccionar, por ejemplo, un [tipo de objeto de caso ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://developer.salesforce.com/docs/atlas.en-us.object_reference.meta/object_reference/sforce_api_objects_case.htm#!) que represente un *caso*, que es un problema de un cliente.
            - Para un origen de datos de Sharepoint, especifique vías de acceso.
            - Para repositorios de archivos, especifique directorios o archivos.

        1.  Pulse **Guardar y sincronizar datos**.

            Se crea la recopilación de datos. Una vez finalizado el proceso, se muestra una página de resumen en la herramienta {{site.data.keyword.discoveryshort}} en otro separador del navegador web.
        1.  Pulse **Configurar el conocimiento en {{site.data.keyword.conversationshort}}** para volver a la herramienta {{site.data.keyword.conversationshort}}.

      - Para crear una recopilación mediante la carga de documentos, pulse **Cargar sus propios datos**.

        1.  En primer lugar, defina la colección y, a continuación, cargue los documentos. Proporcione la siguiente información:

            - Nombre de la recopilación. El nombre debe ser exclusivo para esta instancia de servicio.
            - Configuración. Puede utilizar una plantilla de configuración predeterminada o una configuración guardada. Consulte [Configuración del servicio ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](/docs/services/discovery?topic=discovery-configservice) para obtener más información sobre configuraciones.
            - Idioma. Seleccione el idioma de los archivos que va a añadir a esta recopilación. Consulte [Soporte de idiomas ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](/docs/services/discovery?topic=discovery-language-support) para obtener información acerca de los idiomas a los que {{site.data.keyword.discoveryshort}} da soporte.
        1.  Cargue los documentos.

            Los tipos de archivos soportados incluyen archivos PDF, HTML, JSON y DOC. Consulte [Adición de contenido ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](/docs/services/discovery?topic=discovery-addcontent) para obtener más detalles.
            {: note}

            No está disponible la sincronización en curso de los documentos cargados. Si desea adoptar los cambios realizados en un documento, cargue una versión posterior del documento.

Espere a que la recopilación se haya ingerido por completo antes de volver a {{site.data.keyword.conversationshort}}.

## Configuración de la búsqueda
{: #skill-search-add-configure}

1.  En la página de conocimientos de búsqueda de {{site.data.keyword.conversationshort}}, pulse **Configurar**.

1.  Esboce distintos mensajes para compartirlos con los usuarios en función del éxito que tenga la búsqueda.

    <table>
    <caption>Mensajes del resultado de la búsqueda</caption>
    <tr>
      <th>Nombre de campo</th>
      <th>Caso de ejemplo</th>
      <th>Mensaje de ejemplo</th>
    </tr>
    <tr>
      <td>Mensaje</td>
      <td>Se devuelven resultados de la búsqueda</td>
      <td>He encontrado esta información que puede resultar útil: </td>
    </tr>
    <tr>
      <td>No se han encontrado resultados.</td>
      <td>No se han encontrado resultados de la búsqueda</td>
      <td>He buscado en mi base de conocimientos la información que podría responder a la consulta, pero no ha encontrado nada útil que compartir.</td>
    </tr>
    <tr>
      <td>Mensaje de error</td>
      <td>Por algún motivo, el servicio no ha podido realizar la búsqueda.</td>
      <td>Es posible que tenga información que podría ayudar a responder a su consulta, pero en este momento no puedo realizar búsquedas en mi base de conocimientos.</td>
    </tr>
    </table>

1.  Seleccione los campos de la recopilación de {{site.data.keyword.discoveryshort}} de los que desea extraer texto.

    Los campos que están disponibles difieren en función de los datos que se han ingerido y de la configuración que ha utilizado para ingerirla.

    Cada resultado de la búsqueda puede constar de estas partes de información:

    - **Título**: título del resultado de la búsqueda. Utilice el título, el nombre o un tipo de campo similar de la recopilación como título de resultado de la búsqueda.

      Se debe seleccionar alguno que no sea `Ninguno` para que las integraciones de Facebook y Slack puedan mostrar la respuesta.
    - **Cuerpo**: descripción del resultado de la búsqueda. Utilice un campo abstracto, de resumen o de resaltado de la recopilación como cuerpo del resultado de la búsqueda.

      Se debe seleccionar alguno que no sea `Ninguno` para que las integraciones de Facebook y Slack puedan mostrar la respuesta.
    - **Url**: enlace de hipertexto con el objeto de datos original en su origen de datos nativo. La mayoría de los orígenes de datos en línea proporcionan URL públicos de referencia automática para objetos del almacén para dar soporte al acceso directo.

      El URL resultante debe ser válido y accesible para que la integración de Slack incluya el URL en la respuesta y para que la integración de Facebook pueda mostrar la respuesta. `Ninguno` es una selección aceptable para las integraciones de Facebook y Slack.

    Consulte [Sugerencias para la selección de campos de recopilación](#skill-search-add-field-tips) para obtener ayuda.
  
    Debe elegir un valor distinto de `Ninguno` para al menos una de las opciones.

    Si no hay ninguna opción disponible en los campos desplegables, es posible que tenga que dar más tiempo a {{site.data.keyword.discoveryshort}} para que termine de crear la recopilación. De lo contrario, es posible que la recopilación no contenga ningún documento o que tenga errores de ingestión que deban ser abordados en primer lugar.

1.  En el panel de vista previa, escriba un mensaje de prueba para ver los resultados que se devuelven cuando se aplican las opciones de configuración a la búsqueda. Realice los ajustes necesarios.

1.  Pulse **Crear**.

Si desea cambiar la configuración más adelante, abra de nuevo el conocimiento de búsqueda y realice los cambios. No es necesario guardar los cambios a medida que los realice; se aplican automáticamente. Cuando esté satisfecho con los resultados de la búsqueda, pulse **Guardar** para finalizar la configuración del conocimiento de búsqueda.

## Siguientes pasos
{: #skill-search-add-next-steps}

Después de crear el conocimiento, aparece como un mosaico en la página Conocimientos.

El conocimiento de búsqueda no puede interactuar con los clientes hasta que se añada a un asistente y este se despliegue. Consulte [Creación de asistentes](/docs/services/assistant?topic=assistant-assistant-add).

Cuando se enlacen tanto un conocimiento de diálogo como un conocimiento de búsqueda a un asistente, el conocimiento de búsqueda se activa automáticamente si el conocimiento de búsqueda procesa la entrada del usuario y no puede ser abordada por ninguno de sus nodos del diálogo. En lugar de responder con una respuesta genérica desde el nodo `anything_else`, se inicia una búsqueda que utiliza la entrada de usuario cuando se inicia la serie de consulta.

Si lo desea, puede definir una consulta de búsqueda específica a la que llamar como respuesta a una determinada condición de nodo. Para ello, añada un tipo de respuesta de búsqueda al nodo de diálogo. Consulte [Respuestas](/docs/services/assistant?topic=assistant-dialog-overview#dialog-overview-multimedia) para obtener más detalles.

Si inicia cualquier tipo de búsqueda a partir del conocimiento de diálogo, pruebe el diálogo para asegurarse de que la búsqueda se active según lo esperado. Por ejemplo, si no utiliza tipos de respuesta de búsqueda, pruebe que se active una búsqueda solo cuando no haya nodos de diálogo existentes que puedan responder a la entrada del usuario. Cada vez que se active una búsqueda, asegúrese de que devuelva resultados con sentido.

### Sugerencias para la selección de campos de recopilación
{: #skill-search-add-field-tips}

Los campos de recolección adecuados de los que extraer datos varían en función del origen de los datos de la recopilación y de la configuración utilizada para ingerir el origen de datos. Para obtener más información sobre la estructura de los documentos de la recopilación, incluidos los nombres de los campos que contienen información que podría desear extraer, abra la recopilación en la herramienta {{site.data.keyword.discoveryshort}} y luego pulse **Ver esquema de datos**.

En la tabla siguiente se muestran los campos de recopilación que puede probar para empezar. En estas sugerencias se presupone que ha utilizado la plantilla de configuración predeterminada para crear la recopilación.

| Tipo de origen de datos   | Título | Cuerpo | Url |
|--------------------|-------|------|-----|
| Documentos PDF cargados | enriched_text.concepts.text | text | Ninguno |
| Recuadro                | name | description | listing_url |

Los campos de la recopilación se crean cuando se crea la recopilación. Para obtener más información sobre los campos que se generan cuando se utiliza la plantilla de configuración predeterminada para la ingestión, como por ejemplo `enriqueed_text.concepts.text`, consulte [Configuración del servicio > Adición de características ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](/docs/services/discovery?topic=discovery-configservice#adding-enrichments).

### Adición del conocimiento a un asistente
{: #skill-search-add-to-assistant}

Puede añadir un conocimiento a un asistente. Debe abrir el mosaico del asistente y añadir el conocimiento al asistente desde la página de configuración del asistente; no puede elegir el asistente que utilizará el conocimiento desde dentro de la página de configuración del conocimiento.

Un conocimiento de búsqueda puede ser utilizado por más de un asistente.

1.  En el separador Asistente, pulse para abrir el mosaico correspondiente al asistente al que desea añadir el conocimiento.

1.  Pulse **Añadir conocimiento de búsqueda**.

1.  Pulse **Añadir conocimiento existente**.

    Pulse el conocimiento que desea añadir entre los conocimientos que se muestran.

Configure al menos un canal de integración de prueba. No puede probar el conocimiento de búsqueda desde el panel Pruébelo. Pruebe el conocimiento desde un canal de integración especificando las consultas que activan la búsqueda. Asegúrese de que la búsqueda se active correctamente y de que devuelva resultados relevantes.

La integración de enlace que se puede compartir no funciona actualmente para asistente con un conocimiento de búsqueda.
{: important}
