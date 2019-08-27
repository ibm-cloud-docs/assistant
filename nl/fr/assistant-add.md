---

copyright:
  years: 2015, 2019
lastupdated: "2019-07-26"

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

# Création d'un assistant
{: #assistant-add}

Créez un assistant doté des compétences nécessaires pour répondre aux objectifs de vos clients.
{: shortdesc}

Pour en savoir plus sur ce qu’est un assistant en premier lieu, reportez-vous à la rubrique [Assistants](/docs/services/assistant?topic=assistant-assistants).

Procédez comme suit pour créer un assistant :

1.  Cliquez sur **Create assistant**.

1.  Ajoutez des détails sur le nouvel assistant :

    - **Name** : nom qui ne doit pas excéder 100 caractères. Le nom est obligatoire.
    - **Description** : description facultative qui ne doit pas excéder 200 caractères.

    Une page Web publique IBM est créée automatiquement et peut être utilisée par vous-même et votre équipe pour tester votre assistant. Si vous ne souhaitez pas que la page Web d'aperçu soit créée, désélectionnez la case **Enable Preview Link**.

1.  Cliquez sur **Create assistant**.

1.  Ajoutez une compétence à l'assistant en choisissant l'un des types de compétence suivants à ajouter. 

    **Remarque**: vous pouvez choisir d’ajouter une compétence existante ou d’en créer une autre. 

    - **Add Dialog Skill** : utilise les technologies de traitement du langage naturel et d’apprentissage automatique Watson pour comprendre les questions et les demandes des utilisateurs, et y répondre par des réponses que vous avez écrites.

      Lorsque vous ajoutez une compétence de dialogue à partir de là, vous obtenez la version de développement. Si vous souhaitez ajouter une version de compétence de dialogue spécifique, ajoutez-la à partir de la page*Versions*.

    - **Add Search Skill** ![Forfait Plus ou Premium uniquement](images/plus.png) : pour une requête utilisateur donnée, utilise le service {{site.data.keyword.discoveryfull}} pour extraire des informations d'une source de données que vous identifiez et partage toutes les informations pertinentes qu'il trouve en tant que réponse à l'utilisateur.

      Cette option n'est visible que si vous êtes un utilisateur du forfait Plus ou Premium.
      {: note}

    Reportez-vous à la rubrique [Création d'une compétence](/docs/services/assistant?topic=assistant-skill-add).

## Limites de l'assistant
{: #assistant-add-limits}

Le nombre d'assistants que vous pouvez créez dépend de votre type de forfait {{site.data.keyword.conversationshort}}. 

| Forfait     | Nombre d'assistants par instance de service |
|--------------|--------------------------------:|
| Premium      |                             100 |
| Plus         |                             100 |
| Standard     |                             100 |
| Plus Trial   |                               5 |
| Lite*        |                             100 |
{: caption="Détails du forfait" caption-side="top"}

*Au bout de 30 jours d'inactivité, un assistant inutilisé dans une instance de service de forfait Lite peut être supprimé afin de libérer de l'espace.

Pour plus d'informations sur ce sujet, reportez-vous à la rubrique [Modification du paramètre de délai d'attente d'inactivité](/docs/services/assistant?topic=assistant-assistant-settings).

Vous pouvez connecter une compétence de chaque type à votre assistant. Le nombre des compétences que vous pouvez créer dépend de votre forfait. Pour plus d'informations, reportez-vous à la rubrique [Limites de compétence](/docs/services/assistant?topic=assistant-skill-add#skill-add-limits).

## Suppression d'un assistant
{: #assistant-add-delete}

Lorsque vous supprimez un assistant, toutes les intégrations que vous avez définies pour cet assistant sont également automatiquement supprimées. Les compétences que vous avez ajoutées à l’assistant ne sont pas supprimées.

Pour supprimer un assistant, procédez comme suit :

1.  Dans l’onglet Assistants, recherchez l’assistant que vous souhaitez supprimer.

1.  Cliquez sur l'icône ![ouvrir et fermer la liste d'options](images/kabob-beta.png), puis choisissez **Delete**. Confirmez la suppression.

## Changement de nom d'un assistant
{: #assistant-add-rename}

Vous pouvez modifier le nom d'un assistant et sa description associée après la création de cet assistant.

Pour renommer un assistant, procédez comme suit :

1.  Dans l’onglet Assistants, recherchez l’assistant que vous souhaitez renommer.

1.  Cliquez sur l'icône ![ouvrir et fermer la liste d'options](images/kabob-beta.png), puis choisissez **Rename**.

1.  Modifiez le nom, puis cliquer sur **Rename** pour enregistrer vos modifications.

### Modification de la compétence associée à l'assistant
{: #assistant-add-swap-skill}

Vous pouvez ajouter une compétence de chaque type de compétence à un assistant. Si vous souhaitez modifier une compétence utilisée par votre assistant, vous pouvez échanger une compétence contre une autre. 

1.  Dans l'onglet Assistants, ouvrez l'assistant. 

1.  Cliquez sur l'icône ![ouvrir et fermer la liste d'options](images/kabob-beta.png) pour la compétence à échanger, puis choisissez **Swap skill**. 

    Pour remplacer la compétence de dialogue actuelle par une version différente de celle-ci, choisissez **Change skill version**.

1.  Choisissez une compétence existante à utiliser à la place ou [créez une compétence](/docs/services/assistant?topic=assistant-skill-add).

### Basculement entre les instances de service
{: #assistant-add-switch-instance}

Si vous avez plusieurs instances de service, vous pouvez consulter l'en-tête de page pour savoir quelle instance vous utilisez actuellement. Si vous travaillez dans une compétence, cliquez d'abord sur le lien de navigation **Skills**. La bannière affiche le nom de l'instance actuelle. Pour passer à une autre instance de service, cliquez sur **Change**, puis choisissez l'instance appropriée.
