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

# Sicurezza delle informazioni
{: #information-security}

IBM si impegna a fornire ai nostri clienti e partner soluzioni innovative per la privacy, la sicurezza e la governance dei dati.
{: shortdesc}

**Avviso:**
È responsabilità dei clienti garantire la propria conformità alle varie leggi e normative, incluso il Regolamento generale sulla protezione dei dati (GDPR) dell'Unione Europea. È responsabilità esclusiva dei clienti richiedere una consulenza legale qualificata per identificare e interpretare normative e regolamenti importanti che potrebbero influenzare le attività di business dei clienti ed eventuali azioni che i clienti dovranno intraprendere per rispettare la conformità a tali normative e regolamenti. 

I prodotti, i servizi e le altre funzionalità qui descritti non sono adatti per tutte le situazioni dei clienti e potrebbero avere una disponibilità limitata. IBM non fornisce consulenza legale, contabile o di controllo né dichiara o garantisce che i propri servizi o prodotti assicureranno che i clienti siano conformi ad eventuali norme di legge o regolamentari. 

Se hai bisogno di richiedere il supporto GDPR per le risorse {{site.data.keyword.cloud}} {{site.data.keyword.watson}} che vengono create

- Nell'Unione europea, vedi [Requesting support for IBM Cloud Watson resources created in the European Union![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://cloud.ibm.com/docs/services/watson/getting-started-gdpr-sar#request-EU){: new_window}.
- Fuori dall'Unione europea, vedi [Requesting support for resources outside the European Union![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://cloud.ibm.com/docs/services/watson/getting-started-gdpr-sar#request-non-EU){: new_window}.

## Regolamento generale sulla protezione dei dati dell'Unione europea (GDPR)
{: #information-security-gdpr}

IBM si impegna a offrire ai nostri clienti e partner soluzioni innovative per la privacy, la sicurezza e la governance dei dati per assisterli nel loro percorso verso la conformità GDPR. 

È possibile trovare ulteriori informazioni sul percorso di preparazione verso la disponibilità e sulle nostre funzionalità e offerte GDPR che supportano il tuo percorso di conformità [qui ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](../../icons/launch-glyph.svg "Icona link esterno")](http://www.ibm.com/gdpr){: new_window}.

## Etichettatura ed eliminazione dei dati in {{site.data.keyword.conversationshort}}
{: #information-security-gdpr-wa}

Non aggiungere dati personali ai dati di formazione (entità e intenti, inclusi gli esempi utente) che hai creato. In particolare, assicurati di rimuovere tutte le informazioni d'identificazione personale dai file che contengono le espressioni utente reali che hai caricato per ricavare i consigli utente di esempio. 

**Nota:** le funzioni sperimentale e beta non sono destinate per essere utilizzate con un ambiente di produzione e quindi non è garantito che funzionino come previsto quando si etichettano ed eliminano i dati. Le funzioni sperimentale e beta non dovrebbero essere utilizzate durante l'implementazione di una soluzione che richiede l'etichettatura e l'eliminazione dei dati. 

Se devi rimuovere i dati di un cliente da un'istanza {{site.data.keyword.conversationshort}}, puoi farlo basandoti sull'ID del cliente finché associ il messaggio a un ID cliente quando il messaggio viene inviato al servizio. 

**Nota:** le funzioni di integrazione di Preview Link e di Facebook automatica non supportano l'etichettatura e quindi l'eliminazione dei dati si basa sull'ID cliente. Queste funzioni non devono essere utilizzate in una soluzione che richiede la capacità di eseguire l'eliminazione basata sull'ID cliente. 

### Prima di iniziare
{: #information-security-delete-user-data-prereqs}

Per poter eliminare i dati del messaggio associati a uno specifico utente, devi innanzitutto associare tutti i messaggi a un **ID cliente** univoco per ciascun utente. Per specificare l'**ID cliente** per tutti i messaggi utilizzando l'API `/message`, includi la proprietà `X-Watson-Metadata: customer_id` nella tua intestazione. Ad esempio:

```
curl -X POST -u "apikey:3Df... ...Y7Pc9"
 --header
   'Content-Type: application/json'
   'X-Watson-Metadata: customer_id=abc'
 --data
   '{"input":{"text":"hello"}}'
  'https://gateway-eu-de.watsonplatform.net/assistant/api/v1/workspaces/{workspaceID}/message?version=2018-09-20'
```
{: codeblock}

La stringa `customer_id` non può includere i caratteri punto e virgola (`;`) o segno uguale (`=`). È tua responsabilità garantire che ogni proprietà `customer ID` sia univoca tra i tuoi clienti.
{: note}

Puoi passare più valori **customer ID** con coppie `customer_id={value}` separate da punto e virgola. Ad esempio: `'X-Watson-Metadata: customer_id=abc;customer_id=xyz'`

Se aggiungi una capacità di ricerca a un assistente, l'input utente inviato all'assistente viene passato al servizio {{site.data.keyword.discoveryshort}} come query di ricerca. Se l'integrazione {{site.data.keyword.conversationshort}} fornisce un ID cliente, la richiesta risultante API /message include l'ID cliente nell'intestazione e l'ID viene passato alla richiesta API /query {{site.data.keyword.discoveryshort}}. Per eliminare i dati query associati a uno specifico cliente, devi inviare una richiesta di eliminazione direttamente all'istanza del servizio {{site.data.keyword.discoveryshort}} collegata al tuo assistente. Per i dettagli, vedi l'argomento {{site.data.keyword.discoveryshort}} [Sicurezza delle informazioni](https://cloud.ibm.com/docs/services/discovery/information-security#gdpr-discovery).

### Esecuzione della query dei dati utente
{: #information-security-query-customer-id}

Utilizza il parametro `filter` del metodo v1 `/logs` per ricercare specifici dati utente in un log dell'applicazione. Ad esempio, per ricercare dati specifici per un `customer_id` che corrisponde a `my_best_customer`, la query potrebbe essere:

``` curl
curl -X GET -u "apikey:3Df... ...Y7Pc9"
'https://gateway-eu-de.watsonplatform.net/assistant/api/v1/workspaces/{workspaceID}/logs?version=2018-09-20&filter=customer_id::my_best_customer'
```
{: codeblock}

Per ulteriori dettagli, vedi [Riferimento alla query di filtro](/docs/services/assistant?topic=assistant-filter-reference). 

### Eliminazione dei dati
{: #information-security-delete-data}

Per eliminare i dati di log del messaggio associati a uno specifico utente che potrebbero essere stati memorizzati dal servizio, utilizza il metodo API `DELETE /user_data` v1. Specifica l'ID cliente dell'utente passando un parametro `customer_id` con la richiesta. 

Solo i dati aggiunti utilizzando l'endpoint API `POST /message` con un ID cliente associato possono essere eliminati utilizzando questo metodo di eliminazione. I dati aggiunti da altri metodi non possono essere eliminati in base all'ID cliente. Ad esempio, le entità e gli intenti aggiunti dalle conversazioni del cliente, non possono essere eliminati in questo modo. I dati personali non sono supportati per tali metodi. 

**IMPORTANTE**: se specifichi un `customer_id`, verranno eliminati *tutti* i messaggi con tale `customer_id` ricevuti prima della richiesta di eliminazione, in tutta la tua istanza {{site.data.keyword.conversationshort}}, non solo all'interno di una capacità. 

Ad esempio, per eliminare i dati del messaggio associati a un utente con ID cliente `abc` dalla tua istanza {{site.data.keyword.conversationshort}}, invia il seguente comando cURL:

```
curl -X DELETE -u "apikey:3Df... ...Y7Pc9"
 'https://gateway-eu-de.watsonplatform.net/assistant/api/v1/user_data?customer_id=abc&version=2018-09-20'
```
{: codeblock}

Viene restituito un oggetto JSON `{}` vuoto.

Per ulteriori informazioni, vedi [Riferimento API](https://cloud.ibm.com/apidocs/assistant?curl=#delete-labeled-data).

**Nota:** le richieste di eliminazione vengono elaborate in batch e il completamento può richiedere fino a 24 ore. 
