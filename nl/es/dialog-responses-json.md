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

# Definición de respuestas mediante el editor de JSON
{: #dialog-responses-json}

En algunas situaciones, es posible que tenga que definir respuestas mediante el editor de JSON. (Para obtener más información sobre respuestas de diálogo, consulte [Respuestas](/docs/services/assistant?topic=assistant-dialog-overview#dialog-overview-responses)). La edición del archivo JSON de respuesta le ofrece acceso directo a los datos que se devolverán al canal de comunicación o a la aplicación personalizada.

## Formato JSON genérico
{: #dialog-responses-json-generic}

El formato de JSON genérico para respuestas se utiliza para especificar las respuestas destinadas a cualquier canal. Este formato admite diversos tipos de respuestas que reciben soporte de las integraciones Slack y Facebook, y también lo puede implementar una aplicación cliente personalizada. (Es el formato que se utiliza de forma predeterminada para las respuestas de diálogo definidas mediante la herramienta {{site.data.keyword.conversationshort}}.)

Para obtener información sobre cómo abrir el editor de JSON para una respuesta de nodo de diálogo desde la herramienta, consulte [Variables de contexto en el editor de JSON](/docs/services/assistant?topic=assistant-dialog-runtime#dialog-runtime-context-var-json).

Para especificar una respuesta interactiva en el formato JSON genérico, inserte los objetos JSON adecuados en el campo `output.generic` de
la respuesta del nodo de diálogo. En el siguiente ejemplo se muestra cómo enviar una respuesta con varios tipos de respuesta (texto, imágenes y opciones que es posible pulsar):

```json
{
  "output": {
    "generic":[
      {
        "response_type": "text",
        "values": [
          {
            "text": "Here are your nearest stores."
          }
        ]
      },
      {
        "response_type": "image",
        "source": "http://example.com/image.jpg",
        "title": "Example image",
        "description": "Some description for the image."
      },
      {
        "response_type": "option",
        "title": "Click on one of the following",
        "options": [
          {
            "label": "Location 1",
            "value": {
              "input": {
                "text": "Location 1"
              }
            }
          },
          {
            "label": "Location 2",
            "value": {
              "input": {
                "text": "Location 2"
              }
            }
          },
          {
            "label": "Location 3",
            "value": {
              "input": {
                "text": "Location 3"
              }
            }
          }
        ]
      }
    ]
  }
}
```

Para obtener más información sobre cómo especificar cada tipo de respuesta soportado mediante objetos JSON, consulte [Tipos de respuesta](#dialog-responses-json-response-types).

Si utiliza el conector {{site.data.keyword.conversationshort}}, la respuesta se convierte en tiempo de ejecución en el formato que espera el canal (Slack o Facebook Messenger). Si la respuesta contiene varios tipos de soportes o adjuntos, la respuesta genérica se convierte en una serie de cargas útiles de mensaje por separado según se necesite. A continuación, el conector envía cada mensaje de carga al canal en un mensaje separado.

**Nota:** Cuando una respuesta se divide en varios mensajes, el conector {{site.data.keyword.conversationshort}} envía estos mensajes al canal de forma secuencial. Es el canal quien tiene que entregar estos mensajes al usuario final, lo que puede verse afectado por los problemas del servidor y la red.

Si está creando su propia aplicación cliente, la app debe implementar cada tipo de respuesta según sea necesario. Para obtener más información, consulte [Implementación de respuestas](/docs/services/assistant?topic=assistant-api-dialog-responses).

## Formato JSON nativo
{: #dialog-responses-json-native}

Además del formato JSON genérico, el archivo JSON de nodo de diálogo también da soporte a respuestas específicas del canal escritas utilizando los formatos nativos de Slack y Facebook Messenger. Estos formatos también reciben soporte del conector de {{site.data.keyword.conversationshort}}. Es posible que desee utilizar formatos de JSON nativos si sabe que el espacio de trabajo solo se va a integrar con un tipo de canal y tiene que especificar un tipo de respuesta a la que el formato JSON genérico no da soporte actualmente.

Puede especificar JSON nativo para Slack o Facebook utilizando el campo adecuado en la respuesta del nodo del diálogo:

- `output.slack`: inserte el JSON que desea incluir en el campo `attachment` de la respuesta de Slack. Para obtener más información sobre el formato JSON de Slack, consulte la [documentación ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://api.slack.com/docs/message-attachments){: new_window} de Slack.

- `output.facebook`: inserte el JSON que desea incluir en el campo `message.attachment.payload` de la respuesta Facebook. Para obtener más información sobre el formato JSON de Facebook, consulte la [documentación ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://developers.facebook.com/docs/messenger-platform/send-messages/templates){: new_window} de Facebook.

## Tipos de respuesta
{: #dialog-responses-json-response-types}

El formato JSON genérico da soporte a los siguientes tipos de respuesta.

### Imagen
{: #dialog-responses-json-image}

Muestra una imagen especificada por un URL.

#### Campos
{: #{: #dialog-responses-json-image-fields}

| Nombre          | Tipo   | Descripción                        | ¿Es necesario? |
|---------------|--------|------------------------------------|-----------|
| response_type | enum   | `image`                            | S         |
| source        | serie | URL de la imagen. La imagen especificada debe estar en formato .jpg, .gif o .png. | S |
| title         | serie | Título a mostrar antes de la imagen.| N         |
| description   | serie | Texto de la descripción que acompaña a la imagen. | N |

#### Ejemplo
{: #dialog-responses-json-image-example}

En este ejemplo se visualiza una imagen con un título y un texto descriptivo.

```json
{
  "output": {
    "generic":[
      {
        "response_type": "image",
        "source": "http://example.com/image.jpg",
        "title": "Example image",
        "description": "An example image returned as part of a multimedia response."
      }
    ]
  }
}
```

### Opción
{: #dialog-responses-json-option}

Muestra un conjunto de botones o una lista desplegable que los usuarios pueden utilizar para elegir una opción. El valor especificado se envía entonces al espacio de trabajo como entrada de usuario.

#### Campos
{: #dialog-responses-json-option-fields}

| Nombre          | Tipo   | Descripción                         | ¿Es necesario? |
|---------------|--------|-------------------------------------|-----------|
| response_type | enum   | `option`                            | S         |
| title         | serie | Título a mostrar antes de las opciones. | S       |
| description   | serie | Texto de la descripción que acompaña a las opciones. | N |
| preference    | enum   | El tipo de control preferido que se debe mostrar, si recibe soporte del canal (`dropdown` o `button`). El conector de {{site.data.keyword.conversationshort}} actualmente solo da soporte al valor `button`.| N |
| options       | lista   | Lista de pares de clave/valor especificando las opciones que el usuario puede elegir. | S |
| options[].label | serie | Etiqueta para mostrar al usuario de la opción. | S     |
| options[].value | object | Un objeto que define la respuesta que se enviará al servicio {{site.data.keyword.conversationshort}} si el usuario selecciona la opción. | S |
| options[].value.input | object | Un objeto de entrada que incluye el texto de entrada correspondiente a la opción. | N |
| options[].value.input.text | serie | El texto que se enviará al servicio para la opción. | N |

#### Ejemplo
{: #dialog-responses-json-option-example}

Este ejemplo muestra dos opciones:

- La Opción 1 (con la etiqueta `Buy something`) envía un mensaje de serie simple (`Place order`), que se envía al espacio de trabajo como texto de entrada.
- Opción 2 (con la etiqueta `Exit`) envía un mensaje complejo que incluye tanto texto de entrada como una matriz de intenciones. La respuesta puede incluir cualquier campo que sea una parte válida de un mensaje de {{site.data.keyword.conversationshort}}. (Para obtener más información sobre la estructura de la entrada de mensajes, consulte la [Referencia de API ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://cloud.ibm.com/apidocs/assistant?curl=#get-response-to-user-input){: new_window}).

```json
{
  "output": {
    "generic":[
      {
        "response_type": "option",
        "title": "Choose from the following options:",
        "preference": "button",
        "options": [
          {
            "label": "Buy something",
            "value": {
              "input": {
                "text": "Place order"
              }
            }
          },
          {
            "label": "Exit",
      "value": {
              "input": {
                "text": "Exit"
              },
              "intents": [
                {
                  "intent": "exit_app",
            "confidence": 1.0
          }
              ]
            }
          }
        ]
      }
    ]
  }
}
```

### Pausa
{: #dialog-responses-json-pause}

Realiza una pausa antes de enviar el siguiente mensaje al canal, y, opcionalmente, envía un suceso "el usuario está escribiendo" (para los canales que lo admiten).

#### Campos
{: #dialog-responses-json-pause-fields}

| Nombre          | Tipo   | Descripción        | ¿Es necesario? |
|---------------|--------|--------------------|-----------|
| response_type | enum   | `pause`            | S         |
| time          | int    | La duración de la pausa, en milisegundos. | S |
| typing        | boolean | Si se va a enviar el suceso "el usuario está escribiendo" durante la pausa. Se pasa por alto si el canal no da soporte a este suceso. | N |

#### Ejemplo
{: #dialog-responses-json-pause-example}

Este ejemplo envía el suceso "el usuario está escribiendo" mientras está en pausa durante 5 segundos.

```json
{
  "output": {
    "generic":[
      {
        "response_type": "pause",
        "time": 5000,
        "typing": true
      }
    ]
  }
}
```

### Texto
{: #dialog-responses-json-text}

Muestra texto. Para añadir diversidad, puede especificar varias respuestas de texto alternativas. Si especifica varias respuestas, puede optar por rotar secuencialmente a través de la lista, elegir una respuesta aleatoriamente o generar todas las respuestas especificadas.

#### Campos
{: #dialog-responses-json-text-fields}

| Nombre          | Tipo   | Descripción        | ¿Es necesario? |
|---------------|--------|--------------------|-----------|
| response_type | enum   | `text`             | S         |
| values        | lista   | Una lista de uno o varios objetos que definen la respuesta de texto. | S |
| values.[_n_].text   | serie | El texto de una respuesta. Puede incluir caracteres de salto de línea (`\n`) y etiquetado de Markdown, si el canal lo soporta. (Se ignora cualquier formato no soportado por el canal). | N |
| selection_policy | serie | Cómo se selecciona una respuesta en la lista, si se especifica más de una respuesta. Los valores posibles son `sequential`, `random` y `multiline`. | N |
| delimiter     | serie | El delimitador de la salida como separador entre respuestas. Solo se utiliza si `selection_policy`=`multiline`. El delimitador predeterminado es newline (`\n`). | N |

#### Ejemplo
{: #dialog-responses-json-text-example}

Este ejemplo muestra un mensaje de bienvenida al usuario.

```json
{
  "output": {
    "generic":[
      {
        "response_type": "text",
        "values": [
          { "text": "Hello." },
          { "text": "Hi there." }
        ],
        "selection_policy": "random"
      }
    ]
  }
}  
```
