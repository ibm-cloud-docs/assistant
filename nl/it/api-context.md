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

# Accesso ai dati di contesto
{: #api-client-get-context}

Il *contesto* è un oggetto che contiene variabili che persistono per tutta una conversazione e che possono essere condivise dal dialogo e dall'applicazione client. Se la tua applicazione sta utilizzando l'API v2, il contesto viene mantenuto automaticamente dall'assistente per ogni singola sessione. Sia il dialogo che l'applicazione client possono leggere e scrivere variabili di contesto. Per impostazione predefinita, il contesto non viene restituito a un'applicazione client, ma puoi, facoltativamente, richiedere che venga incluso nella risposta in ogni richiesta `/message`.

**Importante:** un uso del contesto è quello di specificare un ID utente univoco per ciascun utente finale che interagisce con l'assistente. Per i piani basati sull'utente, questo ID viene utilizzato a scopo di fatturazione. (Per ulteriori informazioni, vedi [Piani basati sull'utente](/docs/services/assistant?topic=assistant-services-information#user-based-plans).)

Esistono due tipi di contesto:

- **Contesto globale**: le variabili di contesto che vengono condivise da tutte le capacità utilizzate da un assistente, incluse le variabili di sistema interne utilizzate per gestire il flusso della conversazione.

- **Contesto specifico della capacità**: le variabili di contesto specifiche per una particolare capacità, incluse le variabili definite dall'utente di cui necessita la tua applicazione. Attualmente, è supportata solo una capacità (denominata `main skill`).

## Esempio

Il seguente esempio mostra una richiesta `/message` che include sia variabili di contesto globali che specifiche della capacità; utilizza anche la proprietà `options.return_context` per richiedere che il contesto venga restituito con la risposta.

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

In questa richiesta di esempio, l'applicazione specifica un valore per `user_id` come parte del contesto globale. Inoltre, imposta una variabile di contesto definita dall'utente (`account_number`) come parte del contesto specifico della capacità. Puoi accedere a questa variabile di contesto dai nodi del dialogo come `$account_number`. (Per ulteriori informazioni sull'utilizzo del contesto nel tuo dialogo, vedi [Come viene elaborato il dialogo](/docs/services/assistant?topic=assistant-dialog-runtime).)

Puoi specificare qualsiasi nome variabile che desideri utilizzare per una variabile di contesto definita dall'utente. Se la variabile specificata esiste già, viene sovrascritta con il nuovo valore; nel caso in cui non esista, viene aggiunta una nuova variabile al contesto.

L'output di questa richiesta include non solo il normale output, ma anche il contesto, mostrando che sono stati aggiunti i valori specificati.

```json
{
  "output":{
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

Per informazioni dettagliate su come accedere alle variabili di contesto utilizzando l'API, vedi la [Guida di riferimento API v2 ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://{DomainName}/apidocs/assistant-v2#send-user-input-to-assistant){: new_window}.)
