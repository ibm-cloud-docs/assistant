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

# Accès aux espaces de travail
{: #edit-convo-workspace}

Si vous avez créé un espace de travail avec une version antérieure du service (y compris lorsqu'il s'appelait {{site.data.keyword.watson}} Conversation), vous pouvez continuer à utiliser cet espace de travail.
{: shortdesc}

## Modification de l'espace de travail de conversation 
{: #edit-convo-workspace-task}

Votre espace de travail est toujours disponible ; il s'agit simplement d'une *compétence* désormais. Pour modifier un espace de travail existant, procédez comme suit : 

1.  Dans la page d'accueil de {{site.data.keyword.conversationshort}}, cliquez sur **Skills**.

    Tous les espaces de travail que vous avez créés avec la version commercialisée du service {{site.data.keyword.conversationshort}} sont affichés sous forme de vignettes de compétences de dialogue.
1.  Cliquez sur la compétence de dialogue qui représente l'espace de travail que vous souhaitez modifier. 
1.  Apportez des modifications ou des ajouts aux données d'apprentissage ou au dialogue que vous souhaitez modifier.  
1.  Sauvegardez vos modifications.

Une fois l'apprentissage terminé, vos mises à jour sont disponibles pour l'application personnalisée à partir de laquelle vous appelez le service. Pour plus d'informations, reportez-vous à la rubrique [Présentation de l'API de Watson Assistant](/docs/services/assistant?topic=assistant-api-overview). 

## Limitations
{: #edit-convo-workspace-cons}

Avec la dernière version du service, vous pouvez continuer à faire tout ce que vous pouviez faire avec le service existant, mais avec plus de flexibilité. Vous avez peut-être créé un espace de travail avec une version antérieure du service et vous l'appelez à partir d'une application existante que vous ne souhaitez pas remplacer. Cette application gère déjà l'état et exécute des fonctions utiles lors de tours de dialogue individuels que vous souhaitez continuer à contrôler. Vous pouvez toujours faire le tri entre les appels effectués avec la dernière version de l'API /message. L'avantage est que vous n'avez pas à le faire. Dans la dernière version, vous pouvez prendre en charge plusieurs canaux d'intégration à la fois avec la même compétence de dialogue sous-jacente. 

Si vous choisissez de passer l'étape de création d'un assistant et d'y ajouter votre compétence de dialogue, vous passerez à côté de la simplicité que peuvent offrir les assistants. En particulier, vous **ne pouvez pas** utiliser une seule conversation pour interagir avec les clients par le biais de plusieurs canaux d'intégration à la fois et pour développer ou basculer rapidement vers de nouveaux canaux qui deviennent populaires parmi les utilisateurs. 

## Compétences et espaces de travail 
{: #edit-convo-workspace-names}

Ce que les outils présentent comme une compétence de dialogue est en réalité un encapsuleur pour un espace de travail V1. Bien qu’il n’existe actuellement aucune méthode d’API permettant de créer des compétences avec l’API V2, vous pouvez continuer à utiliser l’API V1 pour la création d’espaces de travail. 

- Lorsque vous ouvrez l'outil, tous les espaces de travail que vous avez créés avant le 9 novembre sont affichés en tant que compétences. Le nom de la compétence provient du nom de l'espace de travail. Toutefois, si vous modifiez ultérieurement le nom ou la description de la compétence, ces modifications n’affectent pas l’espace de travail. De même, si vous utilisez les API pour modifier le nom ou la description de l'espace de travail, ces modifications n'affectent pas la compétence. 
- Dans l'outil, les détails de l'API pour la compétence vous montrent à la fois l'ID de la compétence et l'ID de l'espace de travail. 
