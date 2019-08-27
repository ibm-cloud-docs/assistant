---

copyright:
  years: 2015, 2019
lastupdated: "2019-07-16"

subcollection: assistant


---

{:curl: #curl .ph data-hd-programlang='curl'}
{:javascript: #javascript .ph data-hd-programlang='javascript'}
{:java: #java .ph data-hd-programlang='java'}
{:python: #python .ph data-hd-programlang='python'}
{:swift: data-hd-programlang='swift'}
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:codeblock: .codeblock}
{:download: .download}
{:deprecated: .deprecated}
{:important: .important}
{:note: .note}
{:tip: .tip}

# Présentation de l'API {{site.data.keyword.conversationshort}}
{: #api-overview}

Vous pouvez utiliser les API REST {{site.data.keyword.conversationshort}} et les SDK correspondants pour développer des applications qui interagissent avec le service.

## Applications client

Pour créer un assistant virtuel ou une autre application client qui communique avec un assistant lors de l'exécution, utilisez la nouvelle API v2. En utilisant cette API, vous pouvez développer un client orienté utilisateur pouvant être déployé à des fins de production, une application qui assure la communication entre un assistant et un autre service (tel qu'un service de discussion ou un système dorsal) ou une application de test. 

En utilisant l'API d'exécution v2 pour communiquer avec votre assistant, votre application peut tirer parti des fonctionnalités suivantes : 

- **Gestion automatique des états. ** L'API d'exécution v2 gère chaque session avec un utilisateur final, stockant et conservant toutes les données de contexte dont votre assistant a besoin pour une conversation complète. 

- **Facilité de déploiement à l'aide d'assistants.** En plus de prendre en charge des clients personnalisés, un assistant peut être facilement déployé sur les principaux canaux de messagerie tels que Slack et Facebook Messenger. 

- **Gestion des versions.** Avec la gestion des versions de compétences de dialogue, vous pouvez enregistrer un instantané de votre compétence et lier votre assistant à cette version spécifique. Vous pouvez ensuite continuer à mettre à jour votre version de développement sans affecter l'assistant de production.

- **Fonctions de recherche.** L'API d'exécution v2 peut être utilisée pour recevoir des réponses à la fois des compétences de dialogue et des compétences de recherche. Lorsqu'une requête est soumise à laquelle votre compétence de dialogue ne peut pas répondre, l'assistant peut utiliser une compétence de recherche pour trouver la meilleure réponse à partir des sources de données configurées. (Les compétences de recherche sont une fonctionnalité bêta uniquement disponible pour les utilisateurs du forfait Plus ou Premium.) 

Pour plus d'informations sur l'API v2, reportez-vous à la rubrique {{site.data.keyword.conversationshort}} [Référence d'API v2 ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://{DomainName}/apidocs/assistant-v2){: new_window}.

**Remarque** : l'API {{site.data.keyword.conversationshort}} v1 prend toujours en charge la méthode héritée `/message` qui envoie l'entrée utilisateur directement à l'espace de travail utilisé par une compétence de dialogue. L'API d'exécution v1 est principalement prise en charge à des fins de compatibilité descendante. Si vous utilisez la méthode `/message` v1, vous devez implémenter votre propre gestion d'état et vous ne pouvez pas tirer parti de la gestion des versions ou de toute autre fonctionnalité d'un assistant. 

## Applications de création 

L'API v1 fournit des méthodes permettant à une application de créer ou de modifier des compétences de dialogue, au lieu de créer une compétence sous forme graphique à l'aide de l'interface utilisateur de {{site.data.keyword.conversationshort}}. Une application de création utilise l'API pour créer et modifier des compétences, des intentions, des entités, des noeuds de dialogue et d'autres artefacts constituant une compétence de dialogue. Pour plus d'informations, reportez-vous à la rubrique [Référence d'API v1 ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://{DomainName}/apidocs/assistant){: new_window}.

  **Remarque :** les méthodes de création v1 créent et modifient des espaces de travail plutôt que des compétences. Un espace de travail est un conteneur pour les données de dialogue et d'apprentissage (telles que les intentions et les entités) au sein d'une compétence de dialogue. Si vous créez un espace de travail à l'aide de l'API, il apparaît comme une nouvelle compétence de dialogue dans l'interface utilisateur de {{site.data.keyword.conversationshort}}.

Pour obtenir la liste des méthodes d'API disponibles, reportez-vous à la rubrique [Récapitulatif des méthodes d'API](/docs/services/assistant?topic=assistant-api-methods).
