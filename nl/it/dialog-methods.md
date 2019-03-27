---

copyright:
  years: 2015, 2019
lastupdated: "2019-02-28"

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

# Metodi del linguaggio delle espressioni
{: #dialog-methods}

Puoi elaborare i valori estratti dalle espressioni dell'utente a cui vuoi fare riferimento in una variabile di contesto, in una condizione o altrove nella risposta.
{: shortdesc}

## Sintassi della valutazione
{: #dialog-methods-evaluation-syntax}

Per espandere i valori delle variabili all'interno di altre variabili oppure applicare metodi al testo di output o alle variabili di contesto, utilizza la sintassi dell'espressione `<? expression ?>`. Ad esempio:

- **Incremento di una proprietà numerica**
    - `"output":{"number":"<? output.number + 1 ?>"}`
- **Richiamo di un metodo su un oggetto**
    - `"context":{"toppings": "<? context.toppings.append( 'onions' ) ?>"}`

Le seguenti sezioni descrivono i metodi che puoi utilizzare per elaborare i valori. Sono organizzate in base al tipo di dati:

- [Array](#dialog-methods-arrays)
- [Data e ora](#dialog-methods-date-time)
- [Numeri](#dialog-methods-numbers)
- [Oggetti](#dialog-methods-objects)
- [Stringhe](#dialog-methods-strings)

## Array
{: #dialog-methods-arrays}

Non puoi utilizzare questi metodi per controllare i valori di un array in una condizione di nodo o una condizione di risposta all'interno dello stesso nodo in cui imposti i valori di array.

### JSONArray.append(object)

Questo metodo aggiunge un nuovo valore al JSONArray e restituisce il JSONArray modificato.

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

### JSONArray.clear()

Questo metodo cancella tutti i valori dall'array e restituisce null.

Utilizza la seguente espressione nell'output per definire un campo che cancella i valori di un array che hai salvato in una variabile di contesto ($toppings_array).

```json
{
  "output":{
    "array_eraser": "<? $toppings_array.clear() ?>"
  }
}
```
{: codeblock}

Se successivamente farai riferimento alla variabile di contesto $toppings_array, verrà restituito solo '[]'.

### JSONArray.contains(Object value)

Questo metodo restituisce true se il JSONArray di input contiene il valore di input.

Per questo contesto di runtime di dialogo che viene impostato da un nodo precedente o da un'applicazione esterna:

```json
{
  "context": {
    "toppings_array": ["onion", "olives", "ham"]
  }
}
```
{: codeblock}

Nodo di dialogo o condizione di risposta:

```json
$toppings_array.contains('ham')
```
{: codeblock}

Risultato: `True` perché l'array contiene l'elemento ham.

### JSONArray.containsIntent(String intent_name, Double min_score, [Integer top_n])
{: #dialog-methods-array-containsIntent}

Questo metodo restituisce `true` se il JSONArray `intents` contiene specificatamente l'intento specificato e se l'intento ha un punteggio di affidabilità pari o superiore al punteggio minimo specificato. Facoltativamente, puoi specificare un numero per indicare che l'intento deve rientrare nel numero di elementi superiori nell'array.

Restituisce `false` se l'intento specificato non si trova nell'array, non ha un punteggio di affidabilità pari o superiore al punteggio minimo di affidabilità oppure o se l'intento si trova più in basso, nell'array, rispetto alla posizione di indice specificata.

Il servizio genera automaticamente un array `intents` che genera gli intenti che il servizio rileva nell'input ogniqualvolta viene inoltrato l'input utente. L'array elenca tutti gli intenti che sono stati rilevati dal servizio partendo dal livello di affidabilità più elevato.

Puoi utilizzare questo metodo in una condizione del nodo non solo per controllare la presenza di un intento, ma per impostare una soglia del punteggio di affidabilità che deve esser soddisfatta prima che il nodo possa essere elaborato e la sua risposta restituita.

Ad esempio, utilizza la seguente espressione in una condizione del nodo quando desideri attivare il nodo di dialogo solo nel caso in cui vengano soddisfatte le seguenti condizioni:

- È presente l'intento `#General_Ending`.
- Il punteggio di affidabilità dell'intento `#General_Ending` supera l'80%.
- L'intento `#General_Ending` è uno dei primi due intenti nell'array degli intenti.

```bash
intents.containsIntent("General_Ending", 0.8, 2)
```
{: codeblock}

### JSONArray.filter(temp, "temp.property operator comparison_value")
{: #dialog-methods-array-filter}

Filtra un array confrontando ciascun valore dell'elemento dell'array con un valore che hai specificato. Questo metodo è simile a una [proiezione della raccolta](#collection-projection). Una proiezione della raccolta restituisce un array filtrato basato su un nome in una coppia nome-valore dell'elemento dell'array. Il metodo filter restituisce un array filtrato basato su un valore in una coppia nome-valore dell'elemento dell'array.

L'espressione di filtro è composta dai seguenti valori:

- `temp`: il nome di una variabile utilizzata temporaneamente mentre viene valutato ciascun elemento dell'array. Ad esempio, `city`.
- `property`: la proprietà dell'elemento che desideri confrontare con `comparison_value`. Specifica la proprietà come una proprietà della variabile temporanea che hai specificato nel primo parametro. Utilizza la sintassi: `temp.property`. Ad esempio, se `latitude` è un nome elemento valido per una coppia nome-valore nell'array, specifica la proprietà come `city.latitude`.
- `operator`: l'operatore da utilizzare per confrontare il valore della proprietà con `comparison_value`.

    Gli operatori supportati sono:

    <table>
    <caption>Operatori di filtraggio supportati</caption>
    <tr>
      <th>Operatore</th>
      <th>Descrizione</th>
    </tr>
    <tr>
      <td>`==`</td>
      <td>È uguale a</td>
    </tr>
    <tr>
      <td>`>`</td>
      <td>È maggiore di</td>
    </tr>
    <tr>
      <td>`<`</td>
      <td>È minore di</td>
    </tr>
    <tr>
      <td>`>=`</td>
      <td>È maggiore di o uguale a</td>
    </tr>
    <tr>
      <td>`<=`</td>
      <td>È minore di o uguale a</td>
    </tr>
    <tr>
      <td>`!=`</td>
      <td>Non è uguale a</td>
    </tr>
    </table>

- `comparison_value`: il valore con cui desideri confrontare ciascun valore della proprietà dell'elemento dell'array. Per specificare un valore che può cambiare a seconda dell'input utente, utilizza una variabile di contesto o un'entità come valore. Se specifichi un valore che può variare, aggiungi la logica per garantire che il valore `comparison_value` sia valido al momento della convalida o in caso di errore.

#### Esempio di filtro 1

Ad esempio, puoi utilizzare il metodo filter per valutare un array che contiene una serie di nomi di città e la relativa popolazione per restituire un array più piccolo che contenga solo le città con una popolazione superiore a 5 milioni.

La variabile di contesto `$cities` riportata di seguito contiene un array di oggetto. Ciascun oggetto contiene una proprietà `name` e `population`.

```json
[
   {
      "name":"Tokyo",
      "population":9273000
   },
   {
      "name":"Rome",
      "population":2868104
   },
   {
      "name":"Beijing",
      "population":20693000
   },
   {
      "name":"Paris",
      "population":2241346
   }
]
```
{: codeblock}

Nel seguente esempio, il nome della variabile temporanea arbitraria è `city`. L'espressione SpEL filtra l'array `$cities` per includere solo le città con una popolazione superiore a 5 milioni:

```bash
$cities.filter("city", "city.population > 5000000")
```
{: codeblock}

L'espressione restituisce il seguente array filtrato:

```json
[
   {
      "name":"Tokyo",
      "population":9273000
   },
   {
      "name":"Beijing",
      "population":20693000
   }
]
```
{: codeblock}

Puoi utilizzare una proiezione della raccolta per creare un nuovo array che includa solo i nomi di città provenienti dall'array restituito dal metodo filter. Puoi quindi utilizzare il metodo `join` per visualizzare i due valori di elemento del nome proveniente dall'array come una stringa e separare i valori con una virgola e uno spazio.

```bash
The cities with more than 5 million people include <?  T(String).join(", ",($cities.filter("city", "city.population > 5000000")).![name]) ?>.
```
{: codeblock}

La risposta risultante è: `The cities with more than 5 million people include Tokyo, Beijing.`

#### Esempio di filtro 2

La potenza del metodo filter è costituita dal fatto che non devi codificare nel programma il valore `comparison_value`. In questo esempio, il valore codificato nel programma 5000000 viene sostituito invece con una variabile di contesto. 

In questo esempio, la variabile di contesto `$population_min` contiene il numero `5000000`. Il nome della variabile temporanea arbitraria è `city`. L'espressione SpEL filtra l'array `$cities` per includere solo le città con una popolazione superiore a 5 milioni:

```bash
$cities.filter("city", "city.population > $population_min")
```
{: codeblock}

L'espressione restituisce il seguente array filtrato:

```json
[
   {
      "name":"Tokyo",
      "population":9273000
   },
   {
      "name":"Beijing",
      "population":20693000
   }
]
```
{: codeblock}

Quando confronti i valori numerici, assicurati di impostare la variabile di contesto coinvolta nel confronto su un valore valido prima che il metodo filter venga attivato. Tieni presente che `null` può essere un valore valido se l'elemento dell'array con cui lo stai confrontando potrebbe contenerlo. Ad esempio, se la coppia nome e valore della popolazione per Tokyo è `"population":null` e l'espressione di confronto è `"city.population == $population_min"`, `null` sarebbe un valore valido per la variabile di contesto `$population_min`.
{: tip}

Puoi utilizzare un'espressione di risposta del nodo di dialogo come questa: 

```bash
The cities with more than $population_min people include <?  T(String).join(", ",($cities.filter("city", "city.population > $population_min")).![name]) ?>.
```
{: codeblock}

La risposta risultante è: `The cities with more than 5000000 people include Tokyo, Beijing.`

#### Esempio di filtro 3

In questo esempio, un nome entità viene utilizzato come `comparison_value`. L'input utente è, `What is the population of Tokyo?` Il nome della variabile temporanea arbitraria è `y`. Hai creato un'entità denominata `@city` che riconosce i nomi delle città, inclusa `Tokyo`.

```bash
$cities.filter("y", "y.name == @city")
```

L'espressione restituisce il seguente array:

```json
[
   {
      "name":"Tokyo",
      "population":9273000
   }
]
```
{: codeblock}

Puoi utilizzare una proiezione della raccolta per ottenere un array con solo l'elemento relativo alla popolazione dell'array originale e poi utilizzare il metodo `get` per restituire il valore dell'elemento popolazione.

```bash
The population of @city is: <? ($cities.filter("y", "y.name == @city").![population]).get(0) ?>.
```
{: codeblock}

L'espressione restituisce: `The population of Tokyo is 9273000.`

### JSONArray.get(Integer)

Questo metodo restituisce l'indice di input dal JSONArray.

Per questo contesto di runtime di dialogo che viene impostato da un nodo precedente o da un'applicazione esterna:

```json
{
  "context": {
    "name": "John",
    "nested": {
      "array": [ "one", "two" ]
    }
  }
}
```
{: codeblock}

Nodo di dialogo o condizione di risposta:

```json
$nested.array.get(0).getAsString().contains('one')
```
{: codeblock}

Risultato:
`True` perché l'array nidificato contiene `one` come valore.

Risposta:

```json
"output":{
  "generic":[
      {
      "values": [
        {
        "text" : "The first item in the array is <?$nested.array.get(0)?>"
        }
      ],
      "response_type": "text",
      "selection_policy": "sequential"
    }
  ]
  }
```
{: codeblock}

### JSONArray.getRandomItem()

Questo metodo restituisce un elemento casuale dal JSONArray di input.

Per questo contesto di runtime di dialogo:

```json
{
  "context": {
    "toppings_array": ["onion", "olives", "ham"]
  }
}
```
{: codeblock}

Output del nodo di dialogo:

```json
{
  "output":{
  "generic":[
      {
      "values": [
        {
    "text": "<? $toppings_array.getRandomItem() ?> is a great choice!"
        }
      ],
      "response_type": "text",
      "selection_policy": "sequential"
    }
  ]
  }
}
```
{: codeblock}

Risultato: `"ham is a great choice!"` o `"onion is a great choice!"` o `"olives is a great choice!"`

**Nota:** il testo di output risultante viene scelto in modo casuale.

### JSONArray.indexOf(value)
{: #dialog-methods-array-indexOf}

Questo metodo restituisce il numero di indice dell'elemento che corrisponde al valore che specifichi come parametro o a `-1` se il valore non viene trovato nell'array. Il valore può essere una Stringa (`"School"`), un numero Intero (`8`) o un numero Doppio (`9.1`). Il valore deve essere una corrispondenza esatta ed è sensibile al minuscolo/maiuscolo. 

Ad esempio, le seguenti variabili di contesto contengono array:

```json
{
  "context": {
    "array1": ["Mary","Lamb","School"],
    "array2": [8,9,10],
    "array3": [8.1,9.1,10.1]
  }
}
```

Le seguenti espressioni possono essere utilizzate per determinare l'indice dell'array in cui viene specificato il valore: 

```bash
<? $array1.indexOf("Mary") ?> returns `0`
<? $array2.indexOf(9) ?> returns `1`
<? $array3.indexOf(10.1) ?> returns `2`
```

Questo metodo può essere utile per ottenere l'indice di un elemento in un array di intenti, ad esempio. Puoi applicare il metodo `indexOf` all'array di intenti generato ogni volta che l'input utente viene valutato per determinare il numero di indice dell'array di uno specifico intento.

```bash
intents.indexOf("General_Greetings")
```
{: codeblock}

Se desideri conoscere il punteggio di affidabilità per uno specifico intento, puoi passare l'espressione sopra riportata come valore *`index`* in un'espressione con la sintassi `intents[`*`index`*`].confidence`. Ad esempio:

```bash
intents[intents.indexOf("General_Greetings")].confidence
```
{: codeblock}

### JSONArray.join(String delimiter)

Questo metodo unisce tutti i valori di questo array a una stringa. I valori vengono convertiti in stringa e delimitati dal delimitatore di input.

Per questo contesto di runtime di dialogo:

```json
{
  "context": {
    "toppings_array": ["onion", "olives", "ham"]
  }
}
```
{: codeblock}

Output del nodo di dialogo:

```json
{
  "output":{
  "generic":[
      {
      "values": [
        {
    "text": "This is the array: <? $toppings_array.join(';') ?>"
        }
      ],
      "response_type": "text",
      "selection_policy": "sequential"
    }
  ]
  }
}
```
{: codeblock}

Risultato:

```json
This is the array: onion;olives;ham;
```
{: codeblock}

Se definisci una variabile che memorizza più valori in un array JSON, puoi restituire una sottoserie dei valori dell'array e utilizzare il metodo join() per formattarli in modo corretto. 

#### Proiezione della raccolta
{: #dialog-methods-collection-projection}

Un'espressione SpEL della `proiezione della raccolta` estrae una sottoraccolta da un array che contiene oggetti. La sintassi per una proiezione della raccolta è `array_that_contains_value_sets.![value_of_interest]`.

Ad esempio, la seguente variabile di contesto definisce un array JSON che memorizza le informazioni sul volo. Ci sono due punti di dati per volo, l'orario e il numero di volo. 

```json
"flights_found": [
  {
    "time": "10:00",
    "flight_code": "OK123"
  },
  {
    "time": "12:30",
    "flight_code": "LH421"
  },
  {
    "time": "16:15",
    "flight_code": "TS4156"
  }
]
```
{: codeblock}

Per restituire solo i numeri di volo, puoi creare un'espressione della proiezione della raccolta utilizzando la seguente sintassi: 

```
<? $flights_found.![flight_code] ?>
```

Questa espressione restituisce un array dei valori `flight_code` come `["OK123","LH421","TS4156"]`. Per ulteriori dettagli, vedi la [documentazione di SpEL Collection Projection](https://docs.spring.io/spring/docs/3.0.x/reference/expressions.html).

Se applichi il metodo `join()` ai valori nell'array restituito, i numeri di volo vengono visualizzati in un elenco separato da virgole. Ad esempio, puoi utilizzare la seguente sintassi in una risposta: 

```
The flights that fit your criteria are:
  <? T(String).join(",", $flights_found.![flight_code]) ?>.
```
{: codeblock}

Risultato: `The flights that match your criteria are: OK123,LH421,TS4156.`

### JSONArray.joinToArray(template)
{: #dialog-methods-joinToArray}

Questo metodo si applica al formato che definisci in un template nell'array e restituisce un array formattato in base alle tue specifiche. Questo template è utile per applicare la formattazione ai valori dell'array che desideri restituire in una risposta del dialogo, ad esempio. 

Il template può essere specificato come Stringa, oggetto JSON o array JSON. Per fare riferimento ai valori dall'array che stai modificando nel template, segui queste convenzioni di sintassi: 

- `%`: rappresenta l'inizio e la fine di un elemento o di una proprietà dell'elemento che desideri restituire dall'array che stai modificando. 
- `e`: rappresenta temporaneamente l'elemento dell'array a cui vuoi applicare la formattazione. Questo nome di variabile temporanea non può essere modificato da `e`.

Ad esempio, hai una variabile di contesto che contiene un array con un elenco dei dettagli di volo relativi a tre voli. 

```json
"flights": [
      {
        "flight": "DL1040",
        "origin": "JFK",
        "carrier": "Alitalia",
        "duration": 485,
        "destination": "FCO",
        "arrival_date": "2019-02-03",
        "arrival_time": "07:00",
        "departure_date": "2019-02-02",
        "departure_time": "16:45"
      },
      {
        "flight": "DL1710",
        "origin": "JFK",
        "carrier": "Delta",
        "duration": 379,
        "destination": "LAX",
        "arrival_date": "2019-02-02",
        "arrival_time": "10:19",
        "departure_date": "2019-02-02",
        "departure_time": "07:00"
      },
      {
        "flight": "DL4379",
        "origin": "BOS",
        "carrier": "Virgin Atlantic",
        "duration": 385,
        "destination": "LHR",
        "arrival_date": "2019-02-03",
        "arrival_time": "09:05",
        "departure_date": "2019-02-02",
        "departure_time": "21:40"
      }
    ]
```
{: codeblock}

Desideri restituire solo un elenco dei numeri di volo. Per estrarre solo il valore dell'elemento `flight` da ciascun array e restituirlo in un elenco, puoi utilizzare la seguente espressione:

```
The available flights are <? $flights.joinToArray("%e.flight%"). ?>
```
{: codeblock}

La risposta del nodo di dialogo è `The available flights are ["DL1040","DL1710","DL4379"].`

Per visualizzare l'array come testo, utilizza il metodo `join` nell'espressione come segue: 

```
The available flights are <? $flights.joinToArray("%e.flight%").join(", "). ?>
```
{: codeblock}

La risposta è, `The available flights are DL1040, DL1710, DL4379.`

#### Template complesso
{: #dialog-methods-complex-template}

Per creare un template più complesso, invece di specificare i dettagli del template direttamente nel parametro del metodo, puoi creare una variabile di contesto. 

Questa variabile di contesto del template contiene una sottoserie degli elementi dell'array e aggiunge le etichette davanti ad essi, quindi le informazioni verranno visualizzate in un elenco leggibile nella risposta:

```json
"template": "<br/>Flight number: %e.flight% <br/> Airline: %e.carrier% <br/> Departure date: %e.departure_date% <br/> Departure time: %e.departure_time% <br/> Arrival time: %e.arrival_time% <br/>"
```
{: codeblock}

Il rendering della tag HTML `<br/>` per un'interruzione di riga *non* viene eseguito da alcuni dei canali di integrazione, inclusi Facebook e Slack.
{: note}

Utilizza questa espressione nella risposta del nodo di dialogo per applicare il template definito in `$template` all'array in `$flights`.

```
The flight info is <? $flights.joinToArray($template).join(" ") ?>
```
{: codeblock}

La risposta sarà simile alla seguente:

```
The flight info is
Flight number: DL1040
Airline: Alitalia
Departure date: 2019-02-02
Departure time: 16:45
Arrival time: 07:00

Flight number: DL1710
Airline: Delta
Departure date: 2019-02-02
Departure time: 07:00
Arrival time: 10:19

Flight number: DL4379
Airline: Virgin Atlantic
Departure date: 2019-02-02
Departure time: 21:40
Arrival time: 09:05
```
{: screen}

Il vantaggio di utilizzare questo metodo consiste nel fatto che non importa la frequenza con cui cambiano i valori nell'array oppure se il numero di elementi nell'array aumenta. Fino a quando ciascun elemento dell'array contiene almeno una sottoserie delle proprietà a cui fa riferimento il template, l'espressione funzionerà. 

#### Esempio di template dell'oggetto JSON
{: #dialog-methods-object-template}

In questo esempio, la variabile di contesto del template è definita come un oggetto JSON che estrae il numero di volo e le date e gli orari di partenza e di arrivo da ognuno degli elementi del volo specificati nell'array nella variabile di contesto `$flights`. Puoi utilizzare questo approccio per applicare la formattazione standard ai dettagli dei voli che vengono gestiti da due diversi vettori e chi formatta le informazioni di volo direttamente nei loro servizi web, ad esempio. 

```json
"template": {
      "departure": "Flight %e.flight% departs on %e.departure_date% at %e.departure_time%.",
      "arrival": "Flight %e.flight% arrives on %e.arrival_date% at %e.arrival_time%."
    }
```
{: codeblock}

Potresti voler progettare la tua applicazione client personalizzata per leggere gli oggetti dall'array restituito e formattare correttamente i valori per la risposta del tuo bot della chat. La tua risposta del nodo di dialogo può restituire l'oggetto dei dettagli di arrivo del volo come un array utilizzando questa espressione: 

```
<? $flights.joinToArray($template) ?>
```
{: screen}

Questa è la risposta del nodo di dialogo:

```json
[
  {
    "arrival":"Flight DL1040 arrives on 2019-02-03 at 07:00.",
    "departure":"Flight DL1040 departs on 2019-02-02 at 16:45."
    },
  {
    "arrival":"Flight DL1710 arrives on 2019-02-02 at 10:19.",
    "departure":"Flight DL1710 departs on 2019-02-02 at 07:00."
    },
  {
    "arrival":"Flight DL4379 arrives on 2019-02-03 at 09:05.",
    "departure":"Flight DL4379 departs on 2019-02-02 at 21:40."
    }
  ]
  ```

Tieni presente che l'ordine degli elementi `arrival` e `departure` viene scambiato nella risposta. Di norma, il servizio riordina gli elementi in un oggetto JSON. Se desideri che gli elementi vengano restituiti in uno specifico ordine, definisci il template utilizzando invece un valore Array JSON o Stringa.

### JSONArray.remove(Integer)

Questo metodo rimuove l'elemento nella posizione dell'indice dal JSONArray e restituisce il JSONArray aggiornato.

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

### JSONArray.removeValue(object)

Questo metodo rimuove la prima ricorrenza del valore dal JSONArray e restituisce il JSONArray aggiornato.

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

### JSONArray.set(Integer index, Object value)

Questo metodo imposta l'indice di input del JSONArray sul valore di input e restituisce il JSONArray modificato.

Per questo contesto di runtime di dialogo:

```json
{
  "context": {
    "toppings_array": ["onion", "olives", "ham"]
  }
}
```
{: codeblock}

Output del nodo di dialogo:

```json
{
  "context": {
    "toppings_array": "<? $toppings_array.set(1,'ketchup')?>"
  }
}
```
{: codeblock}

Risultato:

```json
{
  "context": {
    "toppings_array": ["onion", "ketchup", "ham"]
  }
}
```
{: codeblock}

### JSONArray.size()

Questo metodo restituisce la dimensione del JSONArray come numero intero.

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
    "toppings_array_size": "<? $toppings_array.size() ?>"
  }
}
```
{: codeblock}

Risultato:

```json
{
  "context": {
    "toppings_array_size": 2
  }
}
```
{: codeblock}

### JSONArray split(String regexp)

Questo metodo suddivide la stringa di input utilizzando l'espressione regolare di input. Il risultato è un JSONArray di stringhe.

Per questo input:

```
"bananas;apples;pears"
```
{: codeblock}

Questa sintassi:

```json
{
  "context": {
    "array": "<?input.text.split(";")?>
  }
}
```
{: codeblock}

Risultati in questo output:

```json
{
  "context": {
    "array": [ "bananas", "apples", "pears" ]
  }
}
```
{: codeblock}

### Supporto com.google.gson.JsonArray
{: #dialog-methods-com.google.gson.JsonArray}

Oltre ai metodi integrati, puoi utilizzare i metodi standard della classe `com.google.gson.JsonArray`.

#### Nuovo array

new JsonArray().append('value')

Per definire un nuovo array che verrà riempito con i valori forniti dagli utenti, puoi creare l'istanza di un array. Nel momento in cui crei l'istanza dell'array, devi anche aggiungere un valore segnaposto all'array. Per eseguire tale operazione, puoi utilizzare la seguente sintassi:

```json
{
  "context":{
    "answer": "<? output.answer?:new JsonArray().append('temp_value') ?>"
  }
```
{: codeblock}

## Data e ora
{: #dialog-methods-date-time}

Sono disponibili diversi metodi per lavorare con la data e ora.

Per informazioni su come riconoscere ed estrarre le informazioni sulla data e sull'ora dall'input utente, vedi le [entità @sys-date e @sys-time](/docs/services/assistant?topic=assistant-system-entities#system-entities-sys-date-time).

### .after(String date or time)

Determina se il valore data/ora è dopo l'argomento data/ora.

### .before(String date or time)
Determina se il valore data/ora è prima dell'argomento data/ora.

Ad esempio:
- @sys-time.before('12:00:00')
- @sys-date.before('2016-11-21')

- Se si confrontano elementi diversi, come `time vs. date`, `date vs. time` e `time vs. date and time`, il metodo restituisce false e viene stampata un'eccezione nel log JSON di risposta `output.log_messages`.

  Ad esempio, `@sys-date.before(@sys-time)`.
- Se si confronta `date and time vs. time` il metodo ignora la data e confronta solo le ore.

### now()

Restituisce una stringa con la data e ora corrente in formato `yyyy-MM-dd HH:mm:ss`.

- Funzione statica.
- Gli altri metodi di data/ora possono essere richiamati sui valori data-ora restituiti da questa funzione e possono essere passati come argomento.
- Se la variabile di contesto `$timezone` è impostata, questa funzione restituisce date e ore nel fuso orario del client.

Esempio di un nodo di dialogo con `now()` utilizzato nel campo di output:

```json
{
  "conditions": "#what_time_is_it",
  "output": {
    "generic":[
      {
        "values": [
          {
          "text": "<? now() ?>"
          }
        ],
        "response_type": "text",
        "selection_policy": "sequential"
      }
    ]
  }
}
```
{: codeblock}

Esempio di `now()` nelle condizioni del nodo (per decidere se è ancora mattina):

```json
{
  "conditions": "now().before('12:00:00')",
  "output": {
      "generic":[
      {
        "values": [
          {
          "text": "Good morning!"
          }
        ],
        "response_type": "text",
        "selection_policy": "sequential"
        }
      ]
  }
}
```
{: codeblock}

### .reformatDateTime(String format)

Formatta stringhe di data e ora nel formato desiderato per l'output dell'utente.

Restituisce una stringa formattata in base al formato specificato:

- `MM/dd/yyyy` per 12/31/2016
- `h a` per 10pm

Per restituire il giorno della settimana:

- `E` per martedì
- `u` per l'indice di giorni (1 = lunedì, ..., 7 = domenica)

Ad esempio, questa definizione di variabile di contesto crea una variabile $time che salva il valore 17:30:00 come *5:30 PM*.

```json
{
  "context": {
    "time": "<? @sys-time.reformatDateTime('h:mm a') ?>"
  }
}
```
{: codeblock}

Il formato segue le regole Java [SimpleDateFormat ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](http://docs.oracle.com/javase/6/docs/api/java/text/SimpleDateFormat.html){: new_window}.

**Nota**: quando si tenta di formattare solo l'ora, la data viene considerata come `1970-01-01`.

### .sameMoment(String date/time)

- Determina se il valore data/ora è uguale all'argomento data/ora.

### .sameOrAfter(String date/time)

- Determina se il valore data/ora è dopo o uguale all'argomento data/ora.
- Simile a `.after()`.

### .sameOrBefore(String date/time)

- Determina se il valore data/ora è prima o uguale all'argomento data/ora.

### today()

Restituisce una stringa con la data corrente nel formato `yyyy-MM-dd`.

- Funzione statica.
- Gli altri metodi di data possono essere richiamati sui valori di data restituiti da questa funzione e possono essere passati come argomento. 
- Se la variabile di contesto `$timezone` è impostata, questa funzione restituisce le date nel fuso orario del client. In caso contrario, viene utilizzato il fuso orario `GMT`. 

Esempio di un nodo di dialogo con `today()` utilizzato nel campo di output:

```json
{
  "conditions": "#what_day_is_it",
  "output": {
    "generic":[
      {
        "values": [
          {
          "text": "Today's date is <? today() ?>."
          }
        ],
        "response_type": "text",
        "selection_policy": "sequential"
      }
    ]
  }
}
```
{: codeblock}

Risultato: `Today's date is 2018-03-09.`

## Calcoli di data e ora
{: #dialog-methods-calculations}

Utilizza i seguenti metodi per calcolare una data. 

| Metodo                  | Descrizione |
|-------------------------|-------------|
| `<date>.minusDays(n)`   | Restituisce la data del giorno e il numero n di giorni prima della data specificata. |
| `<date>.minusMonths(n)` | Restituisce la data del giorno e il numero n di mesi prima della data specificata. |
| `<date>.minusYears(n)`  | Restituisce la data del giorno e il numero n di anni prima della data specificata. |
| `<date>.plusDays(n)`   | Restituisce la data del giorno e il numero n di giorni dopo la data specificata. |
| `<date>.plusMonths(n)` | Restituisce la data del giorno e il numero n di mesi dopo la data specificata. |
| `<date>.plusYears(n)`  | Restituisce la data del giorno e il numero n di anni dopo la data specificata. |

dove `<date>` viene specificato nel formato `yyyy-MM-dd` o `yyyy-MM-dd HH:mm:ss`.

Per ottenere la data di domani, specifica la seguente espressione: 

```json
{
  "output":{
    "generic":[
      {
        "values": [
          {
          "text": "Tomorrow's date is <? today().plusDays(1) ?>."
          }
        ],
        "response_type": "text",
        "selection_policy": "sequential"
      }
    ]
  }
}
```
{: codeblock}

Risultato se domani fosse il 9 marzo 2018: `Tomorrow's date is 2018-03-10.`

Per ottenere la data del giorno tra una settimana a partire da oggi, specifica la seguente espressione. 

```json
{
  "output":{
    "generic":[
      {
        "values": [
          {
          "text": "Next week's date is <? @sys-date.plusDays(7) ?>."
          }
        ],
        "response_type": "text",
        "selection_policy": "sequential"
      }
    ]
  }
}
```
{: codeblock}

Risultato se la data acquisita dall'entità @sys-date fosse la data di oggi, 9 marzo 2018: `Next week's date is 2018-03-16.`

Per ottenere la data del mese scorso, specifica la seguente espressione: 

```json
{
  "output":{
    "generic":[
      {
        "values": [
          {
          "text": "Last month the date was <? today().minusMonths(1) ?>."
          }
        ],
        "response_type": "text",
        "selection_policy": "sequential"
      }
    ]
  }
}
```
{: codeblock}

Risultato se oggi fosse il 9 marzo 2018: `Last month the date was 2018-02-9.`

Utilizza i seguenti metodi per calcolare l'ora. 

| Metodo                  | Descrizione |
|-------------------------|-------------|
| `<time>.minusHours(n)`   | Restituisce l'ora n ore prima dell'ora specificata. |
| `<time>.minusMinutes(n)` | Restituisce l'ora n minuti prima dell'ora specificata. |
| `<time>.minusSeconds(n)`  | Restituisce l'ora n secondi prima dell'ora specificata. |
| `<time>.plusHours(n)`   | Restituisce l'ora n ore dopo l'ora specificata. |
| `<time>.plusMinutes(n)` | Restituisce l'ora n minuti dopo l'ora specificata. |
| `<time>.plusSeconds(n)`  | Restituisce l'ora n secondi dopo l'ora specificata. |

dove `<time>` viene specificato nel formato `HH:mm:ss`.

Per ottenere l'orario di un'ora a partire da adesso, specifica la seguente sintassi: 

```json
{
  "output":{
    "generic":[
      {
        "values": [
          {
          "text": "One hour from now is <? now().plusHours(1) ?>."
          }
        ],
        "response_type": "text",
        "selection_policy": "sequential"
      }
    ]
  }
}
```
{: codeblock}

Risultato se fossero le 8 AM: `One hour from now is 09:00:00.`

Per ottenere l'orario di 30 minuti fa, specifica la seguente espressione: 

```json
{
  "output":{
    "generic":[
      {
        "values": [
          {
          "text": "A half hour before @sys-time is <? @sys-time.minusMinutes(30) ?>."
          }
        ],
        "response_type": "text",
        "selection_policy": "sequential"
      }
    ]
  }
}
```
{: codeblock}

Risultato se l'orario acquisito dall'entità @sys-time fosse 8 AM: `A half hour before 08:00:00 is 07:30:00.`

Per riformattare l'orario restituito, puoi utilizzare la seguente espressione: 

```json
{
  "output":{
    "generic":[
      {
        "values": [
          {
          "text": "6 hours ago was <? now().minusHours(6).reformatDateTime('h:mm a') ?>."
          }
        ],
        "response_type": "text",
        "selection_policy": "sequential"
      }
    ]
  }
}
```
{: codeblock}

Risultato se fossero le 2:19 PM: `6 hours ago was 8:19 AM.`

### Utilizzo degli intervalli di tempo
{: #dialog-methods-time-spans}

Per mostrare una risposta basata sul fatto che la data di oggi rientra in un determinato intervallo di tempo, puoi utilizzare una combinazione di metodi correlati al tempo. Ad esempio, se ogni anno fai un'offerta speciale durante il periodo delle festività, puoi controllare se la data di oggi rientra nell'intervallo tra il 25 novembre e il 24 dicembre di quest'anno. Innanzitutto, definisci le date di interesse come variabili di contesto. 

Nelle seguenti espressioni di variabili di contesto della data iniziale e finale, la data viene generata concatenando il valore dell'anno corrente derivato dinamicamente ai valori di mese e giorno codificati nel programma. 

```json
"context": {
   "end_date": "<? now().reformatDateTime('Y') + '-12-24' ?>",
   "start_date": "<? now().reformatDateTime('Y') + '-11-25' ?>"
 }
```

Nella condizione di risposta, puoi indicare che desideri mostrare la risposta solo se la data corrente rientra tra la data iniziale e quella finale che hai definito come variabili di contesto. 

`now().after($start_date) && now().before($end_date)`

### Supporto java.util.Date
{: #dialog-methods-java.util.Date}

Oltre ai metodi integrati, puoi utilizzare i metodi standard della classe `java.util.Date`.

Per ottenere la data del giorno tra una settimana, puoi utilizzare la seguente sintassi.

```json
{
  "context": {
    "week_from_today": "<? new Date(new Date().getTime() +
      (7 * (24*60*60*1000L))) ?>"
  }
}
```
{: codeblock}

Questa espressione richiama innanzitutto la data corrente in millisecondi (da January 1, 1970, 00:00:00 GMT). Calcola anche il numero di millisecondi in 7 giorni. (`(24*60*60*1000L)` rappresenta un giorno in millisecondi.) Quindi aggiunge 7 giorni alla data corrente. Il risultato è la data completa del giorno tra una settimana. Ad esempio, `Fri Jan 26 16:30:37 UTC 2018`. Tieni presente che l'ora viene espressa con il fuso orario UTC. Puoi sempre modificare il valore 7 con una variabile (`$number_of_days`, ad esempio) che puoi passare. Assicurati solo che valore venga di nuovo impostato prima della valutazione di questa espressione.

Se non vuoi più confrontare successivamente la data con un'altra data generata dal servizio, devi riformattare la data. Le entità di sistema (`@sys-date`) e gli altri metodi integrati (`now()`) convertono le date nel formato `yyyy-MM-dd`.

```json
{
  "context": {
    "week_from_today": "<? new Date(new Date().getTime() +
      (7 * (24*60*60*1000L))).format('yyyy-MM-dd') ?>"
  }
}
```
{: codeblock}

Una volta riformattata la data, il risultato è `2018-01-26`. Ora puoi utilizzare un'espressione come `@sys-date.after($week_from_today)` in una condizione di risposta per confrontare una data specificata nell'input utente con la data salvata nella variabile di contesto.

La seguente espressione calcola un orario di 3 ore a partire da ora.

```json
{
  "context": {
    "future_time": "<? new Date(new Date().getTime() + (3 * (60*60*1000L)) -
      (5 * (60*60*1000L))).format('h:mm a') ?>"
  }
}
```
{: codeblock}

Il valore `(60*60*1000L)` rappresenta un'ora in millisecondi. Questa espressione aggiunge 3 ore all'ora corrente. Quindi ricalcola l'ora da un fuso orario UTC ad un fuso orario EST sottraendo 5 ore da essa. Riformatta anche i valori della data in modo da includere ore e minuti AM o PM.

## Numeri
{: #dialog-methods-numbers}

Questi metodi ti consentono di acquisire e riformattare i valori numerici.

Per informazioni sulle entità di sistema che possono riconoscere ed estrarre i numeri dall'input utente, vedi [Entità @sys-number](/docs/services/assistant?topic=assistant-system-entities#system-entities-sys-number).

Se desideri che il servizio riconosca formati numerici specifici nell'input utente, ad esempio riferimenti al numero di ordine, prendi in considerazione di creare un'entità modello per eseguire l'acquisizione. Per ulteriori dettagli, vedi [Creazione di entità](/docs/services/assistant?topic=assistant-entities).

Se desideri modificare la posizione decimale per un numero, per riformattare un numero come un valore di valuta, ad esempio, vedi il [Metodo String format()](#dialog-methods-java.lang.String).

### toDouble()

  Converte l'oggetto o il campo nel tipo di numero Doppio. Puoi chiamare questo metodo su qualsiasi oggetto o campo. Se la conversione non riesce, viene restituito *null*.

### toInt()

  Converte l'oggetto o il campo nel tipo di numero Intero. Puoi chiamare questo metodo su qualsiasi oggetto o campo. Se la conversione non riesce, viene restituito *null*.

### toLong()

  Converte l'oggetto o il campo nel tipo di numero Lungo. Puoi chiamare questo metodo su qualsiasi oggetto o campo. Se la conversione non riesce, viene restituito *null*.

  Se specifichi un tipo di numero Lungo in un'espressione SpEL, devi accodare una lettera `L` al numero per identificarlo come tale. Ad esempio, `5000000000L`. Questa sintassi è obbligatoria per i numeri che non si adattano al tipo Intero a 32 bit. Ad esempio, i numeri maggiori di 2^31 (2,147,483,648) o minori di -2^31 (-2,147,483,648) sono considerati tipi di numero Lungo. I tipi di numero Lungo hanno un valore minimo di -2^63 e un valore massimo di 2^63-1.

### Supporto numero Java
{: #dialog-methods-java.lang.Number}

### java.lang.Math()

Esegue operazioni numeriche di base.

Puoi utilizzare i metodi Classe, inclusi quelli riportati di seguito:

- max()

```json
{
  "context": {
    "bigger_number": "<? T(Math).max($number1,$number2) ?>"
  },
  "output": {
    "generic":[
      {
        "values": [
          {
          "text": "The bigger number is $bigger_number."
          }
        ],
        "response_type": "text",
        "selection_policy": "sequential"
      }
    ]
  }
}
```
{: codeblock}

- min()

```json
{
  "context": {
    "smaller_number": "<? T(Math).min($number1,$number2) ?>"
  },
  "output": {
    "generic":[
      {
        "values": [
          {
          "text": "The smaller number is $smaller_number."
          }
        ],
        "response_type": "text",
        "selection_policy": "sequential"
      }
    ]
  }
}
```
{: codeblock}

- pow()

```json
{
  "context": {
    "power_of_two": "<? T(Math).pow($base.toDouble(),2.toDouble()) ?>"
  },
  "output": {
    "generic":[
      {
        "values": [
          {
          "text": "Your number $base to the second power is $power_of_two."
          }
        ],
        "response_type": "text",
        "selection_policy": "sequential"
      }
    ]
  }
}
```
{: codeblock}

Per informazioni sugli altri metodi, vedi la [documentazione di riferimento java.lang.Math](https://docs.oracle.com/javase/7/docs/api/java/lang/Math.html).

### java.util.Random()

Restituisce un numero casuale. Puoi utilizzare una delle seguenti opzioni di sintassi:

- Per restituire un valore booleano casuale (true o false), utilizza `<?new Random().nextBoolean()?>`.
- Per restituire un numero doppio casuale compreso tra 0 (incluso) e 1 (escluso), utilizza `<?new Random().nextDouble()?>`
- Per restituire un numero intero casuale compreso tra 0 (incluso) e un numero da te specificato, utilizza `<?new Random().nextInt(n)?>`  dove n è l'inizio dell'intervallo di numeri desiderato  + 1.
  Ad esempio, se vuoi restituire un numero casuale tra 0 e 10, specifica `<?new Random().nextInt(11)?>`.
- Per restituire un numero intero casuale dall'intervallo completo di valori interi (da -2147483648 a 2147483648), utilizza `<?new Random().nextInt()?>`.

Ad esempio, puoi creare un nodo di dialogo che viene attivato dall'intento #random_number. La prima condizione di risposta potrebbe essere simile alla seguente:

```json
Condition = @sys-number
{
  "context": {
    "answer": "<? new Random().nextInt(@sys-number.numeric_value + 1) ?>"
  },
  "output": {
    "generic":[
      {
        "values": [
          {
          "text": "Here's a random number between 0 and @sys-number.literal: $answer."
          }
        ],
        "response_type": "text",
        "selection_policy": "sequential"
      }
    ]
  }
}
```
{: codeblock}

Per informazioni sugli altri metodi, vedi la [documentazione di riferimento java.util.Random](https://docs.oracle.com/javase/7/docs/api/java/util/Random.html).

Puoi anche utilizzare i metodi standard delle seguenti classi:

- `java.lang.Byte`
- `java.lang.Integer`
- `java.lang.Long`
- `java.lang.Double`
- `java.lang.Short`
- `java.lang.Float`

## Oggetti
{: #dialog-methods-objects}

### JSONObject.clear()

Questo metodo cancella tutti i valori dall'oggetto JSON e restituisce null.

Ad esempio, desideri cancellare i valori correnti dalla variabile di contesto $user.

```json
{
  "context": {
    "user": {
      "first_name": "John",
      "last_name": "Snow"
    }
  }
}
```
{: codeblock}

Utilizza la seguente espressione nell'output per definire un campo che cancella i valori dell'oggetto. 

```json
{
  "output":{
    "object_eraser": "<? $user.clear() ?>"
  }
}
```
{: codeblock}

Se successivamente farai riferimento alla variabile di contesto $user context, verrà restituito solo `{}`.

Puoi utilizzare il metodo `clear()` sugli oggetti JSON `context` o `output` nel corpo della chiamata API `/message`.

#### Cancellazione del contesto
{: #dialog-methods-clearing-context}

Quando utilizzi il metodo `clear()` per cancellare l'oggetto `context`, cancella **tutte** le variabili ad eccezione di queste: 

 - `context.conversation_id`
 - `context.timezone`
 - `context.system`

**Avvertenza**: tutti i valori della variabile di contesto indicano che:

  - Tutti i valori predefiniti impostati per le variabili nei nodi sono stati attivati durante la sessione corrente. 
  - Gli aggiornamenti sono stati apportati ai valori predefiniti con le informazioni fornite dall'utente o dai servizi esterni durante la sessione corrente. 

Per utilizzare il metodo, puoi specificarlo in un'espressione in una variabile che definisci nell'oggetto di output. Ad esempio:

```json
{
  "output":{
    "generic":[
      {
        "values": [
          {
          "text": "Response for this node."
          }
        ],
        "response_type": "text",
        "selection_policy": "sequential"
      }
    ],
    "context_eraser": "<? context.clear() ?>"
  }
}

```

#### Cancellazione dell'output
{: #dialog-methods-clearing-output}

Quando utilizzi il metodo `clear()` per cancellare l'oggetto `output`, cancella tutte le variabili ad eccezione di quelle che utilizzi per cancellare l'oggetto di output e le risposte di testo che definisci nel nodo corrente. Inoltre, non cancella queste variabili: 

- `output.nodes_visited`
- `output.nodes_visited_details`

Per utilizzare il metodo, puoi specificarlo in una variabile nell'espressione che definisci nell'output corrente. Ad esempio:

```json
{
  "output":{
    "generic":[
      {
        "values": [
          {
          "text": "Have a great day!"
          }
        ],
        "response_type": "text",
        "selection_policy": "sequential"
      }
    ],
    "output_eraser": "<? output.clear() ?>"
  }
}
```

Se un nodo precedente nella struttura ad albero definisce una risposta di testo `I'm happy to help.` e poi passa a un nodo con l'oggetto di output JSON sopra definito, verrà visualizzato solo `Have a great day.` come risposta. L'output `I'm happy to help.` non viene visualizzato in quanto viene cancellato e sostituito con la risposta di testo del nodo che sta richiamando il metodo `clear()`.

### JSONObject.has(String)

Questo metodo restituisce true se il JSONObject complesso ha una proprietà del nome di input.

Per questo contesto di runtime di dialogo:

```json
{
  "context": {
    "user": {
      "first_name": "John",
      "last_name": "Snow"
    }
  }
}
```
{: codeblock}

Output del nodo di dialogo:

```json
{
  "conditions": "$user.has('first_name')"
}
```
{: codeblock}

Risultato: la condizione è true perché l'oggetto utente contiene la proprietà `first_name`.

### JSONObject.remove(String)

Questo metodo rimuove una proprietà del nome dall'input `JSONObject`. Il `JSONElement` che viene restituito da questo metodo è il `JSONElement` che verrà rimosso.

Per questo contesto di runtime di dialogo:

```json
{
  "context": {
    "user": {
      "first_name": "John",
      "last_name": "Snow"
    }
  }
}
```
{: codeblock}

Output del nodo di dialogo:

```json
{
  "context": {
    "attribute_removed": "<? $user.remove('first_name') ?>"
  }
}
```
{: codeblock}

Risultato:

```json
{
  "context": {
    "user": {
      "last_name": "Snow"
    },
    "attribute_removed": {
      "first_name": "John"
    }
  }
}
```
{: codeblock}

### Supporto com.google.gson.JsonObject
{: #dialog-methods-com.google.gson.JsonObject}

Oltre ai metodi integrati, puoi utilizzare i metodi standard della classe `com.google.gson.JsonObject`.

## Stringhe
{: #dialog-methods-strings}

Questi metodi ti consentono di lavorare con il testo.

Per informazioni su come riconoscere ed estrarre determinati tipi di stringhe, ad esempio i nomi e le posizioni delle persone, dall'input utente, vedi [Entità di sistema](/docs/services/assistant?topic=assistant-system-entities).

**Nota:** per i metodi che prevedono le espressioni regolari, vedi [Riferimento alla sintassi RE2 ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://github.com/google/re2/wiki/Syntax){: new_window} per i dettagli sulla sintassi da utilizzare quando specifichi l'espressione regolare.

### String.append(Object)

Questo metodo aggiunge un oggetto di input alla stringa sotto forma di stringa e restituisce una stringa modificata.

Per questo contesto di runtime di dialogo:

```json
{
  "context": {
    "my_text": "This is a text."
  }
}
```
{: codeblock}

Questa sintassi:

```json
{
  "context": {
    "my_text": "<? $my_text.append(' More text.') ?>"
  }
}
```
{: codeblock}

Risultati in questo output:

```json
{
  "context": {
    "my_text": "This is a text. More text."
  }
}
```
{: codeblock}

### String.contains(String)

Questo metodo restituisce true se la stringa contiene la sottostringa di input.

Input: "Sì, vorrei andare."

Questa sintassi:

```json
{
  "conditions": "input.text.contains('Yes')"
}
```
{: codeblock}

Risultato: la condizione è `true`.

### String.endsWith(String)

Questo metodo restituisce true se la stringa termina con la sottostringa di input.

Per questo input:

```
"What is your name?".
```
{: codeblock}

Questa sintassi:

```json
{
  "conditions": "input.text.endsWith('?')"
}
```
{: codeblock}

Risultato: la condizione è `true`.

### String.extract(String regexp, Integer groupIndex)

Questo metodo restituisce una stringa estratta dall'indice di gruppi specificato dell'espressione regolare di input.

Per questo input:

```
"Hello 123456".
```
{: codeblock}

Questa sintassi:

```json
{
  "context": {
    "number_extract": "<? input.text.extract('[\\d]+',0) ?>"
  }
}
```
{: codeblock}

  **Importante:** per elaborare `\\d` come espressione regolare, devi eseguire l'escape di entrambe le barre rovesciate aggiungendo un altro `\\`: `\\\\d`

Risultato:

```json
{
  "context": {
    "number_extract": "123456"
  }
}
```
{: codeblock}

### String.find(String regexp)

Questo metodo restituisce true se qualsiasi segmento della stringa corrisponde all'espressione regolare di input.  Puoi richiamare questo metodo su un elemento JSONArray o JSONObject e questo convertirà l'array o l'oggetto in una stringa prima di effettuare il confronto.

Per questo input:

```
"Hello 123456".
```
{: codeblock}

Questa sintassi:

```json
{
  "conditions": "input.text.find('^[^\d]*[\d]{6}[^\d]*$')"
}
```
{: codeblock}

Risultato: la condizione è true perché la parte numerica del testo di input corrisponde all'espressione regolare `^[^\d]*[\d]{6}[^\d]*$`.

### String.isEmpty()

Questo metodo restituisce true se la stringa è una stringa vuota, ma non null.

Per questo contesto di runtime di dialogo:

```json
{
  "context": {
    "my_text_variable": ""
  }
}
```
{: codeblock}

Questa sintassi:

```json
{
  "conditions": "$my_text_variable.isEmpty()"
}
```
{: codeblock}

Risultato: la condizione è `true`.

### String.length()

Questo metodo restituisce la lunghezza dei caratteri della stringa.

Per questo input:

```
"Hello"
```
{: codeblock}

Questa sintassi:

```json
{
  "context": {
    "input_length": "<? input.text.length() ?>"
  }
}
```
{: codeblock}

Risultati in questo output:

```json
{
  "context": {
    "input_length": 5
  }
}
```
{: codeblock}

### String.matches(String regexp)

Questo metodo restituisce true se la stringa corrisponde all'espressione regolare di input.

Per questo input:

```
"Hello".
```
{: codeblock}

Questa sintassi:

```json
{
  "conditions": "input.text.matches('^Hello$')"
}
```
{: codeblock}

Risultato: la condizione è true perché il testo di input corrisponde all'espressione regolare `\^Hello\$`.

### String.startsWith(String)

Questo metodo restituisce true se la stringa inizia con la sottostringa di input.

Per questo input:

```
"What is your name?".
```
{: codeblock}

Questa sintassi:

```json
{
  "conditions": "input.text.startsWith('What')"
}
```
{: codeblock}

Risultato: la condizione è `true`.

### String.substring(Integer beginIndex, Integer endIndex)

Questo metodo ottiene una sottostringa con il carattere in `beginIndex` e l'ultimo carattere impostato sull'indice prima di `endIndex`.
Il carattere endIndex non è incluso.

Per questo contesto di runtime di dialogo:

```json
{
  "context": {
    "my_text": "This is a text."
  }
}
```
{: codeblock}

Questa sintassi:

```json
{
  "context": {
    "my_text": "<? $my_text.substring(5, $my_text.length()) ?>"
  }
}
```
{: codeblock}

Risultati in questo output:

```json
{
  "context": {
    "my_text": "is a text."
  }
}
```
{: codeblock}

### String.toLowerCase()

Questo metodo restituisce la stringa originale convertita in lettere minuscole.

Per questo input:

```
"This is A DOG!"
```
{: codeblock}

Questa sintassi:

```json
{
  "context": {
    "input_lower_case": "<? input.text.toLowerCase() ?>"
  }
}
```
{: codeblock}

Risultati in questo output:

```json
{
  "context": {
    "input_upper_case": "this is a dog!"
  }
}
```
{: codeblock}

### String.toUpperCase()

Questo metodo restituisce la stringa originale convertita in lettere maiuscole.

Per questo input:

```
"hi there".
```
{: codeblock}

Questa sintassi:

```json
{
  "context": {
    "input_upper_case": "<? input.text.toUpperCase() ?>"
  }
}
```
{: codeblock}

Risultati in questo output:

```json
{
  "context": {
    "input_upper_case": "HI THERE"
  }
}
```
{: codeblock}

### String.trim()

Questo metodo elimina gli spazi all'inizio e alla fine della stringa e restituisce la stringa modificata.

Per questo contesto di runtime di dialogo:

```json
{
  "context": {
    "my_text": "   something is here    "
  }
}
```
{: codeblock}

Questa sintassi:

```json
{
  "context": {
    "my_text": "<? $my_text.trim() ?>"
  }
}
```
{: codeblock}

Risultati in questo output:

```json
{
  "context": {
    "my_text": "something is here"
  }
}
```
{: codeblock}

### Supporto java.lang.String
{: #java.lang.String}

Oltre ai metodi integrati, puoi utilizzare i metodi standard della classe `java.lang.String`.

#### java.lang.String.format()

Puoi applicare il metodo Java String `format()` standard al testo. Vedi [Riferimento java.util.formatter ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://docs.oracle.com/javase/7/docs/api/java/util/Formatter#syntax){: new_window} per informazioni sulla sintassi da utilizzare per specificare i dettagli del formato.

Ad esempio, la seguente espressione acquisisce tre numeri interi decimali (1, 1 e 2) e li aggiunge ad una frase.

```json
{
  "formatted String": "<? T(java.lang.String).format('%d + %d equals %d', 1, 1, 2) ?>"
}
```
{: codeblock}

Risultato: `1 + 1 equals 2`.

Per modificare la posizione decimale per un numero, utilizza la seguente sintassi: 

```
{
  <? T(String).format('%.2f',<number to format>) ?>
}
```
{: codeblock}

Ad esempio, se la variabile $number che deve essere formattata in dollari americani è `4.5`, una risposta come `Your total is $<? T(String).format('%.2f',$number) ?>` restituisce `Your total is $4.50.`

## Conversione indiretta del tipo di dati
{: #dialog-methods-indirect-type-conversion}

Quando includi un'espressione all'interno del testo, ad esempio come parte di una risposta del nodo, il valore viene rappresentato come una Stringa. Se desideri che l'espressione venga rappresentata con il suo tipo di dati originale, non inserire testo intorno ad essa.

Ad esempio, puoi aggiungere questa espressione a una risposta del nodo di dialogo per restituire le entità riconosciute nell'input utente in formato Stringa:

```json
  The entities are <? entities ?>.
```
{: codeblock}

Se l'utente specifica *Hello now* come input, le entità @sys-date e @sys-time vengono attivate dal riferimento `now`. L'oggetto entità è un array, ma poiché l'espressione è inclusa nel testo, le entità vengono restituite in formato Stringa, in questo modo:

```json
  The entities are 2018-02-02, 14:34:56.
```
{: codeblock}

Se non includi il testo nella risposta, verrà invece restituito un array. Ad esempio, se la risposta viene specificata solo come un'espressione, senza testo intorno.

```
  <? entities ?>
```
{: codeblock}

Le informazioni sull'entità vengono restituite nel tipo di dati nativo, come un array.

```json
[
  {
    "entity":"sys-date","location":[6,9],"value":"2018-02-02","confidence":1,"metadata":{"calendar_type":"GREGORIAN","timezone":"America/New_York"}
  },
  {
    "entity":"sys-time","location":[6,9],"value":"14:33:22","confidence":1,"metadata":{"calendar_type":"GREGORIAN","timezone":"America/New_York"}
  }
  ]
```
{: codeblock}

Come ulteriore esempio, la seguente variabile di contesto $array è un array, ma la variabile di contesto $string_array è una stringa.

```json
{
  "context": {
    "array": [
      "one",
      "two"
    ],
    "array_in_string": "this is my array: $array"
  }
}
```
{: codeblock}

Se controlli i valori di queste variabili di contesto nel riquadro Provalo, vedrai i valori specificati come segue:

**$array** : `["one","two"]`

**$array_in_string** : `"this is my array: [\"one\",\"two\"]"`

Successivamente, puoi eseguire i metodi array sulla variabile $array, ad esempio `<? $array.removeValue('two') ?>`, ma non sulla variabile $array_in_string.
