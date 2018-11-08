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

# Respuestas multimedia

Si su espacio de trabajo está integrado con Slack o Facebook Messenger utilizando el [conector {{site.data.keyword.conversationshort}}](conversation-connector.html), puede especificar respuestas de nodo de diálogo que incluyan elementos multimedia o interactivos como, por ejemplo botones que es posible pulsar. Para especificar una respuesta interactiva, inserte un bloque de datos JSON en la salida del nodo del diálogo. 

**Nota:** Si desea utilizar mensajes interactivos con Slack, asegúrese de que ha habilitado soporte para los mensajes interactivos. Para obtener más información, consulte el archivo [README ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/watson-developer-cloud/conversation-connector/blob/master/channels/slack/README.md#interactive-messages){: new_window} de despliegue de Slack. 

## Formato JSON genérico

Puede especificar respuestas interactivas utilizando un formato JSON genérico que da soporte tanto a despliegues de Slack como de Facebook Messenger. Para especificar una respuesta interactiva en el formato JSON genérico, inserte el JSON en el campo `output.generic` de la respuesta del nodo de diálogo. En el siguiente ejemplo se muestra cómo enviar una respuesta con varios tipos de respuesta (texto, imágenes y opciones que es posible pulsar): 

```json
{
  "output": {
    "generic":[
      {
        "response_type": "text",
        "text": "Here are your nearest stores."
      },
      {
        "response_type": "image",
        "source": "http://...",
        "title": "Image title",
        "description": "Some description for the image"
      },
      {
        "response_type": "option",
        "title": "Click on one of the following",
        "options": [
          {
            "label": "Location 1",
            "value:" "Location 1"
          },
          {
            "label": "Location 2",
            "value:" "Location 2"
          },
          {
            "label": "Location 3",
            "value:" "Location 3"
          }
        ]
      }
    ]
  }
}
```

Para obtener más información sobre los tipos de respuesta soportados y cómo especificarlos, consulte [Tipos de respuesta](#response-types).

En el tiempo de ejecución, el conector {{site.data.keyword.conversationshort}} convierte esta respuesta en el formato que espera el canal (Slack o Facebook Messenger). Si la respuesta contiene varios tipos de soportes o adjuntos, la respuesta genérica se convierte en una serie de cargas útiles de mensaje por separado. A continuación, el conector envía cada mensaje de carga al canal en un mensaje separado. 

**Nota:** Cuando una respuesta se divide en varios mensajes, el conector {{site.data.keyword.conversationshort}} envía estos mensajes al canal de forma secuencial. Es el canal quien tiene que entregar estos mensajes al usuario final, lo que puede verse afectado por los problemas del servidor y la red. 

## Formato JSON nativo

Además del formato JSON genérico, el conector {{site.data.keyword.conversationshort}} también da soporte a respuestas específicas del canal escritas utilizando los formatos nativos de Slack y Facebook Messenger. Podría desear utilizar los formatos JSON nativos si tuviese la necesidad de especificar un tipo de respuesta a la que el formato JSON genérico no diese soporte actualmente. 

Puede especificar JSON nativo para Slack o Facebook utilizando el campo adecuado en la respuesta del nodo del diálogo: 

- `output.slack`: inserte el JSON que desea incluir en el campo `attachment` de la respuesta Slack. Para obtener más información sobre el formato JSON de Slack, consulte la [documentación ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://api.slack.com/docs/message-attachments){: new_window} de Slack. 

- `output.facebook`: inserte el JSON que desea incluir en el campo `message.attachment.payload` de la respuesta Facebook. Para obtener más información sobre el formato JSON de Facebook, consulte la [documentación ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://developers.facebook.com/docs/messenger-platform/send-messages/templates){: new_window} de Facebook. 

## Tipos de respuesta

El formato JSON genérico da soporte a los siguientes tipos de respuesta. 

### Imagen

Muestra una imagen especificada por un URL.

#### Campos

| Nombre | Tipo   | Descripción | ¿Es necesario? |
|---------------|--------|------------------------------------|-----------|
| response_type | enum   | `image`                            | S         |
| source        | serie | URL de la imagen. La imagen especificada debe estar en formato .jpg, .gif, o .png. | S |
| title         | serie | Título a mostrar antes de la imagen. | N         |
| description   | serie | Texto de la descripción que acompaña a la imagen. | N |

#### Ejemplo

En este ejemplo se visualiza una imagen con un título y un texto descriptivo. 

```json
{
  "response_type": "image",
  "source": "http://example.com/image.jpg",
  "title": "Example image",
  "description": "An example image returned as part of a multimedia response."
}
```

### Opción

Muestra un conjunto de botones que los usuarios pueden pulsar para elegir una opción. El valor especificado se envía entonces al espacio de trabajo como entrada de usuario. 

#### Campos

| Nombre | Tipo   | Descripción | ¿Es necesario? |
|---------------|--------|-------------------------------------|-----------|
| response_type | enum   | `option`                            | S         |
| title         | serie | Título a mostrar antes de las opciones. | S         |
| description   | serie | Texto de la descripción que acompaña a las opciones. | N |
| options       | lista   | Lista de pares de clave/valor especificando las opciones que el usuario puede elegir. | S |
| options[].label | serie | Etiqueta para mostrar al usuario de la opción. | S |
| options[].value | serie <br/>object | Valor que se enviará al servicio {{site.data.keyword.conversationshort}} si el usuario selecciona la opción. Puede ser una serie (para respuestas simples) o un objeto con varios campos. Consulte más abajo para ver algunos ejemplos. | S |

#### Ejemplo

Este ejemplo muestra dos opciones:

- Opción 1 (etiquetada 'Buy something' (Comprar algo)) envía un mensaje de serie simple (`option_1`) al espacio de trabajo utilizando el campo `input.text` del mensaje. 
- Opción 2 (etiquetada 'Exit' (Salir)) envía un mensaje complejo que incluye tanto texto de entrada como una matriz de intenciones. La respuesta puede incluir cualquier campo que sea una parte válida de un mensaje de {{site.data.keyword.conversationshort}}. (Para obtener más información sobre la estructura de la entrada de mensajes, consulte la [Referencia de API ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://www.ibm.com/watson/developercloud/conversation/api/v1/?curl#send_message){: new_window}). 

```json
{
  "response_type": "option",
  "title": "Choose from the following options:",
  "options": [
    {
      "label": "Buy something",
      "value": "Place order"
    },
    {
      "label": "Exit",
      "value": {
        "input": {
      "text": "Exit"
        },
        "intent": [
          {
            "intent": "exit_app",
            "confidence": 1.0
          }
        ]
      }
    }
  ]
}
```

### Texto 

Visualiza texto. 

#### Campos

| Nombre | Tipo   | Descripción | ¿Es necesario? |
|---------------|--------|--------------------|-----------|
| response_type | enum   | `text`             | S         |
| text          | serie | Texto a visualizar. Puede incluir caracteres de salto de línea (`\n`) y etiquetado de Markdown, si el canal lo soporta. (Se ignora cualquier formato no soportado por el canal). | S |

#### Ejemplo

Este ejemplo muestra un mensaje de texto al usuario.

```json
{
  "response_type": "text",
  "text": "This is a text response."
}
```
