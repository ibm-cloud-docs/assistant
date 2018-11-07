---

copyright:
  years: 2015, 2018
lastupdated: "2018-02-09"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:swift: .ph data-hd-programlang='swift'}

# Despliegue en un canal con el conector de {{site.data.keyword.conversationshort}}

Después de haber desarrollado su espacio de trabajo, puede utilizar el conector de {{site.data.keyword.conversationshort}} para conectarse rápidamente a un canal de comunicación como, por ejemplo, Slack o Facebook Messenger. El conector de {{site.data.keyword.conversationshort}} es un conjunto de componentes de {{site.data.keyword.openwhisk}} que actúan como intermediarios entre su espacio de trabajo de {{site.data.keyword.conversationshort}} y una app de Slack o Facebook propia, almacenando datos de la sesión en una base de datos Cloudant. 

Al conectar su espacio de trabajo de trabajo a una app de canal, puede hacer que esté disponible como un chatbot con el que podrán interactuar los usuarios de Slack o Facebook Messenger. Las respuestas desde su diálogo puede incluir respuestas interactivas y multimedia como, por ejemplo, imágenes y botones que es posible pulsar. (Para obtener más información sobre cómo definir una respuesta multimedia, consulte [Respuestas multimedia](dialog-multimedia.html)). 

![{{site.data.keyword.openwhisk_short}} - Diagrama de visión general de despliegue](images/deploytochannel_diagram.png)

El conector de {{site.data.keyword.conversationshort}} tiene algunas limitaciones: 

- Debe crear una app de Facebook o Slack, o tener permisos administrativos para modificar la app que desea utilizar. 
- Debido a las restricciones de {{site.data.keyword.openwhisk_short}}, esta herramienta solo está disponible actualmente para la región EE.UU. Sur de {{site.data.keyword.Bluemix_notm}}.

## Despliegue en Slack utilizando la herramienta de {{site.data.keyword.conversationshort}}

La herramienta de {{site.data.keyword.conversationshort}} proporciona una forma rápida de desplegar un bot utilizando el conector de {{site.data.keyword.conversationshort}}. La herramienta le lleva por los pasos del proceso de recopilar la información necesaria para configurar su app de canal y los componentes del conector y, a continuación, despliega de forma automática los componentes necesarios en IBM Cloud.

**Nota:** La interfaz de la herramienta de {{site.data.keyword.conversationshort}} actualmente únicamente da soporte a Slack, sin embargo, puede utilizar un proceso de {{site.data.keyword.Bluemix_short}} automatizado para desplegar para Facebook Messenger. Para obtener más información sobre el despliegue para Facebook Messenger, consulte la documentación en el [Repositorio de GitHub ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/watson-developer-cloud/conversation-connector/blob/master/channels/facebook/README.md){: new_window} del conector de {{site.data.keyword.conversationshort}}.  

Para desplegar un bot utilizando la herramienta de despliegue automatizada: 

1. En la herramienta de {{site.data.keyword.conversationshort}}, abra el espacio de trabajo que desea desplegar. 
1. Pulse el icono de menú de la esquina superior izquierda y seleccione **Desplegar**. 

   ![Opción de menú de despliegue rápido](images/deploy_menu_testdeploy.png)

1. Bajo **Desplegar con {{site.data.keyword.openwhisk_short}}**, pulse **Desplegar en Slack**. 
1. En la página de Slack, pulse **Desplegar para app Slack** y siga las instrucciones. 

   ![Botón Desplegar para app Slack](images/deploy_deploytoslack.png)

## Despliegue manual

En lugar de utilizar la herramienta automatizada para desplegar el espacio de trabajo como un bot, puede configurar y desplegar manualmente los componentes necesarios en IBM Cloud. Hay varias situaciones en las que tal vez desee hacerlo:

- **Despliegue parcial**. Puede que desee desplegar algunos componentes de un despliegue existente para cambiar la configuración, corregir los problemas o aplicar un arreglo de una versión más reciente. 
- **Ampliar el bot con nuevas funcionalidades**. Puede modificar los componentes proporcionados para añadir nuevas funciones como, por ejemplo, el preprocesamiento de entrada de usuario antes de enviarlo al espacio de trabajo de {{site.data.keyword.conversationshort}}. 
- **Despliegue en un nuevo canal**. Si desea desplegar un bot a un canal distinto de Slack o Facebook Messenger, puede seguir el patrón de los componentes existentes para desarrollar sus propios componentes. 

Los componentes del conector de {{site.data.keyword.conversationshort}} se alojan en un repositorio GitHub público, que puede descargar o clonar. Para obtener más información, consulte la documentación proporcionada en el [Repositorio ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/watson-developer-cloud/conversation-connector){: new_window}.  

## Conversación con el bot

Después de completar el proceso de despliegue, puede utilizar el nombre de usuario del bot para interactuar con el espacio de trabajo de {{site.data.keyword.conversationshort}}, al igual que lo haría con cualquier otro bot de Slack o Facebook Messenger. 

Tenga en cuenta que el conector de {{site.data.keyword.conversationshort}} conserva el estado por separado para cada usuario que está interactuando con el bot. (En el caso de Slack, un usuario individual puede tener varias conversaciones exclusivas con el bot, una a través de mensajes directos y otra en cada canal). Esto significa que cualquier variable que guarde en el contexto de diálogo se mantiene indefinidamente, a menos que las borre. 

Si necesita poder restablecer la conversación a un estado inicial conocido, puede hacerlo en el diálogo. Asegúrese de que el diálogo tiene un nodo que se ejecute al final de la conversación o en cualquier otro momento que deba volver a empezar. Actualice el JSON correspondiente a este nodo para restablecer todas las variables de contexto en los valores de inicio adecuados. (Si es necesario, puede utilizar las acciones **Ir a** o una intención especial "end conversation" (finalizar conversación) para ejecutar este nodo.)

Por ejemplo, si el trabajo utiliza una variable de contexto denominada `drink_order` para guardar la selección de bebidas de un usuario, puede utilizar el método `context.remove` para suprimir esta variable cuando finaliza la conversación:

```json
"context": {
   "reset_drink_order": "<?context.remove('drink_order')?>"
 }
```
{: codeblock}

Para obtener más información sobre cómo modificar valores de variables de contexto, consulte [Actualización de un valor de variable de contexto](dialog-runtime.html#context-update).

También puede borrar el contexto, o hacer otros cambios en el comportamiento del bot, editando las acciones de {{site.data.keyword.openwhisk_short}} desplegadas. Para obtener más información, consulte la documentación proporcionada en el [Repositorio GitHub ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/watson-developer-cloud/conversation-connector){: new_window}.  
