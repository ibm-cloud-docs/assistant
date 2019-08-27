---

copyright:
  years: 2015, 2019
lastupdated: "2019-08-02"

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

# Compétences
{: #skills}

Une compétence est un conteneur d'intelligence artificielle qui permet à un assistant d'aider vos clients.
{: shortdesc}

Un assistant dirige les requêtes vers le chemin optimal pour résoudre un problème client. Ajoutez des compétences pour que votre assistant puisse fournir une réponse directe à une question commune ou faire référence à des résultats de recherche plus généralisés pour des tâches plus complexes. 

## Types de compétences
{: #skills-types}

Vous pouvez ajouter le type de compétence suivant à votre assistant : 

- **[Compétence de dialogue](#skills-dialog-skill)** : comprend les questions ou demandes types des utilisateurs et y répond en suivant un dialogue que vous avez écrit. 

![Forfait Plus ou Premium uniquement](images/plus.png) Si vous êtes un utilisateur de forfait Plus ou Premium, vous pouvez également créer ce type de compétence :

- **[Compétence de recherche](#skills-search-skill)** : répond à la question d'un utilisateur en recherchant des informations pertinentes dans une source de données externe, en extrayant le passage et en le renvoyant comme réponse de l'assistant.

### Compétence de dialogue
{: #skills-dialog-skill}

Une compétence de dialogue contient les données d'apprentissage et la logique permettant à un assistant d'aider vos clients. Elle contient les types d'artefact suivants :

- [**Intentions**](/docs/services/assistant?topic=assistant-intents) : une *intention* représente l'objectif d'une entrée utilisateur, par exemple, une question sur le site d'une entreprise ou sur le règlement d'une facture. Vous définissez une intention pour chaque type de demande d'utilisateur qui doit être prise en charge par votre application. Le nom de l'intention est toujours précédé du préfixe `#`. Pour entraîner la compétence de dialogue à reconnaître vos intentions, vous fournissez un grand nombre d'exemples d'entrée utilisateur et vous indiquez à quelles intentions ils correspondent.

  Un *catalogue de contenu* est fourni. Il contient les intentions communes prédéfinies que vous pouvez ajouter à votre application plutôt que de créer les vôtres. Par exemple, la plupart des applications nécessitent une intention d'accueil qui ouvre un dialogue avec l'utilisateur. Vous pouvez ajouter le catalogue de contenu **General** pour ajouter une intention qui accueille l'utilisateur et effectue d'autres tâches utiles, telles que mettre fin à la conversation.

- [**Dialogue**](/docs/services/assistant?topic=assistant-dialog-build) : un *dialogue* est un flux de conversation constitué de branches qui explique de quelle manière votre application répond lorsqu'elle reconnaît les intentions et les entités définies. Vous utilisez l'éditeur de dialogue pour créer des conversations avec les utilisateurs, en fournissant des réponses basées sur les intentions et les entités reconnues dans leurs entrées.

  ![Diagramme d'une implémentation de base utilisant uniquement l'intention et le dialogue.](images/basic-impl.png)

Pour permettre à votre compétence de dialogue de traiter des questions plus nuancées, définissez des entités et faites-y référence à partir de votre dialogue.

- [**Entités**](/docs/services/assistant?topic=assistant-entities) : une *entité* représente un terme ou un objet en rapport avec vos intentions et qui fournit un contexte spécifique pour votre intention. Par exemple, une entité peut représenter une ville dans laquelle l'utilisateur souhaite
rechercher un site d'entreprise ou le montant d'un règlement de facture. Le nom d'une entité est toujours précédé du caractère `@`. 

  Vous pouvez former la compétence à reconnaître vos entités en fournissant des valeurs et des synonymes de terme d'entité, des canevas d'entité ou en identifiant le contexte dans lequel une entité est généralement utilisée dans une phrase. Pour affiner votre dialogue, revenez en arrière et ajoutez des noeuds qui vérifient les mentions d'entité dans les entrées utilisateur, en plus des intentions.

![Diagramme d'une implémentation plus complexe qui utilise l'intention, l'entité et le dialogue.](images/complex-impl.png)

Au fur et à mesure que vous ajoutez des informations, la compétence utilise ces données uniques pour créer un modèle d’apprentissage automatique capable de reconnaître ces entrées et des entrées similaires. Chaque fois que vous ajoutez ou modifiez les données d'apprentissage, le processus d'apprentissage est déclenché pour garantir que le modèle sous-jacent reste à jour, à mesure de l'évolution des besoins de votre client et des sujets qu'il souhaite aborder.

Pour obtenir de l'aide sur la création d'une compétence de dialogue, reportez-vous à la rubrique [Création d'une compétence de dialogue](/docs/services/assistant?topic=assistant-skill-dialog-add).

### Compétence de recherche ![Forfait Plus ou Premium uniquement](images/plus.png) 
{: #skills-search-skill}

Lorsque Watson Assistant ne dispose pas d'une solution explicite à un problème, il dirige la question de l'utilisateur vers une compétence de recherche afin de trouver une réponse parmi vos sources disparates de contenu en libre-service. La compétence de recherche interagit avec le service {{site.data.keyword.discoveryfull}} pour extraire ces informations à partir d'une collection de données configurée.

Si vous utilisez déjà le service {{site.data.keyword.discoveryshort}}, vous pouvez exploiter vos collections de données existantes pour rechercher des sources que vous pouvez partager avec vos clients pour répondre à leurs questions.

Cependant, vous n'avez pas besoin d'avoir une instance de service {{site.data.keyword.discoveryshort}}.  Si vous choisissez de créer une compétence de recherche, une instance gratuite de {{site.data.keyword.discoveryshort}} est mise à votre disposition. Vous pouvez ensuite créer une collection à partir d'une source de données et configurer votre compétence de recherche pour rechercher dans cette collection les réponses aux requêtes des clients.

Le diagramme suivant illustre le traitement des entrées utilisateur lorsqu'une compétence de dialogue et une compétence de recherche sont ajoutées à un assistant. Toutes les questions auxquelles le dialogue n'est pas conçu pour répondre sont envoyées à la compétence de recherche, qui trouve une réponse pertinente à partir d'une collection de données {{site.data.keyword.discoveryshort}}.

![Diagramme illustrant comment une question est acheminée vers la compétence de recherche.](images/search-skill-diagram.png)

Pour obtenir de l'aide sur la création d'une compétence de recherche, reportez-vous à la rubrique [Création d'une compétence de recherche](/docs/services/assistant?topic=assistant-skill-search-add).
