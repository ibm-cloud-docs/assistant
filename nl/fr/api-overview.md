---

copyright:
  years: 2015, 2019
lastupdated: "2019-02-21"

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
{:deprecated: .deprecated}
{:important: .important}
{:note: .note}
{:tip: .tip}

# Présentation de l'API {{site.data.keyword.conversationshort}}
{: #api-overview}

Vous pouvez utiliser les API REST {{site.data.keyword.conversationshort}} et les SDK correspondants pour développer des applications qui interagissent avec le service. Il existe deux versions de l'API {{site.data.keyword.conversationshort}}, chacune prenant en charge un ensemble de fonctions différent : v1 et v2. L’API à utiliser dépend du type de méthode requis par votre application : 

- **Méthodes d'exécution** : méthodes permettant à une application client d'interagir avec (mais non de modifier) un assistant ou une compétence existants. Vous pouvez utiliser ces méthodes pour développer un client orienté utilisateur pouvant être déployé à des fins de production, une application qui assure la communication entre un assistant et un autre service (tel qu'un service de discussion ou un système dorsal) ou une application de test.  

  L’API {{site.data.keyword.conversationshort}} v2 permet d’accéder aux méthodes que vous pouvez utiliser pour interagir avec un assistant au moment de l’exécution (par exemple `/message`). Il s'agit de l'API préférée à utiliser pour développer de nouvelles applications client. Pour plus d'informations sur l'API v2, reportez-vous à la rubrique {{site.data.keyword.conversationshort}} [Référence d'API v2 ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://{DomainName}/apidocs/assistant-v2){: new_window}.

  L'API v1 {{site.data.keyword.conversationshort}} inclut une méthode `/message` qui envoie directement l'entrée utilisateur à l'espace de travail utilisé par une compétence de dialogue, en contournant l'assistant. L'API d'exécution v1 est principalement prise en charge à des fins de compatibilité descendante. Si vous utilisez la méthode v1 `/message`, votre application ne peut pas tirer parti des fonctionnalités d'orchestration et de gestion des états d'un assistant. Pour plus d'informations, reportez-vous à la rubrique [ Utilisation de l'API d'exécution v1](/docs/services/assistant?topic=assistant-api-client#v1-api).

- **Méthodes de création** : méthodes permettant à une application de créer ou de modifier des compétences de dialogue, au lieu de créer une compétence de manière graphique à l'aide de l'outil {{site.data.keyword.conversationshort}}. Une application de création utilise diverses méthodes pour créer et modifier des compétences, des intentions, des entités, des noeuds de dialogue et d'autres artefacts constituant une compétence de dialogue. 

  Pour créer une application de création, utilisez l’API v1. Pour plus d'informations, reportez-vous à la rubrique [Référence d'API v1 ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://{DomainName}/apidocs/assistant){: new_window}.

  **Remarque :** les méthodes de création v1 interagissent avec des espaces de travail plutôt qu'avec des compétences. Un espace de travail est un conteneur pour les données de dialogue et d'apprentissage (telles que les intentions et les entités) au sein d'une compétence de dialogue. Dans la plupart des cas, lorsque vous utilisez l'API, vous pouvez penser que les espaces de travail et les compétences de dialogue sont interchangeables. Par exemple, si vous créez un espace de travail à l'aide de l'API, il apparaîtra comme une nouvelle compétence de dialogue dans l'outil {{site.data.keyword.conversationshort}}.

Pour obtenir la liste des méthodes d'API disponibles, reportez-vous à la rubrique [Récapitulatif des méthodes d'API](/docs/services/assistant?topic=assistant-api-methods).
