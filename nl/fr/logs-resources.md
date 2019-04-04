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

# Tâches avancées
{: #logs-resources}

Découvrez les API et les autres outils que vous pouvez utiliser pour accéder aux données de journal et les analyser.
{: shortdesc}

## API
{: #logs-resources-api}

Vous pouvez utiliser l'API `/logs` pour répertorier les événements à partir des transcriptions de conversations ayant eu lieu entre vos utilisateurs et votre assistant. Pour obtenir une documentation détaillée sur la référence de l'API, reportez-vous à la rubrique [Liste des événements du journal ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://cloud.ibm.com/apidocs/assistant#list-log-events-in-a-workspace).

Le nombre de jours de stockage des journaux varie selon le type de forfait de service. Pour plus d'informations, reportez-vous à la rubrique [Limites des journaux](/docs/services/assistant?topic=assistant-logs#logs-limits).

Pour un script Python que vous pouvez exécuter pour exporter les journaux et les convertir au format CSV, téléchargez le fichier `export_logs.py` à partir du référentiel [Watson Assistant GitHub ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/watson-developer-cloud/community/blob/master/watson-assistant/export_logs.py).

## Terminologie relative aux journaux 
{: #logs-resources-terminology}

Commencez par examiner les définitions des termes associés aux journaux {{site.data.keyword.conversationshort}} :

- ***Assistant*** : application (parfois appelée "agent conversationnel") qui implémente votre contenu {{site.data.keyword.conversationshort}}.
- ***Conversation*** : ensemble de messages comprenant les messages qu'un utilisateur individuel envoie à votre assistant et les messages que votre assistant renvoie.
- ***Conversation ID*** : identifiant unique ajouté aux appels de message individuels pour lier les échanges de messages associés. Les développeurs d'applications utilisant la version V1 de l'API de service ajoutent cette valeur aux appels de message dans une conversation en incluant l'ID dans les métadonnées de l'objet contextuel. 
- ***Customer ID*** : ID unique pouvant être utilisé pour libeller les données client de manière à pouvoir ensuite les supprimer si le client demande la suppression de ses données.
- ***Deployment ID*** : libellé unique que les développeurs d'application utilisant la version V1 de l'API de service transmettent à chaque message d'utilisateur pour aider à identifier l'environnement de déploiement qui a généré le message.
- ***Instance*** : votre déploiement de {{site.data.keyword.conversationshort}}, accessible avec des données d'identification uniques. Une instance {{site.data.keyword.conversationshort}} peut contenir plusieurs assistants.
- ***Message*** : un message est un simple énoncé qu'un utilisateur envoie à l'assistant.
- ***Skill ID*** : identificateur unique d'une compétence..
- ***User*** : un utilisateur est une personne qui interagit avec votre assistant ; il s'agit souvent de vos clients. 
- ***User ID*** : libellé unique utilisé pour suivre le niveau d'utilisation du service d'un utilisateur spécifique.
- ***Workspace ID*** : identificateur unique d'un espace de travail. Bien que tous les espaces de travail que vous avez créés avant le 9 novembre apparaissent en tant que compétences dans l'outil, une compétence et un espace de travail ne sont pas la même chose. Une compétence est en réalité un encapsuleur pour un espace de travail V1. 

**Important** : la propriété **User ID** *n'est pas* équivalente à la propriété **Customer ID**, bien que toutes deux puissent être transmises au service. La zone **User ID** permet de suivre les niveaux d'utilisation à des fins de facturation, tandis que la zone **Customer ID** prend en charge l'étiquetage et la suppression ultérieure des messages associés aux utilisateurs finaux. L'ID client est utilisé de manière cohérente dans tous les services Watson et est spécifié dans l'en-tête `X-Watson-Metadata`. L'ID utilisateur est utilisé exclusivement par le service {{site.data.keyword.conversationshort}} et est transmis dans l'objet contextuel de chaque appel d'API /message.

## Activation des métriques utilisateur 
{: #logs-resources-user-id}

Les métriques utilisateur vous permettent de voir, par exemple, le nombre d'utilisateurs uniques ayant dialogué avec votre assistant, ou le nombre moyen de conversations par utilisateur sur un intervalle de temps donné dans la [page Overview](/docs/services/assistant?topic=assistant-logs-overview). Les métriques utilisateur sont activées à l’aide d’un paramètre `User ID` unique.

Pour spécifier le `User ID` pour un message envoyé à l'aide de l'API `/message`, incluez la propriété `user_id` au sein de l'objet métadonnées dans votre [contexte ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://cloud.ibm.com/apidocs/assistant?curl=#get-response-to-user-input){: new_window}, comme dans l'exemple suivant :

```
"context" : {
  "metadata" : {
       "user_id": "{UserID}"
  }
}
```
{: codeblock}

## Association de données de message à un utilisateur pour suppression 
{: #logs-resources-customer_id}

Il se peut que vous souhaitiez supprimer complètement un ensemble de données de votre utilisateur d'une instance {{site.data.keyword.conversationshort}}. Lorsque la fonctionnalité de suppression est utilisée, les métriques Overview ne reflètent plus les messages supprimés, par exemple, elles compteront moins de conversations au total. 

### Avant de commencer
{: #logs-resources-delete-customer-id-prereqs}

Pour supprimer des messages pour un ou plusieurs individus, vous devez d'abord associer un message à un **Customer ID** unique pour chaque individu. Pour spécifier **Customer ID** pour tout message envoyé à l'aide de l'API `/message`, incluez la propriété `X-Watson-Metadata: customer_id` dans votre en-tête. Vous pouvez transmettre plusieurs entrées **Customer ID** avec des paires `field=value` séparées par des points-virgules, à l'aide de `customer_id`, comme dans l'exemple suivant :

```
curl -X POST -u "apikey:3Df... ...Y7Pc9"
 --header
   'Content-Type: application/json'
   'X-Watson-Metadata: customer_id={first-customer-ID};customer_id={second-customer-ID}'
 --data '{"input":{"text":"hello"}}' 'https:// gateway-us-south.watsonplatform.net/assistant/api/v1/workspaces/{workspaceID}/message?version=2018-09-20'
```
{: codeblock}

La chaîne `customer_id` ne peut pas inclure le point-virgule (`;`) ni le signe égal (`=`). Vous devez vous assurer que chaque paramètre `Customer ID` est unique parmi vos clients.
{: note}

Pour supprimer les messages utilisant les valeurs `customer_id`, reportez-vous à la rubrique [Sécurité des informations](/docs/services/assistant?topic=assistant-information-security#information-security-gdpr-wa).

## Blocs-notes Jupyter
{: #logs-resources-jupyter-notebooks}

IBM a créé les blocs-notes Jupyter que vous pouvez utiliser pour analyser vos données de journal plus en détail. Un bloc-notes Jupyter est un environnement basé sur le Web pour un calcul interactif. Vous pouvez exécuter de petits fragments de code qui traitent vos données et voir immédiatement les résultats de vos calculs.

Vous pouvez utiliser un ensemble de blocs-notes avec les outils Python standard et un ensemble conçu pour une utilisation optimale avec {{site.data.keyword.DSX_full}}. {{site.data.keyword.DSX_short}} est un produit qui fournit un environnement dans lequel vous pouvez choisir les outils dont vous avez besoin pour analyser et visualiser les données, pour les nettoyer et les mettre en forme, pour intégrer des données en continu ou pour créer, former et déployer des modèles d’apprentissage automatique. Pour plus d'informations, reportez-vous à la [documentation du produit ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://dataplatform.cloud.ibm.com/docs/content/getting-started/welcome-main.html){:new_window}.

Les blocs-notes suivants sont disponibles :  

- **Measure** : rassemble des métriques axées sur la couverture (fréquence à laquelle l'assistant est suffisamment en confiance pour répondre aux utilisateurs) et sur l'efficacité (lorsque l'assistant répond, si les réponses satisfont les besoins des utilisateurs).

- **Effectiveness** : analyse en profondeur vos journaux pour vous aider à comprendre les étapes à suivre pour améliorer votre assistant.

[Watson Assistant Continuous Improvement Best Practices Guide ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://www.ibm.com/common/ssi/cgi-bin/ssialias?htmlfid=54022554USEN&) décrit comment tirer le meilleur parti possible des blocs-notes.

### Utilisation des blocs-notes avec {{site.data.keyword.DSX}}
{: #logs-resources-notebooks-studio}

Si vous choisissez d'utiliser les blocs-notes conçus pour fonctionner avec {{site.data.keyword.DSX}}, les étapes sont les suivantes :

1.  Créez un compte {{site.data.keyword.DSX}}, [créez un projet ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://dataplatform.cloud.ibm.com/docs/content/getting-started/projects.html?context=analytics){:new_window}, et ajoutez-y un compte Cloud Object Storage.
1.  Dans la communauté {{site.data.keyword.DSX}}, obtenez le [bloc-notes Measure Watson Assistant Performance ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")]( https://dataplatform.cloud.ibm.com/exchange/public/entry/view/133dfc4cd1480bbe4eaa78d3f635e568).
1   Suivez les instructions pas à pas fournies avec le bloc-notes pour analyser un sous-ensemble des échanges de dialogue à partir des journaux.

    Les informations sont visualisées de manière à faciliter la compréhension de la couverture et de l'efficacité de l'assistant.
1.  Exportez un échantillon de journaux sous-jacents aux visualisations à partir de conversations inefficaces, puis analysez-le et annotez-le.

     Par exemple, indiquez si une réponse est correcte. Si tel est le cas, indiquez si elle est utile. Si une réponse est incorrecte, identifiez la cause première, par exemple, une intention ou une entité incorrecte a été détectée ou le noeud de dialogue incorrect a été déclenché. Après avoir identifié la cause première, indiquez quel aurait été le bon choix.
1.  Chargez la feuille de calcul annotée dans le [bloc-notes Analyze Watson Assistant Effectiveness](https://dataplatform.cloud.ibm.com/exchange/public/entry/view/133dfc4cd1480bbe4eaa78d3f636921c).

Ce processus vous aide à comprendre les étapes à suivre pour améliorer votre assistant. Il n'y a aucun moyen d'appliquer automatiquement ce que vous avez appris à votre instance de service. Conservez une trace de toutes les modifications que vous avez apportées pour améliorer le système, de sorte que vous puissiez ensuite les appliquer directement aux données d'apprentissage de votre compétence de dialogue. 

### Utilisation des blocs-notes avec les outils Python standard 
{: #logs-resources-notebooks-python}

Si vous choisissez d'utiliser les outils Python standard pour exécuter les blocs-notes, vous pouvez obtenir ces blocs-notes à partir du [référentiel IBM GitHub](https://github.com/watson-developer-cloud/assistant-improve-recommendations-notebook/tree/master/notebook). Veillez à les exécuter dans l'ordre suivant : 

1.  Measure Notebook.ipynb
1.  Effectiveness Notebook.ipynb
