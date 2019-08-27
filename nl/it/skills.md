---

copyright:
  years: 2015, 2019
lastupdated: "2019-08-02"

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

# Capacità
{: #skills}

Una capacità è un contenitore di intelligenza artificiale che consente a un assistente di aiutare i tuoi clienti.
{: shortdesc}

Un assistente indirizza le richieste sul percorso ottimale per la risoluzione di un problema del cliente. Aggiungi delle capacità in modo che il tuo assistente possa fornire una risposta diretta a una domanda comune o fare riferimento a dei risultati di una ricerca più generalizzati per qualcosa di più complesso.

## Tipi di capacità
{: #skills-types}

Puoi aggiungere i seguenti tipi di capacità al tuo assistente:

- **[Capacità di dialogo](#skills-dialog-skill)**: comprende le domande o le richieste tipiche dagli utenti e risponde o le soddisfa seguendo un dialogo che hai scritto tu.

![Solo piano Plus o Premium](images/plus.png) se sei un utente del piano Plus o Premium, puoi anche creare questo tipo di capacità:

- **[Capacità di ricerca](#skills-search-skill)**: risponde alla domanda di un utente ricercando delle informazioni pertinenti da un'origine dati esterna, estraendo il passaggio e restituendolo come la risposta dell'assistente.

### Capacità di dialogo
{: #skills-dialog-skill}

Una capacità di dialogo contiene la logica e i dati di apprendimento che consentono a un assistente di aiutare i tuoi clienti. Contiene i seguenti tipi di risorse:

- [**Intenti**](/docs/services/assistant?topic=assistant-intents): un *intento* rappresenta lo scopo dell'input di un utente, come una domanda relativa alle sedi aziendali o al pagamento di una fattura. Puoi definire un intento per ogni tipo di richiesta utente che desideri sia supportata dalla tua applicazione. Il nome di un intento ha sempre come prefisso il carattere `#`. Per addestrare la capacità di dialogo a riconoscere i tuoi intenti, fornisci molti esempi di input utente e indica a quali intenti sono associati.

  Viene fornito un *catalogo di contenuto* che contiene intenti comuni predefiniti che puoi aggiungere alla tua applicazione invece di crearne di tuoi. Ad esempio, la maggior parte delle applicazioni richiedono un intento greeting che avvia un dialogo con un utente. Puoi aggiungere il catalogo di contenuto **General** per aggiungere un intento che saluta l'utente e fa altre cose utili, come terminare la conversazione.

- [**Dialogo**](/docs/services/assistant?topic=assistant-dialog-build): un *dialogo* è un flusso di conversazione ramificato che definisce come l'applicazione risponde quando riconosce gli intenti e le entità che hai definito. Utilizza l'editor del dialogo per creare conversazioni con gli utenti, fornendo risposte basate sugli intenti e sulle entità che riconosci nel loro input. 

  ![Diagramma di un'implementazione di base che utilizza solo l'intento e il dialogo.](images/basic-impl.png)

Per consentire alla tua capacità di dialogo di gestire domande con più sfumature, definisci le entità e fai riferimento ad esse dal tuo dialogo.

- [**Entità**](/docs/services/assistant?topic=assistant-entities): un'*entità* rappresenta un termine o oggetto che è rilevante per i tuoi intenti e che fornisce un contesto specifico per un intento. Ad esempio, un'entità potrebbe rappresentare una città in cui l'utente vuole trovare una sede aziendale o l'importo di pagamento di una fattura. Il nome di un'entità ha sempre come prefisso il carattere `@`.

  Puoi preparare la capacità per riconoscere le tue entità fornendo valori e sinonimi di entità, modelli di entità o identificando il contesto in cui un'entità viene normalmente utilizzata in una frase. Per ottimizzare il tuo dialogo, vai indietro e aggiungi dei nodi che controllano le citazioni di entità nell'input utente in aggiunta agli intenti.

![Diagramma di un'implementazione più complessa che utilizza intento, entità e dialogo.](images/complex-impl.png)

Come aggiungi le informazioni, la capacità utilizza questi dati univoci per creare un modello di machine learning che può riconoscere questi input utente o input simili. Ogni volta che aggiungi o modifichi i dati di apprendimento, il processo di training viene attivato per garantire che il modello sottostante rimanga aggiornato quando cambiano i bisogni dei tuoi clienti e gli argomenti di cui vogliono discutere.

Per avere supporto nella creazione di una capacità di dialogo, vedi [Creazione di una capacità di dialogo](/docs/services/assistant?topic=assistant-skill-dialog-add).

### Capacità di ricerca ![Solo piano Plus o Premium](images/plus.png)
{: #skills-search-skill}

Quando Watson Assistant non dispone di una soluzione esplicita a un problema, instrada la domanda dell'utente a una capacità di ricerca per trovare una risposta dalle tue varie origini di contenuto self-service. La capacità di ricerca interagisce con il servizio {{site.data.keyword.discoveryfull}} per estrarre queste informazioni da una raccolta dati configurata.

Se già utilizzi il servizio {{site.data.keyword.discoveryshort}}, puoi estrarre le tue raccolte dati esistenti per il materiale di origine che puoi condividere con i clienti per rispondere alle loro domande.

Tuttavia, non hai bisogno di avere un'istanza del servizio {{site.data.keyword.discoveryshort}}. Se scegli di creare una capacità di ricerca, ti viene fornita un'istanza gratuita di {{site.data.keyword.discoveryshort}}. Puoi quindi creare una raccolta da un'origine dati e configurare la tua capacità di ricerca per cercare in questa raccolta e trovare delle risposte alle domande del cliente.

Il seguente diagramma illustra come viene elaborato l'input utente quando vengono aggiunte una capacità di dialogo e di ricerca a un assistente. Tutte le domande per cui il dialogo non è progettato a rispondere, vengono inviate alla capacità di ricerca, che trova una risposta pertinente da una raccolta dati {{site.data.keyword.discoveryshort}}.

![Diagramma di come una domanda viene instradata alla capacità di ricerca.](images/search-skill-diagram.png)

Per avere supporto nella creazione di una capacità di ricerca, vedi [Creazione di una capacità di ricerca](/docs/services/assistant?topic=assistant-skill-search-add).
