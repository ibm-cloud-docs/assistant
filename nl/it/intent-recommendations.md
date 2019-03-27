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

# Richiedere assistenza da Watson
{: #intent-recommendations}

Se hai delle trascrizioni di log di chat, da un centro di supporto clienti, ad esempio, che contengono delle espressioni utente reali, consenti al tuo servizio di estrarre i tuoi dati esistenti per i candidati sull'esempio utente dell'intento.

## Carica i tuoi file
{: #intent-recommendations-log-files-add}

Prima di cominciare, crea un file da fornire al servizio. Il file deve essere un file CSV (comma-separated value), con un'espressione utente per riga. Idealmente, le espressioni sono frasi brevi che vengono estratte dalle tue trascrizioni del call center che contengono domande e richieste reali del cliente. Ciascun file dell'espressione utente può avere una dimensione massima di 20 MB. 

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

Tutti i file che hai caricato vengono condivisi tra tutte le capacità nell'istanza del servizio corrente. Le espressioni che provengono da tutti i file disponibili vengono estratte quando richiedi consigli sull'esempio utente dell'intento. 

## Ottieni i consigli sull'esempio utente dell'intento 
{: #intent-recommendations-get-example-recommendations}

Il seguente video fornisce una panoramica di 2 minuti su come ottenere consigli sull'esempio utente dell'intento. 

<iframe class="embed-responsive-item" id="youtubeplayer" title="Consigli sull'esempio utente dell'intento" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/L3FI8KeZfsc?rel=0" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

I file che carichi non hanno bisogno di associare gli esempi agli intenti. Fornisci semplicemente le espressioni utente non elaborate e lascia che il servizio scelga quelle appropriate per l'intento corrente. Per ogni intento, il servizio analizza gli stessi dati per trovare gli esempi utente appropriati per tale intento. 

1.  Segui i passi presenti in [Creazione di intenti](/docs/services/assistant?topic=assistant-intents#intents-creating-intents-task) per creare un intento. 

1.  Aggiungi almeno 5 esempi utente che mostrano la gamma completa di espressioni tipiche che sai che gli utenti potrebbero utilizzare per attivare questo intento. 

    Questi esempi utente seed istruiscono il servizio sui tipi di espressioni da ricercare nei file che hai caricato. 

1.  Fai clic su **Show recommendations**.

    I file di origine degli esempi utente sono condivisi tra le capacità in un'istanza del servizio. Se i tuoi collaboratori possiedono delle capacità nella stessa istanza e caricano dei file, i loro file verranno aggiunti alla tua raccolta *User example source files* condivisa. 

1.  **Solo la prima volta**: fai clic su **Upload files** e poi fai clic su **Choose a file** per ricercare il file CSV che hai creato in precedenza e selezionalo. 

    Va bene se il file che hai caricato contiene le espressioni per tutti i tipi di intenti. Il servizio sa su quali intenti stai lavorando e trova gli esempi adatti da consigliare per questo intento specifico. 

    Una volta che il file viene caricato ed elaborato dal servizio, vengono visualizzate le espressioni consigliate. Se non vi sono consigli, il file non contiene esempi adatti per questo intento. 

1.  Se il file non fornisce consigli utili per nessuno dei tuoi intenti, puoi provare con una serie diversa di espressioni caricando un altro file. Per ulteriori dettagli, consulta [Gestione dei file caricati](#intent-recommendations-manage-files).

1.  Una volta che il servizio ti mostra i consigli, seleziona le espressioni che desideri aggiungere come esempi utente per questo intento e poi fai clic su **Add**. Oppure fai clic su **Next set** per esaminare altre espressioni. 
1.  Se desideri ricercare tu stesso gli esempi utente nel contenuto del file CSV, fai clic sulla scheda **Search Logs**, immetti una parola chiave in base alla quale eseguire la ricerca e quindi premi **Enter**. 

    Segui queste linee guida per la sintassi della query di ricerca: 

    - Sono supportati gli operatori booleani (ad esempio `AND` e `OR`). 
    - Aggiungi il testo tra virgolette per ricercare una corrispondenza esatta del testo ("thisstringmustbepresent"). 
    - Puoi utilizzare le espressioni regolari, ad esempio `*ly` per trovare tutti i termini che finiscono con `ly`. 
    - I seguenti caratteri vengono utilizzati come operatori di espressioni regolari: 

      `+ - = && || > < ! ( ) { } [ ] ^ " ~ * ? : \ /`

      Se desideri includerne uno in un termine di ricerca senza che venga elaborato come un operatore, devi aggiungere una barra rovesciata (`\`) come prefisso. 

Gli esempi utente che aggiungi in questo modo vengono conteggiati nei tuoi totali degli esempi utente dell'intento per i quali sono stabiliti dei limiti per piano. Per ulteriori dettagli, vedi [Limiti di intenti](/docs/services/assistant?topic=assistant-intents#intents-limits).

## Gestione dei file caricati
{: #intent-recommendations-manage-files}

Una volta che hai caricato almeno un file, puoi aprire la raccolta *User example source files* per gestire i tuoi file. I file caricati vengono aggiunti a una raccolta di file che viene condivisa per tutta l'istanza del servizio {{site.data.keyword.conversationshort}}. 

1.  Per accedere alla raccolta, fai clic per aprire un intento, fai clic su **Show recommendations** e quindi fai clic su **View Files** dalla barra laterale. 

1.  Puoi eseguire le seguenti azioni: 

    - Per aggiungere un file, fai clic su **Add Files** e poi ricerca un file e selezionalo. 
    - Per eliminare un file, devi rimuovere tutti i file che sono stati caricati da qualsiasi capacità associata a questa istanza del servizio; non puoi eliminare solo un file. Innanzitutto, assicurati che nessun altro stia utilizzando i file, poi fai clic su **Delete all** per eliminare tutti i file caricati. 

1.  Chiudi la pagina *User example source files*. 
