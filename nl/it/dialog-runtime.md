---

copyright:
  years: 2015, 2018
lastupdated: "2018-02-16"

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

# Come viene elaborato il dialogo
{: #dialog-runtime}

Apprendi come il tuo dialogo viene elaborato quando una persona interagisce con l'istanza del servizio {{site.data.keyword.conversationshort}} distribuito nel runtime.
{: shortdesc}

## Anatomia di una chiamata di dialogo
{: message-anatomy}

Ogni espressione utente viene passata al dialogo come una chiamata API /message. Ciò include le espressioni che gli utenti generano in risposta a richieste provenienti dal dialogo che richiedono loro ulteriori informazioni. Alcuni piani di sottoscrizione includono un numero di serie di chiamate API, in questo modo puoi comprendere cosa compone una chiamata. Una singola chiamata API /message equivale ad un singolo turno di dialogo che è composto da un input dell'utente e da una risposta corrispondente del dialogo. 

Il corpo della richiesta della chiamata API /message e della risposta include i seguenti oggetti:

- `context`: contiene le variabili che devono essere conservate. Per passare le informazioni da una chiamata alla successiva, lo sviluppatore di applicazioni deve passare il contesto di risposta della chiamata API precedente con ogni chiamata API successiva. Ad esempio, il dialogo può raccogliere il nome dell'utente e quindi fare riferimento all'utente utilizzando il nome nei nodi successivi. 

  ```json
  {
    "context" : {
      "user_name" : "<? @sys-person.literal ?>"
    }
  ```
  {: codeblock}

  Per ulteriori informazioni, vedi [Conservazione delle informazioni tra turni di dialogo](dialog-runtime.html#context).

- `input`: la stringa di testo che è stata inoltrata dall'utente. La stringa di testo può contenere fino a 2.048 caratteri.

  ```json
  {
    "input": {
      "text" : "Where's your nearest store?"
    }
  ```
  {: codeblock}

- `output`: la risposta del dialogo da visualizzare all'utente. Puoi utilizzare questa sezione per definire gli oggetti, ad esempio le variabili, che non devono essere conservati. Ad esempio, se desideri eliminare in modo permanente una variabile di contesto denominata `temp` definita altrove nel dialogo, puoi utilizzare la seguente espressione per eseguire tale operazione.

  ```json
  {
  "output":{
    "text" : {},
    "deleted_variable" : "<? context.remove('temp') ?>"
  ```
  {: codeblock}

  Per ulteriori informazioni sull'oggetto di output, vedi [Risposta complessa](dialog-overview.html#complex). 

Puoi reperire ulteriori informazioni sulla chiamata API /message in [Riferimento API ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://www.ibm.com/watson/developercloud/conversation/api/v1/){: new_window}.

## Conservazione delle informazioni tra turni di dialogo
{: #context}

Il dialogo è privo di stato, il che significa che non conserva le informazioni da un'interazione con l'utente all'altra. È responsabilità dello sviluppatore di applicazioni conservare tutte le informazioni aggiornate di cui necessita l'applicazione. L'applicazione deve ricercare e memorizzare l'oggetto di contesto nella risposta API /message e passarlo nell'oggetto di contesto con la richiesta API /message successiva eseguita come parte del flusso della conversazione. 

Il modo più semplice per conservare le informazioni consiste nel memorizzare l'intero oggetto di contesto in memoria nell'applicazione client - un browser web, ad esempio. Nel momento in cui un'applicazione diventa più complessa oppure se deve passare e memorizzare informazioni identificabili personalmente, puoi memorizzare e richiamare le informazioni da un database.

L'applicazione può passare le informazioni al dialogo e il dialogo può aggiornare queste informazioni e ritrasmetterle all'applicazione o a un nodo successivo. Il dialogo esegue tali operazioni utilizzando le variabili di contesto. 

Una variabile di contesto è una variabile che definisci in un nodo e per la quale puoi facoltativamente specificare un valore predefinito. Altri nodi o la logica di applicazione possono successivamente impostare o modificare il valore della variabile di contesto.

Puoi condizionare i valori delle variabili di contesto facendo riferimento a una variabile di contesto da una condizione del nodo di dialogo per determinare se eseguire un nodo. E puoi fare riferimento a una variabile di contesto dalle condizioni di risposta del nodo di dialogo per mostrare una risposta diversa a seconda del valore fornito da un servizio esterno o dall'utente.

### Passaggio del contesto dall'applicazione
{: #context-from-app}

Passa le informazioni dall'applicazione al dialogo impostando una variabile di contesto e passando tale variabile al dialogo.

Ad esempio, la tua applicazione può impostare una variabile di contesto $time_of_day e passarla al dialogo che può utilizzare le informazioni per adattare il messaggio iniziale visualizzato all'utente.

![Mostra un nodo di benvenuto che utilizza le condizioni di risposta per controllare il valore della variabile di contesto $time_of_day passata al dialogo dall'applicazione.](images/set-context.png)

In questo esempio, il dialogo sa che l'applicazione imposta la variabile su uno di questi valori: *morning*, *afternoon* o *evening*. Può controllare ogni valore e, a seconda del valore che è presente, restituisce il messaggio iniziale appropriato. Se la variabile non viene passata o se ha un valore che non corrisponde a uno dei valori previsti, all'utente viene visualizzato un messaggio iniziale più generico.

### Passaggio del contesto da un nodo all'altro
{: #context-node-to-node}

Il dialogo può anche aggiungere variabili di contesto per passare informazioni da un nodo all'altro o per aggiornare i valori delle variabili di contesto. Mentre il dialogo richiede e riceve informazioni dall'utente, può tenere traccia delle informazioni e farvi riferimento nel corso della conversazione.

Ad esempio, in un nodo potresti chiedere agli utenti il loro nome e in un nodo successivo rivolgerti a loro per nome.

![Mostra un nodo di presentazioni che richiede all'utente il nome e lo memorizza come variabile di contesto. Il nodo successivo si riferisce all'utente per nome utilizzando la variabile di contesto $username.](images/set-context-username.png)

In questo esempio, l'entità di sistema @sys-person viene utilizzata per estrarre il nome dell'utente dall'input,se l'utente ne fornisce uno. Nell'editor JSON, viene definita la variabile di contesto username e viene impostata sul valore @sys-person. In un nodo successivo, la variabile di contesto $username viene inclusa nella risposta per rivolgersi all'utente per nome.

## Definizione di una variabile di contesto
{: #context-var-define}

Definisci una variabile di contesto definendo una coppia nome e valore per la variabile in uno dei seguenti editor:

- **Editor di contesto**: mostra un campo **Variabile** e un campo **Valore** corrispondente nella vista di modifica del nodo che puoi compilare con le informazioni sul nome e sul valore della variabile di contesto. 

  **Nota**: questi campi vengono visualizzati automaticamente nei nodi che hai aggiunto. Per i nodi creati con una versione precedente del servizio, devi aprire l'editor di contesto per i campi da aggiungere. 

- **Editor JSON**: quando aperto, fornisce una vista nel contenuto JSON sottostante che viene passato con la richiesta API /message che viene inviata al servizio {{site.data.keyword.conversationshort}}. Puoi definire le variabili di contesto aggiungendo le coppie nome e valore alla sezione `"context":{}` del corpo JSON.

La coppia nome e valore deve soddisfare questi requisiti:

- Il `name` può contenere caratteri alfabetici maiuscoli e minuscoli, caratteri numerici (0-9) e caratteri di sottolineatura.

  **Nota**: nel nome puoi includere altri caratteri, come punti e trattini. Tuttavia, se li includi, devi utilizzare uno dei seguenti approcci ogni volta che successivamente fai riferimento alla variabile: 

  - **context['variable-name']**

      La sintassi dell'espressione SpEL completa.
  - **$(variable-name)**

      La sintassi abbreviata con il nome della variabile racchiuso tra parentesi.
    Per ulteriori dettagli, vedi [Accesso e valutazione degli oggetti](expression-language.html#shorthand-syntax-for-context-variables).

- Il `value` può essere qualsiasi tipo JSON supportato, come una variabile stringa semplice, un numero o un array JSON. Quando definisci la variabile di contesto utilizzando l'editor JSON, puoi anche specificare un oggetto JSON come valore. 

La seguente tabella mostra come definire le coppie nome e valore nei campi dell'editor della variabile di contesto: 

| Variabile      | Valore             |
|:---------------|--------------------|
| dessert        | cake               |
| toppings_array | ["onion","olives"] |
| age            | 18                 |

Il seguente esempio JSON definisce i valori per le variabili di contesto di stringa $dessert, array $toppings_array e numero $age:

```json
{
  "context": {
    "dessert": "cake",
    "toppings_array": ["onion", "olives"],
    "age": 18
  }
}
```
{: codeblock}

Per definire una variabile di contesto, completa la seguente procedura:

1.  Definisci la variabile di contesto nella sezione del nodo che rappresenta l'ora in cui desideri venga impostata la variabile durante la valutazione del nodo di dialogo.

    **Nota**: i valori della variabile di contesto esistenti definiti per questo nodo vengono visualizzati in una serie di campi **Variabile** e **Valore** corrispondenti. Se non desideri che vengano visualizzati nella vista di modifica del nodo, devi chiudere l'editor di contesto. Puoi chiudere l'editor dallo stesso menu che hai utilizzato per aprirlo; la procedura riportata di seguito descrive come accedere al menu.

    - Per aggiungere una variabile di contesto impostata o modificata dopo l'elaborazione della risposta del nodo, aggiungi la variabile di contesto alla sezione della risposta. 

      Fai clic sull'icona **Opzioni**  ![Risposta avanzata](images/kabob.png) associata alla risposta e quindi scegli un editor selezionando una delle seguenti opzioni:

      - **Apri editor JSON**
      - **Apri editor di contesto**

      ![Mostra come accedere all'editor JSON associato ad una risposta del nodo standard.](images/contextvar-json-response.png)

      Se l'impostazione **Risposte multiple** è **Attivo** per il nodo, devi innanzitutto fare clic sull'icona **Modifica risposta** ![Modifica risposta](images/edit-slot.png).

      ![Mostra come accedere all'editor JSON associato ad un nodo standard per cui sono abilitate più risposte condizionali.](images/contextvar-json-multi-response.png)

    - Per aggiungere una variabile di contesto impostata o aggiornata dopo aver soddisfatto una condizione slot, fai clic sull'icona **Modifica slot** ![Modifica risposta](images/edit-slot.png). Dal menu **Opzioni** ![Risposta avanzata](images/kabob.png) nell'intestazione della vista *Configura slot*, fai clic su **Apri editor JSON**. (Per ulteriori informazioni sugli slot, vedi [Raccolta di informazioni con gli slot](dialog-slots.html).)

      **Nota**: al momento non puoi utilizzare l'editor di contesto per definire le variabili di contesto impostate durante questa fase della valutazione del nodo di dialogo. 

      ![Mostra come accedere all'editor JSON associato ad una condizione slot.](images/contextvar-json-slot-condition.png)

    - Per aggiungere una variabile di contesto elaborata dopo aver soddisfatto una condizione di risposta per uno slot, fai clic sull'icona **Modifica slot** ![Modifica risposta](images/edit-slot.png). Fai clic sull'icona **Opzioni** ![Risposta avanzata](images/kabob.png) e quindi seleziona **Abilita risposte condizionali**. Fai clic sull'icona **Modifica risposta** ![Modifica risposta](images/edit-slot.png) accanto alla risposta a cui si desidera associare la variabile di contesto. Fai clic sull'icona **Opzioni** ![Risposta avanzata](images/kabob.png) nella sezione della risposta e quindi scegli un editor selezionando una delle seguenti opzioni:

      - **Apri editor JSON**
      - **Apri editor di contesto**

      ![Mostra come accedere all'editor JSON associato alla risposta condizionale per uno slot.](images/contextvar-json-slot-multi-response.png)
1.  Per definire la variabile di contesto nell'editor di contesto, aggiungi la coppia nome e valore della variabile nei campi **Variabile** e **Valore**.
1.  Per definire la variabile di contesto nell'editor JSON, completa la seguente procedura aggiuntiva:

    - Aggiungi un blocco `"context":{}` se non è presente. 

      ```json
      {
        "context":{},
      "output":{}
    }
      ```
      {: codeblock}

    - Nel blocco di contesto, aggiungi una coppia di nome e valore per ciascuna variabile di contesto che vuoi definire.

      ```json
      {
        "context":{
          "name": "value"
      },
        "output": {}
      }
      ```
      {: codeblock}

    In questo esempio, una variabile denominata `new_variable` viene aggiunta ad un blocco di contesto che contiene già una variabile. 

    ```json
    {
      "context":{
        "existing_variable": "value",
        "new_variable":"value"
      }
    }
    ```
    {: codeblock}

    Per fare successivamente riferimento alla variabile di contesto, utilizza la sintassi `$name` dove *name* è il nome della variabile di contesto che hai definito. Ad esempio, `$new_variable`.

## Attività comuni della variabile di contesto
{: #context-common-tasks}

Per memorizzare l'intera stringa fornita dall'utente come input, utilizza `input.text`:

| Variabile| Valore           |
|----------|------------------|
| repeat   | `<?input.text?>` |

```json
{
  "context": {
    "repeat": "<?input.text?>"
      }
}
```
{: codeblock}

Per memorizzare il valore di un'entità in una variabile di contesto, utilizza questa sintassi:

| Variabile| Valore           |
|----------|------------------|
| place    | @place           |

```json
{
  "context": {
    "place": "@place"
      }
}
```
{: codeblock}

Puoi aggiungere un oggetto JSON ad una variabile di contesto utilizzando un editor. La seguente espressione definisce un oggetto full_name che contiene una serie di valori di inizio e di fine che insieme formano il nome completo della persona. 

| Variabile     | Valore           |
|---------------|------------------|
| full_name     | { "first":"Paul", "last":"Smith" } |

```json
{
  "context": {
    "full_name": {
      "first":"Paul",
      "last":"Smith"
      }
  }
}
```
{: codeblock}

Se si specifica `$full_name.first` nella risposta, verrà visualizzato `Paul`.

Per memorizzare il valore di una stringa che hai estratto dall'input dell'utente, puoi includere un'espressione SpEL che utilizza il metodo di estrazione per applicare un'espressione regolare all'input utente. La seguente espressione estrae un numero dall'input utente e lo salva nella variabile di contesto `$number`.

| Variabile| Valore                              |
|----------|-------------------------------------|
| number   | `<?input.text.extract('[\d]+',0)?>` |

```json
{
  "context": {
     "number": "<?input.text.extract('[\\d]+',0)?>"
  }
}
```
{: codeblock}

Quando definisci un'espressione regolare nell'editor JSON, devi eseguire l'escape delle barre rovesciate che hai utilizzato nell'espressione con un'altra barra rovesciata (`\\`). Non devi eseguire l'escape delle barre rovesciate nelle espressioni regolari che hai definito utilizzando l'editor della variabile di contesto.
{: tip}

Per memorizzare il valore di un'entità modello, aggiungi .literal al nome entità. L'utilizzo di questa sintassi assicura che nella variabile venga memorizzata l'esatta parte di testo dell'input utente corrispondente al modello specificato.

| Variabile| Valore           |
|----------|------------------|
| email    | @email.literal   |

```json
{
  "context": {
    "email": "<? @email.literal ?>"
  }
}
```
{: codeblock}

## Eliminazione di una variabile di contesto
{: #context-delete}

Per eliminare una variabile di contesto, imposta la variabile su null.

```json
{
  "context": {
    "order_form": null
  }
}
```
{: codeblock}

Se desideri rimuovere tutta la traccia della variabile di contesto, puoi utilizzare il metodo JSONObject.remove(string) per eliminarla dall'oggetto di contesto. Tuttavia, devi utilizzare una variabile per eseguire la rimozione. Definisci la nuova variabile nell'output del messaggio in modo tale che non venga salvata oltre la chiamata corrente. 

```json
{
  "output":{
    "text" : {},
    "deleted_variable" : "<? context.remove('order_form') ?>"
  }
}
```
{: codeblock}

In alternativa, puoi eliminare la variabile di contesto nella logica della tua applicazione. 

### Ordine delle operazioni
{: #context-order-of-ops}

L'ordine in cui definisci le variabili di contesto non determinano l'ordine in cui vengono elaborate dal servizio. Il servizio valuta le variabili, che sono definite come coppie di nome e valore JSON, in ordine casuale. Non impostare un valore nella prima variabile di contesto e sperare di riuscire a utilizzarlo nella seconda, perché non vi è alcuna garanzia che la prima variabile di contesto verrà eseguita prima della seconda nel tuo elenco. Ad esempio, non utilizzare due variabili di contesto per implementare la logica che restituisce un numero casuale tra zero e un valore più alto passato al nodo.

```json
"context": {
    "upper": "<? @sys-number.numeric_value + 1?>",
    "answer": "<? new Random().nextInt($upper) ?>"
}
```
{: codeblock}

Utilizza un'espressione leggermente più complessa per evitare di dover fare affidamento sul valore della variabile di contesto $upper che viene valutata prima della variabile di contesto $answer.

```json
"context": {
    "answer": "<? new Random().nextInt(@sys-number.numeric_value + 1) ?>"
}
```
{: codeblock}

### Memorizzazione dei valori dell'entità modello
{: #context-pattern-entities}

Per memorizzare il valore di un'entità modello in una variabile di contesto, aggiungi .literal al nome dell'entità. L'utilizzo di questa sintassi assicura che nella variabile venga memorizzata l'esatta parte di testo dell'input utente corrispondente al modello specificato.

```json
{
  "context": {
    "email": "<? @email.literal ?>"
  }
}
```
{: codeblock}

Per memorizzare il testo da un singolo gruppo in un'entità modello con gruppi definiti, specifica il numero di array del gruppo che desideri memorizzare. Ad esempio, presupponi che il modello di entità sia definito come segue per l'entità @phone_number. (Ricorda, le parentesi indicano i gruppi di modelli):

`\b((958)|(555))-(\d{3})-(\d{4})\b`

Per memorizzare solo il prefisso dal numero di telefono specificato nell'input utente, puoi utilizzare la seguente sintassi: 

```json
{
  "context": {
    "area_code": "<? @phone_number.groups[1] ?>"
  }
}
```
{: codeblock}

I gruppi sono delimitati dall'espressione regolare utilizzata per definire il modello di gruppo. Ad esempio, se l'input utente che corrisponde al modello definito nell'entità `@phone_number` è: `958-234-3456`, verranno creati i seguenti gruppi:

| Numero gruppo| Valore motore Regex | Valore dialogo | Spiegazione |
|--------------|---------------------|----------------|-------------|
| groups[0]    | `958-234-3456`      | `958-234-3456` | Il primo gruppo è sempre una stringa di corrispondenza completa. |
| groups[1]    | `((958)`l`(555))`   | `958`          | La stringa che corrisponde a regex per il primo gruppo definito, che è `((958)`l`(555))`. |
| groups[2]    | `(958)`             | `958`          | La corrispondenza nel gruppo incluso come primo operando nell'espressione OR `((958)`l`(555))` |
| groups[3]    | `(555)`             | `null`         | Nessuna corrispondenza nel gruppo incluso come secondo operando nell'espressione OR `((958)`l`(555))` |
| groups[4]    | `(\d{3})`           | `234`          | La stringa che corrisponde all'espressione regolare definita per il gruppo. |
| groups[5]    | `(\d{4})`           | `3456`         | La stringa che corrisponde all'espressione regolare definita per il gruppo. |
{: caption="Dettagli del gruppo" caption-side="top"}

Per aiutarti nel decifrare quale numero di gruppo utilizzare per acquisire la sezione dell'input a cui sei interessato, puoi estrarre contemporaneamente le informazioni relative a tutti i gruppi. Utilizza la seguente sintassi per creare una variabile di contesto che restituisca un array di tutte le corrispondenze di entità modello raggruppate:

```json
{
  "context": {
    "array_of_matched_groups": "<? @phone_number.groups ?>"
  }
}
```
{: codeblock}

Utilizza il riquadro "Provalo" per immettere alcuni valori di numero di telefono di test. Per l'input `958-123-2345`, questa espressione imposta `$array_of_matched_groups` su `["958-123-2345","958","958",null,"123","2345"]`.

Puoi quindi conteggiare ciascun valore nell'array partendo da 0 per acquisire il numero di gruppo relativo ad esso. 

| Valore elemento array| Numero elemento array|
|---------------------|----------------------|
| "958-123-2345"      | 0 |
| "958"               | 1 |
| "958"               | 2 |
| null                | 3 |
| "123"               | 4 |
| "2345"              | 5 |
{: caption="Elementi dell'array" caption-side="top"}

È facile stabilire che, per acquisire le ultime quattro cifre del numero di telefono, hai bisogno, ad esempio, del gruppo #5.

Per ritornare alla struttura JSONArray creata per rappresentare l'entità di modello raggruppata, utilizza la seguente sintassi: 

```json
{
  "context": {
    "json_matched_groups": "<? @phone_number.groups_json ?>"
  }
}
```
{: codeblock}

Questa espressione imposta `$json_matched_groups` sul seguente array JSON:

```json
[
  {"group": "group_0","location": [0, 12]},
  {"group": "group_1","location": [0, 3]},
  {"group": "group_2","location": [0, 3]},
  {"group": "group_3"},
  {"group": "group_4","location": [4, 7]},
  {"group": "group_5","location": [8, 12]}
]
```
{: codeblock}

**Nota**: `location` è una proprietà di un'entità che utilizza un offset di caratteri in base zero per indicare dove inizia e termina il valore di entità rilevato nel testo di input. 

Se prevedi che nell'input verranno forniti due numeri di telefono, puoi controllare due numeri di telefono. Se presenti, utilizza la seguente sintassi per acquisire, ad esempio, il prefisso del secondo numero. 

```json
{
  "context": {
    "second_areacode": "<? entities['phone_number'][1].groups[1] ?>"
  }
}
```
{: codeblock}

Se l'input è `I want to change my phone number from 958-234-3456 to 555-456-5678`, `$second_areacode` sarà uguale a `555`.

## Aggiornamento di un valore di variabile di contesto
{: #context-update}

Se un nodo imposta il valore di una variabile di contesto già impostata, il valore precedente viene sovrascritto.

### Aggiornamento di un oggetto JSON complesso

I valori precedenti vengono sovrascritti per tutti i tipi JSON tranne che per un oggetto JSON. Se la variabile di contesto è un tipo complesso come un oggetto JSON, viene utilizzata una procedura di unione JSON per aggiornare la variabile. L'operazione di unione aggiunge qualsiasi proprietà appena definita e sovrascrive tutte le proprietà esistenti dell'oggetto

In questo esempio, una variabile di contesto di nome è definita come un oggetto complesso.

```json
{
  "context": {
    "complex_object": {
      "user_firstname" : "Paul",
      "user_lastname" : "Pan",
      "has_card" : false
    }
  }
}
```
{: codeblock}

Un nodo di dialogo aggiorna l'oggetto JSON della variabile di contesto con i seguenti valori:

```json
{
  "complex_object": {
    "user_firstname": "Peter",
    "has_card": true
  }
}
```
{: codeblock}

Il risultato è il seguente contesto:

```json
{
  "complex_object": {
    "user_firstname": "Peter",
    "user_lastname": "Pan",
    "has_card": true
  }
}
```
{: codeblock}

Vedi [Metodi del linguaggio delle espressioni](dialog-methods.html#objects) per ulteriori informazioni sui metodi che puoi eseguire sugli oggetti. 

### Aggiornamento di array

Se i tuoi dati di contesto del dialogo contengono un array di valori, puoi aggiornare l'array aggiungendo valori, rimuovendo un valore o sostituendo tutti i valori.

Per aggiornare l'array, scegli una delle seguenti azioni. In ogni caso, vedremo l'array prima dell'azione, l'azione e l'array dopo che l'azione è stata applicata.

- **Aggiungi**: per aggiungere valori alla fine di un array, utilizza il metodo `append`.

    Per questo contesto di runtime di dialogo:

    ```json
    {
      "context": {
        "toppings_array": ["onion", "olives"]
      }
    }
    ```
    {: codeblock}

    Effettua questo aggiornamento:

    ```json
    {
      "context": {
        "toppings_array": "<? $toppings_array.append('ketchup', 'tomatoes') ?>"
      }
    }
    ```
    {: codeblock}

    Risultato:

    ```json
    {
      "context": {
        "toppings_array": ["onion", "olives", "ketchup", "tomatoes"]
      }
    }
    ```
    {: codeblock}

- **Rimuovi**: per rimuovere un elemento, utilizza il metodo `remove` e specifica il suo valore o la sua posizione nell'array.

    - **Rimuovi per valore**: rimuove un elemento da un array in base al suo valore.

        Per questo contesto di runtime di dialogo:

        ```json
        {
          "context": {
            "toppings_array": ["onion", "olives"]
          }
        }
        ```
        {: codeblock}

        Effettua questo aggiornamento:

        ```json
        {
          "context": {
            "toppings_array": "<? $toppings_array.removeValue('onion') ?>"
          }
        }
        ```
        {: codeblock}

        Risultato:

        ```json
        {
          "context": {
            "toppings_array": ["olives"]
          }
        }
        ```
        {: codeblock}

    - **Rimuovi per posizione**: rimozione di un elemento da un array in base alla sua posizione nell'indice.

        Per questo contesto di runtime di dialogo:

        ```json
        {
          "context": {
            "toppings_array": ["onion", "olives"]
          }
        }
        ```
        {: codeblock}

        Effettua questo aggiornamento:

        ```json
        {
          "context": {
            "toppings_array": "<? $toppings_array.remove(0) ?>"
          }
        }
        ```
        {: codeblock}

        Risultato:

        ```json
        {
          "context": {
            "toppings_array": ["olives"]
          }
        }
        ```
        {: codeblock}

- **Sovrascrivi**: per sovrascrivere i valori in un array, imposta semplicemente l'array sui nuovi valori:

    Per questo contesto di runtime di dialogo:

        ```json
        {
          "context": {
            "toppings_array": ["onion", "olives"]
          }
        }
        ```
        {: codeblock}

    Effettua questo aggiornamento:

        ```json
        {
          "context": {
            "toppings_array": ["ketchup", "tomatoes"]
          }
        }
        ```
        {: codeblock}

    Risultato:

        ```json
        {
          "context": {
            "toppings_array": ["ketchup", "tomatoes"]
          }
        }
        ```
        {: codeblock}

Vedi [Metodi del linguaggio delle espressioni](dialog-methods.html#arrays) per ulteriori informazioni sui metodi che puoi eseguire sugli array. 

## Digressioni
{: #digressions}

Una digressione si verifica quando un utente si trova in un flusso del dialogo progettato per realizzare un obiettivo e inaspettatamente passa ad argomenti che iniziano un flusso di dialogo che è progettato per realizzare un obiettivo diverso. Il dialogo ha sempre supportato la capacità dell'utente di cambiare argomento. Se nessuno dei nodi nel ramo di dialogo elaborato corrisponde all'obiettivo dell'ultimo input dell'utente, la conversazione torna alla struttura ad albero per controllare le condizioni del nodo root alla ricerca di una corrispondenza appropriata. Le impostazioni di digressione disponibili per il nodo ti forniscono la capacità di personalizzare ancora di più questo comportamento. 

Con le impostazioni di digressione, puoi consentire il ritorno della conversazione al flusso di dialogo che è stato interrotto quando si è verificata la digressione. Ad esempio, è possibile che l'utente stia ordinando un nuovo telefono ma passi agli argomenti per richiedere i tablet. Il tuo dialogo può rispondere alla domanda sui tablet e quindi riportare l'utente al punto in cui si trovava nel processo di ordinazione di un telefono. Consentire le digressioni e poi poter tornare indietro fornisce ai tuoi utenti un maggiore controllo sul flusso della conversazione nel runtime. Possono cambiare gli argomenti, seguire un flusso di dialogo su un argomento non correlato al loro obiettivo e poi ritornare al punto in cui si trovavano prima. Il risultato è un flusso di dialogo che simula più strettamente una conversazione tra persone. 

La seguente immagine utilizza un mockup dell'interfaccia utente della struttura ad albero di dialogo per illustrare il concetto di digressione. Mostra come un utente interagisce con i nodi di dialogo configurati per consentire le digressioni che ritornano al flusso di dialogo che era in corso. L'utente inizia a fornire le informazioni necessarie per prenotare una cena. Mentre sta riempiendo gli slot nel nodo #reservation, l'utente pone una domanda sulle opzioni per il menu vegetariano. Il dialogo risponde alla nuova domanda dell'utente trovando un nodo che lo indirizza tra i nodi root (un nodo che condiziona l'intento #cuisine). Ritorna quindi alla conversazione che era in corso mostrando la richiesta del successivo slot vuoto proveniente dal nodo di dialogo originale. 

![Mostra una persona che sta fornendo dettagli sulla prenotazione di una cena e che richiede le opzioni vegetariane, ottiene una risposta e poi ritorna fornendo i dettagli della prenotazione.](images/digression.gif)

### Prima di iniziare

Mentre verifichi l'intero dialogo, decidi quando e dove consentire le digressioni ed il ritorno dalle digressioni. I controlli di digressione riportati di seguito vengono applicati automaticamente ai nodi. Intervieni solo se desideri modificare questo comportamento predefinito. 

- Ogni nodo root nel dialogo è configurato per consentire alle digressioni di farne la loro destinazione per impostazione predefinita. I nodi figlio non possono essere la destinazione di una digressione. 
- I nodi con slot sono configurati per impedire la generazione di digressioni. Tutti gli altri nodi sono configurati per consentire le digressioni. Tuttavia, la conversazione non consente la generazione di una digressione da un nodo nelle seguenti circostanze: 

  - Se uno qualsiasi dei nodi figlio del nodo corrente contiene la condizione `anything_else` o `true`

    Queste condizioni sono speciali in quanto vengono sempre valutate come true. A causa del loro comportamento noto, spesso vengono utilizzate nei dialoghi per forzare un nodo padre a valutare uno specifico nodo figlio in successione. Per impedire l'interruzione della logica del flusso di dialogo esistente, le digressioni non sono consentite in questo caso. Prima di poter consentire la generazione di digressioni su un nodo di questo tipo, devi modificare la condizione del nodo figlio con un altro valore. 

  - Se il nodo è configurato per passare ad un altro nodo o per ignorare l'input utente dopo averlo elaborato 

    La sezione del passo finale di un nodo specifica le operazioni che devono essere eseguite una volta elaborato il nodo. Quando il dialogo è configurato per passare direttamente ad un altro nodo, spesso è per garantire che venga seguita una sequenza specifica. E quando il nodo è configurato per ignorare l'input utente, equivale a forzare il dialogo ad elaborare il primo nodo figlio dopo il nodo corrente in successione. Per impedire l'interruzione della logica del flusso di dialogo esistente, le digressioni non sono consentite in nessun caso. Prima di poter consentire la generazione delle digressioni da questo nodo, devi modificare il valore specificato nella sezione del passo finale. 

### Personalizzazione delle digressioni
{: #enable-digressions}

Non devi definire l'inizio e la fine di una digressione. L'utente ha il pieno controllo del flusso di digressione nel runtime. Devi solo specificare in che modo ciascun nodo deve partecipare o meno in una digressione guidata dall'utente. Per ciascun nodo, configura se:

- una digressione può iniziare dal nodo e lasciarlo
- una digressione che inizia altrove può essere indirizzata ed entrare nel nodo
- una digressione che inizia altrove ed entra nel nodo deve ritornare al flusso di dialogo interrotto una volta completato il flusso di dialogo corrente

Per modificare il comportamento di un singolo nodo, completa la seguente procedura:

1.  Fai clic sul nodo per aprirne la vista di modifica. 

1.  Fai clic su **Personalizza** e quindi fai clic sulla scheda **Digressioni**.

    Le opzioni di configurazione sono diverse a seconda del fatto che il nodo che stai modificando sia un nodo root, un nodo figlio, un nodo con figli o un nodo con slot.

    **Generazione di digressioni da questo nodo**

    Se le circostanze elencate in precedenza non si applicano, puoi effettuare le seguenti scelte: 

    - **Tutti i tipi di nodo**: scegli se permettere agli utenti di consentire la generazione di una digressione dal nodo corrente prima che raggiungano la fine del ramo di dialogo corrente.

    - **Tutti i nodi che hanno figli**: scegli se desideri che la conversazione torni al nodo corrente dopo una digressione se la risposta del nodo corrente è stata già visualizzata e i suoi nodi figlio sono connessi all'obiettivo di un nodo. Imposta l'interruttore *Consenti il ritorno dalle digressioni attivate dopo questa risposta del nodo* su **No** per impedire al dialogo di tornare al nodo corrente e continuare a elaborare il suo ramo. 

      Ad esempio, se l'utente chiede `Vendi cupcake?` e viene visualizzata la risposta `Abbiamo cupcake di tutti i tipi e dimensioni` prima che l'utente cambi argomento, potresti volere che il dialogo non torni al punto in cui aveva lasciato. In special modo, se i nodi figlio fanno fronte solo a possibili domande di follow-up provenienti dall'utente e possono essere tranquillamente ignorati. 

      Tuttavia, se il nodo si basa sui nodi figlio per far fronte alla domanda, potresti voler forzare la conversazione a ritornare e a proseguire l'elaborazione dei nodi nel ramo corrente. Ad esempio, la risposta iniziale potrebbe essere `Abbiamo cupcake di tutte le forme e dimensioni. Quale menu vuoi vedere: senza glutine, senza lattosio o normale?` Se l'utente cambia argomento a questo punto, potresti volere che il dialogo ritorni in modo che l'utente possa scegliere un tipo di menu e acquisire le informazioni desiderate. 

    - **Nodi con slot**: scegli se desideri consentire agli utenti di generare digressioni dal nodo prima che vengano riempiti tutti gli slot. Imposta l'interruttore *Consenti la generazione di digressioni durante il riempimento dello slot* su **Sì** per consentire la generazione di digressioni.

      Se abilitato, quando la conversazione ritorna dalla digressione, viene visualizzata la richiesta per lo slot non riempito successivo per incoraggiare l'utente a continuare a fornire le informazioni. Se disabilitato, qualsiasi input inoltrato dall'utente che non contiene un valore che può riempire lo slot verrà ignorato. Tuttavia, puoi far fronte a domande non richieste, che sai che i tuoi utenti potrebbero porre mentre interagiscono con il nodo, definendo i gestori slot. Per ulteriori informazioni, vedi [Aggiunta di slot](dialog-slots.html#add-slots).

      La seguente immagine mostra come viene configurata la generazione delle digressioni dal nodo #reservation con gli slot (mostrato nella precedente immagine).

      ![Mostra le impostazioni per la generazione di digressioni da un nodo con slot.](images/digress-away-slots-full.png)

    - **Nodi con slot**: scegli se l'utente può generare le digressioni solo nel caso in cui ritorni al nodo corrente selezionando la casella di spunta **Consenti digressione dagli slot ai nodi che consentono il ritorno**.

      Quando selezionato, mentre il dialogo ricerca un nodo per rispondere ad una domanda non correlata dell'utente, ignora i nodi root che non sono configurati per il ritorno dopo la digressione. Seleziona questa casella di spunta se desideri impedire agli utenti di poter lasciare in modo permanente il nodo prima che abbiano terminato il completamento degli slot richiesti.

    **Digressioni in questo nodo**

    Puoi effettuare le seguenti scelte sul comportamento delle digressioni in un nodo: 

    - Impedisci agli utenti di generare digressioni nel nodo. Per ulteriori dettagli, vedi [Disabilitazione delle digressioni in un nodo root](#diable-digressions).

    - Quando le digressioni nel nodo sono abilitate, scegli se il dialogo deve tornare al flusso di dialogo da cui è stata generata la digressione. Quando selezionato, una volta elaborato il ramo del nodo corrente, il flusso di dialogo torna al nodo interrotto. Per fare in modo che il dialogo ritorni successivamente, seleziona **Ritorna dopo la digressione**.

    La seguente immagine mostra in che modo vengono configurate le digressioni nel nodo #cuisine (mostrato nella precedente immagine).

    ![Mostra le impostazioni per la generazione di digressioni da un nodo con slot.](images/digress-into-cuisine-full.png)

1.  Fai clic su **Applica**.

1.  Utilizza il riquadro "Provalo" per verificare il comportamento della digressione. 

    Di nuovo, non puoi definire l'inizio e la fine di una digressione. L'utente controlla dove e quando si verificano le digressioni. Puoi solo applicare le impostazioni che determinano come un singolo nodo partecipa ad una di esse. Poiché le digressioni sono così imprevedibili, è difficile sapere in che modo le tue decisioni di configurazione influiranno sulla conversazione generale. Per vedere effettivamente l'impatto delle scelte effettuate, devi verificare il dialogo. 

I nodi #reservation e #cuisine rappresentano due rami del dialogo che possono partecipare a una singola digressione diretta dall'utente. Le impostazioni di digressione configurate per ciascun singolo nodo costituiscono ciò che rende possibile questo tipo di digressione nel runtime. 

![Mostra due dialoghi, uno che imposta la generazione di digressioni dal nodo di slot reservation e uno che imposta la digressione nel nodo cuisine.](images/digression-settings.png)

### Disabilitazione delle digressioni in un nodo root
{: #disable-digressions}

Quando un flusso esegue una digressione in un nodo root, segue il corso del dialogo configurato per tale nodo. Pertanto, potrebbe elaborare una serie di nodi figlio prima di raggiungere la fine del ramo del nodo e quindi, se configurato per effettuare ciò, torna al flusso di dialogo che è stato interrotto. Attraverso il test del dialogo, puoi scoprire che un nodo root viene attivato troppo spesso o in momenti inaspettati oppure che il suo dialogo è troppo complesso e porta l'utente troppo lontano dall'essere un buon candidato per una digressione temporanea. Se stabilisci che preferisci non consentire agli utenti di eseguire digressioni in esso, puoi configurare il nodo root in modo da non consentire digressioni. 

Per disabilitare completamente le digressioni in un nodo root, completa la seguente procedura: 

1.  Fai clic per aprire il nodo root che desideri modificare. 
1.  Fai clic su **Personalizza** e quindi fai clic sulla scheda **Digressioni**.
1.  Imposta l'interruttore *Consenti digressioni in questo nodo* su **Disattivo**.
1.  Fai clic su **Applica**.

Se decidi che vuoi impedire le digressioni in diversi nodi root, ma non vuoi modificare ciascun nodo singolarmente, puoi aggiungere i nodi a una cartella. Dalla pagina *Personalizza* della cartella, puoi impostare l'interruttore *Consenti digressioni in questo nodo* su **Disattivo** per applicare la configurazione a tutti i nodi contemporaneamente. Per ulteriori informazioni, vedi [Organizzazione del dialogo utilizzando le cartelle](dialog-build.html#folders).

### Considerazioni sulla progettazione
{: #digression-design-considerations}

- **Evita l'aumento del nodo di fallback**: molti designer del dialogo includono un nodo con una condizione `true` o `anything_else` alla fine di ogni ramo del dialogo come un modo per impedire agli utenti di rimanere bloccati nel ramo. Questa progettazione restituisce un messaggio generico se l'input utente non corrisponde a nessun elemento che hai anticipato e incluso come indirizzato a un nodo di dialogo specifico. Tuttavia, gli utenti non possono generare le digressioni dai flussi di dialogo che utilizzano questo approccio. 

  Valuta i rami che utilizzano questo approccio per stabilire se sarebbe meglio consentire la generazione di digressioni dal ramo. Se l'input dell'utente non corrisponde a nessun elemento che hai anticipato, potrebbe trovare una corrispondenza in un flusso di dialogo completamente diverso nella tua struttura ad albero. Invece di rispondere con un messaggio generico, puoi utilizzare efficacemente il resto del dialogo per cercare di indirizzare l'input utente. E il nodo a livello root `Altro` può sempre rispondere all'input che nessuno degli altri nodi root può indirizzare. 

- **Riconsidera i passaggi a un nodo di chiusura**: molti dialoghi sono progettati per porre una domanda di chiusura standard, ad esempio `Ho risposto alle tue domande oggi?` Gli utenti non possono generare digressioni dai nodi che sono configurati per passare a un altro nodo. Pertanto, se configuri tutti i tuoi nodi finali del ramo in modo che passino a un nodo di chiusura comune, le digressioni non potranno verificarsi. Prendi in considerazione di tracciare la soddisfazione dell'utente attraverso metriche o in altri modi. 

- **Verifica le possibili catene di digressioni**: se un utente genera le digressioni dal nodo corrente a un altro nodo che consente la generazione di digressioni, l'utente potrebbe potenzialmente generare digressioni da questo altro nodo e ripetere di nuovo questo modello una o più volte. Se tutti i nodi nella catena di digressioni sono configurati per ritornare dopo la digressione, l'utente, alla fine, verrà riportato al nodo di dialogo corrente. Tuttavia, verifica gli scenari che generano più volte le digressioni per stabilire se i singoli nodi funzionano come previsto. 

- **Ricorda che il nodo corrente ha la priorità**: ricorda che i nodi al di fuori del flusso corrente vengono considerati solo come destinazioni della digressione se il flusso corrente non può indirizzare l'input utente. In particolare, è ancora più importante, in un nodo con slot che consentono la generazione di digressioni, chiarire agli utenti quali sono le informazioni che devono fornire e aggiungere le istruzioni di conferma che verranno visualizzate una volta che l'utente fornisce un valore. 

  Durante il processo di riempimento dello slot può essere riempito qualsiasi slot. Pertanto, uno slot potrebbe acquisire input utente non previsto. Ad esempio, potresti avere un nodo con slot che raccoglie le informazioni necessarie per prenotare una cena. Uno degli slot raccoglie le informazioni sulla data. Mentre vengono forniti i dettagli della prenotazione, l'utente potrebbe chiedere `Come sarà il tempo domani?` Potresti avere un nodo root che genera condizioni su #forecast che potrebbe rispondere all'utente. Tuttavia, poiché l'input dell'utente include la parola `domani` e il nodo di prenotazione con slot è in fase di elaborazione, il servizio presuppone che l'utente stia invece fornendo o aggiornando la data di prenotazione. *Il nodo corrente ha sempre la priorità.* Se definisci un'istruzione di conferma chiara, ad esempio `Ok, la data di prenotazione è domani,` è più probabile che l'utente si renda conto che si è verificato un fraintendimento e che lo risolva.

  In caso contrario, mentre vengono riempiti gli slot, se l'utente fornisce un valore non previsto da nessuno degli slot, è possibile che corrisponda ad un nodo root completamente non correlato per cui l'utente non ha mai avuto intenzione di generare una digressione. 

  Assicurati di eseguire molti test mentre configuri il comportamento della digressione. 

- **Quando utilizzare le digressioni al posto dei gestori slot**: per domande generali che gli utenti potrebbero porre in qualsiasi momento, utilizza un nodo root che consenta digressioni, elabori l'input e poi torni al flusso che era in corso. Per i nodi con slot, prova ad anticipare i tipi di domande correlate che gli utenti potrebbero voler porre mentre vengono riempiti gli slot e indirizzali aggiungendo i gestori al nodo. 

  Ad esempio, se il nodo con slot raccoglie le informazioni necessarie per compilare una richiesta di indennizzo assicurativo, potresti voler aggiungere i gestori che fanno fronte alle domande relative all'assicurazione. Tuttavia, per domande su come ottenere supporto oppure le posizioni dei negozi oppure la storia della tua azienda, utilizza un nodo a livello di root. 
