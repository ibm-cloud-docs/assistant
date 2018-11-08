---

copyright:
  years: 2015, 2018
lastupdated: "2018-03-19"

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
{:table: .aria-labeledby="caption"}

# Dialogmodul erstellen
{: #dialog-build}

Zum Erstellen Ihres Dialogmoduls verwenden Sie das {{site.data.keyword.conversationshort}}-Tool.
{: shortdesc}

## Begrenzungen für Dialogmodulknoten
{: #dialog-node-limits}

Die Anzahl der Dialogmodulknoten, die Sie erstellen können, richtet sich nach Ihrem Serviceplan.

| Serviceplan     | Dialogmodulknoten pro Arbeitsbereich |
|------------------|---------------------------:|
| Standard/Premium |                    100.000 |
| Lite             |                     25.000 |
{: caption="Details des Serviceplans" caption-side="top"}

Begrenzung für Baumtiefe: Der Service unterstützt 2.000 untergeordnete Dialogmodulknoten; eine optimale Leistung wird bei 20 oder weniger Knoten erzielt.

## Vorgehensweise
{: #dialog-procedure}

So erstellen Sie ein Dialogmodul:

1.  Öffnen Sie die Seite **Build** über die Navigationsleiste, klicken Sie auf die Registerkarte **Dialogmodul** und klicken Sie dann auf **Erstellen**.

    Wenn Sie den Dialogmodulbuilder zum ersten Mal öffnen, werden die folgenden Knoten automatisch erstellt:
    - **Welcome**: Dies ist der erste Knoten. Er enthält eine Begrüßung, die für die Benutzer angezeigt wird, wenn sie den Service aufrufen. Sie können die Begrüßung bearbeiten.
    - **Anything else**: Dies ist der letzte Knoten. Er enthält Ausdrücke, die als Antworten für die Benutzer ausgegeben werden, wenn ihre Eingabe nicht erkannt wird. Sie können die bereitgestellten Antworten ersetzen oder auch weitere Antworten mit einer ähnlichen Bedeutung hinzufügen, um den Dialog abwechslungsreicher zu machen. Außerdem können Sie auswählen, ob der Service die definierten Antworten nacheinander oder in Zufallsreihenfolge zurückgeben soll.
1.  Klicken Sie auf das Symbol **Mehr** ![Symbol 'Mehr'](images/kabob.png) für den Knoten **Welcome** und wählen Sie dann die Option **Knoten darunter hinzufügen** aus.
1.  Geben Sie eine Bedingung an, bei deren Erfüllung die Verarbeitung des Knotens durch den Service ausgelöst wird.

    Sobald Sie mit dem Definieren einer Bedingung beginnen, wird ein Feld angezeigt, das die verfügbaren Optionen enthält. Sie können eines der folgenden Zeichen eingeben und dann einen Wert aus der angezeigten Liste der Optionen auswählen.

    <table>
    <caption>Syntax für Bedingungsbuilder</caption>
    <tr>
      <th>Zeichen</th>
      <th>Listet definierte Werte für diese Artefakttypen auf</th>
    </tr>
    <tr>
      <td>`#`</td>
      <td>Absichten</td>
    </tr>
    <tr>
      <td>`@`</td>
      <td>Entitäten</td>
    </tr>
    <tr>
      <td>`@{entitätsname}:`</td>
      <td>Werte für {entitätsname}</td>
    </tr>
    <tr>
      <td>`$`</td>
      <td>Im Dialogmodul definierte oder referenzierte Kontextvariablen</td>
    </tr>
    </table>

    Sie können eine neue Absicht, eine neue Entität, einen neuen Entitätswert oder eine neue Kontextvariable erstellen, indem Sie eine neue Bedingung definieren, die sie/ihn verwendet. Falls Sie ein Artefakt auf diese Weise erstellen, denken Sie daran, im Prozess zurückzugehen und andere Schritte auszuführen, die für die vollständige Erstellung des Artefakts erforderlich sind, beispielsweise das Definieren von Beispielen für verbale Äußerungen bei einer Absicht.

    Um einen Knoten zu definieren, der auf der Grundlage von mehreren Bedingungen ausgelöst wird, geben Sie eine Bedingung ein und klicken Sie dann neben ihr auf das Pluszeichen (+). Falls Sie auf mehrere Bedingungen einen Operator `OR` anstelle von `AND` anwenden wollen, klicken Sie auf das zwischen den Feldern angezeigte `and`, um den Operatortyp zu ändern. AND-Operationen werden vor OR-Operationen ausgeführt. Sie können die Reihenfolge jedoch durch die Verwendung von runden Klammern ändern. Beispiel:
    `$isMember:true AND ($memberlevel:silver OR $memberlevel:gold)`

    Die Bedingung, die Sie definieren, muss kürzer als 500 Zeichen sein.

    Weitere Informationen zum Testen von Werten in Bedingungen finden Sie unter [Bedingungen](dialog-overview.html#conditions).
1.  **Optional**: Falls Sie in diesem Knoten mehrere Einzelinformationen vom Benutzer erfassen wollen, klicken Sie auf **Anpassen** und aktivieren Sie **Slots**. Weitere Details enthält der Abschnitt [Informationen mit Slots erfassen](dialog-slots.html).
1.  Geben Sie eine Antwort ein.
    - Fügen Sie den Text hinzu, den der Service für den Benutzer als Antwort anzeigen soll.
    - Wenn Sie verschiedene Antworten für bestimmte Bedingungen definieren möchten, klicken Sie auf **Anpassen** und aktivieren Sie **Mehrere Antworten**.
    - Informationen zu bedingten Antworten und zum Hinzufügen von Antwortvarianten finden Sie unter [Antworten](dialog-overview.html##responses).

1.  Geben Sie an, welche Aktion nach der Verarbeitung des aktuellen Knotens erfolgen soll. Die folgenden Optionen stehen zur Auswahl:

    - **Auf Benutzereingabe warten**: Der Service wartet, bis der Benutzer eine neue Eingabe bereitstellt.
    - **Benutzereingabe überspringen**: Der Service springt direkt zum ersten untergeordneten Knoten. Diese Option ist nur verfügbar, wenn der aktuelle Knoten mindestens einen untergeordneten Knoten hat.
    - **Springen zu**: Der Service setzt das Dialogmodul mit der Verarbeitung des Knotens fort, den Sie angeben. Sie können auswählen, ob der Service die Bedingung des Zielknotens auswerten oder direkt zur Antwort des Zielknotens springen soll. Weitere Details finden Sie unter [Aktion 'Springen zu' konfigurieren](dialog-overview.html#jump-to-config).

1.  **Optional**: Vergeben Sie einen Namen für den Knoten.

    Der Name des Dialogmodulknotens kann Buchstaben (in Unicode), Ziffern, Leerzeichen, Unterstreichungszeichen, Bindestriche und Punkte enthalten.

    Wenn Sie für den Knoten einen Namen vergeben, können Sie sich seinen Zweck besser merken und ihn leichter auffinden, falls er als Symbol angezeigt wird. Falls Sie keinen Namen angeben, wird die Bedingung des Knotens als Name verwendet.

1.  Um weitere Knoten hinzuzufügen, wählen Sie einen Knoten in der Baumstruktur aus und klicken Sie dann auf das Symbol **Mehr** ![Symbol 'Mehr'](images/kabob.png).
    - Um einen Peerknoten zu erstellen, der als nächstes überprüft wird, falls die Bedingung des vorhandenen Knotens erfüllt wird, wählen Sie die Option **Knoten darunter hinzufügen** aus.
    - Um einen Peerknoten zu erstellen, der überprüft wird, bevor die Bedingung für den vorhandenen Knoten überprüft wird, wählen Sie die Option **Knoten darüber hinzufügen** aus.
    - Um einen untergeordneten Knoten für den ausgewählten Knoten zu erstellen, wählen Sie die Option **Untergeordneten Knoten hinzufügen** aus. Ein untergeordneter Knoten wird nach seinem übergeordneten Knoten verarbeitet.
    - Um den aktuellen Knoten zu kopieren, wählen Sie die Option **Duplikat** aus.

    Weitere Informationen zu der Reihenfolge, in der Dialogmodulknoten verarbeitet werden, finden Sie unter [Dialogmodule im Überblick](dialog-overview.html#dialog-flow).
1.  Testen Sie das Dialogmodul, während Sie es erstellen.
   Weitere Informationen enthält der Abschnitt [Dialogmodul testen](#test).

## Dialogmodul testen
{: #test}

Wenn Sie Änderungen an Ihrem Dialogmodul vornehmen, können Sie es jederzeit testen und feststellen, wie es auf Eingabe reagiert.

1.  Klicken Sie auf der Registerkarte 'Dialogmodul' auf das Symbol ![Watson fragen](images/ask_watson.png).
1.  Geben Sie im Chatbereich Text ein und drücken Sie die Eingabetaste.

    Vergewissern Sie sich, dass das System das Training für die zuletzt vorgenommenen Änderungen beendet hat, bevor Sie das Dialogmodul testen. Falls das System noch mit dem Training beschäftigt ist, wird im Bereich *Ausprobieren* eine Nachricht angezeigt:
    {: tip}

    ![Screenshot mit Nachricht über Training](images/training.png)
1.  Überprüfen Sie die Antwort, um zu ermitteln, ob das Dialogmodul die Eingabe richtig interpretiert und die entsprechende Antwort ausgewählt hat.

    Im Chatfenster ist angegeben, welche Absichten und Entitäten in der Eingabe erkannt wurden:

    ![Screenshot mit der Ausgabe für den Test des Dialogmoduls](images/test_dialog_output.png)
1.  Wenn Sie wissen möchten, welcher Knoten in der Baumstruktur des Dialogmoduls eine Antwort ausgelöst hat, klicken Sie auf das Symbol **Position** ![Position](images/location.png) neben der Antwort. Öffnen Sie die Registerkarte 'Dialogmodul', falls sie noch nicht geöffnet ist.

    Der Quellenknoten wird in den Fokus genommen und die Route durch die Baumstruktur, über die der Service den Knoten erreicht hat, wird hervorgehoben. Die Route bleibt hervorgehoben, bis Sie eine andere Aktion ausführen (z. B. eine neue Testeingabe eingeben).
1.  Um den Wert einer Kontextvariablen zu überprüfen oder festzulegen, klicken Sie auf den Link **Kontext verwalten**.

    Alle Kontextvariablen, die Sie im Dialogmodul definiert haben, werden angezeigt.

    Außerdem wird eine Kontextvariable namens `$timezone` aufgelistet. Die Benutzerschnittstelle für das Fenster *Ausprobieren* ruft aus dem Web-Browser Informationen zur Benutzerländereinstellung ab und verwendet Sie zum Festlegen der Kontextvariablen `$timezone`. Diese Kontextvariable vereinfacht die Berücksichtigung von Zeitunterschieden beim Austausch von Testdialogmodulen. Es kann sinnvoll sein, etwas Ähnliches in Ihre Benutzeranwendung aufzunehmen. Wenn nichts angegeben ist, wird die Zeitzone GMT (Greenwich Mean Time, mittlere Greenwich-Zeit) verwendet.

    Sie können eine Variable hinzufügen und ihren Wert festlegen, um festzustellen, wie das Dialogmodul in der nächsten Runde des Testdialogmoduls reagiert. Diese Funktionalität ist beispielsweise von Nutzen, wenn das Dialogmodul so konfiguriert ist, dass abhängig vom Wert einer Kontextvariablen, der vom Benutzer bereitgestellt wird, unterschiedliche Antworten angezeigt werden.

    1.  Geben Sie zum Hinzufügen einer Kontextvariablen den Variablennamen an und drücken Sie die **Eingabetaste**.
    1.  Suchen Sie zum Definieren eines Standardwerts für die Kontextvariable in der Liste nach der hinzugefügten Kontextvariablen und geben Sie dann einen Wert für sie an.

    Weitere Informationen finden Sie unter [Kontextvariablen](dialog-runtime.html#context).

1.  Setzen Sie die Interaktion mit dem Dialogmodul fort, um den Ablauf des Dialogs im Modul nachzuvollziehen.
    - Wenn Sie eine verbale Testäußerung suchen und erneut übergeben wollen, können Sie mit der Aufwärtstaste in Ihren letzten Eingaben navigieren.
    - Um frühere verbale Testäußerungen aus dem Chatbereich zu entfernen und neu zu beginnen, klicken Sie auf den Link **Löschen**. Diese Aktion entfernt nicht nur die verbalen Testäußerungen und die Antworten, sondern auch die Werte aller Kontextvariablen, die infolge Ihrer Interaktion mit dem Dialogmodul festgelegt wurden. Werte für Kontextvariablen, die Sie explizit festgelegt oder geändert haben, werden nicht entfernt.

### Nächste Schritte

Wenn Sie feststellen, dass falsche Absichten oder Entitäten erkannt werden, müssen Sie möglicherweise die Absichts- oder Entitätsdefinitionen ändern.

Falls die richtigen Absichten und Entitäten erkannt wurden, jedoch im Dialogmodul falsche Knoten ausgelöst wurden, vergewissern Sie sich, dass Ihre Bedingungen korrekt formuliert sind.

## Dialogmodulknoten kopieren
{: #copy-node}

Sie können einen Knoten duplizieren, um eine exakte Kopie als Peerknoten unmittelbar unter dem Ursprungsknoten in der Baumstruktur des Dialogmoduls) zu erstellen. Der kopierte Knoten trägt den gleichen Namen wie der Ursprungsknoten, jedoch mit der angefügten Endung `- copy`*`n`*. Dabei ist *`n`* eine Zahl, die mit 1 beginnt. Wenn Sie denselben Knoten mehrmals kopieren wird die im Namen angegebene Zahl *`n`* für jede Kopie um eins erhöht, um die Kopien voneinander zu unterscheiden. Wenn der Knoten noch keinen Namen hat, wird `copy`*`n`* als Name verwendet.

Wenn Sie einen Knoten duplizieren, der über untergeordnete Knoten verfügt, werden die untergeordneten Knoten ebenfalls dupliziert. Die kopierten untergeordneten Knoten tragen dieselben Namen wie die ursprünglichen untergeordneten Knoten. Die einzige Möglichkeit, einen kopierten untergeordneten Knoten vom zugehörigen Ursprungsknoten zu unterscheiden, ist die Angebe `copy` im Namen des übergeordneten Knotens.

1.  Klicken Sie für den Knoten, den Sie kopieren möchten, auf das Symbol **Mehr** ![Symbol 'Mehr'](images/kabob.png) und wählen Sie **Duplikat** aus.
1.  Ziehen Sie in Betracht, die kopierten Knoten umzubenennen oder die zugehörigen Bedingungen zu bearbeiten, damit sie einfacher zu unterscheiden sind.

## Dialogmodulknoten verschieben
{: #move-node}

Jeden Knoten, den Sie erstellen, können Sie an eine andere Stelle in der Baumstruktur des Dialogmoduls verschieben.

Beispiel: Sie wollen einen zuvor erstellten Knoten in einen anderen Bereich des Ablaufs verschieben, um den Dialog zu ändern. Durch das Verschieben können Sie Knoten zu gleichgeordneten Knoten oder Peerknoten in einer anderen Verzweigung machen.

1.  Klicken Sie für den Knoten, den Sie verschieben wollen, auf das Symbol **Mehr** ![Symbol 'Mehr'](images/kabob.png) und wählen Sie die Option **Verschieben** aus.
1.  Wählen Sie einen Zielknoten aus, der sich in der Baumstruktur in der Nähe der Position befindet, an die Sie diesen Knoten verschieben wollen. Wählen Sie aus, ob dieser Knoten vor oder hinter dem Zielknoten bzw. als untergeordnetes Element des Zielknotens platziert werden soll.

## Dialogmodul in Ordnern zusammenfassen
{: #folders}

Sie können Dialogmodulknoten zu einem Ordner hinzufügen, um sie zu gruppieren. Knoten können zum Beispiel aus den folgenden Gründen gruppiert werden:

- Knoten zusammenfassen, die ein ähnliches Ziel haben, damit sie leichter zu finden sind. Zum Beispiel können Knoten, die Fragen zu Benutzerkonten beantworten, in einem Ordner *Benutzerkonto* zusammengefasst werden und Knoten, die Fragen zur Zahlungsabwicklung beantworten, in einem Ordner *Zahlung*.
- Knoten gruppieren, die vom Dialogmodul nur unter bestimmten Bedingungen verarbeitet werden sollen. Verwenden Sie zum Beispiel eine Bedingung wie `$isPlatinumMember`, um Knoten zu gruppieren, die zusätzliche Services bereitstellen und nur verarbeitet werden sollen, wenn der aktuelle Benutzer für die zusätzlichen Services berechtigt ist.
- Knoten während der Laufzeit ausblenden, solange Sie diese Knoten bearbeiten. Sie können Knoten zu einem Ordner mit einer Bedingung `false` hinzufügen, um zu verhindern, dass diese Knoten verarbeitet werden.
- Dieselben Konfigurationseinstellungen für das Abschweifen zu einem anderen Knoten gleichzeitig auf mehrere Stammknoten anwenden. Weitere Informationen enthält der Abschnitt [Abschweifungen](dialog-runtime.html#digressions).

Die folgenden Merkmale des Ordners wirken sich auf die Verarbeitung der Knoten in einem Ordner aus:

- Bedingung: Wenn diese Option angegeben ist, wertet der Service zuerst die Ordnerbedingung aus, um festzustellen, ob die Knoten in dem Ordner verarbeitet werden sollen.
- Anpassungen: Alle Konfigurationseinstellungen, die Sie auf den Ordner anwenden, werden von den Knoten in dem Ordner übernommen. Wenn Sie beispielsweise die Abschweifungseinstellungen für den Ordner ändern, werden diese Änderungen für alle Knoten im Ordner übernommen.
- Baumhierarchie: Die Knoten in einem Ordner werden als Stammknoten oder untergeordnete Knoten behandelt, je nachdem, ob der Ordner auf der Stammebene oder der untergeordneten Ebene in der Baumstruktur des Dialogmoduls hinzugefügt wird. Alle Knoten auf der Stammebene, die Sie zu einem Ordner der Stammebene hinzufügen, werden weiterhin als Stammknoten behandelt; d. h. sie werden nicht zu untergeordneten Knoten des Ordners. Wenn Sie jedoch Knoten der Stammebene in einen Ordner verschieben, der einem anderen Knoten untergeordnet ist, dann werden die Stammknoten zu untergeordneten Elementen dieses anderen Ordners.

Ordner haben keine Auswirkung auf die Reihenfolge, in der Knoten verarbeitet werden. Die Knoten werden weiterhin vom ersten bis zum letzten Knoten nacheinander verarbeitet. Wenn der Service beim Durchschreiten der Baumstruktur einen Ordner findet, dessen Ordnerbedingung 'true' ist, wird sofort der erste Knoten in dem Ordner verarbeitet und danach die weiteren Elemente der Baumstruktur in der angegebenen Reihenfolge. Ein Ordner, der keine Ordnerbedingung enthält, ist für den Service transparent.

### Ordner hinzufügen
{: #folders-add}

So fügen Sie einen Ordner zur Baumstruktur des Dialogmoduls hinzu:

1.  Klicken Sie in der Baumstrukturansicht auf der Registerkarte **Dialogmodul** auf **Ordner hinzufügen**.

    Der Ordner wird in der Baumstruktur des Dialogmoduls vor dem letzten Knoten (**Anything else**) hinzugefügt. Falls ein vorhandener Knoten in der Baumstruktur ausgewählt ist, wird der neue Knoten nach dem ausgewählten Knoten hinzugefügt.

    Wenn Sie den Ordner an einer anderen Stelle in der Baumstruktur hinzufügen möchten. klicken Sie ausgehend von dem Knoten oberhalb der Stelle, an der Sie den Knoten hinzufügen möchten, auf das Symbol **Mehr** ![Symbol 'Mehr'](images/kabob.png) und wählen Sie dann **Ordner hinzufügen** aus.

    Sie können einen Ordner unter einem untergeordneten Knoten in einer bestehenden Verzweigung der Baumstruktur hinzufügen. Klicken Sie dazu auf das Symbol **Mehr** ![Symbol 'Mehr'](images/kabob.png) für den untergeordneten Knoten und wählen Sie dann **Ordner hinzufügen** aus.

    Der Ordner wird in der Bearbeitungsansicht geöffnet.

1.  **Optional**: Geben Sie einen Namen für den Ordner an.

1.  **Optional**: Definieren Sie eine Bedingung für den Ordner.

    Wenn Sie keine Bedingung angeben, wird die Bedingung `true` verwendet, d. h. die in dem Ordner enthaltenen Knoten werden immer verarbeitet.

1.  Fügen Sie Dialogmodulknoten in dem Ordner hinzu.

    - Sie können bestehende Dialogmodulknoten zu dem Ordner hinzufügen, indem Sie die betreffenden Knoten in den Ordner verschieben.

      Klicken Sie auf das Symbol **Mehr** ![Symbol 'Mehr'](images/kabob.png) für den Knoten, den Sie verschieben möchten, wählen Sie **Verschieben** aus und klicken Sie dann auf den Ordner. Wählen Sie **In Ordner** als Ziel für das Verschieben aus.

      Wenn Sie Ordner verschieben, werden diese am Anfang der Baumstruktur im Ordner hinzugefügt. Wenn Sie beim Verschieben mehrerer aufeinanderfolgender Stammdialogmodulknoten die Reihenfolge beibehalten möchten, müssen Sie die Knoten in umgekehrter Reihenfolge verschieben (d. h. beginnend mit dem letzten Knoten).{: tip}

    - Um einen neuen Dialogmodulknoten zu dem Ordner hinzuzufügen, klicken Sie auf das Symbol **Mehr** ![Symbol 'Mehr'](images/kabob.png) für den Ordner und wählen Sie dann die Option **Knoten zum Ordner hinzufügen** aus.

      Der Dialogmodulknoten wird in dem Ordner am Ende der Baumstruktur des Dialogmoduls hinzugefügt.

### Ordner löschen
{: #folders-delete}

Sie können einen Ordner entweder allein oder mit allen darin enthaltenen Dialogmodulknoten löschen.

So löschen Sie einen Ordner:

1.  Suchen Sie in der Baumstrukturansicht der Registerkarte **Dialogmodul** den Ordner, den Sie löschen möchten.

1.  Klicken Sie auf das Symbol **Mehr** ![Symbol 'Mehr'](images/kabob.png) für den Ordner und wählen Sie dann **Löschen** aus.

1.  Führen Sie eine der folgenden Aktionen aus:

    - Um nur den Ordner zu löschen und die darin enthaltenen Dialogmodulknoten beizubehalten, wählen Sie das Kontrollkästchen **Knoten im Ordner löschen** auf und klicken Sie dann auf **Jetzt löschen**.
    - Um den Ordner und alle darin enthaltenen Dialogmodulknoten zu löschen, klicken Sie auf **Jetzt löschen**.

Wenn Sie nur den Ordner (ohne die enthaltenen Knoten) gelöscht haben, werden die zugehörigen Knoten in der Baumstruktur des Dialogmoduls an der Stelle angezeigt, an der sich der gelöschte Ordner zuvor befand.

## Dialogmodulknoten anhand der Knoten-ID suchen
{: #get-node-id}

Die Suche nach einem Dialogmodulknoten, dem eine bekannte Knoten-ID zugeordnet ist, kann die folgenden Gründe haben:

- Sie sehen sich Protokolle an und ein Protokoll referenziert einen Abschnitt des Dialogmoduls durch die Knoten-ID.
- Sie wollen die in der Eigenschaft `nodes_visited` der API-Nachrichtenausgabe aufgelisteten Knoten-IDs zu Knoten zuordnen, die in der Baumstruktur Ihres Dialogmoduls angezeigt werden.
- Sie werden in einer Laufzeitfehlernachricht für das Dialogmodul über einen Syntaxfehler informiert und der Knoten, den Sie korrigieren müssen, ist dort mit der Knoten-ID angegeben.

So suchen Sie einen Knoten anhand seiner Knoten-ID:

1.  Wählen Sie auf der Registerkarte 'Dialogmodul' des Tools einen beliebigen Knoten in der Baumstruktur des Dialogmoduls aus.
1.  Schließen Sie die Bearbeitungsansicht, falls sie für den aktuellen Knoten geöffnet ist.
1.  Im Adressfeld Ihres Web-Browsers sollte jetzt eine URL angezeigt werden, die die folgende Syntax aufweist:

    `    https://watson-conversation.ng.bluemix.net/space/instanz-id/workspaces/arbeitsbereichs-id/build/dialog#node=knoten-id
    `

1.  Bearbeiten Sie die URL, indem Sie den aktuellen Wert für `node-id` durch die ID des Knotens ersetzen, den Sie suchen möchten, und übergeben Sie dann die neue URL.
1.  Falls erforderlich, heben Sie die bearbeitete URL erneut hervor und übergeben Sie sie erneut.

Das Tool wird aktualisiert und verschiebt den Fokus auf den Dialogmodulknoten mit der von Ihnen angegebenen Knoten-ID. Wenn es sich um die Knoten-ID für einen Slot, einen Slot-Handler oder eine bedingte Antwort handelt, wird der Knoten, in dem die Slotantwort oder die bedingte Antwort definiert ist, in den Fokus genommen und der entsprechende Modaldialog wird angezeigt.

**Hinweis**: Wenn der Knoten nicht gefunden wird, können Sie den Arbeitsbereich exportieren und in einem JSON-Editor öffnen, um in der JSON-Datei des Arbeitsbereichs zu suchen.
