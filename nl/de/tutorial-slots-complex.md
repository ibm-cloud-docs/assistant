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

# Lernprogramm: Einen Knoten mit Slots verbessern
{: #tutorial-slots-complex}

In diesem Lernprogramm verbessern Sie einen einfachen Knoten mit Slots zum Erfassen der erforderlichen Informationen, um einen Tisch in einem Restaurant zu reservieren.
{: shortdesc}

## Lernziele
{: #tutorial-slots-complex-objectives}

Sobald Sie dieses Lernprogramm abgeschlossen haben, wissen Sie, wie Sie Folgendes ausführen:

- Einen Knoten mit Slots testen
- Antwortbedingungen für Slots hinzufügen, die allgemeine Benutzerinteraktionen verarbeiten
- Nicht zugehörige Benutzereingaben einbeziehen und verarbeiten
- Nicht erwartete Benutzerantworten verarbeiten

### Dauer
{: #tutorial-slots-complex-duration}

Für dieses Lernprogramm benötigen Sie ungefähr zwei bis drei Stunden.

### Voraussetzung
{: #tutorial-slots-complex-prereqs}

Arbeiten Sie das Lernprogramm [Knoten mit Slots zu einem Dialogmodul hinzufügen](/docs/services/assistant?topic=assistant-tutorial-slots) durch, bevor Sie mit diesem Lernprogramm beginnen. Sie müssen die erste Lerneinheit für Slots abschließen, bevor Sie mit diesem Lernprogramm beginnen, da es den Knoten mit Slots verwendet, den Sie in der ersten Lerneinheit erstellen.

## Schritt 1: Das Format der Antworten optimieren
{: #tutorial-slots-complex-fix-format}

Die Werte der Systementitäten für Datum und Uhrzeit werden beim Speichern in ein Standardformat umgewandelt. Dieses Standardformat vereinfacht das Durchführen von Berechnungen für diese Werte. Der Umwandlungsvorgang soll jedoch für die Benutzer nicht sichtbar sein. In diesem Schritt ändern Sie das Format für das Datum (`2017-12-29`) und die Uhrzeit (`17:00:00`) die im Dialogmodul referenziert werden.

1.  Um den Wert der Kontextvariablen '$date' neu zu formatieren, klicken Sie auf das Symbol **Antwort bearbeiten** ![Antwort bearbeiten](images/edit-slot.png) für den Slot '@sys-date'.

1.  Wählen Sie im Menü **Mehr** ![Symbol 'Mehr'](images/kabob.png) die Option **JSON-Editor öffnen** aus und bearbeiten Sie anschließend den JSON-Code, der die Kontextvariable definiert. Fügen Sie eine Umwandlungsmethode für das Datum hinzu, die den Wert `2017-12-29` in den Namen des Wochentags, gefolgt von der Angabe des Monats und des Tages umwandelt. Bearbeiten Sie den JSON-Code wie folgt:

    ```json
    {
      "context": {
        "date": "<? @sys-date.reformatDateTime('EEEE, MMMM d') ?>"
      }
    }
    ```
    {: codeblock}

    'EEEE' gibt an, dass der Wochentag als Textzeichenfolge ausgedrückt werden soll. Wenn Sie 'EEE' (dreimal E) angeben, wird der Name des Tages z. B. von 'Freitag' auf 'Fre' abgekürzt. Die Angabe 'MMMM' bedeutet, dass der Monat als Textzeichenfolge ausgedrückt werden soll. Wenn Sie 'MMM' (dreimal M) angeben, wird der Monatsname z. B. 'Dezember' mit 'Dez' abgekürzt.

1.  Klicken Sie auf **Speichern**.

1.  Um das Format zum Speichern der Uhrzeit in der Kontextvariablen '$time' so zu ändern, dass Stunde, Minuten und AM oder PM angegeben werden, klicken Sie auf das Symbol **Antwort bearbeiten** ![Antwort bearbeiten](images/edit-slot.png) für den Slot '@sys-time'.

1.  Wählen Sie im Menü **Mehr** ![Symbol 'Mehr'](images/kabob.png) die Option **JSON-Editor öffnen** aus und bearbeiten Sie anschließend den JSON-Code, der die Kontextvariable definiert so, dass er wie folgt lautet: 

    ```json
    {
      "context": {
        "time": "<? @sys-time.reformatDateTime('h:mm a') ?>"
      }
    }
    ```
    {: codeblock}

1.  Klicken Sie auf **Speichern**.

1.  Testen Sie den Knoten erneut. Öffnen Sie die Anzeige 'Ausprobieren' und klicken Sie auf **Löschen**, um die Werte der Slot-Kontextvariablen zu löschen, die Sie beim vorherigen Testen des Knotens mit Slots angegeben hatten. Verwenden Sie das folgende Script, um die Wirkung Ihrer Änderungen anzuzeigen:

    <table>
    <caption>Scriptdetails</caption>
    <tr>
      <th>Sprecher</th>
      <th>Äußerung</th>
    </tr>
    <tr>
      <td>Sie</td>
      <td>Ich möchte einen Tisch reservieren.</td>
    </tr>
    <tr>
      <td>Watson</td>
      <td>An welchem Tag möchten Sie reservieren?</td>
    </tr>
    <tr>
      <td>Sie</td>
      <td>Freitag</td>
    </tr>
    <tr>
      <td>Watson</td>
      <td>Für welche Uhrzeit möchten Sie reservieren?</td>
    </tr>
    <tr>
      <td>Sie</td>
      <td>17:00 Uhr</td>
    </tr>
    <tr>
      <td>Watson</td>
      <td>Wie viele Personen werden zum Essen kommen?</td>
    </tr>
    <tr>
      <td>Sie</td>
      <td>6 Personen</td>
    </tr>
    </table>

    An dieser Stelle antwortet Watson mit `OK. Ich erstelle Ihre Reservierung für 6 Personen am Freitag, 29. Dezember, um 17:00 Uhr.`

Sie haben das Format des Dialogmoduls beim Verweisen auf Kontextvariablenwerte in den Dialogantworten erfolgreich verbessert. Im Dialogmodul wird jetzt `Freitag, 29. Dezember` anstelle des technischen Ausdrucks `2017-12-29` verwendet. Außerdem wird als Uhrzeit nicht `5:00 Uhr nachmittags` angegeben, sondern `17:00 Uhr`. Informationen zu weiteren SpEL-Methoden, die für Datums- und Uhrzeitwerte verwendet werden können, finden Sie unter [Verarbeitungsmethoden für Werte](/docs/services/assistant?topic=assistant-dialog-methods#dialog-methods-date-time).

## Schritt 2: Alle Angaben auf einmal abfragen
{: #tutorial-slots-complex-ask-for-everything}

Beim Testen des Dialogmoduls dürfte Ihnen aufgefallen sein, dass es lästig werden kann, die Slotabfragen einzeln nacheinander zu beantworten. Damit die Benutzer die einzelnen Informationen nicht separat bereitstellen müssen, können Sie die Informationen vorab gesammelt abfragen. Dadurch bekommt der Benutzer die Möglichkeit, mehrere oder alle benötigten Informationen auf einmal einzugeben.

Der Knoten mit Slots wird so konzipiert, dass alle Slotwerte lokalisiert und gespeichert werden, die der Benutzer während der Verarbeitung des aktuellen Knotens angibt. Sie können die Benutzer unterstützen, dieses Konzept zu ihrem Vorteil zu nutzen, indem Sie darauf hinweisen, welche Werte angegeben werden sollten.

In diesem Schritt erfahren Sie, wie alle Angaben auf einmal abgefragt werden können.

1.  Klicken Sie im Hauptknoten mit Slots auf **Anpassen**.

1.  Wählen Sie das Kontrollkästchen **Alles abfragen** aus, um die Anfangsabfrage zu aktivieren, und klicken Sie dann auf **Anwenden**.

   ![Zeigt den Anpassungsdialog, in dem Sie das Kontrollkästchen 'Alles abfragen' auswählen.](images/slots-prompt-for-everything.png)

1.  Kehren Sie zurück in die Ansicht für Knotenbearbeitung und blättern Sie abwärts zu dem neu hinzugefügten Feld **Zuerst diese Frage ausgeben, falls noch keine Slots gefüllt sind**. Fügen Sie die folgende Anfangsabfrage für den Knoten hinzu: `Ich kann eine Reservierung für Sie vornehmen. Sagen Sie mir nur, an welchem Tag, um welche Zeit und für wie viele Personen Sie reservieren möchten.`

1.  Klicken Sie auf ![Schließen](images/close.png), um die Bearbeitungsansicht für den Knoten zu schließen.

1.  Testen Sie diese Änderung in der Anzeige 'Ausprobieren'. Öffnen Sie die Anzeige und klicken Sie dann auf **Löschen**, um die Werte der Slot-Kontextvariablen aus dem vorherigen Test zurückzusetzen.

1.  Geben Sie Folgendes ein: `Ich möchte einen Tisch reservieren.`

    Das Dialogmodul antwortet jetzt mit `Ich kann eine Reservierung für Sie vornehmen. Sagen Sie mir nur, an welchem Tag, um welche Zeit und für wie viele Personen Sie reservieren möchten.`

1.  Geben Sie Folgendes ein: `Die Reservierung ist für Samstag. Wir kommen zu zweit um 20:00 Uhr.`

    Das Dialogmodul antwortet mit: `OK. Ich erstelle Ihre Reservierung für 2 Personen am Samstag um 20:00 Uhr.`

    ![Zeigt die Anzeige 'Ausprobieren', wenn der Benutzer alle Angaben in einer einzigen Eingabe bereitstellt.](images/slots-everything-tested.png)

Wenn der Benutzer einen der Slotwerte schon in der ersten Eingabe angibt, wird die Abfrage für die Einzelinformatioen nicht angezeigt. Angenommen, die Anfangseingabe des Benutzers lautet wie folgt: `Ich möchte für diesen Freitagabend reservieren.` In diesem Fall wird die Anfangsabfrage ausgelassen, da bereits angegebene Informationen (in diesem Beispiel der Tag `Freitag`) nicht erneut abgefragt werden müssen. Das Dialogmodul zeigt stattdessen die Abfrage für den nächsten leeren Slot an.
{: note}

## Schritt 3: Nullen ordnungsgemäß behandeln
{: #tutorial-slots-complex-recognize-zero}

Wenn Sie die Systementität `sys-number` in einer Slotbedingung verwenden, werden Nullen nicht ordnungsgemäß behandelt. Anstatt die Kontextvariable, die Sie für den Slot definieren, auf 0 zu setzen, setzt der Service die Kontextvariable auf 'false'. Daher wird der Slot nicht als gefüllt eingestuft und der Benutzer wird wiederholt aufgefordert, eine Zahl einzugeben, bis der Benutzer eine andere Zahl als Null angibt.

1.  Testen Sie den Knoten, damit Sie das Problem besser verstehen können. Öffnen Sie die Anzeige 'Ausprobieren' und klicken Sie auf **Löschen**, um die Werte der Slot-Kontextvariablen zu löschen, die Sie beim vorherigen Testen des Knotens mit Slots angegeben hatten. Verwenden Sie das folgende Script:

    <table>
    <caption>Scriptdetails</caption>
    <tr>
      <th>Sprecher</th>
      <th>Äußerung</th>
    </tr>
    <tr>
      <td>Sie</td>
      <td>Ich möchte einen Tisch reservieren.</td>
    </tr>
    <tr>
      <td>Watson</td>
      <td>Ich kann eine Reservierung für Sie vornehmen. Sagen Sie mir nur, an welchem Tag, um welche Zeit und für wie viele Personen Sie reservieren möchten.</td>
    </tr>
    <tr>
      <td>Sie</td>
      <td>Wir möchten am 23. Mai um 20 Uhr zu Abend essen. Es werden 0 Gäste sein.</td>
    </tr>
    <tr>
      <td>Watson</td>
      <td>Wie viele Personen werden zum Essen kommen?</td>
    </tr>
    <tr>
      <td>Sie</td>
      <td>0</td>
    </tr>
    <tr>
      <td>Watson</td>
      <td>Wie viele Personen werden zum Essen kommen?</td>
    </tr>
    </table>

    Diese Schleife wird wiederholt, bis Sie eine andere Zahl als 0 angeben.

1.  Um sicherzustellen, dass Nullen in dem Slot ordnungsgemäß behandelt werden, ändern Sie die Slotbedingung von `@sys-number` in `@sys-number || @sys-number:0`.

1.  Klicken Sie auf das Symbol **Antwort bearbeiten** ![Antwort bearbeiten](images/edit-slot.png) für den Slot.

1.  Wenn die Kontextvariable erstellt wird, verwendet sie automatisch denselben Ausdruck, der für die Slotbedingung angegeben ist. Die Kontextvariable muss jedoch nur eine Zahl speichern. Bearbeiten Sie den Wert, der als Kontextvariable gespeichert wurde, um den Operator `OR` aus dem Wert zu entfernen. Wählen Sie im Menü **Mehr** ![Symbol 'Mehr'](images/kabob.png) die Option **JSON-Editor öffnen** aus und bearbeiten Sie anschließend den JSON-Code, der die Kontextvariable definiert. Ändern Sie den Variablenwert von `"guests":"@sys-number || @sys-number:0"` so, dass die folgende Syntax verwendet wird: 

    ```json
    {
      "context": {
        "guests": "@sys-number"
      }
    }
    ```
    {: codeblock}

1.  Klicken Sie auf **Speichern**.

1.  Testen Sie den Knoten erneut. Öffnen Sie die Anzeige 'Ausprobieren' und klicken Sie auf **Löschen**, um die Werte der Slot-Kontextvariablen zu löschen, die Sie beim vorherigen Testen des Knotens mit Slots angegeben hatten. Verwenden Sie das folgende Script, um die Wirkung Ihrer Änderungen anzuzeigen:

    <table>
    <caption>Scriptdetails</caption>
    <tr>
      <th>Sprecher</th>
      <th>Äußerung</th>
    </tr>
    <tr>
      <td>Sie</td>
      <td>Ich möchte einen Tisch reservieren.</td>
    </tr>
    <tr>
      <td>Watson</td>
      <td>Ich kann eine Reservierung für Sie vornehmen. Sagen Sie mir nur, an welchem Tag, um welche Zeit und für wie viele Personen Sie reservieren möchten.</td>
    </tr>
    <tr>
      <td>Sie</td>
      <td>Wir möchten am 23. Mai um 20 Uhr zu Abend essen. Es werden 0 Gäste sein.</td>
    </tr>
    </table>

    An dieser Stelle antwortet Watson mit `OK. Ich erstelle eine Reservierung für 0 Personen am Mittwoch, 23. Mai um 20:00 Uhr.`

Sie haben den Slot 'number' erfolgreich so formatiert, dass Nullen ordnungsgemäß behandelt werden. Natürlich kann es gewünscht sein, dass der Knoten eine Null nicht als gültigen Wert für die Gästeanzahl akzeptieren soll. Die Vorgehensweise zum Überprüfen der von Benutzern angegebenen Werte wird im nächsten Schritt erläutert. 

## Schritt 4: Benutzereingabe überprüfen
{: #tutorial-slots-complex-slot-conditions}

Bislang wurde angenommen, dass der Benutzer die entsprechenden Werttypen für die Slots angibt. In der Realität trifft dies nicht immer zu. Um auf ungültige Eingabewerte des Benutzers zu reagieren, können Sie bedingte Antworten für Slots hinzufügen. In diesem Schritt verwenden Sie bedingte Slotantworten für die folgenden Aufgaben:

- Sicherstellen, dass das angefragte Datum nicht in der Vergangenheit liegt
- Prüfen, ob die angefragte Uhrzeit im Zeitfenster für Tischreservierungen liegt
- Die Eingabe des Benutzers bestätigen
- Sicherstellen, dass die angegebene Gästeanzahl größer als null ist.
- Angeben, dass ein Wert durch einen anderen ersetzt wird

So überprüfen Sie die Eingabe des Benutzers:

1.  Klicken Sie in der Bearbeitungsansicht des Knotens mit Slots auf das Symbol **Slot bearbeiten** ![Slot bearbeiten](images/edit-slot.png) für den Slot `@sys-date`.

1.  Wählen Sie im Menü **Optionen** ![Symbol 'Mehr'](images/kabob.png) im Header *Slot 1 konfigurieren* die Option **Bedingte Antworten aktivieren** aus.

1.  Fügen Sie im Abschnitt **Gefunden** eine bedingte Antwort hinzu, indem Sie auf das Symbol **Antwort bearbeiten** ![Antwort bearbeiten](images/edit-slot.png) klicken.

1.  Fügen Sie die folgende Bedingung und Antwort hinzu, um zu prüfen, ob das vom Benutzer angegebene Datum vor dem heutigen Datum liegt:

    <table>
    <caption>Slot 1 - Details für bedingte Antwort 1</caption>
    <tr>
      <th>Bedingung</th>
      <th>Antwort</th>
      <th>Aktion</th>
    </tr>
    <tr>
      <td>`@sys-date.before(now())`</td>
      <td>Für einen bereits vergangenen Tag ist keine Reservierung möglich.</td>
      <td>Slot löschen und Abfrage wiederholen</td>
    </tr>
    </table>

1.  Fügen Sie eine zweite bedingte Antwort hinzu, die angezeigt wird, wenn der Benutzer ein zulässiges Datum angibt. Diese einfache Bestätigung zeigt dem Benutzer, dass die Eingabe verstanden wurde.

    <table>
    <caption>Slot 1 - Details für bedingte Antwort 2</caption>
    <tr>
      <th>Bedingung</th>
      <th>Antwort</th>
      <th>Aktion</th>
    </tr>
    <tr>
      <td>`true`</td>
      <td>Das Datum ist $date</td>
      <td>Fortfahren</td>
    </tr>
    </table>

1.  Klicken Sie in der Bearbeitungsansicht des Knotens mit Slots auf das Symbol **Slot bearbeiten** ![Slot bearbeiten](images/edit-slot.png) für den Slot `@sys-time`.

1.  Wählen Sie im Menü **Optionen** ![Symbol 'Mehr'](images/kabob.png) im Header *Slot 2 konfigurieren* die Option **Bedingte Antworten aktivieren** aus.

1.  Fügen Sie im Abschnitt **Gefunden** eine bedingte Antwort hinzu, indem Sie auf das Symbol **Antwort bearbeiten** ![Antwort bearbeiten](images/edit-slot.png) klicken.

1.  Fügen Sie die folgenden Bedingungen und Antworten hinzu, um zu prüfen, ob die vom Benutzer angegebene Uhrzeit im zulässigen Zeitfenster liegt:

    <table>
    <caption>Slot 2 - bedingte Antwort - Details</caption>
    <tr>
      <th>Bedingung</th>
      <th>Antwort</th>
      <th>Aktion</th>
    </tr>
    <tr>
      <td>`@sys-time.after('21:00:00')`</td>
      <td>Unsere späteste Reservierungszeit ist 21:00 Uhr.</td>
      <td>Slot löschen und Abfrage wiederholen</td>
    </tr>
    <tr>
      <td>`@sys-time.before('09:00:00')`</td>
      <td>Unsere früheste Reservierungszeit ist 9:00 Uhr.</td>
      <td>Slot löschen und Abfrage wiederholen</td>
    </tr>
    </table>

1.  Fügen Sie eine dritte bedingte Antwort hinzu, die angezeigt wird, wenn der Benutzer eine gültige Uhrzeit angibt, die in dem zulässigen Zeitfenster liegt. Diese einfache Bestätigung zeigt dem Benutzer, dass die Eingabe verstanden wurde.

    <table>
    <caption>Slot 2 - Details für bedingte Antwort 3</caption>
    <tr>
      <th>Bedingung</th>
      <th>Antwort</th>
      <th>Aktion</th>
    </tr>
    <tr>
      <td>`true`</td>
      <td>OK. Sie möchten für $time reservieren.</td>
      <td>Fortfahren</td>
    </tr>
    </table>

1.  Bearbeiten Sie den Slot '@sys-number', um den vom Benutzer angegebenen Wert wie folgt zu überprüfen:

    - Überprüfen, dass die angegebene Gästeanzahl größer als null ist.
    - Die Möglichkeit berücksichtigen, dass der Benutzer die Gästeanzahl ändern möchte.

      Wenn der Benutzer während der Verarbeitung des aktuellen Knotens mit Slots einen Slotwert ändert, wird die zugehörige Slotkontextvariable aktualisiert. Es kann hilfreich sein, dem Benutzer mitzuteilen, dass der Wert ersetzt wird. So erhält der Benutzer nicht nur eine Rückmeldung über die Änderung, sondern kann gegebenenfalls die Angabe korrigieren, wenn sie nicht seinen Wünschen entspricht. 

1.  Klicken Sie in der Bearbeitungsansicht des Knotens mit Slots auf das Symbol **Slot bearbeiten** ![Slot bearbeiten](images/edit-slot.png) für den Slot `@sys-number`.

1.  Wählen Sie im Menü **Optionen** ![Symbol 'Mehr'](images/kabob.png) im Header *Slot 3 konfigurieren* die Option **Bedingte Antworten aktivieren** aus.

1.  Fügen Sie im Abschnitt **Gefunden** eine bedingte Antwort hinzu, indem Sie auf das Symbol ![Antwort bearbeiten](images/edit-slot.png) klicken und die folgende Bedingung und Antwort hinzufügen:

    <table>
    <caption>Slot 3 - Details für bedingte Antwort</caption>
    <tr>
      <th>Bedingung</th>
      <th>Antwort</th>
      <th>Aktion</th>
    </tr>
    <tr>
      <td>`entities['sys-number']?.value == 0`</td>
      <td>Geben Sie eine Zahl an, die größer als 0 ist.</td>
      <td>Slot löschen und Abfrage wiederholen</td>
    </tr>
    <tr>
      <td>`(event.previous_value != null) && (event.previous_value != event.current_value)`</td>
      <td>OK, die Anzahl der Gäste wird von `<? event.previous_value ?>` in `<? event.current_value ?>`.</td>
      <td>Fortfahren</td>
    </tr>
    <tr>
      <td>`true`</td>
      <td>OK, die Reservierung gilt für $guests Gäste.</td>
      <td>Fortfahren</td>
    </tr>
    </table>

## Schritt 5: Slot für Bestätigung hinzufügen
{: #tutorial-slots-complex-confirmation-slot}

Sie können das Dialogmodul so gestalten, dass ein externes Reservierungssystem aufgerufen und ein Termin für den Benutzer in dem System reserviert wird. Bevor Ihre Anwendung diesen Reservierungsschritt ausführt, sollte der Benutzer noch bestätigen, dass die Details für die Reservierung im Dialogmodul korrekt erfasst wurden. Zu diesem Zweck können Sie einen Bestätigungsslot im Knoten hinzufügen.

1.  Der Bestätigungsslot erwartet vom Benutzer die Antwort 'Ja' oder 'Nein'. Sie müssen das Dialogmodul in die Lage versetzen, die Aussage 'Ja' oder 'Nein' in der Benutzereingabe zu erkennen.

1.  Klicken Sie auf die Registerkarte **Absichten**, um zur Seite 'Absichten' zurückzukehren. Fügen Sie die folgenden Beispieläußerungen für Absichten hinzu:

- `#yes`

   ```json
   Ja
   Genau
   So soll es sein
   Das stimmt so
   In Ordnung
   OK
   Das klingt gut
   ```
   {: screen}

   ![Zeigt die zustimmende Absicht](images/slots-yes-intent.png)

- `#no`

   ```json
   Nein
   Nein danke
   So nicht
   Auf keinen Fall
   Das ist nicht richtig
   Ganz und gar nicht
   Unmöglich
   ```
   {: screen}

   ![Zeigt die ablehnende Absicht](images/slots-no-intent.png)

1.  Kehren Sie zur Registerkarte **Dialogmodul** zurück und klicken Sie auf die Option zum Bearbeiten des Knotens mit Slots. Klicken Sie auf **Slot hinzufügen**, um einen vierten Slot hinzuzufügen, und geben Sie folgende Werte für den neuen Slot an:

    <table>
    <caption>Details für Bestätigungsslot</caption>
    <tr>
      <th>Überprüfen auf</th>
      <th>Speichern unter</th>
      <th>Anfragen, falls nicht vorhanden</th>
    </tr>
    <tr>
      <td>`(#yes || #no) && slot_in_focus`</td>
      <td>$confirmation</td>
      <td>Ich reserviere Ihnen einen Tisch für $guests am $date um $time. Soll ich fortfahren?</td>
    </tr>
    </table>

    Diese Bedingung lässt beide Antworten zu. Sie legen durch bedingte Slotantworten fest, wie es weiter geht, wenn der Benutzer mit Ja oder Nein antwortet. Die Eigenschaft `slot_in_focus` legt fest, dass diese Bedingung nur auf den aktuellen Slot angewendet werden kann. Diese Einstellung verhindert, dass zufällige Aussagen des Benutzers, die der Absicht `#yes` oder `#no` entsprechen könnten, diesen Slot auslösen.

    Beispiel: Der Benutzer gibt im Slot für die Anzahl der Gäste eine Antwort ähnlich der folgenden an: `Ja, wir kommen zu fünft`. Sie möchten verhindern, dass das Wort `Ja` aus dieser Antwort irrtümlich dem Bestätigungsslot zugeordnet wird. Wenn Sie die Eigenschaft `slot_in_focus` zu der Bedingung hinzufügen, wird eine zustimmende oder ablehnende Äußerung des Benutzers ausschließlich dem betreffenden Slot zugeordnet, während der Benutzer gezielt auf die Abfrage für diesen Slot antwortet.

1.  Klicken Sie auf das Symbol **Slot bearbeiten** ![Slot bearbeiten](images/edit-slot.png). Wählen Sie im Menü **Optionen** ![Symbol 'Mehr'](images/kabob.png) im Header *Slot 4 konfigurieren* die Option **Bedingte Antworten aktivieren** aus.

1.  Geben Sie in der Abfrage für **Gefunden** eine Bedingung hinzu, die überprüft, ob eine ablehnende Antwort (`#no`) gegeben wurde. Verwenden Sie die folgende Antwort: `In Ordnung. Nochmal von vorne. Dieses Mal versuche ich, dran zu bleiben.` Andernfalls können Sie davon ausgehen, dass die Reservierungsdetails vom Benutzer bestätigt wurden, und die Reservierung veranlassen.

    Wenn die Absicht `#no` gefunden wird, müssen Sie die zuletzt gespeicherten Kontextvariablen ebenfalls auf null zurücksetzen, damit Sie erneut Informationen abfragen können. Sie können die Werte der Kontextvariablen mit dem JSON-Editor zurücksetzen. Klicken Sie auf das Symbol **Antwort bearbeiten** ![Antwort bearbeiten](images/edit-slot.png) für die bedingte Antwort, die Sie soeben hinzugefügt haben. Klicken Sie im Menü **Optionen**  ![Erweiterte Antwort](images/kabob.png) auf die Option **JSON-Editor öffnen**. Fügen Sie einen Block `context` hinzu, der die Kontextvariablen des Slots auf `null` setzt, wie unten gezeigt.

    ```json
    {
      "output":{
        "text": {
          "values": [
            "In Ordnung. Nochmal von vorne. Dieses Mal versuche ich, dran zu bleiben."
          ]
        }
      },
  "context":{
        "date": null,
        "time": null,
        "guests": null
      }
    }
    ```
    {: codeblock}

1.  Klicken Sie auf **Zurück** und dann auf **Speichern**.

1.  Klicken Sie erneut auf das Symbol **Slot bearbeiten** ![Slot bearbeiten](images/edit-slot.png) für den Bestätigungsslot. Machen Sie in der Abfrage für **Nicht gefunden** deutlich, dass Sie vom Benutzer eine zustimmende oder ablehnende Antwort (Ja oder Nein) erwarten. Fügen Sie eine Antwort mit den folgenden Werten hinzu.

    <table>
    <caption>Details der Antwort für 'Nicht gefunden'</caption>
    <tr>
      <th>Bedingung</th>
      <th>Antwort</th>
    </tr>
    <tr>
      <td>`true`</td>
      <td>Antworten Sie mit 'Ja', um zu bestätigen, dass die Reservierung so durchgeführt werden soll, oder mit 'Nein', wenn sie nicht durchgeführt werden soll.</td>
    </tr>
    </table>

1.  Klicken Sie auf **Speichern**.

1.  Sie haben nun Bestätigungsantworten für Slotwerte eingerichtet und fragen nach allen Reservierungsdetails auf einmal. Möglicherweise stellen Sie fest, dass die Antworten für die einzelnen Slots vor den Antwort für den Bestätigungsslot angezeigt werden. Für den Benutzer kann dies wie eine unnötige Wiederholung aussehen. Bearbeiten Sie die Antworten für den Slot 'Gefunden' so, dass sie unter bestimmten Bedingungen nicht angezeigt werden.

1.  Ersetzen Sie die im JSON-Snippet angegebene Bedingung `true` für die letzte bedingte Antwort im Slot '@sys-date' durch `!($time && $guests)`. Beispiel:

    <table>
    <caption>Slot 1 - Details für bedingte Antwort 2</caption>
    <tr>
      <th>Bedingung</th>
      <th>Antwort</th>
      <th>Aktion</th>
    </tr>
    <tr>
      <td>`!($time && $guests)`</td>
      <td>Das Datum ist $date</td>
      <td>Fortfahren</td>
    </tr>
    </table>

1.  Ersetzen Sie die im JSON-Snippet angegebene Bedingung `true` für die letzte bedingte Antwort im Slot '@sys-time' durch `!($date && $guests)`. Beispiel:

    <table>
    <caption>Slot 2 - Details für bedingte Antwort 3</caption>
    <tr>
      <th>Bedingung</th>
      <th>Antwort</th>
      <th>Aktion</th>
    </tr>
    <tr>
      <td>`!($date && $guests)`</td>
      <td>OK. Sie möchten für $time reservieren.</td>
      <td>Fortfahren</td>
    </tr>
    </table>

1.  Ersetzen Sie die im JSON-Snippet angegebene Bedingung `true` für die letzte bedingte Antwort im Slot '@sys-number' durch `!($date && $time)`. Beispiel:

    <table>
    <caption>Slot 3 - Details für bedingte Antwort 2</caption>
    <tr>
      <th>Bedingung</th>
      <th>Antwort</th>
      <th>Aktion</th>
    </tr>
    <tr>
      <td>`!($date && $time)`</td>
      <td>OK, die Reservierung gilt für $guests Gäste.</td>
      <td>Fortfahren</td>
    </tr>
    </table>

Wenn Sie später weitere Slots hinzufügen, müssen Sie diese Bedingungen anpassen, um die zugehörigen Kontextvariablen für die zusätzlichen Slots zu berücksichtigen. Wenn Sie keinen Bestätigungsslot einbeziehen, können Sie nur `!all_slots_filled` angeben. Diese Angabe bleibt gültig, unabhängig davon, wie viele Slots Sie später hinzufügen.

## Schritt 6: Werte der Slotkontextvariablen zurücksetzen
{: #tutorial-slots-complex-reset-variables}

Wie bereits erläutert, müssen Sie vor jedem Test die Werte der bei dem vorherigen Test erstellten Kontextvariablen löschen. Dies ist erforderlich, da der Knoten mit Slots nur dann Benutzereingaben abfragt, wenn noch keine entsprechenden Informationen vorliegen. Wenn die Slotkontextvariablen bereits gültige Werte enthalten, werden keine Anfragen angezeigt. Dies gilt auch für das Dialogmodul während der Laufzeit. Sie müssen im Dialogmodul eine Methode zum Zurücksetzen der Slotkontextvariablen vorsehen, damit die Slots vom nächsten Benutzer neu gefüllt werden können. Zu diesem Zweck fügen Sie für den Knoten mit Slots einen übergeordneten Knoten hinzu, der die Kontextvariablen auf null setzt.

1.  Klicken Sie in der Baumstrukturansicht des Dialogmoduls auf das Symbol **Mehr** ![Symbol 'Mehr'](images/kabob.png) für den Knoten mit Slots und wählen Sie dann **Knoten darüber hinzufügen** aus.

1.  Geben Sie `#reservation` als Bedingung für den neuen Knoten an. (Dieselbe Bedingung wird auch für den Knoten mit Slots verwendet; die Bedingung für den Knoten mit Slots wird jedoch im weiteren Verlauf dieser Prozedur geändert.)

1.  Klicken Sie auf das Symbol **Optionen** ![Symbol 'Mehr'](images/kabob.png) neben der Antwort des Knotens und klicken Sie dann auf **JSON-Editor öffnen**. Fügen Sie einen Eintrag für jede Slotkontextvariable hinzu, die Sie in dem Knoten mit Slots definiert haben, und setzen Sie sie auf den Wert `null`.

    ```json
    {
      "context": {
        "date": null,
        "time": null,
        "guests": null,
        "confirmation": null
      },
      "output": {}
    }
    ```
    {: codeblock}

    ![Zeigt die Baumansicht des Dialogmoduls mit zwei bedingten Knoten '#reservation'. Der erste Knoten setzt die Slotkontextvariablen auf null.](images/slots-adding-parent.png)

1.  Klicken Sie auf die Option zum Bearbeiten des Knotens '#reservation', den Sie zuvor erstellt und dem Sie die Slots hinzugefügt haben.

1.  Ändern Sie die Knotenbedingung von `#reservation` in `($date == null && $time == null)` und schließen Sie dann die Ansicht für Knotenbearbeitung durch Klicken auf ![Schließen](images/close.png).

1.  Klicken Sie auf das Symbol **Mehr** ![Symbol 'Mehr'](images/kabob.png) für den Knoten mit Slots und wählen Sie dann **Verschieben** aus.

    ![Zeigt die Baumstruktur des Dialogmoduls an. Der Benutzer klickt auf die Aktion 'Verschieben' für den Knoten mit Slots.](images/slots-move-node.png)

1.  Wählen Sie den Knoten `#reservation` als Ziel für die Verschiebung aus und wählen Sie dann im Menü **Als untergeordneter Knoten** aus.

1.  Klicken Sie auf die Option zum Bearbeiten des Knotens `#reservation`. Ändern Sie im Abschnitt *Abschließend* die Aktion von *Benutzereingabe abwarten* in **Benutzereingabe überspringen**.

    ![Zeigt das angepasste Dialogmodul mit einem Stammknoten mit der Bedingung '#reservation' und einer Sprungaktion direkt zum zugehörigen untergeordneten Knoten mit Slots.](images/slots-skip-user-input.png)

    Wenn eine Benutzereingabe mit der Absicht `#reservation` übereinstimmt, wird dieser Knoten ausgelöst. Alle Slotkontextvariablen werden auf null gesetzt und danach springt das Dialogmodul direkt zu dem Knoten mit Slots, um ihn zu verarbeiten.

## Schritt 7: Benutzern die Möglichkeit zum Beenden des Prozesses geben
{: #tutorial-slots-complex-handler}

Das Hinzufügen eines Knotens mit Slots ist ein leistungsfähiges Werkzeug, um die Benutzereingaben anzufordern, die Sie benötigen, um eine sinnvolle Antwort an die Benutzer zurückzugeben oder eine Aktion im Namen der Benutzer auszuführen. Es kann jedoch vorkommen, dass der Benutzer bei der Eingabe von Reservierungsdetails beschließt, keine Reservierung vorzunehmen, Sie müssen eine Möglichkeit zum schnellen und sauberen Beenden des Prozesses bereitstellen. Zu diesem Zweck können Sie einen Slot-Handler hinzufügen, der erkennen kann, wenn der Benutzer den Prozess beenden und den Knoten verlassen möchte, ohne die erfassten Werte zu speichern.

1.  Zunächst müssen Sie das Dialogmodul in die Lage versetzen, die Beendigungsabsicht (#exit) in der Benutzereingabe zu erkennen.

1.  Klicken Sie auf die Registerkarte **Absichten**, um zur Seite 'Absichten' zurückzukehren. Fügen Sie die Absicht '#exit' mit den folgenden Beispieläußerungen hinzu.

    ```json
    Ich möchte den Vorgang stoppen
    Beenden!
    Diesen Prozess abbrechen
    Ich hab's mir anders überlegt. Ich möchte keinen Tisch reservieren.
    Reservierung stoppen
    Halt, bitte abbrechen.
    Lassen wir das.
    ```
    {: screen}

    ![Zeigt die Absicht zum Beenden an](images/slots-exit-intent.png)

1.  Kehren Sie zum Dialogmodul zurück, indem Sie auf die Registerkarte **Dialog** klicken. Klicken Sie auf den Knoten mit Slots, um ihn zu öffnen, und klicken Sie dann auf **Handler verwalten**.

    ![Zeigt den Link 'Handler verwalten' für den Knoten mit Slots](images/slots-manage-handlers.png)

1.  Fügen Sie die folgenden Werte in den Feldern hinzu.

    <table>
    <caption>Details für Handler auf Knotenebene</caption>
    <tr>
      <th>Bedingung</th>
      <th>Antwort</th>
      <th>Aktion</th>
    </tr>
    <tr>
      <td>`#exit`</td>
      <td>Gut, wir hören hier auf. Es wird kein Tisch reserviert.</td>
      <td>Zur Antwort springen</td>
    </tr>
    </table>

    Die Aktion **Zur Antwort springen** springt direkt zur Antwort auf Knotenebene, ohne Abfragen für die weiteren, nicht gefüllten Slots anzuzeigen.

1.  Klicken Sie auf **Zurück** und dann auf **Speichern**.

1.  Als Nächstes müssen Sie die Antwort auf Knotenebene bearbeiten, damit erkannt werden kann, wenn ein Benutzer den Prozess beenden möchte, ohne eine Reservierung vorzunehmen. Fügen Sie eine bedingte Antwort für den Knoten hinzu.

    Klicken Sie in der Bearbeitungsansicht des Knotens mit Slots auf **Anpassen** und dann auf das Umschaltsteuerelement **Mehrere Antworten**, um es auf **Ein** zu setzen. Klicken Sie dann auf **Anwenden**.

    ![Zeigt das eingeschaltete Umschaltsteuerelement für mehrere Antworten](images/slots-multi-responses.png)

1.  Blättern Sie abwärts zum Antwortabschnitt für den Knoten mit Slots und klicken Sie dann auf **Antwort hinzufügen**.

1.  Fügen Sie die folgenden Werte in den Feldern hinzu.

    <table>
    <caption>Details für bedingte Antwort auf Knotenebene</caption>
    <tr>
      <th>Bedingung</th>
      <th>Antwort</th>
    </tr>
    <tr>
      <td>`has_skipped_slots`</td>
      <td>Ich hoffe, Ihnen bald wieder bei einer Reservierung helfen zu können. Einen schönen Tag noch.</td>
    </tr>
    </table>

    Die Bedingung `has_skipped_slots` prüft die Eigenschaften des Knotens mit Slots und stellt fest, ob Slots übersprungen wurden. Der Handler `#exit` lässt alle übrigen Slots aus und springt direkt zur Antwort des Knotens. Dies bedeutet: Wenn die Eigenschaft `has_skipped_slots` vorhanden ist, wurde die Absicht `#exit` ausgelöst und das Dialogmodul kann eine andere Antwort anzeigen.

    Wenn Sie mehr als einen Slot konfigurieren, um andere Slots zu überspringen, oder wenn sie einen anderen Ereignishandler auf Knotenebene konfigurieren, der Slots überspringt, dann müssen Sie auf andere Weise überprüfen, ob die Absicht '#exit' ausgelöst wurde. Informationen zu einer anderen Prüfmethode finden Sie unter [Anforderungen zur Prozessbeendigung verarbeiten](/docs/services/assistant?topic=assistant-dialog-slots#dialog-slots-node-level-handler).
    {: note}

1.  Der Service soll prüfen, ob die Eigenschaft `has_skipped_slots` vorhanden ist, bevor die Standardantwort auf Knotenebene angezeigt wird. Verschieben Sie die bedingte Antwort `has_skipped_slots` nach oben, sodass sie vor der ursprünglichen bedingten Antwort verarbeitet wird (andernfalls würde sie nie ausgelöst). Klicken Sie zu diesem Zweck auf die Antwort, die Sie gerade hinzugefügt haben und versetzen Sie sie mit dem **Aufwärtspfeil** nach oben. Klicken Sie dann auf **Speichern**.

1.  Testen Sie diese Änderung mit dem folgenden Script in der Anzeige 'Ausprobieren'.

    <table>
    <caption>Scriptdetails</caption>
    <tr>
      <th>Sprecher</th>
      <th>Äußerung</th>
    </tr>
    <tr>
      <td>Sie</td>
      <td>Ich möchte einen Tisch reservieren.</td>
    </tr>
    <tr>
      <td>Watson</td>
      <td>Ich kann eine Reservierung für Sie vornehmen. Sagen Sie mir nur, an welchem Tag, um welche Zeit und für wie viele Personen Sie reservieren möchten.</td>
    </tr>
    <tr>
      <td>Sie</td>
      <td>Für 5 Personen</td>
    </tr>
    <tr>
      <td>Watson</td>
      <td>OK, die Reservierung gilt für 5 Gäste.  An welchem Tag möchten Sie reservieren?</td>
    </tr>
    <tr>
      <td>Sie</td>
      <td>Lassen wir das</td>
    </tr>
    <tr>
      <td>Watson</td>
      <td>Gut, wir hören hier auf. Es wird kein Tisch reserviert.  Ich hoffe, Ihnen bald wieder bei einer Reservierung helfen zu können. Einen schönen Tag noch.</td>
    </tr>
    </table>

## Schritt 8: Gültigen Wert angeben, wenn der Benutzer dies mehrmals vergeblich versucht hat

Es kann vorkommen, dass ein Benutzer nicht versteht, wonach Sie fragen. Der Benutzer gibt als Antwort immer wieder ungültige Werte ein. Um diese Möglichkeit einzubeziehen, können Sie einen Zähler für den Slot hinzufügen und nach 3 Fehlversuchen des Benutzers einen gültigen Wert angeben, damit der Vorgang fortgesetzt werden kann.

Für die Zeitangabe '$time' definieren Sie eine Folgenachricht, die angezeigt wird, wenn der Benutzer keine gültige Uhrzeit eingibt.

1.  Erstellen Sie eine Kontextvariable, die erfassen kann, wie oft der Benutzer einen Wert angibt, der nicht den vom Slot erwarteten Werttyp aufweist. Damit die Kontextvariable initialisiert und auf 0 gesetzt wird, bevor der Knoten mit Slots verarbeitet wird, fügen Sie die Kontextvariable zu dem übergeordneten Knoten `#reservation` hinzu.

1.  Klicken Sie auf die Option zum Bearbeiten des Knotens `#reservation`. Öffnen Sie den zugehörigen JSON-Editor für die Antwort des Knotens, indem Sie auf das Symbol **Optionen** ![Symbol 'Mehr'](images/kabob.png) im Antwortabschnitt klicken und die Option **JSON-Editor öffnen** auswählen. Fügen Sie eine Kontextvariable namens `counter` am Ende des vorhandenen Blocks `"context"` unterhalb der Variablen `confirmation` hinzu. Setzen Sie die Variable `counter` auf den Wert `0`.

       ```json
       {
         "context": {
           "date": null,
           "time": null,
           "guests": null,
           "confirmation": null,
           "counter": 0
         },
         "output": {}
       }
       ```
       {: codeblock}

1.  Erweitern Sie in der Baumstrukturansicht den Knoten `#reservation` und klicken Sie auf die Option zum Bearbeiten des Knotens mit Slots. 

1.  Klicken Sie auf das Symbol **Slot bearbeiten** ![Slot bearbeiten](images/edit-slot.png) für den Slot `@sys-time`.

1.  Wählen Sie im Menü **Optionen** ![Symbol 'Mehr'](images/kabob.png) im Header *Slot 2 konfigurieren* die Option **Bedingte Antworten aktivieren** aus.

1.  Fügen Sie im Abschnitt **Nicht gefunden** eine bedingte Antwort hinzu.

    <table>
    <caption>Details der Antwort für 'Nicht gefunden'</caption>
    <tr>
      <th>Bedingung</th>
      <th>Antwort</th>
    </tr>
    <tr>
      <td>`true`</td>
      <td>Bitte geben Sie an, um welche Zeit Sie speisen möchten. In dem Restaurant können von 9:00 Uhr bis 21:00 Tische reserviert werden.</td>
    </tr>
    </table>

1.  Erhöhen Sie den Wert für `counter` bei jedem Auslösen dieser Antwort um 1. Bedenken Sie, dass diese Antwort nur ausgelöst wird, wenn der Benutzer keinen gültigen Wert für die Uhrzeit angibt. Klicken Sie auf das Symbol **Antwort bearbeiten** ![Antwort bearbeiten](images/edit-slot.png).

1.  Klicken Sie auf das Symbol **Optionen** ![Symbol 'Mehr'](images/kabob.png) und wählen Sie die Option **JSON-Editor öffnen** aus. Fügen Sie die folgende Kontextvariablendefinition hinzu.

    ```json
    {
      "output":{
        "text": {
          "values": [
            "Bitte geben Sie an, um welche Zeit Sie speisen möchten.
              In dem Restaurant können von 9:00 Uhr bis 21:00 Uhr Tische reserviert werden."
          ]
        }
      },
"context": {
        "counter": "<? context['counter'] + 1 ?>"
      }
    }
    ```
    {: codeblock}

    Dieser Ausdruck erhöht den Zählerwert um 1.

1.  Klicken Sie auf **Zurück** und dann auf **Speichern**.

1.  Öffnen Sie erneut den Slot '@sys-time', indem Sie auf das Symbol **Slot bearbeiten** ![Slot bearbeiten](images/edit-slot.png) klicken.

    Sie fügen nun eine zweite bedingte Antwort im Abschnitt **Nicht gefunden** hinzu, um zu prüfen, ob der Zählerwert größer als 1 ist (dies bedeutet, dass der Benutzer bereits dreimal eine ungültige Antwort gegeben hat). Wenn dies zutrifft, fügt das Dialogmodul automatisch die für Tischreservierungen beliebte Zeit 20:00 Uhr ein. Der Benutzer erhält später beim Auslösen des Bestätigungsslots eine Gelegenheit, die Uhrzeit zu ändern. Klicken Sie auf **Antwort hinzufügen**.

1.  Fügen Sie die folgende Bedingung und Antwort hinzu.

    <table>
    <caption>Details der Antwort für 'Nicht gefunden'</caption>
    <tr>
      <th>Bedingung</th>
      <th>Antwort</th>
    </tr>
    <tr>
      <td>`$counter > 1`</td>
      <td>Sie wissen nicht genau, für welche Uhrzeit Sie reservieren möchten? Ich reserviere um 20:00 Uhr einen Tisch für Sie.</td>
    </tr>
    </table>

    Die Variable '$time' muss auf 20:00 Uhr gesetzt werden. Klicken Sie dazu auf das Symbol **Antwort bearbeiten** ![Antwort bearbeiten](images/edit-slot.png). Wählen Sie **JSON-Editor öffnen** aus, fügen Sie die folgende Kontextvariablendefinition hinzu und klicken Sie dann auf **Zurück**.

    ```json
    {
      "output":{
        "text": {
          "values": [
            "Sie wissen nicht genau, für welche Uhrzeit Sie reservieren möchten?
              Ich reserviere um 20:00 Uhr einen Tisch für Sie."
          ]
        }
      },
"context": {
        "time": "<? '20:00:00'.reformatDateTime('h:mm a') ?>"
      }
    }
    ```
    {: codeblock}

1.  Die bedingte Antwort, die Sie soeben hinzugefügt haben, verfügt über eine genauer definierte Bedingung als die Bedingung 'true', die für die erste bedingte Antwort verwendet wird. Sie müssen diese Antwort verschieben, sodass sie vor der ursprünglichen bedingten Antwort verarbeitet wird (andernfalls würde sie nie ausgelöst). Klicken Sie auf die Antwort, die Sie soeben hinzugefügt haben, und versetzen Sie sie mit dem Aufwärtspfeil nach oben. Klicken Sie dann auf **Speichern**.

1.  Testen Sie Ihre Änderungen mit dem folgenden Script.

| Sprecher | Äußerung |
|---------|-----------|
| Sie     | Ich möchte einen Tisch reservieren. |
| Watson  | Ich kann eine Reservierung für Sie vornehmen. Sagen Sie mir nur, an welchem Tag, um welche Zeit und für wie viele Personen Sie reservieren möchten. |
| Sie     | Morgen |
| Watson  | Also Freitag, der 29. Dezember.  Für welche Uhrzeit möchten Sie reservieren? |
| Sie     | Orange |
| Watson  | Bitte geben Sie an, um welche Zeit Sie speisen möchten. In dem Restaurant können von 9:00 Uhr bis 21:00 Tische reserviert werden. |
| Sie     | Rosa |
| Watson  | Bitte geben Sie an, um welche Zeit Sie speisen möchten. In dem Restaurant können von 9:00 Uhr bis 21:00 Tische reserviert werden. |
| Sie     | Lila |
| Watson  | Sie wissen nicht genau, für welche Uhrzeit Sie reservieren möchten? Ich reserviere um 20:00 Uhr einen Tisch für Sie.  Wie viele Personen werden zum Essen kommen? |

## Schritt 9: Mit einem externen Service verbinden
{: #tutorial-slots-complex-action}

Ihr Dialogmodul ist jetzt in der Lage, die Reservierungsdetails beim Benutzer abzufragen und zu erfassen. Sie können nun einen externen Service aufrufen, um im Reservierungssystem des Restaurants oder über ein Online-Reservierungssystem für mehrere Restaurants einen Tisch zu reservieren. Weitere Details enthält der Abschnitt [Programmgesteuerte Aufrufe über einen Dialogmodulknoten absetzen](/docs/services/assistant?topic=assistant-dialog-actions).

Fügen Sie in der Logik zum Aufrufen des Reservierungsservice unbedingt eine Prüfung auf `has_skipped_slots` ein und sorgen Sie dafür, dass der Reservierungsvorgang nicht fortgesetzt wird, wenn diese Variable vorhanden ist.

### Zusammenfassung
{: #tutorial-slots-complex-summary}

In diesem Lernprogramm haben Sie einen Knoten mit Slots getestet und Änderungen vorgenommen, um die Interaktion mit echten Benutzern zu optimieren. Weitere Informationen zu diesem Thema finden Sie unter [Informationen mit Slots erfassen](/docs/services/assistant?topic=assistant-dialog-slots).

## Nächste Schritte
{: #tutorial-slots-complex-deploy}

Stellen Sie Ihren Dialogskill bereit, indem Sie ihn zunächst mit einem Assistenten verbinden und anschließend den Assistenten bereitstellen. Hierzu stehen Ihnen verschiedene Verfahren zur Verfügung. Weitere Details enthält der Abschnitt [Integrationen hinzufügen](/docs/services/assistant?topic=assistant-deploy-integration-add).
