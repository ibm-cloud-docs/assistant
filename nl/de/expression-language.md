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

# Ausdrücke für Objektzugriff

Zum Schreiben von Ausdrücken für den Zugriff auf Objekte und Objekteigenschaften können Sie die Ausdruckssprache SpEL (Spring Expression Language) verwenden. Weitere Informationen finden Sie unter [Spring Expression Language (SpEL) ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](http://docs.spring.io/spring/docs/current/spring-framework-reference/html/expressions.html){: new_window}.
{: shortdesc}

## Auswertungssyntax

Um Variablenwerte innerhalb anderer Variablen zu erweitern oder um Methoden für Eigenschaften und globale Objekte aufzurufen, verwenden Sie die Ausdruckssyntax `<? expression ?>`. Beispiel:

- **Eigenschaft erweitern**
    - `"output":{"text":"Your name is <? context.userName ?>"}`

- **Methoden für Eigenschaften globaler Objekte aufrufen**
    - `"context":{"email": "<? @email.literal ?>"}`

## Kurzformsyntax
{: #shorthand-syntax}

Erfahren Sie, wie mit der Kurzformsyntax SpEL schnell auf die folgenden Objekte verwiesen werden kann:

- [Kontextvariablen](expression-language.html#shorthand-context)
- [Entitäten](expression-language.html#shorthand-entities)
- [Absichten](expression-language.html#shorthand-intents)

### Kurzformsyntax für Kontextvariablen
{: #shorthand-context}

In der folgenden Tabelle sind Beispiele für die Kurzformsyntax aufgeführt, die Sie zum Schreiben von Kontextvariablen in Bedingungsausdrücken verwenden können.

| Kurzformsyntax           | Vollständige Syntax in SpEL                     |
|----------------------------|-----------------------------------------|
| `$card_type`               | `context['card_type']`                  |
| `$(card-type)`             | `context['card-type']`                  |
| `$card_type:VISA`          | `context['card_type'] == 'VISA'`        |
| `$card_type:(MASTER CARD)` | `context['card_type'] == 'MASTER CARD'` |

In den Namen von Kontextvariablen können Sie Sonderzeichen wie Bindestriche oder Punkte verwenden. Dies kann allerdings zu Problemen bei der Auswertung des SpEL-Ausdrucks führen. Beispielsweise könnte ein Bindestrich als Minuszeichen interpretiert werden. Referenzieren Sie die Variable zur Vermeidung solcher Probleme entweder mit der vollständigen Ausdruckssyntax oder der Kurzformsyntax `$(variablenname)` und verwenden Sie im Namen keines der folgenden Sonderzeichen:

- Runde Klammern ()
- Mehr als ein Hochkomma ''
- Anführungszeichen "

### Kurzformsyntax für Entitäten
{: #shorthand-entities}

In der folgenden Tabelle sind Beispiele für die Kurzformsyntax aufgeführt, die Sie zum Referenzieren von Entitäten verwenden können:

| Kurzformsyntax    | Vollständige Syntax in SpEL                      |
|---------------------|------------------------------------------|
| `@year`             | `entities['year']?.value`                |
| `@year == 2016`     | `entities['year']?.value == 2016`        |
| `@year != 2016`     | `entities['year']?.value != 2016`        |
| `@city == 'Boston'` | `entities['city']?.value == 'Boston'`    |
| `@city:Boston`      | `entities['city']?.contains('Boston')`   |
| `@city:(New York)`  | `entities['city']?.contains('New York')` |

In SpEL verhindert das Fragezeichen `(?)` das Auslösen einer Nullzeigerausnahme, wenn ein Entitätsobjekt null ist.

Falls der Entitätswert, den Sie überprüfen wollen, ein Zeichen `)` enthält, können Sie nicht den Operator `:` für Vergleiche verwenden.  Falls Sie beispielsweise prüfen wollen, ob die Entität 'city' den Wert `Dublin (Ohio)` besitzt, müssen Sie `@city == 'Dublin (Ohio)'` anstelle von `@city:(Dublin (Ohio))` verwenden.

### Kurzformsyntax für Absichten
{: #shorthand-intents}

In der folgenden Tabelle sind Beispiele für die Kurzformsyntax aufgeführt, die Sie zum Referenzieren von Absichten verwenden können:

| Kurzformsyntax        | Vollständige Syntax in SpEL |
|-------------------------|---------------------|
| `#help`                 | `intent == 'help'`  |
| `! #help`               | `intent != 'help'`  |
| `NOT #help`             | `intent != 'help'`  |
| `#help` oder `#i_am_lost` | <code>(intent == 'help' \|\| intent == 'I_am_lost')</code> |

## Integrierte globale Variablen
{: #builtin-vars}

Mit der Ausdruckssprache können Sie Eigenschaftsinformationen für die folgenden globalen Variablen extrahieren:

| Globale Variable      | Definition |
|----------------------|------------|
| *context*            | Der Teil der verarbeiteten Dialognachricht mit dem JSON-Objekt. |
| *entities[ ]*        | Die Liste der Entitäten, die den Standardzugriff auf das erste Element unterstützen. |
| *input*              | Der Teil der verarbeiteten Dialognachricht mit dem JSON-Objekt. |
| *intents[ ]*         | Die Liste der Absichten, die den Standardzugriff auf das erste Element unterstützen. |
| *output*             | Der Teil der verarbeiteten Dialognachricht mit dem JSON-Objekt. |

## Auf Entitäten zugreifen
{: #access-entity}

Das Entitätsarray enthält eine oder mehrere Entitäten, die in der Benutzereingabe erkannt wurden.

Während Sie Ihr Dialogmodul testen, können Sie Details der Entitäten anzeigen, die in der Benutzereingabe erkannt wurden, indem Sie den folgenden Ausdruck in der Antwort eines Dialogmodulknotens angeben:

```json
<? entities ?>
```
{: codeblock}

Für die Benutzereingabe *Hello now* erkennt der Service die Systementitäten '@sys-date' und '@sys-time', weshalb die Antwort die folgenden Entitätsobjekte enthält:

```json
[
{"entity":"sys-date","location":[6,9],"value":"2017-08-07",
  "confidence":1,"metadata":{"calendar_type":"GREGORIAN",
  "timezone":"America/New_York"}},
{"entity":"sys-time","location":[6,9],"value":"15:01:00",
  "confidence":1,"metadata":{"calendar_type":"GREGORIAN",
  "timezone":"America/New_York"}}
]
```
{: codeblock}

### Relevanz der Position von Entitäten in der Eingabe

Verwenden Sie den vollständigen SpEL-Ausdruck, wenn die Position von Entitäten in der Eingabe von Bedeutung ist. Die Bedingung `entities['city']?.contains('Boston')` gibt 'true' zurück, wenn mindestens eine Entität 'city' mit dem Wert 'Boston' unter allen Entitäten '@city' gefunden wird, wobei die Platzierung irrelevant ist.

Beispiel: Ein Benutzer übergibt `"I want to go from Toronto to Boston."` Die Entitäten `@city:Toronto` und `@city:Boston` werden erkannt und in den folgenden Entitäten dargestellt:

- `entities.city[0].value = 'Toronto'`
- `entities.city[1].value = 'Boston'`

Die Bedingung `@city.contains('Boston')` in einem Dialogmodulknoten gibt 'true' zurück, obwohl 'Boston' die zweite erkannte Entität ist.

### Entitätseigenschaften

Jeder Entität sind eine Reihe von Eigenschaften zugeordnet. Über die Eigenschaften können Sie auf Informationen zu einer Entität zugreifen.

| Eigenschaft              | Definition | Tipps zur Verwendung |
|-----------------------|------------|------------|
| *confidence*          | Ein als Dezimalzahl ausgedrückter Prozentsatz, der die Konfidenz des Service in der erkannten Entität darstellt. Die Konfidenz einer Entität ist entweder 0 oder 1, es sei denn, die unscharfe Suche für Entitäten ist aktiviert. Falls die unscharfe Suche aktiviert ist, liegt der Standardschwellenwert für das Konfidenzniveau bei 0,3. Systementitäten besitzen ungeachtet der Tatsache, ob die unscharfe Suche aktiviert ist oder nicht, immer das Konfidenzniveau 1,0. | Sie können diese Eigenschaft in einer Bedingung verwendet, damit 'false' zurückgegeben wird, wenn das Konfidenzniveau nicht höher als ein von Ihnen angegebener Prozentsatz ist. |
| *location*            | Eine relative Zeichenposition mit der Basis null, die angibt, wo der erkannte Entitätswert im Eingabetext beginnt und endet. | Mit `.literal` können Sie den Bereich des Textes zwischen den Start- und Endindexwerten extrahieren, die in der Eigenschaft 'location' gespeichert sind. |
| *value*               | Der in der Eingabe ermittelte Entitätswert. | Diese Eigenschaft gibt den Entitätswert so zurück, wie er in den Trainingsdaten definiert ist, und zwar auch dann, wenn die Übereinstimmung mit einem der zugehörigen Synonyme vorlag. Mit `.values` können Sie mehrere Vorkommen einer Entität erfassen, die möglicherweise in der Benutzereingabe enthalten sind. |

### Verwendungsbeispiele für Entitätseigenschaften
In den folgenden Beispielen enthält der Arbeitsbereich eine Entität 'airport', die den Wert 'JFK' und das Synonym 'Kennedy Airport' umfasst. Die Benutzereingabe lautet *I want to go to Kennedy Aiport*.

- Um eine bestimmte Antwort zurückzugeben, falls die Entität 'JFK' in der Benutzereingabe erkannt wird, könnten Sie den folgenden Ausdruck zur Antwortbedingung hinzufügen:
  `entities.airport[0].value == 'JFK'`
  oder
  `@airport = "JFK"`
- Um den Entitätsnamen so zurückzugeben, wie er vom Benutzer in der Dialogmodulantwort angegeben wurde, verwenden Sie die Eigenschaft '.literal':
  `So you want to go to <?entities.airport[0].literal?>...`
  oder
  `So you want to go to @airport.literal ...`

  Beide Formate werden in der Antwort mit 'So you want to go to Kennedy Airport...' ausgewertet.

- Ausdrücke wie `@airport:(JFK)` oder `@airport.contains('JFK')` referenzieren immer den **Wert** der Entität (im Beispiel `JFK`).
- Um die Erkennung von Begriffen als Flughäfen in der Eingabe strenger einzugrenzen, wenn die unscharfe Suche aktiviert ist, können Sie diesen Ausdruck in einer Knotenbedingung wie beispielsweise `@airport && @airport.confidence > 0.7` angeben. Der Knoten wird hier nur dann ausgeführt, wenn der Service mit einer Konfidenz von 70% ermittelt, dass die Eingabe einen Verweis auf einen Flughafen enthält.

Im Beispiel lautet die Benutzereingabe *Are there places to exchange currency at JFK, Logan, and O'Hare?*

- Um mehrere Vorkommen eines Entitätstyps in einer Benutzereingabe zu erfassen, verwenden Sie eine Syntax wie die Folgende:

    ```json
    "context":{
      "airports":"@airport.values"
    }
    ```

  Um die erfasste Liste später in einer Dialogmodulantwort zu referenzieren, verwenden Sie die folgende Syntax:
  `You asked about these airports: <? $airports.join(', ') ?>.`
  Sie wird wie folgt angezeigt.
  `You asked about these airports: JFK, Logan, O'Hare.`

## Auf Absichten zugreifen
{: #access-intent}

Das Absichtenarray enthält eine oder mehrere Absichten, die in der Benutzereingabe erkannt wurden und die in absteigender Reihenfolge gemäß ihrer Konfidenz sortiert sind. 

Jede Absicht besitzt nur eine einzige Eigenschaft, nämlich die Eigenschaft `confidence`. Die Eigenschaft 'confidence' gibt einen als Dezimalzahl ausgedrückten Prozentsatz an, der die Konfidenz des Service bezüglich der erkannten Absicht darstellt.

Während Sie Ihr Dialogmodul testen, können Sie Details der Absichten anzeigen, die in der Benutzereingabe erkannt wurden, indem Sie den folgenden Ausdruck in der Antwort eines Dialogmodulknotens angeben:

```json
<? intents ?>
```
{: codeblock}

Für die Benutzereingabe *Hello now* findet der Service eine exakte Übereinstimmung mit der Absicht '#greeting'. Darum werden die Details des Absichtsobjekts '#greeting' zuerst aufgelistet. Die Antwort enthält außerdem die 10 häufigsten anderen Absichten, die im Know-how definiert sind, unabhängig von der zugehörigen Konfidenzbewertung. (Im vorliegenden Beispiel wird der Konfidenzwert für die anderen Absichten auf 0 gesetzt, da die erste Absicht eine exakte Übereinstimmung ist.) Die 10 Absichten mit den höchsten Werten werden zurückgegeben, da in der Anforderung der Anzeige 'Ausprobieren' der Parameter `alternate_intents:true` übergeben wird. Wenn Sie unmittelbar mit der API arbeiten und die 10 höchsten Ergebnisse sehen möchten, geben Sie in Ihrem Aufruf unbedingt diesen Parameter an. Wenn `alternate_intents` den Standardwert 'false' aufweist, werden im Array nur Absichten mit einem Konfidenzwert größer als 0,2 zurückgegeben.

```json
[{"intent":"greeting","confidence":1},
{"intent":"yes","confidence":0},
{"intent":"pizza-order","confidence":0}]
```
{: codeblock}

Die folgenden Beispiele zeigen, wie Sie die Eingabe auf einen Absichtswert überprüfen können.

- `intents[0] == 'Help'`
- `intent == 'Help'`

`intent == 'help'` unterscheidet sich von `intents[0] == 'help'`, weil `intent == 'help'` keine Ausnahmebedingung auslöst, wenn keine Absicht erkannt wird. Eine Auswertung mit 'true' findet nur dann statt, wenn die Konfidenz der Absicht einen Schwellenwert überschreitet.  Wenn Sie wollen, können Sie ein angepasstes Konfidenzniveau für eine Bedingung angeben, z. B. `intents.size() > 0 && intents[0] == 'help' && intents[0].confidence > 0.1`.

## Auf Eingabe zugreifen
{: #access-input}

Das JSON-Eingabeobjekt enthält eine einzige Eigenschaft, nämlich die Eigenschaft 'text'. Die Eigenschaft 'text' stellt den Text der Benutzereingabe dar.

### Verwendungsbeispiele für Eingabeeigenschaften

Das folgende Beispiel zeigt, wie auf die Eingabe zugegriffen wird:

- Um einen Knoten auszuführen, wenn die Benutzereingabe 'Yes' lautet, wird der folgende Ausdruck zur Knotenbedingung hinzugefügt:
  `input.text == 'Yes'`

Zur Auswertung oder Bearbeitung von Text aus der Benutzereingabe können Sie jede der [Methoden für Zeichenfolgen](/docs/services/conversation/dialog-methods.html#strings) verwenden. Beispiel:

- Um zu überprüfen, ob die Benutzereingabe 'Yes' enthält, verwenden Sie `input.text.contains( 'Yes' )`.
- Um 'true' zurückzugeben, wenn die Benutzereingabe eine Zahl ist, verwenden Sie `input.text.matches( '[0-9]+' )`.
- Um zu überprüfen, ob die Eingabezeichenfolge zehn Zeichen enthält, verwenden Sie `input.text.length() == 10`.
