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

# Implementación de respuestas
{: #dialog-api-responses}

Un nodo de diálogo puede responder a los usuarios con una respuesta que incluya texto, imágenes o elementos interactivos, como opciones que se pueden pulsar. Si está creando su propia aplicación cliente, debe implementar la visualización correcta de cada tipo de respuesta que devuelva el diálogo. (Para obtener más información sobre respuestas de diálogo, consulte [Respuestas](/docs/services/assistant?topic=assistant-dialog-overview#responses)).

## Formato de salida de las respuestas
{: #dialog-api-responses-output}

De forma predeterminada, las respuestas de un nodo de diálogo se especifican en el objeto `output.generic` en la respuesta JSON que de devuelve desde la API /message. El objeto `generic` contiene una matriz de un máximo de 5 elementos de respuesta que están pensados para cualquier canal. En el siguiente ejemplo de JSON se muestra una respuesta que incluye texto y una imagen.

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

**Nota:** Como se muestra en este ejemplo, la respuesta de texto (`OK, here's a picture of a dog.`) también se devuelve en la matriz `output.text`. Esto se incluye por motivos de compatibilidad con versiones anteriores para las aplicaciones que no dan soporte al formato `output.generic`.

Es responsabilidad de la aplicación cliente manejar correctamente todos los tipos de respuesta. En este caso, la aplicación tendrá que mostrar el texto y la imagen especificados al usuario.

## Tipos de respuesta
{: #dialog-api-responses-types}

Cada elemento de una respuesta es de uno de los tipos de respuesta admitidos (actualmente `image`, `option`, `pause` y `text`). Cada tipo de respuesta se especifica utilizando un conjunto diferente de propiedades de JSON, de modo que las propiedades incluidas para cada respuesta variarán en función del tipo de respuesta. Para obtener información completa sobre el modelo de respuesta de la API /message, revise la publicación [Consulta de API ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://cloud.ibm.com/apidocs/assistant?curl=#get-response-to-user-input){: new_window}.)

En esta sección se describen los tipos de respuesta disponibles y cómo se representan en el archivo JSON de respuesta de la API /message. (Si utiliza el SDK de Watson, puede utilizar las interfaces que se suministran para su idioma para acceder a los mismos objetos.)

**Nota:** Los ejemplos de esta sección muestran el formato de los datos JSON que se devuelven desde la API /message en tiempo de ejecución. Tenga en cuenta que es diferente del formato JSON utilizado para definir respuestas dentro de un nodo de diálogo. Para obtener más información, consulte el tema sobre [Definición de respuestas mediante el editor de JSON](/docs/services/assistant?topic=assistant-dialog-responses-json).

### Imagen
{: #dialog-api-responses-image}

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

### Opción
{: #dialog-api-responses-option}

El tipo de respuesta `option` indica a la aplicación cliente que muestre un control de interfaz de usuario que permita al usuario seleccionar entre una lista de opciones y, a continuación, enviar la entrada de nuevo al servicio {{site.data.keyword.conversationshort}} en función de la opción seleccionada:

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

La app puede mostrar las opciones especificadas mediante cualquier control de interfaz de usuario adecuado (por ejemplo, un conjunto de botones o una lista desplegable). La propiedad opcional `preference` indica el tipo preferido de control que debe utilizar la app (`button` o `dropdown`), si se da soporte a la misma.

Para cada opción, la propiedad `label` especifica el texto de etiqueta que debe aparecer para la opción en el control de la interfaz de usuario. La propiedad `value` especifica la entrada que se debe enviar de nuevo al servicio {{site.data.keyword.conversationshort}} (mediante la API /message) cuando el usuario seleccione la opción correspondiente. Tenga en cuenta que la propiedad `value` puede incluir no solo la entrada de texto, sino también otros objetos de entrada como, por ejemplo, intenciones y entidades, todos los cuales deben enviarse al servicio.

### Pausa
{: #dialog-api-responses-pause}

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

### Texto
{: #dialog-api-responses-text}

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

Tenga en cuenta que, por motivos de compatibilidad con versiones anteriores, el mismo texto también se incluye en la matriz `output.text`
en la respuesta del diálogo (tal como se muestra en el ejemplo del principio de esta página).
