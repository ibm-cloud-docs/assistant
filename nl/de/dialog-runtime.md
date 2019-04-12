---

copyright:
  years: 2015, 2019
lastupdated: "2018-02-21"

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

# Informationen zur Verarbeitung des Dialogmoduls
{: #dialog-runtime}

Machen Sie sich damit vertraut, wie Ihr Dialogmodul verarbeitet wird, wenn eine Person während der Laufzeit mit Ihrer Instanz des bereitgestellten Service '{{site.data.keyword.conversationshort}}' interagiert.
{: shortdesc}

## Ablauf eines Dialogmodulaufrufs
{: #dialog-runtime-message-anatomy}

Jede Äußerung eines Benutzers wird als API-Aufruf '/message' an das Dialogmodul übergeben. Dazu gehören auch Benutzeräußerungen, die auf Dialogmodulanfragen zum Eingeben von Informationen antworten. Da manche Abonnementpläne eine festgelegte Anzahl von API-Aufrufen beinhalten, sollten Sie wissen, woraus ein Aufruf besteht. Ein einzelner API-Aufruf '/message' entspricht einer einzelnen Dialogmodulrunde, die aus einer Eingabe des Benutzers und der zugehörigen Antwort des Dialogmoduls besteht.

Der Hauptteil des API-Aufrufs '/message' mit zugehöriger Anforderung und Antwort enthält die folgenden Objekte:

- `context`: Enthält die Variablen, die als persistent definiert werden sollen. Um Informationen von einem Aufruf an den nächsten zu übergeben, muss der Anwendungsentwickler den Antwortkontext des vorherigen API-Aufrufs zusammen mit dem jeweils nachfolgenden API-Aufruf übergeben. Das Dialogmodul kann zum Beispiel den Benutzernamen erfassen und in den nachfolgenden Knoten über diesen Namen auf den Benutzer verweisen.

  ```json
  {
    "context" : {
      "user_name" : "<? @sys-person.literal ?>"
    }
  ```
  {: codeblock}

  Weitere Informationen dazu enthält der Abschnitt [Informationen in mehreren Dialogrunden beibehalten](#dialog-runtime-context).

- `input`: Die vom Benutzer übermittelte Textzeichenfolge. Die Textzeichenfolge kann maximal 2.048 Zeichen umfassen.

  ```json
  {
    "input": {
      "text" : "Welche Filiale liegt in Ihrer Nähe?"
    }
  ```
  {: codeblock}

- `output`: Die Antwort des Dialogmoduls, die an den Benutzer zurückgegeben werden soll.

  ```json
  {
  "output":{
    "generic":[
      {
        "values": [
          {
            "text": "Dies ist mein Antworttext."
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

In der resultierenden Antwort auf den API-Aufruf '/message' ist die Textantwort wie folgt formatiert: 

```json
{
   "text": "Dies ist mein Antworttext.",
   "response_type": "text"
}
```

Das folgende Format für das Objekt `output` wird aus Gründen der Abwärtskompatibilität unterstützt. Alle Arbeitsbereiche, die eine Textantwort in diesem Format angeben, funktionieren weiterhin ordnungsgemäß. Bei der Einführung der erweiterten Antworttypen wurde die Struktur `output.text` durch die Struktur `output.generic` erweitert, um die Unterstützung weiterer Antworttypen neben dem Typ 'Text' zu erleichtern. Die Verwendung des neuen Formats beim Erstellen neuer Knoten bietet größere Flexibilität, da Sie den Antworttyp bei Bedarf später ändern können.
{: note}

  ```json
  {
  "output":{
    "text": {
      "values": [
        "Dies ist mein Antworttext."
      ]
    }
  }
  ```
  {: codeblock}

Außer dem Antworttyp 'Text' können Sie noch weitere Antworttypen definieren. Weitere Details enthält der Abschnitt [Antworten](/docs/services/assistant?topic=assistant-dialog-overview#dialog-overview-responses).

Weitere Informationen zum API-Aufruf '/message' enthält die [API-Referenz ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://{DomainName}/apidocs/assistant-v2){: new_window}.

### Informationen in mehreren Dialogrunden beibehalten
{: #dialog-runtime-context}

Das Dialogmodul in einem Dialogskill ist statusunabhängig, d. h. es werden keine Informationen von einer Interaktion mit dem Benutzer bis zur nächsten aufbewahrt. Wenn Sie einen Dialogskill zu einem Assistenten hinzufügen und bereitstellen, speichert der Assistent den Kontext eines Nachrichtenaufrufs und übergibt ihn in jeder nachfolgenden Anforderung der aktuellen Sitzung erneut. Die aktuelle Sitzung bleibt bestehen, solange der Benutzer mit dem Assistenten interagiert, und danach während einer Inaktivitätszeit von maximal 60 Minuten für Plus- oder Permium-Pläne (5 Minuten für Lite-Pläne). Wenn Sie den Dialogskill nicht zu einem Assistenten hinzufügen, sind Sie als Entwickler der angepassten Anwendung für die Aufbewahrung aller Informationen verantwortlich, die von der Anwendung benötigt werden. Die Anwendung muss das Kontextobjekt in der Antwort der Nachrichten-API suchen, speichern und im Kontextobjekt zusammen mit der nächsten API-Anforderung '/message' übergeben, die im Rahmen des Dialogablaufs abgesetzt wird.

Eine Methode zum Aufbewahren der Informationen in eigener Regie ist das Speichern des vollständigen Kontextobjekts im Arbeitsspeicher der Clientanwendung (z. B. in einem Web-Browser). Wenn es sich um eine komplexe Anwendung handelt oder wenn die Anwendung personenbezogene Daten übergeben und speichern muss, können Sie die Informationen in einer Datenbank speichern bzw. abrufen. Die einfachste Methode ist jedoch diejenige, die Sie von der Verantwortung zum Speichern des Kontexts gänzlich befreit. Wenn Sie diesen Ansatz bevorzugen, fügen Sie den Dialogskill zu einem Assistenten hinzu und überlassen Sie dem Assistenten die Überwachung des Kontexts. 

Die Anwendung kann Informationen an das Dialogmodul übergeben, die vom Dialogmodul an die Anwendung zurückgegeben oder an den nächsten Knoten weitergegeben werden können. Für diesen Zweck werden im Dialogmodul *Kontextvariablen* verwendet.

## Kontextvariablen
{: #dialog-runtime-context-variables}

Eine Kontextvariable ist eine Variable, die Sie in einem Knoten definieren. Sie können einen Standardwert für die Variable angeben. Der Wert der Kontextvariablen kann anschließend durch andere Knoten, durch Anwendungslogik oder Benutzereingaben festgelegt oder geändert werden. 

Sie können für die Werte von Kontextvariablen Bedingungen festlegen, indem Sie eine Kontextvariable aus einer Bedingung eines Dialogmodulknotens heraus referenzieren, um festzulegen, ob ein Knoten ausgeführt wird. Außerdem können Sie eine Kontextvariable aus Antwortbedingungen eines Dialogmodulknotens heraus referenzieren, um abhängig von dem Wert, den ein externer Service oder ein Benutzer angegeben hat, unterschiedliche Antworten anzuzeigen. 

Weitere Informationen

- [Kontext aus der Anwendung heraus übergeben](#dialog-runtime-context-from-app)
- [Kontext von Knoten zu Knoten übergeben](#dialog-runtime-context-node-to-node)
- [Kontextvariable definieren](#dialog-runtime-context-var-define)
- [Häufige Aufgaben für Kontextvariablen](#dialog-runtime-context-common-tasks)
- [Kontextvariable löschen](#dialog-runtime-context-delete)
- [Kontextvariable aktualisieren](#dialog-runtime-context-update)
- [Verarbeitung von Kontextvariablen](#dialog-runtime-context-processing)
- [Reihenfolge der Ausführung](#dialog-runtime-context-order-of-ops)
- [Kontextvariablen zu einem Knoten mit Slots hinzufügen](#dialog-runtime-context-var-slots)

### Kontext aus der Anwendung heraus übergeben
{: #dialog-runtime-context-from-app}

Zum Übergeben von Informationen aus der Anwendung an das Dialogmodul legen Sie eine Kontextvariable fest und übergeben Sie die Kontextvariable an das Dialogmodul.

Beispiel: Ihre Anwendung kann eine Kontextvariable '$time_of_day' festlegen und an das Dialogmodul übergeben, das anhand der Informationen die Begrüßung anpassen kann, die für den Benutzer angezeigt wird.

![Zeigt einen Knoten 'Willkommen', der mittels Antwortbedingungen den Wert der Kontextvariablen '$time_of_day' überprüft, der aus der Anwendung an das Dialogmodul übergeben wird.](images/set-context.png)

In diesem Beispiel weiß der Dialogknoten, dass die Anwendung die Variable auf einen der Werte *morning*, *afternoon* oder *evening* festlegt. Er kann jeden Wert überprüfen und je nach dem vorhandenen Wert die passende Begrüßung zurückgeben. Falls die Variable nicht übergeben wurde oder einen Wert besitzt, der mit keinem der erwarteten Werte übereinstimmt, wird für den Benutzer eine allgemeinere Begrüßung angezeigt.

### Kontext von Knoten zu Knoten übergeben
{: #dialog-runtime-context-node-to-node}

Das Dialogmodul kann auch Kontextvariablen hinzufügen, um Informationen von einem Knoten zu einem anderen Knoten zu übergeben oder um die Werte von Kontextvariablen zu aktualisieren. Sobald das Dialogmodul Informationen vom Benutzer anfordert und erhält, können die Informationen protokolliert und später im Dialog referenziert werden.

Beispielsweise kann in einem Knoten der Benutzer nach seinem Namen gefragt werden und der Benutzer in einem späteren Knoten mit seinem Namen angesprochen werden.

![Zeigt einen einen Einführungsknoten, der einen Benutzer nach seinem Namen fragt und diesen als Kontextvariable speichert. Der nächste Knoten spricht den Benutzer unter Verwendung der Kontextvariablen '$username' mit seinem Namen an.](images/set-context-username.png)

In diesem Beispiel wird die Systementität '@sys-person' verwendet, um den Namen des Benutzers aus der Eingabe zu extrahieren, falls der Benutzer einen Namen angibt. Im JSON-Editor wird die Kontextvariable 'username' definiert und auf den Wert '@sys-person' gesetzt. In einem nachfolgenden Knoten wird die Kontextvariable '$username' in die Antwort einbezogen, um den Benutzer mit seinem Namen anzusprechen.

### Kontextvariable definieren
{: #dialog-runtime-context-var-define}

Definieren Sie eine Kontextvariable, indem Sie den Variablennamen im Feld **Variable** und einen Standardwert für die Variable im Feld **Wert** der Bearbeitungsansicht für den Knoten hinzufügen.

1.  Klicken Sie auf den Dialogmodulknoten, zu dem Sie eine Kontextvariable hinzufügen möchten, um ihn zu öffnen. 

1.  Klicken Sie auf das Symbol **Optionen**  ![Erweiterte Antwort](images/kabob.png), das der Knotenantwort zugeordnet ist, und klicken Sie anschließend auf **Kontexteditor öffnen**.

      ![Zeigt die Vorgehensweise zum Aufrufen des JSON-Editors für eine Standardknotenantwort.](images/contextvar-json-response.png)

      Wenn die Einstellung **Mehrere Antworten** für den Knoten auf **Ein** gesetzt ist, müssen Sie zuerst auf das Symbol **Antwort bearbeiten** ![Antwort bearbeiten](images/edit-slot.png) für die Antwort klicken, der Sie die Kontextvariable zuordnen möchten. 

      ![Zeigt die Vorgehensweise zum Aufrufen des JSON-Editors für einen Standardknoten, für den mehrere bedingte Antworten aktiviert sind.](images/contextvar-json-multi-response.png)

1.  Geben Sie das Name/Wert-Paar für die Variable in den Feldern **Variable** und **Wert** an.

    - Der `name` kann beliebige Groß- und Kleinbuchstaben, numerische Zeichen (0-9) und Unterstreichungszeichen enthalten.

      Sie können auch andere Zeichen (z. B. Punkte und Bindestriche) im Namen verwenden. In diesem Fall müssen Sie die Variable jedoch im nachfolgenden Code immer mit der Kurzformsyntax `$(variable-name)` referenzieren. Weitere Details enthält der Abschnitt [Ausdrücke für Objektzugriff](/docs/services/assistant?topic=assistant-expression-language#expression-language-shorthand-context).
      {:tip}

    - Der `wert` kann ein beliebiger unterstützter JSON-Typ sein (z. B. eine einfache Zeichenfolgevariable, eine Zahl, ein JSON-Array oder ein JSON-Objekt).

Die folgende Tabelle enthält Beispiele für das Definieren von Name/Wert-Paaren für verschiedene Werttypen:

| Variable       | Wert                         | Werttyp |
|:---------------|-------------------------------|------------|
| dessert        | "Kuchen"                        | Zeichenfolge  |
| age            | 18                            | Zahl     |
| toppings_array | ["Zwiebeln","Oliven"]            | JSON-Array |
| full_name      | {"first":"John","last":"Doe"} | JSON-Objekt |

Referenzieren Sie diese Konextvariablen anschließend mit der Syntax `$name`. Hierbei steht *name* für den Namen der von Ihnen definierten Kontextvariablen.

Sie können beispielsweise den folgenden Ausdruck als Dialogantwort angeben: 

`Der Kunde, der $age-year-old <? $full_name.first ?>, möchte eine Pizza mit <? $toppings_array.join(' and ') ?> und anschließend $dessert.`

Als Ergebnis wird die folgende Ausgabe angezeigt:

`Der Kunde, der 18 Jahre alte John, möchte eine Pizza mit Zwiebeln und Oliven und anschließend Kuchen.`

Zum Definieren von Kontextvariablen können Sie auch den JSON-Editor verwenden. Die Verwendung des JSON-Editors empfiehlt sich, wenn Sie einen komplexen Ausdruck als Variablenwert hinzufügen möchten. Weitere Details enthält der Abschnitt [Kontextvariablen im JSON-Editor](#dialog-runtime-context-var-json).

### Häufige Aufgaben für Kontextvariablen
{: #dialog-runtime-context-common-tasks}

Verwenden Sie `input.text`, um die gesamte Zeichenfolge zu speichern, die vom Benutzer eingegeben wurde:

| Variable | Wert            |
|----------|------------------|
| repeat   | `<?input.text?>` |

Angenommen, die Benutzereingabe lautet `Ich möchte ein Gerät bestellen.` Wenn die Knotenantwort `Sie sagten: $repeat` lautet, wird die folgende Antwort angezeigt: `Sie sagten: Ich möchte ein Gerät bestellen.`

Verwenden Sie die folgende Syntax, um den Wert einer Entität in einer Kontextvariablen zu speichern:

| Variable | Wert            |
|----------|------------------|
| place    | `@place`         |

Angenommen, die Benutzereingabe lautet `Ich möchte nach Paris reisen.` Wenn Ihre Entität '@place' den Ortsnamen `Paris` erkennt, speichert der Service `Paris` in der Kontextvariablen `$place`.

Um den Wert einer Zeichenfolge zu speichern, die Sie aus der Benutzereingabe extrahiert haben, können Sie einen SpEL-Ausdruck angeben, der mithilfe der Methode `extract` einen regulären Ausdruck auf die Benutzereingabe anwendet. Der folgende Ausdruck extrahiert eine Zahl aus der Benutzereingabe und speichert sie in der Kontextvariablen `$number`.

| Variable | Wert                               |
|----------|-------------------------------------|
| number   | `<?input.text.extract('[\d]+',0)?>` |

Um den Wert einer Musterentität zu speichern, fügen Sie '.literal' an den Entitätsnamen an. Die Verwendung dieser Syntax stellt sicher, dass der exakte Textbereich aus der Benutzereingabe, der dem angegebenen Muster entspricht, in der Variablen gespeichert wird.

| Variable | Wert                  |
|----------|------------------------|
| email    | `<? @email.literal ?>` |

Angenommen, die Benutzereingabe lautet `Kontaktieren Sie mich unter joe@example.com.` Ihre Entität mit dem Namen `@email` erkennt das E-Mail-Format `name@domain.com`. Indem Sie die Kontextvariable so konfigurieren, dass `@email.literal` gespeichert wird, geben Sie an, dass der Teil der Eingabe gespeichert werden soll, der mit dem Muster übereinstimmt. Wenn Sie die Eigenschaft `.literal` im Wertausdruck nicht angeben, wird der von Ihnen für das Muster angegebene Entitätswertname anstelle des mit dem Muster übereinstimmenden Teils der Benutzereingabe zurückgegeben.

Viele dieser Wertbeispiele verwenden Methoden zum Erfassen verschiedener Teile der Benutzereingabe. Weitere Informationen zu den verfügbaren Methoden, die Sie anwenden können, enthält der Abschnitt [Methoden in der Ausdruckssprache](/docs/services/assistant?topic=assistant-dialog-methods).

### Kontextvariable löschen
{: #dialog-runtime-context-delete}

Um eine Kontextvariable zu löschen, setzen Sie die Variable auf null.

| Variable   | Wert            |
|------------|------------------|
| order_form | `null`           |

Alternativ können Sie die Kontextvariable in Ihrer Anwendungslogik löschen. Informationen zum vollständigen Entfernen der Variablen enthält der Abschnitt [Kontextvariable in JSON löschen](#dialog-runtime-context-delete-json).

### Wert einer Kontextvariablen aktualisieren
{: #dialog-runtime-context-update}

Um den Wert einer Kontextvariablen zu aktualisieren, definieren Sie eine Kontextvariable mit dem gleichen Namen wie die vorherige Kontextvariable, jedoch mit einem anderen zugehörigen Wert. 

Wenn mehr als ein Knoten den Wert derselben Kontextvariablen festlegt, kann der Wert der Kontextvariablen im Verlauf des Dialogs mit einem Benutzer geändert werden. Welcher Wert zu einem bestimmten Zeitpunkt angewendet wird, hängt davon ab, welchen Knoten der Benutzer im Laufe des Dialogs auslöst. Der Wert, der im zuletzt verarbeiteten Knoten für die Kontextvariable angegeben wird, überschreibt alle Werte, die von den zuvor verarbeiteten Knoten für die Variable festgelegt wurden.

Informationen zum Aktualisieren eines Kontextvariablenwerts, der ein JSON-Objekt- oder JSON-Arraydatentyp ist, enthält der Abschnitt [Kontextvariablenwert in JSON aktualisieren](#dialog-runtime-context-update-json).

### Verarbeitung von Kontextvariablen
{: #dialog-runtime-context-processing}

Es ist durchaus von Bedeutung, an welcher Stelle eine Kontextvariable definiert wird. Die Kontextvariable wird erst erstellt und auf den von Ihnen angegeben Wert festgelegt, wenn der Service den Teil des Dialogmodulknotens verarbeitet, in dem Sie die Kontextvariable definiert haben. In den meisten Fällen wird die Kontextvariable als Teil der Knotenantwort definiert. Wenn dies zutrifft, wird die Kontextvariable erstellt und auf den angegebenen Wert festgelegt, wenn der Service zur Knotenantwort zurückkehrt. 

Bei einem Knoten mit bedingten Antworten wird die Kontextvariable erstellt und festgelegt, wenn die Bedingung für eine bestimmte Antwort erfüllt ist und die zugehörige Antwort verarbeitet wird. Beispiel: Wenn Sie eine Kontextvariable für die bedingte Antwort 1 definieren und der Service nur die bedingte Antwort 2 verarbeitet, wird die Kontextvariable, die Sie für die bedingte Antwort 1 definiert haben, nicht erstellt und nicht festgelegt. 

Informationen dazu, an welcher Stelle Kontextvariablen hinzugefügt werden müssen, die der Service erstellen und festlegen soll, während ein Benutzer mit einem Knoten mit Slots interagiert, enthält der Abschnitt [Kontextvariablen zu einem Knoten mit Slots hinzufügen](#dialog-runtime-context-var-slots).

### Reihenfolge der Ausführung
{: #dialog-runtime-context-order-of-ops}

Wenn Sie mehrere Variablen definieren, die zusammen verarbeitet werden sollen, legt die Reihenfolge ihrer Erstellung nicht fest, in welcher Reihenfolge sie von dem Service ausgewertet werden. Der Service wertet die Variablen in einer Zufallsreihenfolge aus. Wenn Sie für die erste Kontextvariable in der Liste einen Wert festlegen, können Sie diesen Wert nicht in der zweiten Variablen der Liste verwenden, da nicht garantiert ist, dass die erste Kontextvariable vor der zweiten ausgeführt wird. Verwenden Sie beispielsweise nicht zwei Kontextvariablen, zum Implementieren von Logik, die prüft, ob die Benutzereingabe das Wort `Yes` enthält.

| Variable        | Wert            |
|-----------------|------------------|
| user_input      | <? input.text ?> |
| contains_yes    | <? $user_input.contains('Yes') ?> |

Verwenden Sie stattdessen einen etwas komplexeren Ausdruck, um nicht darauf angewiesen zu sein, dass der Wert der ersten Variablen in Ihrer Liste (user_input) vor der zweiten Variablen (contains_yes) ausgewertet wird. 

| Variable      | Wert            |
|---------------|------------------|
| contains_yes  | <? input.text.contains('Yes') ?> |

### Kontextvariablen zu einem Knoten mit Slots hinzufügen
{: #dialog-runtime-context-var-slots}

Weitere Informationen zu Slots enthält der Abschnitt [Informationen mit Slots erfassen](/docs/services/assistant?topic=assistant-dialog-slots).

1.  Öffnen Sie den Knoten mit Slots in der Bearbeitungsansicht. 

    - So fügen Sie eine Kontextvariable hinzu, die verarbeitet wird, wenn eine Antwortbedingung für einen Slot erfüllt ist: 

      1.  Klicken Sie auf das Symbol **Slot bearbeiten** ![Antwort bearbeiten](images/edit-slot.png).
      1.  Klicken Sie auf das Symbol **Optionen** ![Erweiterte Antwort](images/kabob.png) und wählen Sie anschließend **Bedingte Antworten aktivieren** aus.
      1.  Klicken Sie auf das Symbol **Antwort bearbeiten** ![Antwort bearbeiten](images/edit-slot.png) neben der Antwort, der Sie die Kontextvariable zuordnen möchten.
      1.  Klicken Sie auf das Symbol **Optionen** ![Erweiterte Antwort](images/kabob.png) im Antwortabschnitt und klicken Sie anschließend auf **Kontexteditor öffnen**.
      1.  Geben Sie das Name/Wert-Paar für die Variable in den Feldern **Variable** und **Wert** an.

      ![Zeigt die Vorgehensweise zum Aufrufen des zugeordneten JSON-Editors für die bedingte Antwort für einen Slot.](images/contextvar-json-slot-multi-response.png)

    - So fügen Sie eine Kontextvariable hinzu, die festgelegt oder aktualisiert wird, wenn eine Slotbedingung erfüllt ist: 

      1.  Klicken Sie auf das Symbol **Slot bearbeiten** ![Antwort bearbeiten](images/edit-slot.png).
      1.  Klicken Sie im Menü **Optionen** ![Erweiterte Antwort](images/kabob.png) im Header der Ansicht *Slot konfigurieren* auf die Option **JSON-Editor öffnen**.
      1.  Geben Sie das Name/Wert-Paar für die Variable im JSON-Format an.

          ```json
          {
            "time_of_day": "morning"
          }
          ```
          {: codeblock}

      Gegenwärtig steht keine Methode zur Verfügung, um mit dem Kontexteditor Kontextvariablen zu definieren, die in dieser Phase der Auswertung des Dialogmodulknotens festgelegt werden. Sie müssen stattdessen den JSON-Editor verwenden. Weitere Informationen zur Verwendung des JSON-Editors enthält der Abschnitt [Kontextvariablen im JSON-Editor](#dialog-runtime-context-var-json).
      {: note}

      ![Zeigt die Vorgehensweise zum Aufrufen des zugeordneten JSON-Editors für eine Slotbedingung.](images/contextvar-json-slot-condition.png)

## Kontextvariablen im JSON-Editor
{: #dialog-runtime-context-var-json}

Eine Kontextvariable kann auch im JSON-Editor definiert werden. Die Verwendung des JSON-Editors kann hilfreich sein, wenn Sie eine komplexe Kontextvariable definieren und beim Hinzufügen oder Ändern der gesamte SpEL-Ausdruck angezeigt werden soll.

Die Name/Wert-Paare müssen die folgenden Voraussetzungen erfüllen:

- Der `name` kann beliebige Groß- und Kleinbuchstaben, numerische Zeichen (0-9) und Unterstreichungszeichen enthalten.

  Sie können auch andere Zeichen (z. B. Punkte und Bindestriche) im Namen verwenden. In diesem Fall müssen Sie die Variable jedoch im nachfolgenden Code immer mit der Kurzformsyntax `$(variable-name)` referenzieren. Weitere Details enthält der Abschnitt [Ausdrücke für Objektzugriff](/docs/services/assistant?topic=assistant-expression-language#expression-lanaguage-shorthand-context).
      {:tip}

- Der `wert` kann ein beliebiger unterstützter JSON-Typ sein (z. B. eine einfache Zeichenfolgevariable, eine Zahl, ein JSON-Array oder ein JSON-Objekt).

Im folgenden JSON-Beispiel werden die Werte für die Kontextvariablen '$dessert' (Zeichenfolge), '$toppings_array' (Array), '$age' (Zahl) und '$full_name' (Objekt) definiert:

```json
{
  "context": {
    "dessert": "Kuchen",
    "toppings_array": [
      "Zwiebeln",
      "Oliven"
    ],
    "age": 18,
    "full_name": {
      "first": "Jane",
      "last": "Doe"
    }
  },
         "output": {}
       }
```
{: codeblock}

So definieren Sie eine Kontextvariable im JSON-Format: 

1.  Klicken Sie auf den Dialogmodulknoten, zu dem Sie die Kontextvariable hinzufügen möchten, um ihn zu öffnen.

    Alle vorhandenen Kontextvariablenwerte, die für diesen Knoten definiert sind, werden in einer Gruppe mit zugehörigen Feldern **Variable** und **Wert** angezeigt. Wenn diese Felder nicht in der Bearbeitungsansicht des Knotens angezeigt werden sollen, müssen Sie den Kontexteditor schließen. Verwenden Sie zum Schließen des JSON-Editors dasselbe Menü wie zum Öffnen des Editors. In den folgenden Schritten wird beschrieben, wie Sie auf dieses Menü zugreifen können. {: note}

1.  Klicken Sie auf das Symbol **Optionen**  ![Erweiterete Antwort](images/kabob.png), das der Antwort zugeordnet ist, und klicken Sie anschließend auf **JSON-Editor öffnen**.

    ![Zeigt die Vorgehensweise zum Aufrufen des JSON-Editors für eine Standardknotenantwort.](images/contextvar-json-response.png)

    Wenn die Einstellung **Mehrere Antworten** für den Knoten auf **Ein** gesetzt ist, müssen Sie zuerst auf das Symbol **Antwort bearbeiten** ![Antwort bearbeiten](images/edit-slot.png) für die Antwort klicken, der Sie die Kontextvariable zuordnen möchten. 

    ![Zeigt die Vorgehensweise zum Aufrufen des JSON-Editors für einen Standardknoten, für den mehrere bedingte Antworten aktiviert sind.](images/contextvar-json-multi-response.png)

1.  Fügen Sie einen Block `"context":{}` hinzu, falls keiner vorhanden ist.

    ```json
    {
      "context":{},
      "output":{}
    }
    ```
    {: codeblock}

1.  Fügen Sie im Block 'context' ein Paar aus `"name"` und `"value"` für jede Kontextvariable hinzu, die Sie definieren möchten. 

    ```json
    {
      "context":{
        "name": "value"
    },
         "output": {}
       }
    ```
    {: codeblock}

    In diesem Beispiel wird eine Variable namens `new_variable` in einem Kontextblock hinzugefügt, der bereits eine Variable enthält.

    ```json
    {
      "context":{
        "existing_variable": "value",
        "new_variable":"value"
      }
    }
    ```
    {: codeblock}

    Um die Kontextvariable anschließend zu referenzieren, verwenden Sie die Syntax `$name`. Hierbei steht *name* für den Namen der von Ihnen definierten Kontextvariablen. Beispiel: `$new_variable`.

Weitere Informationen

- [Kontextvariable in JSON löschen](#dialog-runtime-context-delete-json)
- [Kontextvariable in JSON aktualisieren](#dialog-runtime-context-update-json)
- [Zwei Kontextvariablen gleichsetzen](#dialog-runtime-var-equals-var)

### Kontextvariable in JSON löschen
{: #dialog-runtime-context-delete-json}

Um eine Kontextvariable zu löschen, setzen Sie die Variable auf null.

```json
{
  "context": {
    "order_form": null
  }
}
```
{: codeblock}

Wenn Sie alle Spuren der Kontextvariablen löschen möchten, können Sie die Variable mithilfe der Methode 'JSONObject.remove(string)' aus dem Kontextobjekt löschen. Dabei müssen Sie für den Löschvorgang eine Variable verwenden. Definieren Sie die neue Variable in der Nachrichtenausgabe, damit sie nicht über den aktuellen Aufruf hinaus gespeichert bleibt.

```json
{
  "output":{
    "text" : {},
    "deleted_variable" : "<? context.remove('order_form') ?>"
  }
}
```
{: codeblock}

Alternativ können Sie die Kontextvariable in Ihrer Anwendungslogik löschen.

### Kontextvariable in JSON aktualisieren
{: #dialog-runtime-context-update-json}

Wenn ein Knoten den Wert einer Kontextvariablen festlegt, die bereits festgelegt ist, wird der vorherige Wert durch den neuen Wert überschrieben.

#### Komplexes JSON-Objekt aktualisieren

Vorherige Werte werden bei allen JSON-Typen überschrieben, jedoch mit Ausnahme von JSON-Objekten. Falls die Kontextvariable einen komplexen Typ wie beispielsweise ein JSON-Objekt besitzt, wird die Variable mithilfe der JSON-Prozedur 'merge' aktualisiert. Die Operation 'merge' fügt neu definierte Eigenschaften hinzu und überschreibt alle vorhandenen Eigenschaften des Objekts.

Im folgenden Beispiel ist die Kontextvariable 'name' als komplexes Objekt definiert:

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

Ein Dialogmodulknoten aktualisiert das JSON-Objekt für die Kontextvariable mit den folgenden Werten:

```json
{
  "complex_object": {
    "user_firstname": "Peter",
    "has_card": true
  }
}
```
{: codeblock}

Dies ergibt den folgenden Kontext:

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

Weitere Informationen zu Methoden, die Sie an Objekten ausführen können, finden Sie im Abschnitt [Methoden in der Ausdruckssprache](/docs/services/assistant?topic=assistant-dialog-methods#dialog-methods-objects).

#### Arrays aktualisieren

Falls die Kontextdaten des Dialogmoduls ein Array von Werten enthalten, können Sie das Array aktualisieren, indem Sie Werte anhängen, einen Wert entfernen oder alle Werte ersetzen.

Wählen Sie eine der folgenden Aktionen aus, um das Array zu aktualisieren. Bei allen Fällen ist das Array vor der Aktion, die Aktion selbst und das Array nach Anwendung der Aktion dargestellt.

- **Anhängen**: Um Werte am Ende des Arrays hinzuzufügen, verwenden Sie die Methode `append`.

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

- **Entfernen**: Um ein Element zu entfernen, verwenden Sie die Methode `remove` und geben Sie den Wert oder die Position des Elements im Array an.

    - Beim **Entfernen mit Wertangabe** wird ein Element anhand seines Wertes aus dem Array entfernt.

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

    - Beim **Entfernen mit Positionsangabe** wird ein Element anhand seiner Indexposition aus dem Array entfernt.

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

- **Überschreiben**: Um die Werte in einem Array zu überschreiben, legen Sie einfach die neuen Werte für das Array fest.

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
            "toppings_array": ["Ketchup", "Tomaten"]
          }
        }
        ```
        {: codeblock}

    Ergebnis:

        ```json
        {
          "context": {
            "toppings_array": ["Ketchup", "Tomaten"]
          }
        }
        ```
        {: codeblock}

Weitere Informationen zu Methoden, die Sie für Arrays ausführen können, finden Sie im Abschnitt [Methoden in der Ausdruckssprache](/docs/services/assistant?topic=assistant-dialog-methods#dialog-methods-arrays).

### Zwei Kontextvariablen gleichsetzen
{: #dialog-runtime-var-equals-var}

Wenn Sie eine Kontextvariable mit einer anderen Kontextvariablen gleichsetzen, definieren Sie einen Zeiger von der einen zur anderen. Wird der Wert einer dieser Variablen später geändert, dann wird der Wert der anderen Variablen ebenfalls geändert. 

Wenn Sie beispielsweise eine Kontextvariable wie folgt angeben, wird bei einer nachfolgenden Änderung des Werts für `$var1` oder für `$var2` der Wert der jeweils anderen Variablen ebenfalls geändert. 

| Variable  | Wert  |
|-----------|--------|
| var2      | var1   |

Setzen Sie nicht eine Variable mit einer anderen gleich, um einen Zeitpunktwert zu erfassen. Wenn Sie beispielsweise beim Arbeiten mit Arrays den in einer Kontextvariablen gespeicherten Array-Wert an einer bestimmten Stelle im Dialogmodul speichern möchten, um ihn später zu verwenden, können Sie stattdessen eine neue Variable basierend auf dem aktuellen Wert der gewünschten Variablen erstellen. 

Wenn Sie zum Beispiel eine Kopie der Werte eines Arrays an einer bestimmten Stelle im Dialogablauf erstellen möchten, fügen Sie ein neues Array hinzu, das mit den Werten des bestehenden Arrays gefüllt wird. Zu diesem Zweck können Sie die folgende Syntax verwenden: 

```json
{
"context": {
   "var2": "<? output.var2?:new JsonArray().append($var1) ?>"
 }
 }
 ```
{: codeblock}

## Abschweifungen
{: #dialog-runtime-digressions}

Ein Abschweifung tritt auf, wenn ein Benutzer mitten in einem Dialogablauf zum Erreichen eines bestimmten Ziels das Thema wechselt, um einen Dialogablauf einzuleiten, der ein anderes Ziel erreichen soll. Das Dialogmodul hat stets die Fähigkeit des Benutzers unterstützt, das Thema zu wechseln. Falls keiner der Knoten im gegenwärtig verarbeiteten Dialogmodulzweig dem Ziel der letzten Benutzereingabe entspricht, springt die Konversation zurück zur Baumstruktur und überprüft die Stammknotenbedingungen auf eine entsprechende Übereinstimmung. Die für jeden Knoten verfügbaren Abschweifungseinstellungen bieten Ihnen die Möglichkeit, dieses Verhalten noch genauer anzupassen.

Mit den Abschweifungseinstellungen können Sie den Dialog zurück zu dem Dialogablauf leiten, der durch die Abschweifung unterbrochen wurde. Beispiel: Während der Benutzer ein neues Telefon bestellt, wechselt er das Thema und erkundigt sich über Tablet-Computer. Ihr Dialogmodul kann die Frage zu Tablet-Computern beantworten und den Benutzer anschließend wieder an die Stelle zurückleiten, an der er den Prozess für die Telefonbestellung verlassen hat. Durch die Möglichkeit, nach Abschweifungen zum ursprünglichen Prozess zurückzukehren, können die Benutzer den Dialogablauf während der Ausführung besser kontrollieren. Sie können das Thema wechseln, dem Dialogablauf für das neue Thema bis zum Ende folgen und danach zu der Stelle zurückkehren, die Sie zuvor im ursprünglichen Thema erreicht hatten. Auf diese Weise kann der Dialogablauf noch besser ein tatsächliches Gespräch zwischen Menschen nachbilden.

![Zeigt eine Person, die Einzelangaben für eine Tischreservierung macht und dabei eine Frage zu vegetarischen Menüangeboten stellt. Nachdem die Frage beantwortet ist, wird der Dialog zum Angeben von Reservierungsdetails fortgesetzt.](images/digression.gif){: gif}

Die animierte Grafik erläutert an einem Modell für die Benutzerschnittstelle der Dialogmodul-Baumstruktur das Konzept der Abschweifung. Sie zeigt die Interaktionen eines Benutzers mit Dialogmodulknoten, die Abschweifungen zulassen und danach zum vorherigen Dialogablauf zurückkehren. Der Benutzer beginnt mit dem Angeben der Informationen für eine Tischreservierung zum Abendessen. Beim Ausfüllen der Slots im Knoten '#reservation' stellt der Benutzer eine Frage zu vegetarischen Menüangeboten. Das Dialogmodul beantwortet die neue Frage des Benutzers, indem es unter den Stammknoten einen Knoten mit entsprechenden Informationen ausfindig macht (einen Knoten, der sich auf die Absicht '#cuisine' bezieht). Anschließend wird der vorherige Dialog fortgesetzt, indem die Anfrage für den nächsten leeren Slot des ursprünglichen Dialogmodulknotens angezeigt wird.

In diesem Video erfahren Sie mehr.

<iframe class="embed-responsive-item" id="youtubeplayer" title="Abschweifungen im Überblick" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/I3K7mQ46K3o?rel=0" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

- [Vorbemerkungen](#dialog-runtime-digression-prereqs)
- [Abschweifungen anpassen](#dialog-runtime-enable-digressions)
- [Tipps zur Verwendung von Abschweifungen](#dialog-runtime-digress-tips)
- [Abschweifungen zu Stammknoten inaktivieren](#dialog-runtime-disable-digressions)
- [Lernprogramm für Abschweifungen](#dialog-runtime-digression-tutorial)
- [Designaspekte](#dialog-runtime-digression-design-considerations)

### Vorbemerkungen
{: #dialog-runtime-digression-prereqs}

Legen Sie beim Testen des gesamten Dialogmoduls fest, an welchen Stellen es sinnvoll ist, Abschweifungen und das anschließende Fortsetzen des ursprünglichen Dialogablaufs zuzulassen. Die folgenden Steuerelemente für Abschweifungen werden automatisch auf die Knoten angewendet. Sie sollten nur Maßnahmen ergreifen, wenn Sie dieses Standardverhalten ändern möchten.

- Jeder Stammknoten in Ihrem Dialogmodul ist standardmäßig so ausgelegt, dass er als Ziel für Abschweifungen dienen kann. Untergeordnete Knoten können nicht das Ziel einer Abschweifung sein.
- Knoten mit Slots sind so konfiguriert, dass sie keine abgehenden Abschweifungen zulassen. Alle anderen Knoten sind so konfiguriert, dass abgehende Abschweifungen zulässig sind. In den folgenden Situationen kann der Dialog jedoch nicht von einem Knoten abschweifen:

  - Mindestens ein untergeordneter Knoten des aktuellen Knotens enthält die Bedingung `anything_else` oder `true`

    Diese speziellen Bedingungen werden immer mit 'true' ausgewertet. Aufgrund dieses bekannten Verhaltens werden diese Knoten häufig in Dialogmodulen verwendet, um einen übergeordneten Knoten zu veranlassen, einen bestimmten untergeordneten Knoten als nächstes auszuwerten. Um eine Unterbrechung der Logik des bestehenden Dialogablaufs zu vermeiden, ist in diesem Fall keine Abschweifung zulässig. Abgehende Abschweifungen von einem solchen Knoten sind erst möglich, wenn Sie die Bedingung des untergeordneten Knotens ändern.

  - Die Knotenkonfiguration ermöglicht das Springen zu einem anderen Knoten oder das Überspringen der Benutzereingabe nach der Verarbeitung des Knotens

    Der Abschnitt für den letzten Schritt in einem Knoten gibt an, was nach der Verarbeitung des Knotens geschehen soll. Wenn die Konfiguration des Dialogmoduls den direkten Übergang zu einem anderen Knoten vorsieht, dann soll damit häufig das Einhalten einer bestimmten Abfolge gewährleistet werden. Wenn der Knoten so konfiguriert ist, dass die Benutzereingabe übersprungen wird, dann bedeutet dies, dass der Dialog gezwungen wird, den ersten untergeordneten Knoten nach dem aktuellen Knoten der Abfolge zu verarbeiten. Um das Unterbrechen der bestehenden Dialogablauflogik zu vermeiden, ist in solchen Fällen keine Abschweifung zulässig. Um abgehende Abschweifungen von diesem Knoten zu ermöglichen, müssen Sie die Angaben im Abschnitt für den letzten Schritt ändern.

### Abschweifungen anpassen
{: #dialog-runtime-enable-digressions}

Der Anfang und das Ende einer Abschweifung wird nicht von Ihnen definiert. Der Verlauf einer Abschweifung während der Ausführungszeit steht allein im Ermessen des Benutzers. Sie geben lediglich an, wie jeder Knoten an einer vom Benutzer veranlassten Abschweifung teilnehmen bzw. nicht teilnehmen soll. Für jeden Knoten konfigurieren Sie Folgendes:

- Ob eine abgehende Abschweifung von dem Knoten ausgehen darf
- Ob eine Abschweifung, die an anderer Stelle beginnt, den Knoten als Ziel nutzen darf
- Ob eine Abschweifung, die an anderer Stelle beginnt und den Knoten als Ziel nutzt, nach Beendigung des abzweigenden Dialogablaufs zum unterbrochenen Dialogablauf zurückkehren muss

Führen Sie die folgenden Schritte aus, um das Abschweifungsverhalten für einen einzelnen Knoten zu ändern:

1.  Klicken Sie auf den Knoten, um die zugehörige Bearbeitungsansicht zu öffnen.

1.  Klicken Sie auf **Anpassen** und dann auf die Registerkarte **Abschweifungen**.

    Die Konfigurationsoptionen unterscheiden sich je nachdem, ob der von Ihnen bearbeitete Knoten ein Stammknoten, ein untergeordneter Knoten, ein Knoten mit untergeordneten Elementen oder ein Knoten mit Slots ist.

    **Von diesem Knoten abgehende Abschweifung**

    Wenn die oben aufgelisteten Situationen nicht zutreffen, haben Sie die folgenden Auswahlmöglichkeiten:

    - **Alle Knotentypen**: Wählen Sie aus, ob Benutzer Abschweifungen, die vom aktuellen Knoten abgehen, ausführen können, bevor das Ende der aktuellen Dialogmodulverzweigung erreicht ist.

    - **Alle Knoten mit untergeordneten Elementen**: Wählen Sie aus, ob der Dialog nach einer Abschweifung zum aktuellen Knoten zurückkehren soll, wenn die Antwort des aktuellen Knotens bereits angezeigt wurde und die zugehörigen untergeordneten Knoten für das Ziel des Knotens nebensächlich sind. Setzen Sie das Umschaltsteuerelement *Zurückspringen nach Abweichungen zulassen, die nach der Antwort dieses Knotens ausgelöst wurden* auf **Nein**, um zu verhindern, dass zum aktuellen Dialogmodulknoten zurückgekehrt und die Verarbeitung der zugehörigen Verzweigung fortgesetzt wird.

      Angenommen, der Benutzer fragt `Verkaufen Sie Cupcakes?` und die Antwort `Wir bieten Cupcakes in verschiedenen Geschmacksrichtungen und Größe an` wird angezeigt, bevor der Benutzer das Thema wechselt. In diesem Fall möchten Sie vielleicht nicht, dass wieder die vorherige Stelle im Dialogmodul aufgerufen wird. Dies gilt insbesondere, wenn die untergeordneten Knoten nur potenzielle Nachfragen des Benutzers verarbeiten und gefahrlos ignoriert werden können.

      Wenn der Knoten jedoch auf die zugehörigen untergeordneten Knoten zurückgreift, um die Frage zu beantworten, kann es sinnvoll sein, dafür zu sorgen, dass der Dialog zur vorherigen Stelle zurückkehrt und die Verarbeitung der Knoten in der aktuellen Verzweigung fortsetzt. Angenommen, die ursprüngliche Antwort lautet: `Wir bieten Cupcakes in allen Geschmacksrichtungen und Größen an. Welche Menüangebote möchten Sie sehen: glutenfreie, laktosefreie oder normale?` Wenn der Benutzer an dieser Stelle das Thema wechselt, kann es wünschenswert sein, dass der Dialog zu einer vorherigen Stelle zurückkehrt, damit der Benutzer einen Menütyp weiter verfolgen und die gewünschte Information abrufen kann.

    - **Knoten mit Slots**: Wählen Sie aus, ob Benutzer Abschweifungen von dem Knoten ausführen können, bevor alle Slots gefüllt sind. Setzen Sie das Umschaltsteuerelement *Abgehende Abschweifungen beim Füllen von Slots zulassen* auf **Ja**, um abgehende Abschweifungen zu aktivieren.

      Wenn Abschweifungen aktiviert sind und das Dialogmodul nach der Abschweifung zurückkehrt, wird die Anfrage für den nächsten leeren Slot angezeigt, damit der Benutzer weitere Informationen angeben kann. Wenn Abschweifungen inaktiviert sind, werden alle Benutzereingaben ignoriert, die keine Werte zum Füllen von Slots enthalten. Sie können jedoch nicht angeforderte Fragen einplanen, die Benutzer bei der Interaktion mit dem Knoten möglicherweise stellen werden, indem Sie entsprechende Slot-Handler definieren. Weitere Informationen enthält der Abschnitt [Slots hinzufügen](/docs/services/assistant?topic=assistant-dialog-slots#dialog-slots-add).

      Die folgende Abbildung zeigt, wie abgehende Abschweifungen von dem Knoten mit Slots namens  '#reservation' (dargestellt in der vorherigen Abbildung) konfiguriert werden.

      ![Zeigt die Einstellung für abgehende Abschweifungen von einem Knoten mit Slots](images/digress-away-slots-full.png)

    - **Knoten mit Slots**: Wählen Sie aus, ob der Benutzer nur abgehende Abschweifungen erstellen kann, wenn anschließend zum aktuellen Knoten zurückgekehrt wird, indem Sie das Kontrollkästchen **Nur Abschweifungen von Slots zu Knoten, die die Rückkehr zulassen** auswählen.

      Wenn diese Option aktiviert ist, ignoriert das Dialogmodul bei der Suche nach einem Knoten zum Beantworten der abschweifenden Frage des Benutzers alle Stammknoten, deren Konfiguration die Rückkehr nach der Abschweifung nicht zulässt. Wählen Sie dieses Kontrollkästchen aus, um zu verhindern, dass die Benutzer den Knoten verlassen, bevor das Füllen der erforderlichen Slots abgeschlossen ist.

    **Abschweifungen zu diesem Knoten**

    Mit den folgenden Auswahlmöglichkeiten können Sie das Verhalten von Abschweifungen zu einem Zielknoten festlegen:

    - Verhindern, dass Benutzer zu einem Zielknoten abschweifen. Weitere Informationen finden Sie im Abschnitt [Abschweifungen zu Stammknoten inaktivieren](#dialog-runtime-disable-digressions).

    - Wenn Abschweifungen zu dem Knoten aktiviert sind, wählen Sie aus, ob das Dialogmodul zu dem Dialogablauf zurückkehren muss, von dem die Abschweifung abzweigt. Wenn diese Option aktiviert ist, kehrt der Dialogablauf zu dem unterbrochenen Knoten zurück, nachdem die Verzweigung des aktuellen Knotens vollständig verarbeitet wurde. Damit das Dialogmodul nach der Verarbeitung der Abzweigung zurückkehrt, wählen Sie **Nach Abschweifung zurückkehren** aus.

    Das folgende Bild zeigt, wie Abschweifungen zum Knoten '#cuisine' (dargestellt in einer vorherigen Abbildung) konfiguriert werden.

    ![Zeigt die Einstellung für abgehende Abschweifungen von einem Knoten mit Slots](images/digress-into-cuisine-full.png)

1.  Klicken Sie auf **Anwenden**.

1.  Testen Sie in der Anzeige 'Ausprobieren' das Ausführungsverhalten der Abschweifung.

    Auch in diesem Fall können Sie den Anfang und das Ende der Abschweifung nicht definieren. Es liegt im Ermessen des Benutzers, wo und wann Abschweifungen auftreten. Sie können lediglich Einstellungen dafür festlegen, wie ein einzelner Knoten an einer Abschweifung teilnimmt. Da Abschweifungen nicht vorhersehbar sind, ist nur schwer abzuschätzen, wie Ihre Konfigurationsvorgaben sich auf den Dialog insgesamt auswirken. Um die tatsächlichen Auswirkungen Ihrer Vorgaben zu überprüfen, müssen Sie das Dialogmodul testen.

Die Knoten '#reservation' und '#cuisine' stellen zwei Dialogmodulverzweigungen dar, die an einer einzelnen benutzergesteuerten Abschweifung teilnehmen können. Die für jeden einzelnen Knoten definierten Abschweifungseinstellungen ermöglichen diese Art von Abschweifung während der Laufzeit.

![Zeigt zwei Dialogmodule. Ein Dialogmodul konfiguriert die abgehende Abschweifung von einem Knoten mit Slots namens '#reservation', das andere Dialogmodul konfiguriert die Abschweifung zum Zielknoten '#cuisine'.](images/digression-settings.png)

### Tipps zur Verwendung von Abschweifungen
{: #dialog-runtime-digress-tips}

In diesem Abschnitt werden Lösungen für Situationen beschrieben, die bei der Verwendung von Abschweifungen auftreten können. 

- **Angepasste Antwortnachricht**: Ziehen Sie bei allen Knoten, für die Sie die Rückkehr nach Abschweifungen aktivieren, das Anzeigen von Nachrichten in Betracht, die den Benutzer davon in Kenntnis setzen, dass zu der Stelle im vorherigen Dialogablauf zurückgekehrt wird, an der die Abschweifung begann. Verwenden Sie in der Textantwort eine spezielle Syntax, die es Ihnen ermöglicht, zwei Versionen der Antwort hinzuzufügen.

  Wenn Sie nichts anderes angeben, wird ein zweites Mal dieselbe Textantwort angezeigt, um die Benutzer darüber zu informieren, dass wieder der Knoten aufgerufen wurde, an dem die Abschweifung begann. Sie können die Benutzer deutlicher darauf aufmerksam machen, dass wieder der ursprüngliche Dialogablauf aufgerufen wurde, indem Sie eine eindeutige Rückkehrnachricht angeben. 

  Wenn die ursprüngliche Textantwort für den Knoten beispielsweise `Wie lautet die Bestellnummer?` ist, kann es sinnvoll sein, eine Nachricht wie `Kehren wir nun dahin zurück, wo wir vorher waren. Wie lautet die Bestellnummer?` anzuzeigen, wenn die Benutzer zu dem Knoten zurück geführt werden. 

  Verwenden Sie zu diesem Zweck die folgende Syntax, um die Textantwort für den Knoten anzugeben: 

  `<? (returning_from_digression)? "post-digression message" : "first-time message" ?>`

  Beispiel:

  ```bash
  <? (returning_from_digression)? "Kehren wir nun dahin zurück, wo wir vorher waren.
  Wie lautet die Bestellnummer?" : "Wie ist die Bestellnummer?" ?>
  ```
  {: codeblock}

  Sie können keine SpEL-Ausdrücke und keine Kurzformsyntax in den Textantworten verwenden, die Sie hinzufügen. Die Kurzformsyntax darf generell nicht verwendet werden. Stattdessen muss die Nachricht erstellt werden, indem Sie die Textzeichenfolgen und die ausführliche SpEL-Ausdruckssyntax verknüpfen, um die vollständige Antwort zu bilden.{: note}
  
  Verwenden Sie beispielsweise die folgende Syntax, um eine Kontextvariable in eine Textantwort einzufügen, die Sie normalerweise angeben würden (z. B. `Was kann ich für Sie tun, $username?`):

  ```bash
  <? (returning_from_digression)? "Wo waren wir vorher? " +
  context["username"] + "? Ja genau - ich hatte gefragt, was ich heute
  für Sie tun kann." : "Was kann ich heute für Sie tun " +
  context["username"] + "?" ?>
  ```

  Details zur vollständigen SpEL-Ausdruckssyntax enthält der Abschnitt [Ausdruck für Objektzugriff](/docs/services/assistant?topic=assistant-expression-language#expression-language-shorthand-syntax).

- **Rückkehr verhindern**: In manchen Fällen möchten Sie möglicherweise die Rückkehr zu dem unterbrochenen Dialogablauf aufgrund einer Auswahl des Benutzers im aktuellen Dialogablauf verhindern. Verwenden Sie eine besondere Syntax,  um die Rückkehr von einem bestimmten Knoten aus zu verhindern. 

  Angenommen, Sie verfügen über einen Knoten, der durch `#General_Connect_To_Agent` oder eine ähnliche Absicht bedingt wird. Wenn Sie nach dem Auslösen dieses Knotens die Bestätigung des Benutzers einholen möchten, bevor der Benutzer zu einem externen Service weitergeleitet wird, können Sie eine Antwort wie diese hinzufügen: `Soll ich Sie jetzt mit einem Servicemitarbeiter verbinden?` Anschließend können Sie zwei untergeordnete Knoten hinzufügen, die sich auf `#yes` bzw. `#no` beziehen.
  
  Die beste Methode zum Verwalten von Abschweifungen für Verzweigungen dieses Typs besteht darin, für den Stammknoten die Rückkehr von Abschweifungen zuzulassen. Fügen Sie jedoch im Knoten `#yes` den SpEL-Ausdruck `<? clearDialogStack() ?>` in die Antwort ein. Beispiel:
  
    ```bash
  OK. Ich leite Sie jetzt weiter. <? clearDialogStack() ?>
  ```
  {: codeblock}

  Dieser SpEL-Ausdruck verhindert die Rückkehr von der Abschweifung für diesen Knoten. Wenn der Benutzer die Bestätigungsabfrage mit 'yes' beantwortet, wird die entsprechende Antwort angezeigt und der unterbrochene Dialogablauf wird nicht fortgesetzt. Wenn der Benutzer mit 'no' antwortet, wird der unterbrochene Ablauf fortgesetzt.

### Abschweifungen zu einem Stammknoten inaktivieren
{: #dialog-runtime-disable-digressions}

Wenn der Ablauf zu einem Stammknoten abschweift, wird der für den betreffenden Knoten konfigurierte Dialogablauf ausgeführt. D. h., es wird gegebenenfalls eine Reihe untergeordneter Knoten verarbeitet, bevor das Ende der Knotenverzweigung erreicht ist, und anschließend wird (sofern die Konfiguration dies vorsieht) der unterbrochene Dialogablauf fortgesetzt. Beim Testen des Dialogmoduls stellen Sie womöglich fest, dass ein Stammknoten zu häufig oder an nicht erwarteten Stellen ausgelöst wird, oder dass das zugehörige Dialogmodul zu komplex ist und den Benutzer zu weit vom ursprünglichen Kurs wegführt und daher nicht für eine temporäre Abschweifung geeignet ist. Wenn Sie Abschweifungen zu dem betreffenden Dialogmodul verhindern möchten, können Sie den Stammknoten so konfigurieren, dass er nicht als Ziel für Abschweifungen verwendet werden kann.

Führen Sie die folgenden Schritte aus, um Abschweifungen zu einem Stammknoten zu inaktivieren:

1.  Klicken Sie auf den Stammknoten, um ihn zum Bearbeiten zu öffnen.
1.  Klicken Sie auf **Anpassen** und dann auf die Registerkarte **Abschweifungen**.
1.  Setzen Sie das Umschaltsteuerelement *Abschweifungen zu diesem Knoten zulassen* auf **Aus**.
1.  Klicken Sie auf **Anwenden**.

Wenn Sie Abschweifungen zu mehreren Stammknoten verhindern möchten, ohne jeden Knoten einzeln zu bearbeiten, können Sie die betreffenden Knoten zu einem Ordner hinzufügen. Auf der Seite *Anpassen* für den Ordner können Sie das Umschaltsteuerelement *Abschweifungen zu diesem Knoten zulassen* auf **Aus** setzen, damit die Konfiguration gleichzeitig auf alle Knoten angewendet wird. Weitere Informationen enthält der Abschnitt '[Dialogmodul in Ordnern zusammenfassen](/docs/services/assistant?topic=assistant-dialog-build#dialog-build-folders)'.

### Lernprogramm für Abschweifungen
{: #dialog-runtime-digression-tutorial}

Führen Sie das Lernprogramm '[tutorial](/docs/services/assistant?topic=assistant-tutorial-digressions)' aus, um einen Arbeitsbereich zu importieren, für den bereits eine Reihe von Knoten definiert ist. In einigen Übungen erfahren Sie mehr über die Funktionsweise von Abschweifungen.

### Designaspekte
{: #dialog-runtime-digression-design-considerations}

- **Häufige Nutzung von Rückgriffsknoten vermeiden**: Viele Entwickler von Dialogmodulen fügen einen Knoten mit einer Bedingung 'true' oder 'anything_else' am Ende jeder Dialogmodulverzweigung ein, um zu verhindern, dass Benutzer in der Verzweigung stecken bleiben. Dabei wird eine generische Nachricht zurückgegeben, wenn die Benutzereingabe keinem von Ihnen vorgesehenen Verlauf entspricht, der durch einen bereitgestellten Dialogmodulknoten verarbeitet werden kann. Dialogabläufe, die dieses Designkonzept verwenden, lassen jedoch keine abgehende Abschweifung zu.

  Überprüfen Sie für alle Verzweigungen, die dieses Konzept verwenden, ob es hilfreich wäre, abgehende Abschweifungen von den einzelnen Verzweigungen zuzulassen. Wenn die Benutzereingabe keiner von Ihnen eingeplanten Eingabe entspricht, liegt möglicherweise eine Übereinstimmung mit einem ganz anderen Dialogablauf in Ihrer Baumstruktur vor. Anstatt mit einer generischen Nachricht zu antworten, können Sie den weiteren Verlauf des Dialogmoduls nutzen, um eine Antwort auf die Frage des Benutzers zu finden. Außerdem kann der Stammknoten 'Anything else' auf Eingaben reagieren, die von keinem anderen Stammknoten beantwortet werden können.

- **Sprünge zu einem Endknoten wiederholt in Betracht ziehen**: In vielen Dialogmodulen wird zum Abschluss eine Standardfrage wie 'Ist Ihre Frage damit beantwortet?' gestellt. Benutzer können nicht von Knoten abschweifen, die so konfiguriert sind, dass sie zu einem anderen Knoten springen. Mit anderen Worten: Wenn alle Endknoten Ihrer Verzweigungen so konfiguriert sind, dass sie zu einem gemeinsamen Endknoten springen, dann sind keine Abschweifungen möglich. Ziehen Sie in Betracht, die Benutzerzufriedenheit durch Metriken oder mit anderen Methoden zu überwachen.

- **Potenzielle Abschweifungsketten testen**: Wenn ein Benutzer vom aktuellen Knoten zu einem anderen Knoten abschweift, der abgehende Abschweifungen zulässt, kann der Benutzer potenziell auch von dem anderen Knoten abschweifen und dieses Verhaltensmuster einmal oder mehrere Male wiederholen. Wenn der Startknoten in der Abschweifungskette für die Rückkehr nach der Abschweifung konfiguriert ist, wird der Benutzer anschließend zum aktuellen Dialogmodulknoten zurückgeführt. Alle nachfolgenden Knoten in der Kette, die nicht für die Rückkehr konfiguriert sind, werden nicht als Abschweifungsziele in Betracht gezogen. Szenarios mit mehreren Abschweifungen sollten getestet werden, um festzustellen, ob die einzelnen Knoten wie erwartet funktionieren.

- **Bevorzugte Verarbeitung des aktuellen Knotens berücksichtigen**: Beachten Sie, dass Knoten außerhalb des aktuellen Ablaufs nur als Abschweifungsziele in Betracht kommen, wenn der aktuelle Ablauf die Benutzereingabe nicht abwickeln kann. In einem Knoten mit Slots, der abgehende Abschweifungen zulässt, ist es umso wichtiger, deutlich zu machen, welche Informationen vom Benutzer anzugeben sind. Außerdem sollten entsprechende Bestätigungsnachrichten angezeigt werden, nachdem vom Benutzer ein Wert eingegeben wurde.

  Beim Füllen von Slots kann jeder beliebige Slot gefüllt werden. Es kann daher vorkommen, dass in einem Slot unerwartet eine Benutzereingabe erfasst wird. Angenommen, Sie verfügen über einen Knoten mit Slots zum Erfassen der erforderlichen Informationen für eine Tischreservierung zum Abendessen. In einem der Slots wird die Datumsangabe erfasst. Möglicherweise stellt der Benutzer beim Angeben der Reservierungsdetails die Frage 'Wie sind die Wetteraussichten für morgen?'. Ein vorhandener Stammknoten mit der Bedingung '#forecast' könnte diese Frage beantworten. Da die Benutzereingabe jedoch das Wort 'morgen' enthält und gegenwärtig der Knoten mit Slots für eine Tischreservierung verarbeitet wird, geht der Service davon aus, dass der Benutzer das Datum für die Reservierung angeben oder ändern möchte. *Der aktuelle Knoten wird immer bevorzugt behandelt.* Wenn Sie eine eindeutige Bestätigungsnachricht definieren (z. B. 'OK, die Reservierung wird für morgen vorgenommen.'), kann der Benutzer das vorliegende Missverständnis leichter erkennen und korrigieren.

  Wenn der Benutzer jedoch beim Füllen der Slots einen Wert angibt, der von keinem der Slots erwartet wird, kann es vorkommen, dass der Wert zu einem ganz anderen Stammknoten passt, zu dem der Benutzer gar nicht abschweifen wollte.

  Führen Sie zahlreiche Testläufe durch, um das Abschweifungsverhalten genau auf Ihre Erfordernisse abzustimmen.

- **Abschweifung anstelle von Slot-Handler verwenden**: Verwenden Sie für allgemeine Fragen, die jederzeit von Benutzern gestellt werden können, einen Stammknoten, der als Abschweifungsziel genutzt werden kann, die Eingabe verarbeitet und dann zur ursprünglichen Stelle im Ablauf zurückkehrt. Beziehen Sie beim Einrichten von Dialogmodulknoten mit Slots möglichst schon vorab zugehörige Fragen ein, die Benutzer beim Füllen der Slots stellen könnten und beantworten Sie diese Fragen, indem Sie entsprechende Handler zum Knoten hinzufügen.

  Beispiel: Wenn der Knoten mit Slots Informationen zum Ausfüllen eines Versicherungsanspruchs erfasst, kann es sinnvoll sein, Handler für häufig gestellte Fragen zu Versicherungen hinzuzufügen. Für Fragen zum Anfordern von Hilfe, zu den Standorten von Filialen oder zur Geschichte des Unternehmens sollten Sie jedoch jeweils einen Knoten der Stammebene verwenden.

## Vereindeutigung ![nur Plus- oder Premium-Plan](images/premium.png)
{: #dialog-runtime-disambiguation}

Dieses Feature ist nur für Benutzer des Plus- oder Premium-Plans verfügbar.
{: tip}

Durch das Aktivieren der Vereindeutigung weisen Sie den Service an, die Benutzer um Unterstützung zu bitten, wenn mehr als ein Dialogmodulknoten auf die Benutzereingabe antworten kann. Anstatt zu raten, welcher Knoten verarbeitet werden soll, listet der Assistent die am besten geeigneten Knotenoptionen auf und bittet den Benutzer, die zutreffende Option auszuwählen.

![Zeigt einen Beispieldialog zwischen einem Benutzer und dem Assistenten, in dem der Assistent den Benutzer um Klarstellung bittet.](images/disambig-demo.png)

Die Vereindeutigung (falls aktiviert) wird nur ausgelöst, wenn die folgenden Bedingungen zutreffen:

- Die Konfidenzbewertung für mindestens eine der mit am höchsten bewerteten Absichten ist größer als 55 % der Konfidenzbewertung der am höchsten bewerteten Absicht.
- Die Konfidenzbewertung der am höchsten bewerteten Absicht liegt über 0,2.

- Selbst wenn diese Bedingungen zutreffen, wird die Vereindeutigung nur angewendet, wenn mindestens zwei unabhängige Konten in Ihrem Dialogmodul die folgenden Kriterien erfüllen:

- Die Knotenbedingung enthält eine der Absichten, von denen die Vereindeutigung ausgelöst wurde. Oder die Knotenbedingung wird anderweitig als 'true' ausgewertet. Wenn der Knoten beispielsweise nach einem Entitätstyp sucht und die Entität in der Benutzereingabe erwähnt wird, kommt er in Frage.
- Das Feld *Externer Knotenname* für den Knoten enthält Text.

Weitere Informationen

- [Beispiel für Vereindeutigung](#dialog-runtime-disambig-example)
- [Vereindeutigung aktivieren](#dialog-runtime-disambig-enable)
- [Knoten auswählen](#dialog-runtime-choose-nodes)
- [Keine der vorherigen Optionen verarbeiten](#dialog-runtime-handle-none)
- [Vereindeutigung testen](#dialog-runtime-disambig-test)

### Beispiel für Vereindeutigung
{: #dialog-runtime-disambig-example}

Angenommen, Sie verfügen über ein Dialogmodul, das zwei Knoten mit Absichtsbedingungen enthält, die sich auf Abbruchanforderungen beziehen. Die Bedingungen lauten wie folgt:

- eCommerce_Cancel_Product_Order
- Customer_Care_Cancel_Account

Wenn die Benutzereingabe 'ich muss heute stornieren' lautet, werden möglicherweise die folgenden Absichten in der Eingabe erkannt:

`[`
`{"intent":"Customer_Care_Cancel_Account","confidence":0.6618281841278076},`
`{"intent":"eCommerce_Cancel_Product_Order","confidence":0.4330700159072876},`
`{"intent":"Customer_Care_Appointments","confidence":0.2902342438697815},`
`{"intent":"Customer_Care_Store_Hours","confidence":0.2550420880317688},`
`...]`

Der Service ermittelt einen Konfidenzwert von 0,6618281841278076 (66%) für die Übereinstimmung des Benutzeraussage mit der Absicht '#Customer_Care_Cancel_Account'. Wenn eine andere Absicht eine Konfidenzbewertung aufweist, die größer als 55 % von 66 % ist, dann sind die Kriterien für eine mögliche Vereindeutigung erfüllt.

`0,66 x 0,55 = 0,36`

Absichten mit einer Bewertung über 0,36 kommen infrage.

Im vorliegenden Beispiel liegt die Absicht '#eCommerce_Cancel_Product_Order' mit einer Konfidenzbewertung von 0,4330700159072876 über dem Grenzwert.

Wenn die Benutzereingabe 'ich muss heute stornieren' lautet, werden beide Dialogmodulknoten als mögliche Kandidaten zum Beantworten der Eingabe eingestuft. Um festzulegen, welcher Dialogmodulknoten verarbeitet wird, bittet der Assistent den Benutzer, einen Knoten auszuwählen. Als Hilfe beim Auswählen, zeigt der Assistent eine kurze Zusammenfassung der Zielsetzung der beiden Knoten an. Der vom Assistenten angezeigte zusammenfassende Text wird direkt aus den Informationen für *externer Knotenname* extrahiert, die für jeden Knoten angegeben wurden.

![Der Service fordert den Benutzer auf, aus einer Liste mit Dialogmoduloptionen auszuwählen (dazu gehören 'Konto aufheben', 'Produktbestellung stornieren' und 'Keine der obigen Optionen').](images/disambig-tryitout.png)

Beachten Sie, dass der Service den Begriff 'heute' in der Benutzereingabe als ein Datum, d. h. ein Erwähnung der Entität'@sys-date' erkennt. Wenn die Baumstruktur Ihres Dialogmoduls einen Knoten enthält, der sich auf die Entität '@sys-date' bezieht, wird er ebenfalls in die Auswahlliste der Kandidaten für die Vereindeutigung aufgenommen. In der folgenden Abbildung ist er als Option *Datumsangabe erfassen* in der Liste aufgeführt.

![Der Service fordert den Benutzer auf, aus einer Liste mit Dialogmoduloptionen auszuwählen (dazu gehört die Option 'Datumsangabe erfassen').](images/disambig-tryitout-date.png)

Das folgende Video gib einen Überblick über die Vereindeutigung.

<iframe class="embed-responsive-item" id="youtubeplayer0" title="Vereindeutigung im Überblick" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/VVyklAXlmbA?rel=0" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

### Vereindeutigung aktivieren
{: #dialog-runtime-disambig-enable}

So aktivieren Sie die Vereindeutigung:

1.  Klicken Sie auf der Seite 'Dialogmodule' auf **Einstellungen**.
1.  Klicken Sie auf **Vereindeutigung**.
1.  Setzen Sie das Umschaltsteuerelement im Abschnitt *Vereindeutigung aktivieren* auf **Ein**.
1.  Geben Sie in das Feld für die Aufforderungsnachricht Text ein, der vor der Liste der Dialogknotenoptionen angezeigt werden soll. Beispiel: *Was möchten Sie tun?*
1.  **Optional**: Geben Sie in das Nachrichtenfeld für 'Keine der obigen Optionen' Text ein, der als zusätzliche Option angezeigt werden soll, die der Benutzer auswählen kann, wenn keiner der anderen Dialogmodulknoten dem Anliegen des Benutzers entspricht. Beispiel *Keine der obigen Optionen*.

    Geben Sie eine kurze Nachricht an, die zusammen mit den anderen Optionen angezeigt werden kann. Die Nachricht muss weniger als 512 Zeichen lang sein. Informationen zum Verhalten des Service, wenn ein Benutzer diese Option auswählt, enthält der Abschnitt [Keine der obigen Optionen verarbeiten](#dialog-runtime-handle-none).

1.  Klicken Sie auf **Schließen**.
1.  Legen Sie fest, für welche Dialogmodulknoten der Assistent Hilfe anfordern soll.

    - Sie können Knoten auf jeder Ebene der Baumhierarchie auswählen.
    - Sie können Knoten auswählen die sich auf Absichten, Entitäten, Sonderbedingungen, Kontextvariablen oder eine beliebige Kombination dieser Elemente beziehen.

    Tipps zu diesem Vorgang enthält der Abschnitt [Knoten auswählen](#dialog-runtime-choose-nodes).

    Führen Sie für jeden Knoten, für den Sie die Vereindeutigung zulassen möchten, die folgenden Schritte aus:

    1.  Klicken Sie auf den Knoten, um ihn in der Bearbeitungsansicht zu öffnen.
    1.  Beschreiben Sie im Feld *Externer Knotenname* die Benutzertask, für deren Verarbeitung dieser Dialogmodulknoten konzipiert ist. Beispiel: *Konto aufheben*.

        ![Zeigt, an welcher Stelle die Informationen für den externen Knotennamen in der Knotenbearbeitungsansicht hinzugefügt werden.](images/disambig-node-purpose.png)

### Knoten auswählen
{: #dialog-runtime-choose-nodes}

Wählen Sie Knoten aus, die als Stammelement für eine eindeutige Verzweigung im Dialogmoduls dienen und als Auswahlmöglichkeiten für die Vereindeutigung infrage kommen. Dazu können auch Knoten gehören, die untergeordnete Elemente anderer Knoten sind. Der Knoten muss sich unbedingt auf mindestens einen eindeutigen Wert beziehen, der ihn von allen anderen Elementen unterscheidet.

Das Tool kann Absichtskonflikte erkennen, die auftreten, wenn für zwei oder mehr Absichten Benutzerbeispiele vorhanden sind, die sich überschneiden. Zuerst müssen Sie [solche Konflikte lösen](/docs/services/assistant?topic=assistant-intents#intents-resolve-conflicts), um sicherzustellen, dass die Absichten an sich so eindeutig wie möglich sind, damit der Service bessere Konfidenzbewertungen für Absichten erzielen kann.
{: note}

Beachten Sie Folgendes:

- Wenn der Service bei Knoten, die sich auf Absichten beziehen, mit hinreichender Konfidenz erkennt, dass die Absichtsbedingung des Knotens mit der Absicht des Benutzers übereinstimmt, wird der Knoten als Option für Vereindeutigung einbezogen.
- Ein Knoten mit boolescher Bedingung (Bedingung, die entweder als 'true' oder als 'false' ausgewertet wird) wird als Option für Vereindeutigung einbezogen, wenn die Bedingung als 'true' ausgewertet wird. Wenn sich der Knoten zum Beispiel auf einen Entitätstyp bezieht und die Entität in der Eingabe erwähnt wird, die die Vereindeutigung auslöst, dann wird der Knoten als Option einbezogen.
- Die Reihenfolge der Knoten in der Baumhierarchie wirkt sich auf die Vereindeutigung aus.

  - Sie hat Einfluss darauf, ob die Vereindeutigung überhaupt ausgelöst wird.

    Betrachten wir zum Beispiel das vorherige [Szenario](#dialog-runtime-disambig-example), das als Einführung in die Vereindeutigung verwendet wurde. Wenn der Knoten mit der Bedingung '@sys-date' in der Baumstruktur des Dialogmoduls weiter oben platziert wird, dann wird für die Knoten, die sich auf die Absichten  '#Customer_Care_Cancel_Account' und '#eCommerce_Cancel_Product_Order' beziehen, die Vereindeutigung keinesfalls ausgelöst, wenn ein Benutzer die Nachricht 'ich muss heute stornieren' eingibt. Dies liegt darin begründet, dass der Service die Datumserwähnung 'heute' aufgrund der Position der entsprechenden Knoten in der Baumstruktur für wichtiger hält als die Absichtsverweise.

  - Sie hat Einfluss darauf, welche Knoten in die Liste der Optionen für Vereindeutigung einbezogen werden.

    Es kann vorkommen, dass ein Knoten nicht wie erwartet als Option für die Vereindeutigung aufgelistet wird. Dies kann der Fall sein, wenn ein Bedingungswert auch von einen Knoten referenziert wird, der aus irgendeinem Grund nicht als Kandidat für die Liste der Optionen für Vereindeutigung infrage kommt. Beispiel: Eine Entitätserwähnung löst einen Knoten aus, der in der Baumstruktur des Dialogmoduls zwar weiter oben positioniert, jedoch nicht für die Vereindeutigung aktiviert ist. Wenn die betreffende Entität die einzige Bedingung für einen Knoten ist, der zwar für die Vereindeutigung *aktiviert*, jedoch in der Baumstruktur weiter unten positioniert ist, dann wird sie nicht als Option für die Vereindeutigung hinzugefügt, da der Service sie niemals erreicht. Sie wurde als Übereinstimmung mit dem früheren Knoten erkannt und übergangen, d. h. der spätere Knoten wird vom Service nicht verarbeitet.
Testen Sie für jeden Knoten, den Sie für die Vereindeutigung zulassen, Szenarios, in denen zu erwarten ist, dass der Knoten in die Liste der Optionen für Vereindeutigung aufgenommen wird. Die Tests geben Ihnen die Gelegenheit, die Knotenreihenfolge oder andere Faktoren anzupassen, die sich auf die Effizienz der Vereindeutigung während der Laufzeit auswirken.

### Keine der vorherigen Optionen verarbeiten
{: #dialog-runtime-handle-none}

Wenn ein Benutzer auf die Option *Keine der vorherigen Optionen* klickt, entfernt der Service die in der Benutzereingabe erkannten Absichten aus der Nachricht und übergibt die Nachricht erneut. Diese Aktion löst in der Regel den Knoten 'anything_else' in der Baumstruktur Ihres Dialogmoduls aus.

Um die Antwort anzupassen, die in dieser Situation zurückgegeben wird, können Sie einen Stammknoten mit einer Bedingung hinzufügen, die nach einer Benutzereingabe ohne erkannte Absichten sucht (schließlich wurden die Absichten entfernt) und eine Eigenschaft 'suggestion_id' enthält. Der Service fügt eine Eigenschaft 'suggestion_id' hinzu, wenn die Vereindeutigung ausgelöst wird.
{: tip}

Fügen Sie einen Stammknoten mit der folgenden Bedingung hinzu:

```json
intents.size()==0 && input.suggestion_id
```
{: codeblock}

Diese Bedingung wird nur erfüllt, wenn die Eingabe eine Gruppe von Vereindeutigungsoptionen ausgelöst hat, für die vom Benutzer angegeben wurde, dass keine von ihnen der Intention des Benutzers entspricht.

Fügen Sie eine Antwort hinzu, die den Benutzern zeigt, dass Sie verstanden haben, dass keine der vorgeschlagenen Optionen ihren Anforderungen entspricht, und führen Sie entsprechende Aktionen aus.

Auch hier ist die Platzierung der Knoten in der Baumstruktur von Bedeutung. Wenn ein Knoten, der sich auf einen in der Benutzereingabe erwähnten Entitätstyp bezieht, oberhalb des betreffenden Knotens in der Baumstruktur positioniert ist, dann wird die Antwort des übergeordneten Knotens angezeigt.

### Vereindeutigung testen
{: #dialog-runtime-disambig-test}

So testen Sie die Vereindeutigung:

1.  Geben Sie in der Anzeige 'Ausprobieren' eine Testäußerung ein, die Ihrer Ansicht nach ein geeigneter Kandidat für die Vereindeutigung ist, d. h. zwei oder mehr Ihrer Dialogmodulknoten sind so konfiguriert, dass sie auf Äußerungen wie diese antworten.

1.  Wenn die Antwort keine Auswahlliste mit Optionen für Dialogmodulknoten enthält, prüfen Sie zunächst, dass Sie für jeden der Knoten zusammenfassende Informationen im Feld für den externen Knotennamen hinzugefügt haben.

1.  Wenn die Vereindeutigung weiterhin nicht ausgelöst wird, liegen die Konfidenzwerte für die Knoten möglicherweise nicht so dicht beieinander, wie Sie angenommen haben.

    Sie können Informationen zu den Absichten, Entitäten und zu anderen Eigenschaften abrufen, die für bestimmte Benutzereingaben zurückgegeben werden.

    - Um die Konfidenzbewertungen der Absichten anzuzeigen, die in der Benutzereingabe erkannt wurden, fügen Sie temporär '<? intents ?>' am Ende der Knotenantwort für einen Knoten hinzu, von dem Sie wissen, dass er ausgelöst wird.

      Dieser SpEL-Ausdruck zeigt die in der Benutzereingabe erkannten Absichten als Array an. Das Array enthält den Namen der Absicht und gibt an, welchen Konfidenzwert der Service dafür ermittelt hat, dass die Absicht der Intention des Benutzers entspricht.

    - Um anzuzeigen, welche Entitäten (wenn überhaupt) in der Benutzereingabe erkannt wurden, können Sie die aktuelle Antwort temporär durch eine einzelne Textantwort ersetzen, die den SpEL-Ausdruck enthält: '<? entities ?>'.

      Dieser SpEL-Ausdruck zeigt die in der Benutzereingabe erkannten Entitäten als Array an. Das Array enthält den Entitätsnamen, die Position der Entitätserwähnung innerhalb der Benutzereingabezeichenfolge, die Zeichenfolge mit der Entitätserwähnung und den vom Service ermittelten Konfidenzwert dafür, dass der Begriff eine Erwähnung des angegebenen Entitätstyps ist.

    - Um Details für alle Artefakte auf einmal anzuzeigen (einschließlich anderer Eigenschaften wie dem Wert einer angegebenen Kontextvariablen zum Zeitpunkt des Aufrufs), können Sie die vollständige API-Antwort überprüfen. Weitere Informationen enthält der Abschnitt '[Details zu API-Aufrufen anzeigen](/docs/services/assistant?topic=assistant-dialog-tips#dialog-tips-inspect-api)'.

1.  Entfernen Sie vorübergehend die von Ihnen im Feld *Externer Knotenname* hinzugefügte Beschreibung für mindestens einen der Knoten, der voraussichtlich als Vereindeutigungsoption aufgelistet wird.

1.  Geben Sie die Testäußerung erneut in die Anzeige 'Ausprobieren' ein.

    Wenn Sie den Ausdruck '<? intents ?>' zur Antwort hinzugefügt haben, enthält der zurückgegebene Text eine Liste der Absichten, die der Service in der Testäußerung erkannt hat, sowie den Konfidenzwert für jede dieser Absichten.

    ![Service gibt Array mit Absichten zurück, einschließlich 'Customer_Care_Cancel_Account' und 'eCommerce_Cancel_Product_Order'.](images/disambig-show-intents.png)

Nachdem Sie den Test abgeschlossen haben, entfernen Sie alle SpEL-Ausdrücke, die Sie an Knotenantworten angefügt haben, oder fügen Sie die ursprünglichen Antworten wieder hinzu, die Sie durch Ausdrücke ersetzt hatten, und füllen Sie alle Felder *Externer Knotenname* erneut, in denen Sie Text gelöscht haben.
