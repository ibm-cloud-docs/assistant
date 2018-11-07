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

# A propos du composant Improve

Le composant Improve de {{site.data.keyword.conversationshort}} fournit un historique des interactions avec les utilisateurs de votre espace de travail. Vous pouvez utiliser cet historique pour améliorer la compréhension des entrées des utilisateurs par votre espace de travail.
{: shortdesc}

Sections du panneau **Improve** :

* [Overview](logs_oview.html) : récapitulatif des interactions des utilisateurs avec votre espace de travail. 
* [User conversations](logs_convo.html) : liste des énoncés d'utilisateur. Vous pouvez mettre à jour des intentions et des entités tout en visualisant un énoncé d'utilisateur individuel. **Remarque** : une conversation d'utilisateur unique peut être constituée de plusieurs énoncés. 
* [Recommendations](logs_recommend.html) : solutions pour améliorer votre système. Disponible uniquement pour les utilisateurs Premium.

## Amélioration des espaces de travail
{: #deploy_id}

Pour comprendre comment utiliser des données d'énoncé afin d'améliorer des espaces de travail, il est utile de passer en revue les définitions suivantes pour le service {{site.data.keyword.conversationshort}} :

* ***Instance*** : votre déploiement de {{site.data.keyword.conversationshort}}, accessible avec des données d'identification uniques. Une instance {{site.data.keyword.conversationshort}} peut être constituée de plusieurs espaces de travail. 
* ***Workspace*** : un espace de travail est un modèle de votre contenu {{site.data.keyword.conversationshort}} ; souvent, cela peut s'apparenter à un bot
* ***Workspace ID*** : identificateur unique d'un espace de travail
* ***Deployment ID*** : les ID de déploiement sont des libellés uniques qui sont transmis avec des énoncés d'utilisateur afin d'aider à identifier l'environnement de déploiement d'où proviennent les énoncés
* ***Utterance*** : un énoncé est un message unique qu'un utilisateur envoie à l'espace de travail

La création d'un espace de travail est un processus très itératif. Pendant que vous développez votre espace de travail, vous utilisez le panneau *Try it out* pour vérifier que votre workspace reconnaît les intentions et les entités appropriées dans les entrées de test et pour effectuer d'éventuelles corrections. 

Sur le panneau **Improve**, vous pouvez afficher des informations sur les interactions réelles avec vos utilisateurs et effectuer des corrections similaires pour améliorer l'exactitude avec laquelle votre espace de travail reconnaît les intentions et les entités. Il est difficile de savoir exactement *comment* vos utilisateurs poseront des questions ou quels énoncés aléatoires ils pourront produire, par conséquent, il est indispensable d'afficher fréquemment le panneau **Improve** afin d'améliorer vos espaces de travail. 

Pour une instance {{site.data.keyword.conversationshort}} incluant plusieurs espaces de travail, il peut parfois s'avérer utile d'utiliser des données d'énoncé provenant d'un espace de travail afin d'améliorer un autre espace de travail au sein de cette même instance. **Remarque** : si vous êtes un utilisateur {{site.data.keyword.conversationshort}} Premium, vos instances premium peuvent éventuellement être configurées pour autoriser l'accès aux données de journal à partir des espaces de travail qu'elles comportent. 

Par exemple, supposons que vous ayez une instance {{site.data.keyword.conversationshort}} nommée *HelpDesk*. Vous pouvez avoir un espace de travail Production et un espace de travail Development dans votre instance HelpDesk. Lorsque vous travaillez dans l'espace de travail Development, vous pouvez filtrer les énoncés en fonction de l'`ID de déploiement` pour l'espace de travail Production, de manière à utiliser des énoncés d'espace de travail Production pour améliorer votre espace de travail Development. 

![Lien Data source](images/data_source_1.png)

Toute les modifications que vous effectuez dans l'espace de travail Development affecteront uniquement ce dernier, même si vous utilisez des énoncés d'espace de travail Production. Pour plus d'informations, reportez-vous à la rubrique [Sélection d'une source de données](logs_convo.html#select-source). 

Pour spécifier l'ID de déploiement d'un énoncé envoyé à l'aide de l'API `/message`, incluez la propriété de déploiement au sein de l'objet métadonnées dans votre [contexte ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://www.ibm.com/watson/developercloud/conversation/api/v1/#send_message){: new_window}, comme dans l'exemple suivant :

```
"context" : {
  "username" : "jane_doe@myemail.com",
  "member_type" : "gold",
  "metadata" : {
       "deployment": "HelpDesk-Production"
  }
}
```
{: #codeblock}
