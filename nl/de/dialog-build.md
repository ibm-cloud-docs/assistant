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

# Dialogmodul erstellen
{: #dialog-build}

Zum Erstellen Ihres Dialogmoduls verwenden Sie das {{site.data.keyword.conversationshort}}-Tool.
{: shortdesc}

## Dialogmodul erstellen
{: #dialog-build-task}

So erstellen Sie ein Dialogmodul:

1.  Klicken Sie auf die Registerkarte **Dialogmodul** und anschließend auf **Erstellen**.

    Wenn Sie den Dialogmoduleditor zum ersten Mal öffnen, werden die folgenden Knoten automatisch erstellt: 

    - **Welcome**: Dies ist der erste Knoten. Er enthält eine Begrüßung, die für die Benutzer angezeigt wird, wenn sie den Service aufrufen. Sie können die Begrüßung bearbeiten.

    Dieser Knoten wird in Dialogabläufen, die von Benutzern eingeleitet wurden, nicht ausgelöst. Beispiel: In Dialogmodulen, die in Integrationen mit Facebook- oder Slack-Kanälen verwendet werden, werden Knoten mit der Sonderbedingung `welcome` übersprungen. Weitere Informationen enthält der Abschnitt [Dialogmodul initialiseren](/docs/services/assistant?topic=assistant-dialog-start).
    {: note}

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

    Die von Ihnen definierte Bedingung, muss kürzer als 2.048 Zeichen sein. 

    Weitere Informationen zum Testen von Werten in Bedingungen finden Sie unter [Bedingungen](/docs/services/assistant?topic=assistant-dialog-overview#dialog-overview-conditions).
1.  **Optional**: Falls Sie in diesem Knoten mehrere Einzelinformationen vom Benutzer erfassen wollen, klicken Sie auf **Anpassen** und aktivieren Sie **Slots**. Weitere Details enthält der Abschnitt [Informationen mit Slots erfassen](/docs/services/assistant?topic=assistant-dialog-slots).
1.  Geben Sie eine Antwort ein.
    - Fügen Sie die Text- oder Multimediaelemente hinzu, die der Service als Antwort für den Benutzer anzeigen soll. 
    - Wenn Sie verschiedene Antworten für bestimmte Bedingungen definieren möchten, klicken Sie auf **Anpassen** und aktivieren Sie **Mehrere Antworten**.
    - Informationen zu bedingten Antworten, zu erweiterten Antworten und zum Hinzufügen von Antwortvarianten enthält der Abschnitt [Antworten](/docs/services/assistant?topic=assistant-dialog-overview#dialog-overview-responses).

1.  Geben Sie an, welche Aktion nach der Verarbeitung des aktuellen Knotens erfolgen soll. Die folgenden Optionen stehen zur Auswahl:

    - **Auf Benutzereingabe warten**: Der Service wartet, bis der Benutzer eine neue Eingabe bereitstellt.
    - **Benutzereingabe überspringen**: Der Service springt direkt zum ersten untergeordneten Knoten. Diese Option ist nur verfügbar, wenn der aktuelle Knoten mindestens einen untergeordneten Knoten hat.
    - **Springen zu**: Der Service setzt das Dialogmodul mit der Verarbeitung des Knotens fort, den Sie angeben. Sie können auswählen, ob der Service die Bedingung des Zielknotens auswerten oder direkt zur Antwort des Zielknotens springen soll. Weitere Details finden Sie unter [Aktion 'Springen zu' konfigurieren](/docs/services/assistant?topic=assistant-dialog-overview#dialog-overview-jump-to-config).

1.  **Optional**: Angenommen, dieser Knoten soll in Betracht gezogen werden, wenn während der Laufzeit Knotenauswahlmöglichkeiten für Benutzer angezeigt werden, und der Benutzer wird aufgefordert, die am besten geeignete Option für seine Zielsetzung auszuwählen. Geben Sie in diesem Fall eine kurze Beschreibung des Benutzerziels, auf das dieser Knoten ausgerichtet ist, in das Feld **Externer Knotenname** ein. Beispiel: *Bestellung aufgeben*.

    ![Nur Plus- oder Premium-Plan](images/premium.png) Das Feld *Externer Knotenname* wird nur für Benutzer des Plus- oder des Premium-Plans angezeigt. Weitere Details enthält der Abschnitt [Vereindeutigung](/docs/services/assistant?topic=assistant-dialog-runtime#dialog-runtime-disambiguation).

1.  **Optional**: Vergeben Sie einen Namen für den Knoten.

    Der Name des Dialogmodulknotens kann Buchstaben (in Unicode), Ziffern, Leerzeichen, Unterstreichungszeichen, Bindestriche und Punkte enthalten.

    Wenn Sie für den Knoten einen Namen vergeben, können Sie sich seinen Zweck besser merken und ihn leichter auffinden, falls er als Symbol angezeigt wird. Falls Sie keinen Namen angeben, wird die Bedingung des Knotens als Name verwendet.

1.  Um weitere Knoten hinzuzufügen, wählen Sie einen Knoten in der Baumstruktur aus und klicken Sie dann auf das Symbol **Mehr** ![Symbol 'Mehr'](images/kabob.png).
    - Um einen Peerknoten zu erstellen, der als nächstes überprüft wird, falls die Bedingung des vorhandenen Knotens erfüllt wird, wählen Sie die Option **Knoten darunter hinzufügen** aus.
    - Um einen Peerknoten zu erstellen, der überprüft wird, bevor die Bedingung für den vorhandenen Knoten überprüft wird, wählen Sie die Option **Knoten darüber hinzufügen** aus.
    - Um einen untergeordneten Knoten für den ausgewählten Knoten zu erstellen, wählen Sie die Option **Untergeordneten Knoten hinzufügen** aus. Ein untergeordneter Knoten wird nach seinem übergeordneten Knoten verarbeitet.
    - Um den aktuellen Knoten zu kopieren, wählen Sie die Option **Duplikat** aus.

    Weitere Informationen zu der Reihenfolge, in der Dialogmodulknoten verarbeitet werden, finden Sie unter [Dialogmodule im Überblick](/docs/services/assistant?topic=assistant-dialog-overview#dialog-overview-flow).
1.  Testen Sie das Dialogmodul, während Sie es erstellen.
   Weitere Informationen enthält der Abschnitt [Dialogmodul testen](#dialog-build-test).

## Dialogmodul testen
{: #dialog-build-test}

Während Sie Änderungen an Ihrem Dialogmodul vornehmen, können Sie das Modul jederzeit testen, um festzustellen, wie es auf Benutzereingaben reagiert.

Abfragen, die Sie über die Anzeige 'Ausprobieren' absetzen, generieren zwar API-Aufrufe des Typs `/message`, aber sie werden nicht protokolliert und verursachen keine Kosten. 

1.  Klicken Sie auf der Registerkarte 'Dialogmodul' auf das Symbol ![Ausprobieren](images/ask_watson.png).
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

    Weitere Informationen finden Sie unter [Kontextvariablen](/docs/services/assistant?topic=assistant-dialog-runtime#dialog-runtime-context).

1.  Setzen Sie die Interaktion mit dem Dialogmodul fort, um den Ablauf des Dialogs im Modul nachzuvollziehen.
    - Wenn Sie eine verbale Testäußerung suchen und erneut übergeben wollen, können Sie mit der Aufwärtstaste in Ihren letzten Eingaben navigieren.
    - Um frühere verbale Testäußerungen aus dem Chatbereich zu entfernen und neu zu beginnen, klicken Sie auf den Link **Löschen**. Diese Aktion entfernt nicht nur die verbalen Testäußerungen und die Antworten, sondern auch die Werte aller Kontextvariablen, die infolge Ihrer Interaktion mit dem Dialogmodul festgelegt wurden. Werte für Kontextvariablen, die Sie explizit festgelegt oder geändert haben, werden nicht entfernt.

### Nächste Schritte
{: #dialog-build-next-steps}

Wenn Sie feststellen, dass falsche Absichten oder Entitäten erkannt werden, müssen Sie möglicherweise die Absichts- oder Entitätsdefinitionen ändern.

Falls die richtigen Absichten und Entitäten erkannt wurden, jedoch im Dialogmodul falsche Knoten ausgelöst wurden, vergewissern Sie sich, dass Ihre Bedingungen korrekt formuliert sind.

Hilfreiche Tipps für den Einstieg enthält der Abschnitt [Tipps zum Erstellen von Dialogmodulen](/docs/services/assistant?topic=assistant-dialog-tips).

Wenn der Dialog als Hilfe für Ihre Benutzer einsatzbereit ist, integrieren Sie Ihren Assistenten in eine Messaging-Plattform oder angepasste Anwendung. Weitere Informationen enthält der Abschnitt [Integrationen hinzufügen](/docs/services/assistant?topic=assistant-deploy-integration-add).

## Begrenzungen für Dialogmodulknoten
{: #dialog-build-node-limits}

Wie viele Dialogmodulknoten Sie pro Skill erstellen können, richtet sich nach Ihrem Serviceplan.

| Serviceplan     | Dialogmodulknoten pro Skill     |
|------------------|---------------------------:|
| Premium          |                    100.000 |
| Plus             |                    100.000 |
| Standard         |                    100.000 |
| Lite             |                     100`*` |
{: caption="Serviceplandetails" caption-side="top"}

Die vordefinierten Dialogmodulknoten 'welcome' und 'anything_else' in der Baumstruktur werden auf die Gesamtzahl angerechnet.

Begrenzung für Baumtiefe: Der Service unterstützt 2.000 untergeordnete Dialogmodulknoten; eine optimale Leistung des Tools wird bei 20 oder weniger Knoten erzielt.

`*` Die Begrenzung für Lite-Pläne wurde am 1. Dezember 2018 von 25.000 in 100 geändert. Benutzer von Serviceinstanzen, die vor dieser Änderung erstellt wurden, müssen Ihren Plan bis zum 1. Juni 2019 umstellen oder die Dialogmodule in den Skills der vorhandenen Serviceinstanzen an die neuen Grenzwerte anpassen.

Führen Sie eine der folgenden Aktionen aus, um die Anzahl der Dialogmodulknoten in einem Dialogskill anzuzeigen: 

- Verwenden Sie das Tool, sofern es noch keinem Assistenten zugeordnet ist, um den Dialogskill einem Assistenten zuzuordnen, und zeigen Sie anschließend die Kachel für den Skill auf der Hauptseite des Assistenten an. Im Abschnitt *Trainierte Daten* wird die Anzahl der Dialogmodulknoten angegeben. 
- Senden Sie eine GET-Anforderung an den API-Endpunkt '/dialog_nodes' und fügen Sie den Parameter `include_count=true` ein. Beispiel:

  ```curl
  curl -u "apikey:{apikey}" "https://gateway.watsonplatform.net/assistant/api/v1/workspaces/{workspace_id}/dialog_nodes?version=2018-09-20&include_count=true"
  ```

  In der Antwort ist die Anzahl der Dialogmodulknoten im Attribut `total` des Objekts `pagination` enthalten.

Wenn die Gesamtzahl höher ist, als Sie erwartet haben, kann dies daran liegen, dass das im Tool von Ihnen erstellte Dialogmodul vom Tool in ein JSON-Objekt übersetzt wird. Manche Felder, die Teil eines einzelnen Knotens zu sein scheinen, werden im zugrunde liegenden JSON-Objekt tatsächlich als separate Dialogmodulknoten strukturiert. 

  - Jeder Knoten und Ordner wird als eigener Knoten dargestellt. 
  - Jede bedingte Antwort, die einem einzelnen Dialogmodulknoten zugeordnet ist, wird als einzelner Knoten dargestellt. 
  - Für einen Knoten mit Slots wird jeder Slot, jede Antwort für 'Slot gefunden' oder 'Slot nicht gefunden', jeder Slot-Handler sowie (falls festgelegt) jede Antwort für 'Alles abfragen' als einzelner Knoten dargestellt. Dies bedeutet, dass ein Knoten mit drei Slots insgesamt elf Dialogmodulknoten bilden kann.

## Dialogmodul durchsuchen
{: #dialog-build-search}

Sie können das Dialogmodul durchsuchen, um den oder die Dialogmodulknoten zu finden, in denen ein angegebenes Wort oder ein angegebener Ausdruck erwähnt ist.

1.  Wählen Sie das Symbol 'Suche' aus: ![Symbol 'Suche'](images/search_icon.png)

1.  Geben Sie einen Suchbegriff oder -ausdruck ein.

    Beim ersten Suchvorgang wird ein Index erstellt. Möglicherweise werden Sie aufgefordert, zu warten bis der Text in Ihren Dialogmodulknoten indexiert wurde.
    {: note}

Knoten, die Ihren Suchbegriff enthalten, und entsprechende Beispiele werden angezeigt. Wählen Sie ein Ergebnis aus, damit es zum Bearbeiten geöffnet wird.

  ![Rückgabe der Suche nach Absichten](images/search_dialog.png)

## Dialogmodulknoten anhand der Knoten-ID suchen
{: #dialog-build-get-node-id}

Sie können Dialogmodulknoten anhand der zugehörigen Knoten-ID suchen. Geben Sie die vollständige Knoten-ID in das Suchfeld ein. Die Suche nach einem Dialogmodulknoten, dem eine bekannte Knoten-ID zugeordnet ist, kann die folgenden Gründe haben:

- Sie sehen sich Protokolle an und ein Protokoll referenziert einen Abschnitt des Dialogmoduls durch die Knoten-ID.
- Sie wollen die in der Eigenschaft `nodes_visited` der API-Nachrichtenausgabe aufgelisteten Knoten-IDs zu Knoten zuordnen, die in der Baumstruktur Ihres Dialogmoduls angezeigt werden.
- Sie werden in einer Laufzeitfehlernachricht für das Dialogmodul über einen Syntaxfehler informiert und der Knoten, den Sie korrigieren müssen, ist dort mit der Knoten-ID angegeben.

Eine andere Methode zum Aufspüren eines Knotens anhand der zugehörigen Knoten-ID umfasst die folgenden Schritte:

1.  Wählen Sie im Tool auf der Registerkarte 'Dialogmodul' einen beliebigen Knoten in der Baumstruktur Ihres Dialogmoduls aus.
1.  Schließen Sie die Bearbeitungsansicht, falls sie für den aktuellen Knoten geöffnet ist.
1.  Im Adressfeld Ihres Web-Browsers sollte jetzt eine URL angezeigt werden, die die folgende Syntax aufweist:

    `    https://watson-conversation.ng.bluemix.net/space/instanz-id/workspaces/arbeitsbereichs-id/build/dialog#node=knoten-id
    `

1.  Bearbeiten Sie die URL, indem Sie den aktuellen Wert für `node-id` durch die ID des Knotens ersetzen, den Sie suchen möchten, und übergeben Sie dann die neue URL.
1.  Falls erforderlich, heben Sie die bearbeitete URL erneut hervor und übergeben Sie sie erneut.

Das Tool wird aktualisiert und verschiebt den Fokus auf den Dialogmodulknoten mit der von Ihnen angegebenen Knoten-ID. Wenn es sich um die Knoten-ID für einen Slot, für eine Slotbedingung 'Gefunden' oder 'Nicht gefunden', für einen Slot-Handler oder für eine bedingte Antwort handelt, wird der Fokus auf den Knoten gelegt, in dem der Slot oder die bedingte Antwort definiert ist, und der entsprechende Modaldialog wird angezeigt.

Wenn der Knoten noch immer nicht gefunden wird, können Sie den Dialogskill exportieren und in einem JSON-Editor öffnen, um in der JSON-Datei des Skills zu suchen.
{: tip}

## Dialogmodulknoten kopieren
{: #dialog-build-copy-node}

Sie können einen Knoten duplizieren, um eine exakte Kopie als Peerknoten unmittelbar unter dem Ursprungsknoten in der Baumstruktur des Dialogmoduls) zu erstellen. Der kopierte Knoten trägt den gleichen Namen wie der Ursprungsknoten, jedoch mit der angefügten Endung `- copy`*`n`*. Dabei ist *`n`* eine Zahl, die mit 1 beginnt. Wenn Sie denselben Knoten mehrmals kopieren wird die im Namen angegebene Zahl *`n`* für jede Kopie um eins erhöht, um die Kopien voneinander zu unterscheiden. Wenn der Knoten noch keinen Namen hat, wird `copy`*`n`* als Name verwendet.

Wenn Sie einen Knoten duplizieren, der über untergeordnete Knoten verfügt, werden die untergeordneten Knoten ebenfalls dupliziert. Die kopierten untergeordneten Knoten tragen dieselben Namen wie die ursprünglichen untergeordneten Knoten. Die einzige Möglichkeit, einen kopierten untergeordneten Knoten vom zugehörigen Ursprungsknoten zu unterscheiden, ist die Angebe `copy` im Namen des übergeordneten Knotens.

1.  Klicken Sie für den Knoten, den Sie kopieren möchten, auf das Symbol **Mehr** ![Symbol 'Mehr'](images/kabob.png) und wählen Sie **Duplikat** aus.
1.  Ziehen Sie in Betracht, die kopierten Knoten umzubenennen oder die zugehörigen Bedingungen zu bearbeiten, damit sie einfacher zu unterscheiden sind.

## Dialogmodulknoten verschieben
{: #dialog-build-move-node}

Jeden Knoten, den Sie erstellen, können Sie an eine andere Stelle in der Baumstruktur des Dialogmoduls verschieben.

Beispiel: Sie wollen einen zuvor erstellten Knoten in einen anderen Bereich des Ablaufs verschieben, um den Dialog zu ändern. Durch das Verschieben können Sie Knoten zu gleichgeordneten Knoten oder Peerknoten in einer anderen Verzweigung machen.

1.  Klicken Sie für den Knoten, den Sie verschieben wollen, auf das Symbol **Mehr** ![Symbol 'Mehr'](images/kabob.png) und wählen Sie die Option **Verschieben** aus.
1.  Wählen Sie einen Zielknoten aus, der sich in der Baumstruktur in der Nähe der Position befindet, an die Sie diesen Knoten verschieben wollen. Wählen Sie aus, ob dieser Knoten vor oder hinter dem Zielknoten bzw. als untergeordnetes Element des Zielknotens platziert werden soll.

## Dialogmodul in Ordnern zusammenfassen
{: #dialog-build-folders}

Sie können Dialogmodulknoten zu einem Ordner hinzufügen, um sie zu gruppieren. Knoten können zum Beispiel aus den folgenden Gründen gruppiert werden:

- Knoten zusammenfassen, die ein ähnliches Ziel haben, damit sie leichter zu finden sind. Zum Beispiel können Knoten, die Fragen zu Benutzerkonten beantworten, in einem Ordner *Benutzerkonto* zusammengefasst werden und Knoten, die Fragen zur Zahlungsabwicklung beantworten, in einem Ordner *Zahlung*.
- Knoten gruppieren, die vom Dialogmodul nur unter bestimmten Bedingungen verarbeitet werden sollen. Verwenden Sie zum Beispiel eine Bedingung wie `$isPlatinumMember`, um Knoten zu gruppieren, die zusätzliche Services bereitstellen und nur verarbeitet werden sollen, wenn der aktuelle Benutzer für die zusätzlichen Services berechtigt ist.
- Knoten während der Laufzeit ausblenden, solange Sie diese Knoten bearbeiten. Sie können Knoten zu einem Ordner mit einer Bedingung `false` hinzufügen, um zu verhindern, dass diese Knoten verarbeitet werden.

Die folgenden Merkmale des Ordners wirken sich auf die Verarbeitung der Knoten in einem Ordner aus:

- Bedingung: Wenn keine Bedingung angegeben wird, verarbeitet der Service die Knoten direkt in dem Ordner. Wenn eine Bedingung angegeben wird, wertet der Service zunächst dei Ordnerbedingung aus, um festzustellen, ob die Knoten in dem Ordner verarbeitet werden sollen.
- Anpassungen: Alle Konfigurationseinstellungen, die Sie auf den Ordner anwenden, werden von den Knoten in dem Ordner übernommen. Wenn Sie beispielsweise die Abschweifungseinstellungen für den Ordner ändern, werden diese Änderungen für alle Knoten im Ordner übernommen.
- Baumhierarchie: Die Knoten in einem Ordner werden als Stammknoten oder untergeordnete Knoten behandelt, je nachdem, ob der Ordner auf der Stammebene oder der untergeordneten Ebene in der Baumstruktur des Dialogmoduls hinzugefügt wird. Alle Knoten auf der Stammebene, die Sie zu einem Ordner der Stammebene hinzufügen, werden weiterhin als Stammknoten behandelt; d. h. sie werden nicht zu untergeordneten Knoten des Ordners. Wenn Sie jedoch einen Knoten der Stammebene in einen Ordner verschieben, der einem anderen Knoten untergeordnet ist, dann wird der Stammknoten zu einem untergeordneten Element dieses anderen Knotens.

Ordner haben keine Auswirkung auf die Reihenfolge, in der Knoten verarbeitet werden. Die Knoten werden weiterhin vom ersten bis zum letzten Knoten nacheinander verarbeitet. Wenn der Service beim Durchschreiten der Baumstruktur einen Ordner ohne Bedingung findet oder mit einer Bedingung, die als 'true' ausgewertet wird, wird sofort der erste Knoten in dem Ordner verarbeitet und danach die weiteren Elemente der Baumstruktur in der angegebenen Reihenfolge. Ein Ordner, der keine Ordnerbedingung enthält, ist für den Service transparent. In diesem Fall wird jeder Knoten in dem Ordner wie jeder andere einzelne Knoten in der Baumstruktur behandelt.

### Ordner hinzufügen
{: #dialog-build-folders-add}

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

      Wenn Sie Ordner verschieben, werden diese am Anfang der Baumstruktur im Ordner hinzugefügt. Wenn Sie beim Verschieben mehrerer aufeinanderfolgender Stammdialogmodulknoten die Reihenfolge beibehalten möchten, müssen Sie die Knoten in umgekehrter Reihenfolge verschieben (d. h. beginnend mit dem letzten Knoten).
      {: tip}

    - Um einen neuen Dialogmodulknoten zu dem Ordner hinzuzufügen, klicken Sie auf das Symbol **Mehr** ![Symbol 'Mehr'](images/kabob.png) für den Ordner und wählen Sie dann die Option **Knoten zum Ordner hinzufügen** aus.

      Der Dialogmodulknoten wird in dem Ordner am Ende der Baumstruktur des Dialogmoduls hinzugefügt.

### Ordner löschen
{: #dialog-build-folders-delete}

Sie können einen Ordner entweder allein oder mit allen darin enthaltenen Dialogmodulknoten löschen.

So löschen Sie einen Ordner:

1.  Suchen Sie in der Baumstrukturansicht der Registerkarte **Dialogmodul** den Ordner, den Sie löschen möchten.

1.  Klicken Sie auf das Symbol **Mehr** ![Symbol 'Mehr'](images/kabob.png) für den Ordner und wählen Sie dann **Löschen** aus.

1.  Führen Sie eine der folgenden Aktionen aus:

    - Um nur den Ordner zu löschen und die darin enthaltenen Dialogmodulknoten beizubehalten, wählen Sie das Kontrollkästchen **Knoten im Ordner löschen** auf und klicken Sie dann auf **Jetzt löschen**.
    - Um den Ordner und alle darin enthaltenen Dialogmodulknoten zu löschen, klicken Sie auf **Jetzt löschen**.

Wenn Sie nur den Ordner (ohne die enthaltenen Knoten) gelöscht haben, werden die zugehörigen Knoten in der Baumstruktur des Dialogmoduls an der Stelle angezeigt, an der sich Ordner vor dem Löschen befand.
