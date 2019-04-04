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

# Création d'une application client
{: #api-client}

Vous disposez d'une compétence de dialogue de travail et d'un assistant qui l'utilise. A présent, vous souhaitez développer une application client personnalisée qui va interagir avec vos utilisateurs et communiquer avec le service {{site.data.keyword.conversationfull}}.
{: shortdesc}

## Configuration du service {{site.data.keyword.conversationshort}}
{: #api-client-setup}

L'exemple d'application que nous allons créer au cours de cette section implémente plusieurs fonctions d'un assistant personnel cognitif. Le code d'application se connectera à un assistant {{site.data.keyword.conversationshort}} dans lequel aura lieu le traitement cognitif (par exemple, la détection des intentions d'utilisateur).

Avant de continuer, vous devez configurer l'assistant nécessaire :

1.  Téléchargez la compétence de dialogue <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/assistant/assistant-simple-example.json" download="assistant-simple-example.json">fichier JSON</a>.
1.  [Importez la compétence](/docs/services/assistant?topic=assistant-skill-add#creating-skills) dans une instance du service {{site.data.keyword.conversationshort}}.
1.  [Créez un assistant](/docs/services/assistant?topic=assistant-assistant-add#creating-assistants) et connectez la compétence vous avez importée.

## Obtention d'informations sur le service
{: #api-client-get-info}

Pour accéder aux API REST du service {{site.data.keyword.conversationshort}}, votre application doit pouvoir s'authentifier auprès d'{{site.data.keyword.Bluemix}} et se connecter à l'assistant approprié. Vous devrez copier les données d'identification du service et l'ID d'assistant et les coller dans votre code d'application.

Pour accéder aux données d'identification du service et à l'ID d'assistant à partir de l'outil {{site.data.keyword.conversationshort}}, accédez à l'onglet **Assistants**, puis cliquez sur le ![Menu](images/kabob-grey.png) correspondant à l’assistant auquel vous souhaitez vous connecter. Sélectionnez **View API Details** pour afficher les détails de l'assistant, y compris son ID et la clé de l'API.

Vous pouvez également accéder aux données d'identification du service à partir de votre tableau de bord {{site.data.keyword.Bluemix_short}}.

## Communication avec le service {{site.data.keyword.conversationshort}}
{: #api-client-communicate}

Il est facile d'interagir avec le service {{site.data.keyword.conversationshort}}. Examinons un exemple qui se connecte au service, envoie un message et consigne la sortie sur la console :

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

La première étape consiste à créer un encapsuleur pour le service {{site.data.keyword.conversationshort}}.

L'encapsuleur est un objet utilisé pour envoyer des entrées au service et recevoir des sorties du service. Lorsque vous créez l'encapsuleur de service, spécifiez les données d'authentification provenant de la clé de service, ainsi que la version de l'API {{site.data.keyword.conversationshort}} que vous utilisez.

Dans cet exemple d'application Node.js, l'encapsuleur est une instance de `AssistantV2`, stockée dans la variable `service`. Les logiciels SDK Watson correspondant aux autres langages fournissent des mécanismes équivalents pour l'instanciation d'un encapsuleur de service.
{: javascript}

Dans cet exemple Python, l'encapsuleur est une instance de `watson_developer_cloud.AssistantV2`, stockée dans la variable `service`. Les logiciels SDK Watson correspondant aux autres langages fournissent des mécanismes équivalents pour l'instanciation d'un encapsuleur de service.
{: python}

Dans cet exemple Java, l'encapsuleur est une instance d'`Assistant`, stockée dans la variable `service`. Les logiciels SDK Watson correspondant aux autres langages fournissent des mécanismes équivalents pour l'instanciation d'un encapsuleur de service.
{: java}

Après avoir créé l'encapsuleur de service, nous l’utilisons pour créer une session et envoyer un message à l’assistant. Dans cet exemple, le message est vide ; nous voulons juste déclencher le noeud conversation_start dans le dialogue, par conséquent, nous n'avons pas besoin de texte d'entrée. Nous imprimons ensuite le texte de la réponse sur la console et, finalement, nous supprimons la session. 

Utilisez la commande `node <filename.js>` pour exécuter l'exemple d'application.
{: javascript}

Utilisez la commande `python3 <filename.py>` pour exécuter l'exemple d'application.
{: python}

Collez l'exemple de code dans un fichier nommé `AssistantSimpleExample.java`. Vous pouvez ensuite le compiler et l'exécuter.
{: java}

**Remarque :** assurez-vous que vous avez installé le logiciel SDK Watson pour Node.js à l'aide de la commande `npm install watson-developer-cloud`.
{: javascript}

**Remarque :** assurez-vous que vous avez installé le logiciel SDK Watson pour Python à l'aide de la commande `pip install --upgrade watson-developer-cloud` ou `easy_install --upgrade watson-developer-cloud`.
{: python}

**Remarque :** assurez-vous que vous avez installé [Watson SDK for Java ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/watson-developer-cloud/java-sdk/blob/develop/README.md){: new_window}.
{: java}

Si l'on part du principe que tout fonctionne comme prévu, l'assistant renvoie la sortie générée par le dialogue, qui est ensuite consignée sur la console : 

```
Welcome to the Watson Assistant example!
```
{: screen}

Cette sortie nous indique que nous avons réussi à communiquer avec le service {{site.data.keyword.conversationshort}} et reçu le message d'accueil spécifié par le noeud conversation_start dans le dialogue. A présent, nous pouvons ajouter une interface utilisateur afin de traiter les entrées utilisateur.

## Traitement des entrées utilisateur afin de détecter des intentions
{: #api-client-process-input}

Pour pouvoir traiter les entrées utilisateur, nous devons ajouter une interface utilisateur à notre application client. Pour cet exemple, nous allons simplement utiliser des entrées et des sorties standard.
<span class="ph style-scope doc-content" data-hd-programlang="javascript">Pour cela, nous pouvons utiliser le module prompt-sync Node.js. (Vous pouvez installer le module prompt-sync à l'aide de la commande `npm install prompt-sync`.)</span>
<span class="ph style-scope doc-content" data-hd-programlang="python">Pour cela, nous pouvons utiliser la fonction `input` Python 3.</span>
<span class="ph style-scope doc-content" data-hd-programlang="java">Pour cela, nous pouvons utiliser la fonction `Console.readLine` Java.</span>

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

Avec cette version, l'application débute comme précédemment, en envoyant un message vide à l'assistant pour démarrer la conversation.

La fonction `processResponse()` affiche désormais les intentions détectées par la compétence de dialogue parallèlement au texte de sortie. Elle demande ensuite la série d'entrées utilisateur suivante.
{: javascript }

Elle affiche alors les intentions détectées par la compétence de dialogue parallèlement au texte de sortie. Elle demande ensuite la série d'entrées utilisateur suivante.
{: python }

Elle affiche alors les intentions détectées par le dialogue parallèlement au texte de sortie, puis elle demande la série d'entrées utilisateur suivante.
{: java}

Nous n'avons pas encore implémenté de méthode en langage naturel pour mettre fin à la conversation. Nous utilisons donc la commande littérale `quit` pour indiquer que le programme doit supprimer la session et quitter.

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

Bien, là, on avance ! Le service {{site.data.keyword.conversationshort}} reconnaît correctement nos intentions, et le dialogue renvoie le texte de sortie approprié (lorsqu'il est fourni) pour chaque intention.

Cependant, il ne se passe plus rien. Lorsque nous demandons l'heure, nous n'obtenons pas de réponse, lorsque nous disons au-revoir, la conversation ne prend pas fin. Cela est dû au fait que ces intentions nécessitent que des actions supplémentaires soient exécutées par l'application.

## Implémentation d'actions d'application
{: #api-client-implement-actions}

En plus du texte de sortie qui doit être présenté à l'utilisateur, notre dialogue {{site.data.keyword.conversationshort}} utilise le tableau `actions` dans l'objet JSON de la réponse afin de signaler à quel moment l'application doit exécuter une action, en fonction des intentions détectées. Lorsque le dialogue détermine que l'application client doit faire quelque chose, elle renvoie un objet d'action avec le `type` `client`. Le `nom` de l'action indique l'action spécifique, soit `display_time`, soit `end_conversation`. (Des propriétés supplémentaires de l'action peuvent spécifier des paramètres, des données d'identification et d'autres informations relatives à l'action, mais pour notre exemple, nous avons besoin uniquement du nom de l'action.) 

Nous savons que notre dialogue ne demandera jamais plus d'une action à la fois. Par conséquent, notre client n'a plus qu'à vérifier l'existence d'une seule action `client` dans le tableau `actions`. S'il en trouve une, il peut alors exécuter l'action spécifiée. (Avec cette version, les intentions détectées ne sont plus affichées, à présent que nous sommes savons qu'elles sont correctement identifiées.)

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

L'application vérifie maintenant le tableau `actions` dans la réponse pour voir si une action avec `type`=`client` est présente. Si tel est le cas, elle vérifie la valeur `name` de l'action et exécute l'action appropriée (en affichant l'heure système locale ou en définissant un indicateur interne indiquant que la conversation est terminée).

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

Bravo ! A présent, l'application utilise le service {{site.data.keyword.conversationshort}} pour identifier les intentions dans les entrées en langage naturel, affiche les réponses appropriées et implémente les actions client demandées.

Evidemment, une application réelle utiliserait une interface utilisateur plus sophistiquée, par exemple, une fenêtre de discussion Web. Et elle implémenterait des actions plus complexes, en intégrant probablement une base de données client ou d'autres systèmes métier. Il serait également nécessaire d’envoyer des données supplémentaires à l’assistant, par exemple un ID utilisateur, pour identifier chaque utilisateur unique. Mais, les principes de base relatifs à la façon dont l'application interagit avec le service {{site.data.keyword.conversationshort}} restent les mêmes.

Pour des exemples plus complexes, reportez-vous à [Exemples d'application](/docs/services/assistant?topic=assistant-sample-apps.

## Accès au contexte
{: #api-client-get-context}

Le *contexte* est un objet contenant des variables qui persistent dans une conversation et peuvent être partagées par la boîte de dialogue et l'application client. Si votre application utilise l'API v2, le contexte est automatiquement géré par l'assistant session par session. Le dialogue et l'application client peuvent lire et écrire des variables contextuelles. Par défaut, le contexte n'est pas renvoyé à une application client, mais vous pouvez éventuellement demander qu'il soit inclus dans la réponse à chaque demande `/message`.

**Important: **une des utilisations du contexte est de spécifier un ID utilisateur unique pour chaque utilisateur final qui interagit avec l’assistant. Pour les plans basés sur l'utilisateur, cet identifiant est utilisé à des fins de facturation. (Pour plus d'informations, reportez-vous à la rubrique [Forfaits basés sur l'utilisateur](/docs/services/assistant?topic=assistant-services-information#user-based-plans).) 

Il existe deux types de contexte :

- **Contexte global** : variables contextuelles partagées par toutes les compétences utilisées par un assistant, y compris les variables système internes utilisées pour gérer le flux de conversation.

- **Contexte spécifique à une compétence** : variables contextuelles spécifiques à une compétence particulière, y compris les variables définies par l'utilisateur nécessaires à votre application. Actuellement, une seule compétence (nommée `main skill`) est prise en charge. 

L'exemple suivant montre une demande `/message` incluant des variables contextuelles globales et spécifiques à une compétence. Il utilise également la propriété `options.return_context` pour demander que le contexte soit renvoyé avec la réponse.

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

## Utilisation de l'API v1 
{: #api-client-v1-api}

L'utilisation de l'API v2 est la méthode recommandée pour créer une application client d'exécution qui communique avec le service {{site.data.keyword.conversationshort}}. Cependant, certaines applications plus anciennes peuvent toujours utiliser l'API v1, qui inclut une méthode d'exécution similaire pour l'envoi de messages à l'espace de travail dans le cadre d'une compétence de dialogue. 

Notez que si votre application utilise l'API v1, elle communique directement avec l'espace de travail, en contournant les fonctionnalités d'orchestration et de gestion des états de l'assistant. Cela signifie que votre application est responsable de la gestion des informations d'état à l'aide du contexte. Votre application doit conserver le contexte en enregistrant le contexte reçu avec chaque réponse et en le renvoyant au service avec chaque nouvelle demande de message. (La méthode v1 `/message` renvoie toujours le contexte avec chaque réponse.)

Pour plus d'informations sur la méthode et le contexte v1 `/message`, reportez-vous à la rubrique [Référence d'API v1 ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://cloud.ibm.com/apidocs/assistant#get-response-to-user-input){: new_window}.
