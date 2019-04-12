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

# Methoden in der Ausdruckssprache
{: #dialog-methods}

Sie können Werte verarbeiten, die aus verbalen Äußerungen der Benutzer extrahiert wurden und die Sie in einer Kontextvariablen, in einer Bedingung oder an anderer Stelle in der Antwort referenzieren möchten.
{: shortdesc}

## Auswertungssyntax
{: #dialog-methods-evaluation-syntax}

Um Variablenwerte innerhalb anderer Variablen zu erweitern oder um Methoden auf Ausgabetext oder Kontextvariablen anzuwenden, verwenden Sie die `<? expression ?>`. Beispiel:

- **Numerische Eigenschaft erhöhen**
    - `"output":{"number":"<? output.number + 1 ?>"}`
- **Methode in einem Objekt aufrufen**
    - `"context":{"toppings": "<? context.toppings.append( 'onions' ) ?>"}`

In den folgenden Abschnitten werden Methoden beschrieben, die Sie zum Verarbeiten von Werten verwenden können. Die Methoden sind nach dem Datentyp geordnet:

- [Arrays](#dialog-methods-arrays)
- [Datum und Uhrzeit](#dialog-methods-date-time)
- [Zahlen](#dialog-methods-numbers)
- [Objekte](#dialog-methods-objects)
- [Zeichenfolgen](#dialog-methods-strings)

## Arrays
{: #dialog-methods-arrays}

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

### JSONArray.clear()

Diese Methode löscht alle Werte in dem Array und gibt null zurück.

Verwenden Sie den folgenden Ausdruck in der Ausgabe, um ein Feld zu definieren, das den Inhalt eines Arrays löscht, in dem Sie die Werte einer Kontextvariablen ($toppings_array) gespeichert haben.

```json
{
  "output":{
    "array_eraser": "<? $toppings_array.clear() ?>"
  }
}
```
{: codeblock}

Nachfolgende Verweise auf die Kontextvariable '$toppings_array' geben nur '[]' zurück.

### JSONArray.contains(Object value)

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

### JSONArray.containsIntent(String intent_name, Double min_score, [Integer top_n])
{: #dialog-methods-array-containsIntent}

Diese Methode gibt `true` zurück, wenn das JSON-Array `intents` die angegebene Absicht enthält und der Konfidenzwert dieser Absicht größer-gleich dem angegebenen Mindestwert ist. Sie können optional eine Zahl angeben, um festzulegen, dass die Absicht innerhalb dieser Anzahl der ranghöchsten Elemente im Array enthalten sein muss.

Diese Methode gibt `false` zurück, wenn die angegebene Absicht nicht im Array enthalten ist, keinen Konfidenzwert größer-gleich dem Mindestkonfidenzwert aufweist oder im Array unterhalb der angegebenen Indexposition steht. 

Sobald eine Benutzereingabe übergeben wird, generiert der Service automatisch ein Array `intents` mit einer Liste der in der Eingabe erkannten Absichten. In dem Array werden alle vom Service erkannten Absichten in absteigender Reihenfolge ihrer Konfidenzwerte aufgelistet.

Sie können diese Methode in einer Knotenbedingung verwenden, um das Vorhandensein einer Absicht zu prüfen und um einen Grenzwert für die Konfidenzbewertung festzulegen, der eingehalten werden muss, damit der Knoten verarbeitet und die zugehörige Antwort zurückgegeben wird. 

Sie können beispielsweise den folgenden Ausdruck in einer Knotenbedingung verwenden, damit der Dialogmodulknoten nur ausgelöst wird, wenn die folgenden Bedingungen zutreffen: 

- Die Absicht `#General_Ending` ist vorhanden.
- Die Konfidenzbewertung für die Absicht `#General_Ending` ist höher als 80 %.
- Die Absicht `#General_Ending` ist eine der beiden am häufigsten vorkommenden Absichten im Array 'intents'.

```bash
intents.containsIntent("General_Ending", 0.8, 2)
```
{: codeblock}

### JSONArray.filter(temp, "temp.property operator comparison_value")
{: #dialog-methods-array-filter}

Filtert ein Array, indem der Wert jedes Array-Elements mit einem von Ihnen angegebenen Wert verglichen wird. Diese Methode entspricht weitgehend einer [Erfassungsprognose](#collection-projection). Eine Erfassungsprognose gibt ein gefiltertes Array zurück, basierend auf dem Namen aus einem Name/Wert-Paar in einem Array-Element. Die Filtermethode gibt ein gefiltertes Array zurück, basierend auf dem Wert aus einem Name/Wert-Paar in einem Array-Element.

Der Filterausdruck besteht aus den folgenden Werten: 

- `temp`: Der Name einer temporären Variablen, die beim Auswerten der einzelnen Array-Elemente verwendet wird. Beispiel: `city`.
- `property`: Die Elementeigenschaft, mit der `comparison_value` verglichen werden soll. Geben Sie die Eigenschaft als Eigenschaft der temporären Variablen an, die Sie im ersten Parameter angegeben haben. Verwenden Sie die Syntax `temp.property`. Beispiel: Wenn `latitude` ein gültiger Elementname für ein Name/Wert-Paar in dem Array ist, geben Sie `city.latitude` als Eigenschaft an.
- `operator`: Der Operator, der zum Vergleichen des Eigenschaftswerts mit dem Vergleichswert (`comparison_value`) verwendet werden soll.

    Die folgenden Operatoren werden unterstützt: 

    <table>
    <caption>Unterstützte Filteroperatoren</caption>
    <tr>
      <th>Operator</th>
      <th>Beschreibung</th>
    </tr>
    <tr>
      <td>`==`</td>
      <td>Gleich</td>
    </tr>
    <tr>
      <td>`>`</td>
      <td>Größer als</td>
    </tr>
    <tr>
      <td>`<`</td>
      <td>Kleiner als</td>
    </tr>
    <tr>
      <td>`>=`</td>
      <td>Größer-gleich</td>
    </tr>
    <tr>
      <td>`<=`</td>
      <td>Kleiner-gleich</td>
    </tr>
    <tr>
      <td>`!=`</td>
      <td>Ungleich</td>
    </tr>
    </table>

- `comparison_value`: Der Wert, mit dem der Eigenschaftswert jedes Array-Elements verglichen werden soll. Wenn Sie einen Wert angeben möchten, der je nach Benutzereingabe variieren kann, verwenden Sie eine Kontextvariable oder Entität als Wert. Wenn Sie einen Wert angeben, der variieren kann, fügen Sie entsprechende Logik hinzu, um sicherzustellen, dass der Wert `comparison_value` am Zeitpunkt der Auswertung gültig ist. Andernfalls tritt ein Fehler auf. 

#### Filterbeispiel 1

Sie können die Filtermethode beispielsweise verwenden, um nach der Auswertung eines Arrays, das eine Reihe von Städtenamen und die zugehörigen Einwohnerzahlen enthält, ein kleineres Array zurückzugeben, das nur Städte mit mehr als 5 Millionen Einwohnern enthält.

Die folgende Kontextvariable `$cities` enthält ein Array von Objekten. Jedes Objekt enthält eine Eigenschaft `name` und eine Eigenschaft `population`.

```json
[
   {
      "name":"Tokio",
      "population":9273000
   },
   {
      "name":"Rom",
      "population":2868104
   },
   {
      "name":"Peking",
      "population":20693000
   },
   {
      "name":"Paris",
      "population":2241346
   }
]
```
{: codeblock}

Im nachfolgenden Beispiel ist `city` der Name der beliebigen temporären Variablen. Der SpEL-Ausdruck filtert das Array `$cities` so, dass es nur Städte mit mehr als 5 Millionen Einwohnern enthält:

```bash
$cities.filter("city", "city.population > 5000000")
```
{: codeblock}

Der Ausdruck gibt das folgende gefilterte Array zurück: 

```json
[
   {
      "name":"Tokio",
      "population":9273000
   },
   {
      "name":"Peking",
      "population":20693000
   }
]
```
{: codeblock}

Mithilfe einer Erfassungsprognose können Sie ein neues Array erstellen, dass nur die Städtenamen aus dem von der Filtermethode zurückgegebenen Array enthält. Anschließend können Sie mit der Methode `join` die Werte der beiden Namenselemente aus dem Array als Zeichenfolge zurückgeben und die Werte jeweils durch ein Komma und ein Leerzeichen trennen. 

```bash
Zu den Städten mit mehr als 5 Millionen Einwohnern gehören <?  T(String).join(", ",($cities.filter("city", "city.population > 5000000")).![name]) ?>.
```
{: codeblock}

Die resultierende Antwort lautet wie folgt: `Zu den Städten mit mehr als 5 Millionen Einwohnern gehören Tokio, Beijing.`

#### Filterbeispiel 2

Der Vorteil der Filtermethode ist, dass der Wert `comparison_value` nicht fest codiert werden muss. Im vorliegenden Beispiel wird der fest codierte Wert 5000000 durch eine Kontextvariable ersetzt. 

Die Kontextvariable `$population_min` in diesem Beispiel enthält die Zahl `5000000`. Der Name der beliebigen temporären Variablen lautet `city`. Der SpEL-Ausdruck filtert das Array `$cities` so, dass es nur Städte mit mehr als 5 Millionen Einwohnern enthält:

```bash
$cities.filter("city", "city.population > $population_min")
```
{: codeblock}

Der Ausdruck gibt das folgende gefilterte Array zurück: 

```json
[
   {
      "name":"Tokio",
      "population":9273000
   },
   {
      "name":"Peking",
      "population":20693000
   }
]
```
{: codeblock}

Stellen Sie beim Vergleichen von Zahlenwerten sicher, dass die am Vergleich beteiligte Kontextvariable auf einen gültigen Wert gesetzt wird, bevor die Filtermethode ausgelöst wird. Beachten Sie, dass `null` ein gültiger Wert sein kann, wenn es in dem Array-Element enthalten sein kann, das mit diesem Wert verglichen wird. Angenommen, das Name/Wert-Paar für die Einwohnerzahl von Tokyo ist `"population":null` und der Vergleichsausdruck lautet `"city.population == $population_min"`. In diesem Fall ist `null` ein gültiger Wert für die Kontextvariable `$population_min`.
{: tip}

Sie können für die Antwort des Dialogmodulknotens einen Ausdruck wie den folgenden verwenden: 

```bash
Zu den Städten mit mehr als $population_min Menschen gehören <?  T(String).join(", ",($cities.filter("city", "city.population > $population_min")).![name]) ?>.
```
{: codeblock}

Die resultierende Antwort lautet wie folgt: `Zu den Städten mit mehr als 5000000 Menschen gehören Tokio, Peking.`

#### Filterbeispiel 3

In diesem Beispiel wird als Vergleichswert (`comparison_value`) ein Entitätsname verwendet. Die Benutzereingabe lautet: `Wie viele Einwohner hat Tokio?` Der Name der beliebigen temporären Variablen ist `y`. Sie haben eine Entität namens `@city` erstellt, die Städtenamen (einschließlich `Tokio`) erkennt.

```bash
$cities.filter("y", "y.name == @city")
```

Der Ausdruck gibt das folgende Array zurück: 

```json
[
   {
      "name":"Tokio",
      "population":9273000
   }
]
```
{: codeblock}

Sie können eine Erfassungsprognose verwenden, um ein Array zu generieren, in dem nur das Element 'population' aus dem ursprünglichen Array enthalten ist, und anschließend mit der Methode `get` den Wert des Elements 'population' zurückgeben.

```bash
Die Einwohnerzahl von @city beträgt <? ($cities.filter("y", "y.name == @city").![population]).get(0) ?>.
```
{: codeblock}

Der Ausdruck gibt Folgendes zurück: `Die Einwohnerzahl von Tokio beträgt 9273000.`

### JSONArray.get(Integer)

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
"output":{
  "generic":[
      {
      "values": [
        {
        "text" : "Das erste Element in dem Array ist <?$nested.array.get(0)?>"
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
  "output":{
  "generic":[
      {
      "values": [
        {
    "text": "<? $toppings_array.getRandomItem() ?> ist eine ausgezeichnete Wahl!"
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

Ergebnis: `"Schinken ist eine ausgezeichnete Wahl!"` oder `"Salami ist eine ausgezeichnete Wahl!"` oder `"Paprika ist eine ausgezeichnete Wahl!"`

**Hinweis:** Der resultierende Ausgabetext wird zufällig gewählt.

### JSONArray.indexOf(value)
{: #dialog-methods-array-indexOf}

Diese Methode gibt die Indexnummer des Elements in dem Array zurück, das mit dem Wert übereinstimmt, den Sie als Parameter angeben, oder `-1`, wenn der Wert im Array nicht gefunden wird. Der Wert kann eine Zeichenfolge (`"Schule"`), ein Integer (`8`) oder ein Doppelzeichen (`9,1`) sein. Der Wert muss eine exakte Übereinstimmung sein und die Groß-/Kleinschreibung muss beachtet werden. 

Arrays sind beispielsweise in den folgenden Konextvariablen enthalten:

```json
{
  "context": {
    "array1": ["Mary","Lamb","School"],
    "array2": [8,9,10],
    "array3": [8.1,9.1,10.1]
  }
}
```

Mit dem folgenden Ausdruck kann der Array-Index ermittelt werden, in dem der Wert angegeben ist: 

```bash
<? $array1.indexOf("Mary") ?> returns `0`
<? $array2.indexOf(9) ?> returns `1`
<? $array3.indexOf(10.1) ?> returns `2`
```

Diese Methode kann beispielsweise hilfreich sein, um den Index eines Elements in einem Array 'intents' abzurufen. Die Methode `indexOf` kann auf das Array mit Absichten angewendet werden, das bei jeder Auswertung von Benutzereingaben generiert wird, um die Array-Indexnummer einer bestimmten Absicht zu ermitteln.

```bash
intents.indexOf("General_Greetings")
```
{: codeblock}

Wenn Sie den Konfidenzwert für eine bestimmte Absicht kennen, können Sie den oben angegebenen Ausdruck als Wert *`index`* an einen Ausdruck mit der Syntax `intents[`*`index`*`].confidence` übergeben. Beispiel:

```bash
intents[intents.indexOf("General_Greetings")].confidence
```
{: codeblock}

### JSONArray.join(String delimiter)

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
  "output":{
  "generic":[
      {
      "values": [
        {
    "text": "Dies ist das Array: <? $toppings_array.join(';') ?>"
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

Ergebnis:

```json
Dies ist das Array: Salami;Paprika;Schinken;
```
{: codeblock}

Wenn Sie eine Variable definieren, die mehrere Werte in einem JSON-Array speichert, können Sie eine Teilmenge der Werte aus dem Array zurückgeben und anschließend mit der Methode 'join()' korrekt formatieren.

#### Erfassungsprognose
{: #dialog-methods-collection-projection}

Ein SpEL-Ausdruck `collection projection` extrahiert einen Teil einer Erfassung aus einem Array, das Objekte enthält. Die Syntax für eine Erfassungsprognose lautet `array_mit_enthaltenen_wertgruppen.![relevanter_wert]`.

Die folgende Kontexvariable definiert zum Beispiel ein JSON-Array zum Speichern von Fluginformationen. Für jeden Flug sind zwei Datenpunkte vorhanden: die Uhrzeit und die Flugnummer.

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

Wenn nur die Flugnummern zurückgegeben werden sollen, können Sie einen Ausdruck für eine Erfassungsprognose mit der folgenden Syntax erstellen:

```
<? $flights_found.![flight_code] ?>
```

Dieser Ausdruck gibt ein Array der Werte für `flight_code` im Format `["OK123","LH421","TS4156"]` zurück. Weitere Details finden Sie in der [Dokumentation der SpEL-Erfassungsprognose](https://docs.spring.io/spring/docs/3.0.x/reference/expressions.html).

Wenn Sie die Methode `join()` auf die Werte in dem zurückgegebenen Array anwenden, werden die Flugnummern als eine durch Kommas getrennte Liste angezeigt. In einer Antwort können Sie beispielsweise die folgende Syntax verwenden: 

```
Die folgenden Flüge stimmen mit Ihren Kriterien überein:
  <? T(String).join(",", $flights_found.![flight_code]) ?>.
```
{: codeblock}

Ergebnis: `Die folgenden Flüge stimmen mit Ihren Kriterien überein: OK123,LH421,TS4156.`

### JSONArray.joinToArray(template)
{: #dialog-methods-joinToArray}

Diese Methode wendet das von Ihnen definierte Format als Vorlage auf ein Array an, das nach Ihren Angaben formatiert wird. Diese Methode ist beispielsweise hilfreich, um eine Formatierung auf Array-Werte anzuwenden, die Sie in einer Dialogmodulantwort zurückgeben möchten.

Die Vorlage kann als Zeichenfolge, JSON-Objekt oder JSON-Array angegeben werden. Verwenden Sie die folgenden Syntaxkonventionen, um Werte aus dem von Ihnen bearbeiteten Array in der Vorlage zu referenzieren: 

- `%`: Markiert den Anfang oder das Ende eines Elements oder einer Elementeigenschaft, das bzw. die aus dem bearbeiteten Array zurückgegeben werden soll.
- `e`: Markiert vorübergehend das Array-Element, auf das die Formatierung angewendet werden soll. Dieser temporäre Variablenname mit der Markierung `e` kann nicht geändert werden.

Angenommen, Sie verfügen über eine Kontextvariable mit einem Array, das eine Liste mit Flugdetails für drei Flüge umfasst. 

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

Nur die Liste der Flugnummern soll zurückgegeben werden. Sie können den folgenden Ausdruck verwenden, um nur den Wert des Elements `flight` aus jedem Array zu extrahieren und in einer Liste zurückzugeben: 

```
Die verfügbaren Flüge sind <? $flights.joinToArray("%e.flight%"). ?>
```
{: codeblock}

Die Antwort des Dialogmodulknotens lautet wie folgt: `Die verfügbaren Flüge sind ["DL1040","DL1710","DL4379"].`

Um das Array als Text anzuzeigen, verwenden Sie die Methode `join` wie im folgenden Ausdruck:

```
Die verfügbaren Flüge sind <? $flights.joinToArray("%e.flight%").join(", "). ?>
```
{: codeblock}

Die Antwort lautet wie folgt: `Die verfügbaren Flüge sind DL1040, DL1710, DL4379.`

#### Komplexe Vorlage
{: #dialog-methods-complex-template}

Um eine komplexere Vorlage zu erstellen, können Sie, anstatt die Vorlagendetails direkt im Methodenparameter anzugeben, eine Kontextvariable erstellen. 

Diese Vorlagenkontextvariable enthält eine Teilmenge der Array-Elemente mit vorangestellten Beschriftungen, damit die Informationen in der Antwort als übersichtliche Liste angezeigt werden. 

```json
"template": "<br/>Flugnummer: %e.flight% <br/> Fluggesellschaft: %e.carrier% <br/> Abflugdatum: %e.departure_date% <br/> Abflugzeit: %e.departure_time% <br/> Ankunftszeit: %e.arrival_time% <br/>"
```
{: codeblock}

Der HTML-Tag `<br/>` für einen Zeilenumbruch wird in manchen Integrationskanälen *nicht* dargestellt (z. B. in Facebook und Slack).
{: note}

Verwenden Sie diesen Ausdruck in der Antwort des Dialogmodulknotens, um die in `$template` definierte Vorlage auf das Array in `$flights` anzuwenden.

```
Die Flugdaten sind <? $flights.joinToArray($template).join(" ") ?>
```
{: codeblock}

Die Antwort sieht wie folgt aus: 

```
Ihre Flugdaten:
Flugnummer: DL1040
Fluggesellschaft: Alitalia
Abflugdatum: 2019-02-02
Abflugzeit: 16:45
Ankunftszeit: 07:00

Flugnummer: DL1710
Fluggesellschaft: Delta
Abflugdatum: 2019-02-02
Abflugzeit: 07:00
Ankunftszeit: 10:19

Flugnummer: DL4379
Fluggesellschaft: Virgin Atlantic
Abflugdatum: 2019-02-02
Abflugzeit: 21:40
Ankunftszeit: 09:05
```
{: screen}

Der Vorteil dieser Methode liegt darin, dass es unerheblich ist, wie häufig sich die Werte in dem Array ändern oder ob die Anzahl der Elemente im Array zunimmt. Der Ausdruck bleibt funktionsfähig, solange jedes Array-Element mindestens die Teilmenge der Eigenschaften enthält, die in der Vorlage referenziert wird. 

#### Beispiel für JSON-Objektvorlage
{: #dialog-methods-object-template}

In diesem Beispiel ist die Kontextvariable der Vorlage als JSON-Objekt definiert, das die Flugnummer sowie Datum und Uhrzeit für Ankunft und Abflugsdatum, Abflugdatum und Abflug aus den einzelnen Elementen extrahiert, die im Array in der Kontextvariablen `$flights` angegeben sind. Sie können diesen Ansatz verwenden, um eine Standardformatierung auf die Flugdaten für Flüge anzuwenden, die beispielsweise von zwei verschiedenen Fluggesellschaften verwaltet werden, die in ihren Web-Services unterschiedliche Formatierungen für Flugdaten verwenden.

```json
"template": {
      "departure": "Flug %e.flight% startet am %e.departure_date% um %e.departure_time%.",
      "arrival": "Flug %e.flight% landet am %e.arrival_date% um %e.arrival_time%."
    }
```
{: codeblock}

Es kann hilfreich sein, die angepasste Clientanwendung so einzurichten, dass die Objekte aus dem zurückgegebenen Array gelesen werden, und die Werte in ein geeignetes Antwortformat für Ihren Chat-Bot umzuwandeln. In der Antwort Ihres Dialogmodulknotens kann der folgende Ausdruck verwendet werden, um das Objekt mit den Ankunftsdaten des Flugs als Array zurückzugeben: 

```
<? $flights.joinToArray($template) ?>
```
{: screen}

Die Antwort des Dialogmodulknotens sieht so aus: 

```json
[
  {
    "arrival":"Flug DL1040 landet am 2019-02-03 um 07:00.",
    "departure":"Flug DL1040 startet am 2019-02-02 um 16:45."
    },
  {
    "arrival":"Flug DL1710 landet am 2019-02-02 um 10:19.",
    "departure":"Flug DL1710 startet am 2019-02-02 um 07:00."
    },
  {
    "arrival":"Flug DL4379 landet am 2019-02-03 um 09:05.",
    "departure":"Flug DL4379 startet am 2019-02-02 um 21:40."
    }
  ]
  ```

Beachten Sie dass die Reihenfolge der Elemente `arrival` und `departure` in der Antwort vertauscht ist. Die Elemente in einem JSON-Objekt werden vom Service in der Regel neu geordnet. Wenn die Elemente in einer bestimmten Reihenfolge zurückgegeben werden sollen, definieren Sie die Vorlage stattdessen als JSON-Array oder als Zeichenfolgewert.

### JSONArray.remove(Integer)

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

### JSONArray.set(Integer index, Object value)

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
{: #dialog-methods-com.google.gson.JsonArray}

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
{: #dialog-methods-date-time}

Für die Arbeit mit Datum und Uhrzeit stehen mehrere Methoden zur Verfügung.

Informationen zum Erkennen und Extrahieren von Datums- und Uhrzeitinformationen aus der Benutzereingabe finden Sie unter [Entitäten '@sys-date' und '@sys-time'](/docs/services/assistant?topic=assistant-system-entities#system-entities-sys-date-time).

### .after(String date or time)

Ermittelt, ob der Wert für Datum/Uhrzeit nach dem angegebenen Argument für Datum/Uhrzeit liegt.

### .before(String date or time)
Ermittelt, ob der Wert für Datum/Uhrzeit vor dem angegebenen Argument für Datum/Uhrzeit liegt.

Beispiel:
- @sys-time.before('12:00:00')
- @sys-date.before('2016-11-21')

- Falls unterschiedliche Einträge verglichen werden, z. B. `Uhrzeit mit Datum`, `Datum mit Uhrzeit` und `Uhrzeit mit Datum/Uhrzeit`, gibt die Methode 'false' zurück und im JSON-Protokoll für die Ausgabe `output.log_messages` wird eine Ausnahmebedingung aufgezeichnet.

  Beispiel: `@sys-date.before(@sys-time)`.
- Bei einem Vergleich von `Datum/Uhrzeit mit Uhrzeit` ignoriert die Methode das Datum und vergleicht lediglich die Uhrzeiten.

### now()

Sie gibt eine Zeichenfolge mit den aktuellen Werten für Datum und Uhrzeit im Format `jjjj-MM-tt HH:mm:ss` zurück.

- Dies ist eine statische Funktion.
- Die anderen Methoden für Datum/Uhrzeit können für Datums-/Uhrzeitwerte aufgerufen werden, die von dieser Funktion zurückgegeben werden, und als deren Argument übergeben werden.
- Falls die Kontextvariable `$timezone` festgelegt ist, gibt diese Funktion Datumsangaben und Uhrzeiten in der Zeitzone des Clients zurück.

Beispiel für einen Dialogmodulknoten, bei dem `now()` im Feld 'output' verwendet wird:

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

Beispiel für `now()` in Bedingungen eines Knotens (stellt fest, ob es noch Morgen ist):

```json
{
  "conditions": "now().before('12:00:00')",
  "output": {
      "generic":[
      {
        "values": [
          {
          "text": "Guten Morgen!"
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

### today()

Gibt eine Zeichenfolge mit dem aktuellen Datum im Format `jjjj-MM-tt` zurück.

- Dies ist eine statische Funktion.
- Die anderen Methoden für Datumsangaben können für Datumswerte aufgerufen werden, die von dieser Funktion zurückgegeben werden, und als deren Argument übergeben werden.
- Falls die Kontextvariable `$timezone` festgelegt ist, gibt diese Funktion Datumsangaben in der Zeitzone des Clients zurück. Andernfalls wird die Zeitzone `GMT` verwendet.

Beispiel für einen Dialogmodulknoten, bei dem `today()` im Feld 'output' verwendet wird:

```json
{
  "conditions": "#what_day_is_it",
  "output": {
    "generic":[
      {
        "values": [
          {
          "text": "Das heutige Datum ist <? today() ?>."
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

Ergebnis: `Das heutige Datum ist 2018-03-09.`

## Datum und Uhrzeit berechnen
{: #dialog-methods-calculations}

Verwenden Sie die folgenden Methoden, um ein Datum zu berechnen.

| Methode                  | Beschreibung |
|-------------------------|-------------|
| `<date>.minusDays(n)`   | Gibt das Datum des Tages zurück, der n Tage vor dem angegebenen Datum liegt. |
| `<date>.minusMonths(n)` |Gibt das Datum des Tages zurück, der n Monate vor dem angegebenen Datum liegt. |
| `<date>.minusYears(n)`  | Gibt das Datum des Tages zurück, der n Jahre vor dem angegebenen Datum liegt. |
| `<date>.plusDays(n)`   | Gibt das Datum des Tages zurück, der n Tage nach dem angegebenen Datum liegt. |
| `<date>.plusMonths(n)` | Gibt das Datum des Tages zurück, der n Monate nach dem angegebenen Datum liegt. |
| `<date>.plusYears(n)`  | Gibt das Datum des Tages zurück, der n Jahre nach dem angegebenen Datum liegt.|

Dabei wird `<date>` im Format `jjjj-MM-tt` oder `jjjj-MM-tt HH:mm:ss` angegeben.

Geben Sie den folgenden Ausdruck an, um das morgige Datum abzurufen: 

```json
{
  "output":{
    "generic":[
      {
        "values": [
          {
          "text": "Das morgige Datum ist <? today().plusDays(1) ?>."
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

Ergebnis, wenn heute der 9. März 2018 ist: `Das morgige Datum ist 2018-03-10.`

Geben Sie den folgenden Ausdruck an, um das Datum nach einer Woche ab dem heutigen Tag abzurufen: 

```json
{
  "output":{
    "generic":[
      {
        "values": [
          {
          "text": "Das Datum in einer Woche ist <? @sys-date.plusDays(7) ?>."
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

Ergebnis, wenn die Entität '@sys-date' den 9. März 2018 als heutiges Datum enthält: `Das Datum in einer Woche ist 2018-03-16.`

Geben Sie den folgenden Ausdruck an, um das Datum vor einem Monat abzurufen: 

```json
{
  "output":{
    "generic":[
      {
        "values": [
          {
          "text": "Das Datum vor einem Monat war <? today().minusMonths(1) ?>."
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

Ergebnis, wenn heute der 9. März 2018 ist: `Das Datum vor einem Monat war 2018-02-9.`

Zum Berechnen der Uhrzeit können Sie die folgenden Methoden verwenden. 

| Methode                  | Beschreibung |
|-------------------------|-------------|
| `<time>.minusHours(n)`   | Gibt die Uhrzeit n Stunden vor der angegebenen Zeit zurück. |
| `<time>.minusMinutes(n)` | Gibt die Uhrzeit n Minuten vor der angegebenen Zeit zurück. |
| `<time>.minusSeconds(n)`  | Gibt die Uhrzeit n Sekunden vor der angegebenen Zeit zurück. |
| `<time>.plusHours(n)`   | Gibt die Uhrzeit n Stunden nach der angegebenen Zeit zurück. |
| `<time>.plusMinutes(n)` | Gibt die Uhrzeit n Minuten nach der angegebenen Zeit zurück. |
| `<time>.plusSeconds(n)`  | Gibt die Uhrzeit n Sekunden nach der angegebenen Zeit zurück. |

Dabei wird `<time>` im Format `HH:mm:ss` angegeben.

Geben Sie den folgenden Ausdruck an, um die Uhrzeit in einer Stunde ab jetzt abzurufen: 

```json
{
  "output":{
    "generic":[
      {
        "values": [
          {
          "text": "In einer Stunde ist es <? now().plusHours(1) ?>."
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

Ergebnis, wenn die aktuelle Uhrzeit 8:00 Uhr ist: `In einer Stunde ist es 09:00:00.`

Geben Sie den folgenden Ausdruck an, um die Uhrzeit vor 30 Minuten abzurufen:

```json
{
  "output":{
    "generic":[
      {
        "values": [
          {
          "text": "Eine halbe Stunde vor @sys-time ist <? @sys-time.minusMinutes(30) ?>."
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

Ergebnis, die in der Entität '@sys-time' erfasste Uhrzeit 8:00 Uhr ist: `Eine halbe Stunde vor 08:00:00 ist 07:30:00.`

Sie können den folgenden Ausdruck verwenden, um die zurückgegebene Uhrzeit neu zu formatieren:

```json
{
  "output":{
    "generic":[
      {
        "values": [
          {
          "text": "Vor 6 Stunden war es <? now().minusHours(6).reformatDateTime('h:mm a') ?>."
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

Ergebnis, wenn es jetzt 14:19  ist: `Vor 6 Stunden war es 8:19.`

### Mit Zeitspannen arbeiten
{: #dialog-methods-time-spans}

Zum Anzeigen einer Antwort, wenn das heutige Datum innerhalb eines bestimmten Zeitrahmens liegt, können Sie eine Kombination aus zeitbasierten Methoden verwenden. Wenn Sie beispielsweise jedes Jahr in der Weihnachtszeit ein Sonderangebot anbieten, können Sie prüfen, ob das heutige Datum im Zeitraum zwischen dem 25. November und dem 24. Dezember des Jahres liegt. Definieren Sie zunächst die betreffenden Datumsangaben als Kontextvariablen. 

In den folgenden Ausdrücken mit Kontextvariablen für Anfangs- und Enddatum wird das Datum durch Verketten des dynamisch abgeleiteten Werts für das aktuelle Jahr mit den fest codierten Werten für Monat und Tag erstellt.

```json
"context": {
   "end_date": "<? now().reformatDateTime('Y') + '-12-24' ?>",
   "start_date": "<? now().reformatDateTime('Y') + '-11-25' ?>"
 }
```

In der Antwortbedingung können Sie angeben, dass die Antwort nur angezeigt werden soll, wenn das aktuelle Datum im Zeitraum zwischen dem Start- und dem Enddatum liegt, den Sie durch Kontextvariablen definiert haben. 

`now().after($start_date) && now().before($end_date)`

### Unterstützung für java.util.Date
{: #dialog-methods-java.util.Date}

Zusätzlich zu den integrierten Methoden können Sie Standardmethoden der Klasse `java.util.Date` verwenden.

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
{: #dialog-methods-numbers}

Diese Methoden unterstützen das Abrufen und Umformatieren von Zahlenwerten.

Informationen zu Systementitäten, die Zahlen aus der Benutzereingabe erkennen und extrahieren können, finden Sie unter [Entität '@sys-number'](/docs/services/assistant?topic=assistant-system-entities#system-entities-sys-number).

Wenn der Service bestimmte Zahlenformate in der Benutzereingabe erkennen soll (z. B. Verweise auf Bestellnummern), sollten Sie in Betracht ziehen, eine Musterentität zum Erfassen solcher Zahlenformate zu erstellen. Weitere Details finden Sie unter [Entitäten erstellen](/docs/services/assistant?topic=assistant-entities).

Wenn Sie die Position der Dezimalzeichen für eine Zahl ändern möchten (z. B. um die Zahl als Währungswert zu formatieren), lesen Sie die Informationen zur Methode [String format()](#dialog-methods-java.lang.String).

### toDouble()

  Konvertiert das Objekt oder Feld in den Zahlentyp 'Double'. Sie können diese Methode für jedes beliebige Objekt oder Feld aufrufen. Falls die Konvertierung fehlschlägt, wird *null* zurückgegeben.

### toInt()

  Konvertiert das Objekt oder Feld in den Zahlentyp 'Integer'. Sie können diese Methode für jedes beliebige Objekt oder Feld aufrufen. Falls die Konvertierung fehlschlägt, wird *null* zurückgegeben.

### toLong()

  Konvertiert das Objekt oder Feld in den Zahlentyp 'Long'. Sie können diese Methode für jedes beliebige Objekt oder Feld aufrufen. Falls die Konvertierung fehlschlägt, wird *null* zurückgegeben.

  Wenn Sie einen Zahlentyp 'Long' in einem SpEL-Ausdruck angeben, müssen Sie den Buchstaben `L` an die Zahl anfügen, um diesen Zahlentyp zu identifizieren. Beispiel: `5000000000L`. Diese Syntax ist für alle Zahlen erforderlich, die nicht in das Format einer 32-Bit-Ganzzahl passen. Beispiel: Zahlen, die größer als 2^31 (2,147,483,648) oder kleiner als -2^31 (-2,147,483,648) sind, werden als Zahlentyp 'Long' eingestuft. Zahlen des Typs 'Long' liegen zwischen dem Minimalwert -2^63 und dem Maximalwert 2^63-1.

### Zahlenunterstützung in Java
{: #dialog-methods-java.lang.Number}

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
    "generic":[
      {
        "values": [
          {
          "text": "Die größere Zahl ist $bigger_number."
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
          "text": "Die kleinere Zahl ist $smaller_number."
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
          "text": "Die zweite Potenz Ihrer Basiszahl $base ist $power_of_two."
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
    "generic":[
      {
        "values": [
          {
          "text": "Dies ist eine Zufallszahl zwischen 0 und @sys-number.literal: $answer."
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

Informationen zu weiteren Methoden finden Sie unter [java.util.Random - Referenzinformationen](https://docs.oracle.com/javase/7/docs/api/java/util/Random.html).

Sie können auch Standardmethoden der folgenden Klassen verwenden:

- `java.lang.Byte`
- `java.lang.Integer`
- `java.lang.Long`
- `java.lang.Double`
- `java.lang.Short`
- `java.lang.Float`

## Objekte
{: #dialog-methods-objects}

### JSONObject.clear()

Diese Methode löscht alle Werte in dem JSON-Objekt und gibt null zurück.

Angenommen, Sie möchten die aktuellen Werte in der Kontextvariablen '$user' löschen.

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

Verwenden Sie den folgenden Ausdruck in der Ausgabe, um ein Feld zu definieren, das die Werte in dem Objekt löscht. 

```json
{
  "output":{
    "object_eraser": "<? $user.clear() ?>"
  }
}
```
{: codeblock}

Nachfolgende Verweise auf die Kontextvariable '$user' geben nur `{}` zurück.

Sie können die Methode `clear()` auf die JSON-Objekte `context` oder `output` im Hauptteil des API-Aufrufs `/message` anwenden.

#### Inhalt von 'context' löschen
{: #dialog-methods-clearing-context}

Wenn Sie mit der Methode `clear()` den Inhalt des Objekts `context` lösche, werden **alle** Variablen mit Ausnahme der folgenden gelöscht: 

 - `context.conversation_id`
 - `context.timezone`
 - `context.system`

**Warnung**: Zu den Kontextvariablen, die gelöscht werden, gehören auch die folgenden: 

  - Alle festgelegten Standardwerte für Variablen in Knoten, die während der aktuellen Sitzung ausgelöst wurden. 
  - Alle Änderungen der Standardvariablen, die mithilfe von Informationen vorgenommen wurden, die während der aktuellen Sitzung vom Benutzer oder von externen Services bereitgestellt wurden. 

Um die Methode zu verwenden, können Sie sie in einem Ausdruck in einer Variablen angeben, die Sie im Objekt 'output' definieren. Beispiel:

```json
{
  "output":{
    "generic":[
      {
        "values": [
          {
          "text": "Die Antwort für diesen Knoten."
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

#### Inhalt des Objekts 'output' löschen
{: #dialog-methods-clearing-output}

Wenn Sie mit der Methode `clear()` den Inhalt des Objekts `output` löschen, werden alle Variablen mit Ausnahme derjenigen gelöscht, die Sie zum Löschen des Objekts 'output' verwenden, sowie alle Textantworten, die Sie im aktuellen Knoten definieren. Außerdem werden die folgenden Variablen nicht gelöscht: 

- `output.nodes_visited`
- `output.nodes_visited_details`

Um die Methode zu verwenden, können Sie sie in einem Ausdruck in einer Variablen angeben, die Sie im Objekt 'output' definieren. Beispiel:

```json
{
  "output":{
    "generic":[
      {
        "values": [
          {
          "text": "Einen schönen Tag noch!"
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

Wenn für einen früheren Knoten in der Baumstruktur die Textantwort `Ich helfe Ihnen gern.` definiert ist und anschließend zu einem Knoten mit dem JSON-Objekt 'output' gesprungen wird, wird nur `Einen schönen Tag noch.` als Antwort angezeigt. Die Antwort `Ich helfe Ihnen gern.` wird nicht angezeigt, da sie gelöscht und durch die Textantwort des Knotens ersetzt wird, der die Methode `clear()` aufruft.

### JSONObject.has(String)

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

### JSONObject.remove(String)

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
{: #dialog-methods-com.google.gson.JsonObject}

Zusätzlich zu den integrierten Methoden können Sie Standardmethoden der Klasse `com.google.gson.JsonObject` verwenden.

## Zeichenfolgen
{: #dialog-methods-strings}

Diese Methoden unterstützen das Arbeiten mit Text.

Informationen zum Erkennen und Extrahieren bestimmter Zeichenfolgetypen wie Personennamen und Positionen aus der Benutzereingabe finden Sie unter [Systementitäten](/docs/services/assistant?topic=assistant-system-entities).

**Hinweis:** Für Methoden, die reguläre Ausdrücke einbeziehen, finden Sie auf der Seite [RE2 Syntax reference ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/google/re2/wiki/Syntax){: new_window} Details über die Syntax zum Angeben des regulären Ausdrucks.

### String.append(Object)

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

### String.contains(String)

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

### String.endsWith(String)

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

### String.find(String regexp)

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

### String.matches(String regexp)

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

### String.startsWith(String)

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

### String.substring(Integer beginIndex, Integer endIndex)

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

Sie können die Standardmethode für Java-Zeichenfolgen `format()` auf Text anwenden. Informationen über die Syntax zum Angeben der Formatdetails finden Sie unter [java.util.formatter reference ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://docs.oracle.com/javase/7/docs/api/java/util/Formatter#syntax){: new_window}.

Beispiel: Der folgende Ausdruck akzeptiert drei dezimale Ganzzahlen (1, 1 und 2) und fügt Sie zu einem Satz hinzu.

```json
{
  "formatted String": "<? T(java.lang.String).format('%d + %d gleich %d', 1, 1, 2) ?>"
}
```
{: codeblock}

Ergebnis: `1 + 1 gleich 2`.

Verwenden Sie die folgende Syntax um die Dezimalstellen für eine Zahl zu ändern:

```
{
  <? T(String).format('%.2f',<number to format>) ?>
}
```
{: codeblock}

Beispiel: Wenn die Variable '$number', die in US-Dollar formatiert werden muss, `4,5` ist, dann wird für eine Antwort wie `Ihr Gesamtbetrag ist $<? T(String).format('%.2f',$number) ?>` Folgendes zurückgegeben: `Ihr Gesamtbetrag ist $ 4,50.`

## Indirekte Datentypumwandlung
{: #dialog-methods-indirect-type-conversion}

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
