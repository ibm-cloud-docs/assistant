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

# Aggiunta delle integrazioni
{: #deploy-integration-add}

Per distribuire la tua capacità, aggiungila a un assistente e poi aggiungi le integrazioni all'assistente che pubblica il tuo bot sui canali che vengono visitati dai tuoi clienti per assistenza.
{: shortdesc}

## Aggiungi un'integrazione
{: #deploy-integration-add-task}

Segui questi passi per aggiungere le integrazioni al tuo assistente: 

1.  Fai clic sulla scheda **Assistants**.

1.  Fai clic per aprire il tile per l'assistente che desideri distribuire. 

1.  Vai alla sezione Integrations. 

    **Cos'è l'integrazione Preview Link?** Quando crei un assistente, viene eseguito automaticamente il provisioning di un sito web di test (a meno che tu non lo abbia disabilitato). Ha un'interfaccia widget della chat semplice che puoi utilizzare per interagire con il tuo assistente a scopo di test. Puoi anche condividere l'URL a questo sito con marchio IBM con i membri del tuo team. 

1.  Fai clic su **Add Integration**.

1.  Fai clic sul tile del canale con cui desideri integrare l'assistente. Le opzioni includono:

    - [Applicazione personalizzata](/docs/services/assistant?topic=assistant-deploy-custom-app)
    - [Facebook Messenger](/docs/services/assistant?topic=assistant-deploy-facebook)
    - [Intercom](/docs/services/assistant?topic=assistant-deploy-intercom)  ![Plus or Premium plan only](images/premium.png)
    - [Preview Link](/docs/services/assistant?topic=assistant-deploy-web-link)
    - [Slack](/docs/services/assistant?topic=assistant-deploy-slack)
    - [Voice Agent (Telefonia)  ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://cloud.ibm.com/catalog/services/voice-agent-with-watson){: new_window}

      Apri la pagina della panoramica del servizio **Voice Agent with Watson** in {{site.data.keyword.cloud_notm}}.
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
| Lite             |                        100 |
{: caption="Dettagli piano di servizio" caption-side="top"}
