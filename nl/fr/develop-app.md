---

copyright:
  years: 2015, 2018
lastupdated: "2018-01-29"

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
{:tip: .tip}

# Création d'une application client

Vous disposez d'un dialogue actif. A présent, vous souhaitez développer l'application qui va interagir avec vos utilisateurs et communiquer avec le service {{site.data.keyword.conversationfull}}.
{: shortdesc}

Vous pouvez visualiser ce tutoriel pour Node.js (Javascript) ou Python en cliquant sur le sélecteur de langage dans l'angle supérieur droit. Pour plus d'informations sur tous les langages pris en charge, reportez-vous à la rubrique [Logiciels SDK {{site.data.keyword.watson}} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](/docs/services/watson/getting-started-sdks.html#sdks){: new_window}.
{: tip }

## Configuration du service {{site.data.keyword.conversationshort}}

L'exemple d'application que nous allons créer au cours de cette section implémente plusieurs fonctions d'un assistant personnel cognitif. Le code d'application se connectera à un espace de travail {{site.data.keyword.conversationshort}} dans lequel aura lieu le traitement cognitif (par exemple, la détection des intentions d'utilisateur).

Avant de continuer, vous devez configurer l'espace de travail {{site.data.keyword.conversationshort}} requis :

1.  Téléchargez le <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/conversation/conversation-simple-example.json" download="conversation-simple-example.json">fichier JSON</a> correspondant à l'espace de travail.
1.  [Importez l'espace de travail](/docs/services/conversation/configure-workspace.html#creating-workspaces) dans une instance du service {{site.data.keyword.conversationshort}}.

## Obtention d'informations sur le service

Pour accéder aux API REST du service {{site.data.keyword.conversationshort}}, votre application doit pouvoir s'authentifier auprès d'{{site.data.keyword.Bluemix}} et se connecter à l'espace de travail {{site.data.keyword.conversationshort}} approprié. Vous devrez copier les données d'identification du service et l'ID d'espace de travail et les coller dans votre code d'application.

Pour accéder aux données d'identification du service et à l'ID d'espace de travail depuis votre espace de travail, sélectionnez le menu ![Menu](images/Menu_16.png), choisissez **Déployer**, puis accédez à l'onglet **Données d'identification**.

Vous pouvez également accéder aux données d'identification du service à partir de votre tableau de bord {{site.data.keyword.Bluemix_short}}. 

## Communication avec le service {{site.data.keyword.conversationshort}}

Il est facile d'interagir avec le service {{site.data.keyword.conversationshort}}. Examinons un exemple qui se connecte au service, envoie un message et consigne la sortie sur la console : 

```javascript
// Example 1: sets up service wrapper, sends initial message, and
// receives response.

var ConversationV1 = require('watson-developer-cloud/conversation/v1');

// Set up Conversation service wrapper.
var conversation = new ConversationV1({
  username: 'USERNAME', // replace with service username
  password: 'PASSWORD', // replace with service password
  version_date: '2017-05-26'
});

var workspace_id = 'WORKSPACE_ID'; // replace with workspace ID

// Start conversation with empty message.
conversation.message({
  workspace_id: workspace_id
  }, processResponse);

// Process the conversation response.
function processResponse(err, response) {
  if (err) {
    console.error(err); // something went wrong
    return;
  }
  
  // Display the output from dialog, if any.
  if (response.output.text.length != 0) {
      console.log(response.output.text[0]);
  }
}
```
{: codeblock}
{: javascript}

```python
# Example 1: sets up service wrapper, sends initial message, and
# receives response.

import watson_developer_cloud

# Set up Conversation service.
conversation = watson_developer_cloud.ConversationV1(
  username = 'USERNAME', # replace with username from service key
  password = 'PASSWORD', # replace with password from service key
  version = '2017-05-26'
)
workspace_id = 'WORKSPACE_ID' # replace with workspace ID

# Start conversation with empty message.
response = conversation.message(
  workspace_id = workspace_id,
  input = {
    'text': ''
  }
)

# Print the output from dialog, if any.
if response['output']['text']:
  print(response['output']['text'][0])
```
{: codeblock}
{: python}

La première étape consiste à créer un encapsuleur pour le service {{site.data.keyword.conversationshort}}.

L'encapsuleur est un objet utilisé pour envoyer des entrées au service et recevoir des sorties du service. Lorsque vous créez l'encapsuleur de service, spécifiez les données d'authentification provenant de la clé de service, ainsi que la version de l'API {{site.data.keyword.conversationshort}} que vous utilisez.

Dans cet exemple d'application Node.js, l'encapsuleur est une instance de `ConversationV1`, stockée dans la variable `conversation`. Les logiciels SDK Watson correspondant aux autres langages fournissent des mécanismes équivalents pour l'instanciation d'un encapsuleur de service.
{: javascript}

Dans cet exemple Python, l'encapsuleur est une instance de `watson_developer_cloud.ConversationV1`, stockée dans la variable `conversation`. Les logiciels SDK Watson correspondant aux autres langages fournissent des mécanismes équivalents pour l'instanciation d'un encapsuleur de service.
{: python}

Une fois l'encapsuleur de service créé, nous l'utilisons pour envoyer un message au service {{site.data.keyword.conversationshort}}. Dans cet exemple, le message est vide ; nous voulons juste déclencher le noeud conversation_start dans le dialogue, par conséquent, nous n'avons pas besoin de texte d'entrée.

Utilisez la commande `node <filename.js>` pour exécuter l'exemple d'application.
{: javascript}

Utilisez la commande `python <filename.py>` pour exécuter l'exemple d'application.
{: python}

**Remarque :** assurez-vous que vous avez installé le logiciel SDK Watson pour Node.js à l'aide de la commande `npm install watson-developer-cloud`.
{: javascript}

**Remarque :** assurez-vous que vous avez installé le logiciel SDK Watson pour Python à l'aide de la commande `pip install --upgrade watson-developer-cloud` ou `easy_install --upgrade watson-developer-cloud`.
{: python}

Si l'on part du principe que tout fonctionne comme prévu, le service {{site.data.keyword.conversationshort}} renvoie la sortie générée par le dialogue, qui est ensuite consignée sur la console : 

```
Welcome to the {{site.data.keyword.conversationshort}} example!
```
{: screen}

Cette sortie nous indique que nous avons réussi à communiquer avec le service {{site.data.keyword.conversationshort}} et reçu le message d'accueil spécifié par le noeud conversation_start dans le dialogue. A présent, nous pouvons ajouter une interface utilisateur afin de traiter les entrées utilisateur.

## Traitement des entrées utilisateur afin de détecter des intentions

Pour pouvoir traiter les entrées utilisateur, nous devons ajouter une interface utilisateur à notre application. Pour cet exemple, nous allons simplement utiliser des entrées et des sorties standard.
<span class="ph style-scope doc-content" data-hd-programlang="javascript">Pour cela, nous pouvons utiliser le module prompt-sync Node.js. (Vous pouvez installer le module prompt-sync à l'aide de la commande `npm install prompt-sync`.)</span>
<span class="ph style-scope doc-content" data-hd-programlang="python">Pour cela, nous pouvons utiliser la fonction `input` Python.</span>

```javascript
// Example 2: adds user input and detects intents.

var prompt = require('prompt-sync')();
var ConversationV1 = require('watson-developer-cloud/conversation/v1');

// Set up Conversation service wrapper.
var conversation = new ConversationV1({
  username: 'USERNAME', // replace with service username
  password: 'PASWORD', // replace with service password
  version_date: '2017-05-26'
});

var workspace_id = 'WORKSPACE_ID'; // replace with workspace ID

// Start conversation with empty message.
conversation.message({
  workspace_id: workspace_id
  }, processResponse);

// Process the conversation response.
function processResponse(err, response) {
  if (err) {
    console.error(err); // something went wrong
    return;
  }

  // If an intent was detected, log it out to the console.
  if (response.intents.length > 0) {
    console.log('Detected intent: #' + response.intents[0].intent);
  }

  // Display the output from dialog, if any.
  if (response.output.text.length != 0) {
      console.log(response.output.text[0]);
  }

  // Prompt for the next round of input.
  var newMessageFromUser = prompt('>> ');
  conversation.message({
    workspace_id: workspace_id,
    input: { text: newMessageFromUser }
    }, processResponse)
}
```
{: codeblock}
{: javascript}

```python
# Example 2: adds user input and detects intents.

import watson_developer_cloud

# Set up Conversation service.
conversation = watson_developer_cloud.ConversationV1(
  username = 'USERNAME', # replace with username from service key
  password = 'PASSWORD', # replace with password from service key
  version = '2017-05-26'
)
workspace_id = 'WORKSPACE_ID' # replace with workspace ID

# Initialize with empty value to start the conversation.
user_input = ''

# Main input/output loop
while True:

  # Send message to Conversation service.
  response = conversation.message(
    workspace_id = workspace_id,
    input = {
      'text': user_input
    }
  )

  # If an intent was detected, print it to the console.
  if response['intents']:
    print('Detected intent: #' + response['intents'][0]['intent'])

  # Print the output from dialog, if any.
  if response['output']['text']:
    print(response['output']['text'][0])

  # Prompt for next round of input.
  user_input = input('>> ')
```
{: codeblock }
{: python }

Avec cette version, l'application débute comme précédemment, en envoyant un message vide au service {{site.data.keyword.conversationshort}} pour démarrer la conversation.

La fonction `processResponse()` affiche désormais les intentions détectées par le dialogue parallèlement au texte de sortie, puis elle demande la série d'entrées utilisateur suivante.
{: javascript }

Elle affiche alors les intentions détectées par le dialogue parallèlement au texte de sortie, puis elle demande la série d'entrées utilisateur suivante. (Pour le moment, nous utilisons une boucle `while True` car nous n'avons pas encore implémenté une méthode permettant de mettre fin à la conversation.)
{: python }

Mais, il y a quelque chose qui ne va pas :

```
Welcome to the {{site.data.keyword.conversationshort}} example!
>> hello
Detected intent: #hello
Welcome to the {{site.data.keyword.conversationshort}} example!
>> what time is it?
Detected intent: #time
Welcome to the {{site.data.keyword.conversationshort}} example!
>> goodbye
Detected intent: #goodbye
Welcome to the {{site.data.keyword.conversationshort}} example!
>>
```
{: screen}

Le service {{site.data.keyword.conversationshort}} détecte les intentions appropriées, pourtant, chaque échange de la conversation renvoie le message d'accueil émis par le noeud conversation_start (`Welcome to the {{site.data.keyword.conversationshort}} example!`).

Cela se produit car le service {{site.data.keyword.conversationshort}} est sans état. Il revient à l'application de préserver les informations d'état. Etant donné que pour l'instant, nous ne faisons rien pour préserver les informations d'état, le service {{site.data.keyword.conversationshort}} perçoit chaque cycle d'entrées utilisateur comme le premier échange d'une nouvelle conversation et il déclenche le noeud conversation_start.

## Préservation des informations d'état

Les informations d'état relatives à votre conversation sont préservées à l'aide du *contexte*. Le contexte est un objet JSON qui va et vient entre votre application et le service {{site.data.keyword.conversationshort}}. Il revient à votre application de préserver le contexte entre un échange de la conversation et le suivant.

Le contexte inclut un identificateur unique pour chaque conversation avec un utilisateur, ainsi qu'un compteur qui est incrémenté avec chaque échange de la conversation. Dans la version précédente de l'exemple, le contexte n'était pas préservé, et chaque cycle d'entrées apparaissait comme étant le début d'une nouvelle conversation. Pour remédier à cela, il convient de sauvegarder le contexte et de le renvoyer au service {{site.data.keyword.conversationshort}} à chaque fois.

Outre la préservation de la place dans la conversation, le contexte permet également de stocker d'autres données, quelles qu'elles soient, qui doivent aller et venir entre votre application et le service {{site.data.keyword.conversationshort}}. Il peut s'agir de données persistantes que vous souhaitez préserver tout au long de la conversation (par exemple, le nom ou le numéro de compte d'un client) ou toute autre donnée dont vous souhaitez assurer le suivi (par exemple, le statut en cours de paramètres d'option).

```javascript
// Example 3: maintains state.

var prompt = require('prompt-sync')();
var ConversationV1 = require('watson-developer-cloud/conversation/v1');

// Set up Conversation service wrapper.
var conversation = new ConversationV1({
  username: 'USERNAME', // replace with service username
  password: 'PASSWORD', // replace with service password
  version_date: '2017-05-26'
});

var workspace_id = 'WORKSPACE_ID'; // replace with workspace ID

// Start conversation with empty message.
conversation.message({
  workspace_id: workspace_id
  }, processResponse);

// Process the conversation response.
function processResponse(err, response) {
  if (err) {
    console.error(err); // something went wrong
    return;
  }

  // If an intent was detected, log it out to the console.
  if (response.intents.length > 0) {
    console.log('Detected intent: #' + response.intents[0].intent);
  }
  
  // Display the output from dialog, if any.
  if (response.output.text.length != 0) {
      console.log(response.output.text[0]);
  }

  // Prompt for the next round of input.
    var newMessageFromUser = prompt('>> ');
    // Send back the context to maintain state.
    conversation.message({
      workspace_id: workspace_id,
      input: { text: newMessageFromUser },
      context : response.context,
    }, processResponse)
}
```
{: codeblock}
{: javascript }

```python
# Example 3: maintains state.

import watson_developer_cloud

# Set up Conversation service.
conversation = watson_developer_cloud.ConversationV1(
  username = 'USERNAME', # replace with username from service key
  password = 'PASSWORD', # replace with password from service key
  version = '2017-05-26'
)
workspace_id = 'WORKSPACE_ID' # replace with workspace ID

# Initialize with empty value to start the conversation.
user_input = ''

context = {}

# Main input/output loop
while True:

  # Send message to Conversation service.
  response = conversation.message(
    workspace_id = workspace_id,
    input = {
      'text': user_input
    },
    context = context
  )

  # If an intent was detected, print it to the console.
  if response['intents']:
    print('Detected intent: #' + response['intents'][0]['intent'])

  # Print the output from dialog, if any.
  if response['output']['text']:
    print(response['output']['text'][0])

  # Update the stored context with the latest received from the dialog.
  context = response['context']

  # Prompt for next round of input.
  user_input = input('>> ')
```
{: codeblock }
{: python }

Le seul changement par rapport à l'exemple précédent réside dans le fait que maintenant, avec chaque échange de la conversation, nous renvoyons l'objet `response.context` que nous avons reçu au cours de l'échange précédent :
{: javascript }

```javascript
    conversation.message({
      input: { text: newMessageFromUser },
      context : response.context,
    }, processResponse)
```
{: codeblock}
{: javascript }

Le seul changement par rapport à l'exemple précédent réside dans le fait que désormais, nous stockons le contexte reçu du dialogue dans une variable nommée `context` et que nous le renvoyons avec la série d'entrées utilisateur suivante :
{: python }

```python
  response = conversation.message(
    workspace_id = workspace_id,
    input = {
      'text': user_input
    },
    context = context
  )
```
{: codeblock }
{: python }

Cela permet de garantir que le contexte est préservé d'un échange à l'autre, et par conséquent, le service {{site.data.keyword.conversationshort}} ne considère plus que chaque échange est le premier :

```
>> hello
Detected intent: #hello
Good day to you.
>> what time is it?
Detected intent: #time
>> goodbye
Detected intent: #goodbye
OK! See you later.
>>
```
{: screen}

Bien, là, on avance ! Le service {{site.data.keyword.conversationshort}} reconnaît correctement nos intentions, et le dialogue renvoie le texte de sortie approprié (lorsqu'il est fourni) pour chaque intention.

Cependant, il ne se passe plus rien. Lorsque nous demandons l'heure, nous n'obtenons pas de réponse, lorsque nous disons au-revoir, la conversation ne prend pas fin. Cela est dû au fait que ces intentions nécessitent que des actions supplémentaires soient exécutées par l'application.

## Implémentation d'actions d'application

En plus du texte de sortie qui doit être présenté à l'utilisateur, notre dialogue utilise l'objet `output` dans l'objet JSON de la réponse afin de signaler à quel moment l'application doit exécuter une action, en fonction des intentions détectées.

Ces indicateurs d'action sont envoyés à l'aide de la propriété `action`, que notre dialogue définit dans le cadre de l'objet JSON de la réponse. Lorsque le dialogue détermine que l'application doit faire quelque chose, il affecte à la propriété `action` la valeur appropriée, `display_time` ou `end_conversation`.

Gardez à l'esprit que `output` est juste un objet JSON et que vous pouvez y ajouter n'importe quel contenu valide de votre choix. Pour une application plus complexe, vous pouvez utiliser un tableau comportant plusieurs indicateurs d'action.

Mais, dans le cadre de notre exemple, nous utilisons une paire clé-valeur simple qui prend en charge un seul indicateur d'action. Notre code d'application doit rechercher la valeur de la propriété `action` dans la réponse, puis exécuter l'action spécifiée. (Avec cette version, les intentions détectées ne sont plus affichées, à présent que nous sommes savons qu'elles sont correctement identifiées.)

```javascript
// Example 4: implements app actions.

var prompt = require('prompt-sync')();
var ConversationV1 = require('watson-developer-cloud/conversation/v1');

// Set up Conversation service wrapper.
var conversation = new ConversationV1({
  username: 'USERNAME', // replace with service username
  password: 'PASSWORD', // replace with service password
  version_date: '2017-05-26'
});

var workspace_id = 'WORKSPACE_ID'; // replace with workspace ID

// Start conversation with empty message.
conversation.message({
  workspace_id: workspace_id
  }, processResponse);

// Process the conversation response.
function processResponse(err, response) {
  if (err) {
    console.error(err); // something went wrong
    return;
  }

  var endConversation = false;
  
  // Check for action flags.
  if (response.output.action === 'display_time') {
    // User asked what time it is, so we output the local system time.
    console.log('The current time is ' + new Date().toLocaleTimeString());
  } else if (response.output.action === 'end_conversation') {
    // User said goodbye, so we're done.
    console.log(response.output.text[0]);
    endConversation = true;
  } else {
    // Display the output from dialog, if any.
    if (response.output.text.length != 0) {
        console.log(response.output.text[0]);
    }
  }

  // If we're not done, prompt for the next round of input.
  if (!endConversation) {
    var newMessageFromUser = prompt('>> ');
    conversation.message({
      workspace_id: workspace_id,
      input: { text: newMessageFromUser },
      // Send back the context to maintain state.
      context : response.context,
    }, processResponse)
  }
}
```
{: codeblock}
{: javascript}

```python
# Example 4: implements app actions.

import watson_developer_cloud
import time

# Set up Conversation service.
conversation = watson_developer_cloud.ConversationV1(
  username = 'USERNAME', # replace with username from service key
  password = 'PASSWORD', # replace with password from service key
  version = '2017-05-26'
)
workspace_id = 'WORKSPACE_ID' # replace with workspace ID

# Initialize with empty value to start the conversation.
user_input = ''
context = {}
current_action = ''

# Main input/output loop
while current_action != 'end_conversation':

  # Send message to Conversation service.
  response = conversation.message(
    workspace_id = workspace_id,
    input = {
      'text': user_input
    },
    context = context
  )

  # Print the output from dialog, if any.
  if response['output']['text']:
    print(response['output']['text'][0])

  # Update the stored context with the latest received from the dialog.
  context = response['context']
  # Check for action flags sent by the dialog.
  if 'action' in response['output']:
    current_action = response['output']['action']
  # User asked what time it is, so we output the local system time.
  if current_action == 'display_time':
    print('The current time is ' + time.strftime('%I:%M:%S %p'))
  # If we're not done, prompt for next round of input.
  if current_action != 'end_conversation':
    user_input = input('>> ')
```
{: codeblock}
{: python}

Cette fois-ci, la fonction processResponse() recherche la valeur de la propriété `action` de l'objet `output` reçu de la part du service {{site.data.keyword.conversationshort}}. Si la valeur est `display_time` ou `end_conversation`, l'application exécute l'action appropriée.
{: javascript}

L'application recherche désormais la valeur de la propriété `action` de l'objet `output` reçu de la part du service {{site.data.keyword.conversationshort}}. Si la valeur est `display_time`, l'application exécute l'action appropriée.
Si la valeur est `end_conversation`, l'application sait qu'elle ne doit pas demander d'autres entrées utilisateur et la boucle `while` se termine.
{: python}

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

Evidemment, une application réelle utiliserait une interface utilisateur plus sophistiquée, par exemple, une fenêtre de discussion Web. Et elle implémenterait des actions plus complexes, en intégrant probablement une base de données client ou d'autres systèmes métier. Mais, les principes de base relatifs à la façon dont l'application interagit avec le service {{site.data.keyword.conversationshort}} restent les mêmes.

Pour voir des exemples plus complexes, examinez les modèles d'application dans le panneau de navigation.
