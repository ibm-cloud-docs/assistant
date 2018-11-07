---

copyright:
  years: 2015, 2018
lastupdated: "2018-01-29"

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
{:tip: .tip}

# Creación de una aplicación cliente

Ya tiene un diálogo de trabajo. Ahora supongamos que desea desarrollar la aplicación que va a interactuar con los usuarios y a comunicarse con el servicio {{site.data.keyword.conversationfull}}.
{: shortdesc}

Puede visualizar esta guía de aprendizaje para Node.js (Javascript) o Python pulsando el selector de idioma en la parte superior derecha. Para obtener información más detallada sobre los idiomas soportados, consulte los {{site.data.keyword.watson}} [SDK ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](/docs/services/watson/getting-started-sdks.html#sdks){: new_window}.
{: tip }

## Configuración del servicio {{site.data.keyword.conversationshort}} 

La aplicación de ejemplo que se creará en esta sección implementa varias funciones de un asistente personal cognitivo. El código de la aplicación se conectará a un espacio de trabajo de {{site.data.keyword.conversationshort}}, donde tiene lugar el proceso cognitivo (como por ejemplo la detección de intenciones del usuario).

Antes de continuar con este ejemplo, debe configurar el espacio de trabajo de {{site.data.keyword.conversationshort}} necesario:

1.  Descargue el <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/conversation/conversation-simple-example.json" download="conversation-simple-example.json">archivo JSON</a> del espacio de trabajo.
1.  [Importe el espacio de trabajo](/docs/services/conversation/configure-workspace.html#creating-workspaces) en una instancia del servicio {{site.data.keyword.conversationshort}}.

## Obtención de información del servicio

Para acceder a las API REST del servicio {{site.data.keyword.conversationshort}}, la aplicación debe ser capaz de autenticares con {{site.data.keyword.Bluemix}} y de conectarse al espacio de trabajo de {{site.data.keyword.conversationshort}} adecuado. Tendrá que copiar las credenciales del servicio y el ID del espacio de trabajo y pegarlos en el código de la aplicación.

Para acceder a las credenciales del servicio y al ID del espacio de trabajo desde su espacio de trabajo, seleccione el menú ![Menú](images/Menu_16.png), seleccione **Desplegar** y luego vaya al separador **Credenciales**.

También puede acceder a las credenciales del servicio desde el panel de control de {{site.data.keyword.Bluemix_short}}. 

## Comunicación con el servicio {{site.data.keyword.conversationshort}} 

Interactuar con el servicio {{site.data.keyword.conversationshort}} es sencillo. Examine el ejemplo que conecta al servicio, envía un mensaje individual e imprime la salida en la consola: 

```javascript
// Ejemplo 1: configura el derivador del servicio, envía el mensaje inicial y
// recibe respuesta.

var ConversationV1 = require('watson-developer-cloud/conversation/v1');

// Configurar el derivador del servicio Conversation.
var conversation = new ConversationV1({
  username: 'USERNAME', // sustituir con el nombre de usuario de servicio
  password: 'PASSWORD', // sustituir con la contraseña de servicio
  version_date: '2017-05-26'
});

var workspace_id = 'WORKSPACE_ID'; // sustituir con el ID de espacio de trabajo

// Iniciar conversación con mensaje vacío.
conversation.message({
  workspace_id: workspace_id
  }, processResponse);

// Procesar la respuesta de la conversación.
function processResponse(err, response) {
  if (err) {
    console.error(err); // algo ha fallado
    return;
  }

  // Visualiza la salida del diálogo, si hay.
  if (response.output.text.length != 0) {
      console.log(response.output.text[0]);
  }
}
```
{: codeblock}
{: javascript}

```python
# Ejemplo 1: configura el derivador del servicio, envía el mensaje inicial y
# recibe respuesta.

import watson_developer_cloud

# Configura el servicio Conversation.
conversation = watson_developer_cloud.ConversationV1(
  username = 'USERNAME', # sustituya con el nombre de usuario de la clave de servicio
  password = 'PASSWORD', # sustituya con la contraseña de la clave de servicio
  version = '2017-05-26'
)
workspace_id = 'WORKSPACE_ID' # sustituya con el ID de espacio de trabajo

# Iniciar conversación con mensaje vacío.
response = conversation.message(
  workspace_id = workspace_id,
  input = {
    'text': ''
  }
)

# Imprime la salida del diálogo, si hay.
if response['output']['text']:
  print(response['output']['text'][0])
```
{: codeblock}
{: python}

El primer paso consiste en crear un derivador para el servicio {{site.data.keyword.conversationshort}}.

El derivador es un objeto que utilizará para enviar información de entrada al servicio y para recibir la salida de este. Cuando cree el derivador del servicio, especifique las credenciales de autenticación de la clave de servicio, así como la versión de la API de {{site.data.keyword.conversationshort}} que está utilizando.

En este ejemplo de Node.js, el derivador es una instancia de `ConversationV1`, almacenada en la variable `conversation`. Los Watson SDK de otros lenguajes proporcionan mecanismos equivalentes para crear instancias de un derivador de servicio.
{: javascript}

En este ejemplo de Python, el derivador es una instancia de `watson_developer_cloud.ConversationV1`, almacenada en la variable `conversation`. Los Watson SDK de otros lenguajes proporcionan mecanismos equivalentes para crear instancias de un derivador de servicio.
{: python}

Después de crear el derivador del servicio, lo utilizaremos que enviar un mensaje al servicio {{site.data.keyword.conversationshort}}. En este ejemplo, el mensaje está vacío; sólo queremos activar el nodo conversation_start en el diálogo, de modo que no necesitamos ningún texto de entrada.

Utilice el mandato `node <filename.js>` para ejecutar la aplicación de ejemplo.
{: javascript}

Utilice el mandato `python <filename.py>` para ejecutar la aplicación de ejemplo.
{: python}

**Nota:** Asegúrese de haber instalado Watson SDK for Node.js utilizando `npm install watson-developer-cloud`.
{: javascript}

**Nota:** Asegúrese de haber instalado Watson SDK for Python utilizando `pip install --upgrade watson-developer-cloud` o `easy_install --upgrade watson-developer-cloud`.
{: python}

Si todo funciona según lo esperado, el servicio {{site.data.keyword.conversationshort}} devuelve la salida del diálogo, que luego se imprime en la consola:

```
Welcome to the {{site.data.keyword.conversationshort}} example!
```
{: screen}

Esta salida nos indica que nos hemos comunicado correctamente con el servicio {{site.data.keyword.conversationshort}} y hemos recibido el mensaje especificado por el nodo conversation_start en el diálogo. Ahora podemos añadir una interfaz de usuario, lo que permitirá procesar entrada de usuario.

## Proceso de información de entada de usuario para detectar intenciones

Para poder procesar la entrada de usuario, debemos añadir una interfaz de usuario a nuestra aplicación. En este ejemplo, vamos a simplificar y utilizaremos una entrada y salida estándares. 
<span class="ph style-scope doc-content" data-hd-programlang="javascript">Para ello utilizaremos el módulo prompt-sync de Node.js. (Puede instalar prompt-sync utilizando `npm install prompt-sync`.)</span>
<span class="ph style-scope doc-content" data-hd-programlang="python">Se puede utilizar la función `input` de Python para hacer esto. </span>
```javascript
// Ejemplo 2: añade información de entrada del usuario y detecta intenciones.

var prompt = require('prompt-sync')();
var ConversationV1 = require('watson-developer-cloud/conversation/v1');

// Configurar el derivador del servicio Conversation.
var conversation = new ConversationV1({
  username: 'USERNAME', // sustituir con el nombre de usuario de servicio
  password: 'PASWORD', // sustituir con la contraseña de servicio
  version_date: '2017-05-26'
});

var workspace_id = 'WORKSPACE_ID'; // sustituir con el ID de espacio de trabajo

// Iniciar conversación con mensaje vacío.
conversation.message({
  workspace_id: workspace_id
  }, processResponse);

// Procesar la respuesta de la conversación.
function processResponse(err, response) {
  if (err) {
    console.error(err); // algo ha fallado
    return;
  }

  // Si se detecta una intención, registrarla en la consola.
  if (response.intents.length > 0) {
    console.log('Detected intent: #' + response.intents[0].intent);
  }

  // Mostrar la salida del diálogo, si la hay.
  if (response.output.text.length != 0) {
      console.log(response.output.text[0]);
  }

  // Solicitar la siguiente ronda de información de entrada.
  var newMessageFromUser = prompt('>> ');
  conversation.message({
    workspace_id: workspace_id,
    input: { text: newMessageFromUser }
    }, processResponse)
}
```
{: codeblock}
{: javascript}

```python
# Ejemplo 2: añade información de entrada del usuario y detecta intenciones.

import watson_developer_cloud

# Configura el servicio Conversation.
conversation = watson_developer_cloud.ConversationV1(
  username = 'USERNAME', # sustituya con el nombre de usuario de la clave de servicio
  password = 'PASSWORD', # sustituya con la contraseña de la clave de servicio
  version = '2017-05-26'
)
workspace_id = 'WORKSPACE_ID' # sustituya con el ID de espacio de trabajo

# Inicializar con un valor vacío para empezar la conversación.
user_input = ''

# Bucle de entrada/salida principal
while True:

  # Envía un mensaje al servicio Conversation.
  response = conversation.message(
    workspace_id = workspace_id,
    input = {
      'text': user_input
    }
  )

  # Si se detecta una intención, imprimirla en la consola.
  if response['intents']:
    print('Detected intent: #' + response['intents'][0]['intent'])

  # Imprime la salida del diálogo, si hay.
  if response['output']['text']:
    print(response['output']['text'][0])

  # Solicitud para la siguiente ronda de entrada.
  user_input = input('>> ')
```
{: codeblock }
{: python }

Esta versión de la aplicación empieza igual que antes: enviando un mensaje vacío al servicio {{site.data.keyword.conversationshort}} para iniciar la conversación.

Ahora la función `processResponse()` muestra las intenciones que detecta el diálogo junto con el texto de salida, y luego solicita la siguiente ronda de información de entrada de usuario.
{: javascript }

A continuación, visualiza las intenciones que el diálogo ha detectado junto con el texto de salida y, a continuación, solicita la siguiente ronda de la entrada del usuario. (Se utiliza un bucle `while True` por ahora, puesto que todavía no se ha implementado una forma de finalizar la conversación).
{: python }

Pero aún hay algo que no es correcto:

```
Welcome to the {{site.data.keyword.conversationshort}} example!
>> hello
Detected intent: #hello
Welcome to the {{site.data.keyword.conversationshort}} example!
>> what time is it?
Detected intent: #time
Welcome to the {{site.data.keyword.conversationshort}} example!
>> goodbye
Detected intent: #goodbye
Welcome to the {{site.data.keyword.conversationshort}} example!
>>
```
{: screen}

El servicio {{site.data.keyword.conversationshort}} detecta las intenciones correctas, y cada ronda de la conversación devuelve el mensaje de bienvenida procedente del nodo conversation_start (`Welcome to the {{site.data.keyword.conversationshort}} example!`).

Esto se debe a que el servicio {{site.data.keyword.conversationshort}} no tiene estado; es responsabilidad de la aplicación mantener la información de estado. Puesto que todavía no se está haciendo nada para mantener el estado, el servicio {{site.data.keyword.conversationshort}} ve cada ronda de información de entrada de usuario como la primera ronda de una nueva conversación, por lo que activa el nodo conversation_start.

## Mantenimiento del estado

La información de estado de la conversación se mantiene mediante un *contexto*. El contexto es un objeto JSON que se transfiere entre la aplicación y el servicio {{site.data.keyword.conversationshort}} y viceversa. Es responsabilidad de la aplicación mantener el contexto entre una ronda de la conversación y la siguiente.

El contexto incluye un identificador exclusivo para cada conversación con un usuario, así como un contador que se incrementa con cada ronda de la conversación. La versión anterior del ejemplo no preservaba el contexto, lo que significa que cada ronda de entrada parecía el principio de una nueva conversación. Podemos arreglarlo guardando el contexto y enviándolo de nuevo al servicio {{site.data.keyword.conversationshort}} cada vez.

Además de mantener nuestro lugar en la conversación, el contexto también se puede utilizar para almacenar cualquier dato que desee transferir entre la aplicación y el servicio {{site.data.keyword.conversationshort}} y viceversa. Esto puede incluir datos permanente que desea mantener durante toda la conversación (como el nombre de un cliente o un número de cuenta), o cualquier otra información de la que desee realizar un seguimiento (como, por ejemplo, el estado actual de los valores de las opciones).

```javascript
// Ejemplo 3: mantiene el estado.

var prompt = require('prompt-sync')();
var ConversationV1 = require('watson-developer-cloud/conversation/v1');

// Configurar el derivador del servicio Conversation.
var conversation = new ConversationV1({
  username: 'USERNAME', // sustituir con el nombre de usuario de servicio
  password: 'PASSWORD', // sustituir con la contraseña de servicio
  version_date: '2017-05-26'
});

var workspace_id = 'WORKSPACE_ID'; // sustituir con el ID de espacio de trabajo

// Iniciar conversación con mensaje vacío.
conversation.message({
  workspace_id: workspace_id
  }, processResponse);

// Procesar la respuesta de la conversación.
function processResponse(err, response) {
  if (err) {
    console.error(err); // algo ha fallado
    return;
  }

  // Si se detecta una intención, registrarla en la consola.
  if (response.intents.length > 0) {
    console.log('Detected intent: #' + response.intents[0].intent);
  }

  // Visualiza la salida del diálogo, si hay.
  if (response.output.text.length != 0) {
      console.log(response.output.text[0]);
  }

  // Solicitar la siguiente ronda de información de entrada.
    var newMessageFromUser = prompt('>> ');
    // Enviar el contexto para mantener el estado.
    conversation.message({
      workspace_id: workspace_id,
      input: { text: newMessageFromUser },
      context : response.context,
    }, processResponse)
}
```
{: codeblock}
{: javascript }

```python
# Ejemplo 3: mantiene el estado.

import watson_developer_cloud

# Configura el servicio Conversation.
conversation = watson_developer_cloud.ConversationV1(
  username = 'USERNAME', # sustituya con el nombre de usuario de la clave de servicio
  password = 'PASSWORD', # sustituya con la contraseña de la clave de servicio
  version = '2017-05-26'
)
workspace_id = 'WORKSPACE_ID' # sustituya con el ID de espacio de trabajo

# Inicializar con un valor vacío para empezar la conversación.
user_input = ''

context = {}

# Bucle de entrada/salida principal
while True:

  # Envía un mensaje al servicio Conversation.
  response = conversation.message(
    workspace_id = workspace_id,
    input = {
      'text': user_input
    },
    context = context
  )

  # Si se detecta una intención, imprimirla en la consola.
  if response['intents']:
    print('Detected intent: #' + response['intents'][0]['intent'])

  # Imprime la salida del diálogo, si hay.
  if response['output']['text']:
    print(response['output']['text'][0])

  # Actualiza el contexto almacenado con el último recibido desde el diálogo.
  context = response['context']

  # Solicitud para la siguiente ronda de entrada.
  user_input = input('>> ')
```
{: codeblock }
{: python }

El único cambio con respecto al ejemplo anterior es que, con cada ronda de la conversación, ahora enviamos el objeto `response.context` que hemos recibido en la ronda anterior:
{: javascript }

```javascript
    conversation.message({
      input: { text: newMessageFromUser },
      context : response.context,
    }, processResponse)
```
{: codeblock}
{: javascript }

El único cambio del ejemplo anterior es que ahora el contexto recibido del diálogo se está almacenando en la variable `context`, que se envía de vuelta en la siguiente ronda de la entrada del usuario:
{: python }

```python
  response = conversation.message(
    workspace_id = workspace_id,
    input = {
      'text': user_input
    },
    context = context
  )
```
{: codeblock }
{: python }

Esto garantiza que se mantiene el contexto entre una ronda y la siguiente, de modo que el servicio {{site.data.keyword.conversationshort}} ya no piensa que cada ronda es la primera:

```
>> hello
Detected intent: #hello
Good day to you.
>> what time is it?
Detected intent: #time
>> goodbye
Detected intent: #goodbye
OK! See you later.
>>
```
{: screen}

¡Ahora estamos avanzando! El servicio {{site.data.keyword.conversationshort}} reconoce correctamente nuestras intenciones y el diálogo devuelve el texto de salida correcto (cuando se proporciona) para cada intención.

Sin embargo, no sucede nada más. Cuando preguntamos la hora, no objetemos respuesta, y cuando decimos adiós, la conversación no termina. Esto se debe a que estas intenciones requieren que la app lleve a cabo acciones adicionales.

## Implementación de acciones de la app

Además del texto de salida que se muestra al usuario, nuestro diálogo utiliza el objeto `output` en el JSON de la respuesta para indicar cuándo la aplicación debe llevar a cabo una acción, en función de las intenciones detectadas.

Estos distintivos de acción se envían mediante la propiedad `action`, que nuestro diálogo define como parte del JSON de respuesta. Cuando el diálogo determina que la aplicación debe hacer algo, establece el valor de `action` en el valor adecuado, ya sea `display_time` o `end_conversation`.

Tenga en cuenta que `output` es simplemente un objeto JSON, y puede añadir cualquier contenido válido que desee. Para una aplicación más compleja, puede utilizar una matriz con varios distintivos de acción.

Pero, en nuestro ejemplo, utilizando un sencillo par de clave/valor que da soporte a un solo distintivo de acción. Nuestro código de aplicación debe comprobar el valor de la propiedad `action` en la respuesta y llevar a cabo la acción especificada. (Esta versión también elimina la visualización de las intenciones detectadas, ahora que estamos seguros de que se han definido correctamente.)

```javascript
// Ejemplo 4: implementa acciones de la app.

var prompt = require('prompt-sync')();
var ConversationV1 = require('watson-developer-cloud/conversation/v1');

// Configurar el derivador del servicio Conversation.
var conversation = new ConversationV1({
  username: 'USERNAME', // sustituir con el nombre de usuario de servicio
  password: 'PASSWORD', // sustituir con la contraseña de servicio
  version_date: '2017-05-26'
});

var workspace_id = 'WORKSPACE_ID'; // sustituir con el ID de espacio de trabajo

// Iniciar conversación con mensaje vacío.
conversation.message({
  workspace_id: workspace_id
  }, processResponse);

// Procesar la respuesta de la conversación.
function processResponse(err, response) {
  if (err) {
    console.error(err); // algo ha fallado
    return;
  }

  var endConversation = false;

  // Comprobar los distintivos de la acción.
  if (response.output.action === 'display_time') {
    // El usuario ha preguntado qué hora es, así que devolvemos como salida la hora del sistema local.
    console.log('The current time is ' + new Date().toLocaleTimeString());
  } else if (response.output.action === 'end_conversation') {
    // El usuario se ha despedido, así que hemos terminado.
    console.log(response.output.text[0]);
    endConversation = true;
  } else {
    // Mostrar la salida del diálogo, si la hay.
    if (response.output.text.length != 0) {
        console.log(response.output.text[0]);
    }
  }

  // Si no hemos terminado, solicitar la siguiente ronda de información de entrada.
  if (!endConversation) {
    var newMessageFromUser = prompt('>> ');
    conversation.message({
      workspace_id: workspace_id,
      input: { text: newMessageFromUser },
      // Envía de nuevo el contexto para mantener el estado.
      context : response.context,
    }, processResponse)
  }
}
```
{: codeblock}
{: javascript}

```python
# Ejemplo 4: implementa acciones de la app.

import watson_developer_cloud
import time

# Configura el servicio Conversation.
conversation = watson_developer_cloud.ConversationV1(
  username = 'USERNAME', # sustituya con el nombre de usuario de la clave de servicio
  password = 'PASSWORD', # sustituya con la contraseña de la clave de servicio
  version = '2017-05-26'
)
workspace_id = 'WORKSPACE_ID' # sustituya con el ID de espacio de trabajo

# Inicializar con un valor vacío para empezar la conversación.
user_input = ''
context = {}
current_action = ''

# Bucle de entrada/salida principal
while current_action != 'end_conversation':

  # Envía un mensaje al servicio Conversation.
  response = conversation.message(
    workspace_id = workspace_id,
    input = {
      'text': user_input
    },
    context = context
  )

  # Imprime la salida del diálogo, si hay.
  if response['output']['text']:
    print(response['output']['text'][0])

  # Actualiza el contexto almacenado con el último recibido desde el diálogo.
  context = response['context']
  # Comprueba si el diálogo ha enviado distintivos de acción.
  if 'action' in response['output']:
    current_action = response['output']['action']
  # El usuario ha preguntado la hora, utilizamos para la salida la hora del sistema local.
  if current_action == 'display_time':
    print('The current time is ' + time.strftime('%I:%M:%S %p'))
  # Si no hemos terminado, solicitud para la siguiente ronda de entrada.
  if current_action != 'end_conversation':
    user_input = input('>> ')
```
{: codeblock}
{: python}

Ahora la función processResponse() comprueba el valor de la propiedad `action` del objeto `output` recibido del servicio {{site.data.keyword.conversationshort}}. Si el valor es `display_time` o `end_conversation`, la aplicación lleva a cabo la acción adecuada.
{: javascript}

Ahora la app comprueba el valor de la propiedad `action` del objeto `output` recibido del servicio {{site.data.keyword.conversationshort}}. Si el valor es `display_time`, la aplicación lleva a cabo la acción adecuada. Si el valor es `end_conversation`, la app sabe que no debe solicitar más entrada de usuario, y el bucle `while` finaliza.
{: python}

```
Welcome to the {{site.data.keyword.conversationshort}} example!
>> hello
Good day to you.
>> what time is it?
The current time is 12:40:42 PM.
>> goodbye
OK! See you later.
```
{: screen}

Correcto. Ahora la aplicación utiliza el servicio {{site.data.keyword.conversationshort}} para identificar las intenciones en la entrada de lenguaje natural, muestra las respuestas adecuadas e implementa las acciones de cliente solicitadas.

Por supuesto, una aplicación real utilizaría una interfaz de usuario más sofisticadas, como por ejemplo una ventana de conversación web. Y también implementaría acciones más complejas, posiblemente integrando una base de datos de clientes u otros sistemas empresariales. Pero los principios básicos sobre cómo la aplicación interactúa con el servicio {{site.data.keyword.conversationshort}} seguirían siendo los mismos.

Para ver ejemplos más complejos, consulte las apps de ejemplo del panel de navegación.
