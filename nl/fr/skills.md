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

# Compétences
{: #skills}

Une compétence de dialogue contient les données d'apprentissage et la logique permettant à un assistant d'aider vos clients.
{: shortdesc}

Une compétence de dialogue contient les types d'artefact suivants :

- [**Intentions**](/docs/services/assistant?topic=assistant-intents) : une *intention* représente l'objectif d'une entrée utilisateur, par exemple, une question sur le site d'une entreprise ou sur le règlement d'une facture. Vous définissez une intention pour chaque type de demande d'utilisateur qui doit être prise en charge par votre application. Dans l'outil, le nom de l'intention est toujours précédé du préfixe `#`. Pour entraîner lle compétence de dialogue à reconnaître vos intentions, vous fournissez un grand nombre d'exemples d'entrée utilisateur et vous indiquez à quelles intentions ils correspondent.

  Un *catalogue de contenu* est fourni. Il contient les intentions communes prédéfinies que vous pouvez ajouter à votre application plutôt que de créer les vôtres. Par exemple, la plupart des applications nécessitent une intention d'accueil qui ouvre un dialogue avec l'utilisateur. Vous pouvez ajouter le catalogue de contenu **General** pour ajouter une intention qui accueille l'utilisateur et effectue d'autres tâches utiles, telles que mettre fin à la conversation.

- [**Dialogue**](/docs/services/assistant?topic=assistant-dialog-build) : un *dialogue* est un flux de conversation constitué de branches qui explique de quelle manière votre application répond lorsqu'elle reconnaît les intentions et les entités définies. Vous utilisez l'éditeur de dialogue inclus dans l'outil pour créer des conversations avec les utilisateurs, en fournissant des réponses basées sur les intentions et les entités reconnues dans leurs entrées.

  ![Diagramme d'une implémentation de base utilisant uniquement l'intention et le dialogue.](images/basic-impl.png)

Pour permettre à votre compétence de dialogue de traiter des questions plus nuancées, définissez des entités et faites-y référence à partir de votre dialogue. 

- [**Entités**](/docs/services/assistant?topic=assistant-entities) : une *entité* représente un terme ou un objet en rapport avec vos intentions et qui fournit un contexte spécifique pour votre intention. Par exemple, une entité peut représenter une ville dans laquelle l'utilisateur souhaite
rechercher un site d'entreprise ou le montant d'un règlement de facture. Dans l'outil, le nom d'une entité est toujours précédé du préfixe `@`.

  Vous pouvez former la compétence à reconnaître vos entités en fournissant des valeurs et des synonymes de terme d'entité, des canevas d'entité ou en identifiant le contexte dans lequel une entité est généralement utilisée dans une phrase. Pour affiner votre dialogue, revenez en arrière et ajoutez des noeuds qui vérifient les mentions d'entité dans les entrées utilisateur, en plus des intentions. 

![Diagramme d'une implémentation plus complexe qui utilise l'intention, l'entité et le dialogue.](images/complex-impl.png)

Au fur et à mesure que vous ajoutez des informations, la compétence utilise ces données uniques pour créer un modèle d’apprentissage automatique capable de reconnaître ces entrées et des entrées similaires. Chaque fois que vous ajoutez ou modifiez les données d'apprentissage, le processus d'apprentissage est déclenché pour garantir que le modèle sous-jacent reste à jour, à mesure de l'évolution des besoins de votre client et des sujets qu'il souhaite aborder.

Pour obtenir de l'aide sur la création d'une compétence de dialogue, reportez-vous à la rubrique [Création d'une compétence](/docs/services/assistant?topic=assistant-skill-add).
