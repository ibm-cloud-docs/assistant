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

# Informazioni
{: #index}

{{site.data.keyword.conversationfull}} è un bot cognitivo che puoi personalizzare per le tue esigenze aziendali e distribuire attraverso più canali per fornire supporto ai tuoi clienti dove e quando ne hanno bisogno.
{: shortdesc}

## Modalità di funzionamento
{: #index-how-it-works}

Questo diagramma mostra l'architettura generale:

![Diagramma di flusso del servizio](images/arch-overview.png)

- Gli utenti interagiscono con l'assistente tramite uno di questi punti di **integrazione**:

  - Una chatbot che pubblichi direttamente in una piattaforma di messaggistica di social media esistente, ad esempio Slack o Facebook Messenger.
  - Un'interfaccia utente chatbot semplice ospitata da IBM Cloud.
  - Un'applicazione personalizzata che sviluppi tu, ad esempio un'applicazione mobile o un robot con un'interfaccia vocale.

- L'**assistente** riceve l'input utente e lo instrada alla capacità di dialogo. 

- La **capacità** di dialogo interpreta ulteriormente l'input utente, quindi indirizza il flusso della conversazione e raccoglie le informazioni necessarie per rispondere o eseguire una transazione al posto dell'utente. 

## Implementazione
{: #index-mplementation}

Di seguito viene descritto come implementerai il tuo assistente:

- **Crea una capacità di dialogo**. Utilizza lo strumento grafico intuitivo per definire i dati di addestramento e il dialogo per la conversazione tra l'assistente e i tuoi clienti. 

  I dati di addestramento sono costituiti dalle seguenti risorse:

  - **Intenti**: gli obiettivi che prevedi avranno i tuoi utenti quando interagiscono con il servizio. Definisci un intento per ogni obiettivo che può essere identificato nell'input utente. Ad esempio, potresti definire un intento denominato *store_hours* che risponde a domande relative agli orari del negozio. Per ogni intento, aggiungi delle espressioni di esempio che riflettono l'input che i clienti potrebbero utilizzare per chiedere le informazioni di cui hanno bisogno, ad esempio, `A che ora aprite?`

    Oppure utilizza **cataloghi di contenuto** precostruiti da IBM per iniziare a utilizzare i dati che fanno fronte agli obiettivi comuni del cliente. 

  - **Dialogo**: utilizza lo strumento di dialogo per creare un flusso di dialogo che incorpora i tuoi intenti. Il flusso di dialogo è rappresentato graficamente nello strumento come una struttura ad albero. Puoi aggiungere un ramo per elaborare ciascuno degli intenti che desideri venga gestito dal servizio.

  - **Entità**: un'entità rappresenta un termine o un oggetto che fornisce il contesto per un intento. Ad esempio, un'entità potrebbe essere un nome di città che aiuta il dialogo a distinguere per quale negozio l'utente desidera conoscere gli orari di apertura. Una volta aggiunte le entità, aggiorna il tuo dialogo per utilizzarle. Aggiungi i nodi di dialogo che gestiscono le numerose possibili permutazioni di una richiesta in base alle entità trovate nell'input utente. 

    Man mano che aggiungi dati di addestramento, un classificatore di linguaggio naturale viene automaticamente aggiunto alla capacità ed è addestrato a comprendere i tipi di richieste che l'utente deve ascoltare e a cui deve fornire una risposta. 

- **Crea un assistente**.

- **Aggiungi la capacità di dialogo al tuo assistente.**

- **Integra il tuo assistente.** Crea un'integrazione del canale in cui distribuire l'assistente configurato direttamente nel canale social media o di messaggistica. 

  Il tuo assistente distribuito viene ospitato da {{site.data.keyword.cloud_notm}}, la piattaforma di elaborazione di IBM Cloud. (Per ulteriori informazioni, vedi [Panoramica sulla piattaforma ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://cloud.ibm.com/docs/overview/ibm-cloud#overview).)

Approfondisci questi passi di implementazione seguendo questi link:

- [Panoramica sulla creazione degli intenti](/docs/services/assistant?topic=assistant-intents#intents-described)
- [Panoramica del dialogo](/docs/services/assistant?topic=assistant-dialog-overview)
- [Panoramica sulla creazione dell'entità](/docs/services/assistant?topic=assistant-entities#entities-described)
- [Panoramica sull'assistente](/docs/services/assistant?topic=assistant-assistant-add)
- [Aggiunta delle integrazioni](/docs/services/assistant?topic=assistant-deploy-integration-add)

## Dove sono i miei spazi di lavoro?
{: #index-existing-customers}

Se hai creato uno *spazio di lavoro* in una versione precedente del servizio, tranquillo, puoi ancora raggiungerlo. Gli spazi di lavoro ora vengono chiamati *capacità*. Per raggiungere il tuo spazio di lavoro, fai clic sulla scheda **Skills**.

## Supporto browser
{: #index-browser-support}

Lo strumento del servizio {{site.data.keyword.conversationshort}} richiede lo stesso livello di software browser richiesto da {{site.data.keyword.Bluemix_notm}}. Per dettagli, vedi l'argomento {{site.data.keyword.Bluemix_notm}} [Prerequisiti ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://cloud.ibm.com/docs/overview/prereqs#browsers){: new_window}.

## Supporto linguistico
{: #index-lang-support}

Il supporto linguistico per funzione è descritto in dettaglio nell'argomento [Lingue supportate](/docs/services/assistant?topic=assistant-language-support).

## Termini e informazioni particolari
{: #index-notices}

Vedi [Termini e informazioni particolari IBM Cloud ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](/docs/overview/terms-of-use?topic=overview-terms) per informazioni sui termini del servizio.

## Passi successivi
{: #index-next-steps}

- [Introduzione](/docs/services/assistant?topic=assistant-getting-started) al servizio.
- Esamina l'elenco delle [risorse per gli sviluppatori ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://www.ibm.com/watson/developer-resources/){: new_window}.

Hai ancora altre domande? Contatta [IBM Sales ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://www-01.ibm.com/marketing/iwm/dre/signup?source=urx-20970){: new_window}.
