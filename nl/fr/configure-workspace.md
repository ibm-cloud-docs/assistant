---

copyright:
  years: 2015, 2018
lastupdated: "2018-02-15"

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

# Configuration d'un espace de travail {{site.data.keyword.conversationshort}}

Le traitement automatique du langage naturel pour le service {{site.data.keyword.conversationshort}} a lieu au sein d'un *espace de travail*. Il s'agit du conteneur de tous les artefacts qui définissent le flux de conversation d'une application.
{: shortdesc}

Une instance de service {{site.data.keyword.conversationshort}} peut contenir plusieurs espaces de travail. Un espace de travail contient les types d'artefact suivants :

- [**Intentions**](intents.html) : une *intention* représente l'objectif d'une entrée utilisateur, par exemple, une question sur le site d'une entreprise ou sur le règlement d'une facture. Vous définissez une intention pour chaque type de demande d'utilisateur qui doit être prise en charge par votre application. Dans l'outil, le nom de l'intention est toujours précédé du préfixe `#`. Pour entraîner l'espace de travail à reconnaître vos intentions, vous fournissez un grand nombre d'exemples d'entrée utilisateur et vous indiquez à quelles intentions ils correspondent.
- [**Entités**](entities.html) : une *entité* représente un terme ou un objet en rapport avec vos intentions et qui fournit un contexte spécifique pour votre intention. Par exemple, une entité peut représenter une ville dans laquelle l'utilisateur souhaite
rechercher un site d'entreprise ou le montant d'un règlement de facture. Dans l'outil, le nom d'une entité est toujours précédé du préfixe `@`. Pour entraîner l'espace de travail à reconnaître vos entités, vous fournissez la liste des valeurs possibles pour chaque entité et les synonymes que les utilisateurs sont susceptibles d'entrer.
- [**Dialogue**](dialog-build.html) : un *dialogue* est un flux de conversation constitué de branches qui explique de quelle manière votre application répond lorsqu'elle reconnaît les intentions et les entités définies. Vous utilisez le générateur de dialogue inclus dans l'outil pour créer des conversations avec les utilisateurs, en fournissant des réponses basées sur les intentions et les entités reconnues dans leurs entrées.

A mesure que vous ajoutez des informations, l'espace de travail s'entraîne lui-même. Vous n'avez rien à faire pour initier cet entraînement.

