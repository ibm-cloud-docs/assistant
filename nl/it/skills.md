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

# Capacità
{: #skills}

Una capacità di dialogo contiene la logica e i dati di apprendimento che consentono a un assistente di aiutare i tuoi clienti.
{: shortdesc}

Una capacità di dialogo contiene i seguenti tipi di risorse:

- [**Intenti**](/docs/services/assistant?topic=assistant-intents): un *intento* rappresenta lo scopo dell'input di un utente, come una domanda relativa alle sedi aziendali o al pagamento di una fattura. Puoi definire un intento per ogni tipo di richiesta utente che desideri sia supportata dalla tua applicazione. Nello strumento, il nome di un intento ha sempre come prefisso il carattere `#`. Per addestrare la capacità di dialogo a riconoscere i tuoi intenti, fornisci molti esempi di input utente e indica a quali intenti sono associati. 

  Viene fornito un *catalogo di contenuto* che contiene intenti comuni predefiniti che puoi aggiungere alla tua applicazione invece di crearne di tuoi. Ad esempio, la maggior parte delle applicazioni richiedono un intento greeting che avvia un dialogo con un utente. Puoi aggiungere il catalogo di contenuto **General** per aggiungere un intento che saluta l'utente e fa altre cose utili, come terminare la conversazione.

- [**Dialogo**](/docs/services/assistant?topic=assistant-dialog-build): un *dialogo* è un flusso di conversazione ramificato che definisce come l'applicazione risponde quando riconosce gli intenti e le entità che hai definito. Utilizza l'editor del dialogo nello strumento per creare conversazioni con gli utenti, fornendo risposte basate sugli intenti e sulle entità che riconosci nel loro input. 

  ![Diagramma di un'implementazione di base che utilizza solo l'intento e il dialogo.](images/basic-impl.png)

Per consentire alla tua capacità di dialogo di gestire domande con più sfumature, definisci le entità e fai riferimento ad esse dal tuo dialogo.

- [**Entità**](/docs/services/assistant?topic=assistant-entities): un'*entità* rappresenta un termine o oggetto che è rilevante per i tuoi intenti e che fornisce un contesto specifico per un intento. Ad esempio, un'entità potrebbe rappresentare una città in cui l'utente vuole trovare una sede aziendale o l'importo di pagamento di una fattura. Nello strumento, il nome di un'entità ha sempre come prefisso il carattere `@`.

  Puoi preparare la capacità per riconoscere le tue entità fornendo valori e sinonimi di entità, modelli di entità o identificando il contesto in cui un'entità viene normalmente utilizzata in una frase. Per ottimizzare il tuo dialogo, vai indietro e aggiungi dei nodi che controllano le citazioni di entità nell'input utente in aggiunta agli intenti.

![Diagramma di un'implementazione più complessa che utilizza intento, entità e dialogo.](images/complex-impl.png)

Come aggiungi le informazioni, la capacità utilizza questi dati univoci per creare un modello di machine learning che può riconoscere questi input utente o input simili. Ogni volta che aggiungi o modifichi i dati di apprendimento, il processo di training viene attivato per garantire che il modello sottostante rimanga aggiornato quando cambiano i bisogni dei tuoi clienti e gli argomenti di cui vogliono discutere.

Per avere supporto nella creazione di una capacità di dialogo, vedi [Creazione di una capacità](/docs/services/assistant?topic=assistant-skill-add).
