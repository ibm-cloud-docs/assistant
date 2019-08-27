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

# Acessando dados de contexto
{: #api-client-get-context}

O *contexto* é um objeto que contém variáveis que persistem em toda uma conversa e podem ser compartilhadas pelo diálogo e o aplicativo cliente. Se o seu aplicativo estiver usando a API v2, o contexto será mantido automaticamente pelo assistente em uma base por sessão. Tanto o diálogo quanto o aplicativo cliente podem ler e gravar variáveis de contexto. Por padrão, o contexto não é retornado a um aplicativo cliente, mas é possível solicitar opcionalmente que ele seja incluído na resposta para cada solicitação `/message`.

**Importante:** um uso do contexto é especificar um ID do usuário exclusivo para cada usuário final que interaja com o assistente. Para planos baseados no usuário, esse ID é usado para propósitos de faturamento. (Para obter mais informações, veja [Planos baseados no usuário](/docs/services/assistant?topic=assistant-services-information#user-based-plans).)

Há dois tipos de contexto:

- **Contexto global**: as variáveis de contexto que são compartilhadas por todas as qualificações usadas por um assistente, incluindo variáveis do sistema interno usadas para gerenciar o fluxo de conversa.

- **Contexto específico da qualificação**: variáveis de contexto específicas para uma determinada qualificação, incluindo quaisquer variáveis definidas pelo usuário necessárias para seu aplicativo. Atualmente, somente uma qualificação (denominada `main skill`) é suportada.

## Exemplo

O exemplo a seguir mostra uma solicitação `/message` que inclui as variáveis de contexto globais e específicas da qualificação; ele também usa a propriedade `options.return_context` para solicitar que o contexto seja retornado com a resposta.

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
    }, contexto: {, contexto: {
      'global': {
        'system': {
          'user_id': 'my_user_id' }
      }, 'skills': {
        'main skill': {
          'user_defined': {
            'account_number': '123456' }
        }
      }
    }
  })
  .then (res = >{
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
        'message_type': 'text', 'text': 'Hello', 'options': {
            'return_context': True
        }
    }, contexto = {, contexto = {
        'global': {
            'system': {
                'user_id': 'my_user_id' }
        }, 'skills': {
            'main skill': {
                'user_defined': {
                    'account_number': '123456' }
            }
        }
    }
) .get_result ()

print (json.dumps (response, indent= 2))
```
{: codeblock}
{: python}

```java
MessageInputOptions inputOptions = new MessageInputOptions();
    inputOptions.setReturnContext(true);

MessageInput input = new MessageInput.Builder ()
  .messageType ("text")
  .text ("Hello")
  .options (inputOptions)
  .build ();

// create global context with user ID
    MessageContextGlobalSystem system = new MessageContextGlobalSystem();
    system.setUserId("my_user_id");
    MessageContextGlobal globalContext = new MessageContextGlobal();
    globalContext.setSystem(system);
  
// build user-defined context variables, put in skill-specific context for main skill
    Map<String, String> userDefinedContext = new HashMap<>();
    userDefinedContext.put("account_num","123456");
    Map<String, Map> mainSkillContext = new HashMap<>();
    mainSkillContext.put("user_defined", userDefinedContext);
    MessageContextSkills skillsContext = new MessageContextSkills();
    skillsContext.put("main skill", mainSkillContext);

MessageContext context = new MessageContext();
    context.setGlobal(globalContext);
    context.setSkills(skillsContext);

Opções de MessageOptions = new MessageOptions.Builder ("{", "{")
  .input (entrada)
  .contexto (contexto)
  .build ();

MessageResponse response = service.message(options).execute().getResult();

System.out.println(response);
```
{: codeblock}
{: java}

Neste exemplo de solicitação, o aplicativo especifica um valor para `user_id` como parte do contexto global. Além disso, ele configura uma variável de contexto definida pelo usuário (`account_number`) como parte do contexto específico da qualificação. Essa variável de contexto pode ser acessada por nós de diálogo como `$account_number`. (Para obter mais informações sobre como usar o contexto em seu diálogo, consulte [Como o diálogo é processado](/docs/services/assistant?topic=assistant-dialog-runtime).)

É possível especificar qualquer nome de variável que você deseja usar para uma variável de contexto definida pelo usuário. Se a variável especificada já existir, ela será sobrescrita com o novo valor; se não, uma nova variável será incluída no contexto.

A saída dessa solicitação inclui não somente a saída comum, mas também o contexto, mostrando que os valores especificados foram incluídos.

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
      "principal habilidade": {
        "user_defined": {
          "account_number": "123456"
        }
      }
    }
  }
}
```

Para obter informações detalhadas sobre como acessar as variáveis de contexto usando a API, consulte a [Referência de API v2 ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://{DomainName}/apidocs/assistant-v2#send-user-input-to-assistant){: new_window}.)
