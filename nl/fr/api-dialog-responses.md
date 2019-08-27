---

copyright:
  years: 2015, 2019
lastupdated: "2019-05-14"

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

# Mise en oeuvre des réponses
{: #api-dialog-responses}

Un noeud de dialogue peut répondre aux utilisateurs à l'aide de texte, d'images ou d'éléments interactifs tels que des options cliquables. Si vous créez votre propre application client, vous devez implémenter l'affichage correct de tous les types de réponse renvoyés par votre dialogue. (Pour plus d'informations sur les réponses de dialogue, reportez-vous à la rubrique [Réponses](/docs/services/assistant?topic=assistant-dialog-overview#dialog-overview-responses)).

## Format de sortie des réponses
{: #api-dialog-responses-output}

Par défaut, les réponses issues d'un noeud de dialogue sont spécifiées dans l'objet `output.generic` dans la réponse JSON renvoyée à partir de l'API `/message`. L'objet `generic` contient un tableau de 5 éléments de réponse au maximum, destiné à n'importe quel canal. L'exemple JSON suivant montre une réponse comprenant du texte et une image :

```json
{
  "output": {
    "generic":[
      {
        "response_type": "text",
        "text": "Très bien, voici la photo d'un chien."
      },
      {
        "response_type": "image",
        "source": "http://example.com/dog.jpg"
      }
    ],
    "text" : ["Très bien, voici la photo d'un chien."]
  }
}
```

Comme le montre cet exemple, la réponse textuelle (`Très bien, voici la photo d'un chien.`) est également renvoyée dans le tableau `output.text`. Ceci est inclus à des fins de compatibilité ascendante des applications plus anciennes qui ne prennent pas en charge le format `output.generic`.
{: note}

Il incombe à votre application client de gérer tous les types de réponses de manière appropriée. Dans ce cas, votre application devra présenter à l'utilisateur le texte et l'image spécifiés.

## Types de réponse
{: #api-dialog-responses-types}

Chaque élément d'une réponse correspond à l'un des types de réponse pris en charge (actuellement, `image`, `option`, `pause`, `text` et `suggestion`). Chaque type de réponse est spécifié à l'aide d'un ensemble différent de propriétés JSON. Par conséquent, les propriétés incluses pour chaque réponse varient en fonction du type de réponse. Pour des informations complètes sur le modèle de réponse de l'API `/message`, reportez-vous à la [Référence d'API ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://{DomainName}/apidocs/assistant-v2#send-user-input-to-assistant){: new_window}.)

Cette section décrit les types de réponse disponibles et leur représentation dans le code JSON de réponse de l'API `/message`. (Si vous utilisez le SDK Watson, vous pouvez utiliser les interfaces fournies dans votre langue pour accéder aux mêmes objets.)

Les exemples de cette section montrent le format des données JSON renvoyées par l'API `/message` au moment de l'exécution. N'oubliez pas que ce format diffère du format JSON utilisé pour définir les réponses dans un noeud de dialogue. Pour plus d'informations, reportez-vous à la rubrique [Définition de réponses à l'aide de l'éditeur JSON](/docs/services/assistant?topic=assistant-dialog-responses-json).
{: note}

### Texte
{: #api-dialog-responses-text}

Le type de réponse `text` est utilisé pour les réponses textuelles ordinaires provenant du dialogue :

```json
{
  "output": {
    "generic":[
      {
        "response_type": "text",
        "text": "D'accord, vous souhaitez connaître les vols pour Boston lundi prochain." }
    ]
  }
}
```

Notez que, pour des raisons de compatibilité ascendante, le même texte est également inclus dans le tableau `output.text` de la réponse du dialogue (comme indiqué dans l'exemple au début de cette page).

### Image
{: #api-dialog-responses-image}

Le type de réponse `image` indique à l'application client d'afficher une image, éventuellement accompagnée d'un titre et d'une description :

```json
{
  "output": {
    "generic":[
      {
        "response_type": "image",
        "source": "http://example.com/image.jpg",
        "title": "Exemple d'image",
        "description": "Ceci est un exemple d'image"
      }
    ]
  }
}
```

Votre application est chargée d'extraire l'image spécifiée par la propriété `source` et de l'afficher à l'utilisateur. Si les éléments facultatifs `title` et `description` sont fournis, votre application peut les afficher de la manière appropriée (par exemple, en affichant le titre sous l'image et la description sous forme d'infobulle).

### Pause
{: #api-dialog-responses-pause}

Le type de réponse `pause` indique à l'application d'attendre un intervalle spécifié avant d'afficher la réponse suivante :

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

Cette pause peut être demandée par le dialogue pour laisser le temps nécessaire à l'élaboration d'une demande ou simplement pour imiter le comportement d'un agent humain susceptible de faire une pause entre les réponses. La pause peut durer jusqu’à 10 secondes.

Une réponse `pause` est généralement envoyée en association avec d'autres réponses. Votre application doit faire une pause pendant l'intervalle spécifié par la propriété `time` (en millisecondes) avant d'afficher la réponse suivante dans le tableau. La propriété facultative `typing` demande à l'application client d'afficher un indicateur "l'utilisateur est en train d'écrire", si cette fonction est prise en charge, afin de simuler un agent humain.

### Option
{: #api-dialog-responses-option}

Le type de réponse `option` indique à l'application client d'afficher un contrôle d'interface utilisateur permettant à l'utilisateur d'effectuer un choix dans une liste d'options, puis de renvoyer l'entrée à l'assistant en fonction de l'option sélectionnée :

```json
{
  "output": {
    "generic":[
      {
        "response_type": "option",
        "title": "Options disponibles",
        "description": "Veuillez sélectionner l'une des options suivantes :",
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

Votre application peut afficher les options spécifiées à l'aide de tout contrôle d'interface utilisateur approprié (par exemple, un ensemble de boutons ou une liste déroulante). La propriété facultative `preference` indique le type de contrôle préféré que votre application doit utiliser (`bouton` ou `liste déroulante`), s'il est pris en charge. Pour une expérience utilisateur optimale, une bonne pratique consiste à présenter trois options ou moins sous forme de boutons, et plus de trois options sous forme de liste déroulante. 

Pour chaque option, la propriété `label` spécifie le texte de l'étiquette qui doit apparaître pour l'option dans le contrôle d'interface utilisateur. La propriété `value` spécifie l'entrée qui doit être renvoyée à l'assistant (à l'aide de l'API `/message`) lorsque l'utilisateur sélectionne l'option correspondante. 

Pour un exemple d'implémentation des réponses d'`option` dans une application client simple, reportez-vous à la rubrique [Exemple : implémentation des réponses d'option](#api-dialog-option-example).

### Suggestion
{: #api-dialog-responses-suggestion}

Cette fonction est disponible uniquement pour les utilisateurs du forfait Plus ou Premium.
{: tip}

Le type de réponse `suggestion` est utilisé par la fonctionnalité de désambiguïsation pour suggérer des correspondances possibles lorsque les objectifs de l'utilisateur ne sont pas clairs. Une réponse `suggestion` comprend un tableau de `suggestions`, chacune correspondant à un nœud de dialogue correspondant possible :

```json
{
  "output": {
    "generic":[
      {
        "response_type": "suggestion",
        "title": "Vouliez-vous dire :",
        "suggestions": [
          {
            "label": "Je voudrais commander à boire.",
            "value": {
              "intents": [
                {
                  "intent": "order_drink",
                  "confidence": 0.7330395221710206
                }
              ],
              "entities": [],
              "input": {
                "suggestion_id": "576aba3c-85b9-411a-8032-28af2ba95b13",
                "text": "Je voudrais passer une commande"
              }
            },
  "output": {
              "text": [
                "Je vous apporte à boire."
              ],
              "generic": [
                {
                  "response_type": "text",
                  "text": "Je vous apporte à boire."
                }
              ],
              "nodes_visited_details": [
                {
                  "dialog_node": "node_1_1547675028546",
                  "title": "order drink",
                  "user_label": "Je voudrais commander à boire.",
                  "conditions": "#order_drink"
                }
              ]
            },
            "source_dialog_node": "root"
          },
          {
            "label": "Je voudrais un autre verre.",
            "value": {
              "intents": [
                {
                  "intent": "refill_drink",
                  "confidence": 0.2529746770858765
                }
              ],
              "entities": [],
              "input": {
                "suggestion_id": "6583b547-53ff-4e7b-97c6-4d062270abcd",
                "text": "Je voudrais un autre verre"
              }
            },
  "output": {
              "text": [
                "Je vous apporte un autre verre."
              ],
              "generic": [
                {
                  "response_type": "text",
                  "text": "Je vous apporte un autre verre."
                }
              ],
              "nodes_visited_details": [
                {
                  "dialog_node": "node_2_1547675097178",
                  "title": "refill drink",
                  "user_label": "Je voudrais un autre verre.",
                  "conditions": "#refill_drink"
                }
              ]
            },
            "source_dialog_node": "root"
          }
        ]
      }
    ],
  }
}
```

Notez que la structure d'une réponse `suggestion` est très similaire à la structure d'une réponse d'`option`. Comme pour les options, chaque suggestion inclut un `libellé` pouvant être affiché à l'utilisateur et une `valeur` spécifiant l'entrée qui doit être renvoyée à l'assistant si l'utilisateur choisit la suggestion correspondante. Pour implémenter les réponses `suggestion` dans votre application, vous pouvez utiliser la même approche que celle que vous utilisiez pour les réponses `option`.

Pour plus d'informations sur la fonctionnalité de désambiguïsation, reportez-vous à la rubrique [Désambiguïsation](/docs/services/assistant?topic=assistant-dialog-runtime#dialog-runtime-disambiguation).

## Exemple : implémentation des réponses d'option 
{: #api-dialog-option-example}

Pour montrer comment une application client peut gérer les réponses d'option, qui invitent l’utilisateur à faire une sélection dans une liste de choix, nous pouvons étendre l’exemple de client décrit dans la rubrique [Création d'une application client](/docs/services/assistant?topic=assistant-api-client). Il s'agit d'une application client simplifiée qui utilise les entrées et sorties standard pour gérer trois intentions (envoi d'un message d'accueil, affichage de l'heure actuelle et sortie de l'application) : 

```
Bienvenue dans l'exemple {{site.data.keyword.conversationshort}} !
>> Bonjour
Bonjour.
>> quelle heure est-il ?
Il est actuellement 12 heures 40 minutes et 42 secondes.
>> Au revoir
Très bien ! À plus tard.
```
{: screen}

Si vous souhaitez essayer l'exemple de code présenté dans cette rubrique, vous devez d'abord configurer l'espace de travail requis et obtenir les détails de l'API dont vous aurez besoin. Pour plus d'informations, reportez-vous à la rubrique [Création d'une application client](/docs/services/assistant?topic=assistant-api-client).
{: note}

### Réception d'une réponse d'option 

La réponse d'`option` peut être utilisée lorsque vous souhaitez présenter à l'utilisateur une liste de choix finie, plutôt que d'interpréter une entrée en langage naturel. Elle peut être utilisée dans toutes les situations où vous souhaitez permettre à l'utilisateur de faire un choix rapidement dans un ensemble d'options non ambiguës. 

Dans notre application client simplifiée, nous utilisons cette fonctionnalité pour effectuer une sélection dans une liste des actions prises en charge par l’assistant (messages d’accueil, affichage de l’heure et sortie). Outre les trois intentions précédemment affichées (`#hello`, `#time` et `#goodbye`), l'exemple d'espace de travail prend en charge une quatrième intention : `#menu`, qui est mise en correspondance lorsque l'utilisation demande à afficher une liste des actions disponibles.

Lorsque l'espace de travail reconnaît l'intention `#menu`, le dialogue répond par une réponse d'`option` :

```json
{
  "output": {
    "generic":[
      {
        "title": "Que souhaitez-vous faire ?",
        "options": [
          {
            "label": "Envoyer un message d'accueil",
            "value": {
              "input": {
                "text": "hello"
              }
            }
          },
          {
            "label": "Afficher l'heure locale",
            "value": {
              "input": {
                "text": "time"
              }
            }
          },
          {
            "label": "Quitter",
      "value": {
              "input": {
                "text": "goodbye"
              }
            }
          }
        ],
        "response_type": "option"
      }
    ],
    "intents": [
      {
        "intent": "menu",
        "confidence": 0.6178638458251953
      }
    ],
    "entities": []
  }
}
```

La réponse d'`option` contient plusieurs options à présenter à l'utilisateur. Chaque option inclut deux objets, `label` et `value`. `label` est une chaîne adressée à l'utilisateur identifiant l'option ; `value` spécifie l'entrée de message correspondante qui doit être renvoyée à l'assistant si l'utilisateur choisit l'option.

Notre application client devra utiliser les données de cette réponse pour générer le résultat que nous montrons à l'utilisateur et envoyer le message approprié à l'assistant. 

### Liste des options disponibles 

La première étape du traitement d'une réponse d'option consiste à afficher les options à l'utilisateur, en utilisant le texte spécifié par la propriété `label` de chaque option. Vous pouvez afficher les options à l'aide de toute technique prise en charge par votre application, généralement une liste déroulante ou un ensemble de boutons cliquables. (La propriété `preference` facultative d'une réponse d'option, si spécifiée, indique le type d'affichage que l'application doit afficher si possible.)

Notre exemple simplifié utilise une entrée et une sortie standard, nous n'avons donc pas accès à une véritable interface utilisateur. Au lieu de cela, nous allons simplement présenter les options sous forme de liste numérotée. 

```javascript
// Option example 1: lists options.

const prompt = require('prompt-sync')();
const AssistantV2 = require('ibm-watson/assistant/v2');

// Set up Assistant service wrapper.
const service = new AssistantV2({
  iam_apikey: '{apikey}', // replace with API key
  version: '2019-02-28',
});

const assistantId = '{assistant_id}'; // replace with assistant ID
let sessionId;

// Create session.
service
  .createSession({
    assistant_id: assistantId,
  })
  .then(res => {
    sessionId = res.session_id;
    sendMessage({text: ''}); // start conversation with empty message
  })
  .catch(err => {
    console.log(err); // something went wrong
  });

// Send message to assistant.
function sendMessage(messageInput) {
  service
    .message({
      assistant_id: assistantId,
      session_id: sessionId,
      input: messageInput,
    })
    .then(res => {
      processResponse(res);
    })
    .catch(err => {
      console.log(err); // something went wrong
    });
}

// Process the response.
function processResponse(response) {

  let endConversation = false;

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
    // Display the output from assistant, if any. Supports only a single
    // response.
    if (response.output.generic) {
      if (response.output.generic.length > 0) {
        switch (response.output.generic[0].response_type) {
          case 'text':
            // It's a text response, so we just display it.
            console.log(response.output.generic[0].text);
            break;
          case 'option':
            // It's an option response, so we'll need to show the user
            // a list of choices.
            console.log(response.output.generic[0].title);
            const options = response.output.generic[0].options;
            // List the options by label.
            for (let i = 0; i < options.length; i++) {
              console.log((i+1).toString() + '. ' + options[i].label);
            }
            break;
        }
      }
    }
  }

  // If we're not done, prompt for the next round of input.
  if (!endConversation) {
    const newMessageFromUser = prompt('>> ');
    newMessageInput = {
      message_type: 'text',
      text: newMessageFromUser
    }
    sendMessage(newMessageInput);
  } else {
    // We're done, so we delete the session.
    service
      .deleteSession({
        assistant_id: assistantId,
        session_id: sessionId,
      })
      .then(res => {
        return;
      })
      .catch(err => {
        console.log(err); // something went wrong
      });
  }
}
```
{: codeblock}
{: javascript}

Examinons de plus près le code qui génère la réponse de l'assistant. Désormais, au lieu de supposer une réponse `text`, l’application prend en charge les types de réponse `text` et `option` : 

```javascript
    // Display the output from assistant, if any. Supports only a single
    // response.
    if (response.output.generic) {
      if (response.output.generic.length > 0) {
        switch (response.output.generic[0].response_type) {
          case 'text':
            // It's a text response, so we just display it.
            console.log(response.output.generic[0].text);
            break;
          case 'option':
            // It's an option response, so we'll need to show the user
            // a list of choices.
            console.log(response.output.generic[0].title);
            const options = response.output.generic[0].options;
            // List the options by label.
            for (let i = 0; i < options.length; i++) {
              console.log((i+1).toString() + '. ' + options[i].label);
            }
            break;
        }
      }
    }
```
{: codeblock}
{: javascript}

Si `response_type`=`text`, nous affichons simplement le résultat, comme auparavant. Mais si `response_type`=`option`, nous devons effectuer d'autres tâches. Nous affichons d’abord la valeur de la propriété `title`, qui sert de texte d’introduction pour présenter la liste des options. Ensuite, nous listons les options en utilisant la valeur de la propriété `label` pour les identifier. (Une application réelle affiche ces libellés dans une liste déroulante ou sous forme de libellés sur des boutons cliquables.) 

Vous pouvez voir le résultat en déclenchant l'intention `#menu` :

```
Bienvenue dans l'exemple de l'assistant Watson !
>>
Quelles sont les actions disponibles ?
Que souhaitez-vous faire ?
1. Envoyer un message d'accueil
2. Afficher l'heure locale
3. Quitter
>> 2
Désolé, je n'ai aucune idée de ce dont vous parlez.
>>
```
{: screen}

Comme vous pouvez le constater, l'application gère maintenant correctement la réponse `option` en répertoriant les choix disponibles. Toutefois, nous n'avons pas encore traduit le choix de l'utilisateur en entrée significative.

### Sélection d'une option

Outre l'objet `label`, chaque option de la réponse comprend également un objet `value`, qui contient les données d'entrée à renvoyer à l'assistant si l'utilisateur choisit l'option correspondante. L'objet `value.input` est équivalent à la propriété `input` de l'API `/message`, ce qui signifie que nous pouvons simplement renvoyer cet objet à l'assistant tel quel.

Pour ce faire dans notre application, nous allons définir un nouvel indicateur `promptOption` lorsque le client reçoit une réponse `option`. Lorsque cet indicateur est défini sur true, nous savons que nous souhaitons utiliser le paramètre `value.input` pour la prochaine série d'entrées plutôt que d'accepter l'entrée de texte en langage naturel de l'utilisateur. (Là encore, nous n’avons pas d'interface utilisateur réelle, nous allons simplement inviter l’utilisateur à sélectionner une option valide dans la liste par numéro.) 

```javascript
// Option example 2: sends back selected option value.

const prompt = require('prompt-sync')();
const AssistantV2 = require('ibm-watson/assistant/v2');

// Set up Assistant service wrapper.
const service = new AssistantV2({
  iam_apikey: 'AZkSnK4b40UI5kLepHKxIKYpVcxeg0yPLbVVwGFW8kjM', // replace with API key
  version: '2019-02-28',
});

const assistantId = 'dcd5c5ad-f3a1-4345-89c5-708b0b5ff4f7'; // replace with assistant ID
let sessionId;

// Create session.
service
  .createSession({
    assistant_id: assistantId,
  })
  .then(res => {
    sessionId = res.session_id;
    sendMessage({text: ''}); // start conversation with empty message
  })
  .catch(err => {
    console.log(err); // something went wrong
  });

// Send message to assistant.
function sendMessage(messageInput) {
  service
    .message({
      assistant_id: assistantId,
      session_id: sessionId,
      input: messageInput,
    })
    .then(res => {
      processResponse(res);
    })
    .catch(err => {
      console.log(err); // something went wrong
    });
}

// Process the response.
function processResponse(response) {

  let endConversation = false;
  let promptOption = false;

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
    // Display the output from assistant, if any. Supports only a single
    // response.
    if (response.output.generic) {
      if (response.output.generic.length > 0) {
        switch (response.output.generic[0].response_type) {
          case 'text':
            // It's a text response, so we just display it.
            console.log(response.output.generic[0].text);
            break;
          case 'option':
              // It's an option response, so we'll need to show the user
              // a list of choices.
              console.log(response.output.generic[0].title);
              const options = response.output.generic[0].options;
              // List the options by label.
              for (let i = 0; i < options.length; i++) {
                console.log((i+1).toString() + '. ' + options[i].label);
              }
            promptOption = true;
            break;
        }
      }
    }
  }

  // If we're not done, prompt for the next round of input.
  if (!endConversation) {
    let messageInput;
    if (promptOption == true) {
      // Prompt for a valid selection from the list of options.
      let choice;
      do {
        choice = prompt('? ');
          if (isNaN(choice)) {
            choice = 0;
          }
      } while (choice < 1 || choice > response.output.generic[0].options.length);
      const value = response.output.generic[0].options[choice-1].value;
      // Use message input from the selected option.
      messageInput = value.input;
    } else {
      // We're not showing options, so we just prompt for the next
      // round of input.
      const newText = prompt('>> ');
      messageInput = {
        text: newText
      }
    }
    sendMessage(messageInput); 
  } else {
    // We're done, so we delete the session.
    service
      .deleteSession({
        assistant_id: assistantId,
        session_id: sessionId,
      })
      .then(res => {
        return;
      })
      .catch(err => {
        console.log(err); // something went wrong
      });
  }
}
```
{: codeblock}
{: javascript}

Tout ce que nous avons à faire est d'utiliser l'objet `value.input` à partir de la réponse sélectionnée comme entrée de message suivante, plutôt que de créer un nouvel objet `input` à l'aide d'une entrée de texte. L'assistant répond alors exactement comme si l'utilisateur avait saisi directement le texte d'entrée. 

```
Bienvenue dans l'exemple de l'assistant Watson !
>>
Bonjour
Bonjour.
>> quels sont les choix ?
Que souhaitez-vous faire ?
1. Envoyer un message d'accueil
2. Afficher l'heure locale
3. Quitter
2
Il est actuellement 13 heures 29 minutes et 14 secondes.
>> Au revoir
Très bien ! À plus tard.
```
{: screen}

Nous pouvons maintenant accéder à toutes les fonctions de l'assistant en effectuant des demandes en langage naturel ou en les sélectionnant dans un menu d'options. 

Notez que la même approche est également utilisée pour les réponses `suggestion`. Si votre forfait prend en charge la fonctionnalité de désambiguïsation, vous pouvez utiliser une logique similaire pour inviter les utilisateurs à effectuer une sélection dans une liste lorsqu'ils ne savent pas laquelle des options possibles est correcte. Pour plus d'informations sur la fonctionnalité de désambiguïsation, reportez-vous à la rubrique [Désambiguïsation](/docs/services/assistant?topic=assistant-dialog-runtime#dialog-runtime-disambiguation).
