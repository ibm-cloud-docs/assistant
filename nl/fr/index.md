---

copyright:
  years: 2015, 2019
lastupdated: "2019-08-06"

subcollection: assistant

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:external: target="_blank" .external}
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

# A propos de
{: #index}

Utilisez {{site.data.keyword.conversationfull}} pour créer votre propre assistant de marque dans n’importe quel périphérique, application ou canal. Votre assistant se connecte aux ressources d'engagement client que vous utilisez déjà pour offrir à vos clients une expérience de résolution de problème interactive et unifiée.
{: shortdesc}

## Fonctionnement
{: #index-how-it-works}

Ce diagramme illustre le fonctionnement du produit : 

![Organigramme du service](images/simple-overview.png)

- Les utilisateurs interagissent avec l'assistant via un ou plusieurs des points **d'intégration** suivants :

  - Un assistant virtuel que vous publiez directement sur une plateforme de messagerie de médias sociaux existante, telle que Slack ou Facebook Messenger. 
  - Une application personnalisée que vous développez, telle qu'une application mobile ou un robot avec une interface vocale. 

- L'**assistant** reçoit l'entrée utilisateur et l'achemine vers la compétence de dialogue.

- La **compétence de dialogue** interprète davantage l'entrée utilisateur, puis dirige le flux de la conversation. Le dialogue regroupe toutes les informations nécessaires pour répondre ou effectuer une transaction pour le compte de l'utilisateur. 

- Toutes les questions auxquelles la compétence de dialogue ne peut pas répondre sont envoyées à la **compétence de recherche** qui trouve des réponses pertinentes en effectuant une recherche dans les bases de connaissances de la société que vous avez configurées à cette fin. 

## Implémentation
{: #index-implementation}

Ce diagramme présente la mise en œuvre plus en détail : 

![Organigramme du service](images/arch-overview-search.png)

Voici comment vous implémentez votre assistant : 

- **Créez un assistant**.

- **Ajoutez une compétence à votre assistant**.

  Selon votre forfait de service, vous pouvez ajouter les types de compétences suivants :

  - **Ajouter une compétence de dialogue**.  
  
    Utilisez le produit graphique intuitif pour définir les données d'apprentissage et le dialogue de la conversation entre votre assistant et vos clients. Les données d'apprentissage comprennent les artefacts suivants :

    - **Intentions** : objectifs que vous prévoyez pour vos utilisateurs lorsqu'ils interagissent avec l'assistant. Définissez une intention pour chaque objectif qui peut être identifié dans une entrée utilisateur. Par exemple, vous pouvez définir une intention nommée *store_hours* qui répond aux questions relatives aux heures d'ouverture d'un magasin. Pour chaque intention, vous ajoutez des énoncés qui reflètent l'entrée que les clients sont susceptibles d'utiliser pour demander les informations dont ils ont besoin, par exemple, `A quelle heure ouvrez-vous ?`

      Vous pouvez également utiliser des **catalogues de contenu** prédéfinis, fournis par IBM pour vous familiariser avec les données qui répondent aux objectifs communs des clients.

    - **Dialogue** : utilisez l'éditeur de dialogue pour créer un flux de dialogue qui intègre vos intentions et vos entités. Le flux de dialogue est représenté graphiquement sous la forme d'une arborescence. Vous pouvez ajouter une branche pour traiter chacune des intentions que l'assistant doit gérer.

    - **Entités** : une entité représente un terme ou un objet qui fournit le contexte d'une intention. Par exemple, une entité peut être un nom de ville qui permet à votre dialogue de distinguer le magasin pour lequel l'utilisateur souhaite obtenir les horaires d'ouverture. Après avoir ajouté des entités, mettez à jour votre dialogue pour les utiliser. Ajoutez des noeuds de dialogue qui gèrent les nombreuses permutations possibles d'une demande en fonction des entités trouvées dans l'entrée utilisateur 

    Lorsque vous ajoutez des données d'apprentissage, un classifieur de langage naturel est automatiquement ajouté à la compétence. Le modèle de classifieur est formé pour comprendre les types de demandes que vous formez votre assistant à écouter et à répondre. 

  - **Ajouter une compétence de recherche**. ![Forfait Plus ou Premium uniquement](images/plus.png)

    Tirez parti des collections de données que vous créez dans {{site.data.keyword.discoveryfull}} pour répondre aux questions des clients. Lorsqu'un client pose une question à laquelle le dialogue n'est pas conçu pour répondre, votre assistant peut rechercher des informations pertinentes dans les sources de données configurées, extraire les informations et les renvoyer en tant que réponse de l'assistant. 

- **Intégrez votre assistant.** Ajoutez une intégration de canal pour déployer l'assistant configuré directement sur un support social ou un canal de messagerie. Ou construisez votre propre application client en tant qu'interface utilisateur pour l'assistant. 

  Votre assistant déployé est hébergé par {{site.data.keyword.cloud}}, la plateforme IBM de cloud computing. (Pour plus d'informations, reportez-vous à la rubrique [Présentation de la plateforme](/docs/overview/ibm-cloud#overview){: external}.)

Pour en savoir plus sur ces étapes d'implémentation, cliquez sur les liens suivants :

- [Présentation de l'assistant](/docs/services/assistant?topic=assistant-assistants)
- [Présentation de la compétence de recherche](/docs/services/assistant?topic=assistant-skill-add-search)
- [Présentation de la création d’intention ](/docs/services/assistant?topic=assistant-intents#intents-described)
- [Présentation du dialogue](/docs/services/assistant?topic=assistant-dialog-overview)
- [Présentation de la création d’entité](/docs/services/assistant?topic=assistant-entities#entities-described)
- [Ajout d'intégrations](/docs/services/assistant?topic=assistant-deploy-integration-add)

## Prise en charge des navigateurs
{: #index-browser-support}

{{site.data.keyword.conversationshort}} requiert le même niveau de logiciel de navigateur que celui requis par {{site.data.keyword.Bluemix_notm}}. Pour plus d'informations, reportez-vous à la rubrique {{site.data.keyword.Bluemix_notm}} [Prérequis](/docs/overview?topic=overview-prereqs-platform#browsers-platform){: external}.

## Support de langue
{: #index-lang-support}

Le support de langue par fonction est décrit en détail dans la rubrique [Langues prises en charge](/docs/services/assistant?topic=assistant-language-support).

## Conditions et remarques
{: #index-notices}

Pour plus d'informations sur les conditions de service, reportez-vous à [Conditions et remarques IBM Cloud](/docs/overview/terms-of-use?topic=overview-terms){: external}.

La prise en charge de la loi américaine HIPAA (Health Insurance Portability and Accountability Act ) est disponible pour les forfaits Premium hébergés à Washington, DC, créés le ou après le 1er avril 2019. Pour plus d'informations, reportez-vous à la rubrique [Activation des paramètres pris en charge dans l'Union Européenne et pour la loi HIPAA](/docs/account?topic=account-eu-hipaa-supported#eu-hipaa-supported){: external}.

## Etapes suivantes
{: #index-next-steps}

- [Initiez-vous](/docs/services/assistant?topic=assistant-getting-started) au produit.
- Consultez la liste des [ressources pour développeurs](https://www.ibm.com/watson/developer-resources/){: external}.

Vous avez des questions ? Contactez [IBM Sales](https://www-01.ibm.com/marketing/iwm/dre/signup?source=urx-20970){: external}.
