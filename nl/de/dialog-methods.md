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

# Methoden in der Ausdruckssprache

Sie können Werte verarbeiten, die aus verbalen Äußerungen der Benutzer extrahiert wurden und die Sie in einer Kontextvariablen, in einer Bedingung oder an anderer Stelle in der Antwort referenzieren möchten.
{: shortdesc}

## Auswertungssyntax

Um Variablenwerte innerhalb anderer Variablen zu erweitern oder um Methoden auf Ausgabetext oder Kontextvariablen anzuwenden, verwenden Sie die `<? expression ?>`. Beispiel:

- **Numerische Eigenschaft erhöhen**
    - `"output":{"number":"<? output.number + 1 ?>"}`
- **Methode in einem Objekt aufrufen**
    - `"context":{"toppings": "<? context.toppings.append( 'onions' ) ?>"}`

In den folgenden Abschnitten werden Methoden beschrieben, die Sie zum Verarbeiten von Werten verwenden können. Die Methoden sind nach dem Datentyp geordnet:

- [Arrays](dialog-methods.html#arrays)
- [Datum und Uhrzeit](dialog-methods.html#date-time)
- [Zahlen](dialog-methods.html#numbers)
- [Objekte](dialog-methods.html#objects)
- [Zeichenfolgen](dialog-methods.html#strings)

## Arrays
{: #arrays}

Diese Methoden können nicht verwendet werden, um auf einen Wert in einem Array in einer Knotenbedingung oder Antwortbedingung in demselben Knoten zu prüfen, in dem Sie die Arraywerte festlegen.

### JSONArray.append(object)

Diese Methode hängt einen neuen Wert an das JSON-Array an und gibt das geänderte JSON-Array zurück.

Ausgangspunkt ist der folgende Laufzeitkontext des Dialogmoduls:

```json
{
  "context": {
    "toppings_array": ["Salami", "Paprika"]
  }
}
```
{: codeblock}

Nehmen Sie die folgende Aktualisierung vor:

```json
{
  "context": {
    "toppings_array": "<? $toppings_array.append('Ketchup', 'Tomaten') ?>"
  }
}
```
{: codeblock}

Ergebnis:

```json
{
  "context": {
    "toppings_array": ["Salami", "Paprika", "Ketchup", "Tomaten"]
  }
}
```
{: codeblock}

### JSONArray.contains(object value)

Diese Methode gibt 'true' zurück, wenn das eingegebene JSON-Array den Eingabewert enthält.

Ausgangspunkt ist der folgende Laufzeitkontext des Dialogmoduls, der durch einen vorherigen Knoten oder eine externe Anwendung festgelegt wurde:

```json
{
  "context": {
    "toppings_array": ["Salami", "Paprika", "Schinken"]
  }
}
```
{: codeblock}

Dialogmodulknoten oder Antwortbedingung:

```json
$toppings_array.contains('Schinken')
```
{: codeblock}

Das Ergebnis ist `true`, weil das Array das Element 'Schinken' enthält.

### JSONArray.get(integer)

Diese Methode gibt den Eingabeindex aus dem JSON-Array zurück.

Ausgangspunkt ist der folgende Laufzeitkontext des Dialogmoduls, der durch einen vorherigen Knoten oder eine externe Anwendung festgelegt wurde:

```json
{
  "context": {
    "name": "John",
    "nested": {
      "array": [ "eins", "zwei" ]
    }
  }
}
```
{: codeblock}

Dialogmodulknoten oder Antwortbedingung:

```json
$nested.array.get(0).getAsString().contains('eins')
```
{: codeblock}

Das Ergebnis ist
`true`, weil das verschachtelte Array `eins` als Wert enthält.

Antwort:

```json
"output": {
    "text": {
      "values": [
        "Das erste Element im Array ist <?$nested.array.get(0)?>"
      ],
      "selection_policy": "sequential"
    }
  }
```
{: codeblock}

### JSONArray.getRandomItem()

Diese Methode gibt ein beliebiges Element aus dem eingegebenen JSON-Array zurück.

Ausgangspunkt ist der folgende Laufzeitkontext des Dialogmoduls:

```json
{
  "context": {
    "toppings_array": ["Salami", "Paprika", "Schinken"]
  }
}
```
{: codeblock}

Ausgabe des Dialogmodulknotens:

```json
{
  "output": {
    "text": "<? $toppings_array.getRandomItem() ?> ist eine ausgezeichnete Wahl!"
  }
}
```
{: codeblock}

Ergebnis: `"Schinken ist eine ausgezeichnete Wahl!"` oder `"Salami ist eine ausgezeichnete Wahl!"` oder `"Paprika ist eine ausgezeichnete Wahl!"`

**Hinweis:** Der resultierende Ausgabetext wird zufällig gewählt.

### JSONArray.join(string delimiter)

Diese Methode verknüpft alle Werte in diesem Array zu einer Zeichenfolge. Werte werden in Zeichenfolgen konvertiert und durch das eingegebene Begrenzungszeichen voneinander getrennt.

Ausgangspunkt ist der folgende Laufzeitkontext des Dialogmoduls:

```json
{
  "context": {
    "toppings_array": ["Salami", "Paprika", "Schinken"]
  }
}
```
{: codeblock}

Ausgabe des Dialogmodulknotens:

```json
{
  "output": {
    "text": "Dies ist das Array: <? $toppings_array.join(';') ?>"
  }
}
```
{: codeblock}

Ergebnis:

```json
Dies ist das Array: Salami;Paprika;Schinken;
```
{: codeblock}

### JSONArray.remove(integer)

Diese Methode entfernt das Element an der Indexposition aus dem JSON-Array und gibt das aktualisierte JSON-Array zurück.

Ausgangspunkt ist der folgende Laufzeitkontext des Dialogmoduls:

```json
{
  "context": {
    "toppings_array": ["Salami", "Paprika"]
  }
}
```
{: codeblock}

Nehmen Sie die folgende Aktualisierung vor:

```json
{
  "context": {
    "toppings_array": "<? $toppings_array.remove(0) ?>"
  }
}
```
{: codeblock}

Ergebnis:

```json
{
  "context": {
    "toppings_array": ["Paprika"]
  }
}
```
{: codeblock}

### JSONArray.removeValue(object)

Diese Methode entfernt das erste Vorkommen des Wertes aus dem JSON-Array und gibt das aktualisierte JSON-Array zurück.

Ausgangspunkt ist der folgende Laufzeitkontext des Dialogmoduls:

```json
{
  "context": {
    "toppings_array": ["Salami", "Paprika"]
  }
}
```
{: codeblock}

Nehmen Sie die folgende Aktualisierung vor:

```json
{
  "context": {
    "toppings_array": "<? $toppings_array.removeValue('Salami') ?>"
  }
}
```
{: codeblock}

Ergebnis:

```json
{
  "context": {
    "toppings_array": ["Paprika"]
  }
}
```
{: codeblock}

### JSONArray.set(integer index, object value)

Diese Methode legt den Eingabeindex des JSON-Arrays für den Eingabewert fest und gibt das geänderte JSON-Array zurück.

Ausgangspunkt ist der folgende Laufzeitkontext des Dialogmoduls:

```json
{
  "context": {
    "toppings_array": ["Salami", "Paprika", "Schinken"]
  }
}
```
{: codeblock}

Ausgabe des Dialogmodulknotens:

```json
{
  "context": {
    "toppings_array": "<? $toppings_array.set(1,'Ketchup')?>"
  }
}
```
{: codeblock}

Ergebnis:

```json
{
  "context": {
    "toppings_array": ["Salami", "Ketchup", "Schinken"]
  }
}
```
{: codeblock}

### JSONArray.size()

Diese Methode gibt die Größe des JSON-Arrays als ganze Zahl zurück.

Ausgangspunkt ist der folgende Laufzeitkontext des Dialogmoduls:

```json
{
  "context": {
    "toppings_array": ["Salami", "Paprika"]
  }
}
```
{: codeblock}

Nehmen Sie die folgende Aktualisierung vor:

```json
{
  "context": {
    "toppings_array_size": "<? $toppings_array.size() ?>"
  }
}
```
{: codeblock}

Ergebnis:

```json
{
  "context": {
    "toppings_array_size": 2
  }
}
```
{: codeblock}

### JSONArray split(String regexp)

Diese Methode unterteilt die Eingabezeichenfolge mithilfe des eingegebenen regulären Ausdrucks. Das Ergebnis ist ein JSON-Array mit Zeichenfolgen.

Ausgangspunkt ist die folgende Eingabe:

```
"Bananen;Äpfel;Birnen"
```
{: codeblock}

Verwenden Sie die folgende Syntax:

```json
{
  "context": {
    "array": "<?input.text.split(";")?>
  }
}
```
{: codeblock}

Ergebnis ist die folgende Ausgabe:

```json
{
  "context": {
    "array": [ "Bananen", "Äpfel", "Birnen" ]
  }
}
```
{: codeblock}

### Unterstützung für com.google.gson.JsonArray
{: #com.google.gson.JsonArray}

Zusätzlich zu den integrierten Methoden können Sie Standardmethoden der Klasse `com.google.gson.JsonArray` verwenden.

#### Neues Array

new JsonArray().append('value')

Um ein neues Array zu definieren, das mit von Benutzern bereitgestellten Werten gefüllt werden soll, können Sie ein Array instanziieren. Bei der Instanziierung müssen Sie außerdem einen Platzhalterwert in dem Array hinzufügen. Zu diesem Zweck können Sie die folgende Syntax verwenden:

```json
{
  "context":{
    "answer": "<? output.answer?:new JsonArray().append('temp_value') ?>"
  }
```
{: codeblock}

## Datum und Uhrzeit
{: #date-time}

Für die Arbeit mit Datum und Uhrzeit stehen mehrere Methoden zur Verfügung.

Informationen zum Erkennen und Extrahieren von Datums- und Uhrzeitinformationen aus der Benutzereingabe finden Sie unter [Entitäten '@sys-date' und '@sys-time'](system-entities.html#sys-datetime).

### .after(String date/time)

- Ermittelt, ob der Wert für Datum/Uhrzeit nach dem angegebenen Argument für Datum/Uhrzeit liegt.
- Ist analog zu `.before()`.

### .before(String date or time)

- Beispiel:
- @sys-time.before('12:00:00')
- @sys-date.before('2016-11-21')
- Ermittelt, ob der Wert für Datum/Uhrzeit vor dem angegebenen Argument für Datum/Uhrzeit liegt.
- Falls unterschiedliche Einträge verglichen werden, z. B. `Uhrzeit mit Datum`, `Datum mit Uhrzeit` und `Uhrzeit mit Datum/Uhrzeit`, gibt die Methode 'false' zurück und im JSON-Protokoll für die Ausgabe `output.log_messages` wird eine Ausnahmebedingung aufgezeichnet.
    - Beispiel: `@sys-date.before(@sys-time)`.
- Bei einem Vergleich von `Datum/Uhrzeit mit Uhrzeit` ignoriert die Methode das Datum und vergleicht lediglich die Uhrzeiten.

### now()

- Dies ist eine statische Funktion.
- Sie gibt eine Zeichenfolge mit den aktuellen Werten für Datum und Uhrzeit im Format `jjjj-MM-tt HH:mm:ss` zurück.
- Die anderen Methoden für Datum/Uhrzeit können für Datums-/Uhrzeitwerte aufgerufen werden, die von dieser Funktion zurückgegeben werden, und als deren Argument übergeben werden.
- Falls die Kontextvariable `$timezone` festgelegt ist, gibt diese Funktion Datumsangaben und Uhrzeiten in der Zeitzone des Clients zurück.

Beispiel für einen Dialogmodulknoten, bei dem `now()` im Feld 'output' verwendet wird:

```json
{
  "conditions": "#what_time_is_it",
  "output": {
    "text": "<? now() ?>"
   }
}
```
{: codeblock}

Beispiel für `now()` in Bedingungen eines Knotens (stellt fest, ob es noch Morgen ist):

```json
{
  "conditions": "now().before('12:00:00')",
  "output": {
    "text": "Guten Morgen!"
   }
}
```
{: codeblock}

### .reformatDateTime(String format)

Formatiert Zeichenfolgen mit Datum und Uhrzeit in dem für die Benutzerausgabe gewünschten Format.

Gibt eine formatierte Zeichenfolge gemäß dem angegeben Format zurück:

- `MM/dd/yyyy` für 12/31/2016
- `h a` für 10pm

Zur Rückgabe des Wochentags:

- `E` für Dienstag
- `u` für den Tagesindex (1 = Montag, ..., 7 = Sonntag)

Die folgende Kontextvariablendefinition erstellt beispielsweise eine Variable '$time', die den Wert 17:30:00 als *5:30 PM* speichert.

```json
{
  "context": {
    "time": "<? @sys-time.reformatDateTime('h:mm a') ?>"
  }
}
```
{: codeblock}

Das Format entspricht den Java-Regeln für [SimpleDateFormat ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](http://docs.oracle.com/javase/6/docs/api/java/text/SimpleDateFormat.html){: new_window}.

**Hinweis**: Wenn Sie versuchen, nur die Uhrzeit zu formatieren, wird das Datum als `1970-01-01` behandelt.

### .sameMoment(String date/time)

- Ermittelt, ob der Wert für Datum/Uhrzeit mit dem angegebenen Argument für Datum/Uhrzeit identisch ist.

### .sameOrAfter(String date/time)

- Ermittelt, ob der Wert für Datum/Uhrzeit nach dem angegebenen Argument für Datum/Uhrzeit liegt oder damit identisch ist.
- Ist analog zu `.after()`.

### .sameOrBefore(String date/time)

- Ermittelt, ob der Wert für Datum/Uhrzeit vor dem angegebenen Argument für Datum/Uhrzeit liegt oder damit identisch ist.

### Unterstützung für java.util.Date
{: #java.util.Date}

Zusätzlich zu den integrierten Methoden können Sie Standardmethoden der Klasse `java.util.Date` verwenden.

#### Datumsberechnungen

Um das Datum des Tages abzurufen, der genau eine Woche auf den heutigen Tag folgt, können Sie die folgende Syntax verwenden:

```json
{
  "context": {
    "week_from_today": "<? new Date(new Date().getTime() +
      (7 * (24*60*60*1000L))) ?>"
  }
}
```
{: codeblock}

Dieser Ausdruck ruft zunächst das aktuelle Datum in Millisekunden (seit dem 1. Januar 1970 um 00:00:00 GMT) ab. Außerdem wird berechnet, wie viele Millisekunden 7 Tagen entsprechen. (Die Formel `(24*60*60*1000L)` berechnet die Millisekunden für einen Tag.) Anschließend werden 7 Tage zu dem aktuellen Datum addiert. Das Ergebnis ist das vollständige Datum des Tages, der genau eine Woche auf den heutigen Tag folgt. Beispiel: `Fre, 26. Jan 2018, 16:30:37 UTC`. Beachten Sie, dass die Zeit in der Zeitzone UTC angegeben wird. Der Wert 7 kann jederzeit in eine Variable (z. B. `$number_of_days`) geändert werden, die Sie übergeben können. Sorgen Sie nur dafür, dass der zugehörige Wert vor der Auswertung dieses Ausdrucks festgelegt wird.

Wenn das Datum anschließend mit einem anderen Datum verglichen werden soll, das von dem Service generiert wird, dann muss das Datum neu formatiert werden. Systementitäten (z. B. `@sys-date`) und andere integrierte Methoden (z. B. `now()`) wandeln Datumsangaben in das Format `jjjj-MM-tt` um.

```json
{
  "context": {
    "week_from_today": "<? new Date(new Date().getTime() +
      (7 * (24*60*60*1000L))).format('jjjj-MM-tt') ?>"
  }
}
```
{: codeblock}

Das Ergebnis der Neuformatierung ist das Datum `2018-01-26`. Mit einem Ausdruck wie `@sys-date.after($week_from_today)` in einer Antwortbedingung können Sie nun das Datum aus der Benutzereingabe mit dem in der Kontextvariablen gespeicherten Datum vergleichen.

Der folgende Ausdruck berechnet die Uhrzeit, die 3 Stunden nach der aktuellen Uhrzeit folgt.

```json
{
  "context": {
    "future_time": "<? new Date(new Date().getTime() + (3 * (60*60*1000L)) -
      (5 * (60*60*1000L))).format('h:mm a') ?>"
  }
}
```
{: codeblock}

Der Wert `(60*60*1000L)` entspricht einer Stunde, ausgedrückt in Millisekunden. Dieser Ausdruck addiert 3 Stunden zu der aktuellen Uhrzeit. Anschließend wird die Uhrzeit aus einer UTC-Zeitzone in eine EST-Zeitzone umgerechnet, indem 5 Stunden abgezogen werden. Außerdem werden die Datumswerte so umformatiert, dass sie Stunden und Minuten sowie AM oder PM enthalten.

## Zahlen
{: #numbers}

Diese Methoden unterstützen das Abrufen und Umformatieren von Zahlenwerten.

Informationen zu Systementitäten, die Zahlen aus der Benutzereingabe erkennen und extrahieren können, finden Sie unter [Entität '@sys-number'](system-entities.html#sys-number).

Wenn der Service bestimmte Zahlenformate in der Benutzereingabe erkennen soll (z. B. Verweise auf Bestellnummern), sollten Sie in Betracht ziehen, eine Musterentität zum Erfassen solcher Zahlenformate zu erstellen. Weitere Details finden Sie unter [Entitäten erstellen](entities.html#creating-entities).

### toDouble()

  Konvertiert das Objekt oder Feld in den Zahlentyp 'Double'. Sie können diese Methode für jedes beliebige Objekt oder Feld aufrufen. Falls die Konvertierung fehlschlägt, wird *null* zurückgegeben.

### toInt()

  Konvertiert das Objekt oder Feld in den Zahlentyp 'Integer'. Sie können diese Methode für jedes beliebige Objekt oder Feld aufrufen. Falls die Konvertierung fehlschlägt, wird *null* zurückgegeben.

### toLong()

  Konvertiert das Objekt oder Feld in den Zahlentyp 'Long'. Sie können diese Methode für jedes beliebige Objekt oder Feld aufrufen. Falls die Konvertierung fehlschlägt, wird *null* zurückgegeben.

  Wenn Sie einen Zahlentyp 'Long' in einem SpEL-Ausdruck angeben, müssen Sie den Buchstaben `L` an die Zahl anfügen, um diesen Zahlentyp zu identifizieren. Beispiel: `5000000000L`. Diese Syntax ist für alle Zahlen erforderlich, die nicht in das Format einer 32-Bit-Ganzzahl passen. Beispiel: Zahlen, die größer als 2^31 (2,147,483,648) oder kleiner als -2^31 (-2,147,483,648) sind, werden als Zahlentyp 'Long' eingestuft. Zahlen des Typs 'Long' liegen zwischen dem Minimalwert -2^63 und dem Maximalwert 2^63-1.

### Zahlenunterstützung in Java
{: #java.lang.Number}

### java.lang.Math()

Führt grundlegende Zahlenoperationen aus.

Sie können die Klassenmethoden einschließlich der folgenden verwenden:

- max()

```json
{
  "context": {
    "bigger_number": "<? T(Math).max($number1,$number2) ?>"
  },
  "output": {
    "text": {
      "values": [
        "Die größere Zahl ist $bigger_number."
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
        "Die kleinere Zahl ist $smaller_number."
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
        "Die zweite Potenz der Zahl $base ist $power_of_two."
      ],
      "selection_policy": "sequential"
    }
  }
}
```
{: codeblock}

Informationen zu weiteren Methoden finden Sie unter [java.lang.Math - Referenzinformationen](https://docs.oracle.com/javase/7/docs/api/java/lang/Math.html).

### java.util.Random()

Gibt eine Zufallszahl zurück. Sie können eine der folgenden Syntaxoptionen verwenden:

- Um einen zufälligen booleschen Wert (true oder false) zurückzugeben, verwenden Sie `<?new Random().nextBoolean()?>`.
- Um eine zufällige Zahl des Typs 'Double' zwischen 0 (eingeschlossen) und 1 (ausgeschlossen) zurückzugeben, verwenden Sie `<?new Random().nextDouble()?>`
- Um eine zufällige Ganzzahl zwischen 0 (eingeschlossen) und einer von Ihnen angegebenen Zahl zurückzugeben, verwenden Sie `<?new Random().nextInt(n)?>`, wobei n die obere Grenze des gewünschten Zahlenbereichs + 1 ist.
  Beispiel: Wenn Sie eine Zufallszahl zwischen 0 und 10 zurückgeben wollen, geben Sie Folgendes an: `<?new Random().nextInt(11)?>`.
- Um eine Zufallszahl aus dem vollständigen Ganzzahlwertbereich (-2147483648 bis 2147483648) zurückzugeben, verwenden Sie `<?new Random().nextInt()?>`.

Sie könnten beispielsweise einen Dialogmodulknoten erstellen, der durch die Absicht '#random_number' ausgelöst wird. Die erste Antwortbedingung könnte wie folgt aussehen:

```json
Condition = @sys-number
{
  "context": {
    "answer": "<? new Random().nextInt(@sys-number.numeric_value + 1) ?>"
  },
  "output": {
    "text": {
      "values": [
        "Hier ist eine Zufallszahl zwischen 0 und @sys-number.literal: $answer."
      ],
      "selection_policy": "sequential"
    }
  }
}
```
{: codeblock}

Informationen zu weiteren Methoden finden Sie unter [java.util.Random - Referenzinformationen](https://docs.oracle.com/javase/7/docs/api/java/util/Random.html).

Sie können auch Standardmethoden der folgenden Klassen verwenden:

- `java.lang.Byte`
- `java.lang.Integer`
- `java.lang.Long`
- `java.lang.Double`
- `java.lang.Short`
- `java.lang.Float`

## Objekte
{: #objects}

### JSONObject.has(string)

Diese Methode gibt 'true' zurück, falls das komplexe JSON-Objekt eine Eigenschaft mit dem eingegebenen Namen besitzt.

Ausgangspunkt ist der folgende Laufzeitkontext des Dialogmoduls:

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

Ausgabe des Dialogmodulknotens:

```json
{
  "conditions": "$user.has('first_name')"
}
```
{: codeblock}

Ergebnis: Diese Bedingung wird mit 'true' ausgewertet, weil das Objekt die Eigenschaft `first_name` enthält.

### JSONObject.remove(string)

Diese Methode entfernt eine Eigenschaft mit dem angegebenen Namen aus dem eingegebenen `JSON-Objekt`. Das von dieser Methode zurückgegebene `JSON-Element` ist das `JSON-Element`, das entfernt wird.

Ausgangspunkt ist der folgende Laufzeitkontext des Dialogmoduls:

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

Ausgabe des Dialogmodulknotens:

```json
{
  "context": {
    "attribute_removed": "<? $user.remove('first_name') ?>"
  }
}
```
{: codeblock}

Ergebnis:

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

### Unterstützung für com.google.gson.JsonObject
{: #com.google.gson.JsonObject}

Zusätzlich zu den integrierten Methoden können Sie Standardmethoden der Klasse `com.google.gson.JsonObject` verwenden.

## Zeichenfolgen
{: #strings}

Diese Methoden unterstützen das Arbeiten mit Text.

Informationen zum Erkennen und Extrahieren bestimmter Zeichenfolgetypen wie Personennamen und Positionen aus der Benutzereingabe finden Sie unter [Systementitäten](system-entities.html).

**Hinweis:** Für Methoden, die reguläre Ausdrücke einbeziehen, finden Sie auf der Seite [RE2 Syntax reference ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/google/re2/wiki/Syntax){: new_window} Details über die Syntax zum Angeben des regulären Ausdrucks.

### String.append(object)

Diese Methode hängt ein Eingabeobjekt als Zeichenfolge an die Zeichenfolge an und gibt eine geänderte Zeichenfolge zurück.

Ausgangspunkt ist der folgende Laufzeitkontext des Dialogmoduls:

```json
{
  "context": {
    "my_text": "Dies ist ein Text."
  }
}
```
{: codeblock}

Verwenden Sie die folgende Syntax:

```json
{
  "context": {
    "my_text": "<? $my_text.append(' Mehr Text.') ?>"
  }
}
```
{: codeblock}

Ergebnis ist die folgende Ausgabe:

```json
{
  "context": {
    "my_text": "Dies ist ein Text. Mehr Text."
  }
}
```
{: codeblock}

### String.contains(string)

Diese Methode gibt 'true' zurück, wenn die Zeichenfolge die eingegebene Teilzeichenfolge enthält.

Eingabe: "Ja, ich möchte gehen."

Verwenden Sie die folgende Syntax:

```json
{
  "conditions": "input.text.contains('Ja')"
}
```
{: codeblock}

Ergebnis: Die Bedingung wird mit `true` ausgewertet.

### String.endsWith(string)

Diese Methode gibt 'true' zurück, wenn die Zeichenfolge mit der eingegebenen Teilzeichenfolge endet.

Ausgangspunkt ist die folgende Eingabe:

```
"Wie heißen Sie?".
```
{: codeblock}

Verwenden Sie die folgende Syntax:

```json
{
  "conditions": "input.text.endsWith('?')"
}
```
{: codeblock}

Ergebnis: Die Bedingung wird mit `true` ausgewertet.

### String.extract(String regexp, Integer groupIndex)

Diese Methode gibt eine Zeichenfolge zurück, die durch den angegebenen Gruppenindex aus dem eingegebenen regulären Ausdruck extrahiert wurde.

Ausgangspunkt ist die folgende Eingabe:

```
"Hallo 123456".
```
{: codeblock}

Verwenden Sie die folgende Syntax:

```json
{
  "context": {
    "number_extract": "<? input.text.extract('[\\d]+',0) ?>"
  }
}
```
{: codeblock}

  **Wichtig:** Damit `\\d` als regulärer Ausdruck verarbeitet wird, müssen Sie für beide umgekehrten Schrägstriche Escapezeichen verwenden, indem Sie `\\` noch einmal hinzufügen: `\\\\d`

Ergebnis:

```json
{
  "context": {
    "number_extract": "123456"
  }
}
```
{: codeblock}

### String.find(zeichenfolge regulärer_ausdruck)

Diese Methode gibt 'true' zurück, falls ein Segment der Zeichenfolge mit dem eingegebenen regulären Ausdruck übereinstimmt.  Sie können diese Methode für ein JSON-Array oder ein JSON-Objekt aufrufen. Sie konvertiert das Array oder Objekt in eine Zeichenfolge, bevor der Vergleich vorgenommen wird.

Ausgangspunkt ist die folgende Eingabe:

```
"Hallo 123456".
```
{: codeblock}

Verwenden Sie die folgende Syntax:

```json
{
  "conditions": "input.text.find('^[^\d]*[\d]{6}[^\d]*$')"
}
```
{: codeblock}

Ergebnis: Die Bedingung wird mit 'true' ausgewertet, weil der numerische Teil des Eingabetextes mit dem regulären Ausdruck `^[^\d]*[\d]{6}[^\d]*$` übereinstimmt.

### String.isEmpty()

Diese Methode gibt 'true' zurück, wenn die Zeichenfolge eine leere Zeichenfolge, jedoch nicht null ist.

Ausgangspunkt ist der folgende Laufzeitkontext des Dialogmoduls:

```json
{
  "context": {
    "my_text_variable": ""
  }
}
```
{: codeblock}

Verwenden Sie die folgende Syntax:

```json
{
  "conditions": "$my_text_variable.isEmpty()"
}
```
{: codeblock}

Ergebnis: Die Bedingung wird mit `true` ausgewertet.

### String.length()

Diese Methode gibt die Zeichenlänge der Zeichenfolge zurück.

Ausgangspunkt ist die folgende Eingabe:

```
"Hallo"
```
{: codeblock}

Verwenden Sie die folgende Syntax:

```json
{
  "context": {
    "input_length": "<? input.text.length() ?>"
  }
}
```
{: codeblock}

Ergebnis ist die folgende Ausgabe:

```json
{
  "context": {
    "input_length": 5
  }
}
```
{: codeblock}

### String.matches(string regexp)

Diese Methode gibt 'true' zurück, falls die Zeichenfolge mit dem eingegebenen regulären Ausdruck übereinstimmt.

Ausgangspunkt ist die folgende Eingabe:

```
"Hallo".
```
{: codeblock}

Verwenden Sie die folgende Syntax:

```json
{
  "conditions": "input.text.matches('^Hallo$')"
}
```
{: codeblock}

Ergebnis: Die Bedingung wird mit 'true' ausgewertet, weil der Eingabetext mit dem regulären Ausdruck `\^Hallo\$` übereinstimmt.

### String.startsWith(string)

Diese Methode gibt 'true' zurück, wenn die Zeichenfolge mit der eingegebenen Teilzeichenfolge beginnt.

Ausgangspunkt ist die folgende Eingabe:

```
"Wie heißen Sie?".
```
{: codeblock}

Verwenden Sie die folgende Syntax:

```json
{
  "conditions": "input.text.startsWith('Wie')"
}
```
{: codeblock}

Ergebnis: Die Bedingung wird mit `true` ausgewertet.

### String.substring(int beginIndex, int endIndex)

Diese Methode ruft eine Teilzeichenfolge mit dem Zeichen an der Position `beginIndex` und dem letzten auf Index gesetzten Zeichen vor `endIndex` ab.
Das Zeichen bei 'endIndex' wird nicht einbezogen.

Ausgangspunkt ist der folgende Laufzeitkontext des Dialogmoduls:

```json
{
  "context": {
    "my_text": "Dies ist ein Text."
  }
}
```
{: codeblock}

Verwenden Sie die folgende Syntax:

```json
{
  "context": {
    "my_text": "<? $my_text.substring(5, $my_text.length()) ?>"
  }
}
```
{: codeblock}

Ergebnis ist die folgende Ausgabe:

```json
{
  "context": {
    "my_text": "ist ein Text."
  }
}
```
{: codeblock}

### String.toLowerCase()

Diese Methode gibt die ursprüngliche Zeichenfolge in Kleinbuchstaben konvertiert zurück.

Ausgangspunkt ist die folgende Eingabe:

```
"Dies ist EIN HUND!"
```
{: codeblock}

Verwenden Sie die folgende Syntax:

```json
{
  "context": {
    "input_lower_case": "<? input.text.toLowerCase() ?>"
  }
}
```
{: codeblock}

Ergebnis ist die folgende Ausgabe:

```json
{
  "context": {
    "input_upper_case": "dies ist ein Hund!"
  }
}
```
{: codeblock}

### String.toUpperCase()

Diese Methode gibt die ursprüngliche Zeichenfolge in Großbuchstaben konvertiert zurück.

Ausgangspunkt ist die folgende Eingabe:

```
"guten tag".
```
{: codeblock}

Verwenden Sie die folgende Syntax:

```json
{
  "context": {
    "input_upper_case": "<? input.text.toUpperCase() ?>"
  }
}
```
{: codeblock}

Ergebnis ist die folgende Ausgabe:

```json
{
  "context": {
    "input_upper_case": "GUTEN TAG"
  }
}
```
{: codeblock}

### String.trim()

Diese Methode schneidet alle Leerzeichen am Beginn und am Ende der Zeichenfolge ab und gibt die geänderte Zeichenfolge zurück.

Ausgangspunkt ist der folgende Laufzeitkontext des Dialogmoduls:

```json
{
  "context": {
    "my_text": "   hier ist etwas    "
  }
}
```
{: codeblock}

Verwenden Sie die folgende Syntax:

```json
{
  "context": {
    "my_text": "<? $my_text.trim() ?>"
  }
}
```
{: codeblock}

Ergebnis ist die folgende Ausgabe:

```json
{
  "context": {
    "my_text": "hier ist etwas"
  }
}
```
{: codeblock}

### Unterstützung für java.lang.String
{: #java.lang.String}

Zusätzlich zu den integrierten Methoden können Sie Standardmethoden der Klasse `java.lang.String` verwenden.

#### java.lang.String.format()

Sie können die Standardmethode für Java-Zeichenfolgen `format()` auf Text anwenden. Informationen über die Syntax zum Angeben der Formatdetails finden Sie unter [java.util.formatter reference ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://docs.oracle.com/javase/7/docs/api/java/util/Formatter.html#syntax){: new_window}.

Beispiel: Der folgende Ausdruck akzeptiert drei dezimale Ganzzahlen (1, 1 und 2) und fügt Sie zu einem Satz hinzu.

```json
{
  "formatted String": "<? T(java.lang.String).format('%d + %d gleich %d', 1, 1, 2) ?>"
}
```
{: codeblock}

Ergebnis: `1 + 1 gleich 2`.

## Indirekte Datentypumwandlung

Wenn Sie einen Ausdruck mit Text umgeben (z. B. als Teil einer Knotenantwort), wird der Wert als Zeichenfolge wiedergegeben. Wenn der Ausdruck als der ursprüngliche Datentyp dargestellt werden soll, umgeben Sie ihn nicht mit Text.

Sie können beispielsweise den folgenden Ausdruck zur Antwort eines Dialogmodulknotens hinzufügen, damit die in der Benutzereingabe erkannten Entitäten im Zeichenfolgeformat zurückgegeben werden:

```json
  The entities are <? entities ?>.
```
{: codeblock}

Falls der Benutzer *Hello now* als Eingabe angibt, werden die Entitäten '@sys-date' und '@sys-time' durch die Angabe `now` ausgelöst. Das Entitätsobjekt ist ein Array, aber da der Ausdruck Text enthält, werden die Entitäten wie folgt im Zeichenfolgeformat zurückgegeben:

```json
  The entities are 2018-02-02, 14:34:56.
```
{: codeblock}

Wenn Sie keinen Text in die Antwort einfügen, wird stattdessen ein Array zurückgegeben. Dies ist beispielsweise der Fall, wenn die Antwort nur als Ausdruck (ohne umgebenden Text) angegeben wird.

```
  <? entities ?>
```
{: codeblock}

Die Entitätsinformationen werden im nativen Datentyp (d. h. als Array) zurückgegeben.

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

Ein weiteres Beispiel: Die folgende Kontextvariable '$array' ist ein Array, aber die Kontextvariable '$string_array' ist eine Zeichenfolge.

```json
{
  "context": {
    "array": [
      "eins",
      "zwei"
    ],
    "array_in_string": "Dies ist mein Array: $array"
  }
}
```
{: codeblock}

Falls Sie die Werte dieser Kontextvariablen im Fenster 'Ausprobieren' überprüfen, werden ihre Werte dort folgendermaßen angegeben:

**$array** : `["eins","zwei"]`

**$array_in_string** : `"Dies ist mein Array: [\"eins\",\"zwei\"]"`

Sie können nachfolgend Arraymethoden für die Variable '$array' ausführen, z. B. `<? $array.removeValue('two') ?>`, jedoch nicht für die Variable '$array_in_string'.
