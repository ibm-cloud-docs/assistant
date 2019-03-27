---

copyright:
  years: 2015, 2019
lastupdated: "2019-02-21"

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

# Creazione di un'applicazione client
{: #api-client}

Hai già una capacità di dialogo funzionante e un assistente che la utilizza. Adesso, vuoi sviluppare un'applicazione client personalizzata che interagirà con i tuoi utenti e comunicherà con il servizio {{site.data.keyword.conversationfull}}.
{: shortdesc}

## Configurazione del servizio {{site.data.keyword.conversationshort}}
{: #api-client-setup}

L'applicazione di esempio che verrà creata in questa sezione implementa diverse funzioni di un assistente personale cognitivo. Il codice applicativo verrà collegato a un'assistente {{site.data.keyword.conversationshort}}, in cui avviene l'elaborazione cognitiva (come il rilevamento degli intenti dell'utente).

Prima di continuare con questo esempio, devi configurare l'assistente necessario:

1.  Scarica il <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/assistant/assistant-simple-example.json" download="assistant-simple-example.json">file JSON</a> della capacità di dialogo.
1.  [Importa la capacità](/docs/services/assistant?topic=assistant-skill-add#creating-skills) in un'istanza del servizio {{site.data.keyword.conversationshort}}.
1.  [Crea un assistente](/docs/services/assistant?topic=assistant-assistant-add#creating-assistants) e collega la capacità che hai importato.

## Recupero delle informazioni sul servizio
{: #api-client-get-info}

Per accedere alle API REST del servizio {{site.data.keyword.conversationshort}}, la tua applicazione deve essere in grado di autenticarsi con {{site.data.keyword.Bluemix}} e di connettersi all'assistente corretto. Dovrai copiare le credenziali del servizio e l'ID assistente e incollarli nel tuo codice applicativo.

Per accedere alle credenziali del servizio e all'ID assistente dallo strumento {{site.data.keyword.conversationshort}}, vai alla scheda **Assistants** e fai clic sul menu ![Menu](images/kabob-grey.png) relativo all'assistente a cui desideri collegarti. Seleziona **View API Details** per vedere i dettagli dell'assistente, inclusi l'ID assistente e la chiave API.

Puoi accedere alle credenziali del servizio anche dal tuo dashboard {{site.data.keyword.Bluemix_short}}.

## Comunicazione con il servizio {{site.data.keyword.conversationshort}}
{: #api-client-communicate}

L'interazione con il servizio {{site.data.keyword.conversationshort}} è semplice. Diamo un'occhiata ad un esempio che si connette al servizio, invia un singolo messaggio e stampa l'output nella console:

```javascript
// Example 1: sets up service wrapper, sends initial message, and
// receives response.

var AssistantV2 = require('watson-developer-cloud/assistant/v2');

// Set up Assistant service wrapper.
var service = new AssistantV2({
  iam_apikey: '{apikey}', // replace with API key
  version: '2018-09-20'
});

var assistantId = '{assistant_id}'; // replace with assistant ID
var sessionId;

// Create session.
service.createSession({
  assistant_id: assistantId
}, function(err, result) {
  if (err) {
    console.error(err); // something went wrong
    return;
  }
  sessionId = result.session_id;
  sendMessage(); // start conversation with empty message
});

// Send message to assistant.
function sendMessage() {
  service.message({
    assistant_id: assistantId,
    session_id: sessionId
  }, processResponse);
}

// Process the response.
function processResponse(err, response) {
  if (err) {
    console.error(err); // something went wrong
    return;
  }

  // Display the output from assistant, if any. Assumes a single text response.
  if (response.output.generic.length != 0) {
      console.log(response.output.generic[0].text);
  }

  // We're done, so we close the session
  service.deleteSession({
    assistant_id: assistantId,
    session_id: sessionId
  }, function(err, result) {
    if (err) {
      console.error(err); // something went wrong
    }
  });
}
```
{: codeblock}
{: javascript}

```python
# Example 1: sets up service wrapper, sends initial message, and
# receives response.

import watson_developer_cloud

# Set up Assistant service.
service = watson_developer_cloud.AssistantV2(
    iam_apikey = '{apikey}', # replace with API key
    version = '2018-09-20'
)

assistant_id = '{assistant_id}' # replace with assistant ID

# Create session.
session_id = service.create_session(
    assistant_id = assistant_id
).get_result()['session_id']

# Start conversation with empty message.
response = service.message(
    assistant_id,
    session_id
).get_result()

# Print the output from dialog, if any. Assumes a single text response.
if response['output']['generic']:
    print(response['output']['generic'][0]['text'])

# We're done, so we delete the session.
service.delete_session(
    assistant_id = assistant_id,
    session_id = session_id
)
```
{: codeblock}
{: python}

```java
/*
 * Example 1: sets up service wrapper, sends initial message, and
 * receives response.
 */

import com.ibm.watson.developer_cloud.assistant.v2.Assistant;
import com.ibm.watson.developer_cloud.assistant.v2.model.CreateSessionOptions;
import com.ibm.watson.developer_cloud.assistant.v2.model.DeleteSessionOptions;
import com.ibm.watson.developer_cloud.assistant.v2.model.MessageOptions;
import com.ibm.watson.developer_cloud.assistant.v2.model.MessageResponse;
import com.ibm.watson.developer_cloud.assistant.v2.model.SessionResponse;
import com.ibm.watson.developer_cloud.service.security.IamOptions;
import java.util.logging.LogManager;

public class AssistantSimpleExample {
  public static void main(String[] args) {

    // Suppress log messages in stdout.
    LogManager.getLogManager().reset();

    // Set up Assistant service.
    IamOptions iamOptions = new IamOptions.Builder().apiKey("{apikey}").build();
    Assistant service = new Assistant("2018-09-20", iamOptions);
    String assistantId = "{assistant_id}"; // replace with assistant ID

    // Create session.
    CreateSessionOptions createSessionOptions = new CreateSessionOptions.Builder(assistantId).build();
    SessionResponse session = service.createSession(createSessionOptions).execute();
    String sessionId = session.getSessionId();

    // Start conversation with empty message.
    MessageOptions messageOptions = new MessageOptions.Builder(assistantId, sessionId).build();
    MessageResponse response = service.message(messageOptions).execute();

    // Print the output from dialog, if any. Assumes a single text response.
    System.out.println(response.getOutput().getGeneric().get(0).getText());

    // We're done, so we delete the session.
    DeleteSessionOptions deleteSessionOptions = new DeleteSessionOptions.Builder(assistantId, sessionId).build();
    service.deleteSession(deleteSessionOptions).execute();
  }
}
```
{: codeblock}
{: java}

Il primo passo è quello di creare un wrapper per il servizio {{site.data.keyword.conversationshort}}.

Il wrapper è un oggetto che utilizzerai per inviare e ricevere l'input dal servizio. Quando crei il wrapper del servizio, specifica le credenziali di autenticazione dalla chiave del servizio, oltre alla versione dell'API {{site.data.keyword.conversationshort}} che stai utilizzando.

In questo esempio di Node.js, il wrapper è un'istanza di `AssistantV2`, memorizzato nella variabile `service`. Gli SDK Watson per gli altri linguaggi forniscono meccanismi equivalenti per creare un'istanza di un wrapper del servizio.
{: javascript}

In questo esempio Python, il wrapper è un'istanza di `watson_developer_cloud.AssistantV2`, memorizzato nella variabile `service`. Gli SDK Watson per gli altri linguaggi forniscono meccanismi equivalenti per creare un'istanza di un wrapper del servizio.
{: python}

In questo esempio Java, il wrapper è un'istanza di `Assistant`, memorizzato nella variabile `service`. Gli SDK Watson per gli altri linguaggi forniscono meccanismi equivalenti per creare un'istanza di un wrapper del servizio.
{: java}

Dopo aver creato il wrapper del servizio, lo utilizziamo per creare una sessione e inviare un messaggio all'assistente. In questo esempio, il messaggio è vuoto; vogliamo solo attivare il nodo conversation_start e pertanto non ci serve alcun testo di input. Stampiamo quindi il testo della risposta nella console e infine eliminiamo la sessione.

Utilizza il comando `node <filename.js>` per eseguire l'applicazione di esempio.
{: javascript}

Utilizza il comando `python3 <filename.py>` per eseguire l'applicazione di esempio.
{: python}

Incolla il codice di esempio in un file denominato `AssistantSimpleExample.java`. Puoi quindi compilarlo ed eseguirlo.
{: java}

**Nota:** assicurati di aver installato l'SDK Watson per Node.js utilizzando `npm install watson-developer-cloud`.
{: javascript}

**Nota:** assicurati di aver installato l'SDK Watson per Python utilizzando `pip install --upgrade watson-developer-cloud` o `easy_install --upgrade watson-developer-cloud`.
{: python}

**Nota:** assicurati di aver installato l'[SDK Watson per Java ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://github.com/watson-developer-cloud/java-sdk/blob/develop/README.md){: new_window}.
{: java}

Supponendo che tutto funzioni come previsto, l'assistente restituisce l'output dal dialogo, che viene quindi stampato nella console:

```
Welcome to the Watson Assistant example!
```
{: screen}

Questo output ci indica che abbiamo comunicato correttamente con il servizio {{site.data.keyword.conversationshort}} e abbiamo ricevuto il messaggio di benvenuto specificato dal nodo conversation_start nel dialogo. Ora possiamo aggiungere un'interfaccia utente, che consente di elaborare l'input utente.

## Elaborazione dell'input utente per rilevare gli intenti
{: #api-client-process-input}

Per poter elaborare l'input utente, dobbiamo aggiungere un'interfaccia utente alla nostra applicazione client. Per questo esempio, manterremo le cose semplici e utilizzeremo l'input e l'output standard.
<span class="ph style-scope doc-content" data-hd-programlang="javascript">Per farlo, possiamo utilizzare il modulo prompt-sync di Node.js. (Puoi installare prompt-sync utilizzando `npm install prompt-sync`.)</span>
<span class="ph style-scope doc-content" data-hd-programlang="python">Per farlo, possiamo utilizzare la funzione `input` di Python 3.</span>
<span class="ph style-scope doc-content" data-hd-programlang="java">Per farlo, possiamo utilizzare la funzione `Console.readLine()` di Java.</span>

```javascript
// Example 2: adds user input and detects intents.

var prompt = require('prompt-sync')();
var AssistantV2 = require('watson-developer-cloud/assistant/v2');

// Set up Assistant service wrapper.
var service = new AssistantV2({
  iam_apikey: '{apikey}', // replace with API key
  version: '2018-09-20'
});

var assistantId = '{assistant_id}'; // replace with assistant ID
var sessionId;

// Create session.
service.createSession({
  assistant_id: assistantId
}, function(err, result) {
  if (err) {
    console.error(err); // something went wrong
    return;
  }
  sessionId = result.session_id;
  sendMessage(); // start conversation with empty message
});

// Send message to assistant.
function sendMessage(messageText) {
  service.message({
    assistant_id: assistantId,
    session_id: sessionId,
    input: {
      message_type: 'text',
      text: messageText
    }
  }, processResponse);
}

// Process the response.
function processResponse(err, response) {
  if (err) {
    console.error(err); // something went wrong
    return;
  }

  // If an intent was detected, log it out to the console.
  if (response.output.intents.length > 0) {
    console.log('Detected intent: #' + response.output.intents[0].intent);
  }

  // Display the output from assistant, if any. Assumes a single text response.
  if (response.output.generic.length != 0) {
    console.log(response.output.generic[0].text);
  }

  // Prompt for the next round of input.
  var newMessageFromUser = prompt('>> ');
    if (newMessageFromUser === 'quit') {
      service.deleteSession({
        assistant_id: assistantId,
        session_id: sessionId
      }, function(err, result) {
        if (err) {
          console.error(err); // something went wrong
    }
      });
      return;
    }
  sendMessage(newMessageFromUser);
}
```
{: codeblock}
{: javascript}

```python
# Example 2: adds user input and detects intents.

import watson_developer_cloud

# Set up Assistant service.
service = watson_developer_cloud.AssistantV2(
    iam_apikey = '{apikey}', # replace with API key
    version = '2018-09-20'
)

assistant_id = '{assistant_id}' # replace with assistant ID

# Create session.
session_id = service.create_session(
    assistant_id = assistant_id
).get_result()['session_id']

# Initialize with empty value to start the conversation.
user_input = ''

# Main input/output loop
while user_input != 'quit':

    # Send message to assistant.
    response = service.message(
        assistant_id,
        session_id,
        input = {
            'text': user_input
    }
    ).get_result()

    # If an intent was detected, print it to the console.
    if response['output']['intents']:
        print('Detected intent: #' + response['output']['intents'][0]['intent'])

    # Print the output from dialog, if any. Assumes a single text response.
    if response['output']['generic']:
    print(response['output']['generic'][0]['text'])

    # Prompt for next round of input.
    user_input = input('>> ')

# We're done, so we delete the session.
service.delete_session(
    assistant_id = assistant_id,
    session_id = session_id
)
```
{: codeblock }
{: python }

```java
/*
 * Example 2: adds user input and detects intents.
 */

import com.ibm.watson.developer_cloud.assistant.v2.Assistant;
import com.ibm.watson.developer_cloud.assistant.v2.model.CreateSessionOptions;
import com.ibm.watson.developer_cloud.assistant.v2.model.DeleteSessionOptions;
import com.ibm.watson.developer_cloud.assistant.v2.model.DialogRuntimeResponseGeneric;
import com.ibm.watson.developer_cloud.assistant.v2.model.MessageInput;
import com.ibm.watson.developer_cloud.assistant.v2.model.MessageOptions;
import com.ibm.watson.developer_cloud.assistant.v2.model.MessageResponse;
import com.ibm.watson.developer_cloud.assistant.v2.model.RuntimeIntent;
import com.ibm.watson.developer_cloud.assistant.v2.model.SessionResponse;
import com.ibm.watson.developer_cloud.service.security.IamOptions;
import java.util.List;
import java.util.logging.LogManager;

public class AssistantSimpleExample {
  public static void main(String[] args) {

    // Suppress log messages in stdout.
    LogManager.getLogManager().reset();

    // Set up Assistant service.
    IamOptions iamOptions = new IamOptions.Builder().apiKey("{apikey}").build();
    Assistant service = new Assistant("2018-09-20", iamOptions);
    String assistantId = "{assistant_id}"; // replace with assistant ID

    // Create session.
    CreateSessionOptions createSessionOptions = new CreateSessionOptions.Builder(assistantId).build();
    SessionResponse session = service.createSession(createSessionOptions).execute();
    String sessionId = session.getSessionId();

    // Initialize with empty value to start the conversation.
    String inputText = "";

    // Main input/output loop
    do {
      // Send message to assistant.
      MessageInput input = new MessageInput.Builder().text(inputText).build();
      MessageOptions messageOptions = new MessageOptions.Builder(assistantId, sessionId)
                                                  .input(input)
                                                  .build();
      MessageResponse response = service.message(messageOptions).execute();

      // If an intent was detected, print it to the console.
      List<RuntimeIntent> responseIntents = response.getOutput().getIntents();
      if(responseIntents.size() > 0) {
        System.out.println("Detected intent: #" + responseIntents.get(0).getIntent());
      }

      // Print the output from dialog, if any. Assumes a single text response.
      List<DialogRuntimeResponseGeneric> responseGeneric = response.getOutput().getGeneric();
      if(responseGeneric.size() > 0) {
        System.out.println(response.getOutput().getGeneric().get(0).getText());
      }

      // Prompt for next round of input.
      System.out.print(">> ");
      inputText = System.console().readLine();
    } while(!inputText.equals("quit"));

    // We're done, so we delete the session.
    DeleteSessionOptions deleteSessionOptions = new DeleteSessionOptions.Builder(assistantId, sessionId).build();
    service.deleteSession(deleteSessionOptions).execute();
  }
}
```
{: codeblock }
{: java }

Questa versione dell'applicazione inizia allo stesso modo di prima: inviando un messaggio vuoto all'assistente per avviare la conversazione.

La funzione `processResponse()` adesso visualizza qualsiasi intento rilevato dalla capacità di dialogo insieme al testo di output. Richiede quindi il ciclo di input utente successivo.
{: javascript }

Visualizza quindi qualsiasi intento rilevato dalla capacità di dialogo insieme al testo di output. Richiede quindi il ciclo di input utente successivo.
{: python }

Visualizza quindi qualsiasi intento rilevato dal dialogo insieme al testo di output e quindi richiede il prossimo ciclo di input utente.
{: java}

Non abbiamo ancora implementato un modo per utilizzare il linguaggio naturale per terminare la conversazione, stiamo invece utilizzando il comando letterale `quit` per indicare che il programma dovrebbe eliminare la sessione e uscire. 

```
Welcome to the Watson Assistant example!
>> hello
Detected intent: #hello
Good day to you.
>> what time is it?
Detected intent: #time
>> goodbye
Detected intent: #goodbye
OK! See you later.
>> quit
```
{: screen}

Stiamo facendo progressi! Il servizio {{site.data.keyword.conversationshort}} sta riconoscendo correttamente i nostri intenti e il dialogo restituisce il testo di output corretto (dove fornito) per ogni intento.

Tuttavia, non succede altro. Quando chiediamo l'ora non otteniamo risposta e quando diciamo goodbye, la conversazione non finisce. Ciò è dovuto al fatto che questi intenti richiedono ulteriori azioni da parte dell'applicazione.

## Implementazione delle azioni dell'applicazione
{: #api-client-implement-actions}

Oltre a visualizzare il testo di output per l'utente, il nostro dialogo {{site.data.keyword.conversationshort}} utilizza l'array delle `azioni` nel JSON di risposta per segnalare quando l'applicazione deve eseguire un'azione, in base agli intenti rilevati. Quando il dialogo determina che l'applicazione client deve fare qualcosa, restituisce un oggetto azione con un `tipo` di `client`. Il `nome` dell'azione indica l'azione specifica, `display_time` o `end_conversation`. (Le proprietà aggiuntive dell'azione possono specificare parametri, credenziali e altre informazioni correlate all'azione, ma per il nostro esempio abbiamo bisogno solo del nome dell'azione.)

Sappiamo che il nostro dialogo non richiederà mai più di un'azione alla volta, quindi il nostro client dovrà controllare solamente l'esistenza di una singola azione `client` nell'array delle `azioni`. Se ne trova una, può quindi eseguire l'azione specificata. (Questa versione rimuove anche la visualizzazione degli intenti rilevati, ora che siamo sicuri che questi sono stati identificati correttamente.)

```javascript
// Example 3: implements app actions.

var prompt = require('prompt-sync')();
var AssistantV2 = require('watson-developer-cloud/assistant/v2');

// Set up Assistant service.
var service = new AssistantV2({
  iam_apikey: '{apikey}', // replace with API key
  version: '2018-09-20'
});

var assistantId = '{assistant_id}'; // replace with assistant ID
var sessionId;

// Create session.
service.createSession({
  assistant_id: assistantId
}, function(err, result) {
  if (err) {
    console.error(err); // something went wrong
    return;
  }
  sessionId = result.session_id;
  sendMessage(''); // start conversation with empty message
});

// Send message to assistant.
function sendMessage(messageText) {
  service.message({
    assistant_id: assistantId,
    session_id: sessionId,
    input: {
      message_type: 'text',
      text: messageText
    }
  }, processResponse);
}

// Process the response.
function processResponse(err, response) {
  if (err) {
    console.error(err); // something went wrong
    return;
  }

  var endConversation = false;

  // Check for client actions requested by the assistant.
  if (response.output.actions) {
    if (response.output.actions[0].type === 'client'){
      if (response.output.actions[0].name === 'display_time') {
        // User asked what time it is, so we output the local system time.
        console.log('The current time is ' + new Date().toLocaleTimeString() + '.');
      } else if (response.output.actions[0].name === 'end_conversation') {
        // User said goodbye, so we're done.
        console.log(response.output.generic[0].text);
        endConversation = true;
      }
    }
  } else {
    // Display the output from assistant, if any. Assumes a single text response.
    if (response.output.generic.length != 0) {
      console.log(response.output.generic[0].text);
  }
  }

  // If we're not done, prompt for the next round of input.
  if (!endConversation) {
    var newMessageFromUser = prompt('>> ');
    sendMessage(newMessageFromUser);
  } else {
    service.deleteSession({
      assistant_id: assistantId,
      session_id: sessionId
    }, function(err, result) {
      if (err) {
        console.error(err); // something went wrong
    }
    });
    return;
  }
}
```
{: codeblock}
{: javascript}

```python
# Example 3: Implements app actions.

import watson_developer_cloud
import time

# Set up Assistant service.
service = watson_developer_cloud.AssistantV2(
    iam_apikey = '{apikey}', # replace with API key
    version = '2018-09-20'
)

assistant_id = '{assistant_id}' # replace with assistant ID

# Create session.
session_id = service.create_session(
    assistant_id = assistant_id
).get_result()['session_id']

# Initialize with empty values to start the conversation.
user_input = ''
current_action = ''

# Main input/output loop
while current_action != 'end_conversation':
    # Clear any action flag set by the previous response.
    current_action = ''

    # Send message to assistant.
    response = service.message(
        assistant_id,
        session_id,
        input = {
            'text': user_input
    }
    ).get_result()

    # Print the output from dialog, if any. Assumes a single text response.
    if response['output']['generic']:
    print(response['output']['generic'][0]['text'])

    # Check for client actions requested by the assistant.
    if 'actions' in response['output']:
        if response['output']['actions'][0]['type'] == 'client':
            current_action = response['output']['actions'][0]['name']

    # User asked what time it is, so we output the local system time.
    if current_action == 'display_time':
        print('The current time is ' + time.strftime('%I:%M:%S %p') + '.')
    # If we're not done, prompt for next round of input.
    if current_action != 'end_conversation':
    user_input = input('>> ')

# We're done, so we delete the session.
service.delete_session(
    assistant_id = assistant_id,
    session_id = session_id
)
```
{: codeblock}
{: python}

```java
/*
 * Example 3: implements app actions.
 */

import com.ibm.watson.developer_cloud.assistant.v2.Assistant;
import com.ibm.watson.developer_cloud.assistant.v2.model.CreateSessionOptions;
import com.ibm.watson.developer_cloud.assistant.v2.model.DeleteSessionOptions;
import com.ibm.watson.developer_cloud.assistant.v2.model.DialogNodeAction;
import com.ibm.watson.developer_cloud.assistant.v2.model.DialogRuntimeResponseGeneric;
import com.ibm.watson.developer_cloud.assistant.v2.model.MessageInput;
import com.ibm.watson.developer_cloud.assistant.v2.model.MessageOptions;
import com.ibm.watson.developer_cloud.assistant.v2.model.MessageResponse;
import com.ibm.watson.developer_cloud.assistant.v2.model.RuntimeIntent;
import com.ibm.watson.developer_cloud.assistant.v2.model.SessionResponse;
import com.ibm.watson.developer_cloud.service.security.IamOptions;
import java.time.LocalTime;
import java.time.format.DateTimeFormatter;
import java.util.List;
import java.util.logging.LogManager;

public class AssistantSimpleExample {
  public static void main(String[] args) {

    // Suppress log messages in stdout.
    LogManager.getLogManager().reset();

    // Set up Assistant service.
    IamOptions iamOptions = new IamOptions.Builder().apiKey("{apikey}").build();
    Assistant service = new Assistant("2018-09-20", iamOptions);
    String assistantId = "{assistant_id}"; // replace with assistant ID

    // Create session.
    CreateSessionOptions createSessionOptions = new CreateSessionOptions.Builder(assistantId).build();
    SessionResponse session = service.createSession(createSessionOptions).execute();
    String sessionId = session.getSessionId();

    // Initialize with empty values to start the conversation.
    String inputText = "";
    String currentAction;

    // Main input/output loop
    do {
      // Clear any action flag set by the previous response.
      currentAction = "";

      // Send message to assistant.
      MessageInput input = new MessageInput.Builder().text(inputText).build();
      MessageOptions messageOptions = new MessageOptions.Builder(assistantId, sessionId)
                                                  .input(input)
                                                  .build();
      MessageResponse response = service.message(messageOptions).execute();

      // Print the output from dialog, if any. Assumes a single text response.
      List<DialogRuntimeResponseGeneric> responseGeneric = response.getOutput().getGeneric();
      if(responseGeneric.size() > 0) {
        System.out.println(response.getOutput().getGeneric().get(0).getText());
      }

      // Check for any actions requested by the assistant.
      List<DialogNodeAction> responseActions = response.getOutput().getActions();
      if(responseActions != null) {
        if(responseActions.get(0).getActionType().equals("client")) {
          currentAction = responseActions.get(0).getName();
        }
      }

      // User asked what time it is, so we output the local system time.
      if(currentAction.equals("display_time")) {
        DateTimeFormatter fmt = DateTimeFormatter.ofPattern("h:mm:ss a");
        LocalTime time = LocalTime.now();
        System.out.println("The current time is " + time.format(fmt) + ".");
      }

      // If we're not done, prompt for next round of input.
      if(!currentAction.equals("end_conversation")) {
        System.out.print(">> ");
        inputText = System.console().readLine();
      }

    } while(!currentAction.equals("end_conversation"));

    // We're done, so we delete the session.
    DeleteSessionOptions deleteSessionOptions = new DeleteSessionOptions.Builder(assistantId, sessionId).build();
    service.deleteSession(deleteSessionOptions).execute();
  }
}
```
{: codeblock}
{: java}

Ora l'applicazione controlla l'array delle `azioni` nella risposta per vedere se è presente un'azione con `type`=`client`. Se presente, controlla il valore `name` dell'azione ed esegue l'azione appropriata (visualizzando l'ora di sistema locale o impostando un indicatore interno indicante che la conversazione è terminata).

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

Ci siamo riusciti! L'applicazione ora utilizza il servizio {{site.data.keyword.conversationshort}} per identificare gli intenti nell'input in linguaggio naturale, visualizza le risposte appropriate e implementa le azioni client richieste.

Naturalmente, un'applicazione del mondo reale può utilizzare un'interfaccia utente più sofisticata, ad esempio una finestra di chat web. E può implementare azioni più complesse, possibilmente integrando un database clienti o altri sistemi aziendali. Dovrebbe anche inviare dati aggiuntivi all'assistente, ad esempio un ID utente per identificare ciascun utente univoco. Ma i principi di base su come l'applicazione interagisce con il servizio {{site.data.keyword.conversationshort}} restano uguali.

Per alcuni esempi più complessi, vedi [Applicazioni di esempio](/docs/services/assistant?topic=assistant-sample-apps.

## Contesto di accesso
{: #api-client-get-context}

Il *contesto* è un oggetto che contiene variabili che persistono per tutta una conversazione e che possono essere condivise dal dialogo e dall'applicazione client. Se la tua applicazione sta utilizzando l'API v2, il contesto viene mantenuto automaticamente dall'assistente per ogni singola sessione. Sia il dialogo che l'applicazione client possono leggere e scrivere variabili di contesto. Per impostazione predefinita, il contesto non viene restituito a un'applicazione client, ma puoi, facoltativamente, richiedere che venga incluso nella risposta in ogni richiesta `/message`.

**Importante:** un uso del contesto è quello di specificare un ID utente univoco per ciascun utente finale che interagisce con l'assistente. Per i piani basati sull'utente, questo ID viene utilizzato a scopo di fatturazione. (Per ulteriori informazioni, vedi [Piani basati sull'utente](/docs/services/assistant?topic=assistant-services-information#user-based-plans).)

Esistono due tipi di contesto:

- **Contesto globale**: le variabili di contesto che vengono condivise da tutte le capacità utilizzate da un assistente, incluse le variabili di sistema interne utilizzate per gestire il flusso della conversazione.

- **Contesto specifico della capacità**: le variabili di contesto specifiche per una particolare capacità, incluse le variabili definite dall'utente di cui necessita la tua applicazione. Attualmente, è supportata solo una capacità (denominata `main skill`).

Il seguente esempio mostra una richiesta `/message` che include sia variabili di contesto globali che specifiche della capacità; utilizza anche la proprietà `options.return_context` per richiedere che il contesto venga restituito con la risposta.

```javascript
service.message({
  assistant_id: '{assistant_id}',
  session_id: '{session_id}',
  input: {
    'message_type': 'text',
    'text': 'Hello',
    'options': {
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
}, function(err, response) {
  if (err)
    console.log('error:', err);
  else
    console.log(JSON.stringify(response, null, 2));
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

    MessageResponse response = service.message(options).execute();

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

Per informazioni dettagliate su come accedere alle variabili di contesto utilizzando l'API, vedi il [Riferimento API v2 ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://{DomainName}/apidocs/assistant-v2#send-user-input-to-assistant){: new_window}.)

## Utilizzo dell'API v1
{: #api-client-v1-api}

L'utilizzo dell'API v2 è il metodo consigliato per creare un'applicazione client di runtime che comunica con il servizio {{site.data.keyword.conversationshort}}. Tuttavia, è possibile che alcune applicazioni più vecchie utilizzino ancora l'API v1 che include un metodo di runtime simile per l'invio dei messaggi allo spazio di lavoro all'interno di una capacità di dialogo. 

Tieni presente che se la tua applicazione utilizza l'API v1, comunica direttamente con lo spazio di lavoro, escludendo le capacità di orchestrazione e di gestione dello stato dell'assistente. Ciò significa che la tua applicazione è responsabile della gestione delle informazioni sullo stato utilizzando il contesto. La tua applicazione deve mantenere il contesto salvando quello ricevuto da ciascuna risposta e reinviandolo al servizio con ogni nuova richiesta di messaggio. (Il metodo v1 `/message` restituisce sempre il contesto con ogni risposta.)

Per ulteriori informazioni sul metodo v1 `/message` e sul contesto, vedi il [Riferimento API v1 ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://cloud.ibm.com/apidocs/assistant#get-response-to-user-input){: new_window}.
