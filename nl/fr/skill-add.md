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

# Création d'une compétence
{: #skill-add}

Le traitement en langage naturel du service {{site.data.keyword.conversationshort}} est défini dans une *compétence de dialogue*, qui contient tous les artefacts qui définissent une flux de conversation.
{: shortdesc}

Vous pouvez ajouter une compétence à un assistant. 

Vous pouvez créer une compétence, utiliser un exemple de compétence fourni par IBM ou importer une compétence à partir d'un fichier JSON. Pour ajouter une compétence, procédez comme suit : 

1.  Si vous n'avez pas créé d'instance de service {{site.data.keyword.conversationshort}}, suivez d'abord cette procédure unique. Sinon, ignorez cette étape. 

    1.  Accédez à la page [{{site.data.keyword.conversationshort}} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://{DomainName}/catalog/services/watson-assistant) dans le catalogue {{site.data.keyword.cloud_notm}}.

    1.  Inscrivez-vous à un compte {{site.data.keyword.cloud_notm}} ou connectez-vous.

        L'instance de service sera créée dans le groupe de ressources **par défaut** si vous n'en choisissez pas un autre et elle *ne pourra pas* être modifiée ultérieurement. Le groupe *par défaut* est généralement suffisant. Toutefois, si vous souhaitez en savoir plus sur les groupes de ressources, reportez-vous à la [Documentation {{site.data.keyword.Bluemix_notm}}![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://{DomainName}/docs/resources?topic=resources-bp_resourcegroups#bp_resourcegroups){: new_window}.

        Vous pouvez utiliser une instance de service pour développer, tester et déployer un assistant. Par conséquent, ne créez pas de groupes de ressources distincts pour différents types d'environnements de déploiement.
        {:tip}

        Avec un forfait Premium, vous pouvez créer plusieurs instances. Les instances doivent toutes être créées dans le même groupe de ressources. 

    1.  Cliquez sur **Create**.

    1.  Cliquez sur **Launch tool**. Si vous êtes invité à vous connecter à l'outil, entrez vos données d'identification {{site.data.keyword.cloud_notm}}. 

1.  Cliquez sur l'onglet **Skills**.

    Si vous avez créé ou avez reçu un accès de rôle de développeur à des espaces de travail construits avec la version généralement disponible du service {{site.data.keyword.conversationshort}} (anciennement Watson Conversation), vous les verrez énumérés ici en tant que compétences de dialogue.
    {: note}

1.  Cliquez sur **Create new**.

1.  Effectuez l'une des tâches suivantes :

    - Pour créer une compétence, cliquez sur **Create skill**.
    - Pour ajouter un exemple de compétence fourni avec le service comme point de départ de votre propre compétence ou comme exemple à explorer avant de créer une compétence vous-même, cliquez sur **Use sample skill**, puis cliquez sur l'exemple que vous souhaitez utiliser.

      L'exemple de compétence est ajoutée à la liste de vos compétences. Il n'est pas associé à des assistants. Passez les dernières étapes de cette procédure.

    - Pour ajouter une compétence existante à cette instance de service, vous pouvez l'importer sous forme de fichier JSON. Cliquez sur **Import skill**, puis cliquez sur **Choose JSON File**, puis sélectionnez le fichier JSON à importer.

      **Important :**

      - Le fichier JSON importé doit utiliser le codage UTF-8, sans codage BOM (Byte Order Mark).
      - Un fichier JSON de compétence ne doit pas excéder 10 Mo. Si la compétence que vous devez importer est plus grande, vous avez la possibilité d'importer les intentions et les entités séparément après avoir importé la compétence. (Vous pouvez également importer des compétences plus grandes à l'aide de l'API REST. Pour plus d'informations, reportez-vous à la documentation [Référence d'API ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://cloud.ibm.com/apidocs/assistant?curl=#create-workspace){: new_window}.)
      - Le fichier JSON ne peut pas contenir de tabulations, de retours à la ligne ou de retours chariot.

      Spécifiez les données que vous souhaitez inclure :

        - Sélectionnez **Everything (Intents, Entities, and Dialog)** si vous souhaitez importer une copie entière de la compétence exportée, y compris le dialogue.
        - Sélectionnez **Intents and Entities** si vous souhaitez utiliser les intentions et les entités de la compétence exportée, mais que vous prévoyez de créer un dialogue.

      Cliquez sur **Import**.

      Si vous ne parvenez pas à importer une compétence, reportez-vous à la rubrique [Traitement des incidents liés à l'importation de compétences](#skill-add-import-errors).

1.  Spécifiez les détails de la compétence :

    - **Name** : nom qui ne doit pas excéder 100 caractères. Le nom est obligatoire.
    - **Description** : description facultative qui ne doit pas excéder 200 caractères.
    - **Language** : langue dans laquelle les entrées utilisateur seront saisies et que la compétence sera formée à comprendre. La valeur par défaut est l'anglais.

Une fois la compétence créée, elle apparaît sous la forme d'une vignette sur la page Skills. Vous pouvez maintenant commencer à identifier les objectifs utilisateur que la compétence de dialogue doit traiter. 

- Pour ajouter des intentions prédéfinies à votre compétence, reportez-vous à la rubrique [Utilisation des catalogues de contenu](/docs/services/assistant?topic=assistant-catalog).
- Pour définir vos propres intentions, reportez-vous à la rubrique [Définition d'intentions](/docs/services/assistant?topic=assistant-intents).

La compétence de dialogue ne peut pas interagir avec les clients tant qu'elle n'est pas ajoutée à un assistant et que celui-ci n'est pas déployé. Reportez-vous à la rubrique [Création d'un assistant](/docs/services/assistant?topic=assistant-assistant-add).


### Traitement des incidents liés à l'importation de compétences 
{: #skill-add-import-errors}

Si vous recevez un message indiquant que la compétence contient des artefacts dépassant les limites imposées par votre forfait de service, procédez comme suit pour importer la compétence : 

1.  Achetez un forfait avec des limites d'artefact plus élevées. 
1.  Créez une instance de service dans le nouveau forfait. 
1.  Importez la compétence vers la nouvelle instance de service.  
1.  Apportez des modifications à la compétence pour qu'elle réponde aux exigences en matière de limites d'artefact pour le forfait que vous souhaitez utiliser. Par exemple, vous devrez peut-être réduire le nombre de noeuds de dialogue utilisés dans l’arborescence. 
1.  Exportez la compétence modifiée en la téléchargeant. 
1.  Réessayez d'importer la compétence modifiée dans l'instance de service d'origine du forfait souhaité. 

### Ajout de la compétence à un assistant  
{: #skill-add-to-assistant}

Vous pouvez ajouter une compétence à un assistant. Vous devez ouvrir la vignette de l’assistant et ajouter la compétence à l’assistant à partir de la page de configuration de l’assistant ; vous ne pouvez pas choisir l'assistant qui utilisera la compétence dans la page de configuration de la compétence. Une compétence de dialogue peut être utilisée par plusieurs assistants.

1.  Dans l'onglet Assistants, cliquez pour ouvrir la vignette de l'assistant auquel vous souhaitez ajouter la compétence. 

1.  Cliquez sur **Add Dialog Skill**.

1.  Cliquez sur **Add existing skill**.

    Cliquez sur la compétence que vous souhaitez ajouter parmi les compétences disponibles affichées. 

    Lorsque vous ajoutez une compétence à partir de là, vous obtenez la version de développement. Si vous souhaitez ajouter une version de compétence spécifique, ajoutez-la à partir de l'onglet *Version History* de la compétence.

## Limites de compétence
{: #skill-add-limits}

Le nombre de compétences que vous pouvez créer dans une instance de service dépend de votre forfait {{site.data.keyword.conversationshort}}. Tous les exemples de compétences de dialogue disponibles dans votre instance de service ne sont pris en compte dans votre limite que si vous les modifiez ou les dupliquez. 

| Forfait de service     | Compétences par instance de service |
|------------------|----------------------------:|
| Premium          |                          50 |
| Plus             |                          50 |
| Standard         |                          20 |
| Lite*            |                           5 |
{: caption="Détails de forfait de service" caption-side="top"}

*Au bout de 30 jours d'inactivité, une compétence inutilisée dans une instance de service de forfait Lite peut être supprimée afin de libérer de l'espace.

## Téléchargement d'une compétence de dialogue 
{: #skill-add-download}

Vous pouvez télécharger une compétence de dialogue au format JSON. Vous pouvez télécharger une compétence si vous souhaitez utiliser la même compétence de dialogue dans une instance différente du service {{site.data.keyword.conversationshort}} par exemple. Vous pouvez la télécharger à partir d'une instance et l'importer dans une autre instance en tant que nouvelle compétence de dialogue. 

Pour télécharger une compétence de dialogue, procédez comme suit : 

1.  Recherchez la vignette de compétence de dialogue sur la page Skills ou sur la page de configuration d'un assistant utilisant la compétence. 

1.  Cliquez sur l'icône ![ouvrir et fermer la liste d'options](images/kabob-beta.png), puis choisissez **Download JSON**. 

1.  Spécifiez un nom pour le fichier JSON et l'emplacement de sauvegarde, puis cliquez sur **Save**.

Vous pouvez exporter une compétence en utilisant également l'API. Incluez le paramètre `export=true` dans la demande. Pour plus d'informations, reportez-vous à [API reference](https://cloud.ibm.com/apidocs/assistant#get-information-about-a-workspace).

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

## Partage d'une compétence de dialogue avec des membres de l'équipe 
{: #skill-add-invite-others}

Une fois l'instance de service créée, vous pouvez autoriser d'autres personnes à y accéder. Vous pouvez définir les données d'apprentissage et créer le dialogue en même temps.

**Important** : une seule personne à la fois peut éditer une intention, une entité ou un noeud de dialogue. Si plusieurs personnes travaillent en même temps sur le même élément, les modifications apportées par la personne qui les sauvegarde en dernier sont les seules qui sont appliquées. Les modifications qui sont apportées au cours de la même période par quelqu'un d'autre et qui sont sauvegardées en premier ne sont pas conservées. Coordonnez les mises à jour que vous prévoyez d'effectuer avec les membres de votre équipe afin d'empêcher les personnes de perdre leur travail.

Pour partager une compétence de dialogue avec d'autres personnes, vous devez octroyer à ces dernières un accès à l'instance de service qui héberge la compétence. Notez que la personne que vous invitez pourra accéder à toutes les compétences hébergées par cette instance de service. 

1.  Notez le nom de l'instance en cours, qui s'affiche en haut de la page en cours 
1.  Cliquez sur l'icône Utilisateur ![Utilisateur](images/user-icon2.png) dans l'en-tête de page, puis sélectionnez **Gérer les utilisateurs** dans le menu déroulant. 

1.  Cliquez sur **Inviter des utilisateurs**, puis entrez les adresses électroniques des personnes de votre équipe auxquelles vous souhaitez accorder un accès.

    Si vous avez donné à quelqu'un l'accès à une instance de service lorsqu'elle était gérée dans Cloud Foundry, il est possible que cette personne soit déjà répertoriée en tant qu'utilisateur invité. Cliquez sur le nom de la personne pour ouvrir les paramètres de gestion des accès pour l'utilisateur. Cliquez sur **Affecter un accès**, puis choisissez **Affecter l'accès aux ressources**.


1.  Dans la section *Services*, effectuez au minimum les sélections suivantes : 

    - **Services** : {{site.data.keyword.conversationshort}}
    - **Affecter des rôles d'accès à une plateforme** : Opérateur

    Pour plus d'informations sur les rôles de gestion des plateformes, reportez-vous à la rubrique [Accès IAM ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](/docs/iam?topic=iam-userroles). (Les rôles d'accès au service ne sont pas exploités par {{site.data.keyword.conversationshort}} par défaut.)

    Pour les instances plus anciennes gérées par Cloud Foundry, vous devez développer la section *Accès Cloud Foundry*, choisir votre organisation, puis affecter cette personne au rôle d'espace **Développeur**.
    {: note}

1.  Cliquez sur **Inviter des utilisateurs**.

    Si vous modifiez l'accès d'un utilisateur existant, cliquez sur **Affecter un accès**.

Lorsque les personnes que vous invitez ensuite se connecteront à {{site.data.keyword.cloud_notm}}, votre compte sera inclus dans leur liste de comptes. Si elles sélectionnent votre compte, elles peuvent voir votre instance de service et ouvrir et modifier vos compétences. 

Dans la mesure où davantage de personnes contribuent au développement des compétences de dialogue, des modifications inattendues peuvent se produire, notamment des suppressions de compétences. Pensez à créer régulièrement des copies de sauvegarde de votre compétence de dialogue de manière à pouvoir rétablir une version antérieure si besoin. Pour créer une copie de sauvegarde, il vous suffit de [télécharger la compétence en tant que fichier JSON](#skill-add-download).