## Nombre limite d'espaces de travail
{: #workspace-limits}

Le nombre d'espaces de travail que vous pouvez créer dans une instance de service dépend de votre plan de service {{site.data.keyword.conversationshort}}. L'exemple d'espace de travail n'est pas pris en compte dans le nombre limite d'espaces de travail tant que vous ne l'éditez ou ne le dupliquez pas :

| Plan de service     | Nombre d'espaces de travail par instance de service |
|------------------|--------------------------------:|
| Standard/Premium |                              20 |
| Lite*            |                               5 |

*Au bout de 30 jours d'inactivité, un espace de travail Lite inutilisé peut être supprimé afin de libérer de l'espace.

## Création d'espaces de travail
{: #creating-workspaces}

Vous pouvez créer un espace de travail, utiliser l'exemple d'espace de travail fourni ou importer un espace de travail à partir d'un fichier JSON. Vous pouvez également dupliquer un espace de travail existant dans une même instance de service.

Vous utilisez l'outil {{site.data.keyword.conversationshort}} pour créer des espaces de travail. Procédez comme suit pour créer un espace de travail :

1.  Si la page "Service Details" n'est pas encore ouverte, cliquez sur votre instance de service {{site.data.keyword.conversationshort}} à partir de la console. (Lorsque vous créez une instance de service, la page "Service Details" s'affiche.)

1.  Cliquez sur **Launch tool**.

1.  Effectuez l'une des tâches suivantes :
    - Pour créer un espace de travail, cliquez sur **Create**.
    - Pour utiliser l'exemple d'espace de travail comme point de départ pour votre propre espace de travail, éditez-le. En revanche, si vous souhaitez l'utiliser pour plusieurs espaces de travail, dupliquez-le.
    - Pour importer un espace de travail à partir d'un fichier JSON, cliquez sur l'icône ![Importer un espace de travail](images/workspace_import.png), puis sélectionnez le fichier JSON que vous souhaitez utiliser comme source pour l'importation.

        **Important :**

        - Le fichier JSON importé doit utiliser le codage UTF-8.
        - Un fichier JSON d'espace de fichier ne doit pas excéder 10 Mo. Si l'espace de travail que vous devez importer est plus grand, vous avez la possibilité d'importer les intentions et les entités séparément après avoir importé l'espace de travail. (Vous pouvez également importer des espaces de travail plus grands à l'aide de l'API REST. Pour plus d'informations, reportez-vous à la documentation [API Reference ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://www.ibm.com/watson/developercloud/conversation/api/v1/#create_workspace){: new_window}.)
        - Le fichier JSON ne peut pas contenir de tabulations, de retours à la ligne ou de retours chariot.

        Spécifiez les données que vous souhaitez inclure :

        - Sélectionnez **Everything (Intents, Entities, and Dialog)** si vous souhaitez importer une copie entière de l'espace de travail exporté, y compris le dialogue.
        - Sélectionnez **Intents and Entities** si vous souhaitez utiliser les intentions et les entités de l'espace de travail exporté, mais que vous prévoyez de créer un nouveau dialogue.

1.  Spécifiez les détails du nouvel espace de travail :
    - **Name** : nom qui ne doit pas excéder 64 caractères. Cette valeur est obligatoire.
    - **Description** : description qui ne doit pas excéder 128 caractères.
    - **Language** : langue dans laquelle les entrées utilisateur seront saisies et que l'espace de travail sera formé à comprendre.

Une fois l'espace de travail créé, il apparaît sous la forme d'une vignette sur la page Workspaces.

## Exportation et copie d'espaces de travail
{: #exporting-and-copying-workspaces}

Vous pouvez utiliser la page Workspaces pour exporter des espaces de travail et pour copier un espace de travail dans un nouvel espace de travail.

Lorsqu'un espace de travail est exporté, une copie de toutes ses données est sauvegardée dans un fichier JSON. Utilisez cette option si vous voulez importer l'espace de travail dans une autre instance de service ou sauvegarder une copie des données de l'espace de travail hors ligne. Lorsque vous importez un espace de travail, vous pouvez choisir d'importer uniquement les intentions et les entités, ce qui peut être utile si vous souhaitez créer un nouveau dialogue en utilisant les mêmes données d'apprentissage.

Lorsqu'un espace de travail est copié, une copie complète de celui-ci est créée dans la même instance de service. Utilisez cette option si vous voulez adapter un espace de travail existant tout en conservant la version originale.

- Pour exporter un espace de travail existant dans un fichier JSON, cliquez sur l'icône de menu dans la vignette représentant l'espace de travail, puis sélectionnez **Download as JSON**. 
- Pour créer une copie d'un espace de travail existant dans la même instance de service, cliquez sur l'icône de menu dans la vignette représentant l'espace de travail, puis sélectionnez **Duplicate**. 

    Spécifiez un nom, une description et une langue pour le nouvel espace de travail. Toutes les données (y compris les intentions, les entités et le dialogue) sont incluses dans la copie.

## Partage de l'espace de travail avec des membres d'équipe
{: #invite-others}

Une fois l'instance de service créée, vous pouvez autoriser d'autres personnes à y accéder. Vous pouvez définir les données d'apprentissage et créer le dialogue en même temps. 

**Important** : une seule personne à la fois peut éditer une intention, une entité ou un noeud de dialogue. Si plusieurs personnes travaillent en même temps sur le même élément, les modifications apportées par la personne qui les sauvegarde en dernier sont les seules qui sont appliquées. Les modifications qui sont apportées au cours de la même période par quelqu'un d'autre et qui sont sauvegardées en premier ne sont pas conservées. Coordonnez les mises à jour que vous prévoyez d'effectuer avec les membres de votre équipe afin d'empêcher les personnes de perdre leur travail. 

Pour partager un espace de travail avec d'autres personnes, vous devez octroyer à ces dernières un accès développeur à l'instance de service qui héberge l'espace de travail. Si l'instance de travail que vous partagez héberge d'autres espaces de travail qu'aucune autre personne ne doit éditer, prenez soin de préciser cela lorsque vous invitez ces personnes. 

1.  Accédez à la page {{site.data.keyword.watson}} Developer Console [Projects ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://console.{DomainName}/developer/watson/projects) et connectez-vous. Cliquez sur **Manage > Account > Users** dans le menu. 
1.  Cliquez sur **Invite users**, puis entrez les adresses électroniques des personnes de votre équipe auxquelles vous souhaitez accorder un accès. 
1.  Dans la section *Cloud Foundry access*, choisissez votre organisation dans la liste *Organization*. 

    La zone *Organization roles* est automatiquement renseignée avec *Auditor*. Vous pouvez conserver la valeur par défaut dans la zone.
1.  Si vous le souhaitez, limitez l'accès aux espaces de travail dans une région ou dans un espace. 
1.  Dans la zone *Space roles*, choisissez **Developer**.

    Pour plus d'informations sur les rôles, reportez-vous à la rubrique [Cloud Foundry roles ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://console.bluemix.net/docs/iam/cfaccess.html#cfroles).
1.  Cliquez sur **Invite users**.

Lorsque les personnes que vous invitez ensuite se connecteront à {{site.data.keyword.cloud_notm}}, votre organisation sera incluse dans leur liste de comptes. 

Dans la mesure où davantage de personnes contribuent au développement des espaces de travail, des modifications inattendues peuvent se produire, notamment des suppressions d'espace de travail. Pensez à créer régulièrement des copies de sauvegarde de votre espace de travail de manière à pouvoir rétablir une version antérieure si besoin. Pour créer une copie de sauvegarde, il vous suffit d'exporter l'espace de travail en tant que fichier JSON. 
