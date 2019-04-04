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

# Création d'un assistant
{: #assistant-add}

Créez un assistant doté des compétences nécessaires pour répondre aux objectifs de vos clients.
{: shortdesc}

Procédez comme suit pour créer un assistant :

1.  Cliquez sur l'onglet **Assistants**.

1.  Effectuez l'une des tâches suivantes :

    - Pour créer un exemple d'assistant que vous pouvez consulter et dont vous pouvez tirer des leçons, cliquez sur **Add a sample**, puis choisissez l'assistant exemple à créer.

      L’exemple d'assistant est ajouté. Vous pouvez ignorer les étapes restantes de cette procédure.

      Un exemple de compétence est fourni avec l’exemple d’assistant et est ajouté à votre liste de compétences. Si vous avez déjà créé un exemple de compétence du même type, la compétence existante est automatiquement associée à ce nouvel assistant.
      {: note}

    - Pour créer un assistant de toutes pièces, cliquez sur **Create new**, puis effectuez les étapes restantes de cette procédure.

1.  Spécifiez les détails du nouvel assistant :
    - **Name** : nom qui ne doit pas excéder 100 caractères. Le nom est obligatoire.
    - **Description** : description facultative qui ne doit pas excéder 200 caractères.

    Une page Web publique IBM est créée automatiquement et peut être utilisée par vous-même et votre équipe pour tester votre assistant. Si vous ne souhaitez pas que la page Web d'aperçu soit créée, désélectionnez la case **Enable Preview Link**.

1.  Cliquez sur **Create**.

1.  Ajoutez une compétence à l’assistant en cliquant sur **Add skill**. Vous pouvez choisir d’ajouter une compétence existante ou d’en créer une nouvelle. 

    Lorsque vous ajoutez une compétence à partir de là, vous obtenez la version de développement. Si vous souhaitez ajouter une version de compétence spécifique, ajoutez-la à partir de l'onglet *Version History* de la compétence.

    Si vous avez créé ou avez reçu un accès de rôle de développeur à des espaces de travail construits avec la version généralement disponible du service {{site.data.keyword.conversationshort}} (anciennement Watson Conversation), vous les verrez énumérés en tant que compétences de dialogue existantes.
    {: note}

    Pour plus d'informations sur la création d'une compétence, reportez-vous à la rubrique [Création d'une compétence](/docs/services/assistant?topic=assistant-skill-add).

## Limites de l'assistant 
{: #assistant-add-limits}

Le nombre d'assistants que vous pouvez créer dans une instance de service dépend de votre forfait {{site.data.keyword.conversationshort}}. 

| Forfait de service |Nombre d'assistants par instance de service | Intégrations par assistant  | Période d'inactivité de session de discussion |
|--------------|--------------------------------:|----------------------------:|-----------------:|
| Premium      |                             100 |                         100 |       60 minutes |
| Plus         |                             100 |                         100 |       60 minutes |
| Standard     |                             100 |                         100 |        5 minutes |
| Lite*        |                             100 |                         100 |        5 minutes |
{: caption="Détails de forfait de service" caption-side="top"}

*Au bout de 30 jours d'inactivité, un assistant inutilisé dans une instance de service de forfait Lite peut être supprimé afin de libérer de l'espace.

Vous pouvez connecter une compétence à votre assistant. Le nombre des compétences que vous pouvez créer dépend de votre forfait. Pour plus d'informations, reportez-vous à la rubrique [Limites de compétence](/docs/services/assistant?topic=assistant-skill-add#skill-add-limits).

## Suppression d'un assistant
{: #assistant-add-delete}

Lorsque vous supprimez un assistant, toutes les intégrations que vous avez définies pour cet assistant sont également automatiquement supprimées.  Les compétences que vous avez ajoutées à l’assistant ne sont pas supprimées.

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

Vous pouvez ajouter une compétence à un assistant. Si vous souhaitez modifier la compétence utilisée par votre assistant, vous pouvez échanger une compétence contre une autre. 

1.  Dans l'onglet Assistants, cliquez pour ouvrir la vignette de l'assistant pour lequel vous souhaitez modifier la compétence 

1.  Cliquez sur l'icône ![ouvrir et fermer la liste d'options](images/kabob-beta.png), puis choisissez **Swap skill**. Pour remplacer la compétence actuelle par une version différente de celle-ci, choisissez **Change skill version**.

1.  Choisissez une compétence existante à utiliser à la place ou [créez une compétence](/docs/services/assistant?topic=assistant-skill-add).

### Basculement entre les instances de service
{: #assistant-add-switch-instance}

Si vous avez plusieurs instances de service, vous pouvez consulter l'en-tête de page pour savoir quelle instance vous utilisez actuellement. Si vous travaillez dans une compétence, cliquez d'abord sur le lien de navigation **Skills**. La bannière affiche le nom de l'instance actuelle. Pour passer à une autre instance de service, cliquez sur **change**, puis choisissez l'instance appropriée.
