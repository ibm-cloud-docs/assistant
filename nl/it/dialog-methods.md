---

copyright:
  years: 2015, 2018
lastupdated: "2018-02-05"

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

# Metodi del linguaggio delle espressioni

Puoi elaborare i valori estratti dalle espressioni dell'utente a cui vuoi fare riferimento in una variabile di contesto, in una condizione o altrove nella risposta.
{: shortdesc}

## Sintassi della valutazione

Per espandere i valori delle variabili all'interno di altre variabili oppure applicare metodi al testo di output o alle variabili di contesto, utilizza la sintassi dell'espressione `<? expression ?>`. Ad esempio:

- **Incremento di una proprietà numerica**
    - `"output":{"number":"<? output.number + 1 ?>"}`
- **Richiamo di un metodo su un oggetto**
    - `"context":{"toppings": "<? context.toppings.append( 'onions' ) ?>"}`

Le seguenti sezioni descrivono i metodi che puoi utilizzare per elaborare i valori. Sono organizzate in base al tipo di dati: 

- [Array](dialog-methods.html#arrays)
- [Data e ora](dialog-methods.html#date-time)
- [Numeri](dialog-methods.html#numbers)
- [Oggetti](dialog-methods.html#objects)
- [Stringhe](dialog-methods.html#strings)

## Array
{: #arrays}

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

### JSONArray.contains(object value)

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

### JSONArray.get(integer)

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
    "text": {
      "values": [
        "The first item in the array is <?$nested.array.get(0)?>"
      ],
      "selection_policy": "sequential"
    }
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
    "text": "<? $toppings_array.getRandomItem() ?> is a great choice!"
  }
}
```
{: codeblock}

Risultato: `"ham is a great choice!"` o `"onion is a great choice!"` o `"olives is a great choice!"`

**Nota:** il testo di output risultante viene scelto in modo casuale.

### JSONArray.join(string delimiter)

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
    "text": "This is the array: <? $toppings_array.join(';') ?>"
  }
}
```
{: codeblock}

Risultato:

```json
This is the array: onion;olives;ham;
```
{: codeblock}

### JSONArray.remove(integer)

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

### JSONArray.set(integer index, object value)

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
{: #com.google.gson.JsonArray}

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
{: #date-time}

Sono disponibili diversi metodi per lavorare con la data e ora.

Per informazioni su come riconoscere ed estrarre le informazioni sulla data e sull'ora dall'input utente, vedi le [entità @sys-date e @sys-time](system-entities.html#sys-datetime).

### .after(String date/time)

- Determina se il valore data/ora è dopo l'argomento data/ora.
- Simile a `.before()`.

### .before(String date or time)

- Ad esempio:
- @sys-time.before('12:00:00')
- @sys-date.before('2016-11-21')
- Determina se il valore data/ora è prima dell'argomento data/ora.
- Se si confrontano elementi diversi, come `time vs. date`, `date vs. time` e `time vs. date and time`, il metodo restituisce false e viene stampata un'eccezione nel log JSON di risposta `output.log_messages`.
    - Ad esempio, `@sys-date.before(@sys-time)`.
- Se si confronta `date and time vs. time` il metodo ignora la data e confronta solo le ore.

### now()

- Funzione statica.
- Restituisce una stringa con la data e ora corrente in formato `yyyy-MM-dd HH:mm:ss`.
- Gli altri metodi di data/ora possono essere richiamati sui valori data-ora restituiti da questa funzione e possono essere passati come argomento.
- Se la variabile di contesto `$timezone` è impostata, questa funzione restituisce date e ore nel fuso orario del client.

Esempio di un nodo di dialogo con `now()` utilizzato nel campo di output:

```json
{
  "conditions": "#what_time_is_it",
  "output": {
    "text": "<? now() ?>"
   }
}
```
{: codeblock}

Esempio di `now()` nelle condizioni del nodo (per decidere se è ancora mattina):

```json
{
  "conditions": "now().before('12:00:00')",
  "output": {
    "text": "Good morning!"
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

### Supporto java.util.Date
{: #java.util.Date}

Oltre ai metodi integrati, puoi utilizzare i metodi standard della classe `java.util.Date`. 

#### Calcoli della data

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
{: #numbers}

Questi metodi ti consentono di acquisire e riformattare i valori numerici. 

Per informazioni sulle entità di sistema che possono riconoscere ed estrarre i numeri dall'input utente, vedi [Entità @sys-number](system-entities.html#sys-number).

Se desideri che il servizio riconosca formati numerici specifici nell'input utente, ad esempio riferimenti al numero di ordine, prendi in considerazione di creare un'entità modello per eseguire l'acquisizione. Per ulteriori dettagli, vedi [Creazione di entità](entities.html#creating-entities).

### toDouble()

  Converte l'oggetto o il campo nel tipo di numero Doppio. Puoi chiamare questo metodo su qualsiasi oggetto o campo. Se la conversione non riesce, viene restituito *null*.

### toInt()

  Converte l'oggetto o il campo nel tipo di numero Intero. Puoi chiamare questo metodo su qualsiasi oggetto o campo. Se la conversione non riesce, viene restituito *null*.

### toLong()

  Converte l'oggetto o il campo nel tipo di numero Lungo. Puoi chiamare questo metodo su qualsiasi oggetto o campo. Se la conversione non riesce, viene restituito *null*.

  Se specifichi un tipo di numero Lungo in un'espressione SpEL, devi accodare una lettera `L` al numero per identificarlo come tale. Ad esempio, `5000000000L`. Questa sintassi è obbligatoria per i numeri che non si adattano al tipo Intero a 32 bit. Ad esempio, i numeri maggiori di 2^31 (2,147,483,648) o minori di -2^31 (-2,147,483,648) sono considerati tipi di numero Lungo. I tipi di numero Lungo hanno un valore minimo di -2^63 e un valore massimo di 2^63-1.

### Supporto numero Java
{: #java.lang.Number}

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
    "text": {
      "values": [
        "The bigger number is $bigger_number."
      ],
      "selection_policy": "sequential"
    }
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
    "text": {
      "values": [
        "The smaller number is $smaller_number."
      ],
      "selection_policy": "sequential"
    }
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
    "text": {
      "values": [
        "Your number $base to the second power is $power_of_two."
      ],
      "selection_policy": "sequential"
    }
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
    "text": {
      "values": [
        "Here's a random number between 0 and @sys-number.literal: $answer."
      ],
      "selection_policy": "sequential"
    }
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
{: #objects}

### JSONObject.has(string)

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

### JSONObject.remove(string)

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
{: #com.google.gson.JsonObject}

Oltre ai metodi integrati, puoi utilizzare i metodi standard della classe `com.google.gson.JsonObject`. 

## Stringhe
{: #strings}

Questi metodi ti consentono di lavorare con il testo. 

Per informazioni su come riconoscere ed estrarre determinati tipi di stringhe, ad esempio i nomi e le posizioni delle persone, dall'input utente, vedi [Entità di sistema](system-entities.html).

**Nota:** per i metodi che prevedono le espressioni regolari, vedi [Riferimento alla sintassi RE2 ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://github.com/google/re2/wiki/Syntax){: new_window} per i dettagli sulla sintassi da utilizzare quando specifichi l'espressione regolare. 

### String.append(object)

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

### String.contains(string)

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

### String.endsWith(string)

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

### String.find(string regexp)

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

### String.matches(string regexp)

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

### String.startsWith(string)

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

### String.substring(int beginIndex, int endIndex)

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

Puoi applicare il metodo Java String `format()` standard al testo. Vedi [Riferimento java.util.formatter ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://docs.oracle.com/javase/7/docs/api/java/util/Formatter.html#syntax){: new_window} per informazioni sulla sintassi da utilizzare per specificare i dettagli del formato. 

Ad esempio, la seguente espressione acquisisce tre numeri interi decimali (1, 1 e 2) e li aggiunge ad una frase. 

```json
{
  "formatted String": "<? T(java.lang.String).format('%d + %d equals %d', 1, 1, 2) ?>"
}
```
{: codeblock}

Result: `1 + 1 equals 2`.

## Conversione indiretta del tipo di dati

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
