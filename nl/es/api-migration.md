---

copyright:
  years: 2015, 2019
lastupdated: "2019-07-30"

subcollection: assistant


---

{:curl: #curl .ph data-hd-programlang='curl'}
{:external: target="_blank" .external}
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

# Migración a la API v2
{: #api-migration}

La API de tiempo de ejecución Assistant v2, que da soporte al uso de asistentes y conocimientos, se introdujo en noviembre de 2018. Esta API ofrece ventajas significativas con respecto a la API de tiempo de ejecución v1, incluyendo la gestión automática del estado, la facilidad de despliegue, el mantenimiento de versiones de conocimientos y la disponibilidad de nuevas funciones, como el conocimiento de búsqueda.

La API v2 está disponible para todos los usuarios, independientemente del plan de servicio, sin ningún coste adicional.

La API v2 actualmente solo admite la interacción de tiempo de ejecución con un asistente existente. Las aplicaciones de creación que crean o modifican espacios de trabajo deben continuar utilizando la API v1.
{: note}

## Descripción general

Con la API v2, la app cliente se comunica con un asistente, en lugar de directamente con un espacio de trabajo. Un asistente es una nueva capa de orquestación que ofrece varias prestaciones nuevas, incluyendo la gestión automática de estados, el mantenimiento de versiones de conocimientos, el despliegue más fácil y los conocimientos de búsqueda (para Plus y Premium). El espacio de trabajo existente (ahora conocido como _conocimiento de diálogo_) continúa funcionando como antes, pero las nuevas prestaciones las proporciona la nueva capa de asistente.

Toda la comunicación con un asistente tiene lugar dentro del contexto de una _sesión_, que mantiene el estado de conversación todo el tiempo que dure la conversación. Los datos de estado, incluidas las variables de contexto definidas por su aplicación de diálogo o cliente, se almacenan de forma automática mediante {{site.data.keyword.conversationshort}}, sin que sea necesaria acción alguna por parte de su aplicación.

Los datos de estado persisten hasta que se suprime explícitamente la sesión, o hasta que la sesión excede el tiempo de espera debido a la inactividad.

Si tiene una aplicación existente que utiliza la API v1 para enviar la entrada de usuario directamente a un espacio de trabajo, la migración de su aplicación para que utilice la API v2 es un proceso directo.

## Configuración de un asistente

La API de tiempo de ejecución de v2 envía mensajes a un asistente, que direcciona los mensajes a su conocimiento de diálogo (anteriormente espacio de trabajo). Para configurar un asistente, utilice la interfaz de usuario de {{site.data.keyword.conversationshort}}:

1. Pulse el separador **Conocimientos**. Verifique que el espacio de trabajo se muestra como un conocimiento disponible. (Todos los espacios de trabajo existentes para la instancia de servicio se convierten automáticamente a conocimientos en la interfaz de usuario de {{site.data.keyword.conversationshort}}. Esta conversión no realiza ningún cambio en el espacio de trabajo subyacente).

1. Pulse el separador **Asistentes**. Pulse **Crear asistente** para crear un asistente nuevo. Cuando se le solicite añadir conocimientos, pulse **Añadir conocimiento de diálogo** y seleccione el conocimiento de diálogo que corresponde a su espacio de trabajo.

  Para obtener más información sobre la creación de asistentes, consulte [Creación de un asistente](https://cloud.ibm.com/docs/services/assistant?topic=assistant-assistant-add).

1. Una vez que se haya creado el nuevo asistente, pulse el menú ![Menu](images/kebab-react.png) y, a continuación, seleccione **Valores**.

1. En la página **Valores de asistente**, busque el ID de asistente. La aplicación utilizará este ID (en lugar de un ID de espacio de trabajo) para comunicarse con el asistente. Las credenciales de servicio son las mismas para las API de v1 y v2.

  Actualmente, no hay soporte de API para recuperar un ID de asistente. Para encontrar el ID de asistente, debe utilizar la interfaz de usuario de {{site.data.keyword.conversationshort}}.
  {: note}

## Llamar a la API de ejecución v2

Después de haber creado un asistente, puede actualizar la aplicación cliente para que utilice la API de tiempo de ejecución v2 en lugar de la API de tiempo de ejecución v1.

1. Antes de enviar el primer mensaje en una conversación, utilice el método [**Crear una sesión**](https://cloud.ibm.com/apidocs/assistant-v2#create-a-session){: external} de v2 para crear una sesión. Guarde el ID de sesión devuelto:

  ```javascript
  service
  .createSession({
    assistant_id: assistantId,
  })
  .then(res => {
    sessionId = res.session_id;
  })
  ```
  {: codeblock}
  {: javascript}

  ```python
  session_id = service.create_session(
    assistant_id = assistant_id
).get_result()['session_id']
  ```
  {: codeblock }
  {: python }

  ```java
  CreateSessionOptions createSessionOptions = new CreateSessionOptions.Builder(assistantId).build();
  SessionResponse session = service.createSession(createSessionOptions).execute().getResult();
  String sessionId = session.getSessionId();
  ```
  {: codeblock}
  {: java}

1. Utilice el método [**Enviar entrada de usuario a asistente**](https://cloud.ibm.com/apidocs/assistant-v2#send-user-input-to-assistant){: external} de v2 para enviar la entrada de usuario al asistente. En lugar de especificar el ID de espacio de trabajo tal como se hacía con la API v1, especifique el ID de asistente y el ID de sesión:

  ```javascript
  service
    .message({
      assistant_id: assistantId,
      session_id: sessionId,
      input: messageInput
    })
  ```
  {: codeblock}
  {: javascript}

  ```python
  response = service.message(
      assistant_id,
      session_id,
      input = message_input
  ).get_result()
  ```
  {: codeblock}
  {: python}

  ```java
  MessageInput input = new MessageInput.Builder().text(inputText).build();
      MessageOptions messageOptions = new MessageOptions.Builder(assistantId, sessionId)
    .input(input)
    .build();
  MessageResponse response = service.message(messageOptions)
    .execute()
    .getResult();
  ```
  {: codeblock}
  {: java}

  La estructura de mensajes básica no se ha modificado; concretamente, la entrada de usuario todavía se envía como `input.text`.

1. Después de finalizar una conversación, utilice el método [**Suprimir sesión**](https://cloud.ibm.com/apidocs/assistant-v2#delete-session){: external} de v2 para suprimir la sesión.

  ```javascript
  service
    .deleteSession({
      assistant_id: assistantId,
      session_id: sessionId,
    })
  ```
  {: codeblock}
  {: javascript}

  ```python
  service.delete_session(
    assistant_id = assistant_id,
    session_id = session_id
)
  ```
  {: codeblock}
  {: python}

  ```java
  DeleteSessionOptions deleteSessionOptions = new DeleteSessionOptions.Builder(assistantId, sessionId
    .build();
  service.deleteSession(deleteSessionOptions).execute();
  ```
  {: codeblock}
  {: java}

   Si no suprime explícitamente la sesión, se suprimirá automáticamente después del intervalo de tiempo de espera configurado. (La duración del tiempo de espera depende de su plan; para obtener más información, consulte [Límites de sesión](/docs/services/assistant?topic=assistant-assistant-settings#assistant-settings-session-limits)).

Para ver ejemplos de las API v2 en el contexto de una aplicación de cliente sencilla, consulte [Creación de una aplicación cliente](/docs/services/assistant?topic=assistant-api-client).

## Gestión del formato de respuesta de v2

Es posible que haya que actualizar la aplicación para que trabaje con el formato de respuesta de tiempo de ejecución v2, en función de las partes de la respuesta a la que la aplicación tenga que acceder:

- La salida para todos los tipos de respuesta (por ejemplo, `text` y `option`) todavía se devuelve en el objeto `output.generic`. El código de aplicación para la gestión de estas respuestas debe funcionar sin necesidad de modificaciones.

- Los datos y las entidades detectados se devuelven ahora como parte del objeto `output`, en lugar de hacerlo en la raíz del JSON de respuesta.

- El contexto de conversación está ahora organizado en dos objetos:

  - El **contexto global** contiene los datos de contexto a nivel de sistema compartidos por todos los conocimientos utilizados por el asistente.

  - El **contexto de conocimiento** contiene las variables de contexto definidas por el usuario utilizadas por el conocimiento del diálogo.

  Sin embargo, tenga en cuenta que los datos de estado, incluido el contexto de conversación, los mantiene el asistente, por lo que es posible que la aplicación no tenga que acceder al contexto en absoluto (consulte [Dejar que el asistente mantenga el estado](#api-migration-state)).

Consulte la publicación [API Reference ](https://cloud.ibm.com/apidocs/assistant-v2#send-user-input-to-assistant){: external} de v2 para obtener la documentación completa del formato de respuesta en v2.

## Dejar que el asistente mantenga el estado
{: #api-migration-state}

Para la mayoría de las aplicaciones, ahora puede eliminar cualquier código incluido con el fin de mantener el estado. Ya no es necesario guardar el contexto y enviarlo de nuevo a {{site.data.keyword.conversationshort}} con cada turno de la conversación. El contexto lo mantiene automáticamente {{site.data.keyword.conversationshort}} y se puede acceder mediante el cuadro de diálogo, como antes.

Tenga en cuenta que con la API v2, el contexto, de forma predeterminada, no se incluye en las respuestas a la aplicación cliente. Sin embargo, el código todavía puede acceder a las variables de contexto, si es necesario:

- Todavía puede enviar un objeto `context` como parte de la entrada del mensaje. Todas las variables de contexto que incluya se almacenan como parte del contexto que mantiene {{site.data.keyword.conversationshort}}. (Si la variable de contexto que envía ya existe en el contexto, el nuevo valor sobrescribe el valor almacenado anteriormente).

  Asegúrese de que el objeto de contexto que envía se ajusta al formato v2. Todas las variables de contexto definidas por el usuario enviadas por la aplicación deben formar parte del contexto de conocimientos; normalmente, la única variable de contexto global que es posible que tenga que establecer es `system.user_id`, usada por los planes Plus y Premium para la facturación.

- Puede seguir recuperando variables de contexto desde el contexto global o el contexto del conocimiento. Para que el objeto `context` se incluya con respuestas de mensajes, utilice la propiedad **return_context** en las opciones de entrada de mensaje. Para obtener más información, consulte [Acceso a datos de contexto](/docs/services/assistant?topic=assistant-api-client-get-context).
