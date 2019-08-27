---

copyright:
  years: 2015, 2019
lastupdated: "2019-07-16"

subcollection: assistant


---

{:curl: #curl .ph data-hd-programlang='curl'}
{:javascript: #javascript .ph data-hd-programlang='javascript'}
{:java: #java .ph data-hd-programlang='java'}
{:python: #python .ph data-hd-programlang='python'}
{:swift: data-hd-programlang='swift'}
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:codeblock: .codeblock}
{:download: .download}
{:deprecated: .deprecated}
{:important: .important}
{:note: .note}
{:tip: .tip}

# Panoramica sull'API {{site.data.keyword.conversationshort}}
{: #api-overview}

Puoi utilizzare le API REST {{site.data.keyword.conversationshort}}, e gli SDK corrispondenti, per sviluppare le applicazioni che interagiscono con il servizio.

## Applicazioni client

Per creare un assistente virtuale o un'altra applicazione client che comunica con un assistente nel runtime, utilizza la nuova API v2. Utilizzando questa API, puoi sviluppare un client rivolto all'utente che può essere distribuito per un utilizzo di produzione, un'applicazione che agisce da broker per la comunicazione tra un assistente e un altro servizio (ad esempio un servizio chat o un servizio di backend) o un'applicazione di test. 

Utilizzando l'API runtime v2 per comunicare con il tuo assistente, la tua applicazione può trarre vantaggio dalle seguenti funzioni: 

- **Gestione automatica dello stato.** L'API runtime v2 gestisce ogni sessione con un utente finale, archiviando e conservando tutti i dati di contesto di cui ha bisogno il tuo assistente per una conversazione completa. 

- **Facilità di distribuzione utilizzando gli assistenti.** Oltre a supportare i client personalizzati, un assistente può essere distribuito facilmente ai canali di messaggistica popolari come Slack e Facebook Messenger.

- **Controllo della versione.** Con il controllo della versione della capacità di dialogo, puoi salvare un'istantanea della tua capacità e collegare il tuo assistente a quella specifica versione. Puoi quindi continuare ad aggiornare la tua versione di sviluppo senza influire sull'assistente di produzione. 

- **Funzionalità di ricerca.** L'API runtime v2 può essere utilizzata per ricevere le risposte sia dalle capacità di dialogo che da quelle di ricerca. Quando viene inoltrata una query a cui il tuo dialogo non può rispondere, l'assistente può utilizzare una capacità di ricerca per trovare la risposta migliore dalle origini dati configurate. (Le capacità di ricerca sono funzioni beta disponibili solo per gli utenti con piano Plus o Premium.)

Per i dettagli sull'API v2, vedi la {{site.data.keyword.conversationshort}} [Guida di riferimento API v2 ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://{DomainName}/apidocs/assistant-v2){: new_window}.

**Nota**: l'API v1 {{site.data.keyword.conversationshort}} supporta ancora il metodo `/message` legacy che invia l'input utente direttamente allo spazio di lavoro utilizzato da una capacità di dialogo. L'API runtime v1 è supportata principalmente a scopo di compatibilità con le versioni precedenti. Se usi il metodo v1 `/message`, devi implementare la tua gestione dello stato e non puoi usufruire del controllo della versione o di qualsiasi altra funzione di un assistente. 

## Applicazioni di creazione

L'API v1 fornisce i metodi che consentono ad un'applicazione di creare o modificare le capacità di dialogo, come alternativa alla creazione grafica di una capacità utilizzando l'interfaccia utente {{site.data.keyword.conversationshort}}. Un'applicazione di creazione utilizza l'API per creare e modificare capacità, intenti, entità, nodi di dialogo e altre risorse che compongono una capacità di dialogo. Per ulteriori informazioni, vedi la [Guida di riferimento API v1 ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://{DomainName}/apidocs/assistant){: new_window}.

  **Nota:** i metodi di creazione v1 creano e modificano gli spazi di lavoro invece delle capacità. Uno spazio di lavoro è un contenitore per il dialogo e i dati di addestramento (ad esempio intenti e entità) all'interno di una capacità di dialogo. Se crei un nuovo spazio di lavoro utilizzando l'API, comparirà come una nuova capacità di dialogo nell'interfaccia utente {{site.data.keyword.conversationshort}}.

Per un elenco dei metodi API disponibili, vedi [Riepilogo dei metodi API](/docs/services/assistant?topic=assistant-api-methods).
