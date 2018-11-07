---

copyright:
  years: 2015, 2018
lastupdated: "2018-02-09"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:swift: .ph data-hd-programlang='swift'}

# Réponses multimédia

Si votre espace de travail est intégré à Slack ou Facebook Messenger à l'aide du [connecteur {{site.data.keyword.conversationshort}}](conversation-connector.html), vous pouvez spécifier des réponses de noeud de dialogue incluant des éléments multimédia ou interactifs, tels que des boutons cliquables. Pour spécifier une réponse interactive, vous insérez un bloc de données JSON dans la sortie d'un noeud de dialogue.

**Remarque :** si vous souhaitez utiliser des messages interactifs avec Slack, vérifiez que vous avez activé le support de message interactif. Pour plus d'informations, reportez-vous au [fichier README sur le déploiement Slack![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/watson-developer-cloud/conversation-connector/blob/master/channels/slack/README.md#interactive-messages){: new_window}.

## Format JSON générique

Vous pouvez spécifier des réponses interactives à l'aide d'un format JSON générique qui prend en charge les déploiements Slack ou Facebook Messenger. Pour spécifier une réponse interactive au format JSON générique, insérez le JSON dans la zone `output.generic` de la réponse de noeud de dialogue. L'exemple suivant vous montre comment envoyer une réponse contenant plusieurs types de réponse (du texte, une image et des options cliquables) :

```json
{
  "output": {
    "generic":[
      {
        "response_type": "text",
        "text": "Here are your nearest stores."
      },
      {
        "response_type": "image",
        "source": "http://...",
        "title": "Image title",
        "description": "Some description for the image"
      },
      {
        "response_type": "option",
        "title": "Click on one of the following",
        "options": [
          {
            "label": "Location 1",
            "value:" "Location 1"
          },
          {
            "label": "Location 2",
            "value:" "Location 2"
          },
          {
            "label": "Location 3",
            "value:" "Location 3"
          }
        ]
      }
    ]
  }
}
```

Pour plus d'informations sur les types de réponse pris en charge et pour savoir comment les spécifier, reportez-vous à la rubrique [Types de réponse](#response-types).

Lors de l'exécution, le connecteur {{site.data.keyword.conversationshort}} convertit la réponse au format attendu par le canal (Slack ou Facebook Messenger). Si la réponse contient plusieurs types de support ou plusieurs pièces jointes, la réponse générique est convertie en une série de charges de message distinctes. Le connecteur envoie ensuite chaque charge de message au canal dans un message distinct.

**Remarque :** lorsqu'une réponse est scindée en plusieurs messages, le connecteur {{site.data.keyword.conversationshort}} envoie ces messages au canal de façon séquentielle. Il incombe au canal de distribuer ces messages à l'utilisateur final ; cela peut être affecté par des problèmes liés au réseau ou au serveur. 

## Format JSON natif

Outre le format JSON générique, le connecteur {{site.data.keyword.conversationshort}} prend également en charge des réponses spécifiques du canal qui sont écrites à l'aide des formats Slack et Facebook Messenger natifs. Vous souhaiterez peut-être utiliser les formats JSON natifs si vous devez spécifier un type de réponse qui n'est pas pris en charge pour l'instant par le format JSON générique. 

Vous pouvez spécifier le format JSON natif pour Slack ou Facebook à l'aide de la zone appropriée dans la réponse de noeud de dialogue :

- `output.slack` : insérez n'importe quel format JSON que vous souhaitez inclure dans la zone `attachment` de la réponse Slack. Pour plus d'informations sur le format JSON Slack, reportez-vous à la [documentation Slack ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://api.slack.com/docs/message-attachments){: new_window}.

- `output.facebook` : insérez n'importe quel format JSON que vous souhaitez inclure dans la zone `message.attachment.payload` de la réponse Facebook. Pour plus d'informations sur le format JSON Facebook, reportez-vous à la [documentation Facebook![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://developers.facebook.com/docs/messenger-platform/send-messages/templates){: new_window}.

## Types de réponse

Les types de réponse suivants sont pris en charge par le format JSON générique :

### Image

Affiche une image spécifiée par une URL.

#### Zones 

| Nom           | Type   | Description                        | Obligatoire ? |
|---------------|--------|------------------------------------|-----------|
| response_type | Enum   | `image`                            | O|
| source        | Chaîne | URL de l'image. L'image spécifiée doit être au format .jpg, .gif ou .png. | O|
| title         | Chaîne | Titre à afficher avant l'image.| N         |
| description   | Chaîne | Texte de la description qui accompagne l'image. | N |

#### Exemple

Cet exemple affiche une image avec un titre et un texte descriptif. 

```json
{
  "response_type": "image",
  "source": "http://example.com/image.jpg",
  "title": "Example image",
  "description": "An example image returned as part of a multimedia response."
}
```

### Option

Affiche un ensemble de boutons sur lesquels les utilisateurs peuvent cliquer pour choisir une option. La valeur spécifiée est ensuite envoyée à l'espace de travail en tant qu'entrée utilisateur.

#### Zones 

| Nom           | Type   | Description                         | Obligatoire ? |
|---------------|--------|-------------------------------------|-----------|
| response_type | Enum   | `option`                            | O|
| title         | Chaîne | Titre à afficher avant les options. | O|
| description   | Chaîne | Texte de la description qui accompagne les options. | N |
| options       | liste | Liste de paires clé-valeur spécifiant les options parmi lesquelles l'utilisateur peut choisir. | O|
| options[].label | Chaîne | Libellé de l'option tourné vers l'utilisateur. | O|
| options[].value | objet<br/>chaîne | Valeur qui sera envoyée au service {{site.data.keyword.conversationshort}} si l'utilisateur sélectionne l'option. Il peut s'agir d'une chaîne (pour les réponses simples) ou un objet contenant plusieurs zones. Examinez les exemples ci-après. | O|

#### Exemple

Cet exemple affiche deux options :

- L'option 1 (libellée 'Buy something') envoie un message de chaîne simple (`option_1`), lequel est envoyé à l'espace de travail à l'aide de la zone `input.text` du message. 
- L'option 2 (libellée 'Exit') envoie un message complexe qui inclut un texte d'entrée et un tableau d'intentions. La réponse peut inclure n'importe quelle zone représentant une partie valide d'un message {{site.data.keyword.conversationshort}}. (Pour plus d'informations sur la structure d'entrée de message, reportez-vous à la documentation [API Reference ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://www.ibm.com/watson/developercloud/conversation/api/v1/?curl#send_message){: new_window}.)

```json
{
  "response_type": "option",
  "title": "Choose from the following options:",
  "options": [
    {
      "label": "Buy something",
      "value": "Place order"
    },
    {
      "label": "Exit",
      "value": {
        "input": {
          "text": "Exit"
        },
        "intent": [
          {
            "intent": "exit_app",
            "confidence": 1.0
          }
        ]
      }
    }
  ]
}
```

### Texte

Affiche un texte.

#### Zones 

| Nom           | Type   | Description        | Obligatoire ? |
|---------------|--------|--------------------|-----------|
| response_type | Enum   | `text`             | O|
| text          | Chaîne | Texte à afficher. Peut inclure des caractères de retour à la ligne `\n`) et du balisage Markdown, si le canal le permet. (Tout formatage non pris en charge par le canal est ignoré.) | O|

#### Exemple

Cet exemple affiche un message texte pour l'utilisateur. 

```json
{
  "response_type": "text",
  "text": "This is a text response."
}
```
