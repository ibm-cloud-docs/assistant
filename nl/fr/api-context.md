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

# Accès aux données contextuelles 
{: #api-client-get-context}

Le *contexte* est un objet contenant des variables qui persistent dans une conversation et peuvent être partagées par la boîte de dialogue et l'application client. Si votre application utilise l'API v2, le contexte est automatiquement géré par l'assistant session par session. Le dialogue et l'application client peuvent lire et écrire des variables contextuelles. Par défaut, le contexte n'est pas renvoyé à une application client, mais vous pouvez éventuellement demander qu'il soit inclus dans la réponse à chaque demande `/message`.

**Important : **une des utilisations du contexte est de spécifier un ID utilisateur unique pour chaque utilisateur final qui interagit avec l’assistant. Pour les plans basés sur l'utilisateur, cet identifiant est utilisé à des fins de facturation. (Pour plus d'informations, reportez-vous à la rubrique [Forfaits basés sur l'utilisateur](/docs/services/assistant?topic=assistant-services-information#user-based-plans).)

Il existe deux types de contexte :

- **Contexte global** : variables contextuelles partagées par toutes les compétences utilisées par un assistant, y compris les variables système internes utilisées pour gérer le flux de conversation.

- **Contexte spécifique à une compétence** : variables contextuelles spécifiques à une compétence particulière, y compris les variables définies par l'utilisateur nécessaires à votre application. Actuellement, une seule compétence (nommée `main skill`) est prise en charge.

## Exemple

L'exemple suivant montre une demande `/message` incluant des variables contextuelles globales et spécifiques à une compétence. Il utilise également la propriété `options.return_context` pour demander que le contexte soit renvoyé avec la réponse.

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

MessageOptions options = new MessageOptions.Builder("{assistant_id}", "{session_id}")
  .input(input)
  .context(context)
  .build();

MessageResponse response = service.message(options).execute().getResult();

System.out.println(response);
```
{: codeblock}
{: java}

Dans cet exemple de demande, l'application spécifie une valeur pour `user_id` dans le cadre du contexte global. En outre, elle définit une variable contextuelle définie par l'utilisateur (`account_number`) dans le cadre du contexte spécifique à la compétence. Les noeuds de dialogue peuvent accéder à cette variable contextuelle sous la forme `$account_number`. (Pour plus d'informations sur l'utilisation du contexte dans votre dialogue, reportez-vous à la rubrique [Mode de traitement du dialogue](/docs/services/assistant?topic=assistant-dialog-runtime).)

Vous pouvez spécifier le nom de variable que vous souhaitez utiliser pour une variable contextuelle définie par l'utilisateur. Si la variable spécifiée existe déjà, elle est remplacée par la nouvelle valeur. sinon, une nouvelle variable est ajoutée au contexte.

La sortie de cette requête inclut non seulement la sortie habituelle, mais également le contexte, indiquant que les valeurs spécifiées ont été ajoutées.

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

Pour plus d'informations sur l'accès aux variables contextuelles à l'aide de l'API, reportez-vous à la rubrique [Référence d'API v2![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://{DomainName}/apidocs/assistant-v2#send-user-input-to-assistant){: new_window}.)
