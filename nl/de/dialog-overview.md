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
{:table: .aria-labeledby="caption"}

# Dialogmodule im Überblick
{: #dialog-overview}

Das Dialogmodul verwendet die in der Benutzereingabe erkannten Absichten sowie Kontext aus der Anwendung, um mit dem Benutzer zu interagieren und letztendlich eine hilfreiche Antwort zu liefern.
{: shortdesc}

Das Dialogmodul ordnet Absichten (Äußerungen der Benutzer) den Antworten (Erwiderungen des Bots) zu. Die Antwort könnte die Antwort auf eine Frage wie beispielsweise `Wo kann ich tanken?` oder die Ausführung eines Befehls sein, z. B. das Einschalten des Radios. Entweder geben die Absicht und die Entität genug Informationen an, damit die korrekte Antwort ermittelt werden kann, oder das Dialogmodul kann vom Benutzer eine weitere Eingabe erfragen, die benötigt wird, um richtig zu antworten. Fragt ein Benutzer beispielsweise `Wo bekomme ich etwas zu essen?`, dann kann es sein, dass geklärt werden muss, ob er beispielsweise ein Restaurant, ein Lebensmittelgeschäft oder einen Take-Away-Imbiss sucht. Sie können weitere Details in einer Textantwort abfragen und einen oder mehrere untergeordnete Knoten erstellen, um die neue Eingabe zu verarbeiten.

<iframe class="embed-responsive-item" id="youtubeplayer" title="Dialogmodul um Überblick" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/XkhAMe9gSFU?rel=0" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

Hinweis: Das Video dauert 15 Minuten. In den ersten 5 Minuten wird das Hinzufügen eines Knotens beschrieben.

Das Dialogmodul wird im Tool '{{site.data.keyword.conversationshort}}' grafisch als Baumstruktur dargestellt. Erstellen Sie für jede Absicht, die Ihr Dialog verarbeiten soll, eine Verzweigung. Eine Verzweigung besteht aus mehreren Knoten.

## Dialogmodulknoten
{: #dialog-overview-nodes}

Jeder Dialogmodulknoten enthält mindestens eine Bedingung und eine Antwort.

![Zeigt die Benutzereingabe für ein Feld, das die Anweisung 'If: BEDINGUNG, Then: ANTWORT' enthält](images/node1-empty.png)

- Bedingung: Gibt die Informationen an, die in der Benutzereingabe für diesen Knoten im Dialogmodul enthalten sein müssen, damit der Knoten ausgelöst wird. Bei den Informationen handelt es sich normalerweise um eine bestimmte Absicht. Die Informationen können aber auch einen Entitätstyp, einen Entitätswert oder den Wert einer Kontextvariablen angeben. Weitere Informationen enthält der Abschnitt [Bedingungen](#dialog-overview-conditions).
- Antwort: Die verbale Äußerung, mit der der Service dem Benutzer antwortet. Die Antwort kann auch so konfiguriert werden, dass ein Bild bzw. eine Liste der Optionen angezeigt wird oder programmgestützte Aktionen ausgelöst werden. Weitere Informationen enthält der Abschnitt [Antworten](#dialog-overview-responses).

Ein Knoten ist mit einer IF-THEN-Konstruktion vergleichbar; falls diese Bedingung zutrifft, wird diese Antwort zurückgegeben.

Der folgende Knoten wird beispielsweise ausgelöst, wenn die Funktion für die Verarbeitung natürlicher Sprache des Service feststellt, dass die Benutzereingabe die Absicht `#cupcake-menu` enthält. Als Ergebnis der Knotenauslösung reagiert der Service mit einer entsprechenden Antwort.

![Zeigt die Frage des Benutzers nach Cupcake-Geschmacksrichtungen. Die IF-Bedingung ist '#cupcake-menu'; die THEN-Antwort ist eine Liste von Cupcake-Geschmacksrichtungen.](images/node1-simple.png)

Ein einzelner Knoten mit einer Bedingung und einer Antwort kann einfache Benutzeranforderungen abwickeln. Meistens haben Benutzer jedoch kompliziertere Fragen oder benötigen Hilfe bei komplexeren Aufgaben. Durch das Hinzufügen von untergeordneten Knoten können Sie den Benutzer auffordern, alle weiteren Informationen anzugeben, die der Service benötigt.

![Der erste Knoten im Dialogmodul fragt nach dem Typ des vom Benutzer gewünschten Cupcakes, also glutenfrei oder normal. Er hat zwei untergeordnete Knoten, die je nach Angabe des Benutzers eine andere Antwort geben.](images/node1-children.png)

## Dialogmodulablauf
{: #dialog-overview-flow}

Das Dialogmodul, das Sie erstellen, wird durch den Service vom ersten bis zum letzten Knoten nacheinander verarbeitet.

![Der Pfeil weist neben drei Knoten abwärts, um kenntlich zu machen, dass der Dialogmodulablauf vom ersten zum letzten Knoten verläuft](images/node-flow-down.png)

Wenn der Service beim Durcharbeiten der Baumstruktur eine Bedingung findet, die erfüllt ist, löst er den entsprechenden Knoten aus. Anschließend bewegt er sich an dem ausgelösten Knoten entlang, um die Benutzereingabe auf etwaige untergeordnete Knotenbedingungen zu überprüfen. Die Überprüfung der untergeordneten Knoten erfolgt wieder vom ersten bis zum letzten Knoten.

Der Service setzt die Durcharbeitung der Baumstruktur für das Dialogmodul vom ersten bis zum letzten Knoten, entlang aller ausgelösten Knoten, dann vom ersten bis zum letzten untergeordneten Knoten und entlang aller ausgelösten untergeordneten Knoten fort, bis der letzte Knoten in der verfolgten Verzweigung erreicht ist.

![Pfeil 1 zeigt vom ersten zum letzten Stammknoten, Pfeil 2 zeigt an einem ausgelösten Knoten entlang und Pfeil 3 zeigt vom ersten zum letzten untergeordneten Knoten des ausgelösten Knotens.](images/node-flow.png)

Wenn Sie damit beginnen, das Dialogmodul zu erstellen, müssen Sie festlegen, welche Verzweigungen Sie aufnehmen wollen und wo sie platziert sein sollen. Die Reihenfolge der Verzweigungen ist wichtig, da Knoten vom ersten bis zum letzten Knoten ausgewertet werden. Der erste Stammknoten, mit dessen Bedingung die Eingabe übereinstimmt, wird verwendet. Knoten, die in der Baumstruktur unter ihm liegen, werden nicht ausgelöst.

Wenn der Service das Ende einer Verzweigung erreicht oder in der Gruppe der untergeordneten Knoten, die momentan ausgewertet wird keine Bedingung finden kann, die mit 'true' ausgewertet wird, springt er zurück Ausgangsebene der Baumstruktur. Dann beginnt der Service erneut, die Stammknoten vom ersten bis zum letzten Knoten zu verarbeiten. Wenn keine der Bedingungen mit 'true' ausgewertet wird, wird die Antwort vom letzten Knoten der Baumstruktur zurückgegeben, der in der Regel eine Sonderbedingung `anything_else` enthält.

Der standardmäßige Ablauf (vom ersten bis zum letzten Knoten) kann mit den folgenden Methoden unterbrochen werden:

- Durch Anpassen der Aktionen, die nach der Verarbeitung eines Knotens ausgeführt werden. Sie können einen Knoten beispielsweise so konfigurieren, dass nach seiner Verarbeitung direkt zu einem anderen Knoten gesprungen wird, selbst wenn sich der andere Knoten in der Baumstruktur weiter oben befindet. Weitere Informationen enthält der Abschnitt [Nächste Schritte definieren](#dialog-overview-jump-to).
- Durch Konfigurieren bedingter Aktionen, um zu einem anderen Knoten zu wechseln. Weitere Informationen enthält der Abschnitt [Bedingte Antworten](#dialog-overview-multiple).
- Durch Konfigurieren von Abschweifungseinstellungen für Dialogmodulknoten. Abschweifungen können sich auch darauf auswirken, wie Benutzer während der Laufzeit durch die Knoten navigieren. Wenn Sie abgehende Abschweifungen von den meisten Knoten zulassen und die Rückkehr konfigurieren, können die Benutzer leichter von einem Knoten zu einem anderen und zurück springen. Weitere Informationen enthält der Abschnitt [Abschweifungen](/docs/services/assistant?topic=assistant-dialog-runtime#dialog-runtime-digressions).

## Bedingungen
{: #dialog-overview-conditions}

Ein Knotenbedingung legt fest, ob dieser Knoten im Dialog verwendet wird. Antwortbedingungen legen fest, welche Antwort an einen Benutzer zurückgegeben wird.

- [Artefakte für Bedingungen](#dialog-overview-condition-artifacts)
- [Sonderbedingungen](#dialog-overview-special-conditions)
- [Details zur Bedingungssyntax](#dialog-overview-condition-syntax)

Tipps zum Ausführen erweiterter Aktionen in Bedingungen enthält der Abschnitt [Tipps zur Verwendung von Bedingungen](/docs/services/assistant?topic=assistant-dialog-tips#dialog-tips-condition-usage-tips).

### Artefakte für Bedingungen
{: #dialog-overview-condition-artifacts}

Zum Definieren einer Bedingung können Sie eines oder mehrere der folgenden Artefakte in einer beliebigen Kombination verwenden:

- **Kontextvariable**: Der Knoten wird verwendet, wenn der Ausdruck der Kontextvariablen, die Sie angeben, mit 'true' ausgewertet wird. Verwenden Sie die Syntax `$variablenname:wert` oder `$variablenname == 'wert'`.

  Bei Knotenbedingungen wird dieses Artefakt in der Regel zusammen mit einem Operator AND oder OR und einem weiteren Bedingungswert verwendet. Der Grund dafür ist, dass ein Element in der Benutzereingabe den Knoten auslösen muss. Der Abgleich mit dem Wert der Kontextvariablen reicht zum Auslösen nicht aus. Wenn das Benutzereingabeobjekt beispielsweise einen Wert für die Kontextvariable festlegt, dann wird der Knoten ausgelöst. 

  Definieren Sie eine Knotenbedingung, die sich auf den Wert einer Kontextvariablen bezieht, nicht im selben Dialogmodulknoten, in dem Sie den Wert der Kontextvariablen festlegen. {: tip}

  Für Antwortbedingungen kann dieser Artefakttyp einzeln verwendet werden. Die Antwort kann basierend auf einem bestimmten Kontextvariablenwert variieren. Beispiel: `$city:Boston` prüft, ob die Kontextvariable `$city` den Wert `Boston` enthält. Wenn dies der Fall ist, wird die Antwort zurückgegeben. 
  
  Weitere Informationen zu Kontextvariablen finden Sie unter [Konextvariablen](/docs/services/assistant?topic=assistant-dialog-runtime#dialog-runtime-context).

- **Entität**: Der Knoten wird verwendet, wenn ein Wert oder Synonym für die Entität in der Benutzereingabe erkannt wird. Verwenden Sie die Syntax `@entitätsname`. Beispiel: `@city` überprüft, ob Städtenamen, die für die Entität '@city' definiert sind, in der Benutzereingabe erkannt wurden. Wenn dies der Fall ist, wird der Knoten oder die Antwort verarbeitet. 

  Ziehen Sie die Erstellung eines Peerknotens in Betracht, der den Fall abwickelt, dass keiner der Werte bzw. keines der Synonyme für die Entität erkannt wird. {: tip}

  Weitere Informationen zu Entitäten finden Sie unter [Entitäten definieren](/docs/services/assistant?topic=assistant-entities).

- **Entitätswert**: Der Knoten wird verwendet, wenn der Entitätswert in der Benutzereingabe erkannt wird. Verwenden Sie die Syntax `@entitätsname:wert` und geben Sie einen für die Entität definierten Wert an (kein Synonym). Beispiel: `@city:Boston` prüft, ob der angegebene Städtename (`Boston`) in der Benutzereingabe erkannt wurde.

  Wenn Sie in einem Peerknoten überprüfen möchten, ob eine Entität vorhanden ist, ohne einen bestimmten Wert für sie anzugeben, achten Sie darauf, den Knoten, der diesen bestimmten Entitätswert prüft, vor dem Peerknoten anzuordnen, der nur das Vorhandensein der Entität prüft. Andernfalls wird dieser Knoten niemals ausgelöst.
  {: tip}

  Wenn es sich bei der Entität um eine Musterentität mit Erfassungsgruppen handelt, können Sie nach einer Übereinstimmung mit einem bestimmten Wert der Gruppe suchen. Sie können zum Beispiel die folgende Syntax verwenden: `@us_phone.groups[1] == '617'`.
  Weitere Informationen enthält der Abschnitt [Musterentitätsgruppen in der Eingabe speichern und erkennen](/docs/services/assistant?topic=assistant-dialog-tips#dialog-tips-get-pattern-groups).

- **Absicht**: Die einfachste Bedingung ist eine einzelne Absicht. Der Knoten wird verwendet, wenn nach Auswertung der Benutzereingabe durch die Verarbeitungsfunktion für natürliche Sprache des Service festgestellt wird, dass der Zweck der Benutzereingabe mit der vordefinierten Absicht übereinstimmt. Verwenden Sie die Syntax `#absichtsname`. `#weather` überprüft zum Beispiel, ob in der Benutzereingabe nach einer Wettervorhersage gefragt wird. Wenn dies der Fall ist, wird der bedingte Knoten mit der Absicht `#weather` verarbeitet. 

  Weitere Informationen zu Absichten finden Sie unter [Absichten definieren](/docs/services/assistant?topic=assistant-intents).

- **Sonderbedingung**: Dies sind Bedingungen, die mit dem Service bereitgestellt werden und die Sie zur Ausführung von allgemeinen Dialogmodulfunktionen verwenden können. Weitere Details finden Sie in der Tabelle **Sonderbedingungen** im nächsten Abschnitt.

### Sonderbedingungen
{: #dialog-overview-special-conditions}

| Bedingungssyntax     | Beschreibung |
|----------------------|-------------|
| `anything_else`      | Sie können diese Bedingung am Ende eines Dialogmoduls verwenden, damit sie verarbeitet wird, wenn die Benutzereingabe mit keinen anderen Dialogmodulknoten übereinstimmt. Durch diese Bedingung wird der Knoten **Anything else** ausgelöst. |
| `conversation_start` | Wie **welcome** wird diese Bedingung während der ersten Dialogmodulphase mit 'true' ausgewertet. Anders als **welcome** wird sie unabhängig davon mit 'true' ausgewertet, ob die erstmalige Anforderung aus der Anwendung eine Benutzereingabe enthält oder nicht. Ein Knoten mit der Bedingung **conversation_start** kann verwendet werden, um am Anfang des Dialogmoduls Kontextvariablen zu initialisieren oder andere Tasks auszuführen. |
| `false`              | Diese Bedingung wird immer mit 'false' ausgewertet. Sie können sie am Anfang einer Verzweigung verwenden, die sich noch in der Entwicklung befindet, um ihre Verwendung zu verhindern, oder als Bedingung für einen Knoten, der eine allgemeine Funktion bereitstellt und nur als Ziel einer Aktion **Springen zu** dient. |
| `irrelevant`         | Diese Bedingung wird mit 'true' ausgewertet, wenn die Eingabe des Benutzers vom Service '{{site.data.keyword.conversationshort}}' als irrelevant eingestuft wird. |
| `true`               | Diese Bedingung wird immer mit 'true' ausgewertet. Sie können sie am Ende einer Liste von Knoten oder Antworten verwenden, um alle Antworten abzufangen, die keiner der vorherigen Bedingungen entsprechen. |
| `welcome`            | Diese Bedingung wird in der ersten Runde des Dialogmoduls (am Anfang des Dialogs) und nur dann mit 'true' ausgewertet, wenn die erstmalige Anforderung von der Anwendung keine Benutzereingabe enthält. Bei allen nachfolgenden Dialogmodulrunden wird sie mit 'false' ausgewertet. Der Knoten **Welcome** wird durch diese Bedingung ausgelöst. Ein Knoten mit dieser Bedingung dient normalerweise dazu, den Benutzer zu begrüßen. Beispielsweise durch Ausgeben einer Nachricht wie `Willkommen bei unserer Pizza-Bestellapp`. Dieser Knoten wird nicht im Rahmen von Transaktionen verarbeitet, die über Kanäle wie Facebook oder Slack aufgerufen werden.|
{: caption="Sonderbedingungen" caption-side="top"}

### Details zur Bedingungssyntax
{: #dialog-overview-condition-syntax}

Verwenden Sie eine der folgenden Syntaxoptionen, um gültige Ausdrücke in Bedingungen zu erstellen:

- Verwenden Sie Kurzbefehle, um Absichten, Entitäten und Kontextvariablen zu referenzieren. Weitere Informationen finden Sie unter [Auf Objekte zugreifen und Objekte auswerten](/docs/services/assistant?topic=assistant-expression-language).

- Spring Expression Language (SpEL): Diese Ausdruckssprache unterstützt das Abfragen und Bearbeiten eines Objektdiagramms während der Laufzeit. Weitere Informationen finden Sie auf der Seite zu [Spring Expression Language (SpEL) ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](http://docs.spring.io/spring/docs/current/spring-framework-reference/html/expressions.html){: new_window}.

Sie können reguläre Ausdrücke verwenden, um Werte für Bedingungen zu überprüfen.  Verwenden Sie beispielsweise die Methode `String.find`, um eine übereinstimmende Zeichenfolge zu suchen. Weitere Informationen enthält der Abschnitt [Methoden](/docs/services/assistant?topic=assistant-dialog-methods).

## Antworten
{: #dialog-overview-responses}

Die Dialogmodulantwort definiert, wie dem Benutzer geantwortet wird.

Sie können mit den folgenden Antworttypen antworten: 

- [Einfache Textantwort](#dialog-overview-simple-text)
- [Erweiterte Antwort](#dialog-overview-multimedia)
- [Bedingte Antwort](#dialog-overview-multiple)

### Einfache Textantwort
{: #dialog-overview-simple-text}

Wenn Sie eine Textantwort angeben wollen, geben Sie einfach den Text ein, den der Service für den Benutzer anzeigen soll.

![Zeigt einen Knoten mit der Benutzerfrage 'Wo ist Ihr Standort?' und der Dialogmodulantwort 'Wir haben keine herkömmlichen Filialen. Aber mit einer Internetverbindung können Sie überall bei uns einkaufen!'](images/response-simple.png)

Wenn Sie einen Kontextvariablenwert in die Antwort einbeziehen möchten, geben Sie ihn mit der Syntax `$variablenname` an. Weitere Informationen finden Sie unter [Kontextvariablen](/docs/services/assistant?topic=assistant-dialog-runtime#dialog-runtime-context). Wenn Ihnen zum Beispiel bekannt ist, dass die Kontextvariable '$user' vor der Verarbeitung eines Knotens auf den Namen des aktuellen Benutzers gesetzt wird, können Sie in der Textantwort des Knotens wie folgt darauf verweisen:

```
Hello $user
```
{: screen}

Wenn der Name des aktuellen Benutzers `Norman` lautet, wird die folgende Antwort für den Benutzer angezeigt: `Hello Norman`.

Wenn die Textantwort eines der folgenden Sonderzeichen enthält, geben Sie vor dem Sonderzeichen einen umgekehrten Schrägstrich als Escapezeichen an (``\`). Wenn Sie den JSON-Editor verwenden, müssen Sie zwei umgekehrte Schrägstriche als Escapezeichen  (``\\`) angeben. Durch das Einfügen von Escapezeichen wird verhindert, dass der Service das Sonderzeichen irrtümlich als einen der folgenden Artefakttypen interpretiert: 

| Sonderzeichen | Artefakt | Beispiel |
|-------------------|----------|---------|
| `$` | Kontextvariable | `Die Transaktionsgebühr beträgt \$2.` |
| `@` | Entität | `Senden Sie Ihr Feedback an feedback\@example.com.` |
{: caption="Sonderzeichen in Antworten, für die Escapezeichen angegeben werden müssen" caption-side="top"}

Die Standardintegrationen unterstützen die folgenden Elemente der Markdown-Syntax: 

| Format | Syntax | Beispiel |
|------------|--------|---------|
| Kursivschrift | `Wir sprechen hier über die *Praxis*.` | Wir sprechen hier über die *Praxis*. |
| Fettschrift | `Beim Baseball gibt es **kein** Geheule.` | Beim Baseball gibt es **kein** Geheule. |
| Hypertext-Link | `Kontaktieren Sie uns unter [ibm.com](https://www.ibm.com).` | Kontaktieren Sie uns unter [ibm.com ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://www.ibm.com). |
{: caption="Unterstützte Markdown-Syntax" caption-side="top"}

In der Anzeige 'Ausprobieren' wird die Markdown-Syntax derzeit nicht unterstützt. Zum Einfügen eines Links, der nur in der Anzeige 'Ausprobieren' und in der Vorschaulink-Integration dargestellt wird, können Sie die HTML-Syntax verwenden. Beispiel: `Kontaktieren Sie uns unter <a href="https://www.ibm.com">ibm.com</a>.` (Versuchen Sie beispielsweise *nicht*, für das Anführungszeichen einen umgekehrten Schrägstrich `\"` als Escapezeichen anzugeben.) Die HTML-Syntax wird jedoch von keiner der Standardintegrationen unterstützt.
{: note}

#### Weitere Informationen zu einfachen Antworten
{: #dialog-overview-variety}

- [Mehrere Zeilen hinzufügen](#dialog-overview-multiline)
- [Varianten hinzufügen](#dialog-overview-add-variety)

#### Mehrere Zeilen hinzufügen
{: #dialog-overview-multiline}

Wenn eine einzelne Textantwort aus mehreren Zeilen bestehen soll, die durch Wagenrücklaufzechen getrennt werden, führen Sie die folgenden Schritte aus: 

1.  Fügen Sie jede Zeile, die dem Benutzer als separater Satz angezeigt werden soll, in ein eigenes Antwortvariantenfeld ein. Beispiel:

  <table>
  <caption>Mehrzeilige Antwort</caption>
  <tr>
    <th>Antwortvarianten</th>
  </tr>
  <tr>
    <td>Hallo.</td>
  </tr>
  <tr>
    <td>Wie geht es Ihnen heute?</td>
  </tr>
  </table>

1.  Wählen Sie als Antwortvarianteneinstellung die Option **multiline** aus.

    Wenn Sie einen Dialogskill verwenden, der erstellt wurde, bevor die Unterstützung für erweiterten Antworttypen im Service hinzugefügt wurde, wird die Option *multiline* möglicherweise nicht angezeigt. Fügen Sie einen zweiten Textantworttyp zur aktuellen Knotenantwort hinzu. Durch diese Aktion wird die Darstellung der Antwort im zugrunde liegenden JSON-Code geändert. Dadurch wird die Option 'multiline' verfügbar. Wählen Sie den mehrzeiligen Variantentyp aus. Jetzt können Sie den zweiten Textantworttyp löschen, den Sie zu der Antwort hinzugefügt haben.{: note}

Wenn die Antwort dem Benutzer angezeigt wird, wird jede der beiden Antwortvarianten in einer separaten Zeile angezeigt, wie nachfolgend dargestellt: 

```
Hallo.
Wie geht es Ihnen heute?
```
{: screen}

#### Varianten hinzufügen
{: #dialog-overview-add-variety}

Wenn Benutzer Ihren Dialogservice häufig nutzen, ist es für sie möglicherweise langweilig, immer dieselben Begrüßungen und Antworten zu erhalten.  Durch das Hinzufügen von *Varianten* zu Ihren Antworten können Sie im Dialog dieselbe Bedingung unterschiedlich beantworten lassen.

Im folgenden Beispiel unterscheiden sich die Antworten, die der Service auf Fragen zu Filialstandorten gibt, von einer Interaktion zur nächsten Interaktion:

![Zeigt einen Knoten mit der Benutzerfrage 'Wo ist Ihr Standort?' und drei unterschiedlichen Antworten des Dialogmoduls](images/variety.png)

Sie können auswählen, ob die Antwortvarianten nacheinander oder ein einer Zufallsreihenfolge verwendet werden sollen. Standardmäßig werden Antworten turnusmäßig nacheinander verwendet, so als ob sie aus einer sortierten Liste ausgewählt würden.

Um die Reihenfolge zu ändern, in der die einzelnen Textantworten zurückgegeben werden, führen Sie die folgenden Schritte aus:

1.  Fügen Sie jede Variante der Antwort in ein eigenes Antwortvariantenfeld ein. Beispiel:

  <table>
  <caption>Antworten variieren</caption>
  <tr>
    <th>Antwortvarianten</th>
  </tr>
  <tr>
    <td>Hallo.</td>
  </tr>
  <tr>
    <td>Hi.</td>
  </tr>
  <tr>
    <td>Wie geht's?</td>
  </tr>
  </table>

1.  Wählen Sie eine der folgenden Einstellungen für die Antwortvariante aus: 

    - **sequential**: Das System gibt beim ersten Auslösen des Dialogmodulknotens die erste Antwortvariante zurück, beim zweiten Auslösen des Knotens die zweite Antwortvariante usw. (in der Reihenfolge der Varianten, die Sie im Knoten definiert haben).

      Die Antworten werden in der folgenden Reihenfolge zurückgegeben, wenn der Knoten verarbeitet wird: 

      - Beim ersten Mal:

        ```
        Hallo.
        ```
        {: screen}

      - Beim zweiten Mal:

        ```
        Hi.
        ```
        {: screen}

      - Beim dritten Mal:
        ```
        Wie geht's?
        ```
        {: screen}

    - **random**: Das System gibt beim ersten Auslösen des Dialogmodulknotens eine nach dem Zufallsprinzip ausgewählte Textzeichenfolge aus der Liste der Varianten zurück, beim zweiten Mal eine andere zufällig ausgewählte Variante, jedoch ohne dieselbe Textzeichenfolge fortlaufend zu wiederholen. 

      Beispiel für mögliche Rückgabereihenfolge der Antworten beim Verarbeiten des Knotens: 

      - Beim ersten Mal:

        ```
        Wie geht's?
        ```
        {: screen}

      - Beim zweiten Mal:

        ```
        Hi.
        ```
        {: screen}

      - Beim dritten Mal:

        ```
        Hallo.
        ```
        {: screen}

### Erweiterte Antworten
{: #dialog-overview-multimedia}

Sie können Antworten mit Multimediaelementen oder interaktiven Elementen wie Bildern oder per Mausklick steuerbaren Schaltflächen zurückgeben, um das Interaktionsmodell Ihrer Anwendung zu vereinfachen und das Benutzererlebnis zu verbessern. 

Neben dem Standardantworttyp **Text**, bei dem Sie angeben, welcher Text als Antwort an den Benutzer zurückgegeben werden soll, werden die folgenden Antworttypen unterstützt: 

- **Servicemitarbeiter kontaktieren**: ![Nur Plus-Plan oder Premium-Plan](images/premium.png) Das Dialogmodul ruft einen von Ihnen angegebenen Service auf (in der Regel ein Service zum Verwalten von Warteschlangen mit Support-Tickets für Servicemitarbeiter), um den Dialog an eine Person zu übergeben. Sie können optional eine Nachricht einfügen, die das vom Benutzer erkannte Problem zusammenfasst, das an den Servicemitarbeiter übergeben werden soll. Der externe Service ist dafür verantwortlich, eine Nachricht auszugeben, die dem Benutzer angezeigt wird und die erläutert, dass der Dialog übertragen wird. Der Nachrichtenaustausch wird nicht vom Dialogmodul selbst verwaltet. Die Dialogübertragung wird nicht ausgeführt, während Sie Knoten mit diesem Antworttyp in der Anzeige 'Ausprobieren' testen. Sie müssen über eine Testbereitstellung auf einen Knoten zugreifen, der diesen Antworttyp verwendet, um zu prüfen, wie er von Ihren Benutzern erlebt wird. 

  Dieser Antworttyp ist nur in Serviceinstanzen für einen Plus- oder Premium-Plan sichtbar, und er wird nur für Intercom-Integrationen oder angepasste Anwendungsintegrationen unterstützt. {: note}

- **Bild**: Integriert ein Bild in die Antwort. Die Quellenbilddatei muss an beliebiger Stelle gehostet werden und über eine URL verfügen, über die sie von Ihnen referenziert werden kann. Die Datei muss in einem Verzeichnis gespeichert sein, das öffentlich zugänglich ist. 
- **Option**: Fügt eine Liste mit mindestens einer Option hinzu. Wenn eine Benutzer auf eine der Optionen klickt, wird ein zugehöriger Benutzereingabewert zum Service gesendet. Die Darstellung der Optionen kann je nach Bereitstellungsort des Dialogmoduls variieren. Die Optionen können beispielsweise in einem Integrationskanal als per Mausklick steuerbare Schaltflächen angezeigt werden und in einem anderen als Dropdown-Liste. 
- **Pause**: Weist die Anwendung an, eine angegebene Anzahl von Millisekunden zu warten, bevor die Verarbeitung fortgesetzt wird. Wahlweise kann durch einen Indikator angezeigt werden, dass vom Dialogmodul gerade eine Antwort verfasst wird. Verwenden Sie diesen Antworttyp, wenn die auszuführende Aktion eine Weile dauern kann. Beispiel: Ein übergeordneter Knoten führt einen Cloud Function-Aufruf aus und zeigt das Ergebnis in einem untergeordneten Knoten an. Diesen Antworttyp können Sie als Antwort für den übergeordneten Knoten verwenden, damit der programmgesteuerte Aufruf abgeschlossen werden kann, bevor das Ergebnis im untergeordneten Knoten angezeigt wird. Dieser Antworttyp kann nicht in der Anzeige 'Ausprobieren' dargestellt werden. Sie müssen über eine Testbereitstellung auf einen Knoten zugreifen, der diesen Antworttyp verwendet, um zu prüfen, wie er von Ihren Benutzern erlebt wird. 

#### Erweiterte Antworten hinzufügen
{: #dialog-overview-multimedia-add}

So fügen Sie eine erweiterte Antwort hinzu: 

1.  Klicken Sie auf das Dropdown-Menü im Antwortfeld, um einen Antworttyp auszuwählen, und geben Sie anschließend die erforderlichen Informationen an:

    - **Servicemitarbeiter kontaktieren**: ![Nur Plus-Plan oder Premium-Plan](images/premium.png) Sie können optional eine Nachricht für den Servicemitarbeiter hinzufügen, an den der Dialog übergeben werden soll. 

        Dieser Antworttyp wird nur für Intercom-Integrationen und angepasste Anwendungsintegrationen unterstützt. Bei angepassten Anwendungen müssen Sie die Clientanwendung so programmieren, dass sie erkennen kann, wenn dieser Antworttyp ausgelöst wird. {: note}

    - **Bild**: Fügen Sie die vollständige URL zu der gehosteten Bilddatei in das Feld **Bildquelle** ein. Das Bild muss im Dateiformat .jpg, .gif oder .png vorliegen. Die Bilddatei muss an einer Position gespeichert sein, die über eine URL öffentlich zugänglich ist. 

        Beispiel: `https://www.example.com/assets/common/logo.png`.

        Wenn ein Titel und eine Beschreibung über dem eingebetteten Bild in der Antwort angezeigt werden soll, dann fügen Sie diese Angaben in den entsprechenden Feldern hinzu. 

        Für Slack-Integrationen ist ein Titel erforderlich. In anderen Integrationskanälen werden Titel oder Beschreibungen ignoriert.
        {: note}

    - **Option**: Führen Sie die folgenden Schritte aus: 

      1.  Klicken Sie auf **Option hinzufügen**.
      1.  Geben Sie in das Feld **Listenbezeichnung** ein, welche Option in der Liste angezeigt werden soll. Die Bezeichnung muss weniger als 64 Zeichen lang sein. 
      1.  Geben Sie in das zugehörige Feld **Wert** die Benutzereingabe ein, die an den Service übergeben werden soll, wenn diese Option ausgewählt wird. Der Wert muss weniger als 2.048 Zeichen lang sein. (Die derzeit bestehende Begrenzung auf 64 Zeichen wird aufgehoben.)

          Geben Sie einen Wert ein, von dem Sie wissen, dass er die richtige Absicht auslöst, wenn er zur Verarbeitung übergeben wird. Dies kann zum Beispiel ein Benutzerbeispiel aus den Trainingsdaten für die Absicht sein. 
      1.  Wiederholen Sie die vorherigen Schritte, um weitere Optionen in der Liste hinzuzufügen.
      1.  Fügen Sie eine kurze Erläuterung zur Verwendung der Liste im Feld **Titel** hinzu. Der Titel kann den Benutzer auffordern, einen Eintrag in der Liste der Optionen auszuwählen. 

          In manchen Integrationskanälen wird der Titel nicht angezeigt.
          {: note}

      1.  (Optional) Fügen Sie weitere Informationen im Feld **Beschreibung** hinzu. Der Inhalt dieses Felds wird nach dem Titel und vor der Liste der Optionen angezeigt. 

      In manchen Integrationskanälen wird die Beschreibung nicht angezeigt.
      {: note}

      Sie können beispielsweise eine Antwort wie die folgende erstellen:

        <table>
        <caption>Antwortoptionen</caption>
        <tr>
          <th>Titel der Liste</th>
          <th>Beschreibung der Liste</th>
          <th>Bezeichnung der Option</th>
          <th>Nach dem Klicken übergebene Benutzereingabe</th>
        </tr>
        <tr>
          <td>Versicherungsarten</td>
          <td>Welches der folgenden Objekte möchten Sie versichern?</td>
          <td></td>
          <td></td>
        </tr>
        <tr>
          <td></td>
          <td></td>
          <td>Boot</td>
          <td>Ich möchte eine Bootversicherung abschließen</td>
        </tr>
        <tr>
          <td></td>
          <td></td>
          <td>Auto</td>
          <td>Ich möchte eine Autoversicherung abschließen</td>
        </tr>
         <tr>
          <td></td>
          <td></td>
          <td>Haus</td>
          <td>Ich möchte eine Hausversicherung abschließen</td>
        </tr>
        </table>

    - **Pause**: Geben Sie die Dauer der Wartezeit in Millisekunden (ms) in das Feld **Dauer** ein.

        Der Wert darf nicht größer als 10.000 ms sein. Benutzer akzeptieren in der Regel eine Wartezeit von etwa 8 Sekunden (8.000 ms), bis eine Antwort eingegeben wird. Wenn während der Pause kein Eingabeindikator angezeigt werden soll, geben Sie die Einstellung **Aus** an.

        Fügen Sie nach der Pause einen weiteren Antworttyp (z. B. eine Textantwort) hinzu, um deutlich zu machen, dass die Wartezeit vorüber ist.{: tip}

    - **Text**: Fügen Sie im Textfeld den Text hinzu, der an den Benutzer zurückgegeben werden soll. Wählen Sie optional eine Varianteneinstellung für die Textantwort aus. Weitere Details enthält der Abschnitt [Einfache Textantwort](#dialog-overview-simple-text).

1.  Klicken Sie auf **Antwort hinzufügen**, um einen weiteren Antworttyp zur aktuellen Antwort hinzuzufügen.

    Es kann sinnvoll sein, mehrere Antworttypen in einer einzelnen Antwort hinzuzufügen, um eine Benutzeranfrage gezielter zu beantworten. Wenn ein Benutzer beispielsweise nach Geschäftsstandorten fragt, kann eine Landkarte mit Schaltflächen für die einzelnen Filialen angezeigt werden, auf  die der Benutzer klicken kann, um die zugehörigen Adressdetails abzurufen. Eine solche Antwort können Sie mit einer Kombination aus den Antworttypen 'Bild', 'Option' und 'Text' erstellen. Ein anderes Beispiel ist die Verwendung eines Antworttyps 'Text' vor einem Antworttyp 'Pause', um Benutzer über die bevorstehende Unterbrechung des Dialogs zu informieren. 

    Sie können höchstens fünf Antworttypen in einer einzelnen Antwort hinzufügen. Mit anderen Worten: Wenn Sie für einen Dialogmodulknoten drei bedingte Antworten definieren, können jeder bedingten Antwort maximal fünf Antworttypen zugeordnet werden.{: note}

    Ein einzelner Dialogmodulknoten darf nicht mehr als einen Antworttyp **Servicemitarbeiter kontaktieren** enthalten.
    {: note}

1.  Wenn Sie mehr als einen Antworttyp hinzugefügt haben, können Sie auf den **Aufwärts- oder Abwärtspfeil** klicken, um die Antworttypen in die gewünschte Reihenfolge zu bringen, in der Sie vom Service verarbeitet werden sollen.

### Bedingte Antworten
{: #dialog-overview-multiple}

Ein einzelner Dialogmodulknoten kann unterschiedliche Antworten bereitstellen, die jeweils durch eine andere Bedingung ausgelöst werden.  Verwenden Sie diese Lösung, wenn Sie mehrere Szenarios in einem einzigen Knoten abdecken wollen.

<iframe class="embed-responsive-item" id="youtubeplayer1" title="Bedingte Antworten hinzufügen" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/Q5_-f7_Iyvg?rel=0" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

Der Knoten besitzt weiterhin eine Hauptbedingung, also die Bedingung für die Verwendung des Knotens und die Verarbeitung der Bedingungen und Antworten, die er enthält.

Im folgenden Beispiel verwendet der Service Informationen, die zuvor über den Standort des Benutzers erfasst wurden, um die Antwort anzupassen und Informationen zu der Filiale bereitzustellen, die für den Benutzer am nächsten liegt. Weitere Informationen zum Speichern von Informationen, die vom Benutzer erfasst wurden, finden Sie unter [Kontextvariablen](/docs/services/assistant?topic=assistant-dialog-runtime#dialog-runtime-context).

![Zeigt einen Knoten mit der Benutzerfrage 'Wo ist Ihr Standort?' und drei unterschiedlichen Antworten des Dialogmoduls, die von Bedingungen abhängig sind, die Daten aus der Kontextvariablen '$state' verwenden, um Standorte in den entsprechenden Bundesstaaten anzugeben](images/multiple-responses.png)

Dieser einzelne Knoten bietet jetzt die äquivalente Funktion von vier separaten Knoten.

So fügen Sie bedingte Antworten in einem Knoten hinzu: 

1.  Klicken Sie auf **Anpassen** und setzen Sie das Umschaltsteuerelement **Mehrere Antworten** auf **Ein**.

    Im Abschnitt für die Knotenantwort wird ein Paar mit Bedingungs- und Antwortfeld angezeigt. In diesen Feldern können Sie eine Bedingung und eine Antwort hinzufügen.
1.  Um eine angepasste Antwort weiter zu differenzieren, klicken Sie auf das Symbol **Antwort bearbeiten** ![Antwort bearbeiten](images/edit-slot.png) neben der Antwort.

    Sie müssen die Antwort zum Bearbeiten öffnen, um die folgenden Tasks auszuführen: 

    - **Kontext aktualisieren**: Um den Wert einer Kontextvariablen zu ändern, wenn die Antwort ausgelöst wird, geben Sie den Kontextwert im Kontexteditor an. Der Kontext für jede einzelne bedingte Antwort muss separat aktualisiert werden. Es gibt keinen allgemeinen Kontexteditor oder JSON-Editor für alle bedingten Antworten. 
    - **Erweiterte Antworten hinzufügen**: Um mehr als eine Textantwort oder andere Antworttypen als den Typ 'Text' in einer einzelnen bedingten Antwort hinzuzufügen, müssen Sie die Antwort in der Bearbeitungsansicht öffnen. 
    - **Sprung konfigurieren**: Um den Service anzuweisen, zu einem anderen Knoten zu springen, nachdem die aktuelle bedingte Antwort verarbeitet wurde, wählen Sie **Springen zu** im Abschnitt *Abschließend* für die Antwort in der Bearbeitungsansicht aus. Geben Sie an, welchen Knoten der Service als nächstes bearbeiten soll. Weitere Informationen enthält der Abschnitt [Aktion 'Springen zu' konfigurieren](#dialog-overview-jump-to-config).

      Eine für den Knoten konfigurierte Aktion **Springen zu** wird erst ausgeführt, nachdem alle bedingten Antworten verarbeitet wurden. Mit anderen Worten: Wenn eine bedingte Antwort so konfiguriert ist, dass zu einem anderen Knoten gesprungen wird, und die bedingte Antwort ausgelöst wird, wird die für den Knoten konfigurierte Sprungaktion niemals verarbeitet und findet somit nicht statt. 

1.  Klicken Sie auf **Antwort hinzufügen**, um eine weitere bedingte Antwort hinzuzufügen. 

Die Bedingungen werden, ebenso wie Knoten, nacheinander ausgewertet. Stellen Sie sicher, dass Ihre bedingten Antworten in der richtigen Reihenfolge aufgelistet sind. Falls Sie die Reihenfolge ändern müssen, wählen Sie ein Bedingung/Antwort-Paar aus und verschieben Sie es mithilfe der angezeigten Aufwärts- und Abwärtspfeile an die gewünschte Position in der Liste.

## Nächste Schritte definieren
{: #dialog-overview-jump-to}

Nachdem die angegebene Antwort ausgegeben wurde, können Sie den Service anweisen, eine der folgenden Aktionen auszuführen:

- **Auf Benutzereingabe warten**: Der Service wartet, bis der Benutzer als Reaktion auf die Antwort eine neue Eingabe bereitstellt. Beispielsweise kann die Antwort eine Frage enthalten, die der Benutzer mit 'Ja' oder 'Nein' beantworten muss. Das Dialogmodul wird erst dann fortgesetzt, wenn der Benutzer eine weitere Eingabe bereitgestellt hat.
- **Benutzereingabe überspringen**: Verwenden Sie diese Option, wenn Sie das Warten auf eine Benutzereingabe umgehen und stattdessen direkt mit dem ersten untergeordneten Knoten des aktuellen Dialogmodulknotens fortfahren möchten.

  Der aktuelle Knoten muss über mindestens einen untergeordneten Knoten verfügen, damit diese Option zu Verfügung steht. {: note}

- **Zu anderem Dialogmodulknoten springen**: Verwenden Sie diese Option, wenn der Dialog direkt zu einem ganz anderen Dialogmodulknoten übergehen soll. Mit einer Aktion *Springen zu* können Sie den Ablauf von mehreren Positionen in der Baumstruktur beispielsweise zu einem allgemeinen Dialogmodulknoten weiterleiten.

  Der Zielknoten der Sprungaktion muss vorhanden sein, damit er in einer Aktion 'Springen zu' als Ziel definiert werden kann.
  {: note}

### Aktion 'Springen zu' konfigurieren
{: #dialog-overview-jump-to-config}

Wenn Sie auswählen, dass zu einem anderen Knoten gesprungen werden soll, geben Sie an, wann der Zielknoten verarbeitet werden soll, indem Sie eine der folgenden Optionen auswählen: 

- **Bedingung**: Falls die Anweisung auf den Bedingungsteil des ausgewählten Dialogmodulknotens abzielt, überprüft der Service zunächst, ob die Bedingung des Zielknotens mit 'true' ausgewertet wird.
    - Wird die Bedingung mit 'true' ausgewertet, verarbeitet das System diesen Knoten sofort.
    - Wird die Bedingung nicht mit 'true' ausgewertet, setzt das System den Auswertungsprozess bei einer Bedingung des nächsten gleichgeordneten Knotens des Zielknotens fort und wiederholt diesen Prozess, bis ein Dialogmodulknoten mit einer Bedingung gefunden wird, die mit 'true' ausgewertet wird.

    - Wenn das System alle gleichgeordneten Knoten verarbeitet hat und keine Bedingung mit 'true' ausgewertet wird, kommt die  Basisstrategie für Rückübertragung zum Einsatz und das Dialogmodul wertet die Knoten auf der Basisebene der Baumstruktur des Dialogmoduls aus.

    Die Verwendung der Bedingung als Ziel ist von Nutzen, wenn die Bedingungen von Dialogmodulknoten verkettet werden sollen. Beispielsweise kann es sein, dass zuerst überprüft werden soll, ob die Eingabe eine Absicht enthält (z. B. `#einschalten`), und in diesem Fall dann überprüft werden soll, ob die Eingabe Entitäten enthält (z. B. `@scheinwerfer`), `@radio` oder `@scheibenwischer`). Die Verkettung von Bedingungen hilft Ihnen bei der Strukturierung von umfangreicheren Baumstrukturen in Dialogmodulen.

    Vermeiden Sie das Auswählen dieser Option beim Konfigurieren einer Aktion 'Springen zu' von einer bedingten Antwort zu einem übergeordneten Knoten des aktuellen Knotens in der Baumstruktur des Dialogmoduls. Andernfalls kann eine Endlosschleife entstehen. Wenn der Service zu dem übergeordneten Knoten springt und die zugehörige Bedingung prüft, wird diese möglicherweise als falsch eingestuft, da dieselbe Benutzereingabe ausgewertet wird, von der der aktuelle Knoten zuletzt über das Dialogmodul ausgelöst wurde. Der Service fährt mit dem nächsten gleichgeordneten Element fort oder springt zurück zum Stammelement und prüft die Bedingungen dieser Knoten. Dies führt häufig dazu, dass derselbe Knoten erneut ausgelöst wird, d. h. der Prozess wiederholt sich immer wieder. {: note}

- **Antwort**: Falls die Anweisung auf den Antwortteil des ausgewählten Dialogmodulknotens abzielt, wird sie sofort ausgeführt. Dies bedeutet, dass das System den Bedingungsteil des ausgewählten Dialogmodulknotens nicht auswertet und den Antwortteil des ausgewählten Dialogmodulknotens unmittelbar ausführt.

  Die Verwendung der Antwort als Ziel ist hilfreich, wenn mehrere Dialogmodulknoten verkettet werden sollen. Die Antwort wird so verarbeitet, als ob die Bedingung dieses Dialogmodulknotens mit 'true' ausgewertet würde. Falls der ausgewählte Dialogmodulknoten eine weitere Aktion **Springen zu** enthält, wird diese Aktion ebenfalls sofort ausgeführt.

- **Auf Benutzereingabe warten**: Es wird auf eine neue Eingabe des Benutzers gewartet. Die Verarbeitung dieser neuen Eingabe beginnt ab dem Zielknoten der Sprungaktion. Diese Option ist hilfreich, wenn der Quellenknoten beispielsweise eine Frage stellt und die Verarbeitung der Benutzerantwort auf die Frage mit einem anderen Knoten beginnen soll. 

## Weitere Informationen

Informationen zu der vom Dialogmodul verwendeten Ausdruckssprache sowie zu Methoden, Systementitäten und anderen nützlichen Details finden Sie im Abschnitt **Referenzinformationen** im Navigationsbereich.

Sie können auch die API verwenden, um Knoten zu bearbeiten oder andere Beareitungsvorgänge an einem Dialogmodul vorzunehmen. Weitere Informationen enthält der Abschnitt [Dialogmodul mithilfe der API ändern](/docs/services/assistant?topic=assistant-api-dialog-modify).
