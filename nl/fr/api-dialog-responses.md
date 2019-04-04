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

# Mise en oeuvre des réponses 
{: #dialog-api-responses}

Un noeud de dialogue peut répondre aux utilisateurs à l'aide de texte, d'images ou d'éléments interactifs tels que des options cliquables. Si vous créez votre propre application client, vous devez implémenter l'affichage correct de tous les types de réponse renvoyés par votre dialogue. (Pour plus d'informations sur les réponses de dialogue, reportez-vous à la rubrique [Réponses](/docs/services/assistant?topic=assistant-dialog-overview#responses)).

## Format de sortie des réponses 
{: #dialog-api-responses-output}

Par défaut, les réponses issues d'un noeud de dialogue sont spécifiées dans l'objet `output.generic` dans la réponse JSON renvoyée à partir de l'API /message. L'objet `generic` contient un tableau de 5 éléments de réponse au maximum, destiné à n'importe quel canal. L'exemple JSON suivant montre une réponse comprenant du texte et une image :  

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

**Remarque :** comme le montre cet exemple, la réponse textuelle (`Très bien, voici la photo d'un chien.`) est également renvoyée dans le tableau `output.text`. Ceci est inclus à des fins de compatibilité ascendante des applications qui ne prennent pas en charge le format `output.generic`.

Il incombe à votre application client de gérer tous les types de réponses de manière appropriée. Dans ce cas, votre application devra présenter à l'utilisateur le texte et l'image spécifiés. 

## Types de réponse
{: #dialog-api-responses-types}

Chaque élément d'une réponse correspond à l'un des types de réponse pris en charge (actuellement,`image`, `option`, `pause` et `text`). Chaque type de réponse est spécifié à l'aide d'un ensemble différent de propriétés JSON. Par conséquent, les propriétés incluses pour chaque réponse varient en fonction du type de réponse. Pour des informations complètes sur le modèle de réponse de l'API /message, reportez-vous à la [Référence d'API ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://cloud.ibm.com/apidocs/assistant?curl=#get-response-to-user-input){: new_window}.)

Cette section décrit les types de réponse disponibles et leur représentation dans le code JSON de réponse de l'API /message. (Si vous utilisez le SDK Watson, vous pouvez utiliser les interfaces fournies dans votre langue pour accéder aux mêmes objets.) 

**Remarque :** les exemples de cette section montrent le format des données JSON renvoyées par l'API /message au moment de l'exécution. N'oubliez pas que ce format diffère du format JSON utilisé pour définir les réponses dans un noeud de dialogue. Pour plus d'informations, reportez-vous à la rubrique [Définition de réponses à l'aide de l'éditeur JSON](/docs/services/assistant?topic=assistant-dialog-responses-json).

### Image
{: #dialog-api-responses-image}

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

### Option
{: #dialog-api-responses-option}

Le type de réponse `option` indique à l'application client d'afficher un contrôle d'interface utilisateur permettant à l'utilisateur d'effectuer un choix dans une liste d'options, puis de renvoyer l'entrée au service {{site.data.keyword.conversationshort}} en fonction de l'option sélectionnée :

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

Votre application peut afficher les options spécifiées à l'aide de tout contrôle d'interface utilisateur approprié (par exemple, un ensemble de boutons ou une liste déroulante). La propriété facultative `preference` indique le type de contrôle préféré que votre application doit utiliser (`bouton` ou `liste déroulante`), s'il est pris en charge.

Pour chaque option, la propriété `label` spécifie le texte de l'étiquette qui doit apparaître pour l'option dans le contrôle d'interface utilisateur. La propriété `value` spécifie l'entrée qui doit être renvoyée au service {{site.data.keyword.conversationshort}} (à l'aide de l'API /message) lorsque l'utilisateur sélectionne l'option correspondante. Notez que la propriété `value` peut inclure non seulement la saisie de texte, mais également d'autres objets d'entrée tels que les intentions et les entités, qui doivent tous être envoyés au service. 

### Pause
{: #dialog-api-responses-pause}

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

### Texte
{: #dialog-api-responses-text}

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
