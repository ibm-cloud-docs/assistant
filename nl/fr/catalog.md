---

copyright:
  years: 2015, 2019
lastupdated: "2019-02-28"

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

# Utilisation de catalogues de contenu 
{: #catalog}

Les ***catalogues de contenu *** permettent d'ajouter facilement des intentions communes à votre compétence de dialogue dans {{site.data.keyword.conversationshort}}.
{: shortdesc}

Les intentions que vous ajoutez à partir du catalogue servent à fournir un point de départ. Ajoutez ou modifiez les intentions du catalogue pour les adapter à votre cas d'utilisation. 

## Ajout d'un catalogue de contenu à votre compétence de dialogue 
{: #catalog-add}

Utilisez l'outil {{site.data.keyword.conversationshort}} pour ajouter des catalogues de contenu.

1.  Dans l'outil {{site.data.keyword.conversationshort}}, ouvrez votre compétence de dialogue, puis cliquez sur l'onglet **Content Catalog**.

1.  Sélectionnez un catalogue de contenu, tel que *Banking*, pour voir les intentions qui y sont fournies.

    ![Capture d'écran illustrant les catalogues disponibles](images/catalog_overview.png)

    Vous verrez des informations sur les intentions incluses dans le catalogue. 

    ![Capture d'écran illustrant les intentions de la catégorie Banking](images/catalog_open.png)

    Les intentions ajoutées à partir d'un catalogue de contenu se distinguent des autres intentions par leur nom. Chaque nom d'intention est précédé du nom du catalogue de contenu. 

1.  Sélectionnez ![Flèche de fermeture](images/close_arrow.png) pour revenir à l'onglet **Content Catalog**. 

1.  Ajoutez ensuite un catalogue de contenu à votre compétence de dialogue en cliquant sur le bouton `Add to skill`.

1.  A présent, sélectionnez l'onglet **Intents** et vérifiez que les intentions du catalogue ont été ajoutées et sont disponibles.

    ![Capture d'écran illustrant les intentions Banking répertoriées dans l'onglet Intents](images/catalog_intents.png)

Le système commence à se former sur les nouvelles données.

Une fois que vous avez ajouté un catalogue à votre compétence, les intentions deviennent partie intégrante de vos données d'apprentissage. Si IBM effectue des mises à jour ultérieures dans un catalogue de contenu, les modifications ne sont pas automatiquement appliquées aux intentions que vous avez ajoutées à partir d'un catalogue.
{: note}

## Modification d'exemples de catalogue de contenu 
{: #catalog-edit-content}

Comme toute autre intention, après avoir ajouté des intentions de catalogue de contenu à votre compétence, vous pouvez y apporter les modifications suivantes : 

- Renommer l'intention
- Supprimer l'intention
- Ajouter, éditer, ou supprimer des exemples
- Déplacer un exemple vers une autre intention
