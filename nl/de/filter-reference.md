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

# Referenzinformationen zu Filterabfragen
{: #filter-reference}

Die REST-API des Service '{{site.data.keyword.conversationshort}}' bietet leistungsfähige Protokollsuchfunktionen durch Filterabfragen. Mit dem Parameter `filter` für die API '/logs' können Sie Ihr Skillprotokoll nach Ereignissen durchsuchen, die mit einer angegebenen Abfrage übereinstimmen.

Der Parameter `filter` ist eine zwischenspeicherbare Abfrage, die die Ergebnisse auf die mit dem angegebenen Filter übereinstimmenden Ergebnisse beschränkt. Sie können den Filter für jedes beliebige Objekt definieren, das zum JSON-Antwortmodell gehört (z. B. den Text der Benutzereingabe, die erkannten Absichten und Entitäten oder die Konfidenzbewertung).

Beispiele für die verschiedenen Arten von Filterabfragen enthält der Abschnitt [Beispiele](#filter-reference-examples).

Weitere Informationen zur Methode `GET` für '/logs' und das zugehörige Antwortmodell finden Sie in der [API-Referenz ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://cloud.ibm.com/apidocs/assistant?curl=#list-log-events-in-a-workspace){: new_window}.

## Syntax für Filterabfragen
{: #filter-reference-syntax}

Das folgende Beispiel zeigt die allgemeine Form einer Filterabfrage:

| Position           | Abfrageoperator | Term         |
|:------------------:|:--------------:|:------------:|
|`request.input.text`|`::`            |`"IBM Watson"`|

- Die _Position_ gibt das Feld an, für das der Filter definiert werden soll (im Beispiel `request.input.text`).
- Der _Abfrageoperator_ gibt den Typ des Abgleichs an, den Sie verwenden wollen (unscharfe Suche oder exakter Abgleich).
- Der _Term_ gibt den Ausdruck oder Wert an, den Sie beim Abgleich zur Auswertung des Feldes verwenden wollen. Der Term kann Literaltext und Operatoren enthalten (dies ist im [nächsten Abschnitt](#filter-reference-operators) beschrieben).

Eine Filterung nach Absicht oder Entität erfordert eine etwas andere Syntax als die Filterung nach anderen Feldern. Weitere Informationen finden Sie unter [Nach Absicht oder Entität filtern](#filter-reference-intent-entity).

**Hinweis:** Die Filterabfragesyntax verwendet einige Zeichen, die in HTTP-Abfragen nicht zulässig sind. Achten Sie darauf, dass alle Sonderzeichen (inklusive Leerzeichen und Anführungszeichen) in URL-Codierung angegeben werden, wenn sie als Teil einer HTTP-Abfrage gesendet werden. Beispielsweise müsste der Filter `response_timestamp<2016-11-01` als `response_timestamp%3C2016-11-01` angegeben werden.

## Operatoren
{: #filter-reference-operators}

In Ihrer Filterabfrage können Sie die folgenden Operatoren verwenden.

| Operator | Beschreibung |
|:-------------------:|-----------|
| `:` | Abfrageoperator für unscharfe Suche. Verwenden Sie für den Abfrageterm das Präfix `:`, falls Sie eine Übereinstimmung für alle Werte erzielen wollen, die den Abfrageterm oder eine seiner grammatikalischen Varianten enthalten. Die unscharfe Suche ist für Benutzereingabetext, Antwortausgabetext und Entitätswerte verfügbar. |
| `::` | Abfrageoperator für exakte Übereinstimmung. Verwenden Sie für den Abfrageterm das Präfix `::`, falls Sie eine Übereinstimmung nur für die Werte erzielen wollen, die exakt mit dem Abfrageterm übereinstimmen. |
| `:!` | Abfrageoperator für negative unscharfe Suche. Verwenden Sie für den Abfrageterm das Präfix `:!`, falls Sie eine Übereinstimmung nur für die Werte erzielen wollen, die den Abfrageterm oder eine seiner grammatikalischen Varianten _nicht_ enthalten. |
| `::!` | Abfrageoperator für negative exakte Übereinstimmung. Verwenden Sie für den Abfrageterm das Präfix `::!`, falls Sie eine Übereinstimmung nur für die Werte erzielen wollen, die _nicht_ exakt mit dem Abfrageterm übereinstimmen. |
| `<=`, `>=`, `>`, `<` | Vergleichsoperatoren. Verwenden Sie diese Operatoren als Präfix für den Abfrageterm, um den Abgleich auf der Basis eines arithmetischen Vergleichs durchzuführen. |
| `\` | Operator für Escapezeichen. Verwenden Sie diesen Operator in Abfragen, die Steuerzeichen enthalten, welche andernfalls als Operatoren ausgewertet würden. Die Angabe `\!hallo` würde beispielsweise eine Übereinstimmung mit dem Text `!hallo` ergeben. |
| `""` | Literaler Ausdruck. Verwenden Sie diesen Operator zum Einschließen eines Abfrageterms, der Leerzeichen oder andere Sonderzeichen enthält. Von der API werden keine Sonderzeichen ausgewertet, die innerhalb der Anführungszeichen angegeben sind. |
| `~` | Suche nach ähnlichen Übereinstimmungen. Hängen Sie diese Operator gefolgt von der Ziffer `1` oder `2` am Ende des Abfrageterms an, um die zulässige Anzahl von abweichenden Einzelzeichen zwischen der Abfragezeichenfolge und einer Übereinstimmung im Antwortobjekt anzugeben. Die Angabe `Auto~1` würde beispielsweise eine Übereinstimmung  mit `Auto`, `Autor` oder `Autos` ergeben, jedoch nicht mit `Autoren`. Dieser Operator ist nicht gültig, wenn die Filterung für das Feld `log_id` oder ein anderes Feld mit einem Wert für Datum oder Uhrzeit bzw. mit unscharfer Suche ausgeführt wird. |
| `*` | Operator für Platzhalterzeichen. Ergibt eine Übereinstimmung mit jeder beliebigen Folge aus 0 oder mehr Zeichen. Dieser Operator ist nicht gültig, wenn die Filterung für das Feld `log_id` oder ein anderes Feld mit einem Wert für Datum oder Uhrzeit ausgeführt wird. |
| `()`, `[]` | Gruppierungsoperatoren. Verwenden Sie diese Operatoren, um eine logische Gruppierung von mehreren Ausdrücken einzuschließen, die boolesche Operatoren enthalten. |
| `|` | Boolescher Operator _or_. |
| `,` | Boolescher Operator _and_. |

### Nach Absicht oder Entität filtern
{: #filter-reference-intent-entity}

Aufgrund der Unterschiede bei der internen Speicherung von Absichten und Entitäten weicht die Syntax für die Filterung nach einer bestimmten Absicht oder Entität etwas von der Syntax ab, die für andere Felder in den zurückgegebenen JSON-Daten verwendet wird. Zur Angabe eines Feldes `intent` oder `entity` in einer Objektgruppe `intents` oder `entities` müssen Sie den Abgleichsoperator `:` anstelle eines Punktes verwenden.

Die folgende Abfrage erzielt beispielsweise eine Übereinstimmung für jedes protokollierte Ereignis, bei dem die Antwort eine erkannte Absicht namens `hello` enthält:

`response.intents:intent::hello`

Beachten Sie, dass hier der Operator `:` anstelle eines Punktes verwendet wird (intents:intent).

Verwenden Sie dasselbe Muster, um den Abgleich anhand eines beliebigen Feldes einer erkannten Absicht oder Entität in der Antwort vorzunehmen. Die folgende Abfrage beispielsweise erzielt eine Übereinstimmung für jedes protokollierte Ereignis, bei dem die Antwort eine erkannte Entität mit dem Wert `soda` enthielt:

`response.entities:value::soda`

Eine ähnliche Filterung können Sie wie im folgenden Beispiel für Absichten oder Entitäten vornehmen, die als Teil der Antforderung gesendet wurden:

`request.intents:intent::hello`

Die Filterung nach Absichten wird auf alle erkannten Absichten angewendet. Um die Filterung auf die erkannte Absicht mit der höchsten Konfidenz zu beschränken, können Sie die Kurzformsyntax `response.top_intent` verwenden. Beispiel:

`response.top_intent::goodbye`

### Nach Kunden-ID filtern
{: #filter-reference-customer-id}

Verwenden Sie zum Filtern nach der Kunden-ID die Sonderbedingung `customer_id`. (Weitere Informationen zum Kennzeichnen von Nachrichten durch eine Kunden-ID enthält der Abschnitt [Informationssicherheit](/docs/services/assistant?topic=assistant-information-security)).

### Nach anderen Feldern filtern
{: #filter-reference-fields}

Um die Filterung anhand eines anderen Feldes in den Protokolldaten vorzunehmen, geben Sie die Position als Pfad an, der die Ebene der verschachtelten Objekte in der JSON-Antwort von der API '/logs' angibt. Verwenden Sie jeweils einen Punkt (`.`) um aufeinanderfolgende Verschachtelungsebenen in den JSON-Daten kenntlich zu machen. Die Position `request.input.text` gibt beispielsweise das Benutzereingabefeld wie im folgenden JSON-Fragment gezeigt an:

```json
  "request": {
    "input": {
      "text": "Guten Morgen"
    }
  }
```

## Beispiele
{: #filter-reference-examples}

Die folgenden Beispiele veranschaulichen die unterschiedlichen Typen von Abfragen, die diese Syntax verwenden.

| Beschreibung | Abfrage |
|---------|-----------|
| Das Datum der Antwort liegt im Monat Juli 2017. | `response_timestamp>=2017-07-01,response_timestamp<2017-08-01` |
| Die Zeitmarke der Antwort liegt vor dem Zeitpunkt `2016-11-01T04:00:00.000Z`. | `response_timestamp<2016-11-01T04:00:00.000Z` |
| Die Nachricht ist mit der Kunden-ID `my_id` gekennzeichnet. | `customer_id::my_id` |
| Der Benutzereingabetext enthält das Wort 'Bestellung' oder eine seiner grammatikalischen Varianten (z. B. `Bestellungen` oder `bestellen`). | `request.input.text:bestellung` |
| Der Name einer Absicht in der Antwort stimmt exakt mit `place_order` überein. | `response.intents:intent::place_order` |
| Ein Entitätsname in der Antwort stimmt exakt mit `beverage` überein.  | `response.entities:entity::beverage` |
| Der Benutzereingabetext enthält das Wort 'Bestellung' oder eine seiner grammatikalischen Varianten nicht. | `request.input.text:!bestellung` |
| Der Name der erkannten Absicht mit der höchsten Konfidenz stimmt nicht exakt mit `hello` überein. | `response.top_intent::!hello` |
| Der Benutzereingabetext enthält die Zeichenfolge `!hallo`. | `request.input.text:\!hallo` |
| Der Benutzereingabe enthält die Zeichenfolge `IBM Watson`. | `request.input.text:"IBM Watson"` |
| Der Benutzereingabetext enthält eine Zeichenfolge, die sich in höchstens 2 Zeichen von `Watson` unterscheidet. | `request.input.text:Watson~2` |
| Der Benutzereingabetext enthält eine Zeichenfolge, die aus `comm`, gefolgt von 0 oder mehr weiteren Zeichen, gefolgt von `s` besteht. | `request.input.text:comm*s` |
| Die Bereitstellungs-ID im Kontext stimmt mit `my_app` überein. | `request.context.metadata.deployment::my_app` |
| Eine Absicht in der Antwort hat einen Konfidenzwert, der größer als 0,8 ist. | `response.intents:confidence>0.8` |
| Der Name einer Absicht in der Antwort stimmt exakt mit `hello` oder `goodbye` überein. | `response.intents:intent::(hello|goodbye)` |
| Eine Absicht in der Antwort hat den Namen `hello` und einen Konfidenzwert größer-gleich 0,8. | `response.intents:(intent:hello,confidence>=0.8)` |
| Der Name einer Absicht in der Antwort stimmt exakt mit `order` und ein Entitätsname in der Antwort stimmt exakt mit `beverage` überein. | `[response.intents:intent::order,response.entities:entity::beverage]` |
