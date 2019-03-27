---

copyright:
  years: 2015, 2019
lastupdated: "2019-02-12"

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

# Ottieni supporto nella creazione degli intenti ![Beta](images/beta.png) ![Solo Plus o Premium](images/premium.png)
{: #beta-intent-recommendations}

Se hai dati di trascrizioni delle chat esistenti del supporto clienti aziendale, consenti a Watson di analizzare tali dati per comprendere le esigenze del cliente alla cui soddisfazione il tuo team di supporto dedica la maggior parte del suo tempo.
{: shortdesc}

Questa funzione è disponibile per essere utilizzata solo dai partecipanti al programma beta. Per capire come richiedere l'accesso, vedi [Partecipa al programma beta](/docs/services/assistant?topic=assistant-feedback#feedback-beta).

![Beta](images/beta.png) IBM rilascia servizi, funzioni e supporto linguistico classificati come beta per la tua valutazione. Queste funzioni potrebbero essere instabili, cambiare frequentemente e potrebbero essere sospese con breve preavviso. Inoltre, le funzioni beta potrebbero non fornire lo stesso livello di prestazioni o di compatibilità fornito generalmente dalle funzioni e non sono progettate per essere utilizzate in un ambiente di produzione.

Quando viene rilasciata, questa funzione sarà disponibile solo per gli utenti che hanno un piano Plus o Premium e che utilizzano le espressioni in lingua Inglese.
{: tip}

Le esigenze del cliente vengono rappresentate in {{site.data.keyword.conversationshort}} come *intenti*. Se non hai ancora definito gli intenti, puoi iniziare velocemente richiedendo supporto a Watson. Carica i file con le espressioni utente dalle trascrizioni del call center affinché il servizio {{site.data.keyword.conversationshort}} le analizzi. In base alle informazioni approfondite che non vengono trattate, il servizio consiglia una serie di base di intenti che devi creare per soddisfare le esigenze più frequenti dei tuoi clienti. 

Poiché gli argomenti di cui i tuoi clienti desiderano discutere cambiano, puoi utilizzare la funzione dei consigli utente di esempio dell'intento come supporto per tenere i tuoi intenti aggiornati e rilevanti nel tempo. 

Estrai i dati esistenti per eseguire una delle seguenti attività: 

- [Ottieni i consigli sull'intento](#beta-intent-recommendations-get-intent-recommendations)
- [Ottieni i consigli sull'esempio utente dell'intento](#beta-intent-recommendations-get-example-recommendations)

## Carica i tuoi file
{: #beta-intent-recommendations-log-files-add}

Prima di iniziare, crea un file da fornire al servizio. Il file deve essere un file CSV (comma-separated value), con un'espressione utente per riga. Idealmente, le espressioni sono frasi brevi che vengono estratte dalle tue trascrizioni del call center che contengono domande e richieste reali del cliente. Ciascun file dell'espressione utente può avere una dimensione massima di 20 MB.

Segui queste linee guida aggiuntive:

  - Rimuovi tutti i dati sensibili dalle espressioni che includerai nel file. 

    I dati sensibili includono tutte le informazioni relative a una persona fisica identificabile come ad esempio nomi, indirizzi email e ID cliente e dati regolamentati come le informazioni sanitarie protette.
  - Non includere le espressioni utente più lunghe di 1.024 caratteri. Le espressioni più lunghe vengono troncate. 
  - Il file deve contenere almeno 100 espressioni; un file con 500 o più espressioni ti fornirà risultati migliori. 
  - Se un'espressione contiene una virgola, racchiudila tra virgolette. 
  - Il file CSV deve includere solo una colonna. 
  - Rimuovi qualsiasi elemento che non sia un'espressione generata dall'utente, incluse le note o le risposte dell'operatore. 

  Ad esempio:

  ```
  What happens to my coverage if I trade in my car?
  i'd like to buy a house.
  How do I add a dependent to my plan?
  "first, i want to know if i am already registered."
  ```

Tutti i file che hai caricato vengono condivisi tra tutte le capacità nell'istanza del servizio corrente. Le espressioni che provengono da tutti i file disponibili vengono estratte quando richiedi sia i consigli sull'intento che sull'esempio utente dell'intento. 

## Ottieni i consigli sull'intento da Watson
{: #beta-intent-recommendations-get-intent-recommendations}

Inizia rapidamente lasciando che il servizio analizzi le tue trascrizioni delle chat del call center e ti consigli alcuni intenti iniziali con cui iniziare. Se hai già creato alcuni intenti, lascia che il servizio analizzi i tuoi log e confronti le ricerche con i tuoi intenti esistenti per identificare le lacune nei tuoi dati di addestramento e suggerirti nuovi intenti per colmarle. 

Per utilizzare questa funzione, carica uno o più file contenenti le espressioni che sono state inoltrate da utenti reali nel momento in cui hanno chiesto supporto. Il file potrebbe elencare le richieste utente che sono state estratte dai log del call center, ad esempio. Il servizio valuta queste espressioni e identifica le aree problematiche comuni che i clienti menzionano frequentemente. Lo strumento {{site.data.keyword.conversationshort}} visualizza quindi una serie di intenti separati che acquisiscono queste esigenze utente di tendenza. Puoi esaminare ciascun intento consigliato e gli esempi utente corrispondenti per scegliere quelli che desideri aggiungere ai tuoi dati di addestramento. 

## Ottieni i consigli sull'intento
{: #beta-intent-recommendations-get-intent-recommendations-task}

Per ottenere i consigli sull'intento, completa i seguenti passi:

1.  Nello strumento {{site.data.keyword.conversationshort}}, apri la tua capacità di dialogo e poi fai clic sulla scheda **Intents** nella barra di navigazione. Se **Intents** non è visibile, utilizza il menu ![Menu](images/Menu_16.png) per aprire la pagina.

1.  Fai clic su **Get recommendations**.

1.  **Solo la prima volta**: fai clic su **Add file** e poi fai clic su **Choose a file** per ricercare il file CSV che hai creato in precedenza e selezionalo.

    Concedi al servizio il tempo di analizzare i tuoi dati e raggruppare le espressioni. 

1.  Esamina gli intenti consigliati dal servizio. 

    Le esigenze utente più frequenti vengono acquisite negli intenti che vengono elencati per primi. Puoi passare con il mouse su un intento per vedere alcuni esempi delle sue espressioni associate. Se desideri vedere un elenco di tutti i raggruppamenti degli intenti, fai clic su **Show all recommendations**.

1.  Fai clic su un intento per vedere la serie completa di esempi utente associati ad esso ed esegui una delle seguenti azioni: 

    Deseleziona le espressioni che non desideri aggiungere come esempi utente. 

    - Per aggiungere l'intento consigliato con le espressioni selezionate come esempi utente, fai clic su **Create intent**.
    - Per aggiungere invece le espressioni selezionate dall'intento consigliato a uno dei tuoi intenti esistenti come esempi utente, fai clic su **Add to existing intent**, scegli l'intento e poi fai clic su **Add**.

Gli intenti e gli esempi utente dell'intento che aggiungi in questo modo vengono conteggiati nei tuoi totali degli intenti e degli esempi utente dell'intento per i quali sono stabiliti dei limiti basati sul piano. Per ulteriori dettagli, vedi [Limiti di intenti](/docs/services/assistant?topic=assistant-intents#intents-limits).

## Ottieni i consigli sull'esempio utente dell'intento
{: #beta-intent-recommendations-get-example-recommendations}

Il seguente video fornisce una panoramica di due minuti su come ottenere consigli sugli esempi utente dell'intento. 

<iframe class="embed-responsive-item" id="youtubeplayer" title="Consigli sugli esempi utente dell'intento" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/L3FI8KeZfsc?rel=0" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

Per ottenere esempi utente per gli intenti esistenti, puoi utilizzare gli stessi file che hai caricato in precedenza. Non hai bisogno di associare gli esempi agli intenti. Fornisci semplicemente le espressioni utente non elaborate e lascia che il servizio scelga quelle appropriate per l'intento corrente. Per ogni intento, il servizio analizza gli stessi dati per trovare gli esempi utente appropriati per tale intento. 

1.  Segui i passi presenti in [Creazione di intenti](/docs/services/assistant?topic=assistant-intents#intents-creating-intents-task) per creare un intento.

1.  Aggiungi almeno 5 esempi utente che mostrano la gamma completa di espressioni tipiche che sai che gli utenti potrebbero utilizzare per attivare questo intento. 

    Questi esempi utente seed istruiscono il servizio sui tipi di espressioni da ricercare nei file che hai caricato. 

1.  Fai clic su **Show recommendations**.

    I file di origine degli esempi utente sono condivisi tra le capacità in un'istanza del servizio. Se i tuoi collaboratori possiedono delle capacità nella stessa istanza e caricano dei file, i loro file verranno aggiunti alla tua raccolta *User example source files* condivisa.

1.  **Solo la prima volta**: fai clic su **Upload files** e poi fai clic su **Choose a file** per ricercare il file CSV che hai creato in precedenza e selezionalo.

    Va bene se il file che hai caricato contiene le espressioni per tutti i tipi di intenti. Il servizio sa su quali intenti stai lavorando e trova gli esempi adatti da consigliare per questo intento specifico. 

    Una volta che il file viene caricato ed elaborato dal servizio, vengono visualizzate le espressioni consigliate. Se non vi sono consigli, il file non contiene esempi adatti per questo intento. 

1.  Se il file non fornisce consigli utili per nessuno dei tuoi intenti, puoi provare con una serie diversa di espressioni caricando un altro file. Per ulteriori dettagli, vedi [Gestione dei file caricati](#beta-intent-recommendations-manage-files). 

1.  Una volta che il servizio ti mostra i consigli, seleziona le espressioni che desideri aggiungere come esempi utente per questo intento e poi fai clic su **Add**. Oppure fai clic su **Next set** per esaminare altre espressioni. 
1.  Se desideri ricercare tu stesso gli esempi utente nel contenuto del file CSV, fai clic sulla scheda **Search Logs**, immetti una parola chiave in base alla quale eseguire la ricerca e quindi premi **Invio**.

    Segui queste linee guida per la sintassi della query di ricerca:

    - Sono supportati gli operatori booleani (ad esempio `AND` e `OR`). 
    - Aggiungi il testo tra virgolette per ricercare una corrispondenza esatta del testo ("thisstringmustbepresent").
    - Puoi utilizzare le espressioni regolari, ad esempio `*ly` per trovare tutti i termini che finiscono con `ly`.
    - I seguenti caratteri vengono utilizzati come operatori di espressioni regolari: 

      `+ - = && || > < ! ( ) { } [ ] ^ " ~ * ? : \ /`

      Se desideri includerne uno in un termine di ricerca senza che venga elaborato come un operatore, devi aggiungere una barra rovesciata (`\`) come prefisso.

Gli esempi utente che aggiungi in questo modo vengono conteggiati nei tuoi totali degli esempi utente dell'intento per i quali sono stabiliti dei limiti basati sul piano. Per ulteriori dettagli, vedi [Limiti di intenti](/docs/services/assistant?topic=assistant-intents#intents-limits).

## Gestione dei file caricati
{: #beta-intent-recommendations-manage-files}

Una volta che hai caricato almeno un file, puoi aprire la raccolta *User example source files* per gestire i tuoi file. I file caricati vengono aggiunti a una raccolta di file che viene condivisa per tutta l'istanza del servizio {{site.data.keyword.conversationshort}} corrente. 

1.  Per accedere alla raccolta, fai clic per aprire un intento, fai clic su **Show recommendations** e quindi fai clic su **View Files** dalla barra laterale.

    Se non hai ancora aggiunto nessun intento, dalla pagina principale Intents, espandi la sezione *Intent recommendations*, fai clic su **Get recommendations**, fai clic su **Show all recommendations** e poi fai clic su **View Files**.

1.  Puoi eseguire le seguenti azioni:

    - Per aggiungere un file, fai clic su **Add Files** e poi ricerca un file e selezionalo. 
    - Per eliminare un file, devi rimuovere tutti i file che sono stati caricati da qualsiasi capacità associata a questa istanza del servizio; non puoi eliminare solo un file. Innanzitutto, assicurati che nessun altro stia utilizzando i file, poi fai clic su **Delete all** per eliminare tutti i file caricati. 

1.  Chiudi la pagina *User example source files*.
