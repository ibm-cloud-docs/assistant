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

# Création d'une compétence
{: #skill-add}

Personnalisez l’assistant en y ajoutant les compétences dont il a besoin pour satisfaire les objectifs de vos clients.
{: shortdesc}

Vous pouvez créer le type suivant de compétence :

- **Compétence de dialogue** : utilise les technologies de traitement du langage naturel et d’apprentissage automatique Watson pour comprendre les questions et les demandes des utilisateurs, et y répondre par des réponses que vous avez écrites.

- **Compétence de recherche** ![Forfait Plus ou Premium uniquement](images/plus.png) : pour une requête utilisateur donnée, utilisez le service {{site.data.keyword.discoveryfull}} pour rechercher une source de données de votre contenu en libre-service et renvoyer une réponse.

  Seuls les utilisateurs des forfaits Plus ou Premium peuvent créer ce type de compétence. 
  
  Généralement, vous créez d’abord une compétence de chaque type. Ensuite, lorsque vous créez un dialogue pour la compétence de dialogue, vous décidez quand lancer la compétence de recherche. Pour certaines questions ou demandes, une réponse codée en dur ou à l'aide d'un programme (définie dans la compétence de dialogue) est suffisante. Pour d'autres, vous souhaiterez peut-être fournir une réponse plus rigoureuse en renvoyant un passage complet des informations associées (extraites d'une source de données externe à l'aide de la compétence de recherche). 

## Création de la compétence
{: #skill-add-task}

Vous pouvez ajouter une compétence de chaque type de compétence à un assistant. 

Pour créer une compétence, procédez comme suit :

1.  Cliquez sur l'onglet **Skills**, puis cliquez sur **Create skill**.

1.  Choisissez le type de compétence à créer, puis cliquez sur **Next**.

    Suivez les étapes restantes de la procédure appropriée pour terminer le processus de création de compétence. 

      - [Compétence de dialogue](/docs/services/assistant?topic=assistant-skill-dialog-add)
      - [Compétence de recherche](/docs/services/assistant?topic=assistant-skill-search-add)

## Limites de compétence
{: #skill-add-limits}

Le nombre de compétences que vous pouvez créez dépend de votre type de forfait {{site.data.keyword.conversationshort}}. Tous les exemples de compétences de dialogue que vous pouvez utiliser ne sont pas pris en compte dans votre limite, sauf si vous les utilisez. Une version de compétence ne compte pas comme une compétence.

| Forfait     | Compétences par instance de service |
|------------------|----------------------------:|
| Premium          |                          50 |
| Plus             |                          50 |
| Standard         |                          20 |
| Lite`*`, Plus Trial |                        5 |
{: caption="Détails du forfait" caption-side="top"}

`*` Au bout de 30 jours d'inactivité, une compétence inutilisée dans une instance de service de forfait Lite peut être supprimée afin de libérer de l'espace.

## Suppression d'une compétence
{: #skill-add-delete}

Vous pouvez supprimer toutes les compétences auxquelles vous avez accès, sauf si elles sont utilisées par un assistant. Si elles sont utilisées, vous devez les supprimer de l'assistant qui les utilise avant de pouvoir les supprimer.

Veillez à consulter toute autre personne susceptible d’utiliser la compétence avant de la supprimer.
{: tip}

Pour supprimer une compétence, procédez comme suit :

1.  Découvrez si la compétence est utilisée par des assistants. Dans l'onglet Skills, recherchez la vignette de la compétence à supprimer. La zone **Assistants** répertorie les assistants qui utilisent actuellement cette compétence.

1.  Si la compétence que vous souhaitez supprimer est associée à un assistant, supprimez-la de l'assistant en procédant comme suit :

    - Consultez le propriétaire de l'assistant qui utilise la compétence avant de la supprimer.
    - Ouvrez l'onglet Assistants, puis cliquez pour ouvrir la vignette de l'assistant.
    - Recherchez la vignette correspondant à la compétence que vous souhaitez supprimer. Cliquez sur l'icône ![ouvrir et fermer la liste d'options](images/kabob-beta.png), puis choisissez **Remove**.
    - Répétez les étapes précédentes pour tous les autres assistants qui utilisent la compétence.
    - Revenez à l'onglet Skills et recherchez la vignette de la compétence à supprimer.

1.  Cliquez sur l'icône ![ouvrir et fermer la liste d'options](images/kabob-beta.png), puis choisissez **Delete**. Confirmez la suppression.
