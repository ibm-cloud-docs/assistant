---

copyright:
  years: 2015, 2018
lastupdated: "2018-01-09"

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

# Pianificazione di intenti ed entità
{: #planning-your-entities}

Per pianificare gli *intenti* per la tua applicazione, devi considerare cosa potrebbero voler fare i tuoi clienti e cosa vuoi che sia in grado di gestire la tua applicazione. Scegliere l'intento corretto per l'input dell'utente è il primo passo per fornire una risposta utile. Gli intenti che identificherai per la tua applicazione determineranno i flussi di dialogo che devi creare; possono anche determinare i sistemi di back-end con i quali l'applicazione deve integrarsi per completare le richieste dei clienti (come i database dei clienti o i sistemi di elaborazione dei pagamenti).

Una *entità* rappresenta un termine o un oggetto nell'input dell'utente che fornisce chiarimenti o specifici contesti per un particolare intento. Se gli intenti rappresentano i verbi (qualcosa che un utente vuole fare), le entità rappresentano i nomi (come l'oggetto o il contesto per un'azione). Le entità consentono a un singolo intento di rappresentare più azioni specifiche. Un'entità definisce una classe di oggetti, con valori specifici che rappresentano possibili oggetti in quella classe.

In sostanza, se vuoi acquisire una richiesta o eseguire un'azione, utilizza un oggetto. Se vuoi acquisire informazioni che possono influire sul modo in cui rispondi a una richiesta o un'azione, utilizza un'entità.

Ad esempio, supponi di voler creare un'applicazione dashboard di automobile cognitiva che consenta agli utenti di attivare o disattivare gli accessori:

1.  Raccogli il maggior numero di domande, comandi o altri input dei clienti reali. L'utilizzo dell'input da utenti reali fornisce un quadro migliore dell'input previsto rispetto all'utilizzo di elenchi di possibili espressioni creati dagli esperti. Ricorda che i clienti potrebbero esprimere lo stesso tipo di richiesta in molti modi diversi. Ad esempio, tutte le seguenti espressioni di esempio rappresentano richieste per disattivare qualcosa:

    - `Fari spenti`
    - `Spegni la radio`
    - `Arresta il condizionatore d'aria`

1.  Una volta che hai un elenco di esempi, ordinali in categorie in base alle funzionalità che vuoi che la tua applicazione supporti; queste categorie rappresentano gli intenti che definirai, mentre gli esempi aiuteranno la tua applicazione a identificare quegli intenti in un nuovo input.

    Non creare intenti troppo simili. Per il servizio {{site.data.keyword.conversationshort}} potrebbe essere difficile distinguere gli intenti simili. Se scopri di avere diversi intenti con un significato simile, considera se combinarli in un unico intento e quindi utilizzare le entità per fornire più risposte possibili per tale intento.
    {: tip}

    Poiché gli esempi precedenti rappresentano tutti lo stesso intento (disattivare qualcosa), potresti chiamare questa categoria `#turn_off`.
    Ricorda che l'input di un cliente non deve essere una corrispondenza esatta per i tuoi esempi; gli intenti vengono riconosciuti mediante l'elaborazione del linguaggio naturale.
    {: tip}

1.  Puoi quindi utilizzare un'entità per rappresentare ciò che l'utente desidera disattivare. Per farlo, potresti creare un'entità denominata `@accessory` e fornirle i seguenti valori possibili:

    - `fari`
    - `radio`
    - `condizionatore d'aria`

    Per ogni valore, puoi specificare anche dei sinonimi. Ad esempio, potresti specificare `AC` e `raffreddamento` come sinonimi di `condizionatore d'aria`; tutte e tre le stringhe sono considerate come uno stesso valore. Elencare più modi in cui gli utenti potrebbero fare riferimento allo stesso valore aiuta a migliorare la precisione dell'applicazione.
1.  Quando l'input dell'utente viene ricevuto, la conversazione riconosce sia gli intenti che le entità. Il flusso di dialogo può quindi utilizzarli per fornire la risposta migliore. Dovresti solo creare un'entità per qualcosa che conti in termini di modifica del modo in cui l'applicazione risponde a un intento.
    Oltre alle entità che definisci, il servizio fornisce un insieme di entità di sistema già definite e disponibili per l'utilizzo. Per ulteriori informazioni, vedi [Abilitazione delle entità di sistema](entities.html#enable_system_entities).
    {: tip}

1.  Continua a perfezionare i tuoi intenti, entità ed esempi secondo necessità. Non considerare il tuo insieme di intenti ed entità come dei prodotti finiti. È probabile che quando [crei i tuoi dialoghi](dialog-build.html), identificherai ulteriori intenti ed entità da aggiungere. Puoi anche continuare a raccogliere input dai nuovi clienti e usarli per aggiungere nuovi esempi; questo processo iterativo migliora la capacità della tua applicazione di riconoscere accuratamente intenti ed entità.
