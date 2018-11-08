---

copyright:
  years: 2015, 2018
lastupdated: "2018-02-13"

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

# Gestion des conversations
{: #logs_convo}

Pour ouvrir une liste d'interactions entre des utilisateurs et votre espace de travail, sélectionnez **User conversations** dans la barre de navigation. Si l'onglet **User conversations** ne s'affiche pas, utilisez le menu ![Menu](images/Menu_16.png) pour ouvrir la page.
{: shortdesc}

Lorsque vous ouvrez la page **User conversations**, la vue par défaut répertorie les résultats correspondant au dernier jour, les résultats les plus récents étant affichés en premier. L'intention supérieure (#intent) et toute valeur d'entité (@entity) reconnue utilisée dans un message, ainsi que le texte du message, sont disponibles. Pour les intentions qui ne sont pas reconnues, la valeur affichée est *Irrelevant*. Si une entité n'est pas reconnue ou n'a pas été indiquée, la valeur affichée est *No entities found*.
![Page de journaux affichée par défaut](images/logs_page1.png)

Il est important de noter que la page **User conversations** affiche le nombre total d'*énoncés* entre les utilisateurs et votre espace de travail. Un énoncé est un message unique que l'utilisateur envoie à l'espace de travail. Chaque conversation peut être constituée de plusieurs énoncés. Par conséquent, le nombre de résultats sur cette page **User conversations** est différent du nombre de conversations affiché sur la page [Overview](logs_oview.html).

## Nombre limite de journaux
{: #log-limits}

La durée de conservation des messages dépend de votre plan de service {{site.data.keyword.conversationshort}} :

  Plan de service                         | Conversation du message de discussion
  ------------------------------------ | ------------------------------------
  Premium                              | 90 derniers jours
  Standard                             | 30 derniers jours
  Lite                                 | 7 derniers jours

## Sélection d'une source de données
{: #select-source}

Par défaut, la page **User conversations** affiche des données d'énoncé pour l'espace de travail en cours. Toutefois, il peut s'avérer utile d'améliorer un espace de travail avec des énoncés qui ont été envoyés à d'autres espaces de travail au sein de votre instance. Par exemple, si vous possédez plusieurs versions d'espaces de travail de production et d'espaces de travail de développement, vous pouvez utiliser les mêmes données d'énoncé pour améliorer n'importe lequel de ces espaces de travail. 

Lors du passage à une autre source de données, le service {{site.data.keyword.conversationshort}} recherche les énoncés d'un élément appelé `Deployment ID`. Les ID de déploiement sont des identificateurs uniques dans l'API de service {{site.data.keyword.conversationshort}} que vous ajoutez à vos appels d'API de message. Pour savoir comment ajouter des ID de déploiement à des appels de message, reportez-vous à la rubrique [Amélioration des espaces de travail](logs.html#deploy_id).

Pour remplir la section Improve à l'aide d'énoncés associés à un ID de déploiement spécifique :

1.  Sélectionnez **Data source:**
    ![Lien Data source](images/data_source_1.png)
1.  Sélectionnez un déploiement
    ![Lien Data source](images/data_source_2.png)
1.  Cliquez sur **View Data**

La source de données sélectionnée apparaît. 

**Remarque :** si la section **Data source:** contient désormais la source des énoncés que vous utilisez pour améliorer cet espace de travail, l'espace de travail auquel vous appliquez les modifications apparaît toujours en haut de la page. 

Dans cet exemple, la page Improve est remplie avec des énoncés pour lesquels l'ID de déploiement `HelpDesk-Production` était inclus dans les appels d'API de message, mais si l'énoncé *test input* était ajouté à l'intention **#No** en cliquant sur **Save**, *test input* serait ajouté en tant qu'exemple de `#No` dans l'espace de travail `HelpDesk-Development`.
![Lien Data source](images/data_source_3.png)

## Filtrage des énoncés

Vous pouvez filtrer les énoncés à l'aide des critères suivants : *Search user statements*, *Intents*, *Entities* et *Last* n *days*:

*Search user statements* - tapez un mot dans la barre de recherche. Les entrées des utilisateurs sont recherchées, mais pas les réponses de l'espace de travail. 

*Intents* - sélectionnez le menu déroulant et tapez une intention dans la zone d'entrée ou choisissez une intention dans la liste proposée. Vous pouvez sélectionner plusieurs  intentions, ce qui permet de filtrer les résultats à l'aide de n'importe quelle intention sélectionnée, y compris *Irrelevant*.

![Menu déroulant Intents](images/intents_filter.png)

*Entities* - sélectionnez le menu déroulant et tapez un nom d'entité dans la zone d'entrée ou choisissez une entité dans la liste proposée. Vous pouvez sélectionner plusieurs entités, ce qui permet de filtrer les résultats à l'aide de n'importe quelle entité sélectionnée. Si vous effectuez un filtrage à l'aide de *intent and entity*, les messages qui comportent les deux valeurs seront inclus dans les résultats. Vous pouvez également effectuer un filtrage à l'aide de *No entities found*.

![Menu déroulant Entities](images/entities_filter.png)

La mise à jour des énoncés peut prendre un certain temps. Laissez passer au moins 30 minutes après une interaction entre l'utilisateur et votre espace de travail avant de tenter de filtrer l'affichage sur ce contenu. 

## Affichage d'un énoncé individuel
Vous pouvez développer chaque entrée d'énoncé pour voir ce que l'utilisateur a dit au cours de la totalité de la conversation et ce que votre espace de travail a répondu. Pour ce faire, sélectionnez **Open conversation**. Vous accédez automatiquement à l'énoncé que vous avez sélectionné au sein de cette conversation.

![Panneau de conversation ouverte](images/open_convo.png)

Vous pouvez ensuite choisir d'afficher la ou les classification(s) de l'énoncé que vous avez sélectionné.

![Panneau de conversion ouverte avec des classifications](images/open_convo_classes.png)

## Correction d'une intention

1.  Pour corriger une intention, sélectionnez l'icône d'édition ![Edit](images/edit_icon.png) en regard de l'entité #intent choisie.
1.  Dans la liste proposée, sélectionnez l'intention correcte pour cette entrée.
    - Commencez à taper dans la zone d'entrée pour filtrer la liste d'intentions.
    - Vous pouvez également choisir **Mark as irrelevant** dans ce menu. (Pour plus d'informations, reportez-vous à la rubrique sur l'option [Mark as irrelevant](intents.html#mark-irrelevant).) Ou, vous pouvez choisir **Do not train on intent**, et dans ce cas, cet énoncé n'est pas sauvegardé comme exemple pour l'entraînement.

    ![Sélection d'intention](images/select_intent.png)
1.  Sélectionnez **Save**.

    ![Sauvegarde d'intention](images/save_intent.png)

    **Remarque** : le service {{site.data.keyword.conversationshort}} prend en charge l'ajout *en l'état* d'une entrée utilisateur en tant qu'exemple à une intention. Si vous utilisez des références @entity en tant qu'exemples dans vos données d'apprentissage d'intention et qu'un énoncé d'utilisateur que vous souhaitez sauvegarder contient une valeur ou un synonyme d'entité provenant de vos données d'apprentissage, vous devrez éditer l'énoncé ultérieurement. Après avoir sauvegardé l'énoncé, éditez-le sur la page Intents afin de remplacer l'entité auquel il fait référence. Pour plus d'informations, reportez-vous à la rubrique [Référencement direct d'une entité @Entity en tant qu'exemple d'intention](intents.html#entity-as-example).

## Ajout d'une valeur d'entité ou d'un synonyme

1.  Pour ajouter une valeur d'entité ou un synonyme, sélectionnez l'icône d'édition ![Edit](images/edit_icon.png) en regard de l'entité @entity choisie.
1.  Sélectionnez **Add entity**.

    ![Ajout d'entité](images/add_entity.png)
1.  A présent, sélectionnez un mot ou une phrase dans l'entrée utilisateur soulignée.

    ![Sélection d'entité](images/select_entity.png)
1.  Choisissez une entité à laquelle la phrase mise en évidence sera ajoutée en tant que valeur.
    - Commencez à taper dans la zone d'entrée pour filtrer la liste d'entités et de valeurs.
    - Pour ajouter la phrase mise en évidence en tant que synonyme pour une valeur existante, choisissez `@entity:value` dans la liste déroulante.

    ![Ajout de mot d'entité](images/add_entity_word.png)
1.  Sélectionnez **Save**.

    ![Sauvegarde d'entité](images/add_entity_save.png)
