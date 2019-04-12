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

# Details über Systementitäten
{: #system-entities}

In diesem Abschnitt der Referenzinformationen finden Sie umfassende Angaben über die verfügbaren Systementitäten. Weitere Informationen zu Systementitäten und ihre Verwendung enthält der Abschnitt [Entitäten definieren](/docs/services/assistant?topic=assistant-entities#entities-enable-system-entities). Suchen Sie dort nach 'Systementitäten aktivieren'.
{: shortdesc}

Systementitäten sind für Sprachen verfügbar, die im Abschnitt [Unterstützte Sprachen](/docs/services/assistant?topic=assistant-language-support) aufgeführt sind.

## Entität '@sys-currency'
{: #system-entities-sys-currency}

Die Systementität '@sys-currency' erkennt Währungswerte, die in einer verbalen Äußerung mit einem Währungssymbol ausgedrückt sind, oder währungsspezifische Begriffe. Zurückgegeben wird ein numerischer Wert.

### Erkannte Formate
{: #system-entities-sys-currency-formats}

- 20 cents
- Five dollars
- $10

### Metadaten
{: #system-entities-sys-currency-metadata}

- `.numeric_value`: Der kanonische numerische Wert als Zahl des Typs 'Integer' oder 'Double' in Basiseinheiten.
- `.unit`: Der Währungscode für die Basiseinheit (z. B. 'USD' oder 'EUR').

### Rückgaben
{: #system-entities-sys-currency-returns}

Für die Eingabe `twenty dollars` oder `$1,234.56` gibt die Systementität '@sys-currency' die folgenden Werte zurück:

| Attribut                   | Typ   | Rückgabe für `twenty dollars` | Rückgabe für `$1,234.56` |
|-----------------------------|--------|-------------------------------|-------------------------:|
| @sys-currency               | string | 20                            |                  1234.56 |
| @sys-currency.literal       | string | twenty dollars                |                $1,234.56 |
| @sys-currency.numeric_value | number | 20                            |                  1234.56 |
| @sys-currency.location      | array  | [0,14]                        |                    [0,9] |
| @sys-currency.unit          | string | USD*                          |                      USD |

*'@sys-currency.unit' gibt immer den dreistelligen ISO-Währungscode zurück.

Für die Eingabe `veinte euro` oder <code>&euro;1.234,56</code> in Spanisch gibt die Systementität '@sys-currency' die folgenden Werte zurück:

| Attribut                   | Typ   | Rückgabe für  `veinte euro` | Rückgabe für <code>&euro;1.234,56</code> |
|-----------------------------|--------|-----------------------------|-------------------------:|
| @sys-currency               | string | 20                          |                  1234.56 |
| @sys-currency.literal       | string | veinte euro                 |                &euro;1.234,56 |
| @sys-currency.numeric_value | number | 20                          |                  1234.56 |
| @sys-currency.location      | array  |[0,11]                       |                     [0,9]|
| @sys-currency.unit          | string | EUR*                        |                     EUR  |
*'@sys-currency.unit' gibt immer den dreistelligen ISO-Währungscode zurück.

Bei anderen unterstützten Sprachen und Landeswährungen werden funktional entsprechende Ergebnisse erzielt.

### Tipps zur Verwendung von '@system-currency'
{: #system-entities-currencty-usage-tips}

- Währungswerte werden ebenfalls als Instanzen von Entitäten '@sys-number' erkannt. Falls Sie separate Bedingungen verwenden, um sowohl auf Währungswerte als auch auf Zahlen zu überprüfen, positionieren Sie die Bedingung für die Überprüfung auf Währungen über der Bedingung, die auf eine Zahl überprüft.

- Falls Sie die Entität '@sys-currency' als Knotenbedingung verwenden und der Benutzer `$0` als Wert angibt, wird der Wert korrekt als Währung erkannt, die Bedingung jedoch für die Zahl 0 und nicht für die Währung 0 ausgewertet. Infolgedessen wird nicht die erwartete Antwort zurückgegeben. Um so auf Währungswerte zu prüfen, dass Nullen korrekt verarbeitet werden, verwenden Sie stattdessen die vollständige SpEL-Ausdruchssyntax `entities['sys-currency']?.value` in der Knotenbedingung.

## Entitäten '@sys-date' und '@sys-time'
{: #system-entities-sys-date-time}

Die Systementität `@sys-date` extrahiert Angaben wie `Freitag`, `heute` oder `1. November`. Der Wert dieser Entität speichert das entsprechende abgeleitete Datum als Zeichenfolge im Format 'jjjj-MM-tt', z. B. '2016-11-21'. Das System erweitert fehlende Elemente eines Datums (z. B. die Jahresangabe für '21. November') mit den aktuellen Datumswerten.

**Hinweis:** Bei der Ländereinstellung für Englisch ist das Standardsystemverhalten für die Datumseingabe MM/TT/JJJJ. Dies wird nur dann in TT/MM/JJJJ geändert, wenn die ersten beiden Zahlen größer als 12 sind. Der gespeicherte Wert hat weiterhin das Format 'jjjj-MM-tt'.

Die Systementität `@sys-time` extrahiert Angaben wie `2 Uhr nachmittags`, `um 4` oder `15:30`. Der Wert dieser Entität speichert die Uhrzeit als Zeichenfolge im Format 'HH:mm:ss'. Beispiel: '13:00:00'.

### Angaben mit Datum/Uhrzeit
{: #system-entities-date-time-mentions}

Angaben von Datum und Uhrzeit, z. B. `jetzt` oder `in zwei Stunden` werden als Angaben von zwei separaten Entitäten extrahiert, nämlich einer Entität `@sys-date` und einer Entität `@sys-time`. Diese beiden Angaben werden nur dann miteinander verknüpft, wenn sie dieselbe Literalzeichenfolge gemeinsam nutzen, die sich über die gesamte Angabe von Datum und Uhrzeit erstreckt.

Angaben für Datum und Uhrzeit, die aus mehreren Wörtern bestehen (z. B. `am Montag um 4`) werden ebenfalls als zwei Entitäten '@sys-date' und '@sys-time' extrahiert. Wenn sie zusammen aufeinanderfolgend angegeben sind, verwenden sie auch dieselbe Literalzeichenfolge, die sich über die gesamte Angabe von Datum und Uhrzeit erstreckt.

### Datums-/Zeitbereiche
{: #system-entities-date-time-ranges}

Angaben eines Datumsbereichs wie `am Wochenende`, `nächste Woche` oder `von Montag bis Freitag` werden als Paar von Entitäten `@sys-date` extrahiert, die den Anfang und das Ende des Bereichs angeben. Ähnlich werden Angaben von Zeitbereichen wie `von 2 bis 3` als zwei Entitäten `@sys-time` extrahiert, die die Anfangs- und die Endzeit angeben. Die beiden Entitäten im Paar nutzen eine Literalzeichenfolge gemeinsam, die der vollständigen Angabe des Datums- oder Zeitbereichs entspricht.

### Datums- und Zeitangaben mit `letzte/r/s` und `nächste/r/s`
{: #system-entities-last-next}

In einigen Ländereinstellungen wird mit einem Ausdruck wie 'letzter Montag' ausschließlich der Montag der vorherigen Woche angegeben. Bei anderen Ländereinstellungen hingegen wird mit 'letzter Montag' der letzte Tag angegeben, bei dem es sich um einen Montag handelte, der jedoch in derselben, aber auch in der vorherigen Woche liegen kann.

Beispiel: Für Freitag, den 16. Juni, könnte in einigen Ländereinstellungen 'letzter Montag' entweder den 12. Juni oder den 5. Juni referenzieren, während die Angabe in anderen Ländereinstellungen lediglich den 5. Juni (in der Vorwoche) referenziert. Dieselbe Logik gilt für einen Ausdruck wie 'nächster Montag'.

Der Service '{{site.data.keyword.conversationshort}}' behandelt Datumsangaben mit 'letze/r/s' und 'nächste/r/s' so , als ob der unmittelbar letzte oder nächste Tag referenziert wird, der entweder in derselben oder in einer vorherigen Woche liegen kann.

Bei Zeitausdrücken wie 'in den letzten 3 Tagen' oder 'in den nächsten 4 Stunden' ist die Logik funktional entsprechend. Beispielsweise führt dies bei der Angabe 'in den nächsten 4 Stunden' zu zwei Entitäten `@sys-time`, nämlich einer Entität für die aktuelle Uhrzeit und einer weiteren Entität für die Uhrzeit, die vier Stunden nach der aktuellen Uhrzeit liegt.

### Zeitzonen
{: #system-entities-time-zones}

Angaben eines Datums oder einer Uhrzeit, die sich auf die aktuelle Uhrzeit beziehen, werden unter Berücksichtigung einer ausgewählten Zeitzone aufgelöst. Standardmäßig ist dies UTC (GMT). Dies bedeutet, dass bei REST-API-Clients, die sich in anderen Zeitzonen als UTC befinden, der Wert für `jetzt` standardmäßig gemäß der aktuellen UTC-Zeit extrahiert wird.

Optional kann der REST-API-Client die lokale Zeitzone als Kontextvariable `$timezone` hinzufügen. Diese Kontextvariable sollte mit jeder Clientanforderung gesendet werden. Der Wert für `$timezone` sollte beispielsweise `America/Los_Angeles`, `EST` oder `UTC` sein. Eine vollständige Liste der unterstützten Zeitzonen finden Sie im Abschnitt [Unterstützte Zeitzonen](/docs/services/assistant?topic=assistant-time-zones).

Wenn die Variable `$timezone` angegeben ist, werden die Werte von relativen Angaben für '@sys-date' und '@sys-time' auf der Grundlage der Clientzeitzone anstelle von UTC berechnet.

#### Beispiele für Angaben, die relativ zur Zeitzone sind:
{: #system-entities-time-zone-examples}

- jetzt
- in zwei Stunden
- heute
- morgen
- in 2 Tagen

### Erkannte Formate
{: #system-entities-sys-date-time-formats}

- 21. November
- 10:30
- um 6
- dieses Wochenende

### Rückgaben
{: #system-entities-sys-date-time-returns}

Für die Angabe `November 21` gibt '@sys-date' die folgenden Werte zurück:

| Attribut               | Typ   | Rückgabe für `November 21` |
|-------------------------|--------|---------------------------:|
| @sys-date.literal       | string |                November 21 |
| @sys-date               | string |                20xx-11-21 *|
| @sys-date.location      | array |                     [0,11]  |
| @sys-date.calendar_type | string |                  GREGORIAN |

- Die Entität '@sys-date' gibt das Datum immer im Format jjjj-MM-tt zurück.
- \* Gibt das nächste übereinstimmende Datum zurück. Falls dieses Datum im aktuellen Jahr bereits zurückliegt, wird hierdurch das Datum des nächsten Jahres zurückgegeben.

Für die Eingabe `at 6 pm` gibt die Entität '@sys-time' die folgenden Werte zurück:

| Attribut               | Typ   | Rückgabe für `at 6 pm` |
|-------------------------|--------|-----------------------:|
| @sys-time.literal       | string |                at 6 pm |
| @sys-time               | string |               18:00:00 |
| @sys-time.location      | array |                   [0,7]|
| @sys-time.calendar_type | string |              GREGORIAN |

- Die Entität '@sys-time' gibt die Uhrzeit immer im Format 'HH:mm:ss' zurück.

Informationen zur Verarbeitung von Datums- und Zeitwerten enthalten die Referenzinformationen zu Methoden für [Datum und Uhrzeit](/docs/services/assistant?topic=assistant-dialog-methods#dialog-methods-date-time).
{: tip}

## Entität '@sys-location'
{: #system-entities-sys-location}

**Diese Entität ist für Sprachen, die im Abschnitt [Unterstützte Sprachen](/docs/services/assistant?topic=assistant-language-support) aufgeführt sind, als BETAVERSION verfügbar**: Die Systementität '@sys-location' extrahiert Ortsnamen (Staat, Land/Provinz, Stadt, Gemeinde usw.) aus der Eingabe des Benutzers. Der Wert der Entität ist kein Systemstandardwert des Standorts.

### Erkannte Formate
{: #system-entities-sys-location-formats}

- Boston
- U.S.A.
- New South Wales

Informationen zur Verarbeitung von Zeichenfolgewerten enthalten die Referenzinformationen zu Methoden für [Zeichenfolgen](/docs/services/assistant?topic=assistant-dialog-methods#dialog-methods-strings).
{: tip}

## Entität '@sys-number'
{: #system-entities-sys-number}

Die Systementität '@sys-number' erkennt Zahlen, die als Ziffern oder Zahlwörter geschrieben sind. In beiden Fällen wird ein numerischer Wert zurückgegeben.

### Erkannte Formate
{: #system-entities-sys-number-formats}

- 21
- einundzwanzig
- 3.13

### Metadaten
{: #system-entities-sys-number-metadata}

- `.numeric_value`: Der kanonische numerische Wert als Zahl des Typs 'Integer' oder 'Double'.

### Rückgaben
{: #system-entities-sys-number-returns}

Für die Eingabe `twenty` oder `1,234.56`, gibt die Systementität '@sys-number' die folgenden Werte zurück:

| Attribut                   | Typ   | Rückgabe für `twenty` | Rückgabe für `1,234.56` |
|-----------------------------|--------|-------------------|------------------------:|
| @sys-number               | string | 20                |                 1234.56 |
| @sys-number.literal       | string | twenty            |                1,234.56 |
| @sys-number.location      | array |  [0,6]             |                    [0,8]|
| @sys-number.numeric_value | number | 20                |                 1234.56 |

Für die Eingabe `veinte` oder `1.234,56` in Spanisch gibt die Entität '@sys-number' die folgenden Werte zurück:

| Attribut                   | Typ   | Rückgabe für `veinte` | Rückgabe für `1.234,56` |
|-----------------------------|--------|-----------------------|------------------------:|
| @sys-number               | string | 20                    |                 1234.56 |
| @sys-number.literal       | string | veinte                |                1.234,56 |
| @sys-number.location      | array  | [0,6]                 |                   [0,8] |
| @sys-number.numeric_value | number | 20                    |                 1234.56 |

Bei anderen unterstützten Sprachen werden funktional entsprechende Ergebnisse erzielt.

### Tipps zur Verwendung von '@system-number'
{: #system-entities-sys-number-usage-tips}

- Falls Sie die Entität '@sys-number' als Knotenbedingung verwenden und der Benutzer 0 als Wert angibt, wird der Wert 0 korrekt als Zahl erkannt, die Bedingung jedoch mit 'false' ausgewertet und die zugehörige Antwort kann nicht korrekt zurückgegeben werden. Um so auf Zahlen zu prüfen, dass Nullen korrekt verarbeitet werden, verwenden Sie stattdessen die vollständige SpEL-Ausdruckssyntax `entities['sys-number']?.value` in der Knotenbedingung.

- Falls Sie die Entität '@sys-number' zum Vergleichen von Zahlenwerten in einer Bedingung verwenden, müssen Sie unbedingt eine separate Überprüfung auf das Vorhandensein einer Zahl aufnehmen. Wenn keine Zahl gefunden wird, wird '@sys-number' mit null ausgewertet, was dazu führen kann, dass der Vergleich mit 'true' ausgewertet wird, obwohl keine Zahl vorhanden ist.

  Verwenden Sie beispielsweise `@sys-number<4` nicht alleine, weil `@sys-number` mit null ausgewertet wird, wenn keine Zahl gefunden wird. Da null kleiner als 4 ist, wird die Bedingung in diesem Fall mit 'true' ausgewertet, obwohl keine Zahl vorhanden ist.

  Verwenden Sie stattdessen `@sys-number AND @sys-number<4` . Falls keine Zahl vorhanden ist, wird die erste Bedingung mit 'false' ausgewertet, was entsprechend dazu führt, dass die gesamte Bedingung mit 'false' ausgewertet wird.

Informationen zur Verarbeitung von Zahlenwerten enthalten die Referenzinformationen zu Methoden für [Zahlen](/docs/services/assistant?topic=assistant-dialog-methods#dialog-methods-numbers).
{: tip}

## Entität '@sys-percentage'
{: #system-entities-sys-percentage}

Die Systementität '@sys-percentage' erkennt Prozentsätze, die in einer verbalen Äußerung mit dem Prozentzeichen oder mit dem Wort `Prozent` geschrieben sind. In beiden Fällen wird ein numerischer Wert zurückgegeben.

### Erkannte Formate
{: #system-entities-sys-percentage-formats}

- 15%
- 10 Prozent

### Metadaten
{: #system-entities-sys-percentage-metadata}

`.numeric_value`: Der kanonische numerische Wert als Zahl des Typs 'Integer' oder 'Double'.

### Rückgaben
{: #system-entities-sys-percentage-returns}

Für die Eingabe `1,234.56%` gibt '@sys-percentage' die folgenden Werte zurück:

| Attribut                     | Typ   | Rückgabe für `1,234.56%` |
|-------------------------------|--------|-------------------------:|
| @sys-percentage               | string |                  1234.56 |
| @sys-percentage.literal       | string |                1,234.56% |
| @sys-percentage.location      | array  |                    [0,9] |
| @sys-percentage.numeric_value | number |                  1234.56 |

Für die Eingabe `1.234,56%` in Spanisch gibt '@sys-currency' die folgenden Werte zurück:

| Attribut                     | Typ   | Rückgabe für `1.234,56%` |
|-------------------------------|--------|-------------------------:|
| @sys-percentage               | string |                  1234.56 |
| @sys-percentage.literal       | string |                1.234,56% |
| @sys-percentage.location      | array  |                    [0,9] |
| @sys-percentage.numeric_value | number |                  1234.56 |

Bei anderen unterstützten Sprachen werden funktional entsprechende Ergebnisse erzielt.

### Tipps zur Verwendung von '@system-percentage'
{: #system-entities-sys-percentage-usage-tips}

- Prozentwerte werden ebenfalls als Instanzen von Entitäten '@sys-number' erkannt. Falls Sie separate Bedingungen verwenden, um sowohl auf Prozentwerte als auch auf Zahlen zu überprüfen, positionieren Sie die Bedingung für die Überprüfung auf Prozentwerte über der Bedingung, die auf eine Zahl überprüft.

- Falls Sie die Entität '@sys-percentage' als Knotenbedingung verwenden und der Benutzer `0%` als Wert angibt, wird der Wert korrekt als Prozentsatz erkannt, die Bedingung jedoch mit der Zahl 0 und nicht mit dem Prozentzsatz ausgewertet und daher nicht die erwartete Antwort zurückgegeben. Um so auf Prozentsätze zu prüfen, dass Nullwerte korrekt verarbeitet werden, verwenden Sie stattdessen die vollständige SpEL-Ausdruckssyntax `entities['sys-percentage']?.value` in der Knotenbedingung.

- Falls Sie einen Wert wie `1-2%` eingeben, werden die Werte `1%` und `2%` als Systementitäten zurückgegeben. Der Index ist der gesamte Bereich zwischen 1% und 2%, beide Entitäten besitzen denselben Index.

## Entität '@sys-person'
{: #system-entities-sys-person}

**Diese Entität ist für Sprachen, die im Abschnitt [Unterstützte Sprachen](/docs/services/assistant?topic=assistant-language-support) aufgeführt sind, als BETAVERSION verfügbar**: Die Systementität '@sys-person' extrahiert Namen aus der Eingabe des Benutzers. Namen werden einzeln erkannt, d. h. 'Joe' wird nicht wie 'Joseph' behandelt und umgekehrt. Der Wert der Entität ist kein Systemstandardwert des Namens.

### Erkannte Formate
{: #system-entities-sys-person-formats}

- Ronald
- Jane Doe
- Vijay

Informationen zur Verarbeitung von Zeichenfolgewerten enthalten die Referenzinformationen zu Methoden für [Zeichenfolgen](/docs/services/assistant?topic=assistant-dialog-methods#dialog-methods-strings).
{: tip}
