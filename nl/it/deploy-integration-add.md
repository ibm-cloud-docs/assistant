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

# Aggiunta delle integrazioni
{: #deploy-integration-add}

Per distribuire la tua capacità, aggiungila a un assistente e poi aggiungi le integrazioni all'assistente che pubblica il tuo bot sui canali che vengono visitati dai tuoi clienti per assistenza.
{: shortdesc}

## Come funzionano le integrazioni della piattaforma service desk
{: #deploy-integration-service-desk-integrations}

Questo video di 3 minuti di Watson contiene ulteriori informazioni sull'integrazione del tuo assistente con una piattaforma service desk, ad esempio Intercom.

<iframe class="embed-responsive-item" id="youtubeplayer" title="Panoramica su come funzionano le integrazioni del service desk" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/pJSCZLQVgCY?rel=0" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

Per informazioni su come le integrazioni del service desk con il tuo assistente possano andare a beneficio della tua azienda, [leggi questo post del blog ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://medium.com/ibm-watson/contact-center-post-394dff427c8){: new_window}.

## Aggiungi un'integrazione
{: #deploy-integration-add-task}

Segui questi passi per aggiungere le integrazioni al tuo assistente:

1.  Fai clic sulla scheda **Assistants**.

    Se non vedi la scheda **Assistants**, fai clic sul link del menu di navigazione nell'intestazione della pagina.
    {: note}

1.  Fai clic per aprire il tile per l'assistente che desideri distribuire.

1.  Vai alla sezione Integrations.

    **Cos'è l'integrazione Preview Link?** Quando crei un assistente, viene eseguito automaticamente il provisioning di un sito web di test (a meno che non scegli di non abilitare Preview Link). Ha un'interfaccia widget della chat semplice che puoi utilizzare per interagire con il tuo assistente a scopo di test. Puoi anche condividere l'URL a questo sito personalizzato IBM con i membri del tuo team.

1.  Fai clic su **Add integration**.

1.  Fai clic sul tile del canale con cui desideri integrare l'assistente. Le opzioni includono:

    - [Applicazione personalizzata](/docs/services/assistant?topic=assistant-deploy-custom-app)
    - [Facebook Messenger](/docs/services/assistant?topic=assistant-deploy-facebook)
    - [Intercom](/docs/services/assistant?topic=assistant-deploy-intercom)  ![Solo piano Plus o Premium](images/plus.png)
    - [Preview Link](/docs/services/assistant?topic=assistant-deploy-web-link)
    - [Slack](/docs/services/assistant?topic=assistant-deploy-slack)
    - [Voice Agent (Telefonia)  ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://cloud.ibm.com/catalog/services/voice-agent-with-watson){: new_window}

      Apri la pagina della panoramica del servizio **Voice Agent with Watson** in {{site.data.keyword.cloud}}.
    - [Plug-in WordPress ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://wordpress.org/plugins/conversation-watson/){: new_window}

      Apri la pagina della panoramica del plug-in IBM Watson Assistant sul sito WordPress.

1.  Segui le istruzioni fornite a video per completare il processo di integrazione.

Una volta integrato l'assistente, verificalo dal canale finale per assicurarti che l'assistente funzioni come previsto.

Per suggerimenti su come avviare il dialogo in modo congruente su tutti i tipi di integrazione, vedi [Avvio del dialogo](/docs/services/assistant?topic=assistant-dialog-start).

## Limiti delle integrazioni
{: #deploy-integration-add-limits}

Il numero di integrazioni che puoi creare in una singola istanza del servizio dipende dal tuo piano {{site.data.keyword.conversationshort}}.

| Piano di servizio     | Integrazioni per assistente |
|------------------|---------------------------:|
| Premium          |                        100 |
| Plus             |                        100 |
| Standard         |                        100 |
| Plus Trial       |                        100 |
| Lite             |                        100 |
{: caption="Dettagli piano di servizio" caption-side="top"}
