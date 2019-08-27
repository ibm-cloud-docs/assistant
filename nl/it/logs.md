---

copyright:
  years: 2015, 2019
lastupdated: "2019-06-12"

keywords: mark as irrelevant, counterexample, data source, deployment ID

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

# Migliora la tua capacità
{: #logs}

La pagina Analisi di {{site.data.keyword.conversationshort}} fornisce una cronologia di conversazioni tra gli utenti e un assistente distribuito. Puoi utilizzare questa cronologia per migliorare come i tuoi assistenti comprendono e rispondono alle richieste dell'utente.
{: shortdesc}

Per aprire un elenco di singoli messaggi tra i clienti e l'assistente che utilizza questa capacità di dialogo, seleziona **Conversazioni utente** nella barra di navigazione.

Quando apri la pagina **Conversazioni utente**, la vista predefinita elenca gli input che sono stati inviati all'assistente nell'ultimo giorno, con i risultati più recenti per primi. Sono disponibili i valori di intenti principali (#intent) e qualsiasi entità riconosciuta (@entity) utilizzati in un messaggio e il testo del messaggio. Per gli intenti che non sono riconosciuti, il valore mostrato è *Irrilevante*. Se un'entità non è riconosciuta o non è stata fornita, il valore mostrato è *Nessuna entità trovata*.


![Pagina predefinita Log](images/logs_page1.png)

La pagina Conversazioni utente visualizza il numero totale di *messaggi* tra i clienti e il tuo assistente. Un messaggio è una singola espressione che un utente invia all'assistente. Una conversazione normalmente è formata da più messaggi. Pertanto, il numero di risultati nella pagina **Conversazioni utente** è diverso dal numero di conversazioni mostrato nella pagina **Panoramica**.
{: important}

## Limiti di log
{: #logs-limits}

La durata prevista per la conservazione dei messaggi dipende dal tuo piano di servizio {{site.data.keyword.conversationshort}}:

  Piano di servizio                         | Conservazione messaggi di chat
  ------------------------------------ | ------------------------------------
  Premium                              | Ultimi 90 giorni
  Plus                                 | Ultimi 30 giorni
  Standard                             | Ultimi 30 giorni
  Plus Trial                           | Ultimi 30 giorni
  Lite                                 | Ultimi 7 giorni

## Filtro dei messaggi
{: #logs-filter-messages}

Puoi filtrare i messaggi in base ai valori *Cerca istruzioni utente*, *Intenti*, *Entità* e *Ultimi n giorni*.

*Cerca istruzioni utente* - Immetti una parola nella barra di ricerca. La ricerca viene effettuata negli input degli utenti ma non nelle risposte del tuo assistente.

*Intenti* - Seleziona il menu a discesa e immetti un intento nel campo di input o scegli una voce dall'elenco. Puoi selezionare più di un intento, che filtra i risultati utilizzando uno qualsiasi degli intenti selezionati, incluso *Irrilevante*.

![Menu a discesa Intenti](images/intents_filter.png)

*Entità* - Seleziona il menu a discesa e immetti un nome entità nel campo di input o scegli una voce dall'elenco. Puoi selezionare più di un'entità, che filtra i risultati in base a una qualsiasi entità selezionata. Se filtri per intento *ed* entità, i tuoi risultati includeranno i messaggi che hanno entrambi i valori. Puoi anche filtrare i risultati con *Nessuna entità trovata*.

![Menu a discesa Entità](images/entities_filter.png)

L'aggiornamento dei messaggi può richiedere del tempo. Attendi almeno 30 minuti dopo l'interazione di un utente con il tuo assistente prima di tentare di filtrare quel contenuto.

## Visualizzazione di singoli messaggi
{: #logs-see-message}

Per qualsiasi voce di input dell'utente, fai clic su **Apri conversazione** per visualizzare l'input dell'utente e la risposta restituita dall'assistente all'interno del contesto della conversazione completa.

L'ora mostrata all'inizio di ogni conversazione viene localizzata per rispecchiare il fuso orario del tuo browser. Questa volta può scostarsi dalla data/ora mostrata se controlli lo stesso log di conversazione tramite una chiamata API; le chiamate di log API vengono sempre mostrate in UTC.

![Pannello Apri conversazione](images/open_convo.png)

Puoi quindi scegliere di mostrare le classificazioni per il messaggio che hai selezionato.

![Pannello Apri conversazione con le classificazioni](images/open_convo_classes.png)

Se la funzione di controllo ortografico è abilitata per la capacità, tutte le espressioni dell'utente che sono state corrette vengono evidenziate dall'icona di correzione automatica. Il termine che è stato corretto viene sottolineato. Puoi passare con il mouse sul termine sottolineato per visualizzare l'input originale dell'utente.

![Pannello Apri conversazione che mostra il testo originale di un termine per cui è stata applicata la logica di correzione ortografica](images/open_convo_spellchecked.jpg)

## Miglioramento tra assistenti
{: #logs-deploy-id}

La creazione di una capacità di dialogo è un processo iterattivo. Mentre sviluppi la tua capacità, utilizza il pannello *Try it out* per verificare che il tuo assistente riconosca intenti ed entità corretti negli input di test e per apportare le correzioni necessarie. 

Dalla pagina Conversazioni utente, puoi analizzare interazioni reali tra l'assistente che hai utilizzato per distribuire la capacità e i tuoi utenti. In base a tali interazioni, puoi apportare delle correzioni per migliorare l'accuratezza con cui gli intenti e le entità vengono riconosciuti dalla tua capacità di dialogo. È difficile sapere esattamente *come* i tuoi utenti porranno le domande o quali messaggi casuali potrebbero inviare, per cui è importante analizzare frequentemente le conversazioni reali per migliorare le tue capacità di dialogo.

Per un'istanza {{site.data.keyword.conversationshort}} che include più assistenti, potrebbero verificarsi casi in cui è utile utilizzare i dati del messaggio dalla capacità di dialogo di un assistente per migliorare la capacità di dialogo utilizzata da un altro assistente all'interno della stessa istanza.

![Solo piano Premium](images/premium0.png) Se sei un utente {{site.data.keyword.conversationshort}} Premium, le tue istanze possono essere facoltativamente configurate per consentire l'accesso ai dati di log dagli assistenti tra diverse istanze premium.

Ad esempio, diciamo che hai un'istanza {{site.data.keyword.conversationshort}} denominata *HelpDesk*. Nella tua istanza HelpDesk potresti avere sia un assistente Produzione che un assistente Sviluppo. Quando utilizzi la capacità di dialogo per l'assistente Sviluppo, puoi utilizzare i log dai messaggi dell'assistente Produzione per migliorare la capacità di dialogo dell'assistente Sviluppo.

Tutte le modifiche che apporti nella capacità di dialogo per l'assistente Sviluppo, influenzeranno solo la capacità di dialogo dell'assistente Sviluppo, anche se stai utilizzando i dati dai messaggi inviati all'assistente Produzione.

Allo stesso modo, se crei più versioni di una capacità, potresti voler utilizzare i dati del messaggio da una versione per migliorare i dati di apprendimento di un'altra versione.

### Selezione di un'origine dati
{: #logs-pick-data-source}

Il termine *origine dati* si riferisce ai log compilati dalle conversazioni tra i clienti e l'assistente o l'applicazione personalizzata da cui è stata distribuita una capacità di dialogo.

Quando apri la scheda *Analisi*, vengono mostrate le metriche che sono state generate dalle interazioni dell'utente con la capacità di dialogo corrente. Non viene mostrata alcuna metrica se la capacità corrente non è stata distribuita e utilizzata dai clienti.

Per popolare le metriche con i dati del messaggio da una capacità di dialogo o dalla versione della capacità che è stata aggiunta a un assistente o a un'applicazione personalizzata differente, uno che ha interagito con i clienti, completa questa procedura:

1.  Fai clic sul campo **Origine dati** per visualizzare un elenco di assistenti con i dati di log che potresti voler utilizzare.

    L'elenco include gli assistenti che sono stati distribuiti e a cui hai accesso. Vedi [*Mostra gli ID di distribuzione* spiegati](#logs-deployment-id-explained) per ulteriori informazioni su tale opzione.

1.  Scegli un'origine dati.

Vengono visualizzate le informazioni statistiche per l'origine dati selezionata.

Tieni presente che l'elenco non include le versioni della capacità. Per ottenere i dati associati a una versione della capacità specifica, devi conoscere l'intervallo di tempo durante il quale è stata utilizzata una versione della capacità specifica da un assistente di distribuzione. Puoi selezionare l'assistente come l'origine dati e poi filtrare i dati delle metriche in base alle date appropriate.

### *Mostra gli ID di distribuzione* spiegati
{: #logs-deployment-id-explained}

Le applicazioni che utilizzano la versione V1 dell'API devono specificare un ID di distribuzione in ogni messaggio inviato utilizzando l'API `/message`. Questo ID identifica l'applicazione distribuita da cui è stata effettuata la chiamata. La pagina Analisi può utilizzare questo ID di distribuzione per richiamare e visualizzare i log associati a un'applicazione live specifica.

Per gli assistenti o le applicazioni personalizzate che utilizzano la versione V2 dell'API, il tuo assistente include automaticamente un ID di sistema e l'ID della capacità con ogni chiamata /message, in modo che puoi scegliere un'origine dati in base al nome dell'assistente invece di utilizzare un ID di distribuzione.

Per aggiungere l'ID di distribuzione, gli utenti dell'API V1 includono la proprietà di distribuzione all'interno dei metadati del [contesto ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://cloud.ibm.com/apidocs/assistant?curl=#get-response-to-user-input){: new_window}, come in questo esempio:

```json
"context" : {
  "metadata" : {
       "deployment": "HelpDesk-Production"
  }
}
```
{: codeblock}

## Apportare dei miglioramenti ai dati di apprendimento
{: #logs-fix-data}

Utilizza le informazioni approfondite dalle conversazioni utente reali per correggere il modello associato alla tua capacità di dialogo.

Se utilizzi i dati da un'altra origine dati, tutti i miglioramenti che apporti al modello vengono applicati solo alla capacità di dialogo corrente. Il campo **Origine dati** mostra l'origine dei messaggi che stai utilizzando per migliorare questa capacità di dialogo e la parte superiore della pagina mostra la capacità di dialogo a cui stai applicando le modifiche.

### Correzione di un intento
{: #logs-correct-intent}

1.  Per correggere un intento, seleziona l'icona di modifica ![Modifica](images/edit_icon.png) accanto all'#intent scelto.
1.  Dall'elenco fornito, seleziona l'intento corretto per questo input.
    - Inizia a scrivere nel campo di immissione e l'elenco di intenti verrà filtrato.
    - Puoi anche scegliere **Contrassegna come irrilevante** da questo menu. (Per ulteriori informazioni, vedi [Istruisci il tuo assistente sugli argomenti da ignorare](#logs-mark-irrelevant).) In alternativa, puoi scegliere **Non addestrare sull'intento**, che non salva questo messaggio come esempio per l'addestramento.

    ![Seleziona intento](images/select_intent.png)
1.  Seleziona **Salva**.

    ![Salva intento](images/save_intent.png)

    Il servizio {{site.data.keyword.conversationshort}} supporta l'aggiunta di input utente come un esempio ad un intento *così com'è*. Se stai utilizzando riferimenti @entity come esempi nei tuoi dati di addestramento dell'intento e un messaggio utente che desideri salvare contiene un sinonimo o un valore di entità proveniente dai tuoi dati di addestramento, devi modificare il messaggio in un secondo momento. Una volta salvato, modifica il messaggio dalla pagina Intenti per sostituire l'entità a cui fa riferimento. Per ulteriori informazioni, vedi [Riferimento diretto a @Entity come un esempio di intento](/docs/services/assistant?topic=assistant-intents#intents-entity-as-example).
    {: tip}

### Aggiunta di un valore o un sinonimo di entità
{: #logs-add-entity}

1.  Per aggiungere un valore o un sinonimo di entità, seleziona l'icona di modifica ![Modifica](images/edit_icon.png) accanto all'@entity scelta.
1.  Seleziona **Aggiungi entità**.

    ![Aggiungi entità](images/add_entity.png)
1.  Ora, seleziona una parola o una frase nell'input utente sottolineato.

    ![Seleziona entità](images/select_entity.png)
1.  Scegli un'entità a cui la frase evidenziata verrà aggiunta come valore.
    - Inizia a scrivere nel campo di immissione e l'elenco di entità e valori verrà filtrato.
    - Per aggiungere la frase evidenziata come sinonimo di un valore esistente, scegli `@entity:value` dall'elenco a discesa.

    ![Aggiungi parola di entità](images/add_entity_word.png)
1.  Seleziona **Salva**.

    ![Salva entità](images/add_entity_save.png)

### Istruisci il tuo assistente sugli argomenti da ignorare 
{: #logs-mark-irrelevant}

È importante aiutare il tuo assistente a rimanere focalizzato sui tipi di domande e transazioni di business del cliente che hai progettato gestisca. Puoi utilizzare le informazioni raccolte dalle conversazioni del cliente reali per evidenziare i soggetti che non vuoi il tuo assistente tenti di affrontare.

Per istruire il tuo assistente sui soggetti che dovrebbe ignorare, segna le espressioni che parlano di questi soggetti fuori argomento come irrilevanti.

L'opzione **Contrassegna come irrilevante** non è disponibile in tutte le lingue. Per i dettagli, vedi le [lingue supportate](/docs/services/assistant?topic=assistant-language-support).

Gli intenti contrassegnati come irrilevanti vengono salvati come esempi contatori (counterexamples) nello spazio di lavoro JSON e vengono inclusi come parte dei dati di addestramento. Istruiscono il tuo assistente per non rispondere in modo esplicito alle espressioni di questo tipo.

Devi essere sicuro prima di designare un input come irrilevante.

- Non esiste modo per accedere o modificare gli input dall'interfaccia utente in un secondo momento.
- L'unico modo di invertire l'identificazione di un input dall'essere irrilevante è di riutilizzare lo stesso input in un canale di integrazione di test e poi assegnarlo in modo esplicito a un intento.

Puoi contrassegnare un intento come irrilevante anche direttamente dal pannello *Try it out*.

  ![Schermata di Contrassegna come irrilevante](images/irrelevant.png)
