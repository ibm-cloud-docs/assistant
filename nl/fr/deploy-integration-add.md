---

copyright:
  years: 2015, 2019
lastupdated: "2019-07-19"

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

# Ajout d'intégrations
{: #deploy-integration-add}

Pour déployer votre compétence, ajoutez-la à un assistant, puis ajoutez des intégrations à l'assistant qui publie votre bot sur les canaux où vos clients demandent de l'aide.
{: shortdesc}

## Fonctionnement des intégrations de plateforme de centre de services
{: #deploy-integration-service-desk-integrations}

Visionnez cette vidéo de 3 minutes pour en savoir plus sur l'intégration de votre assistant à une plateforme de centre de services, telle qu'Intercom. 

<iframe class="embed-responsive-item" id="youtubeplayer" title="Présentation du fonctionnement des intégrations de centre de services" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/pJSCZLQVgCY?rel=0" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

Pour en savoir plus sur les avantages pour votre activité des intégrations de centre de services à votre assistant, [lisez cet article ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://medium.com/ibm-watson/contact-center-post-394dff427c8){: new_window}.

## Ajout d'une intégration
{: #deploy-integration-add-task}

Procédez comme suit pour ajouter des intégrations à votre assistant :

1.  Cliquez sur l'onglet **Assistants**.

    Si vous ne voyez pas l'onglet **Assistants**, cliquez sur le lien du menu de navigation dans l'en-tête de page.
    {: note}

1.  Cliquez pour ouvrir la vignette de l'assistant que vous souhaitez déployer.

1.  Accédez à la section Integrations.

    **Qu'est-ce que l'intégration Preview Link ? ** Lorsque vous créez un assistant, un site Web de test est automatiquement mis à disposition (sauf si vous choisissez de ne pas activer le lien d'aperçu). Il possède une interface simple de widget de discussion que vous pouvez utiliser pour interagir avec votre assistant à des fins de test. Vous pouvez également partager l'URL de ce site IBM avec vos membres d'équipe.

1.  Cliquez sur **Add integration**.

1.  Cliquez sur la vignette du canal avec laquelle vous souhaitez intégrer l'assistant. Ces options comprennent :

    - [Application personnalisée](/docs/services/assistant?topic=assistant-deploy-custom-app)
    - [Facebook Messenger](/docs/services/assistant?topic=assistant-deploy-facebook)
    - [Intercom](/docs/services/assistant?topic=assistant-deploy-intercom)  ![Forfait Plus ou Premium uniquement](images/plus.png)
    - [Preview Link](/docs/services/assistant?topic=assistant-deploy-web-link)
    - [Slack
](/docs/services/assistant?topic=assistant-deploy-slack)
    - [Voice Agent (Téléphonie)  ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://cloud.ibm.com/catalog/services/voice-agent-with-watson){: new_window}

      Ouvre la page de présentation du service **Voice Agent with Watson** sur {{site.data.keyword.cloud}}.
    - [Plug-in WordPress ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://wordpress.org/plugins/conversation-watson/){: new_window}

      Ouvre la page de présentation du plug-in IBM Watson Assistant sur le site WordPress.

1.  Suivez les instructions fournies à l'écran pour effectuer le processus d'intégration.

Après avoir intégré l'assistant, testez-le à partir du canal cible pour vous assurer qu'il fonctionne comme prévu.

Pour des conseils sur la manière de démarrer le dialogue de manière cohérente pour tous les types d'intégration, voir [Démarrage du dialogue](/docs/services/assistant?topic=assistant-dialog-start).

## Limites d'intégration
{: #deploy-integration-add-limits}

Le nombre d'intégrations que vous pouvez créer dans une instance de service dépend de votre forfait {{site.data.keyword.conversationshort}}.

| Forfait de service     | Intégrations par assistant |
|------------------|---------------------------:|
| Premium          |                        100 |
| Plus             |                        100 |
| Standard         |                        100 |
| Plus Trial       |                        100 |
| Lite             |                        100 |
{: caption="Détails de forfait de service" caption-side="top"}
