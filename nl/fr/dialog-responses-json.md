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

# Définition de réponses à l'aide de l'éditeur JSON  
{: #dialog-responses-json}

Dans certaines situations, vous devrez peut-être définir les réponses à l'aide de l'éditeur JSON. (Pour plus d'informations sur les réponses de dialogue, reportez-vous à la rubrique [Réponses](/docs/services/assistant?topic=assistant-dialog-overview#dialog-overview-responses)). La modification de la réponse au format JSON vous donne un accès direct aux données qui seront renvoyées au canal de communication ou à l'application personnalisée. 

## Format JSON générique
{: #dialog-responses-json-generic}

Le format JSON générique des réponses permet de spécifier les réponses destinées à n’importe quel canal. Ce format peut prendre en charge différents types de réponses pris en charge par les intégrations Slack et Facebook et peut également être implémenté par une application client personnalisée. (Il s'agit du format utilisé par défaut pour les réponses de dialogue définies à l'aide de l'outil {{site.data.keyword.conversationshort}}.)

Pour plus d'informations sur la procédure d'ouverture de l'éditeur JSON pour une réponse du noeud de dialogue à partir de l'outil, reportez-vous à la rubrique [Variables contextuelles dans l'éditeur JSON](/docs/services/assistant?topic=assistant-dialog-runtime#dialog-runtime-context-var-json).

Pour spécifier une réponse interactive au format JSON générique, insérez les objets JSON appropriés dans la zone `output.generic` de la réponse du noeud du dialogue. L'exemple suivant vous montre comment envoyer une réponse contenant plusieurs types de réponse (du texte, une image et des options cliquables) :

```json
{
  "output": {
    "generic":[
      {
        "response_type": "text",
        "values": [
          {
            "text": "Voici les magasins les plus proches."
          }
        ]
      },
      {
        "response_type": "image",
        "source": "http://example.com/image.jpg",
        "title": "Exemple d'image",
        "description": "Description de l'image."
      },
      {
        "response_type": "option",
        "title": "Cliquez sur l'un des éléments suivants",
        "options": [
          {
            "label": "Emplacement 1",
            "value": {
              "input": {
                "text": "Emplacement 1"
              }
            }
          },
          {
            "label": "Emplacement 2",
            "value": {
              "input": {
                "text": "Emplacement 2"
              }
            }
          },
          {
            "label": "Emplacement 3",
            "value": {
              "input": {
                "text": "Emplacement 3"
              }
            }
          }
        ]
      }
    ]
  }
}
```

Pour plus d'informations sur la spécification de chaque type de réponse pris en charge à l'aide d'objets JSON, reportez-vous à la rubrique [Types de réponse](#dialog-responses-json-response-types).

Si vous utilisez le connecteur {{site.data.keyword.conversationshort}}, la réponse est convertie au moment de l'exécution au format attendu par le canal (Slack ou Facebook Messenger). Si la réponse contient plusieurs types de support ou plusieurs pièces jointes, la réponse générique est convertie en une série de charges de message distinctes, selon les besoins. Le connecteur envoie ensuite chaque charge de message au canal dans un message distinct.

**Remarque :** lorsqu'une réponse est scindée en plusieurs messages, le connecteur {{site.data.keyword.conversationshort}} envoie ces messages au canal de façon séquentielle. Il incombe au canal de distribuer ces messages à l'utilisateur final ; cela peut être affecté par des problèmes liés au réseau ou au serveur.

Si vous créez votre propre application client, elle doit mettre en oeuvre chaque type de réponse selon le cas. Pour plus d'informations, reportez-vous à la rubrique [Mise en oeuvre des réponses](/docs/services/assistant?topic=assistant-api-dialog-responses).

## Format JSON natif
{: #dialog-responses-json-native}

Outre le format JSON générique, le noeud de dialogue JSON prend également en charge les réponses spécifiques aux canaux écrites à l'aide des formats natifs Slack et Facebook Messenger. Ces formats sont également pris en charge par le connecteur {{site.data.keyword.conversationshort}}. Vous souhaiterez peut-être utiliser les formats JSON natifs si vous savez que votre espace de travail ne sera intégré qu'avec un seul type de canal et si vous devez spécifier un type de réponse qui n'est actuellement pas pris en charge par le format JSON générique. 

Vous pouvez spécifier le format JSON natif pour Slack ou Facebook à l'aide de la zone appropriée dans la réponse de noeud de dialogue :

- `output.slack` : insérez n'importe quel format JSON que vous souhaitez inclure dans la zone `attachment` de la réponse Slack. Pour plus d'informations sur le format JSON Slack, reportez-vous à la [documentation Slack ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://api.slack.com/docs/message-attachments){: new_window}.

- `output.facebook` : insérez n'importe quel format JSON que vous souhaitez inclure dans la zone `message.attachment.payload` de la réponse Facebook. Pour plus d'informations sur le format JSON Facebook, reportez-vous à la [documentation Facebook![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://developers.facebook.com/docs/messenger-platform/send-messages/templates){: new_window}.

## Types de réponse
{: #dialog-responses-json-response-types}

Les types de réponse suivants sont pris en charge par le format JSON générique :

### Image
{: #dialog-responses-json-image}

Affiche une image spécifiée par une URL.

#### Zones
{: #{: #dialog-responses-json-image-fields}

| Nom          | Type   | Description                        | Obligatoire ? |
|---------------|--------|------------------------------------|-----------|
| response_type | Enum   | `image`                            | O         |
| source        | Chaîne | URL de l'image. L'image spécifiée doit être au format .jpg, .gif ou .png. | O |
| title         | Chaîne | Titre à afficher avant l'image.| N         |
| description   | Chaîne | Texte de la description qui accompagne l'image. | N |

#### Exemple
{: #dialog-responses-json-image-example}

Cet exemple affiche une image avec un titre et un texte descriptif.

```json
{
  "output": {
    "generic":[
      {
        "response_type": "image",
        "source": "http://example.com/image.jpg",
        "title": "Exemple d'image",
        "description": "Exemple d'image renvoyé dans le cadre d'une réponse multimédia."
      }
    ]
  }
}
```

### Option
{: #dialog-responses-json-option}

Affiche un ensemble de boutons ou une liste déroulante à partir desquels les utilisateurs peuvent choisir une option. La valeur spécifiée est ensuite envoyée à l'espace de travail en tant qu'entrée utilisateur.

#### Zones
{: #dialog-responses-json-option-fields}

| Nom          | Type   | Description                         | Obligatoire ? |
|---------------|--------|-------------------------------------|-----------|
| response_type | Enum   | `option`                            | O         |
| title         | Chaîne | Titre à afficher avant les options. | O       |
| description   | Chaîne | Texte de la description qui accompagne les options. | N |
| preference    | Enum   | Type préféré de contrôle à afficher, si pris en charge par le canal (`liste déroulante` ou `bouton`). Actuellement, le connecteur {{site.data.keyword.conversationshort}} prend uniquement en charge le `bouton`.| N |
| options       | liste   | Liste de paires clé-valeur spécifiant les options parmi lesquelles l'utilisateur peut choisir. | O |
| options[].label | Chaîne | Libellé de l'option tourné vers l'utilisateur. | O     |
| options[].value | objet | Objet définissant la réponse qui sera envoyée au service {{site.data.keyword.conversationshort}} si l'utilisateur sélectionne l'option. | O |
| options[].value.input | objet | Objet d'entrée contenant le texte d'entrée correspondant à l'option. | N |
| options[].value.input.text | Chaîne | Texte qui sera envoyé au service pour l'option. | N |

#### Exemple
{: #dialog-responses-json-option-example}

Cet exemple affiche deux options :

- L'option 1 (libellée `Acheter quelque chose`) envoie un message de chaîne simple (`Passer une commande`), qui est envoyé à l'espace de travail sous forme de texte d'entrée.
- L'option 2 (libellée `Quitter`) envoie un message complexe qui inclut un texte d'entrée et un tableau d'intentions. La réponse peut inclure n'importe quelle zone représentant une partie valide d'un message {{site.data.keyword.conversationshort}}. (Pour plus d'informations sur la structure d'entrée de message, reportez-vous à la documentation [Référence d'API ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://cloud.ibm.com/apidocs/assistant?curl=#get-response-to-user-input){: new_window}.)

```json
{
  "output": {
    "generic":[
      {
        "response_type": "option",
        "title": "Choisissez parmi les options suivantes :",
        "preference": "bouton",
        "options": [
          {
            "label": "Acheter quelque chose",
            "value": {
              "input": {
                "text": "Passer une commande"
              }
            }
          },
          {
            "label": "Quitter",
      "value": {
              "input": {
                "text": "Quitter"
              },
              "intents": [
                {
                  "intent": "exit_app",
            "confidence": 1.0
          }
              ]
            }
          }
        ]
      }
    ]
  }
}
```

### Pause
{: #dialog-responses-json-pause}

Effectue une pause avant d’envoyer le message suivant au canal et envoie éventuellement un événement "l'utilisateur est en train d'écrire" (pour les canaux qui prennent en charge cette fonction). 

#### Zones
{: #dialog-responses-json-pause-fields}

| Nom          | Type   | Description        | Obligatoire ? |
|---------------|--------|--------------------|-----------|
| response_type | Enum   | `pause`            | O         |
| time          | ent    | Durée de la pause, en millisecondes. | O |
| typing        | booléen | Indique s'il faut envoyer l'événement "l'utilisateur est en train d'écrire" pendant la pause. Ignoré si le canal ne prend pas en charge cet événement. | N |

#### Exemple
{: #dialog-responses-json-pause-example}

Cet exemple envoie l'événement "l'utilisateur est en train d'écrire" en mettant en pause pendant 5 secondes. 

```json
{
  "output": {
    "generic":[
      {
        "response_type": "pause",
        "time": 5000,
        "typing": true
      }
    ]
  }
}
```

### Texte
{: #dialog-responses-json-text}

Affiche un texte. Pour diversifier les réponses, vous pouvez spécifier plusieurs réponses textuelles alternatives. Si vous spécifiez plusieurs réponses, vous pouvez choisir d'effectuer une rotation séquentielle dans la liste, de choisir une réponse au hasard ou de générer toutes les réponses spécifiées. 

#### Zones
{: #dialog-responses-json-text-fields}

| Nom          | Type   | Description        | Obligatoire ? |
|---------------|--------|--------------------|-----------|
| response_type | Enum   | `text`             | O         |
| values        | liste   | Liste d'un ou plusieurs objets définissant la réponse textuelle. | O |
| values.[_n_].text   | Chaîne | Texte d'une réponse. Peut inclure des caractères de retour à la ligne (`\n`) et du balisage Markdown, si le canal le permet. (Tout formatage non pris en charge par le canal est ignoré.) | N |
| selection_policy | Chaîne | La manière dont une réponse est sélectionnée dans la liste, si plusieurs réponses sont spécifiéee. Les valeurs possibles sont `sequential`, `random` et `multiline`. | N |
| délimiteur     | Chaîne | Délimiteur à utiliser comme séparateur entre les réponses. Utilisé uniquement lorsque `selection_policy`=`multiline`. Le délimiteur par défaut est le retour à la ligne (`\n`). | N |

#### Exemple
{: #dialog-responses-json-text-example}

Cet exemple affiche un message d'accueil à l'utilisateur. 

```json
{
  "output": {
    "generic":[
      {
        "response_type": "text",
        "values": [
          { "text": "Bonjour." },
          { "text": "Salut." }
        ],
        "selection_policy": "random"
      }
    ]
  }
}  
```
