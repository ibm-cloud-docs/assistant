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

# A propos de
{: #index}

{{site.data.keyword.conversationfull}} est un bot cognitif que vous pouvez personnaliser en fonction des besoins de votre entreprise et que vous pouvez déployer sur plusieurs canaux pour apporter de l'aide à vos clients où et quand ils en ont besoin.
{: shortdesc}

## Fonctionnement
{: #index-how-it-works}

Ce diagramme représente l'ensemble de l'architecture : 

![Organigramme du service](images/arch-overview.png)

- Les utilisateurs interagissent avec l'assistant via un ou plusieurs des points **d'intégration** suivants :

  - Un chat bot (ou agent conversationnel) que vous publiez directement sur une plateforme de messagerie de médias sociaux existante, telle que Slack ou Facebook Messenger.
  - Une interface utilisateur de chat bot simple hébergée par IBM Cloud. 
  - Une application personnalisée que vous développez, telle qu'une application mobile ou un robot avec une interface vocale. 

- L'**assistant** reçoit l'entrée utilisateur et l'achemine vers la compétence de dialogue.

- La **compétence** de dialogue interprète ensuite l'entrée utilisateur, puis dirige le flux de la conversation et rassemble toutes les informations nécessaires pour répondre ou effectuer une transaction pour le compte de l'utilisateur. 

## Implémentation
{: #index-mplementation}

Voici comment vous allez implémenter votre assistant :

- **Créez une compétence de dialogue**. Utilisez l’outil graphique intuitif pour définir les données d'apprentissage et le dialogue de la conversation entre votre assistant et vos clients. 

  Les données d'apprentissage comprennent les artefacts suivants :

  - **Intentions** : objectifs que vous prévoyez pour vos utilisateurs lorsqu'ils interagiront avec le service. Définissez une intention pour chaque objectif qui peut être identifié dans une entrée utilisateur. Par exemple, vous pouvez définir une intention nommée *store_hours* qui répond aux questions relatives aux heures d'ouverture d'un magasin. Pour chaque intention, vous ajoutez des énoncés qui reflètent l'entrée que les clients sont susceptibles d'utiliser pour demander les informations dont ils ont besoin, par exemple, `A quelle heure ouvrez-vous ?`

    Vous pouvez également utiliser des **catalogues de contenu** prédéfinis, fournis par IBM pour vous familiariser avec les données qui répondent aux objectifs communs des clients.

  - **Dialogue** : utilisez l'outil de dialogue pour créer un flux de dialogue qui intègre vos intentions et vos entités. Le flux de dialogue est représenté graphiquement dans l'outil sous la forme d'une arborescence. Vous pouvez ajouter une branche pour traiter chacune des intentions que le service doit gérer.

  - **Entités** : une entité représente un terme ou un objet qui fournit le contexte d'une intention. Par exemple, une entité peut être un nom de ville qui permet à votre dialogue de distinguer le magasin pour lequel l'utilisateur souhaite obtenir les horaires d'ouverture. Après avoir ajouté des entités, mettez à jour votre dialogue pour les utiliser. Ajoutez des noeuds de dialogue qui gèrent les nombreuses permutations possibles d'une demande en fonction des entités trouvées dans l'entrée utilisateur 

    A mesure que vous ajoutez des données d'apprentissage, un classificateur de langage naturel est ajouté automatiquement à la compétence et s'entraîne à comprendre les types des demandes que vous avez indiquées et que le service doit écouter et auxquelles il doit répondre.

- **Créez un assistant**.

- **Ajoutez la compétence de dialogue à votre assistant.**

- **Intégrez votre assistant.** Créez une intégration de canal pour déployer l'assistant configuré directement sur un canal de réseau social ou de messagerie.  

  Votre assistant déployé est hébergé par {{site.data.keyword.cloud_notm}}, la plateforme IBM de cloud computing. (Pour plus d'informations, reportez-vous à [Présentation de la plateforme ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://cloud.ibm.com/docs/overview/ibm-cloud#overview).)

Pour en savoir plus sur ces étapes d'implémentation, cliquez sur les liens suivants :

- [Présentation de la création d’intention ](/docs/services/assistant?topic=assistant-intents#intents-described)
- [Présentation du dialogue](/docs/services/assistant?topic=assistant-dialog-overview)
- [Présentation de la création d’entité](/docs/services/assistant?topic=assistant-entities#entities-described)
- [Présentation de l'assistant](/docs/services/assistant?topic=assistant-assistant-add)
- [Ajout d'intégrations](/docs/services/assistant?topic=assistant-deploy-integration-add)

## Où sont mes espaces de travail ? 
{: #index-existing-customers}

Si vous avez créé un *espace de travail* dans une version précédente du service, ne craignez rien ; vous pouvez toujours y accéder. Les espaces de travail sont désormais appelés *compétences*. Pour accéder à votre espace de travail, cliquez sur l'onglet **Skills**.

## Prise en charge des navigateurs
{: #index-browser-support}

L'outil du service {{site.data.keyword.conversationshort}} requiert le même niveau de logiciel de navigateur que celui requis par {{site.data.keyword.Bluemix_notm}}. Pour plus d'informations, reportez-vous à la rubrique {{site.data.keyword.Bluemix_notm}} [Prerequisites ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://cloud.ibm.com/docs/overview/prereqs#browsers){: new_window}.

## Support de langue
{: #index-lang-support}

Le support de langue par fonction est décrit en détail dans la rubrique [Langues prises en charge](/docs/services/assistant?topic=assistant-language-support).

## Conditions et remarques 
{: #index-notices}

Pour plus d'informations sur les conditions de service, reportez-vous à [Conditions et remarques IBM Cloud ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](/docs/overview/terms-of-use?topic=overview-terms).

## Etapes suivantes
{: #index-next-steps}

- [Initiez-vous](/docs/services/assistant?topic=assistant-getting-started) au service.
- Consultez la liste de [ressources pour développeurs ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://www.ibm.com/watson/developer-resources/){: new_window}.

D'autres questions ? Contactez [IBM Sales ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://www-01.ibm.com/marketing/iwm/dre/signup?source=urx-20970){: new_window}.
