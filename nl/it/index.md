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

# Informazioni
{: #index}

Utilizza {{site.data.keyword.conversationfull}} per creare il tuo assistente personalizzato in qualsiasi dispositivo, applicazione o canale. Il tuo assistente si connette alle risorse di coinvolgimento del cliente che utilizzi già per fornire un'esperienza di risoluzione dei problemi unificata e coinvolgente per i tuoi clienti.
{: shortdesc}

## Modalità di funzionamento
{: #index-how-it-works}

Questo diagramma illustra come funziona il prodotto:

![Diagramma di flusso del servizio](images/simple-overview.png)

- Gli utenti interagiscono con l'assistente tramite uno di questi punti di **integrazione**:

  - Un assistente virtuale che pubblichi direttamente in una piattaforma di messaggistica di social media esistente, ad esempio Slack o Facebook Messenger. 
  - Un'applicazione personalizzata che sviluppi tu, ad esempio un'applicazione mobile o un robot con un'interfaccia vocale. 

- L'**assistente** riceve l'input utente e lo instrada alla capacità di dialogo.

- La **capacità di dialogo** interpreta ulteriormente l'input utente, quindi indirizza il flusso della conversazione. Il dialogo raccoglie le informazioni necessarie per rispondere o eseguire una transazione al posto dell'utente. 

- Le domande a cui la capacità di dialogo non può rispondere vengono inviate alla **capacità di ricerca**, che trova risposte pertinenti eseguendo una ricerca nelle knowledge base aziendali che configuri a tale scopo. 

## Implementazione
{: #index-implementation}

Questo diagramma mostra l'implementazione in modo più dettagliato:

![Diagramma di flusso del servizio](images/arch-overview-search.png)

Di seguito viene descritto come implementi il tuo assistente:

- **Crea un assistente**.

- **Aggiungi una capacità al tuo assistente**.

  A seconda del tuo piano di servizio, puoi aggiungere i seguenti tipi di capacità: 

  - **Aggiungi una capacità di dialogo**.  
  
    Utilizza il prodotto grafico intuitivo per definire i dati di addestramento e il dialogo per la conversazione tra l'assistente e i tuoi clienti. I dati di addestramento sono costituiti dalle seguenti risorse:

    - **Intenti**: gli obiettivi che prevedi avranno i tuoi utenti quando interagiscono con il tuo assistente. Definisci un intento per ogni obiettivo che può essere identificato nell'input utente. Ad esempio, potresti definire un intento denominato *store_hours* che risponde a domande relative agli orari del negozio. Per ogni intento, aggiungi delle espressioni di esempio che riflettono l'input che i clienti potrebbero utilizzare per chiedere le informazioni di cui hanno bisogno, ad esempio, `A che ora aprite?`

      Oppure utilizza **cataloghi di contenuto** precostruiti forniti da IBM per iniziare a utilizzare i dati che fanno fronte agli obiettivi comuni del cliente. 

    - **Dialogo**: utilizza l'editor di dialogo per creare un flusso di dialogo che incorpora i tuoi intenti. Il flusso di dialogo viene rappresentato graficamente come una struttura ad albero. Puoi aggiungere un ramo per elaborare ciascuno degli intenti che desideri venga gestito dal tuo assistente. 

    - **Entità**: un'entità rappresenta un termine o un oggetto che fornisce il contesto per un intento. Ad esempio, un'entità potrebbe essere un nome di città che aiuta il dialogo a distinguere per quale negozio l'utente desidera conoscere gli orari di apertura. Una volta aggiunte le entità, aggiorna il tuo dialogo per utilizzarle. Aggiungi i nodi di dialogo che gestiscono le numerose possibili permutazioni di una richiesta in base alle entità trovate nell'input utente. 

    Man mano che aggiungi dati di addestramento, un classificatore di linguaggio naturale viene aggiunto automaticamente alla capacità. Il modello di classificatore viene addestrato a comprendere i tipi di richieste assistente per le quali hai insegnato al tuo assistente ad ascoltare e rispondere. 

  - **Aggiungi una capacità di ricerca**. ![Solo piano Plus o Premium](images/plus.png)

    Usufruisci delle raccolte di dati che crei in {{site.data.keyword.discoveryfull}} per fornire risposte alle domande del cliente. Quando un cliente pone una domanda a cui il dialogo non è progettato per rispondere, il tuo assistente può ricercare informazioni pertinenti dalle origini dati configurate, estrarre le informazioni e restituirle come risposta dell'assistente. 

- **Integra il tuo assistente.** Aggiungi un'integrazione del canale integrata in cui distribuire l'assistente configurato direttamente nel canale social media o di messaggistica. Oppure crea la tua applicazione client come interfaccia utente per l'assistente. 

  Il tuo assistente distribuito viene ospitato da {{site.data.keyword.cloud}}, la piattaforma di elaborazione di IBM Cloud. (Per ulteriori informazioni, vedi [Panoramica sulla piattaforma](/docs/overview/ibm-cloud#overview){: external}.)

Approfondisci questi passi di implementazione seguendo questi link:

- [Panoramica sull'assistente](/docs/services/assistant?topic=assistant-assistants)
- [Panoramica sulla capacità di ricerca](/docs/services/assistant?topic=assistant-skill-add-search)
- [Panoramica sulla creazione degli intenti](/docs/services/assistant?topic=assistant-intents#intents-described)
- [Panoramica del dialogo](/docs/services/assistant?topic=assistant-dialog-overview)
- [Panoramica sulla creazione dell'entità](/docs/services/assistant?topic=assistant-entities#entities-described)
- [Aggiunta delle integrazioni](/docs/services/assistant?topic=assistant-deploy-integration-add)

## Supporto browser
{: #index-browser-support}

{{site.data.keyword.conversationshort}} richiede lo stesso livello di software browser richiesto da {{site.data.keyword.Bluemix_notm}}. Per ulteriori informazioni, vedi {{site.data.keyword.Bluemix_notm}} [Prerequisiti](/docs/overview?topic=overview-prereqs-platform#browsers-platform){: external}.

## Supporto linguistico
{: #index-lang-support}

Il supporto linguistico per funzione è descritto in dettaglio nell'argomento [Lingue supportate](/docs/services/assistant?topic=assistant-language-support).

## Termini e informazioni particolari
{: #index-notices}

Vedi [Termini e informazioni particolari IBM Cloud](/docs/overview/terms-of-use?topic=overview-terms){: external} per informazioni sui termini del servizio. 

Il supporto HIPAA (Health Insurance Portability and Accountability Act) degli Stati Uniti è disponibile per i piani Premium ospitati nell'ubicazione Washington DC creati il 1° aprile 2019 o successivamente. Per ulteriori informazioni, vedi [Abilitazione delle impostazioni supportate da UE e HIPAA](/docs/account?topic=account-eu-hipaa-supported#eu-hipaa-supported){: external}.

## Passi successivi
{: #index-next-steps}

- [Introduzione](/docs/services/assistant?topic=assistant-getting-started) al prodotto.
- Esamina l'elenco delle [risorse per gli sviluppatori](https://www.ibm.com/watson/developer-resources/){: external}.

Hai domande? Contatta [IBM Sales](https://www-01.ibm.com/marketing/iwm/dre/signup?source=urx-20970){: external}.
