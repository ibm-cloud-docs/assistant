---

copyright:
  years: 2015, 2019
lastupdated: "2019-02-12"

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

# Hilfe zum Erstellen von Absichten anfordern ![Beta,](images/beta.png)![Plus oder Premium](images/premium.png)
{: #beta-intent-recommendations}

Wenn Aufzeichnungen von Chats mit der Kundenunterstützung vorhanden sind, lassen Sie diese Daten von Watson analysieren, um festzustellen, mit welchen Kundenanforderungen sich Ihr Support-Team hauptsächlich befasst.
{: shortdesc}

Diese Feature ist nur für Benutzer verfügbar, die am Betaprogramm teilnehmen. Informationen zum Anfordern des Zugriffs enthält der Abschnitt [Am Betaprogramm teilnehmen](/docs/services/assistant?topic=assistant-feedback#feedback-beta).

![Beta](images/beta.png) IBM gibt Services, Features und Sprachunterstützung für Sie zum Bewerten frei, die als Betaversion klassifiziert sind. Diese Services können unter Umständen instabil sein, häufig geändert werden und nach Ankündigung kurzfristig eingestellt werden. Außerdem bieten Beta-Features möglicherweise nicht dieselben Leistungswerte oder dieselbe Kompatibilität wie allgemein verfügbare Features und sind nicht für die Verwendung in einer Produktionsumgebung bestimmt.

Nach der Freigabe wird diese Funktion für Benutzer des Plus- oder des Premium-Plans verfügbar und kann nur für Äußerungen in englischer Sprache verwendet werden.
{: tip}

Kundenanforderungen werden in {{site.data.keyword.conversationshort}} als *Absichten* dargestellt. Wenn Sie noch keine Absichten definiert haben, können Sie für einen schnelleren Einstieg Watson um Hilfe bitten. Laden Sie Dateien mit Benutzeräußerungen aus Call-Center-Aufzeichnungen hoch, damit sie vom {{site.data.keyword.conversationshort}}-Service analysiert werden können. Abhängig von den gewonnenen Erkenntnissen empfiehlt der Service, welche grundlegenden Absichten Sie erstellen sollten, um die häufigsten Anliegen Ihrer Kunden abzudecken.

Wenn die von den Kunden angesprochenen Themen sich im Laufe der Zeit ändern, können Sie die Funktion zum Empfehlen von Benutzerabsichten verwenden, um die Absichten auf dem aktuellen Stand zu halten.

Analysieren Sie Ihre vorhandenen Daten, um eine der folgenden Tasks auszuführen: 

- [Empfehlungen für Absichten abrufen](#beta-intent-recommendations-get-intent-recommendations)
- [Absichtsempfehlungen für Benutzerbeispiele abrufen](#beta-intent-recommendations-get-example-recommendations)

## Dateien hochladen
{: #beta-intent-recommendations-log-files-add}

Erstellen Sie zuerst eine Datei, die Sie für den Service bereitstellen möchten. Die Datei muss durch Kommas getrennte Werte (Comma-Separated Value, CSV) mit einer Benutzeräußerung pro Zeile enthalten. Die Äußerungen sollten nach Möglichkeit kurze Ausdrücke enthalten, die aus Ihren Call-Center-Aufzeichnungen mit realistischen Kundenfragen und -anfragen extrahiert wurden. Jede Benutzeräußerung darf maximal 20 MB umfassen. 

Befolgen Sie außerdem diese Richtlinien: 

  - Entfernen Sie alle sensiblen Daten aus den Äußerungen, die Sie in die Datei aufnehmen möchten.

    Sensible Daten sind z. B. Informationen zu identifizierbaren natürlichen Personen wie Namen, E-Mail-Adressen und Kunden-IDs sowie schutzwürdige Daten (z. B. geschützte Gesundheitsdaten).
  - Verwenden Sie keine Benutzeräußerungen, die mehr als 1.024 Zeichen umfassen. Längere Äußerungen werden abgeschnitten.
  - Die Datei sollte mindestens 100 Äußerungen enthalten (500 oder mehr Äußerungen liefern noch bessere Ergebnisse).
  - Wenn eine Äußerung ein Komma enthält, schließen Sie die Äußerung in Anführungszeichen ein.
  - Die CSV-Datei darf nur eine einzige Spalte enthalten.
  - Entfernen Sie alle Inhalte, die keine vom Benutzer generierten Äußerungen sind (z. B. Antworten von Servicemitarbeitern und Anmerkungen).

  Beispiel:

  ```
  Was ist mit meinem Versicherungsschutz, wenn ich mein Auto in Zahlung gebe?
  ich  möchte ein haus kaufen.
  Wie kann ich einen Angehörigen in meinen Plan aufnehmen?
  "zuerst will ich wissen, ob ich schon registriert bin."
  ```

Alle Dateien, die Sie hochladen, werden von allen Skills in der aktuellen Serviceinstanz gemeinsam genutzt. Die Äußerungen aus allen verfügbaren Dateien werden durchsucht, wenn Sie sowohl Empfehlungen für Absichten als auch Empfehlungen für Benutzerbeispiele für Absichten anfordern.

## Empfehlungen für Absichten bei Watson anfordern
{: #beta-intent-recommendations-get-intent-recommendations}

Als schnellen Einstieg können Sie Ihre Call-Center-Chataufzeichnungen durch den Service analysieren lassen, um erste Absichtsempfehlungen zu erhalten. Wenn Sie bereits Absichten erstellt haben, kann der Service Ihre Protokolle analysieren und die Ergebnisse mit den vorhandenen Absichten vergleichen, um Lücken in Ihren Trainingsdaten zu erkennen und neue Absichten vorzuschlagen, die diese Lücken füllen können.

Wenn Sie diese Funktion nutzen möchten, laden Sie mindestens eine Datei hoch, die Äußerungen realer Benutzer beim Anfordern von Hilfe enthalten. Die Datei kann eine Liste der Benutzeranfragen enthalten, die beispielsweise aus Call-Center-Protokollen extrahiert wurden. Der Service wertet diese Äußerungen aus und ermittelt Problembereiche, die häufig von Kunden angesprochen werden. Anschließend zeigt das {{site.data.keyword.conversationshort}}-Tool verschiedene Absichten an, die diese Tendenz der Benutzeranforderungen widerspiegeln. Sie können jede empfohlene Absicht und die zugehörigen Benutzerbeispiele überprüfen und auswählen, welche Beispiele zu Ihren Trainingsdaten hinzugefügt werden sollen.

## Empfehlungen für Absichten anfordern
{: #beta-intent-recommendations-get-intent-recommendations-task}

So fordern Sie Empfehlungen für Absichten an: 

1.  Öffnen Sie Ihren Dialogskill im {{site.data.keyword.conversationshort}}-Tool und klicken Sie anschließend in der Navigationsleiste auf die Registerkarte **Absichten**. Falls die Registerkarte **Absichten** nicht angezeigt wird, öffnen Sie die Seite über das Menü ![Menü](images/Menu_16.png).

1.  Klicken Sie auf **Empfehlungen abrufen**.

1.  **Nur beim ersten Mal**: Klicken Sie auf **Datei hinzufügen** und anschließend auf **Datei auswählen**, um die CSV-Datei zu suchen und auszuwählen, die Sie zuvor erstellt haben.

    Geben Sie dem Service Zeit, die Daten zu analysieren und die Äußerungen zu gruppieren.

1.  Prüfen Sie die vom Service empfohlenen Absichten. 

    Die Absichten am Anfang der Liste adressieren die am häufigsten auftretenden Kundenanforderungen. Bewegen Sie den Mauszeiger über eine Absicht, um Beispiele für die zugehörigen Äußerungen anzuzeigen. Um eine Liste aller Absichtsgruppierungen anzuzeigen, klicken Sie auf **Alle Empfehlungen anzeigen**.

1.  Klicken Sie auf eine Absicht, um die vollständige Sammlung der zugehörigen Benutzerbeispiele anzuzeigen und eine der folgenden Aktionen auszuführen: 

    Äußerungen löschen, die nicht als Benutzerbeispiele hinzugefügt werden sollen.

    - Um die empfohlene Absicht mit den ausgewählten Äußerungen als Benutzerbeispiele hinzuzufügen, klicken Sie auf **Absicht erstellen**.
    - Um die ausgewählten Äußerungen aus der empfohlenen Absicht als Benutzerbeispiele zu einer vorhandenen Absicht hinzuzufügen, klicken Sie auf **Zu vorhandener Absicht hinzufügen**, wählen Sie die Absicht aus und klicken Sie anschließend auf **Hinzufügen**.

Absichten und Benutzerbeispiele, die Sie auf diese Weise hinzufügen, werden auf die Gesamtzahl Ihrer Absichten und Benutzerbeispiele für Absichten angerechnet, die in dem verwendeten Plan zulässig sind. Weitere Details enthält der Abschnitt [Begrenzungen für Absichten](/docs/services/assistant?topic=assistant-intents#intents-limits).

## Empfehlungen für die Benutzerbeispiele für Absichten anfordern
{: #beta-intent-recommendations-get-example-recommendations}

Das nachfolgende Video bietet in zwei Minuten einen Überblick über die Vorgehensweise zum Anfordern von Empfehlungen für Benutzerbeispiele für Absichten. 

<iframe class="embed-responsive-item" id="youtubeplayer" title="Empfehlungen für Benutzerbeispiele für Absichten" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/L3FI8KeZfsc?rel=0" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

Zum Abrufen von Benutzerbeispielen für vorhandene Absichten können Sie die Dateien verwenden, die Sie zuvor hochgeladen haben. Sie müssen den Beispielen keine Absichten zuordnen. Sie müssen lediglich unaufbereitete Benutzeräußerungen bereitstellen. Der Service kann die Äußerungen auswählen, die für die aktuelle Absicht geeignet sind. Der Service analysiert für jede Absicht dieselben Daten, um geeignete Benutzerbeispiele für die jeweilige Absicht zu finden. 

1.  Führen Sie die Schritte im Abschnitt [Absichten erstellen](/docs/services/assistant?topic=assistant-intents#intents-creating-intents-task) aus, um eine Absicht zu erstellen.

1.  Fügen Sie mindestens 5 Benutzerbeispiele hinzu, um die Bandbreite der Benutzeräußerungen für diese Absicht zu veranschaulichen.

    Anhand dieser grundlegenden Beispiele kann der Service einschätzen, welche Arten von Äußerungen in den hochgeladenen Dateien gesucht werden sollen.

1.  Klicken Sie auf **Empfehlungen anzeigen**.

    Die Quellendateien mit Benutzerbeispielen werden von den Skills in einer Serviceinstanz gemeinsam genutzt. Wenn Ihre Kollegen Eigner von Skills in derselben Instanz sind und ebenfalls Dateien hochladen, werden diese Dateien zu Ihrer gemeinsam genutzten Sammlung mit *Quellendateien für Benutzerbeispiele* hinzugefügt.

1.  **Nur beim ersten Mal**: Klicken Sie auf **Dateien hochladen** und anschließend auf **Datei auswählen**, um die CSV-Datei zu suchen und auszuwählen, die Sie zuvor erstellt haben.

    Die hochgeladene Datei kann Äußerungen für verschiedene Arten von Absichten enthalten. Der Service weiß, mit welcher Absicht Sie arbeiten möchten, und findet geeignete Empfehlungen für diese Absicht. 

    Nachdem die Datei hochgeladen und vom Service verarbeitet wurde, werden empfohlene Äußerungen angezeigt. Wenn keine Empfehlungen angezeigt werden, dann enthält die Datei keine geeigneten Beispiele für die betreffende Absicht. 

1.  Wenn die Datei keine hilfreichen Empfehlungen für eine Ihrer Absichten bereitstellen kann, können Sie eine weitere Datei hochladen, um eine andere Sammlung von Äußerungen zu verwenden. Weitere Details enthält der Abschnitt [Hochgeladene Dateien verwalten](#beta-intent-recommendations-manage-files).

1.  Nachdem der Service Empfehlungen bereitgestellt hat, wählen Sie die Äußerungen aus, die Sie als Benutzerbeispiele für diese Absicht hinzufügen möchten, und klicken Sie anschließend auf **Hinzufügen**. Oder klicken Sie auf **Nächste Gruppe**, um weitere Äußerungen zu prüfen.
1.  Wenn Sie den Inhalt der CSV-Datei selbst nach Benutzerbeispielen durchsuchen möchten, klicken Sie auf die Registerkarte **Protokolle durchsuchen**, geben Sie ein Schlüsselwort für die Suche ein und drücken Sie anschließend die **Eingabetaste**.

    Befolgen Sie diese Syntaxrichtlinien für den Suchvorgang: 

    - Boolesche Operatoren (z. B. `AND` und `OR`) werden unterstützt.
    - Suchtext in Anführungszeichen hinzufügen, um exakte Übereinstimmungen zu finden ("diesezeichenfolgemussenthaltensein").
    - Reguläre Ausdrücke wie `*ly` verwenden, um alle Begriffe zu finden, die mit `ly` enden.
    - Die folgenden Zeichen können in regulären Ausdrücken als Operatoren verwendet werden:

      `+ - = && || > < ! ( ) { } [ ] ^ " ~ * ? : \ /`

      Wenn eines dieser Zeichen im Suchbegriff nicht als Operator behandelt werden soll, muss ein Backslash (`\`) vorangestellt werden.

Benutzerbeispiele, die Sie auf diese Weise hinzufügen, werden auf die Summe Ihrer Benutzerbeispiele für Absichten angerechnet, die in dem verwendeten Plan zulässig sind. Weitere Details enthält der Abschnitt [Begrenzungen für Absichten](/docs/services/assistant?topic=assistant-intents#intents-limits).

## Hochgeladene Dateien verwalten
{: #beta-intent-recommendations-manage-files}

Nachdem Sie mindestens eine Datei hochgeladen haben, können Sie die Datensammlung mit *Quellendateien für Benutzerbeispiele* öffnen, um Ihre Dateien zu verwalten. Die hochgeladenen Dateien werden zu einer Dateisammlung hinzugefügt, die innerhalb der aktuellen {{site.data.keyword.conversationshort}}-Serviceinstanz gemeinsam genutzt wird. 

1.  Wenn Sie auf die Datensammlung zugreifen möchten, klicken Sie auf eine Absicht, um sie zu öffnen, klicken Sie auf **Empfehlungen anzeigen** und klicken Sie anschließend in der Seitenleiste auf **Dateien anzeigen**. 

    Wenn Sie noch keine Absichten hinzugefügt haben, erweitern Sie auf der Hauptseite 'Absichten' den Abschnitt *Absichtsempfehlungen* und klicken Sie nacheinander auf **Empfehlungen anfordern**, **Alle Empfehlungen anzeigen** und **Dateien anzeigen**.

1.  Sie können die folgenden Aktionen ausführen: 

    - Um eine Datei hinzuzufügen, klicken Sie auf **Dateien hinzufügen**, suchen Sie nach der gewünschten Datei und wählen Sie sie aus.
    - Um eine Datei zu löschen, müssen Sie alle hochgeladenen Dateien aus aus allen Skills entfernen, die der jeweiligen Serviceinstanz zugeordnet sind. Es ist nicht möglich, nur eine Datei zu löschen. Stellen Sie zuerst sicher, dass die Dateien von keinem anderen Benutzer verwendet werden, und klicken Sie anschließend auf **Alle löschen**, um alle hochgeladenen Dateien zu löschen.

1.  Schließen Sie die Seite *Quellendateien für Benutzerbeispiele*.
