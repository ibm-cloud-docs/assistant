---

copyright:
  years: 2015, 2018
lastupdated: "2018-01-26"

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

# Informazioni

Con il servizio {{site.data.keyword.conversationfull}}, puoi creare una soluzione in grado di comprendere l'input in linguaggio naturale e utilizzare il machine learning per rispondere ai clienti in modo da simulare una conversazione tra gli esseri umani.
{: shortdesc}

## Modalità di funzionamento

Questo diagramma mostra l'architettura generale di una soluzione completa:![Diagramma del servizio](images/conversation_arch_overview.png)

- Gli utenti interagiscono con la tua applicazione attraverso l'**interfaccia** utente che implementi, ad esempio una semplice finestra di chat, un'applicazione mobile o un robot con un'interfaccia vocale.

- L'**applicazione** invia l'input utente al servizio {{site.data.keyword.conversationshort}}.
    - L'applicazione si connette a uno *spazio di lavoro*, che è un contenitore per il flusso di dialogo e i dati di addestramento.
    - Il servizio interpreta l'input dell'utente, dirige il flusso della conversazione e raccoglie le informazioni di cui ha bisogno.
    - Puoi connettere ulteriori servizi {{site.data.keyword.watson}} per analizzare l'input dell'utente, come {{site.data.keyword.toneanalyzershort}} o {{site.data.keyword.speechtotextshort}}.

- L'applicazione può interagire con i tuoi **sistemi di back-end** in base all'intento dell'utente e a ulteriori informazioni. Ad esempio, per rispondere a domande, aprire ticket, aggiornare le informazioni account o effettuare ordini. Non c'è limite a ciò che puoi fare.

## Implementazione

Di seguito viene descritto come implementerai la tua conversazione:

- **Configura uno spazio di lavoro.** Con l'ambiente grafico di facile utilizzo, configura i dati di addestramento e il dialogo per la tua conversazione.

    I dati di addestramento sono costituiti dalle seguenti risorse:
    - **Intenti**: gli obiettivi che prevedi avranno i tuoi utenti quando interagiscono con il servizio. Definisci un intento per ogni obiettivo che può essere identificato nell'input utente. Ad esempio, potresti definire un intento denominato *store_hours* che risponde a domande relative agli orari del negozio. Per ogni intento, aggiungi delle espressioni di esempio che riflettono l'input che i clienti potrebbero utilizzare per chiedere le informazioni di cui hanno bisogno, ad esempio, `A che ora aprite?`
    - **Entità**: un'entità rappresenta un termine o un oggetto che fornisce il contesto per un intento. Ad esempio, un'entità potrebbe essere un nome di città che aiuta il dialogo a distinguere per quale negozio l'utente desidera conoscere gli orari di apertura.

      Man mano che aggiungi dati di addestramento, un classificatore di linguaggio naturale viene automaticamente aggiunto allo spazio di lavoro ed è addestrato a comprendere i tipi di richieste per le quali hai indicato che il servizio deve ascoltare e fornire una risposta.

    Utilizza lo strumento di dialogo per creare un flusso di dialogo che incorpori gli intenti e le entità. Il flusso di dialogo è rappresentato graficamente nello strumento come una struttura ad albero. Puoi aggiungere un ramo per elaborare ciascuno degli intenti che desideri venga gestito dal servizio. Puoi quindi aggiungere nodi di diramazione che gestiscono le numerose possibili permutazioni di una richiesta in base ad altri fattori, ad esempio le entità trovate nell'input utente o le informazioni che vengono passate al servizio dall'applicazione o da un altro servizio esterno.

- **Distribuisci il tuo spazio di lavoro.** Distribuisci lo spazio di lavoro configurato agli utenti connettendolo a un'interfaccia utente di front-end, a un social media o a un canale di messaggistica. La tua istanza distribuita del servizio {{site.data.keyword.conversationshort}} è ospitata da {{site.data.keyword.cloud_notm}}, la piattaforma di elaborazione cloud IBM. (Per ulteriori informazioni, vedi [Platform overview ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://console.bluemix.net/docs/overview/ibm-cloud.html#overview).)

Approfondisci questi passi di implementazione seguendo questi link: 

- [Pianificazione di intenti ed entità](intents-entities.html#planning-your-entities)
- [Panoramica del dialogo](dialog-overview.html)
- [Panoramica della distribuzione](deploy.html)

## Supporto browser

Gli strumenti del servizio {{site.data.keyword.conversationshort}} richiedono lo stesso livello di software browser richiesto da {{site.data.keyword.Bluemix_notm}}. Per dettagli, vedi l'argomento {{site.data.keyword.Bluemix_notm}} [Prerequisites ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://console.bluemix.net/docs/overview/prereqs.html#browsers){: new_window}.

## Supporto linguistico

Il supporto linguistico per funzione è descritto in dettaglio nell'argomento [Lingue supportate](lang-support.html).

## Passi successivi

- [Introduzione](getting-started.html) al servizio
- Prova alcune [demo](sample-applications.html).
- Visualizza l'elenco di [SDK ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://www.ibm.com/watson/developercloud/developer-tools.html){: new_window}.

Hai ancora altre domande? Contatta [IBM Sales ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://www-01.ibm.com/marketing/iwm/dre/signup?source=urx-20970){: new_window}.
