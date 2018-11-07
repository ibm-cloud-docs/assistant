---

copyright:
  years: 2015, 2018
lastupdated: "2018-02-20"

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

# Releaseinformationen

## Versionierung der Service-API
{: shortdesc}

API-Anforderungen erfordern einen Parameter 'version', der das Datum im Format `version=JJJJ-MM-TT` annimmt. Bei jeder abwärtskompatiblen Änderung der API wird eine neue untergeordnete Version der API freigegeben.

Senden Sie den Parameter 'version' in jeder API-Anforderung. Der Service verwendet die API-Version des von Ihnen angegebenen Datums oder die letzte Version vor diesem Datum. Verwenden Sie nicht standardmäßig das aktuelle Datum. Geben Sie stattdessen ein Datum an, das mit einer Version übereinstimmt, die mit Ihrer App kompatibel ist, und ändern Sie es erst dann, wenn Ihre App für eine neuere Version bereit ist.

Die aktuelle Version ist `2018-02-16`.

In der Anzeige 'Ausprobieren' des {{site.data.keyword.conversationshort}}-Tools wird die aktuelle API-Version verwendet.

## Features als Betaversion

IBM gibt Services, Features und Sprachunterstützung für Sie zum Bewerten frei, die als Betaversion klassifiziert sind. Diese Services können unter Umständen instabil sein, häufig geändert werden und nach Ankündigung kurzfristig eingestellt werden. Außerdem bieten Beta-Features möglicherweise nicht dieselben Leistungswerte oder Kompatibilität wie allgemein verfügbare Features und sind nicht für die Verwendung in einer Produktionsumgebung bestimmt. Beta-Features werden nur in [developerWorks Answers ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://developer.ibm.com/answers/topics/watson-conversation/){: new_window} unterstützt.

## Aktualisierte Modelle
{: #updated-models}

Die {{site.data.keyword.conversationshort}}-Algorithmen können aufgrund von Rückmeldungen, wissenschaftlichen Verbesserungen und weiteren Faktoren regelmäßig optimiert und aktualisiert werden, um die Leistung kontinuierlich zu verbessern. Aktualisierungen des Modells werden in den vorliegenden Releaseinformationen mitgeteilt.

Vorhandene Modelle, die Sie trainiert haben, sind nicht unmittelbar betroffen. Abgelaufene Modelle werden jedoch 60 Tage nach Beginn der Verfügbarkeit eines neuen Modells auf das aktuelle Modell aktualisiert, wenn Sie dies bis dahin nicht ausgeführt haben.

> **Hinweis:** Diese Aktualisierungsaussage gilt nur für Sprachen und Features mit dem Status 'GA' (= allgemein verfügbar).

## Änderungen
{: #change-log}

Die folgenden neuen Features und Änderungen am Service sind verfügbar.

### 16. Februar 2018
{: #16February2018}

- **Tracing für Dialogmodulknoten**: Beim Testen eines Dialogmoduls in der Anzeige 'Ausprobieren' wird neben jeder Antwort ein Positionssymbol angezeigt. Durch Klicken auf dieses Symbol können Sie den Pfad hervorheben, den der Service in der Baumstruktur des Dialogmoduls durchlaufen hat, um zu der Antwort zu gelangen. Details finden Sie unter [Dialogmodul erstellen](dialog-build.html#test).

- **Neue API-Version**: Die aktuelle API-Version ist jetzt `2018-02-16`. Mit dieser Version werden die folgenden Änderungen eingeführt:

  - In den meisten GET-Anforderungen wird jetzt ein neuer Parameter `include_audit` unterstützt. Dieser optionale boolesche Parameter gibt an, ob die Antwort die Prüfeigenschaften (Zeitmarken `created` und `updated`) enthalten soll. Der Standardwert ist `false`. (Wenn Sie eine API-Version vor `2018-02-16` verwenden, ist `true` der Standardwert.) Weitere Informationen enthält die [API-Referenz ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://www.ibm.com/watson/developercloud/conversation/api/v1/){: new_window}.

  - Antworten von API-Aufrufen, die die neue API-Version verwenden, enthalten nur Eigenschaften mit Werten ungleich `null`.

  - Die Eigenschaften `output.nodes_visited` und `output.nodes_visited_details` in Nachrichtenantworten schließen jetzt auch Knoten der folgenden Typen ein, die bislang ausgelassen wurden:

    - Knoten mit `type`=`response_condition`
    - Knoten mit `type`=`event_handler` und `event_name`=`input`

### 9. Februar 2018
{: #9February2018}

- **Niederländische Systementitäten (Betaversion)**: Die Sprachunterstützung für Niederländisch wurde erweitert und enthält jetzt die Verfügbarkeit von [Systementitäten](system-entities.html) als Betaversion. Details finden Sie im Abschnitt [Unterstützte Sprachen](lang-support.html).

### 29. Januar 2018
{: #29January2018}

- Die REST-API von {{site.data.keyword.conversationshort}} unterstützt jetzt die folgenden neuen Anforderungsparameter:
  - `append` - Verwenden Sie diesen Parameter beim Aktualisieren eines Arbeitsbereiches, um anzugeben, ob die vorhandenen Daten durch die neuen Arbeitsbereichsdaten ergänzt oder ersetzt werden sollen. Weitere Informationen finden Sie unter [Arbeitsbereich aktualisieren![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://www.ibm.com/watson/developercloud/conversation/api/v1/#update_workspace){: new_window}.
  - `nodes_visited_details` - Verwenden Sie diesen Parameter beim Senden einer Nachricht, um anzugeben, ob die Antwort zusätzliche Diagnoseinformationen zu den Knoten enthalten soll, die beim Verarbeiten der Nachricht besucht wurden. Weitere Informationen finden Sie unter [Nachricht senden![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://www.ibm.com/watson/developercloud/conversation/api/v1/#send_message){: new_window}.

### 23. Januar 2018
{: #23January2018}

- **Liste der Arbeitsbereiche kann nicht abgerufen werden**: Wenn beim Arbeiten mit dem Tool diese oder eine ähnlich lautende Fehlernachricht angezeigt wird, kann dies bedeuten, dass Ihre Sitzung abgelaufen ist. Melden Sie sich ab, indem Sie die Funktion **Abmelden** über das Symbol **Benutzerinformationen** ![Menü des Symbols 'Benutzerinformationen'](images/user-icon.png) auswählen, und melden Sie sich danach wieder an.

### 8. Dezember 2017
{: #8December2017}

- **Zugriff auf Protokolldaten in mehreren Instanzen (nur Benutzer der Kategorie 'Premium')**: Wenn Sie ein Benutzer der Premiumversion von {{site.data.keyword.conversationshort}} sind, können Ihre Premiuminstanzen optional so konfiguriert werden, dass der Zugriff auf die Protokolldaten über Arbeitsbereiche Ihrer verschiedenen Premiuminstanzen möglich ist.

- **Knoten kopieren**: Sie können jetzt einen Knoten duplizieren, um eine Kopie des Knotens und der zugehörigen untergeordneten Elemente zu erstellen. Diese Funktion ist hilfreich, wenn Sie einen Knoten mit nützlicher Logik erstellt haben, die Sie an anderer Stelle in Ihrem Dialogmodul wiederverwenden möchten. Weitere Informationen finden Sie unter [Dialogmodulknoten kopieren](dialog-build.html#copy-node).

- **Gruppen in Musterentitäten erfassen**: Sie können Gruppen in dem regulären Ausdrucksmuster identifizieren, das Sie für eine Entität definieren. Das Identifizieren von Gruppen ist hilfreich, wenn Sie später auf Teilbereiche des Musters verweisen möchten. Angenommen, Ihre Entität verfügt über ein Muster für einen regulären Ausdruck zum Erfassen von US-Telefonnummern. Wenn Sie das Vorwahlsegment des Musters für Telefonnummern als eine Gruppe identifizieren, können Sie später auf diese Gruppe verweisen, um allein auf das Vorwahlsegment einer Telefonnummer zuzugreifen. Weitere Informationen finden Sie unter [Entitäten definieren](entities.html#creating-entities).

### 6. Dezember 2017
{: #6December2017}

- **{{site.data.keyword.openwhisk}}-Integration (Betaversion)**: Rufen Sie {{site.data.keyword.openwhisk}}-Aktionen (früher als IBM OpenWhisk bezeichnet) direkt aus einem Dialogmodulknoten auf. Diese Funktion ermöglicht beispielsweise das Aufrufen einer Aktion zum Abrufen von Wetterinformationen über einen Dialogmodulknoten und das Verwenden der zurückgegebenen Informationen als Bedingung in der Antwort des Dialogmoduls. Gegenwärtig können Sie eine Aktion über eine {{site.data.keyword.openwhisk_short}}-Instanz, die in der Region 'Vereinigte Statten (Süden)' gehostet wird, über {{site.data.keyword.conversationshort}} -Instanzen aufrufen, die in der Region 'Vereinigte Staaten (Süden)' gehostet werden. Weitere Details finden Sie unter [Programmgesteuerte Aufrufe über einen Dialogmodulknoten absetzen](dialog-actions.html).

### 5. Dezember 2017
{: #5December2017}

- **Überarbeitete Benutzerschnittstelle für Absichten und Entitäten**: Die Registerkarten `Absichten` und `Entitäten` wurden neu gestaltet und ermöglichen jetzt einen einfacheren und effizienteren Workflow beim Erstellen und Bearbeiten von Entitäten und Absichten. Weitere Informationen zum Arbeiten mit diesen Registerkarten finden Sie unter [Absichten definieren](intents.html#defining-intents) und [Entitäten definieren](entities.html#defining-entities).

### 30. November 2017
{: #30November2017}

- **Unterstützung für ostarabische Zahlen**: Für Systementitäten in arabischer Sprache werden jetzt ostarabische Zahlen unterstützt.

### 29. November 2017
{: #29November2017}

- **Erkennung der Benutzereingaben durch mehrere Arbeitsbereiche verbessern**: Sie können jetzt einen Arbeitsbereich durch Äußerungen verbessern, die an andere Arbeitsbereiche in Ihrer Instanz gesendet wurden. Beispiel: Wenn Sie über mehrere Versionen von Produktions- und Entwicklungsarbeitsbereichen verfügen, können Sie dieselben Äußerungsdaten verwenden, um beliebige dieser Arbeitsbereiche zu verbessern. Weitere Informationen finden Sie unter [Verbesserungen für mehrere Arbeitsbereiche](logs.html#deploy_id) und [Datenquelle auswählen](logs_convo.html#select-source).

### 20. November 2017
{: #20November2017}

- **Konformität mit GB18030**: GB18030 ist eine chinesische Norm, die eine erweiterte Codepage für die Verwendung im chinesischen Markt spezifiziert. Diese normierte Codepage ist für die Softwarebranche von großer Bedeutung, da vom China National Information Technology Standardization Technical Committee verfügt wurde, dass jede Softwareanwendung, die nach dem 1. September 2001 im chinesischen Markt freigegeben wird, für GB18030 aktiviert sein muss. Der Service '{{site.data.keyword.conversationshort}}' unterstützt diese Codierung und ist für die Einhaltung von GB18030 zertifiziert.

### 9. November 2017
{: #9November2017}

- **Direkte Referenzierung von Entitäten in Beispielen für Absichten**: Sie können jetzt eine direkte Referenz auf eine Entität in einem Beispiel für Absichten angeben. Diese Entitätsreferenz und alle zugehörigen Werte oder Synonyme werden vom Klassifikationsmerkmal für den Service '{{site.data.keyword.conversationshort}}' zum Trainieren der Absicht verwendet. Weitere Informationen finden Sie unter [*Entität als Beispiel*](intents.html#entity-as-example) im Abschnitt [Absichten](intents.html).

  **Hinweis**: Gegenwärtig können Sie nur von Ihnen definierte geschlossene Entitäten direkt referenzieren. Sie können keine [Musterentitäten](entities.html#pattern-entities) oder [Systementitäten](system-entities.html) direkt referenzieren.

### 8. November 2017
{: #8November2017}

- **{{site.data.keyword.conversationshort}}-Connector**: >Mit dem neuen {{site.data.keyword.conversationshort}}-Connector-Tool können Sie Ihren Arbeitsbereich mit einer eigenen Slack- oder Facebook Messenger-App verbinden und als Chat-Bot für die Interaktion mit Benutzern von Slack oder Facebook Messenger bereitstellen. Dieses Tool ist nur für die {{site.data.keyword.Bluemix_notm}}-Region 'Vereinigte Staaten (Süden)' verfügbar. Weitere Informationen finden Sie unter [Mit dem {{site.data.keyword.conversationshort}}-Connector in einem Kanal bereitstellen](conversation-connector.html).

### 3. November 2017
{: #3November2017}

- **Aktualisierungen für Dialogmodule**: Die folgenden Aktualisierungen erleichtern Ihnen das Erstellen eines Dialogmoduls. (Details finden Sie unter [Dialogmodul erstellen](dialog-build.html).)

    - Sie können eine Bedingung für einen Slot hinzufügen, damit der Slot nur erforderlich ist, wenn bestimmte Bedingungen erfüllt sind. Beispiel: Ein Slot, der den Namen eines Ehegatten abfragt, soll nur dann als erforderlich eingestuft werden, wenn ein vorheriger (erforderlicher) Slot, der den Familienstand abfragt, darauf hindeutet, dass der Benutzer verheiratet ist.

    - Sie können jetzt **Benutzereingabe überspringen** als nächsten Schritt für einen Knoten auswählen. Wenn diese Option ausgewählt wird, springt der Service nach der Verarbeitung des aktuellen Knotens direkt zum ersten untergeordneten Knoten des aktuellen Knotens. Diese Option entspricht weitgehend der vorhandenen Option *Springen zu* für nächste Schritte, sie bietet jedoch mehr Flexibilität. Sie müssen nicht genau angeben, zu welchem Knoten gesprungen werden soll. Der Service springt während der Laufzeit stets zum ersten zugehörigen untergeordneten Knoten, selbst wenn nach dem Definieren des Verhaltens für nächste Schritte die Reihenfolge der untergeordneten Knoten geändert wurde oder neue Knoten hinzugefügt wurden.

    - Sie können bedingte Antworten für Slots hinzufügen. Bei den Antworten für 'Gefunden' und für 'Nicht gefunden' können Sie anpassen, wie der Service reagiert, wenn bestimmte Bedingungen erfüllt sind. Diese Funktion ermöglicht das Erkennen und Korrigieren potenzieller Missverständnisse, bevor der vom Benutzer bereitgestellte Wert in der Kontextvariablen für den Slot gespeichert wird. Beispiel: Wenn der Slot das Alter des Benutzers speichert und die Variable `@sys-number` im Feld *Überprüfen auf* verwendet, um diese Information zu erfassen, können Sie eine Bedingung hinzufügen, die Zahlen größer als 100 findet und eine Antwort wie die folgende zurückgibt: *Bitte geben Sie eine gültiges Altersangabe in Jahren an.* Weitere Details finden Sie unter [Bedingungen zu Antworten für 'Gefunden' und 'Nicht gefunden' hinzufügen](dialog-slots.html#slot-handler-next-steps).

    - Die Schnittstelle zum Hinzufügen von Antworten zu einem Knoten wurde überarbeitet um das Auflisten der einzelnen Bedingungen und der zugehörigen Antworten zu vereinfachen. Um bedingte Antworten auf Knotenebene hinzuzufügen, klicken Sie auf **Anpassen** und aktivieren Sie anschließend die Option **Mehrere Antworten**.

     **Hinweis**: Das Umschaltsteuerelement **Mehrere Antworten** aktiviert bzw. inaktiviert die Funktion nur für die Antwort auf Knotenebene. Es steuert nicht die Möglichkeit zum Hinzufügen bedingter Antworten für einen Slot. Die Einstellung für mehrere Antworten in Slots wird separat gesteuert.

    - Damit die Seite zum Bearbeiten eines Slots übersichtlich bleibt, stehen jetzt Menüoptionen für folgende Aufgaben zur Auswahl: a) eine Bedingung hinzufügen, die erfüllt sein muss, damit der Slot verarbeitet wird und b) bedingte Antworten für die Bedingungen für 'Gefunden' und 'Nicht gefunden' in einem Slot hinzufügen. Wenn Sie diese Zusatzfunktion nicht ausdrücklich hinzufügen, werden die Felder für Slotbedingung und für mehrere Antworten ausgeblendet, um die Seite übersichtlicher und benutzerfreundlicher zu gestalten.

### 25. Oktober 2017
{: #25October2017}

- **Aktualisierungen für vereinfachtes Chinesisch**: Die Sprachunterstützung für vereinfachtes Chinesisch wurde verbessert. Dies beinhaltet Verbesserungen für die Absichtsklassifizierung mithilfe von Worteinbettungen auf Zeichenebene und die Verfügbarkeit von Systementitäten. Die Lernmodelle des Service '{{site.data.keyword.conversationshort}}' wurden im Rahmen dieser Verbesserung ebenfalls aktualisiert und bei einem erneuten Training des Modells werden alle Änderungen angewendet. Weitere Informationen finden Sie unter [Aktualisierte Modelle](release-notes.html#updated-models).
- **Aktualisierungen für Spanisch:** Die Absichtsklassifizierung für Spanisch wurde für sehr große Datensätze verbessert.

### 11. Oktober 2017
{: #11October2017}

- **Aktualisierungen für Koreanisch**: Die Sprachunterstützung für Koreanisch wurde verbessert. Die Lernmodelle des Service '{{site.data.keyword.conversationshort}}' wurden im Rahmen dieser Verbesserung ebenfalls aktualisiert und bei einem erneuten Training des Modells werden alle Änderungen angewendet. Weitere Informationen finden Sie unter [Aktualisierte Modelle](release-notes.html#updated-models).

### 3. Oktober 2017
{: #3October2017}

- **Musterdefinierte Entitäten (Betaversion):** Sie können nun unter Verwendung von regulären Ausdrücken bestimmte Muster für eine Entität definieren. Dies kann für die Ermittlung von Entitäten von Nutzen sein, die ein definiertes Muster befolgen, z. B. Artikel- bzw. Teilenummern, Telefonnummern oder E-Mail-Adressen. Weitere Details enthält der Abschnitt [Musterdefinierte Entitäten](entities.html#pattern-entities).
    - Sie können entweder Synonyme oder Muster für einen einzelnen Entitätswert hinzufügen, jedoch nicht beides.
    - Für jeden Entitätswert kann es höchstens 5 Muster geben.
    - Jedes Muster (also jeder reguläre Ausdruck) ist auf 128 Zeichen begrenzt.
    - Beim Importieren oder Exportieren über eine CSV-Datei werden Muster gegenwärtig nicht unterstützt.
    - Die REST-API unterstützt keinen direkten Zugriff auf Muster. Sie können Muster jedoch mit dem Endpunkt `/values` abrufen oder ändern.

- **Nach Wörterverzeichnis gefilterte unscharfe Suche (nur bei Englisch):** Für Englisch gibt es jetzt eine verbesserte Version der unscharfen Suche für Entitäten. Diese Verbesserung verhindert die Erfassung einiger gängiger und gültiger englischer Wörter als grobe Übereinstimmungen für eine bestimmte Entität. Bei der unscharfen Suche wird beispielsweise der Entitätswert `like` nicht als Übereinstimmung für die gültigen englischen Wörter `hike` oder `bike` gewertet, jedoch weiterhin beispielsweise für `lkie` oder `oike`.

### 27. September 2017
{: #27September2017}

- **Aktualisierungen beim Bedingungsbuilder:** Das Steuerelement, das angezeigt wird, um Sie beim Definieren einer Bedingung in einem Dialogmodulknoten zu unterstützen, wurde aktualisiert. Zu den Erweiterungen zählen die Unterstützung der Auflistung von verfügbaren Kontextvariablennamen, nachdem Sie das Zeichen $ eingegeben haben, um eine Kontextvariable hinzuzufügen.

### 31. August 2017
{: #31August2017}

- **Verbesserungen des Abschnittsrollbacks**: Die Metrik für die gemittelte Dialogzeit und entsprechende Filter wurden vorübergehend von der Seite 'Übersicht' des Abschnitts 'Verbessern' entfernt. Dies verhindert, dass die Berechnung bestimmter Metriken die Metrik für die gemittelte Dialogzeit verursacht und im Diagramm für Dialoge über einen bestimmten Zeitraum hinweg ungenaue Informationen angezeigt werden. IBM bedauert das Entfernen von Funktionalität aus dem Tool, setzt hierdurch jedoch seine Zusage um, die Bereitstellung von präzisen Informationen für die Benutzer sicherzustellen.
- **Namen von Dialogmodulknoten**: Einem Dialogmodulknoten kann nun ein beliebiger Name zugeordnet werden und der Name muss nicht eindeutig sein. Außerdem kann der Knotenname nachfolgend geändert werden, ohne dass hierdurch die interne Referenzierung des Knotens beeinflusst wird. Der Name, den Sie angeben, wird als Attribut 'title' des Knotens in der JSON-Datei für den Arbeitsbereich gespeichert; das System referenziert den Knoten mithilfe einer eindeutigen ID, die im Attribut 'name' gespeichert wird.

### 23. August 2017
{: #23August2017}

- **Aktualisierungen für Koreanisch, Japanisch und Italienisch**: Die Sprachunterstützung für Koreanisch, Japanisch und Italienisch wurde verbessert. Die Lernmodelle des Service '{{site.data.keyword.conversationshort}}' wurden im Rahmen dieser Verbesserung ebenfalls aktualisiert und bei einem erneuten Training des Modells werden alle Änderungen angewendet. Weitere Informationen finden Sie unter [Aktualisierte Modelle](release-notes.html#updated-models).

### 10. August 2017
{: #10August2017}

- **Akzentnormalisierung**: In einer dialogorientierten Einstellung können Benutzer bei der Interaktion mit dem Service '{{site.data.keyword.conversationshort}}' Akzente verwenden oder nicht. Insofern wurde eine Aktualisierung des Algorithmus vorgenommen, damit Fassungen von Wörtern mit Akzent und ohne Akzent bei der Absichts- und Entitätserkennung identisch behandelt werden.

  Bei einigen Sprachen (z. B. Spanisch) können manche Akzente jedoch die Bedeutung der Entität ändern. Obwohl die Originalentität implizit einen Akzent besitzt, kann der Service daher bei der Entitätserkennung auch eine Übereinstimmung mit der Version derselben Entität ohne Akzent erzielen, allerdings mit einer etwas geringeren Konfidenzbewertung.

  Beispiel: Für das Wort `barrió` mit Akzent, das der Vergangenheitsform des Verbs `barrer` (fegen) entspricht, kann der Service auch eine Übereinstimmung mit dem Wort `barrio` (Nachbarschaft) ermitteln, jedoch mit einer etwas geringeren Konfidenzbewertung.

  Das System stellt die höchsten Konfidenzbewertungen in Entitäten mit exakten Übereinstimmungen bereit. Beispielsweise wird `barrio` nicht erkannt, wenn `barrió` im Trainingsset angegeben ist, und `barrió` wird nicht erkannt, wenn `barrio` im Trainingsset enthalten ist.

  Sie müssen dafür sorgen, dass das System mit den richtigen Zeichen und Akzenten trainiert wird. Falls Sie beispielsweise `barrió` als Antwort erwarten, müssen Sie `barrió` in das Trainingsset aufnehmen.

  Obwohl es sich streng genommen bei der Tilde nicht um ein Akzentzeichen handelt, gilt dasselbe für Wörter, die z. B. den spanischen Buchstaben `ñ` bzw. den Buchstaben `n` verwenden (z. B. `uña` und `una`). In diesem Fall ist der Buchstabe `ñ` nicht einfach ein `n` mit einem Akzent, sondern ein eindeutiger besonderer Buchstabe des Spanischen.

  Sie können die unscharfe Suche aktivieren, wenn Sie vermuten, dass Ihre Kunden möglicherweise nicht die richtigen Akzente verwenden oder Wörter falsch schreiben (beispielsweise `n` statt `ñ` verwenden). Sie können die Varianten aber auch explizit in die Trainingsbeispiele aufnehmen.

  **Hinweis:** Die Akzentnormalisierung ist für Portugiesisch, Spanisch, Französisch und Tschechisch aktiviert.

- **Flag zum Ablehnen von Arbeitsbereichen**: Die REST-API von {{site.data.keyword.conversationshort}} unterstützt jetzt ein Flag für die Ablehnung von Arbeitsbereichen. Dieses Flag gibt an, dass Trainingsdaten für den Arbeitsbereich wie Absichten und Entitäten nicht für allgemeine Serviceverbesserungen durch IBM verwendet werden sollen. Weitere Informationen enthält die [API-Referenz ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://www.ibm.com/watson/developercloud/conversation/api/v1/#data-collection){: new_window}.

### 7. August 2017
{: #7August2017}

- **Interpretation von Datumsangaben mit `nächste/r/s` und `letzte/r/s`**: Der Service '{{site.data.keyword.conversationshort}}' behandelt Datumsangaben mit `letzte/r/s` und `nächste/r/s` so, als ob sich diese auf den unmittelbar letzten oder nächsten Tag beziehen, der in derselben, aber auch in einer vorherigen Woche liegen kann. Weitere Informationen enthält der Abschnitt [Systementitäten](system-entities.html#sys-datetime).

### 3. August 2017
{: #3August2017}

- **Unscharfe Suche für zusätzliche Sprachen (Betaversion)**: Die unscharfe Suche für Entitäten ist jetzt für weitere Sprachen verfügbar (siehe [Unterstützte Sprachen](lang-support.html)).
- **Suche mit teilweiser Übereinstimmung (Betaversion, nur Englisch)**: Bei der unscharfen Suche werden jetzt automatisch auf Teilzeichenfolgen basierende Synonyme vorgeschlagen, die in benutzerdefinierten Entitäten vorhanden sind; und im Vergleich zur exakten Übereinstimmung für die Entität wird eine geringere Konfidenzbewertung zugeordnet. Details finden Sie unter [Unscharfe Suche](entities.html#fuzzy-matching).

### 28. Juli 2017
{: #28July2017}

- Wenn Sie für das Tool Vorgaben für bidirektionale Sprachen festlegen, können Sie nun die Richtung der grafischen Benutzerschnittstelle angeben.
- Das Toolfarbschema wurde aktualisiert und ist nun mit anderen Watson-Services und -Produkten konsistent.

### 19. Juli 2017
{: #19July2017}

- Die REST-API von {{site.data.keyword.conversationshort}} unterstützt jetzt den Zugriff auf Dialogmodulknoten. Weitere Informationen enthält die [API-Referenz ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://www.ibm.com/watson/developercloud/conversation/api/v1/#dialog_nodes){: new_window}.

### 14. Juli 2017
{: #14July2017}

- Die Slotfunktionalität von Dialogmodulen wurde erweitert. Beispielsweise wurde eine Eigenschaft *slot_in_focus* hinzugefügt, damit Sie eine Bedingung definieren können, die nur auf einen einzigen Slot angewendet wird. Details können Sie dem Abschnitt [Informationen mit Slots erfassen](/docs/services/conversation/dialog-slots.html) entnehmen.

### 12. Juli 2017
{: #12July2017}

- **Unterstützung für Tschechisch**: Die Unterstützung der tschechischen Sprache wurde eingeführt. Weitere Details enthält der Abschnitt [Unterstützte Sprachen](lang-support.html).

### 11. Juli 2017
{: #11July2017}

- **In Slack testen**: Mit dem neuen Tool **In Slack testen** können Sie Ihren Arbeitsbereich jetzt ohne großen Aufwand zu Testzwecken als Slack-Bot-Benutzer bereitstellen. Dieses Tool ist nur für die {{site.data.keyword.Bluemix_notm}}-Region 'Vereinigte Staaten (Süden)' verfügbar. Weitere Informationen finden Sie unter [In Slack testen](test-deploy.html).
- **Aktualisierungen für Arabisch**: Die Unterstützung der arabischen Sprache wurde erweitert und beinhaltet jetzt die absolute Bewertung pro Absicht sowie die Möglichkeit, Absichten als irrelevant zu markieren (weitere Details enthält der Abschnitt [Unterstützte Sprachen](lang-support.html)). Die Lernmodelle des Service '{{site.data.keyword.conversationshort}}' wurden im Rahmen dieser Verbesserung ebenfalls aktualisiert und bei einem erneuten Training des Modells werden alle Änderungen angewendet. Weitere Informationen finden Sie unter [Aktualisierte Modelle](release-notes.html#updated-models).

### 23. Juni 2017
{: #23June2017}

- **Aktualisierungen für Koreanisch**: Die Unterstützung der koreanischen Sprache wurde erweitert. Weitere Details enthält der Abschnitt [Unterstützte Sprachen](lang-support.html). Die Lernmodelle des Service '{{site.data.keyword.conversationshort}}' wurden im Rahmen dieser Verbesserung ebenfalls aktualisiert und bei einem erneuten Training des Modells werden alle Änderungen angewendet. Weitere Informationen finden Sie unter [Aktualisierte Modelle](release-notes.html#updated-models).

### 22. Juni 2017
{: #22June2017}

- **Einführung von Slots**: Die Erfassung mehrerer Einzelinformationen von einem Benutzer in einem einzigen Knoten wird jetzt durch das Hinzufügen von Slots vereinfacht. Zuvor mussten mehrere Dialogmodulknoten erstellt werden, um alle möglichen Kombinationen der Möglichkeiten zu berücksichtigen, die Benutzer bei der Bereitstellung von Informationen haben. Mithilfe von Slots können Sie einen einzigen Knoten konfigurieren, der alle vom Benutzer bereitgestellten Informationen speichert und alle benötigten Details anfordert, die vom Benutzer nicht angegeben wurden. Weitere Details enthält der Abschnitt [Informationen mit Slots erfassen](dialog-slots.html).
- **Vereinfachte Baumstruktur von Dialogmodulen:** Die Baumstruktur von Dialogmodulen wurde überarbeitet, um den Bedienungskomfort zu verbessern. Die Baumstrukturansicht ist jetzt kompakter und Sie können einfacher erkennen, an welcher Stelle Sie sich in der Baumstruktur befinden. Außerdem werden die Links zwischen Knoten jetzt so dargestellt, dass Sie die Beziehungen zwischen den Knoten leichter verstehen können.

### 21. Juni 2017
{: #21June2017}

- **Unterstützung für Arabisch**: Die Unterstützung für Arabisch ist jetzt allgemein verfügbar. Details enthält der Abschnitt [Bidirektionale Sprachen konfigurieren](lang-support.html#configuring-bi-directional).
- **Aktualisierungen für Sprachen**: Die Algorithmen des Service '{{site.data.keyword.conversationshort}}' wurden aktualisiert, um die gesamte Sprachunterstützung zu verbessern. Einzelheiten enthält der Abschnitt [Unterstützte Sprachen](lang-support.html).

### 16. Juni 2017
{: #16June2017}

- **Empfehlungen (Betaversion, nur für Premium-Benutzer)**: Die Anzeige 'Verbessern' enthält jetzt auch eine Seite **Empfehlungen**, auf der Verfahren vorgeschlagen werden, mit denen Sie Ihr System verbessern können. Hierzu werden die Dialoge von Benutzern mit Ihrem Chat-Bot analysiert und die aktuellen Trainingsdaten sowie die gegenwärtige Antwortsicherheit Ihres Systems berücksichtigt.

### 14. Juni 2017
{: #14June2017}

- **Unscharfe Suche für zusätzliche Sprachen (Betaversion)**: Die unscharfe Suche für Entitäten ist jetzt für weitere Sprachen verfügbar (siehe [Unterstützte Sprachen](lang-support.html)). Durch eine Aktivierung der unscharfen Suche für eine jeweilige Entität können Sie die Fähigkeit des Service verbessern, Benutzereingabeterms mit einer Syntax zu erkennen, die Ähnlichkeit mit der Entität hat, ohne dass eine exakte Übereinstimmung erforderlich ist. Das Feature kann die Benutzereingabe der entsprechenden Entität trotz einer falschen Schreibweise oder leichten syntaktischen Abweichungen zuordnen. Wenn Sie beispielsweise 'Giraffe' als Synonym einer Entität für ein Tier definieren und die Benutzereingabe den Begriff 'Giraffen' oder Girafe' enthält, kann die unscharfe Suche den Begriff korrekt zur Entität für das Tier zuordnen. Details finden Sie unter [Unscharfe Suche](entities.html#fuzzy-matching).

### 13. Juni 2017
{: #13June2017}

- **Benutzerdialoge**: Die Anzeige 'Verbessern' enthält jetzt eine Seite **Benutzerdialoge** mit einer Liste der Benutzerinteraktionen mit Ihrem Chat-Bot, die nach Schlüsselwort, Absicht, Entität oder Anzahl von Tagen gefiltert werden kann. Sie können einzelne Dialoge öffnen, um Absichten zu korrigieren oder um Entitätswerte bzw. Synonyme hinzuzufügen.
- **Änderung für reguläre Ausdrücke**: Die regulären Ausdrücke, die von SpEL-Funktionen unterstützt werden (z. B. find, matches, extract, replaceFirst, replaceAll und split) wurden geändert. Gruppen von Konstrukten mit regulären Ausdrücken sind nicht mehr zulässig, hierzu zählen Konstrukte mit 'look-ahead', 'look-behind', possessiver Wiederholung und Rückverweisen. Diese Änderung war erforderlich, um ein Sicherheitsrisiko in der ursprünglichen Bibliothek für reguläre Ausdrücke zu verhindern.

### 12. Juni 2017
{: #12June2017}

- Die maximale Anzahl von Arbeitsbereichen, die Sie mit dem Plantyp **Lite** (zuvor 'Free' genannt) erstellen können, wurde von 3 auf 5 erhöht.
- Sie können künftig einem Dialogmodulknoten einen beliebigen Namen zuordnen, der nicht eindeutig sein muss. Außerdem kann der Knotenname nachfolgend geändert werden, ohne dass hierdurch die interne Referenzierung des Knotens beeinflusst wird. Der von Ihnen angegebene Name wird als Aliasname behandelt; das System verwendet eine eigene interne ID, um den Knoten zu referenzieren.
- Die Sprache eines Arbeitsbereichs kann künftig nicht mehr nach seiner Erstellung durch eine Bearbeitung der Arbeitsbereichsdetails geändert werden. Falls Sie die Sprache ändern müssen, können Sie den Arbeitsbereich als JSON-Datei exportieren, die Eigenschaft 'language' aktualisieren und dann die JSON-Datei als neuen Arbeitsbereich importieren.

### 6. Juni 2017
{: #6June2017}

- **Weitere Informationen**: Es gibt eine neue Seite mit *Informationen zu {{site.data.keyword.watson}} {{site.data.keyword.conversationshort}}*, die einführende Informationen und Links zur Servicedokumentation sowie zu anderen nützlichen Ressourcen bietet. Zum Öffnen der Seite klicken Sie auf das Symbol ![i für Informationen](images/info.png) in der Seitenkopfzeile.
- **Massenexport und -löschung**: Sie können jetzt gleichzeitig eine Reihe von Absichten oder Entitäten in eine CSV-Datei exportieren, um sie anschließend zu importieren und für eine andere {{site.data.keyword.conversationshort}}-Anwendung wiederzuverwenden. Sie können ebenfalls gleichzeitig eine Reihe von Entitäten oder Absichten für eine Massenlöschung auswählen.
- **Aktualisierungen für Koreanisch**: Die Tokenizer für Koreanisch wurden aktualisiert, um die Unterstützung von inoffizieller Sprache abzudecken. IBM arbeitet fortlaufend an Verbesserungen bei der Erkennung und Klassifizierung von Entitäten.
- **Emoji-Unterstützung**: Emojis, die zu Beispielen für Absichten oder als Entitätswerte hinzugefügt wurden, werden jetzt korrekt klassifiziert bzw. extrahiert. **Hinweis:** Nur Emojis, die in Ihren Trainingsdaten enthalten sind, werden korrekt und konsistent erkannt; Emojis mit abweichenden Farbtönen oder anderen Variationen werden von der Emoji-Unterstützung möglicherweise nicht korrekt klassifiziert.
- **Normalformenreduktion für Entitäten (Betaversion, nur Englisch)**: Die Betaversion für Features für die unscharfe Suche legt die Stammform des Entitätswerts zu Grunde, um Entitäten zu erkennen und abzugleichen. Dieses Feature erkennt beispielsweise, dass 'bananas' Ähnlichkeit mit 'banana' hat bzw. 'run' mit 'running', da diese Wörter jeweils eine gemeinsame Stammform besitzen. Weitere Informationen finden Sie unter [Unscharfe Suche](entities.html#fuzzy-matching).
- **Verarbeitungsfortschritt beim Arbeitsbereichsimport**: Wenn Sie einen Arbeitsbereich aus einer JSON-Datei importieren, wird sofort eine Kachel für den Arbeitsbereich angezeigt, auf der Sie Informationen zum Verarbeitungsfortschritt des Imports erhalten.
- **Verringerte Trainingsdauer**: Mehrere Modelle werden jetzt parallel trainiert, was die Trainingsdauer für umfangreiche Arbeitsbereiche deutlich reduziert.

### 26. Mai 2017
{: #26May2017}

- Die aktuelle API-Version ist jetzt `2017-05-26`. Mit dieser Version werden die folgenden Änderungen eingeführt:
    - Das Schema von Objekten des Typs 'ErrorResponse' wurde geändert. Diese Änderung betrifft alle Endpunkte und Methoden. Weitere Informationen enthält die [API-Referenz ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://www.ibm.com/watson/developercloud/conversation/api/v1/){: new_window}.
    - Das interne Schema für die Darstellung von Dialogmodulknoten in exportierten JSON-Dateien für Arbeitsbereiche wurde geändert. Falls Sie die API-Version `2017-05-26` verwenden, um einen Arbeitsbereich zu importieren, der mit einer früheren Version exportiert wurde, werden einige Knoten möglicherweise nicht korrekt importiert. Ein Arbeitsbereich sollte daher nach Möglichkeit immer mit derselben Version importiert werden, die auch für seinen Export verwendet wurde.

### 25. Mai 2017
{: #25May2017}

- Kontextvariablen können nun im Fenster 'Ausprobieren' verwaltet werden. Klicken Sie auf den Link **Kontext verwalten**, um ein neues Fenster zu öffnen, in dem Sie die Werte von Kontextvariablen direkt beim Testen des Dialogmoduls festlegen und überprüfen können. Weitere Informationen enthält der Abschnitt [Dialogmodul testen](dialog-test.html).

### 16. Mai 2017
{: #16May2017}

- Beim Öffnen des Tools ist jetzt ein Beispielarbeitsbereich **Car Dashboard** verfügbar. Sie können das Beispiel als Ausgangspunkt für Ihren eigenen Arbeitsbereich verwenden, indem Sie den Arbeitsbereich bearbeiten. Falls Sie das Beispiel für mehrere Arbeitsbereiche verwenden wollen, kopieren Sie es stattdessen. Der Beispielarbeitsbereich wird nur dann bei der Zählung für die Begrenzung der Arbeitsbereiche berücksichtigt, wenn Sie ihn verwenden.
- Die Navigation im Tool ist jetzt einfacher. Die Navigationsmenüoptionen sind jetzt am seitlichen und nicht mehr am oberen Rand der Hauptseite verfügbar. Am Anfang der Seite machen Breadcrumb-Links kenntlich, an welcher Stelle Sie sich befinden. Auf der Seite 'Arbeitsbereiche' können Sie jetzt zwischen Serviceinstanzen wechseln. Wenn Sie im Navigationsmenü auf **Zurück zu Arbeitsbereichen** klicken, können Sie diese Seite direkt aufrufen. Falls mehrere Serviceinstanzen vorhanden sind, wird der Name der aktuellen Instanz angezeigt. Sie können auf den Link **Ändern** neben der Instanz klicken, um eine andere Instanz auszuwählen.
- Wenn Sie ein Dialogmodul erstellen, werden zwei Knoten jetzt automatisch für Sie hinzugefügt: 1. Knoten **Welcome** am Anfang der Baumstruktur des Dialogmoduls, der die Begrüßung enthält, die für den Benutzer angezeigt werden soll. 2. Knoten **Anything else** am Ende der Baumstruktur für das Dialogmodul, der alle Benutzeranfragen erfasst, die von keinem anderen Knoten im Dialogmodul abgefangen werden, und diese Anfragen beantwortet. Weitere Informationen enthält der Abschnitt [Dialogmodul erstellen](dialog-create.html).
- Wenn Sie ein Dialogmodul im Fenster 'Ausprobieren' testen, können Sie nun eine kürzlich verwendete verbale Testäußerung suchen und erneut übergeben, indem Sie die Aufwärtstaste drücken und die vorherigen Eingaben durchblättern.
- Für fünf Systementitäten (`@sys-date`, `@sys-time`, `@sys-currency`, `@sys-number`, `@sys-percentage`) ist jetzt die experimentelle Sprachunterstützung für Koreanisch verfügbar. Für einige der numerischen Entitäten liegen bekannte Probleme vor und die Eingabe inoffizieller Sprache wird nur begrenzt unterstützt.
- Auf der Registerkarte 'Verbessern' gibt es jetzt eine Seite 'Übersicht'. Die Seite bietet eine Zusammenfassung der Interaktionen mit Ihrem Bot. Sie können den Umfang des Datenverkehrs in einem bestimmten Zeitraum ebenso anzeigen wie die Absichten und Entitäten, die am häufigsten in Benutzerdialogen erkannt wurden. Weitere Informationen finden Sie unter [Seite 'Übersicht'](logs_oview.html).

### 27. April 2017
{: #27April2017}

- Die folgenden Systementitäten sind jetzt als Betaversionsfeatures ausschließlich in Englisch verfügbar:
    - sys-location: Erkennt die Referenzierung von Standorten wie Gemeinden, Städten und Ländern in verbalen Äußerungen der Benutzer.
    - sys-person: Erkennt die Referenzierung von Vor- und Nachnamen für Personen in verbalen Äußerungen der Benutzer.

    Weitere Angaben enthalten die [Referenzinformationen zu Systementitäten](system-entities.html).
- Die unscharfe Suche nach Entitäten ist ein Betaversionsfeature, das jetzt in Englisch verfügbar ist. Durch eine Aktivierung der unscharfen Suche für eine jeweilige Entität können Sie die Fähigkeit des Service verbessern, Benutzereingabeterms mit einer Syntax zu erkennen, die Ähnlichkeit mit der Entität hat, ohne dass eine exakte Übereinstimmung erforderlich ist. Das Feature kann die Benutzereingabe der entsprechenden Entität trotz einer falschen Schreibweise oder leichten syntaktischen Abweichungen zuordnen. Wenn Sie beispielsweise **giraffe** als Synonym einer Entität für ein Tier definieren und die Benutzereingabe den Begriff *giraffes* oder *girafe* enthält, kann die unscharfe Suche den Begriff korrekt zur Entität für das Tier zuordnen. Weitere Informationen enthält der Abschnitt [Entitäten definieren](entities.html#fuzzy-matching). Suchen Sie dort nach `unscharfe Suche`, um Details zu erfahren.

### 18. April 2017
{: #18April2017}

- Die REST-API von {{site.data.keyword.conversationshort}} unterstützt jetzt die folgenden Ressourcen:
    - Entitäten
    - Entitätswerte
    - Synonyme für Entitätswerte
    - Protokolle

    Weitere Informationen enthält die [API-Referenz ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://www.ibm.com/watson/developercloud/conversation/api/v1/){: new_window}.
- Beim Verhalten der Methode `POST` für '/messages' wurde die Behandlung von Entitäten und Absichten geändert, die als Teil der Nachrichteneingabe angegeben sind:
    - Falls Sie Absichten in der Eingabe angeben, verwendet der Service die angegebenen Absichten, jedoch die Verarbeitung natürlicher Sprache, um Entitäten in der Benutzereingabe zu erkennen.
    - Falls Sie Entitäten in der Eingabe angeben, verwendet der Service die angegebenen Entitäten, jedoch die Verarbeitung natürlicher Sprache, um Absichten in der Benutzereingabe zu erkennen.

        Für Nachrichten, die sowohl Absichten als auch Entitäten angeben oder die weder Absichten noch Entitäten angeben, wurde das Verhalten nicht geändert.
- Die Option, eine Benutzereingabe als irrelevant zu markieren, ist jetzt für alle unterstützten Sprachen verfügbar. Dieses Feature liegt als Betaversion vor.
- Die neue Registerkarte 'Berechtigungsnachweise' bietet eine zentrale Stelle für alle Informationen, die Sie benötigen, um Ihre Anwendung mit einem Arbeitsbereich zu verbinden (z. B. die Serviceberechtigungsnachweise und die Arbeitsbereichs-ID), sowie weitere Optionen für die Bereitstellung. Klicken Sie zum Zugreifen auf die Registerkarte 'Berechtigungsnachweise' für Ihren Arbeitsbereich auf das Symbol ![Menü](images/Menu_16.png) und wählen Sie **Berechtigungsnachweise** aus.

### 9. März 2017
{: #9March2017}

Die REST-API von {{site.data.keyword.conversationshort}} unterstützt jetzt die folgenden Ressourcen:

- Arbeitsbereiche
- Absichten
- Beispiele
- Gegenbeispiele

Weitere Informationen enthält die [API-Referenz ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://www.ibm.com/watson/developercloud/conversation/api/v1/){: new_window}.

### 7. März 2017
{: #7March2017}

- Die Verwendung von `.` oder `..` als Name einer Absicht verursacht Probleme und wird nicht mehr unterstützt.

    Eine Absicht mit diesem Namen kann weder umbenannt noch gelöscht werden; zur Änderung des Namens müssen Sie Ihre Absichten in eine Datei importieren, die Absicht in der Datei umbenennen und die aktualisierte Datei in Ihren Arbeitsbereich importieren.

    Zahlungspflichtige Kunden können Unterstützung für eine Datenbankänderung anfordern.

### 1. März 2017
{: #1March2017}

- Systementitäten sind jetzt in Deutsch verfügbar.

### 22. Februar 2017
{: #22February2017}

- Nachrichten sind künftig auf 2048 Zeichen begrenzt.

### 3. Februar 2017
{: #3February2017}

- Die Bewertung von Absichten wurde geändert und die Möglichkeit eingeführt, Eingaben als irrelevant für die Anwendung zu markieren. Details enthält der Abschnitt [Absichten definieren](intents.html). Suchen Sie dort nach `Als irrelevant markieren`.

- In diesem Release wurde eine bedeutende Änderung für den Arbeitsbereich eingeführt. Um von dieser Änderung zu profitieren, müssen Sie ein manuelles Upgrade für Ihren Arbeitsbereich durchführen. Führen Sie dazu die folgenden Schritte aus:

  1.  [Duplizieren Sie Ihren Arbeitsbereich](configure-workspace.html#exporting-and-copying-workspaces).
  1.  Aktualisieren Sie den duplizierten Arbeitsbereich, indem Sie auf das Upgradesymbol (![Upgradesymbol](images/upgrade.png)) klicken.
  1.  Testen Sie den Arbeitsbereich nach dem Upgrade.
  1.  Nachdem Sie den Test abgeschlossen haben und der Arbeitsbereich wie erwartet funktioniert, wenden Sie das Upgrade auf Ihre Anwendung an, indem Sie den Nachrichten-API-Aufruf so ändern, dass **2017-02-03** oder eine höhere Version verwendet wird.

- Die Verarbeitung von Aktionen **Springen zu** wurde geändert, um Schleifen zu verhindern, die unter bestimmten Bedingungen auftreten können. Wenn Sie zuvor zur Bedingung eines Knotens gesprungen sind und weder dieser Knoten noch einer seiner Peerknoten eine Bedingung enthielten, die mit 'true' ausgewertet wurde, führte das System die Aktion 'Springen zu' zum Stammknoten aus und suchte nach einem Knoten, dessen Bedingung mit der Eingabe übereinstimmte. In einigen Situationen entstand durch diese Verarbeitung eine Schleife, die eine Fortsetzung des Dialogmoduls verhinderte.

  Wenn beim neuen Prozess weder der Zielknoten noch einer seiner Peerknoten mit 'true' ausgewertet werden, wird die Dialogmodulrunde beendet. Falls Sie das alte Verarbeitungsmodell wiederherstellen möchten, fügen Sie einen finalen Peerknoten mit einer Bedingung `true` hinzu. Verwenden Sie in der Antwort eine Aktion **Springen zu**, deren Ziel die Bedingung des ersten Knotens auf der Stammebene der Baumstruktur Ihres Dialogmoduls ist.

### 11. Januar 2017
{: #11January2017}

- In diesem Release können Sie jetzt Knotentitel im Dialogmodul anpassen.

### 22. Dezember 2016
{: #22December2016}

- In diesem Release wird in Dialogmodulknoten nun ein neuer Abschnitt für den `Knotentitel` angezeigt. Die Anpassung eines `Knotentitels` ist nicht möglich. Bei einem komprimierten Knoten wird als `Knotentitel` die `Knotenbedingung` des Dialogmodulknotens angezeigt. Falls es keine `Knotenbedingung` gibt, wird 'Nicht benannter Knoten' als Titel angezeigt.

### 19. Dezember 2016
{: #19December2016}

Die Verwendung des Dialogmodulbuilders ist jetzt durch mehrere Änderungen einfacher und intuitiver:

- Durch eine größere Bearbeitungsansicht können Sie leichter alle Details eines Knotens einsehen, wenn Sie ihn bearbeiten.
- Ein Knoten kann mehrere Antworten enthalten, die jeweils durch eine eigene Bedingung ausgelöst werden. Weitere Informationen finden Sie unter [Mehrere Antworten](dialog-overview.html#responses).

### 5. Dezember 2016
{: #5December2016}

- Neue Sprachen werden unterstützt (sämtlich im Modus 'Experimentell'): Deutsch, traditionelles Chinesisch, vereinfachtes Chinesisch und Niederländisch.
- Mit '@sys-date' und '@sys-time' sind zwei neue Systementitäten verfügbar. Details finden Sie unter [Systementitäten](system-entities.html).

### 21. Oktober 2016
{: #21October2016}

- Der Service '{{site.data.keyword.conversationshort}}' bietet jetzt mit Systementitäten allgemeine Entitäten, die Sie in beliebigen Anwendungsfällen verwenden können. Details enthält der Abschnitt [Entitäten definieren](entities.html). Suchen Sie dort nach `Systementitäten aktivieren`.
- Sie können jetzt auf der Seite 'Verbessern' ein Protokoll der Dialoge mit Benutzern anzeigen. Anhand dieses Protokolls können Sie das Verhalten des Bots nachvollziehen. Details enthält der Abschnitt [Informationen zur Komponente 'Verbessern'](logs.html).
- Sie können jetzt Entitäten aus einer CSV-Datei importieren, was bei einer großen Anzahl von Entitäten hilfreich ist. Details enthält der Abschnitt [Entitäten definieren](entities.html). Suchen Sie dort nach `Entitäten importieren`.

### 20. September 2016
{: #20September2016}

**Neue Version:** 2016-09-20

Um die Änderungen in einer neuen Version nutzen zu können, ändern Sie den Wert des Parameters `version` in das neue Datum. Falls Sie nicht für eine Aktualisierung auf diese Version bereit sind, ändern Sie das Versionsdatum nicht.

- Version **2016-09-20**: `dialog_stack` wurde aus einem Array von Zeichenfolgen in ein Array von Objekten geändert.

### 29. August 2016
{: #29August2016}

- Sie können Dialogmodulknoten von einer Verzweigung zu einer anderen Verzweigung als gleichgeordnete Elemente oder Peerknoten verschieben. Details enthält der Abschnitt [Dialogmodulknoten verschieben](dialog-build.html#move-node).
- Sie können das Fenster mit dem JSON-Editor erweitern.
- Sie können Chatprotokolle für die Dialoge Ihres Bots anzeigen, um sein Verhalten besser nachzuvollziehen. Sie können nach Absichten, Entitäten, Datum und Uhrzeit filtern. Details enthält der Abschnitt [Informationen zur Komponente 'Verbessern'](logs.html).

### 11. Juli 2016
{: #21July2016}

- Dieses allgemein verfügbare Release ermöglicht Ihnen die Arbeit mit Entitäten und Dialogmodulen und somit die Erstellung eines voll funktionsfähigen Bots.

### 18. Mai 2016
{: #18May2016}

- Dieses experimentelle Release von {{site.data.keyword.conversationshort}} führt die Benutzerschnittstelle ein und ermöglicht Ihnen die Arbeit mit Arbeitsbereichen, Absichten und Beispielen.
