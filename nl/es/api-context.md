---

copyright:
  years: 2015, 2019
lastupdated: "2019-05-13"

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

# Acceso a datos de contexto
{: #api-client-get-context}

El valor *context* es un objeto que contiene variables que permanecen durante una conversación y que se pueden compartir entre el diálogo y la aplicación cliente. Si la aplicación utiliza la API v2, el asistente mantiene automáticamente el contexto por sesión. Tanto el diálogo como la aplicación cliente pueden leer y escribir variables de contexto. De forma predeterminada, el contexto no se devuelve a una aplicación cliente, pero si lo desea puede solicitar que se incluya en la respuesta a cada solicitud `/message`.

**Importante:** Un uso del contexto consiste en especificar un ID de usuario exclusivo para cada usuario final que interactúe con el asistente. Para planes basados en usuario, este ID se utiliza para calcular la facturación. (Para obtener más información, consulte [Planes basados en usuario](/docs/services/assistant?topic=assistant-services-information#user-based-plans).)

Hay dos tipos de contexto:

- **Contexto global**: las variables de contexto que comparten todos los conocimientos que utiliza un asistente, incluidas las variables internas del sistema utilizadas para gestionar el flujo de la conversación.

- **Contexto específico de conocimiento**: variables de contexto específicas de un conocimiento en particular, incluidas las variables definidas por el usuario que necesita la aplicación. Actualmente solo se da soporte a un conocimiento (denominada `conocimiento principal`).

## Ejemplo

En el ejemplo siguiente se muestra una solicitud `/message` que incluye tanto variables de contexto globales como específicas del conocimiento; también se utiliza la propiedad `options.return_context` para solicitar que se devuelva el contexto con la respuesta.

```javascript
service
  .message({
    assistant_id: '{assistant_id}',
    session_id: '{session_id}',
    input: {
      message_type: 'text',
      text: 'Hello',
      options: {
        'return_context': true
    }
    },
  context: {
      'global': {
        'system': {
          'user_id': 'my_user_id'
            }
      },
        'skills': {
        'main skill': {
          'user_defined': {
            'account_number': '123456'
                }
        }
      }
    }
  })
  .then(res => {
    console.log(JSON.stringify(res, null, 2));
  })
  .catch(err => {
    console.log(err);
  });
```
{: codeblock}
{: javascript}

```python
response=service.message(
    assistant_id='{assistant_id}',
    session_id='{session_id}',
    input={
        'message_type': 'text',
        'text': 'Hello',
        'options': {
            'return_context': True
        }
    },
    context={
        'global': {
            'system': {
                'user_id': 'my_user_id'
            }
        },
        'skills': {
            'main skill': {
                'user_defined': {
                    'account_number': '123456'
                }
            }
        }
    }
).get_result()

print(json.dumps(response, indent=2))
```
{: codeblock}
{: python}

```java
MessageInputOptions inputOptions = new MessageInputOptions();
    inputOptions.setReturnContext(true);

MessageInput input = new MessageInput.Builder()
  .messageType("text")
  .text("Hello")
  .options(inputOptions)
  .build();

// crear contexto global con ID de usuario
    MessageContextGlobalSystem system = new MessageContextGlobalSystem();
    system.setUserId("my_user_id");
    MessageContextGlobal globalContext = new MessageContextGlobal();
    globalContext.setSystem(system);
  
// crear variables de contexto definidas por el usuario, colocarlas en contexto específico del conocimiento para el conocimiento principal
    Map<String, String> userDefinedContext = new HashMap<>();
    userDefinedContext.put("account_num","123456");
    Map<String, Map> mainSkillContext = new HashMap<>();
    mainSkillContext.put("user_defined", userDefinedContext);
    MessageContextSkills skillsContext = new MessageContextSkills();
    skillsContext.put("main skill", mainSkillContext);

MessageContext context = new MessageContext();
    context.setGlobal(globalContext);
    context.setSkills(skillsContext);

MessageOptions options = new MessageOptions.Builder("{assistant_id}", "{session_id}")
  .input(input)
  .context(context)
  .build();

MessageResponse response = service.message(options).execute().getResult();

System.out.println(response);
```
{: codeblock}
{: java}

En esta solicitud de ejemplo, la aplicación especifica un valor para `user_id` como parte del contexto global. Además, establece una variable de contexto definida por el usuario (`account_number`) como parte del contexto específico del conocimiento. Los nodos del diálogo pueden acceder a esta variable de contexto como `$account_number`. (Para obtener más información sobre cómo utilizar el contexto en el diálogo, consulte [Cómo se procesa el diálogo](/docs/services/assistant?topic=assistant-dialog-runtime)).

Puede especificar cualquier nombre de variable que desee utilizar para una variable de contexto definida por el usuario. Si la variable especificada ya existe, se sobrescribe con el nuevo valor; si no es así, se añade una nueva variable al contexto.

La salida de esta solicitud incluye no solo la salida habitual, sino también el contexto, que muestra que se han añadido los valores especificados.

```json
{
  "output": {
    "generic":[
      {
        "response_type": "text",
        "text": "Welcome to the Watson Assistant example!"
      }
    ],
    "intents": [
      {
        "intent": "hello",
        "confidence": 1
      }
    ],
    "entities": []
  },
"context": {
    "global": {
      "system": {
        "turn_count": 1,
        "user_id": "my_user_id"
      }
    },
    "skills": {
      "main skill": {
        "user_defined": {
          "account_number": "123456"
        }
      }
    }
  }
}
```

Para ver información detallada sobre cómo acceder a las variables de contexto mediante la API, revise la [Consulta de API v2 ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://{DomainName}/apidocs/assistant-v2#send-user-input-to-assistant){: new_window}.
