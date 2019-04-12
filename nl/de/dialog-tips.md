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
{:gif: data-image-type='gif'}

# Tipps zum Erstellen von Dialogmodulen
{: #dialog-tips}

Hier finden Sie Informationen zur Vorgehensweise beim Erstellen eines Dialogmoduls und Tipps zum Ausführen komplexerer Schritte.
{: shortdesc}

Lesen Sie diese Tipps von erfahrenen Dialogentwicklern.

## Gesamtes Dialogmodul planen
{: #dialog-tips-plan}

- Planen Sie die Gestaltung des gesamten Dialogmoduls, das Sie erstellen möchten, bevor Sie einen Dialogmodulknoten im Tool hinzufügen. Skizzieren Sie das Modul auf Papier, falls erforderlich. 
- Gründen Sie Ihre Designentscheidungen soweit möglich auf Daten aus realen Szenarien und Verhaltensweisen. Fügen Sie keine Knoten zum Verarbeiten einer Situation hinzu, von der jemand *meint*, dass sie auftreten könnte.
- Vermeiden Sie es, Geschäftsprozesse unverändert zu kopieren. Geschäftsprozesse sind nur selten dialogorientiert.
- Wenn ein Prozess bereits von Menschen genutzt wird, untersuchen Sie deren Herangehensweise. Menschen optimieren den Prozess normalerweise aus der Dialogperspektive. 
- Legen Sie den Umgangston, die Persönlichkeit und die Positionierung Ihres virtuellen Assistenten fest. Halten Sie diese Festlegungen in dem Dialogmodul, das Sie erstellen, konsequent durch. 
- Stellen Sie den Assistenten niemals als menschliche Person dar. Wenn die Benutzer den Assistenten für eine Person halten und dann erkennen müssen, dass dies nicht zutrifft, werden sie dem Assistenten wahrscheinlich misstrauen. 
- Es muss nicht immer ein Dialog sein. Manchmal erfüllt ein Webformular den Zweck besser.

## Knoten hinzufügen
{: #dialog-tips-nodes}

- Fügen Sie einen aussagefähigen Knotennamen hinzu, der den Zweck des Knotens beschreibt.

  Momentan ist Ihnen klar, was der Knoten bewirkt, aber in ein paar Monaten möglicherweise nicht mehr. Sie selbst und alle künftigen Teammitglieder werden Ihnen für den beschreibenden Knotennamen dankbar sein. Außerdem wird der Knotenname im Protokoll angegeben. Dies kann bei der Fehlerbehebung für einen Dialog später hilfreich sein. 
- Verwenden Sie zum Sammeln der Informationen, die zum Ausführen einer Task erforderlich sind, einen Knoten mit Slots anstelle einer Reihe separater Knoten, um Informationen bei den Benutzern abzufragen. Weitere Informationen enthält der Abschnitt [Informationen mit Slots erfassen](/docs/services/assistant?topic=assistant-dialog-slots).
- Informieren Sie bei einem komplexen Prozessablauf die Benutzer bereits am Anfang, welche Informationen von ihnen bereitgestellt werden müssen.
- Machen Sie sich damit vertraut, wie der Service die Baumstruktur des Dialogmoduls durchläuft und wie sich Ordner, Verzweigungen, Sprungaktionen und Abschweifungen auf die Route auswirken. Weitere Informationen enthält der Abschnitt [Dialogablauf](/docs/services/assistant?topic=assistant-dialog-overview#dialog-overview-flow).
- Fügen Sie nicht überall Sprungaktionen ein. Sie erhöhen die Komplexität des Dialogablaufs und erschweren später die Fehlerbehebung für das Dialogmodul.
- Verwenden Sie zum Springen zu einem Knoten in derselben Verzweigung wie der aktuelle Knoten die Aktion *Benutzereingabe überspringen* anstelle der Aktion *Springen zu*.

  Durch diese Vorgehensweise erübrigt sich das Bearbeiten der Einstellungen des aktuellen Knotens, wenn die als Sprungziele verwendeten untergeordneten Knoten entfernt oder neu angeordnet werden. Weitere Informationen enthält der Abschnitt [Nächste Schritte definieren](/docs/services/assistant?topic=assistant-dialog-overview#dialog-overview-jump-to).
- Testen Sie vor dem Aktivieren von Abschweifungen, die von einem Knoten abgehen, die gängigsten Benutzerszenarios. Und stellen Sie sicher, dass Knoten, die potenzielle Abschweifungsziele sind, so konfiguriert werden, dass der Dialogablauf von ihnen zurück geleitet wird. Weitere Informationen enthält der Abschnitt [Abschweifungen](/docs/services/assistant?topic=assistant-dialog-runtime#dialog-runtime-digressions).

## Antworten hinzufügen
{: #dialog-tips-responses}

- Antworten sollten kurz und aussagekräftig sein. 
- Beziehen Sie die Absicht des Benutzers in die Antwort ein.

  Dies zeigt den Benutzern, dass der Bot ihr Anliegen versteht oder ihnen die Möglichkeit gibt, ein Missverständnis sofort zu korrigieren. 
- Fügen Sie in Antworten nur dann Links zu externen Sites ein, wenn die Antwort von Daten abhängt, die häufig geändert werden.
- Verwenden Sie nicht zu viele Schaltflächen. Das Auffordern der Benutzer, vordefinierte Optionen in einer Gruppe von Schaltflächen auszuwählen, hat wenig mit einem echten Dialog zu tun und verringert Ihre Chancen, die eigentliche Intention der Benutzer zu erfahren. Wenn reale Benutzer ihre Anliegen mit eigenen Worten formulieren, können Sie mithilfe dieser Eingaben das System trainieren und zutreffendere Absichten ableiten. 
- Verzichten Sie auf eine Gruppe von Knoten, wenn ein einziger Knoten ausreicht. Fügen Sie beispielsweise mehrere bedingte Antworten in einem einzelnen Knoten hinzu, um entsprechend den vom Benutzer angegebenen Details verschiedene Antworten zurückzugeben. Weitere Informationen enthält der Abschnitt [Bedingte Antworten](/docs/services/assistant?topic=assistant-dialog-overview#dialog-overview-multiple).
- Formulieren Sie die Antworten mit Bedacht. Wie auf Ihr System reagiert wird, hängt sehr von der Formulierung Ihrer Antwort ab. Durch eine kleine Änderung in einer einzigen Textzeile kann vermieden werden, dass viele Codezeilen eingefügt werden müssen, um eine komplexe programmgesteuerte Lösung zu implementieren. 
- Erstellen Sie häufig Sicherungskopien von Ihrem Skill. Weitere Informationen enthält der Abschnitt [Skill herunterladen](/docs/services/assistant?topic=assistant-skill-add#skill-add-download).

## Tipps zum Erfassen von Informationen aus Benutzereingaben
{: #dialog-tips-user-input}

Es ist nicht immer einfach, die richtige Syntax für Ihr Dialogmodul zu finden, um die Informationen präzise zu erfassen, die Sie in den Benutzereingaben finden möchten. Die folgenden Methoden können Ihnen helfen, häufig vorkommende Zielsetzungen zu erreichen.

- **Eingabe des Benutzers zurückgeben**: Sie können den genauen Wortlaut der Benutzeräußerung erfassen und in Ihrer Antwort zurückgeben. Mit dem folgenden SpEL-Ausdruck können Sie den zuvor vom Benutzer angegebenen Text in Ihrer Antwort wiederholen:

  `Sie sagten: <? input.text ?>.`

- **Wörterzahl der Benutzereingabe ermitteln**: Sie können jede der unterstützten Methoden für Zeichenfolgen auf das Objekt 'input.text' anwenden. Mit dem folgenden SpEL-Audruck können Sie beispielsweise feststellen, wie viele Wörter eine Benutzeräußerung enthält: 

  `input.text.split(' ').size()`

  Informationen zu weiteren Methoden, die Sie anwenden können, enthält der Abschnitt [Methoden für Zeichenfolgen in der Ausdruckssprache](/docs/services/assistant?topic=assistant-dialog-methods#dialog-methods-strings). 

- **Mehrere Absichten verarbeiten**: In einer Benutzereingabe wird der Wunsch geäußert, zwei verschiedene Tasks auszuführen. `Ich möchte ein Sparkonto eröffnen und eine Kreditkarte beantragen.` Wie kann das Dialogmodul beide Tasks erkennen? Mögliche Strategien hierfür enthält der Blog zum Thema [Compound questions](https://sodoherty.ai/2017/02/06/compound-questions/){: new_window} (zusammengesetzte Fragen) von Simon O'Doherty. (Simon ist ein Entwickler im {{site.data.keyword.conversationshort}}-Team.)

- **Mehrdeutige Absichten verarbeiten**: In einer Benutzereingabe wird ein mehrdeutiger Wunsch geäußert und der Service findet zwei oder mehr Knoten mit Absichten, die darauf zutreffen könnten. Wie kann das Dialogmodul erkennen, welche Verzweigung ausgewählt werden sollte? Wenn Sie die Vereindeutigung aktivieren, kann das Dialogmodul die verfügbaren Optionen anzeigen und den Benutzer auffordern, die zutreffende Option auszuwählen. Weitere Details enthält der Abschnitt [Vereindeutigung](/docs/services/assistant?topic=assistant-dialog-runtime#dialog-runtime-disambiguation).

- **Mehrere Entitäten in der Eingabe verarbeiten**: Wenn Sie nur den Wert der ersten erkannten Instanz eines Entitätstyps auswerten möchten, können Sie die Syntax `@entität == 'bestimmter_wert'` anstelle des Formats `@entität:(bestimmter_wert)` verwenden.

  Wenn Sie beispielsweise die Syntax `@gerät == 'Klimaanlage'` verwenden, werten Sie nur den Wert der ersten erkannten Entität `@gerät` aus. Die Verwendung von `@gerät:(Klimaanlage)` wird jedoch auf `entity['gerät'].contains('Klimaanlage')` erweitert, was immer dort zu einer Übereinstimmung führt, wo mindestens eine Entität `@gerät` mit dem Wert 'Klimaanlage' in der Benutzereingabe erkannt wird.

## Tipps zur Verwendung von Bedingungen
{: #dialog-tips-condition-usage}

- **Auf Werte mit Sonderzeichen überprüfen**: Wenn Sie überprüfen möchten, ob eine Entität oder Kontextvariable einen Wert enthält, in dem ein Sonderzeichen, z. B. ein Hochkomma (') vorkommt, müssen Sie den Wert, den Sie überprüfen möchten, in runde Klammern einschließen. Beispiel: Um zu überprüfen, ob eine Entität oder Kontextvariable den Namen `O'Reilly` enthält, müssen Sie den Namen in runde Klammern einschließen.

  `@person:(O'Reilly)` and `$person:(O'Reilly)`

  Der Service wandelt diese gekürzten Verweise in die folgenden vollständigen SpEL-Ausdrücke um:

  `entities['person']?.contains('O''Reilly')` and `context['person'] == 'O''Reilly'`

  SpEL verwendet ein zweites Hochkomma als Escapezeichen für das Hochkomma in dem Namen.
  {: note}

- **Auf mehrere Werte prüfen**: Wenn Sie mehr als einen Wert überprüfen möchten, können Sie eine Bedingung mit Operatoren des Typs OR (`||`) erstellen, um mehrere Werte in der Bedingung aufzulisten. Wenn Sie beispielsweise eine Bedingung definieren möchten, die wahr (true) ist, wenn die Kontextvariable `$state` die Abkürzungen für Massachusetts, Maine oper New Hampshire enthält, können Sie den folgenden Ausdruck verwenden:

  `$state:MA || $state:ME || $state:NH`

- **Auf Zahlenwerte prüfen**: Stellen Sie beim Vergleichen von Zahlen zuerst sicher, dass die zu prüfende Entität oder Variable über einen Wert verfügt. Wenn die Entität oder Variable nicht über einen Zahlenwert verfügt, wird beim Vergleichen numerischer Werte ein Nullwert (0) vorausgesetzt.

  Angenommen Sie möchten prüfen, ob ein in der Benutzereingabe angegebener Dollarwert kleiner als 100 ist. Wenn Sie die Bedingung `@price < 100`verwenden und die Entität `@price` null ist, dann wird die Bedingung als `true` ausgewertet, da 0 kleiner als 100 ist, obwohl nie ein Preis festgelegt wurde. Um ungenaue Ergebnisse dieser Art zu verhindern, können Sie eine Bedingung wie `@price AND @price < 100` verwenden. Falls `@price` keinen Wert besitzt, gibt diese Bedingung richtigerweise 'false' zurück.

- **Auf Absichten mit einem bestimmten Muster im Absichtsnamen überprüfen**: Sie können eine Bedingung verwenden, die nach Absichten sucht, die mit einem bestimmten Muster übereinstimmen. Beispiel: Um alle erkannten Absichten zu suchen, deren Absichtsnamen mit 'User_' beginnen, können Sie eine Syntax wie die folgende in der Bedingung verwenden:

  `intents[0].intent.startsWith("User_")`

  Dabei werden jedoch alle erkannten Absichten berücksichtigt, auch solche mit einem niedrigeren Konfidenzwert als 0,2. Überprüfen Sie zusätzlich, dass Absichten, die Watson aufgrund des Konfidenzwerts als irrelevant einstuft, nicht zurückgegeben werden. Um dies zu erreichen, ändern Sie die Bedingung wie folgt:

  `!irrelevant && intents[0].intent.startsWith("User_")`

- **Auswirkungen der unscharfen Suche auf die Entitätserkennung**: Wenn Sie eine Entität als Bedingung verwenden und die unscharfe Suche aktiviert ist, wird `@entitätsname` nur dann mit 'true' ausgewertet, wenn die Konfidenz der Übereinstimmung größer als 30 % ist. Also nur, wenn `@entitätsnname.confidence > .3` zutrifft.

## Entitätsmustergruppen in der Eingabe erkennen und speichern
{: #dialog-tips-get-pattern-groups}

Fügen Sie '.literal' an den Entitätsnamen an, um den Wert einer Musterentität in einer Kontextvariablen zu speichern. Die Verwendung dieser Syntax stellt sicher, dass der exakte Textbereich aus der Benutzereingabe, der dem angegebenen Muster entspricht, in der Variablen gespeichert wird.

| Variable   | Wert               |
|------------|---------------------|
| email      | <? @email.literal ?> |

Um den Text aus einer einzelnen Gruppe in einer Musterentität mit definierten Gruppen zu speichern, geben Sie die Arraynummer der Gruppe an, die Sie speichern möchten. Angenommen, das Entitätsmuster für die Entität '@phone_number' ist wie folgt definiert (beachten Sie, dass die Klammern Mustergruppen einschließen):

`\b((958)|(555))-(\d{3})-(\d{4})\b`

Um nur die Ortsnetzkennzahl der in der Benutzereingabe angegebenen Telefonnummer zu speichern, können Sie die folgende Syntax verwenden:

| Variable       | Wert                         |
|----------------|-------------------------------|
| area_code      | <? @phone_number.groups[1] ?> |

Die Gruppen werden durch den regulären Ausdruck begrenzt, der zum Definieren des Gruppenmusters verwendet wird. Angenommen, die Benutzereingabe, die mit dem in der Entität `@phone_number` definierten Muster übereinstimmt, lautet wie folgt: `958-234-3456`. In diesem Fall werden die folgenden Gruppen erstellt:

| Gruppennummer | Wert in der Regex-Engine  | Wert im Dialogmodul   | Erläuterung |
|--------------|---------------------|----------------|-------------|
| groups[0]    | `958-234-3456`      | `958-234-3456` | Die erste Gruppe ist immer die vollständige übereinstimmende Zeichenfolge |
| groups[1]    | `((958)`l`(555))`   | `958`          | Zeichenfolge, die dem regulären Ausdruck für die erste definierte Gruppe, d. h. `((958)`l`(555))`, entspricht |
| groups[2]    | `(958)`             | `958`          | Übereinstimmung mit der Gruppe, die als erster Operand im Ausdruck OR `((958)`l`(555))` enthalten ist |
| groups[3]    | `(555)`             | `null`         | Keine Übereinstimmung mit der Gruppe, die als zweiter Operand im Ausdruck OR `((958)`l`(555))` enthalten ist |
| groups[4]    | `(\d{3})`           | `234`          | Zeichenfolge, die mit dem für die Gruppe definierten regulären Ausdruck übereinstimmt |
| groups[5]    | `(\d{4})`           | `3456`         | Zeichenfolge, die mit dem für die Gruppe definierten regulären Ausdruck übereinstimmt |
{: caption="Gruppendetails" caption-side="top"}

Als Hilfe für die Entscheidung, welche Gruppennummer zum Erfassen des für Sie relevanten Eingabeabschnitts verwendet werden soll, können Sie Informationen zu allen Gruppen auf einmal extrahieren. Erstellen Sie mit der folgenden Syntax eine Kontextvariable, die ein Array aller gruppierten Übereinstimmungen mit Musterentitäten zurückgeben:

| Variable                 | Wert                      |
|--------------------------|----------------------------|
| array_of_matched_groups  | <? @phone_number.groups ?> |

Geben Sie in der Anzeige 'Ausprobieren' einige Testwerte mit Telefonnummern ein. Für die Eingabe `958-123-2345` setzt dieser Ausdruck `$array_of_matched_groups` auf den Wert `["958-123-2345","958","958",null,"123","2345"]`.

Anschließend können Sie alle Werte in dem mit 0 beginnenden Array zählen, um die entsprechende Gruppennummer abzurufen.

| Array-Elementwert | Array-Elementnummer |
|---------------------|----------------------|
| "958-123-2345"      | 0 |
| "958"               | 1 |
| "958"               | 2 |
| null                | 3 |
| "123"               | 4 |
| "2345"              | 5 |
{: caption="Array-Elemente" caption-side="top"}

Am Ergebnis ist zu erkennen, dass mit der Gruppe 5 die letzten vier Stellen der Telefonnummer erfasst werden können. 

Verwenden Sie die folgende Syntax, um die JSONArray-Struktur zurückzugeben, die zum Darstellen der gruppierten Musterentität erstellt wird:

| Variable             | Wert                           |
|----------------------|---------------------------------|
| json_matched_groups  | <? @phone_number.groups_json ?> |

Dieser Ausdruck legt für `$json_matched_groups` das folgende JSON-Array fest:

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

Die Entitätseigenschaft `location` gibt durch eine relative Zeichenposition mit der Basis null an, wo der erkannte Entitätswert im Eingabetext beginnt und endet.
{: note}

Wenn Sie erwarten, dass die Eingabe zwei Telefonnummern enthält, können Sie auf zwei Telefonnummern prüfen. Verwenden Sie gegebenenfalls die folgende Syntax, um beispielsweise die Ortsnetzkennzahl der zweiten Nummer zu erfassen.

| Variable         | Wert                                       |
|------------------|---------------------------------------------|
| second_areacode  | <? entities['phone_number'][1].groups[1] ?> |

Wenn die Eingabe aus der Zeichenfolge `Ich möchte meine Rufnummer von 958-234-3456 in 555-456-5678 ändern` besteht, dann ist `$second_areacode` gleich `555`.

## Details zu API-Aufrufen anzeigen
{: #dialog-tips-inspect-api}

Beim Testen Ihres Dialogmoduls in der Anzeige 'Ausprobieren' möchten Sie möglicherweise wissen, wie die zugrunde liegenden API-Aufrufe aussehen, die von dem Service zurückgegeben werden. Sie können die API-Aufrufe mit den Entwicklertools in Ihrem Webbrowser untersuchen. 

Öffnen Sie in Chrome beispielsweise die Entwicklertools. Klicken Sie auf das Tool 'Network'. Im Abschnitt 'Name' werden mehrere API-Aufrufe aufgelistet. Klicken Sie auf den zugeordneten Aufruf 'message' für Ihre Testäußerung und klicken Sie anschließend auf die Spalte 'Response', um den Hauptteil der API-Antwort anzuzeigen. Darin werden die in der Benutzereingabe erkannten Absichten und Entitäten mit zugehörigen Konfidenzbewertungen aufgelistet sowie die Werte der Kontextvariablen zum Zeitpunkt des Aufrufs. Um den Antworthauptteil im strukturierten Format anzuzeigen, klicken Sie auf die Spalte 'Preview'.

![Zeigt die Vorgehensweise zum Anzeigen der Details zum API-Aufruf mit den Entwicklertools des Web-Browsers 'Chrome'.](images/api-browser-dev.png)
