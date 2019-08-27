---

copyright:
  years: 2015, 2019
lastupdated: "2019-05-28"

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

# Attività avanzate
{: #logs-resources}

Ulteriori informazioni sulle API e sugli altri strumenti che puoi utilizzare per accedere e analizzare i dati di log.
{: shortdesc}

## API
{: #logs-resources-api}

Puoi utilizzare l'API `/logs` per elencare gli eventi dalle trascrizioni di conversazioni che si sono verificate tra i tuoi utenti e il tuo assistente. Per una documentazione di riferimento API dettagliata, vedi [List log events ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://cloud.ibm.com/apidocs/assistant#list-log-events-in-a-workspace).

Il numero di giorni in cui i log vengono archiviati varia in base al tipo di piano del servizio. Per i dettagli, consulta [Limiti di log](/docs/services/assistant?topic=assistant-logs#logs-limits).

Puoi eseguire uno script Python per esportare i log e convertirli nel formato CSV, scaricare il file `export_logs.py` dal repository [Watson Assistant GitHub ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://github.com/watson-developer-cloud/community/blob/master/watson-assistant/export_logs.py).

## Terminologia relativa ai log
{: #logs-resources-terminology}

Per prima cosa, controlla le definizioni dei termini associati ai log {{site.data.keyword.conversationshort}}:

- ***Assistente***: un'applicazione - a cui a volte si fa riferimento come 'chatbot' - che implementa il tuo contenuto {{site.data.keyword.conversationshort}}.
- ***Conversazione***: una serie di messaggi formata dai messaggi che un utente individuale invia al tuo assistente e quelli con cui risponde il tuo assistente.
- ***ID conversazione***: l'identificativo univoco che viene aggiunto alle chiamate messagge individuali per collegare gli scambi di messaggi correlati tra loro. Gli sviluppatori dell'applicazione utilizzano la versione V1 dell'API {{site.data.keyword.conversationshort}} per aggiungere questo valore alle chiamate message in una conversazione includendo l'ID nei metadati dell'oggetto di contesto.
- ***ID cliente***: un ID univoco che può essere utilizzato per etichettare i dati del cliente come ad esempio quelli che possono venire successivamente eliminati se il cliente richiede la rimozione dei propri dati.
- ***ID distribuzione***: un'etichetta univoca che gli sviluppatori dell'applicazione che utilizzano la versione V1 dell'API {{site.data.keyword.conversationshort}} passano con ogni messaggio utente per identificare l'ambiente di distribuzione che ha prodotto il messaggio.
- ***Istanza***: la tua distribuzione di {{site.data.keyword.conversationshort}}, accessibile con credenziali univoche. Un'istanza {{site.data.keyword.conversationshort}} potrebbe contenere più assistenti.
- ***Messaggio***: un messaggio è una singola espressione che un utente invia all'assistente.
- ***ID capacità***: l'identificativo univoco di una capacità.
- ***Utente***: un utente è chiunque interagisca con il tuo assistente; spesso sono i tuoi clienti.
- ***ID utente***: un'etichetta univoca utilizzata per tenere traccia del livello di utilizzo del servizio di un utente specifico.
- ***ID spazio di lavoro***: l'identificativo univoco di uno spazio di lavoro. Sebbene tutti gli spazi di lavoro che hai creato prima del 9 novembre siano mostrati come capacità nell'interfaccia utente del prodotto, una capacità e uno spazio di lavoro non sono la stessa cosa. Una capacità è essenzialmente un wrapper per uno spazio di lavoro V1.

**Importante**: la proprietà **ID utente** *non* è equivalente alla proprietà **ID cliente**, anche se entrambe possono essere passate con il messaggio. Il campo **ID utente** viene utilizzato per tenere traccia dei livelli di utilizzo per scopi di fatturazione, mentre il campo **ID cliente** viene utilizzato per supportare l'etichettatura e la seguente eliminazione dei messaggi associati agli utenti finali. L'ID cliente viene utilizzato coerentemente tra tutti i servizi Watson e viene specificato nell'intestazione `X-Watson-Metadata`. L'ID utente viene utilizzato esclusivamente dal servizio {{site.data.keyword.conversationshort}} e viene passato nell'oggetto di contesto di ogni chiamata API /message.

## Abilitazione delle metriche utente
{: #logs-resources-user-id}

Le metriche utente ti permettono di visualizzare, ad esempio, il numero di utenti univoci che hanno interagito con il tuo assistente o il numero medio di conversazioni per utente per un periodo di tempo selezionato sulla [pagina Panoramica](/docs/services/assistant?topic=assistant-logs-overview). Le metriche utente vengono definite utilizzando un parametro `User ID` univoco.

Per specificare l'ID utente (`User ID`) per un messaggio inviato utilizzando l'API `/message`, includi la proprietà `user_id` nell'oggetto di metadati nel tuo [contesto ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://cloud.ibm.com/apidocs/assistant?curl=#get-response-to-user-input){: new_window}, come in questo esempio:

```
"context" : {
  "metadata" : {
       "user_id": "{UserID}"
  }
}
```
{: codeblock}

## Associazione dei dati del messaggio a un utente per l'eliminazione
{: #logs-resources-customer_id}

Potrebbe arrivare il momento in cui vuoi rimuovere completamente una serie di dati del tuo utente da un'istanza {{site.data.keyword.conversationshort}}. Quando viene utilizzata la funzione di eliminazione, le metriche generali non rifletteranno più i messaggi eliminati; ad esempio, avranno meno conversazioni totali.

### Prima di iniziare
{: #logs-resources-delete-customer-id-prereqs}

Per eliminare i messaggi per uno o più individui, devi prima associare un messaggio a un **ID cliente** per ogni individuo. Per specificare l'**ID cliente** per tutti i messaggi inviati utilizzando l'API `/message`, includi la proprietà `X-Watson-Metadata: customer_id` nella tua intestazione. Puoi passare più voci **ID clienti** con coppie `field=value` separate da punto e virgola, utilizzando `customer_id`, come nel seguente esempio:

```
curl -X POST -u "apikey:3Df... ...Y7Pc9"
 --header
   'Content-Type: application/json'
   'X-Watson-Metadata: customer_id={first-customer-ID};customer_id={second-customer-ID}'
 --data '{"input":{"text":"hello"}}' 'https:// gateway-us-south.watsonplatform.net/assistant/api/v1/workspaces/{workspaceID}/message?version=2018-09-20'
```
{: codeblock}

La stringa `customer_id` non può includere i caratteri punto e virgola (`;`) o segno uguale (`=`). È tua responsabilità garantire che ogni parametro `Customer ID` sia univoco tra i tuoi clienti.
{: note}

Per eliminare i messaggi utilizzando i valori `customer_id`, consulta l'argomento [Informazioni sulla sicurezza](/docs/services/assistant?topic=assistant-information-security#information-security-gdpr-wa).

## Notebook Jupyter
{: #logs-resources-jupyter-notebooks}

IBM ha creato dei notebook Jupyter che puoi utilizzare per analizzare i tuoi dati di log in modo più dettagliato. Un notebook Jupyter è un ambiente basato sul web per il calcolo interattivo. Puoi eseguire piccola parti di codice che elaborano i tuoi dati e puoi visualizzare immediatamente i risultati del tuo calcolo.

Esiste una serie di notebook che puoi utilizzare con gli strumenti Python standard e una serie progettata per l'utilizzo ottimale con {{site.data.keyword.DSX_full}}. {{site.data.keyword.DSX_short}} è un prodotto che fornisce un ambiente in cui puoi selezionare e scegliere gli strumenti di cui hai bisogno per analizzare e visualizzare i dati, per ripulire e modellare i dati, per inserire i dati di streaming, o per creare, preparare e distribuire i modelli di machine learning. Per ulteriori dettagli, vedi la [documentazione del prodotto ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://dataplatform.cloud.ibm.com/docs/content/getting-started/welcome-main.html){:new_window}.

Per ulteriori informazioni su come i notebook possono aiutarti a migliorare il tuo assistente, [leggi questo post di blog ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://medium.com/ibm-watson/continuously-improve-your-watson-assistant-with-jupiter-notebooks-60231df4f01f).

Sono disponibili i seguenti notebook:

- **Measure**: raccoglie le metriche che si focalizzano sulla copertura (quanto spesso l'assistente è abbastanza sicuro per rispondere agli utenti) e l'efficacia (quando l'assistente risponde, se le risposte soddisfano i bisogni dell'utente).

- **Effectiveness**: esegue delle analisi approfondite sui tuoi log per aiutarti a comprendere i passi che puoi eseguire per migliorare il tuo assistente.

La [Guida Watson Assistant Continuous Improvement Best Practices ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://www.ibm.com/common/ssi/cgi-bin/ssialias?htmlfid=54022554USEN&) descrive come ottenere il massimo dai notebook.

### Utilizzo dei notebook con {{site.data.keyword.DSX}}
{: #logs-resources-notebooks-studio}

Se scegli di utilizzare i notebook progettati per l'utilizzo con {{site.data.keyword.DSX}}, la procedura è approssimativamente questa:

1.  Crea un account {{site.data.keyword.DSX}}, [crea un progetto ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://dataplatform.cloud.ibm.com/docs/content/getting-started/projects.html?context=analytics){:new_window} e aggiungi un account Cloud Object Storage ad esso.
1.  Dalla community {{site.data.keyword.DSX}}, ottieni il [Notebook Measure Watson Assistant Performance ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")]( https://dataplatform.cloud.ibm.com/exchange/public/entry/view/133dfc4cd1480bbe4eaa78d3f635e568).
1   Segui le istruzioni passo dopo passo fornite con il notebook per analizzare una sottoserie di scambi di dialogo dai log.

    Le informazioni approfondite vengono visualizzate in modi che rendono più facile comprendere l'efficacia e la copertura dell'assistente.
1.  Esporta una serie di esempio di log da conversazioni inefficaci, quindi le analizza e annota.

    Ad esempio, indica se una risposta è corretta. Se corretta, contrassegna se è utile. Se una risposta non è corretta, ne identifica la causa principale, ad esempio se è stato rilevato un intento o un'entità non corretto oppure se è stato attivato un nodo di dialogo non corretto. Dopo aver identificato la causa principale, indica quale sarebbe stata la scelta corretta.
1.  Inserisce il foglio di calcolo annotato nel [Notebook Analyze Watson Assistant Effectiveness](https://dataplatform.cloud.ibm.com/exchange/public/entry/view/133dfc4cd1480bbe4eaa78d3f636921c).

Questo processo ti aiuta a comprendere i passi che puoi eseguire per migliorare il tuo assistente. Non c'è modo per applicare automaticamente quello che hai imparato alla tua istanza del servizio. Tieni traccia di tutte le modifiche che apporti per migliorare il sistema, in modo da poterle successivamente applicare direttamente ai dati di addestramento della tua capacità di dialogo.

### Utilizzo dei notebook con gli strumenti Python standard
{: #logs-resources-notebooks-python}

Se scegli di utilizzare gli strumenti Python standard per eseguire i notebook, puoi ottenere i notebook dal [repository IBM GitHub](https://github.com/watson-developer-cloud/assistant-improve-recommendations-notebook/tree/master/notebook). Assicurati di eseguirli nel seguente ordine:

1.  Measure Notebook.ipynb
1.  Effectiveness Notebook.ipynb
