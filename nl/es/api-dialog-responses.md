---

copyright:
  years: 2015, 2019
lastupdated: "2019-05-14"

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

# Implementación de respuestas
{: #api-dialog-responses}

Un nodo de diálogo puede responder a los usuarios con una respuesta que incluya texto, imágenes o elementos interactivos, como opciones que se pueden pulsar. Si está creando su propia aplicación cliente, debe implementar la visualización correcta de cada tipo de respuesta que devuelva el diálogo. (Para obtener más información sobre respuestas de diálogo, consulte [Respuestas](/docs/services/assistant?topic=assistant-dialog-overview#dialog-overview-responses)).

## Formato de salida de las respuestas
{: #api-dialog-responses-output}

De forma predeterminada, las respuestas de un nodo de diálogo se especifican en el objeto `output.generic` en la respuesta JSON que de devuelve desde la API `/message`. El objeto `generic` contiene una matriz de un máximo de 5 elementos de respuesta que están pensados para cualquier canal. En el siguiente ejemplo de JSON se muestra una respuesta que incluye texto y una imagen.

```json
{
  "output": {
    "generic":[
      {
        "response_type": "text",
        "text": "OK, here's a picture of a dog."
      },
      {
        "response_type": "image",
        "source": "http://example.com/dog.jpg"
      }
    ],
    "text" : ["OK, here's a picture of a dog."]
  }
}
```

Como se muestra en este ejemplo, la respuesta de texto (`OK, here's a picture of a dog.`) también se devuelve en la matriz `output.text`. Esto se incluye por motivos de compatibilidad con versiones anteriores para las aplicaciones antiguas que no admiten el formato `output.generic`.
{: note}

Es responsabilidad de la aplicación cliente manejar correctamente todos los tipos de respuesta. En este caso, la aplicación tendrá que mostrar el texto y la imagen especificados al usuario.

## Tipos de respuesta
{: #api-dialog-responses-types}

Cada elemento de una respuesta es de uno de los tipos de respuesta admitidos (actualmente `image`, `option`, `pause`, `text` y `suggestion`). Cada tipo de respuesta se especifica utilizando un conjunto diferente de propiedades de JSON, de modo que las propiedades incluidas para cada respuesta variarán en función del tipo de respuesta. Para obtener información completa sobre el modelo de respuesta de la API `/message`, revise la publicación [Consulta de API ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://{DomainName}/apidocs/assistant-v2#send-user-input-to-assistant){: new_window}).

En esta sección se describen los tipos de respuesta disponibles y cómo se representan en el archivo JSON de respuesta de la API `/message`. (Si utiliza el SDK de Watson, puede utilizar las interfaces que se suministran para su idioma para acceder a los mismos objetos.)

Los ejemplos de esta sección muestran el formato de los datos JSON que se devuelven desde la `/message API` en tiempo de ejecución. Tenga en cuenta que es diferente del formato JSON utilizado para definir respuestas dentro de un nodo de diálogo. Para obtener más información, consulte [Definición de respuestas utilizando el editor JSON](/docs/services/assistant?topic=assistant-dialog-responses-json)).
{: note}

### Texto
{: #api-dialog-responses-text}

El tipo de respuesta `text` se utiliza para las respuestas de texto normales desde el diálogo:

```json
{
  "output": {
    "generic":[
      {
        "response_type": "text",
        "text": "OK, you want to fly to Boston next Monday."
      }
    ]
  }
}
```

Tenga en cuenta que, por motivos de compatibilidad con versiones anteriores, el mismo texto también se incluye en la matriz `output.text` en la respuesta del diálogo (tal como se muestra en el ejemplo del principio de esta página).

### Imagen
{: #api-dialog-responses-image}

El tipo de respuesta `image` indica a la aplicación cliente que muestre una imagen, acompañada opcionalmente por un título y una descripción:

```json
{
  "output": {
    "generic":[
      {
        "response_type": "image",
        "source": "http://example.com/image.jpg",
        "title": "Image example",
        "description": "This is an example image"
      }
    ]
  }
}
```

La aplicación es la responsable de recuperar la imagen especificada por la propiedad `source` y de mostrarla al usuario. Si se proporcionan los valores opcionales `title` y `description`, la aplicación puede mostrarlos de forma adecuada (por ejemplo, representando el título debajo de la imagen y la descripción como texto contextual).

### Pausa
{: #api-dialog-responses-pause}

El tipo de respuesta `pause` indica a la aplicación que espere un intervalo de tiempo especificado antes de mostrar la siguiente respuesta:

```json
{
  "output": {
    "generic":[
      {
        "response_type": "pause",
        "time": 500,
        "typing": false
      }
    ]
  }
}
```

Es posible que el diálogo solicite esta paula para dejar tiempo para que se complete una solicitud, o simplemente para emular la actuación de un agente humano, que puede hacer pausas entre respuestas. La pausa puede durar un máximo de 10 segundos.

La respuesta `pause` se suele enviar en combinación con otras respuestas. La aplicación debe hacer una pausa durante el intervalo de tiempo especificado por la propiedad `time` (en milisegundos) antes de mostrar la siguiente respuesta en la matriz. La propiedad opcional `typing` solicita que la aplicación cliente muestre un indicador de "el usuario está escribiendo", si se admite, para simular un agente humano.

### Opción
{: #api-dialog-responses-option}

El tipo de respuesta `option` indica a la aplicación cliente que muestre un control de interfaz de usuario que permita al usuario seleccionar entre una lista de opciones y, a continuación, enviar la entrada de nuevo al asistente en función de la opción seleccionada:

```json
{
  "output": {
    "generic":[
      {
        "response_type": "option",
        "title": "Available options",
        "description": "Please select one of the following options:",
        "preference": "button",
        "options": [
          {
            "label": "Option 1",
            "value": {
              "input": {
                "text": "option 1"
              }
            }
          },
          {
            "label": "Option 2",
            "value": {
              "input": {
                "text": "option 2"
              }
            }
          }
        ]
      }
    ]
  }
}
```

La app puede mostrar las opciones especificadas mediante cualquier control de interfaz de usuario adecuado (por ejemplo, un conjunto de botones o una lista desplegable). La propiedad opcional `preference` indica el tipo preferido de control que debe utilizar la app (`button` o `dropdown`), si se da soporte a la misma. Para una mejor experiencia del usuario, se recomienda ofrecer como botones si hay hasta tres opciones y, si hay más de tres opciones, como lista desplegable.

Para cada opción, la propiedad `label` especifica el texto de etiqueta que debe aparecer para la opción en el control de la interfaz de usuario. La propiedad `value` especifica la entrada que se debe devolver al asistente (usando la API `/message`) cuando el usuario selecciona la opción correspondiente.

Para ver un ejemplo de implementación de respuestas de `option` en una aplicación de cliente sencilla, consulte [Ejemplo: implementación de respuestas de opciones](#api-dialog-option-example).

### Suggestion (Recomendación)
{: #api-dialog-responses-suggestion}

Esta característica solo está disponible para los usuarios de los planes Plus o Premium.
{: tip}

El tipo de respuesta `suggestion` lo utiliza la característica de desambiguación para sugerir posibles coincidencias cuando no está claro lo que el usuario quiere hacer. Una respuesta de `suggestion` incluye una matriz `suggestions`, cada una de ellas correspondiente a un posible nodo de diálogo coincidente:

```json
{
  "output": {
    "generic":[
      {
        "response_type": "suggestion",
        "title": "Did you mean:",
        "suggestions": [
          {
            "label": "I'd like to order a drink.",
            "value": {
              "intents": [
                {
                  "intent": "order_drink",
                  "confidence": 0.7330395221710206
                }
              ],
              "entities": [],
              "input": {
                "suggestion_id": "576aba3c-85b9-411a-8032-28af2ba95b13",
                "text": "I want to place an order"
              }
            },
  "output": {
              "text": [
                "I'll get you a drink."
              ],
              "generic": [
                {
                  "response_type": "text",
                  "text": "I'll get you a drink."
                }
              ],
              "nodes_visited_details": [
                {
                  "dialog_node": "node_1_1547675028546",
                  "title": "order drink",
                  "user_label": "I'd like to order a drink.",
                  "conditions": "#order_drink"
                }
              ]
            },
            "source_dialog_node": "root"
          },
          {
            "label": "I need a drink refill.",
            "value": {
              "intents": [
                {
                  "intent": "refill_drink",
                  "confidence": 0.2529746770858765
                }
              ],
              "entities": [],
              "input": {
                "suggestion_id": "6583b547-53ff-4e7b-97c6-4d062270abcd",
                "text": "I need a drink refill"
              }
            },
  "output": {
              "text": [
                "I'll get you a refill."
              ],
              "generic": [
                {
                  "response_type": "text",
                  "text": "I'll get you a refill."
                }
              ],
              "nodes_visited_details": [
                {
                  "dialog_node": "node_2_1547675097178",
                  "title": "refill drink",
                  "user_label": "I need a drink refill.",
                  "conditions": "#refill_drink"
                }
              ]
            },
            "source_dialog_node": "root"
          }
        ]
      }
    ],
  }
}
```

Tenga en cuenta que la estructura de una respuesta de `suggestion` es muy similar a la estructura de una respuesta `option`. Al igual que con las opciones, cada sugerencia incluye una `label` (etiqueta) que se puede visualizar al usuario y un `valor` que especifica la entrada que debe devolver al asistente si el usuario elige la sugerencia correspondiente. Para implementar las respuestas de `sugerencia` en la aplicación, puede utilizar el mismo enfoque que utilizaría para las respuestas de `opción`.

Para obtener más información sobre la función de desambiguación, consulte [Desambiguación](/docs/services/assistant?topic=assistant-dialog-runtime#dialog-runtime-disambiguation).

## Ejemplo: Implementación de respuestas de opciones
{: #api-dialog-option-example}

Para mostrar cómo una aplicación cliente puede manejar respuestas de opciones, que solicita al usuario que seleccione de entre una lista de opciones, se puede ampliar el ejemplo de cliente descrito en [Creación de una aplicación cliente](/docs/services/assistant?topic=assistant-api-client). Se trata de una app cliente simplificada que utiliza la entrada y salida estándar para manejar tres intenciones (enviando un mensaje de bienvenida, mostrando la hora actual y saliendo de la app):

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

Si quiere probar el código de ejemplo que se muestra en este tema, antes debe configurar el espacio de trabajo necesario y obtener los detalles de la API que necesitará. Para obtener más información, consulte
[Creación de una aplicación cliente](/docs/services/assistant?topic=assistant-api-client).
{: note}

### Recepción de una respuesta de opción

La respuesta `opción` se puede utilizar cuando quiera ofrecer al usuario una lista finita de opciones, en lugar de interpretar la entrada de lenguaje natural. Esto se puede utilizar en cualquier situación en la que quiera permitir al usuario seleccionar rápidamente un conjunto de opciones sin ambigüedades.

En nuestra app cliente simplificada, usaremos esta capacidad para seleccionar de una lista de las acciones que admite el asistente (saludo, mostrar la hora y salir). Además de las tres intenciones anteriores (`#hello`, `#time` y `#goodbye`), el espacio de trabajo de ejemplo admite una cuarta intención: `#menu`, que se activa cuando el usuario solicita ver la lista de acciones disponibles.

Cuando el espacio de trabajo reconoce la intención `#menu`, el diálogo responde con una respuesta `option`:

```json
{
  "output": {
    "generic":[
      {
        "title": "What do you want to do?",
        "options": [
          {
            "label": "Send greeting",
            "value": {
              "input": {
                "text": "hello"
              }
            }
          },
          {
            "label": "Display the local time",
            "value": {
              "input": {
                "text": "time"
              }
            }
          },
          {
            "label": "Exit",
      "value": {
              "input": {
                "text": "goodbye"
              }
            }
          }
        ],
        "response_type": "option"
      }
    ],
    "intents": [
      {
        "intent": "menu",
        "confidence": 0.6178638458251953
      }
    ],
    "entities": []
  }
}
```

La respuesta de `opción` contiene varias opciones que se presentarán al usuario. Cada opción incluye dos objetos, `label` y `value`. La `etiqueta` es una serie de cara al usuario que identifica la opción; el `valor` especifica la entrada de mensaje correspondiente que se debe devolver al asistente si el usuario elige la opción.

Nuestra app cliente tendrá que utilizar los datos en esta respuesta para crear la salida que mostramos al usuario, y enviar el mensaje apropiado al asistente.

### Listado de las opciones disponibles

El primer paso en el manejo de una respuesta de opción es mostrar las opciones al usuario, utilizando el texto especificado por la propiedad `label` de cada opción. Puede visualizar las opciones utilizando cualquier técnica admitida por su aplicación, normalmente una lista desplegable o un conjunto de botones que se puedan pulsar. (La propiedad opcional `preference` de una respuesta de opción, si se especifica, indica qué tipo de visualización debe mostrar la aplicación, si es posible).

Nuestro ejemplo simplificado utiliza la entrada y salida estándar, por lo que no tenemos acceso a una interfaz de usuario real. En lugar de ello, vamos a presentar las opciones como una lista numerada.

```javascript
// Ejemplo de opción 1: listar opciones.

const prompt = require('prompt-sync')();
const AssistantV2 = require('ibm-watson/assistant/v2');

// Configurar el derivador del servicio de asistente.
const service = new AssistantV2({
  iam_apikey: '{apikey}', // sustituir por la clave de API
  version: '2019-02-28',
});

const assistantId = '{assistant_id}'; // sustituir por el ID de asistente
let sessionId;

// Crear sesión.
service
  .createSession({
    assistant_id: assistantId,
  })
  .then(res => {
    sessionId = res.session_id;
    sendMessage({text: ''}); // Iniciar la conversación con el mensaje vacío
  })
  .catch(err => {
    console.log(err); // algo ha ido mal
  });

// Enviar mensaje al asistente.
function sendMessage(messageInput) {
  service
    .message({
      assistant_id: assistantId,
      session_id: sessionId,
      input: messageInput,
    })
    .then(res => {
      processResponse(res);
    })
    .catch(err => {
      console.log(err); // algo ha ido mal
    });
}

// Procesar la respuesta.
function processResponse(response) {

  let endConversation = false;

  // Comprobar si hay acciones cliente solicitadas por el asistente.
  if (response.output.actions) {
    if (response.output.actions[0].type === 'client'){
      if (response.output.actions[0].name === 'display_time') {
        // El usuario ha preguntado qué hora es, así que devolvemos como salida la hora del sistema local.
        console.log('The current time is ' + new Date().toLocaleTimeString() + '.');
      } else if (response.output.actions[0].name === 'end_conversation') {
        // El usuario se ha despedido, así que hemos terminado.
        console.log(response.output.generic[0].text);
        endConversation = true;
      }
    }
  } else {
    // Mostrar la salida del asistente, si la hay. Solo admite
    // una única respuesta.
    if (response.output.generic) {
      if (response.output.generic.length > 0) {
        switch (response.output.generic[0].response_type) {
          case 'text':
            // Es una respuesta de texto, así que solo hay que mostrarla.
            console.log(response.output.generic[0].text);
            break;
          case 'option':
            // Es una respuesta de opciones, por lo que hay que mostrar al usuario
            // una lista de opciones.
            console.log(response.output.generic[0].title);
            const options = response.output.generic[0].options;
            // Listar las opciones por etiqueta.
            for (let i = 0; i < options.length; i++) {
              console.log((i+1).toString() + '. ' + options[i].label);
            }
            break;
        }
      }
    }
  }

  // Si no hemos terminado, solicitar la siguiente ronda de información de entrada.
  if (!endConversation) {
    const newMessageFromUser = prompt('>> ');
    newMessageInput = {
      message_type: 'text',
      text: newMessageFromUser
    }
    sendMessage(newMessageInput);
  } else {
    // Hemos terminado, así que suprimimos la sesión.
    service
      .deleteSession({
        assistant_id: assistantId,
        session_id: sessionId,
      })
      .then(res => {
        return;
      })
      .catch(err => {
        console.log(err); // algo ha ido mal
      });
  }
}
```
{: codeblock}
{: javascript}

Echemos un vistazo más detallado al código de salida de la respuesta desde el asistente. Ahora, en lugar de presuponer una respuesta de `texto`, la aplicación admite los tipos de respuesta `text` y `option`:

```javascript
    // Mostrar la salida del asistente, si la hay. Solo admite
    // una única respuesta.
    if (response.output.generic) {
      if (response.output.generic.length > 0) {
        switch (response.output.generic[0].response_type) {
          case 'text':
            // Es una respuesta de texto, así que solo hay que mostrarla.
            console.log(response.output.generic[0].text);
            break;
          case 'option':
            // Es una respuesta de opciones, por lo que hay que mostrar al usuario
            // una lista de opciones.
            console.log(response.output.generic[0].title);
            const options = response.output.generic[0].options;
            // Listar las opciones por etiqueta.
            for (let i = 0; i < options.length; i++) {
              console.log((i+1).toString() + '. ' + options[i].label);
            }
            break;
        }
      }
    }
```
{: codeblock}
{: javascript}

Si `response_type`=`text`, solo se muestra la salida, como antes. Pero si `response_type`=`option`, hay que hacer más cosas. En primer lugar, visualizamos el valor de la propiedad `title`, que sirve como texto de inicio para introducir la lista de opciones; a continuación, enumeramos las opciones, utilizando el valor de la propiedad `label` para identificar cada una de ellas. (Una aplicación en el mundo real mostraría estas etiquetas en una lista desplegable o como las etiquetas en botones que se pueden pulsar).

Puede ver el resultado activando la intención `#menu`:

```
Welcome to the Watson Assistant example! (Bienvenido al ejemplo de Watson Assistant)
>> what are the available actions? (¿qué acciones hay disponibles?)
What do you want to do? (¿Qué quiere hacer?)
1. Send greeting (Enviar saludo)
2. Display the local time (Mostrar la hora local)
3. Exit (Salir)
>> 2
Sorry, I have no idea what you're talking about. (Lo siento, pero no le he entendido).
>>
```
{: screen}

Como puede ver, la aplicación ahora está manejando correctamente la respuesta `option` listando las opciones disponibles. Sin embargo, todavía no estamos traduciendo la elección del usuario en una entrada significativa.

### Selección de una opción

Además de `label` (etiqueta), cada opción de la respuesta también incluye un objeto `value`, que contiene datos de entrada que se deben devolver al asistente si el usuario elige la opción correspondiente. El objeto `value.input` es equivalente a la propiedad `input` de la API `/message`, lo que significa que podemos enviar este objeto de nuevo a la asistente tal cual.

Para hacerlo en nuestra aplicación, estableceremos un nuevo distintivo `promptOption` cuando el cliente reciba una respuesta `option`. Cuando este distintivo es verdadero, sabemos que queremos aceptar la siguiente ronda de entrada de `value.input` en lugar de aceptar la entrada de texto de lenguaje natural del usuario. (De nuevo, no tenemos una interfaz de usuario real, así que simplemente pedimos al usuario que seleccione una opción válida de la lista por número).

```javascript
// Ejemplo de opción 2: devuelve el valor de la opción seleccionada.

const prompt = require('prompt-sync')();
const AssistantV2 = require('ibm-watson/assistant/v2');

// Configurar el derivador del servicio de asistente.
const service = new AssistantV2({
  iam_apikey: 'AZkSnK4b40UI5kLepHKxIKYpVcxeg0yPLbVVwGFW8kjM', // sustituir por clave de API
  version: '2019-02-28',
});

const assistantId = 'dcd5c5ad-f3a1-4345-89c5-708b0b5ff4f7'; // sustituir por ID de asistente
let sessionId;

// Crear sesión.
service
  .createSession({
    assistant_id: assistantId,
  })
  .then(res => {
    sessionId = res.session_id;
    sendMessage({text: ''}); // iniciar la conversación con el mensaje vacío
  })
  .catch(err => {
    console.log(err); // algo ha ido mal
  });

// Enviar mensaje al asistente.
function sendMessage(messageInput) {
  service
    .message({
      assistant_id: assistantId,
      session_id: sessionId,
      input: messageInput,
    })
    .then(res => {
      processResponse(res);
    })
    .catch(err => {
      console.log(err); // algo ha ido mal
    });
}

// Procesar la respuesta.
function processResponse(response) {

  let endConversation = false;
  let promptOption = false;

  // Comprobar si hay acciones cliente solicitadas por el asistente.
  if (response.output.actions) {
    if (response.output.actions[0].type === 'client'){
      if (response.output.actions[0].name === 'display_time') {
        // El usuario ha preguntado qué hora es, así que devolvemos como salida la hora del sistema local.
        console.log('The current time is ' + new Date().toLocaleTimeString() + '.');
      } else if (response.output.actions[0].name === 'end_conversation') {
        // El usuario se ha despedido, así que hemos terminado.
        console.log(response.output.generic[0].text);
        endConversation = true;
      }
    }
  } else {
    // Mostrar la salida del asistente, si la hay. Solo admite
    // una única respuesta.
    if (response.output.generic) {
      if (response.output.generic.length > 0) {
        switch (response.output.generic[0].response_type) {
          case 'text':
            // Es una respuesta de texto, así que solo hay que mostrarla.
            console.log(response.output.generic[0].text);
            break;
          case 'option':
              // Es una respuesta de opciones, por lo que hay que mostrar al usuario
              // una lista de opciones.
              console.log(response.output.generic[0].title);
              const options = response.output.generic[0].options;
              // Listar las opciones por etiqueta.
              for (let i = 0; i < options.length; i++) {
                console.log((i+1).toString() + '. ' + options[i].label);
              }
            promptOption = true;
            break;
        }
      }
    }
  }

  // Si no hemos terminado, solicitar la siguiente ronda de información de entrada.
  if (!endConversation) {
    let messageInput;
    if (promptOption == true) {
      // Solicitar una selección válida de la lista de opciones.
      let choice;
      do {
        choice = prompt('? ');
          if (isNaN(choice)) {
            choice = 0;
          }
      } while (choice < 1 || choice > response.output.generic[0].options.length);
      const value = response.output.generic[0].options[choice-1].value;
      // Utilizar la entrada de mensaje de la opción seleccionada.
      messageInput = value.input;
    } else {
      // No mostramos opciones, por lo que únicamente pedimos la
      // siguiente ronda de entrada.
      const newText = prompt('>> ');
      messageInput = {
        text: newText
      }
    }
    sendMessage(messageInput);
  } else {
    // Hemos terminado, así que suprimimos la sesión.
    service
      .deleteSession({
        assistant_id: assistantId,
        session_id: sessionId,
      })
      .then(res => {
        return;
      })
      .catch(err => {
        console.log(err); // algo ha ido mal
      });
  }
}
```
{: codeblock}
{: javascript}

Todo lo que tenemos que hacer es utilizar el objeto `value.input` de la respuesta seleccionada como la siguiente ronda del mensaje de entrada, en lugar de crear un nuevo objeto `input` utilizando el texto de entrada. El asistente responderá exactamente como si el usuario hubiera tecleado directamente el texto de entrada.

```
Welcome to the Watson Assistant example! (Bienvenido al ejemplo de Watson Assistant)
>> hi (hola)
Good day to you. (Buenos días.)
>> what are the choices? (¿qué opciones hay?)
What do you want to do? (¿Qué quiere hacer?)
1. Send greeting (Enviar saludo)
2. Display the local time (Mostrar la hora local)
3. Exit (Salir)
? 2
The current time is 1:29:14 PM. (Son las 13:29:14)
>> bye (adiós)
OK! See you later. (Muy bien. Hasta pronto.)
```
{: screen}

Ahora podemos acceder a todas las funciones del asistente ya sea haciendo solicitudes en lenguaje natural o seleccionando en un menú de opciones.

Tenga en cuenta que para las respuestas de `sugerencia` se utiliza también el mismo enfoque. Si el plan admite la función de desambiguación, puede utilizar lógica similar para solicitar a los usuarios que seleccionen en una lista cuando no está claro cuál de las distintas opciones posibles es correcta. Para obtener más información sobre la función de desambiguación, consulte [Desambiguación](/docs/services/assistant?topic=assistant-dialog-runtime#dialog-runtime-disambiguation).
