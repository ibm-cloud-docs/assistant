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

# Informazioni sui servizi IBM Cloud
{: #services-information}

L'assistente è un bot ospitato completo gestito da {{site.data.keyword.cloud_notm}}, ciò significa che non devi preoccuparti della configurazione o della manutenzione dell'infrastruttura per supportarlo.
{: shortdesc}

## Informazioni sul piano di servizio
{: #services-information-plans}

Esplora le {{site.data.keyword.conversationshort}} [opzioni del piano di servizio ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://www.ibm.com/cloud/watson-assistant/pricing/){: new_window}.

Prima di creare un'istanza del servizio, decidi come vuoi organizzare le risorse nel tuo account {{site.data.keyword.cloud_notm}}. Se non definisci il tuo gruppo di risorse, viene utilizzato il gruppo di risorse **predefinito** e *non puoi* modificarlo successivamente. Per ulteriori dettagli, vedi [Prassi ottimali per organizzare le risorse in un gruppo di risorse ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://cloud.ibm.com/docs/resources/bestpractice_rgs#bp_resourcegroups){: new_window}. Tutti gli utenti devono avere il ruolo di accesso alla piattaforma di Operatore. (I ruoli di accesso al servizio non vengono utilizzati da {{site.data.keyword.conversationshort}}.)

### Limiti del piano per tipo di risorsa 
{: #services-information-limits}

Le informazioni sui limiti della risorsa per piano sono disponibili dagli argomenti che descrivono come creare le risorse, per cui puoi fare riferimento ai limiti quando hai bisogno di conoscerli. Questi sono i link agli argomenti:

- [Assistenti](/docs/services/assistant?topic=assistant-assistant-add#assistant-add-limits)
- [Nodi del dialogo](/docs/services/assistant?topic=assistant-dialog-build#dialog-build-node-limits)
- [Entità](/docs/services/assistant?topic=assistant-entities#entities-limits)
- [Intenti](/docs/services/assistant?topic=assistant-intents#intents-limits)
- [Integrazioni](/docs/services/assistant?topic=assistant-deploy-integration-add#deploy-integration-add-limits)
- [Log](/docs/services/assistant?topic=assistant-logs#logs-limits)
- [Capacità](/docs/services/assistant?topic=assistant-skill-add#skill-add-limits)
- [Versioni](/docs/services/assistant?topic=assistant-versions#versions-limits)

### Limiti di chiamate API
{: #services-information-api-limits}

Il numero di chiamate API consentite per ogni istanza dipende dal tuo piano di servizio. Per i dettagli, vedi la descrizione del tuo piano.

Se hai un piano Lite e raggiungi il tuo limite di chiamate API, ma i log mostrano che hai fatto meno chiamate del previsto, ricorda che il piano Lite memorizza le informazioni di log solo per 7 giorni.

Se vuoi eseguire l'upgrade da un piano a un altro, vedi [Aggiornamento](/docs/services/assistant?topic=assistant-upgrade).

### Funzioni del piano Plus e Premium
{: #services-information-premium}

Le seguenti funzioni sono disponibili solo per gli utenti dei piani Premium. 

- [Disambiguazione](/docs/services/assistant?topic=assistant-dialog-runtime#dialog-runtime-disambiguation)
- [Risoluzione del conflitto degli intenti ](/docs/services/assistant?topic=assistant-intents#intents-resolve-conflicts)
- [Consigli sull'esempio utente dell'intento](/docs/services/assistant?topic=assistant-intent-recommendations)
- [Integrazione Intercom](/docs/services/assistant?topic=assistant-deploy-intercom)

### Piani basati sull'utente
{: #services-information-user-based-plans}

A differenza dei piani basati sull'API, che misurano l'utilizzo in base al numero di chiamate API effettuate durante un determinato intervallo di tempo, il nuovo piano Plus e il piano Premium aggiornato utilizzano la fatturazione basata sull'utente. Misurano l'utilizzo in base al numero di utenti univoci che hanno interagito con l'assistente durante un determinato intervallo di tempo.

Il servizio controlla le seguenti informazioni dalle richieste API in questo ordine per scopi di fatturazione:

  1.  **user_id**: una proprietà definita nell'API inviata nell'oggetto di contesto di una chiamata API /message. L'utilizzo di questa proprietà è il modo migliore per assicurarti di assegnare in modo accurato le chiamate API /message a utenti univoci. Per ulteriori informazioni sulla proprietà ID utente, vedi la documentazione della guida di riferimento API.
  
    - `context.global.system.user_id`: [API v2](https://cloud.ibm.com/apidocs/assistant-v2#send-user-input-to-assistant)
    - `context.metadata.user_id`: [API v1](https://cloud.ibm.com/apidocs/assistant#get-response-to-user-input)

  1.  **session_id**: una proprietà definita nell'API v2 che identifica una sola conversazione tra un utente e l'assistente. Viene fornito un ID sessione nelle chiamate API /message che viene generato dalle integrazioni predefinite. La sessione termina quando un utente chiude la finestra di chat o dopo che la chat rimane non attiva per 60 minuti.

  1.  **conversation_id**: una proprietà definita nell'API v1 che viene archiviata nell'oggetto di contesto di una chiamata API /message. Questa proprietà può essere utilizzata per identificare più chiamate API /message associate a un solo scambio colloquiale con un utente. Tuttavia, lo stesso ID viene utilizzato solo se lo conservi esplicitamente e lo passi con ogni richiesta effettuata come parte della stessa conversazione. Altrimenti, viene generato un nuovo ID per ogni chiamata API /message.

Per avvalerti al massimo dei nuovi piani di servizio basati sull'utente, progetta ogni applicazione personalizzata che utilizzi per distribuire il tuo assistente per acquisire un ID utente o sessione univoco e passare le informazioni al servizio.

## Autenticazione delle chiamate API 
{: #services-information-authenticate-api-calls}

Il meccanismo di autenticazione utilizzato dalla tua istanza del servizio influisce su come devi fornire le credenziali quando effettui una chiamata API.

1.  Ottieni le credenziali del servizio. 

    - Trova e fai clic sull'istanza del servizio nell'[Elenco di risorse {{site.data.keyword.Bluemix_notm}} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://cloud.ibm.com/resources){: new_window}.

    - Fai clic per aprire la tua istanza del servizio, fai clic su **Credenziali del servizio** e quindi su **Visualizza credenziali**.

      **Credenziali Cloud Foundry**

      ![Mostra la pagina delle credenziali del servizio per le istanze ospitate da Cloud Foundry.](images/cf-cred-ui.png)

      **Credenziali IAM**

      ![Mostra la pagina delle credenziali del servizio per le istanze ospitate da IAM.](images/iam-creds.png)

1.  Utilizza queste credenziali nella tua chiamata API.

    **Chiamata API Cloud Foundry**

    Fornisci le tue credenziali nome utente e password.

    ```curl
    curl -X GET \
    --user {username}:{password} \
    'https://gateway.watson.net/assistant/api/v1/workspaces?version=2018-09-20'
    ```
    {: codeblock}

     **Chiamata API IAM**

    - L'URL di base deve includere l'ubicazione. Utilizza la sintassi `gateway-<location>.watsonplatform.net` per specificare l'ubicazione in cui hai creato l'istanza del servizio. I codici ubicazione sono elencati nella tabella *Ubicazioni data center*.
    - Fornisci il tipo di token appropriato nell'intestazione. Puoi passare un token di connessione o una chiave API. 

      - I token supportano le richieste autenticate senza le credenziali del servizio incorporate in ogni chiamata. Il seguente esempio mostra l'utilizzo di un token di connessione.

        ```curl
        curl -X GET \
        'https://gateway-syd.watsonplatform.net/assistant/api/v1/workspaces?version=2018-09-20' \
        --header 'Authorization: Bearer eyJhbGciOiJIUz......sgrKIi8hdFs'
        ```
        {: codeblock}

      - Le chiavi API utilizzano l'autenticazione di base. Il seguente esempio mostra l'utilizzo di una chiave API.

        ```curl
        curl -X GET -u "apikey:3Df... ...Y7Pc9" \
        'https://gateway-us-east.watsonplatform.net/assistant/api/v1/workspaces?version=2018-09-20' \
        ```
        {: codeblock}

        Quando utilizzi uno degli SDK Watson, puoi passare la chiave API e lasciare che l'SDK gestisca il ciclo di vita dei token.
        {: note}

        Le risorse IAM non possono essere gestite con la CLI (Command Line Interface) Cloud Foundry. Ad esempio, i comandi CLI Cloud Foundry (che iniziano con `cf`) che creano o gestiscono le istanze del servizio non funzionano con le istanze ospitate in ubicazioni utilizzando IAM. Invece, devi utilizzare la CLI {{site.data.keyword.cloud_notm}} e i relativi comandi associati. Per ulteriori dettagli, vedi [Gestione di risorse e gruppi di risorse ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](/docs/cli/reference/ibmcloud?topic=cloud-cli-ibmcloud_commands_resource). 

        Per ulteriori informazioni, vedi [Autenticazione con i token IAM![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](/docs/services/watson?topic=watson-iam){: new_window}.

    Ad esempio, vedi [Autenticazione ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://{DomainName}/apidocs/assistant-v2#authentication){: new_window} per la tua lingua nella guida di riferimento API.

### Data center
{: #services-information-regions}

{{site.data.keyword.cloud_notm}} ha una rete di data center globali che fornisce benefici prestazionali sui propri servizi cloud. Per ulteriori dettagli, vedi [Data center globali {{site.data.keyword.cloud_notm}}![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://www.ibm.com/cloud/data-centers/){: new_window}.

{{site.data.keyword.cloud_notm}} la gestione dell'accesso utente con Cloud Foundry è stata modificata per utilizzare l'autenticazione Identity and Access Management (IAM) basata sul token. IAM è stato distribuito in diverse ubicazioni in momenti differenti. Puoi migrare un'istanza del servizio per spostarla dai suoi organizzazione e spazio Cloud Foundry correnti a un gruppo di risorse. Per ulteriori dettagli, vedi [Migrazione](/docs/services/assistant?topic=assistant-migrate).

Puoi creare delle istanze del servizio {{site.data.keyword.conversationshort}} che vengono ospitate nelle seguenti ubicazioni data center:

| Posizione    | Codice ubicazione | Tipo di autenticazione | Data di adesione a IAM  | Note |
|-------------|---------------|---------------------|-------------------|-------|
| Dallas      | us-south      | IAM                 | 30 ottobre 2018 | N/D |
| Francoforte   | eu-de         | IAM                 | 30 ottobre 2018 | N/D |
| Sydney      | au-syd        | IAM                 | 7 maggio 2018 | Le istanze create prima del 7 maggio sono state diffuse a Dallas. |
| Tokyo       | jp-tok        | IAM                 | 8 novembre 2018 | N/D |
| Londra      | eu-gb, lon    | IAM                 | 13 dicembre 2018 | Le istanze che sono state create nella regione Regno Unito prima del 13 dicembre sono state diffuse alla regione Stati Uniti Sud. |
| Washington DC  | us-east    | IAM                 | 14 giugno 2018 | N/D |
{: caption="Ubicazioni data center" caption-side="top"}

Per informazioni sui data center in cui vengono ospitati altri servizi {{site.data.keyword.cloud_notm}}, vedi [Servizi per regione ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://cloud.ibm.com/docs/resources/services_region#services_region){: new_window}.

## Termini e sicurezza
{: #services-information-terms}

Per ulteriori informazioni sui termini del servizio e sulla sicurezza dei dati, leggi le seguenti informazioni:

- [Service terms ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://www-03.ibm.com/software/sla/sladb.nsf/sla/home?OpenDocument){: new_window}
- [Data security and privacy ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://www.ibm.com/software/sla/sladb.nsf/sla/csdsp?OpenDocument){: new_window}
- [Informazioni sulla sicurezza](/docs/services/assistant?topic=assistant-information-security)

Vedi [Panoramica della piattaforma ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](/docs/overview?topic=overview-whatis-platform){: new_window} per ulteriori informazioni su {{site.data.keyword.cloud_notm}}.

## Hai ancora altre domande? 
{: #services-information-sales}

Contatta [IBM Sales ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://www-01.ibm.com/marketing/iwm/dre/signup?source=urx-20970){: new_window}.
