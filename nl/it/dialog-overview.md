---

copyright:
  years: 2015, 2018
lastupdated: "2018-02-09"

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
{:table: .aria-labeledby="caption"}

# Panoramica del dialogo
{: #dialog-overview}

Il dialogo utilizza gli intenti e le entità che vengono identificati nell'input dell'utente, oltre al contesto ottenuto dall'applicazione, per interagire con l'utente e infine fornire una risposta utile.
{: shortdesc}

La risposta potrebbe essere quella fornita per una domanda del tipo `Dove posso fare benzina?` o l'esecuzione di un comando, come ad esempio l'accensione della radio. L'intento e l'entità potrebbero essere informazioni sufficienti per identificare la risposta corretta o il dialogo potrebbe chiedere all'utente un ulteriore input necessario per rispondere correttamente. Ad esempio, se un utente chiede `Dove posso prendere qualcosa da mangiare?` potresti voler chiarire se vuole un ristorante o un supermercato, in loco o da asporto e così via. Puoi chiedere ulteriori dettagli in una riposta di testo e creare uno o più nodi figlio per elaborare il nuovo input.

<iframe class="embed-responsive-item" id="youtubeplayer" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/oQUpejt6d84?rel=0" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

Questo dialogo viene rappresentato graficamente nello strumento {{site.data.keyword.conversationshort}} come una struttura ad albero. Crea un ramo per elaborare ogni intento che desideri venga gestito dalla conversazione. Un ramo è composto da più nodi.

## Nodi del dialogo

Ogni nodo di dialogo contiene, come minimo, una condizione e una risposta.

![Mostra l'input utente che va a una casella che contiene l'istruzione If: CONDITION, Then: RESPONSE](images/node1-empty.png)

- Condizione: specifica le informazioni che devono essere presenti nell'input utente affinché questo nodo nel dialogo venga attivato. Le informazioni potrebbero essere un intento specifico, un valore di entità o un valore di variabile di contesto. Per ulteriori informazioni, vedi [Condizioni](dialog-runtime.html#conditions).
- Risposta: l'espressione utilizzata dal servizio per rispondere all'utente. La risposta può anche essere configurata per attivare azioni programmatiche. Per ulteriori informazioni, vedi [Risposte](#responses).

Il nodo può essere considerato come avente una costruzione if/then: se la condizione è true, allora restituirà questa risposta.

Ad esempio, il seguente nodo viene attivato se la funzione di elaborazione del linguaggio naturale del servizio determina che l'input utente contiene l'intento `#cupcake-menu`. Come risultato del nodo che viene attivato, il servizio risponde con una risposta appropriata.

![Mostra l'utente che chiede dei sapori di cupcake, la condizione If è #cupcake-menu e la riposta Then è un elenco dei sapori di cupcake.](images/node1-simple.png)

Un singolo nodo con una sola condizione e risposta può gestire semplici richieste dell'utente. Ma, il più delle volte, gli utenti hanno domande più sofisticate o desiderano aiuto con attività più complesse. Puoi aggiungere dei nodi figlio che richiedano all'utente di fornire qualsiasi ulteriore informazione richiesta dal servizio.

![Mostra che il primo nodo nel dialogo chiede quale tipo di cupcake l'utente desidera, senza glutine o normale, e ha due nodi figlio che forniscono una risposta diversa a seconda della risposta dell'utente.](images/node1-children.png)

## Flusso del dialogo

Il dialogo che crei viene elaborato dal servizio dal primo nodo nella struttura ad albero all'ultimo. 

![Freccia rivolta verso il basso accanto ai 3 nodi per mostrare che il dialogo scorre dal primo nodo all'ultimo](images/node-flow-down.png)

Mentre scende lungo la struttura ad albero, se il servizio trova una condizione che viene soddisfatta, attiverà tale nodo. Si sposta quindi insieme al nodo attivato per controllare l'input utente e verificare la presenza di eventuali condizioni del nodo figlio. Mentre controlla i nodi figlio, si sposta di nuovo dal primo nodo figlio all'ultimo.

Il servizio continua il suo percorso attraverso la struttura ad albero di dialogo dal primo all'ultimo nodo, insieme a ciascun nodo attivato, quindi dal primo all'ultimo nodo figlio, insieme a ciascun nodo figlio attivato finché non raggiunge l'ultimo nodo nel ramo che sta seguendo. 

![Mostra la freccia 1 rivolta dal primo nodo root all'ultimo, la freccia 2 che percorre la lunghezza di un nodo attivato e la freccia 3 rivolta dal primo all'ultimo dei nodi figlio del nodo attivato.](images/node-flow.png)

Quando inizi a creare il dialogo, devi stabilire i rami da includere e dove posizionarli. L'ordine dei rami è importante in quanto i nodi vengono valutati dal primo all'ultimo. Viene utilizzato il primo nodo root la cui condizione corrisponde all'input; tutti i nodi che seguono nella struttura ad albero non vengono attivati.

Quando il servizio raggiunge la fine di un ramo oppure non può trovare una condizione valutata come true nella serie corrente di nodi figlio che sta valutando, ritorna alla base della struttura ad albero. E ancora una volta, il servizio elabora i nodi root dal primo all'ultimo. Se nessuna condizione viene valutata come true, viene restituita la risposta proveniente dall'ultimo nodo nella struttura ad albero che di solito contiene una condizione speciale `anything_else` che viene sempre valutata come true.

Puoi interrompere il flusso standard dal-primo-all'ultimo personalizzando le operazioni da eseguire una volta elaborato un nodo. Ad esempio, puoi configurare un nodo in modo che passi direttamente ad un altro nodo una volta elaborato anche se l'altro nodo è posizionato prima nella struttura ad albero. Per ulteriori dettagli, vedi [Definizione delle operazioni successive](dialog-overview.html#jump-to).

Il modo in cui configuri le impostazioni di digressione per ciascun nodo influisce anche su come gli utenti si spostano tra i nodi nel runtime. Se abiliti la generazione delle digressioni dalla maggior parte dei nodi, gli utenti potranno passare da un nodo all'altro più facilmente. Per ulteriori informazioni, vedi [Digressioni](dialog-runtime.html#digressions).

## Condizioni
{: #conditions}

Una condizione del nodo determina se il nodo viene utilizzato nella conversazione. Le condizioni della risposta determinano la risposta da visualizzare all'utente.

- [Risorse della condizione](dialog-overview.html#condition-artifacts)
- [Dettagli della sintassi della condizione](dialog-overview.html#condition-syntax)
- [Suggerimenti sull'utilizzo delle condizioni](dialog-overview.html#condition-tips)

### Risorse della condizione
{: #condition-artifacts}

Per definire una condizione, puoi utilizzare una o più delle seguenti risorse in qualsiasi combinazione:

- **Variabile di contesto**: il nodo viene utilizzato se l'espressione di variabili di contesto da te specificata è true. Utilizza la sintassi `$variable_name:value` o `$variable_name == 'value'`. Ad esempio, `$city:Boston` controlla se la variabile di contesto `$city` contiene il valore `Boston`. Se è così, la risposta o il nodo viene elaborato. 

  Non definire una condizione di nodo o riposta in base al valore di una variabile di contesto nello stesso nodo di dialogo in cui imposti il valore della variabile di contesto.
  {: tip}

  Per ulteriori informazioni sulle variabili di contesto, vedi [Variabili di contesto](dialog-runtime.html#context).

- **Entità**: il nodo viene utilizzato quando qualsiasi valore o sinonimo dell'entità viene riconosciuto nell'input utente. Utilizza la sintassi `@entity_name`. Ad esempio, `@city` controlla se i nomi di città definiti per l'entità @city sono stati rilevati nell'input utente. Se è così, la risposta o il nodo viene elaborato. 

  Assicurati di creare un nodo peer per gestire il caso in cui nessuno dei valori o sinonimi dell'entità venga riconosciuto.
  {: tip}

  Per ulteriori informazioni sulle entità, vedi [Definizione di entità](entities.html).

- **Valore entità**: il nodo viene utilizzato se il valore dell'entità viene rilevato nell'input utente. Utilizza la sintassi `@entity_name:value` e specifica un valore definito per l'entità, non un sinonimo. Ad esempio: `@city:Boston` controlla se il nome di città indicato, `Boston`, è stato rilevato nell'input utente. 

  Se l'entità è un'entità modello con gruppi di acquisizione, puoi controllare la presenza di una determinata corrispondenza del valore del gruppo. Ad esempio puoi utilizzare la sintassi: `@us_phone.groups[1] == '617'`
  Per ulteriori informazioni, vedi [Memorizzazione dei valori dell'entità modello nelle variabili di contesto](dialog-runtime.html#context-pattern-entities).

  Se controlli la presenza dell'entità, senza specificarne un particolare valore, in un nodo peer, assicurati di posizionare questo nodo (che controlla la presenza di un particolare valore dell'entità) prima del nodo peer che controlla solo la presenza dell'entità. In caso contrario, questo nodo non verrà mai valutato.
  {: tip}

- **Intento**: la condizione più semplice è un singolo intento. Il nodo viene utilizzato se l'input dell'utente viene associato a tale intento. Utilizza la sintassi `#intent_name`. Ad esempio, `#weather` controlla se l'intento rilavato nell'input utente è `weather`. Se è così, il nodo viene elaborato.

  Per ulteriori informazioni sugli intenti, vedi [Definizione di intenti](intents.html).

- **Condizione speciale**: condizioni fornite con il servizio che puoi utilizzare per eseguire funzioni di dialogo comuni.

| Sintassi delle condizioni     | Descrizione |
|----------------------|-------------|
| `anything_else`      | Puoi utilizzare questa condizione alla fine di un dialogo, affinché venga elaborata quando l'input utente non corrisponde a nessun altro nodo di dialogo. Il nodo **Altro** viene attivato da questa condizione. |
| `conversation_start` | Come **welcome**, questa condizione viene valutata come true durante il primo turno di dialogo. A differenza di **welcome**, è true a prescindere che la richiesta iniziale dall'applicazione contenga o meno l'input utente. Un nodo con la condizione **conversation_start** può essere utilizzato per inizializzare le variabili di contesto o per eseguire altre attività all'inizio del dialogo. |
| `false`              | Questa condizione viene sempre valutata come false. Potresti utilizzarla all'inizio di un ramo in fase di sviluppo, per impedire che venga utilizzato, o come condizione per un nodo che fornisce una funzione comune ed è utilizzato solo come destinazione di un'azione **Passa a**.|
| `irrelevant`         | Questa condizione viene valutata come true se l'input dell'utente viene considerato irrilevante dal servizio {{site.data.keyword.conversationshort}}. |
| `true`               | Questa condizione viene sempre valutata come true. Puoi utilizzarla alla fine di un elenco di nodi o risposte per recuperare eventuali risposte che non corrispondono a nessuna delle condizioni precedenti. |
| `welcome`            | Questa condizione viene valutata come true durante il primo turno di dialogo (all'inizio della conversazione), solo se la richiesta iniziale dall'applicazione non contiene alcun input utente. Viene valutata come false in tutti i turni di dialogo successivi. Il nodo **Benvenuto** viene attivato da questa condizione. In genere, un nodo con questa condizione viene utilizzato per salutare l'utente, ad esempio, per visualizzare un messaggio come `Benvenuto alla nostra applicazione di ordinazione della pizza.`|
{: caption="Condizioni speciali" caption-side="top"}

### Dettagli della sintassi della condizione
{: #condition-syntax}

Utilizza una di queste opzioni di sintassi per creare espressioni valide nelle condizioni:

- Notazioni abbreviate che si riferiscono a intenti, entità e variabili di contesto. Vedi [Accesso e valutazione degli oggetti](expression-language.html).

- Linguaggio Spring Expression (SpEL), che è un linguaggio delle espressioni che supporta l'esecuzione di query e la manipolazione di un grafico di oggetti in fase di runtime. Per ulteriori informazioni, vedi [Linguaggio Spring Expression Language (SpEL)![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](http://docs.spring.io/spring/docs/current/spring-framework-reference/html/expressions.html){: new_window}.

Puoi utilizzare le espressioni regolari per controllare i valori nella condizione.  Per trovare una stringa corrispondente, puoi utilizzare ad esempio il metodo `String.find`. Per ulteriori dettagli, vedi [Metodi](dialog-methods.html).

### Suggerimenti sull'utilizzo delle condizioni
{: #condition-tips}

- **Controllo dei valori con caratteri speciali**: se desideri controllare se un'entità o una variabile di contesto contiene un valore e se il valore include un carattere speciale, ad esempio un apostrofo ('), devi racchiudere tra parentesi il valore che desideri controllare. Ad esempio, per controllare se un'entità o una variabile di contesto contiene il nome `O'Reilly`, devi racchiudere il nome tra parentesi.

  `@person:(O'Reilly)` e `$person:(O'Reilly)`

  Il servizio converte questi riferimenti abbreviati in queste espressioni SpEL complete:

  `entities['person']?.contains('O''Reilly')` e `context['person'] == 'O''Reilly'`

  **Nota**: SpEL utilizza un secondo apostrofo per eseguire l'escape del singolo apostrofo nel nome. 

- **Controllo dei valori numerici**: quando utilizzi variabili numeriche, assicurati che le variabili abbiano dei valori. Se una variabile non ha un valore, viene considerata come avente un valore null (0) in un confronto numerico.

  Ad esempio, se controlli il valore di una variabile con la condizione `@price < 100`e l'entità @price è null, la condizione verrà valutata come `true` perché 0 è inferiore a 100, anche se il prezzo non è stato mai impostato. Per impedire il controllo delle variabili null, utilizza una condizione come `@price AND @price < 100`. Se `@price` non ha alcun valore, questa condizione restituirà correttamente false. 

- **Controllo degli intenti con uno specifico modello di nome intento**: puoi utilizzare una condizione che ricerca gli intenti che corrispondono ad un modello. Ad esempio, per trovare tutti gli intenti rilevati con nomi intento che iniziano con 'User_', nella condizione puoi utilizzare una sintassi come quella riportata di seguito:

  `intents[0].intent.startsWith("User_")`

  Tuttavia, quando esegui questa operazione, vengono considerati tutti gli intenti rilevati, anche quelli con una affidabilità inferiore a 0,2. Inoltre, tieni presente che gli intenti ritenuti irrilevanti da Watson in base al loro punteggio di affidabilità non vengono restituiti. Affinché questo avvenga, modifica la condizione come segue: 

  `!irrelevant && intents[0].intent.startsWith("User_")`

- **In che modo la corrispondenza fuzzy influisce sul riconoscimento dell'entità**: se utilizzi un'entità come condizione ed è abilitata la corrispondenza fuzzy, `@entity_name` viene valutato su true solo se l'affidabilità della corrispondenza è superiore al 30%.  Ossia, solo se `@entity_name.confidence > .3`. 

- **Gestione di più entità nell'input**: se vuoi valutare solo il valore della prima istanza rilevata di un tipo di entità, puoi utilizzare la sintassi  `@entity == 'specific-value'` invece del formato `@entity:(specific-value)`. 

  Ad esempio, quando utilizzi `@appliance == 'air conditioner'`, valuti solo il valore della prima entità `@appliance` rilevata. Ma, l'utilizzo di `@appliance:(air conditioner)` viene esteso in `entity['appliance'].contains('air conditioner')`, che corrisponde ogni volta che viene rilevata almeno un'entità `@appliance` di valore 'air conditioner' nell'input utente.

## Risposte
{: #responses}

La risposta del dialogo definisce come rispondere all'utente.

Puoi rispondere con uno di questi tipi di risposta:

- [Risposta di testo semplice](#simple-text)
- [Risposte condizionali](#multiple)
- [Risposta complessa](#complex)
- [Risposta multimediale](#multimedia)

### Risposta di testo semplice
{: #simple-text}

Se vuoi fornire una risposta di testo, immetti semplicemente il testo che vuoi che il servizio visualizzi all'utente.

![Mostra un nodo che visualizza la domanda dell'utente, Dove vi trovate, e la risposta del dialogo è, Non abbiamo negozi fisici! Ma con una connessione internet, puoi fare acquisti da qualsiasi luogo.](images/response-simple.png)

Se includi un indirizzo email nella risposta, devi eseguire l'escape del simbolo chiocciola (`@`) con una barra rovesciata (`\`). Ad esempio, `Inviaci il tuo feedback all'indirizzo feedback\@example.com.` Allo stesso modo, se includi un simbolo cancelletto (`#`) nella risposta, devi eseguirne l'escape. Ad esempio, `Siamo i venditori \#1 di panini all'astice nel Maine.` I nomi entità iniziano con `@` e i nomi intento iniziano con `#`. Se esegui l'escape di questi simboli, impedisci al servizio di fraintendere il testo della risposta.
{: tip}

#### Aggiunta di varietà
{: #variety}

Se i tuoi utenti utilizzano spesso il tuo servizio di conversazione, potrebbero essere stanchi di ascoltare ogni volta gli stessi messaggi iniziali e le stesse risposte.  Puoi aggiungere delle *variazioni* alle tue risposte in modo che la conversazione possa rispondere alla stessa condizione in modi diversi.

<iframe class="embed-responsive-item" id="youtubeplayer" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/nAlIW3YPrAs?rel=0" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

In questo esempio, la risposta che il servizio fornisce alle domande relative alle posizioni del negozio differisce da un'interazione all'altra.

![Mostra un nodo che visualizza la domanda dell'utente, Dove vi trovate, e il dialogo ha tre diverse risposte definite.](images/variety.png)

Puoi scegliere di ruotare le variazioni di risposta in sequenza o in ordine casuale. Per impostazione predefinita, le risposte vengono ruotate in sequenza, come se fossero scelte da un elenco ordinato.

### Risposte condizionali
{: #multiple}

Un singolo nodo di dialogo può fornire risposte diverse, ognuna attivata da una diversa condizione.  Utilizza questo approccio per risolvere più scenari in un singolo nodo.

<iframe class="embed-responsive-item" id="youtubeplayer" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/KcvVQAsnhLM?rel=0" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

Il nodo ha ancora una condizione principale, che è la condizione per l'utilizzo del nodo e l'elaborazione delle condizioni e delle risposte che contiene.

In questo esempio, il servizio utilizza le informazioni sulla posizione dell'utente raccolte in precedenza per personalizzare la risposta e fornire informazioni sul negozio più vicino all'utente. Vedi [Variabili di contesto](dialog-runtime.html#context) per ulteriori informazioni su come memorizzare le informazioni raccolte dall'utente.

![Mostra un nodo che visualizza la domanda dell'utente, Dove vi trovate, e il dialogo ha tre risposte diverse a seconda delle condizioni che utilizzano le informazioni dalla variabile di contesto $state per specificare le posizioni in quegli stati.](images/multiple-responses.png)

Questo singolo nodo fornisce ora la funzione equivalente di quattro nodi separati.

Per aggiungere risposte condizionali ad un nodo, fai clic su **Personalizza** e quindi fai clic sull'interruttore **Risposte multiple** per **attivarlo**.

Le condizioni all'interno di un nodo vengono valutate in ordine, proprio come i nodi.  Assicurati che le tue condizioni e risposte siano elencate nell'ordine corretto.  Se devi modificare l'ordine, seleziona una condizione e spostala in alto o in basso nell'elenco utilizzando le frecce visualizzate. Se vuoi aggiornare il contesto, devi farlo nell'editor JSON per ogni singola risposta. Non esiste un editor JSON comune per tutte le risposte. Se hai associato un'azione **Passa a** al nodo, il passaggio non avverrà fino a quando non verranno elaborate le risposte.
{: tip}

### Risposta complessa
{: #complex}

Per specificare una risposta più complessa, puoi utilizzare l'editor JSON per specificare la risposta nella proprietà `"output":{}`.

Per includere un valore di variabile di contesto nella risposta, utilizza la sintassi `$variable-name` per specificarlo. Per ulteriori informazioni, vedi [Variabili di contesto](dialog-runtime.html#context).

```json
{
  "output":{
    "text": "Hello $user"
  }
}
```
{: codeblock}

Per specificare più di un'istruzione da visualizzare in righe separate, definisci l'output come un array JSON.

```json
{
  "output":{
    "text": ["Hello there.", "How are you?"]
  }
}
```
{: codeblock}

La prima frase viene visualizzata su una riga e la seconda frase viene visualizzata come nuova riga dopo di essa.

Per implementare un comportamento più complesso, puoi definire il testo di output come un oggetto JSON complesso. Ad esempio, puoi utilizzare un oggetto complesso nell'output JSON per simulare il comportamento di aggiungere variazioni di risposta al nodo. Nell'oggetto complesso, puoi includere le seguenti proprietà:

- **values**: un array di stringhe JSON che contiene più versioni del testo di output che può essere restituito da questo nodo di dialogo. L'ordine in cui vengono restituiti i valori nell'array dipende dall'attributo `selection_policy`.

- **selection_policy**: sono validi i seguenti valori:

    - **random**: il sistema seleziona in modo casuale il testo di output dall'array di `values` e non li ripete in modo consecutivo. Ad esempio, considera output.text che contiene tre valori. Per le prime tre volte, viene selezionato un valore casuale ma non viene ripetuto un'altra volta. Dopo che sono stati forniti tutti i valori di output, il sistema seleziona in modo casuale un altro valore e ripete il processo.

        ```json
        {
            "output":{
                "text":{
                    "values":["Hello.","Hi.","Howdy!"],
                    "selection_policy":"random"
                }
            }
        }
        ```
        {: codeblock}

    Il sistema restituisce un messaggio iniziale da queste tre opzioni che sceglie a caso. La prossima volta che viene attivata la risposta, viene visualizzato un altro messaggio iniziale dall'elenco. Il messaggio iniziale viene di nuovo scelto a caso, salvo che il messaggio utilizzato in precedenza non sia intenzionalmente ripetuto.

    - **sequential**: il sistema restituisce il primo testo di output la prima volta che il nodo di dialogo viene attivato, il secondo testo di output la seconda volta che il nodo viene attivato e così via.

        ```json
        {
            "output":{
                "text":{
                    "values":["Hello.", "Hi.", "Howdy!"],
                    "selection_policy":"sequential"
                }
            }
        }
        ```
        {: codeblock}

- **append**: specifica se aggiungere un valore a un array o sovrascrivere i valori nell'array con uno o più nuovi valori. Se impostato su false, l'output raccolto nei nodi di dialogo eseguiti in precedenza viene sovrascritto dal valore di testo specificato in questo determinato nodo.

    ```json
    {
        "output":{
            "text":{
                "values": ["Hello."],
                "append":false
            }
        }
    }
    ```
    {: codeblock}

    In questo caso, tutto l'altro testo di output viene sovrascritto da questo testo di output.

Il comportamento predefinito presuppone `selection_policy = random` e `append = true`. Quando l'array di valori contiene più di una voce, il testo di output viene selezionato casualmente dai suoi elementi.

### Risposta multimediale
{: #multimedia}

Se intendi integrare il dialogo con Slack o Facebook Messenger utilizzando il connettore {{site.data.keyword.conversationshort}}, puoi specificare risposte del nodo di dialogo che includano elementi multimediali o interattivi come ad esempio i pulsanti su cui è possibile fare clic.

Per ulteriori informazioni, vedi [Risposte multimediali](dialog-multimedia.html).

## Definizione delle operazioni successive
{: #jump-to}

Dopo aver eseguito la risposta specificata, puoi istruire il servizio affinché effettui una delle seguenti operazioni:

- **Attendi input utente**: il servizio attende che l'utente fornisca un nuovo input indotto dalla risposta. Ad esempio, la risposta potrebbe chiedere all'utente una domanda "sì o no". Il dialogo non avanzerà finché l'utente non fornisce altro input.
- **Ignora input utente**: utilizza questa opzione quando desideri ignorare l'attesa dell'input utente e vuoi che la conversazione vada invece direttamente al primo nodo figlio del nodo corrente. 

  **Nota**: il nodo corrente deve avere almeno un nodo figlio affinché questa opzione sia disponibile. 

- **Passa a un altro nodo di dialogo**: utilizza questa opzione quando desideri ignorare l'attesa dell'input utente e vuoi che la conversazione vada direttamente a un nodo di dialogo completamente diverso. Puoi utilizzare un'azione *Passa a*, ad esempio, per instradare il flusso in un nodo di dialogo comune da più posizioni nella struttura ad albero. 

  **Nota**: il nodo di destinazione a cui vuoi passare deve esistere prima di poter configurare e quindi utilizzare l'azione Passa a. 

### Configurazione dell'azione Passa a
{: #jump-to-config}

Se scegli di passare a un altro nodo, devi specificare se l'azione è destinata alla **risposta** o alla **condizione** del nodo di dialogo selezionato.

- **Risposta**: se l'istruzione è destinata alla sezione della risposta del nodo di dialogo selezionato, viene eseguita immediatamente. Ovvero, il sistema non valuta la condizione del nodo di dialogo selezionato, esegue immediatamente la risposta del nodo di dialogo selezionato. 

  La specifica della risposta è utile per concatenare insieme più nodi di dialogo. La risposta viene elaborata come se la condizione di questo nodo di dialogo fosse true. Se il nodo di dialogo selezionato ha un'altra azione **Passa a**, anche tale azione viene eseguita immediatamente.

- **Condizione**: se l'istruzione è destinata alla sezione della condizione del nodo di dialogo selezionato, il servizio verifica prima se la condizione del nodo di destinazione viene valutata come true. 
    - Se la condizione viene valutata come true, il sistema elabora immediatamente il nodo di destinazione.
    - Se la condizione non viene valutata come true, il sistema si sposta sul nodo di pari livello successivo del nodo di destinazione per valutarne la condizione e ripete questo processo fino a quando non trova un nodo di dialogo con una condizione che viene valutata come true.
    - Se il sistema elabora tutti i nodi di pari livello e nessuna delle condizioni viene valutata come true, verrà utilizzata la strategia di fallback di base e il dialogo valuterà i nodi a livello di base della struttura ad albero di dialogo. 

    La specifica della condizione è utile per concatenare le condizioni dei nodi di dialogo. Ad esempio, potresti voler prima verificare se l'input contiene un intento, come `#turn_on`, e se lo contiene, potresti voler controllare se l'input contiene entità, come `@lights`, `@radio` o `@wipers`. Il concatenamento di condizioni aiuta a strutturare alberi di dialogo più grandi.

## Ulteriori informazioni

Per informazioni sul linguaggio delle espressioni utilizzato dal dialogo, oltre ai metodi, alle entità di sistema e altri dettagli utili, vedi la sezione **Riferimento** nel riquadro di navigazione. 
