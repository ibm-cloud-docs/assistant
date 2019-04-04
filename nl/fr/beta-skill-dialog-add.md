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

# Génération d'une compétence de dialogue 
{: #beta-skill-dialog-add}

Le traitement en langage naturel du service {{site.data.keyword.conversationshort}} est défini dans une *compétence de dialogue*, qui contient tous les artefacts qui définissent une flux de conversation.
{: shortdesc}

Cette fonctionnalité est uniquement disponible pour les participants du programme bêta. Pour savoir comment demander un accès, reportez-vous à la rubrique [Participer au programme bêta](/docs/services/assistant?topic=assistant-feedback#feedback-beta).

![Beta](images/beta.png) IBM publie des services, des fonctions et un support de langue pour votre évaluation qui sont classifiés comme version bêta. Ces fonctions peuvent être instables, changer fréquemment ou être arrêtées dans un délai très court. Les fonctions bêta, qui risquent de ne pas fournir le même niveau de performance ou de compatibilité que les fonctions de l'édition GA (General Availability), ne sont pas destinées à être utilisées dans un environnement de production. 

Vous pouvez ajouter une compétence de dialogue à un assistant. Pour plus d'informations sur les limites de chaque forfait, reportez-vous à la rubrique [Limites de compétence](/docs/services/assistant?topic=assistant-skill-add#skill-add-limits).

## Création d'une compétence de dialogue 
{: #beta-skill-dialog-add-task}

Vous pouvez créer une compétence, utiliser un exemple de compétence fourni par IBM ou importer une compétence à partir d'un fichier JSON. 

Si vous ne l'avez pas déjà fait, suivez les étapes préalables décrites dans le [Tutoriel d'initiation](/docs/services/assistant?topic=assistant-getting-started#getting-started-prerequisites) pour créer une instance de service {{site.data.keyword.conversationshort}} et lancer l'outil.

Vous utilisez l'outil {{site.data.keyword.conversationshort}} pour générer des compétences. Procédez comme suit pour créer une compétence de dialogue :

1.  Cliquez sur l'onglet **Skills**.

    Si vous avez créé ou reçu un accès de rôle de développeur à des espaces de travail conçus avec la version commercialisée du service {{site.data.keyword.conversationshort}} (anciennement Watson Conversation), vous les verrez énumérés ici en tant que compétences de dialogue.
    {: note}

1.  Cliquez sur **Create new**.

1.  Cliquez sur **Add** pour créer une *compétence de dialogue*.

1.  Effectuez l'une des tâches suivantes :

    - Pour créer une compétence, cliquez sur **Create skill**.
    - Pour ajouter un exemple de compétence fourni avec le service comme point de départ de votre propre compétence ou comme exemple à explorer avant de créer une compétence vous-même, cliquez sur **Use sample skill**, puis cliquez sur l'exemple que vous souhaitez utiliser.

      L'exemple de compétence est ajoutée à la liste de vos compétences. Il n'est pas associé à des assistants. Passez les dernières étapes de cette procédure.

    - Pour ajouter une compétence existante à cette instance de service, vous pouvez l'importer sous forme de fichier JSON. Cliquez sur **Import skill**, puis cliquez sur **Choose JSON File**, puis sélectionnez le fichier JSON à importer.

      **Important :**

      - Le fichier JSON importé doit utiliser le codage UTF-8.
      - Un fichier JSON de compétence ne doit pas excéder 10 Mo. Si la compétence que vous devez importer est plus grande, vous avez la possibilité d'importer les intentions et les entités séparément après avoir importé la compétence. (Vous pouvez également importer des compétences plus grandes à l'aide de l'API REST. Pour plus d'informations, reportez-vous à la documentation [Référence d'API ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://www.ibm.com/watson/developercloud/conversation/api/v1/#create_workspace){: new_window}.)
      - Le fichier JSON ne peut pas contenir de tabulations, de retours à la ligne ou de retours chariot.

      Spécifiez les données que vous souhaitez inclure :

        - Sélectionnez **Everything (Intents, Entities, and Dialog)** si vous souhaitez importer une copie entière de la compétence exportée, y compris le dialogue.
        - Sélectionnez **Intents and Entities** si vous souhaitez utiliser les intentions et les entités de la compétence exportée, mais que vous prévoyez de créer un dialogue.

      Cliquez sur **Import**.

      Si vous ne parvenez pas à importer une compétence, reportez-vous à la rubrique [Traitement des incidents liés à l'importation de compétences](#beta-skill-dialog-add-import-errors).

1.  Spécifiez les détails de la compétence :

    - **Name** : nom qui ne doit pas excéder 100 caractères. Le nom est obligatoire.
    - **Description** : description facultative qui ne doit pas excéder 200 caractères.
    - **Language** : langue dans laquelle les entrées utilisateur seront saisies et que la compétence sera formée à comprendre. La valeur par défaut est l'anglais.

Une fois la compétence créée, elle apparaît sous la forme d'une vignette sur la page Skills. Vous pouvez maintenant commencer à identifier les objectifs utilisateur que la compétence de dialogue doit traiter. 

- Pour ajouter des intentions prédéfinies à votre compétence, reportez-vous à la rubrique [Utilisation des catalogues de contenu](/docs/services/assistant?topic=assistant-catalog).
- Pour définir vos propres intentions, reportez-vous à la rubrique [Intentions](/docs/services/assistant?topic=assistant-intents).

La compétence de dialogue ne peut pas interagir avec les clients tant qu'elle n'est pas ajoutée à un assistant et que celui-ci n'est pas déployé. Reportez-vous à la rubrique [Création d’assistants](/docs/services/assistant?topic=assistant-assistant-add).

### Traitement des incidents liés à l'importation de compétences 
{: #beta-skill-dialog-add-import-errors}

Si vous recevez un message indiquant que la compétence contient des artefacts dépassant les limites imposées par votre forfait de service, procédez comme suit pour importer la compétence : 

1.  Achetez un forfait avec des limites d'artefact plus élevées. 
1.  Créez une instance de service dans le nouveau forfait. 
1.  Importez la compétence vers la nouvelle instance de service.  
1.  Apportez des modifications à la compétence pour qu'elle réponde aux exigences en matière de limites d'artefact pour le forfait que vous souhaitez utiliser. Par exemple, vous devrez peut-être réduire le nombre de noeuds de dialogue utilisés dans l’arborescence. 
1.  Exportez la compétence modifiée en la téléchargeant. 
1.  Réessayez d'importer la compétence modifiée dans l'instance de service d'origine du forfait souhaité. 

### Ajout de la compétence à un assistant  
{: #beta-skill-dialog-add-to-assistant}

Vous pouvez ajouter une compétence à un assistant. Vous devez ouvrir la vignette de l’assistant et ajouter la compétence à l’assistant à partir de la page de configuration de l’assistant ; vous ne pouvez pas choisir l'assistant qui utilisera la compétence dans la page de configuration de la compétence. Une compétence de dialogue peut être utilisée par plusieurs assistants.

1.  Dans l'onglet Assistants, cliquez pour ouvrir la vignette de l'assistant auquel vous souhaitez ajouter la compétence. 

1.  Cliquez sur **Add Dialog Skill**.

1.  Cliquez sur **Add existing skill**.

    Cliquez sur la compétence que vous souhaitez ajouter parmi les compétences disponibles affichées. 

    Si vous ajoutez une compétence de dialogue existante et si vous avez créé ou reçu un accès de rôle de développeur à des espaces de travail conçus avec la version commercialisée du service {{site.data.keyword.conversationshort}} (anciennement Watson Conversation), vous les verrez énumérés ici en tant que compétences de dialogue.
    {: note}

## Téléchargement d'une compétence de dialogue 
{: #beta-skill-dialog-add-download}

Vous pouvez télécharger une compétence de dialogue au format JSON. Vous pouvez télécharger une compétence si vous souhaitez utiliser la même compétence de dialogue dans une instance différente du service {{site.data.keyword.conversationshort}} par exemple. Vous pouvez la télécharger à partir d'une instance et l'importer dans une autre instance en tant que nouvelle compétence de dialogue. 

Pour télécharger une compétence de dialogue, procédez comme suit : 

1.  Recherchez la vignette de compétence de dialogue sur la page Skills ou sur la page de configuration d'un assistant utilisant la compétence. 

1.  Cliquez sur l'icône ![ouvrir et fermer la liste d'options](images/kabob-beta.png), puis choisissez **Download JSON**. 

1.  Spécifiez un nom pour le fichier JSON et l'emplacement de sauvegarde, puis cliquez sur **Save**.

Vous pouvez exporter une compétence en utilisant également l'API. Incluez le paramètre `export=true` dans la demande. Pour plus d'informations, reportez-vous à [API reference](https://cloud.ibm.com/apidocs/assistant#get-information-about-a-workspace).

## Partage d'une compétence de dialogue avec des membres de l'équipe 
{: #beta-skill-dialog-add-invite-others}

Une fois l'instance de service créée, vous pouvez autoriser d'autres personnes à y accéder. Vous pouvez définir les données d'apprentissage et créer le dialogue en même temps.

Une seule personne à la fois peut éditer une intention, une entité ou un noeud de dialogue. Si plusieurs personnes travaillent en même temps sur le même élément, les modifications apportées par la personne qui les sauvegarde en dernier sont les seules qui sont appliquées. Les modifications qui sont apportées au cours de la même période par quelqu'un d'autre et qui sont sauvegardées en premier ne sont pas conservées. Coordonnez les mises à jour que vous prévoyez d'effectuer avec les membres de votre équipe afin d'empêcher les personnes de perdre leur travail.
{: important}

Pour partager une compétence de dialogue avec d'autres personnes, vous devez octroyer à ces dernières un accès à l'instance de service qui héberge la compétence. Notez que la personne que vous invitez pourra accéder à toutes les compétences hébergées par cette instance de service. 

1.  Notez le nom de l'instance en cours, qui s'affiche en haut de la page en cours 
1.  Cliquez sur l'icône Utilisateur ![Utilisateur](images/user-icon2.png) dans l'en-tête de page, puis sélectionnez **Gérer les utilisateurs** dans le menu déroulant. 

1.  Cliquez sur **Inviter des utilisateurs**, puis entrez les adresses électroniques des personnes de votre équipe auxquelles vous souhaitez accorder un accès.

Si vous avez donné à quelqu'un l'accès à une instance de service dans Cloud Foundry, il est possible que cette personne soit déjà répertoriée en tant qu'utilisateur invité. Cliquez sur le nom de la personne pour ouvrir les paramètres de gestion des accès pour l'utilisateur. Cliquez sur **Affecter un accès**, puis choisissez **Affecter l'accès aux ressources**.
1.  Dans la section *Services*, effectuez au minimum les sélections suivantes : 

    - **Services** : {{site.data.keyword.conversationshort}}
    - **Affecter des rôles d'accès à une plateforme** : Opérateur

    Pour plus d'informations sur les rôles de gestion des plateformes, reportez-vous à la rubrique [Accès IAM ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](/docs/iam?topic=iam-userroles). (Les rôles d'accès au service ne sont pas exploités par {{site.data.keyword.conversationshort}} par défaut.)

    Pour les instances plus anciennes gérées par Cloud Foundry, vous devez développer la section *Accès Cloud Foundry*, choisir votre organisation, puis affecter cette personne au rôle d'espace **Développeur**.
    {: note}

1.  Cliquez sur **Inviter des utilisateurs**.

    Si vous modifiez l'accès d'un utilisateur existant, cliquez sur **Affecter un accès**.

Lorsque les personnes que vous invitez ensuite se connecteront à {{site.data.keyword.cloud_notm}}, votre compte sera inclus dans leur liste de comptes. Si elles sélectionnent votre compte, elles peuvent voir votre instance de service et ouvrir et modifier vos compétences. 

Dans la mesure où davantage de personnes contribuent au développement des compétences de dialogue, des modifications inattendues peuvent se produire, notamment des suppressions de compétences. Pensez à créer régulièrement des copies de sauvegarde de votre compétence de dialogue de manière à pouvoir rétablir une version antérieure si besoin. Pour créer une copie de sauvegarde, il vous suffit de [télécharger la compétence en tant que fichier JSON](#beta-skill-dialog-add-download).
