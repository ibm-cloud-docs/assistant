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

# Hilfe bei Watson anfordern
{: #intent-recommendations}

Wenn Sie über Aufzeichnungen mit Chat-Protokollen (z. B. von einem Support-Center) verfügen, die Äußerungen von Benutzern enthalten, können Sie diese Daten mit dem Service nach möglichen Benutzerabsichten durchsuchen. 

## Dateien hochladen
{: #intent-recommendations-log-files-add}

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
  "ich will zuerst wissen, ob ich schon registriert bin."
  ```

Alle Dateien, die Sie hochladen, werden von allen Skills in der aktuellen Serviceinstanz gemeinsam genutzt. Die Äußerungen aus allen verfügbaren Dateien werden durchsucht, wenn Sie Empfehlungen für Benutzerabsichten anfordern. 

## Empfehlungen für Benutzerabsichten abrufen
{: #intent-recommendations-get-example-recommendations}

Das nachfolgende Video bietet in zwei Minuten einen Überblick über die Vorgehensweise zum Anfordern von Empfehlungen für Benutzerabsichten.

<iframe class="embed-responsive-item" id="youtubeplayer" title="Empfehlungen für Benutzerabsichten" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/L3FI8KeZfsc?rel=0" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

In den Dateien, die Sie hochladen, müssen den Beispielempfehlungen keine Absichten zugeordnet werden. Sie müssen lediglich unaufbereitete Benutzeräußerungen bereitstellen. Der Service soll die Äußerungen auswählen, die für die aktuelle Absicht geeignet sind. Der Service analysiert für jede Absicht dieselben Daten, um geeignete Benutzerbeispiele für die jeweilige Absicht zu finden. 

1.  Führen Sie die Schritte im Abschnitt [Absichten erstellen](/docs/services/assistant?topic=assistant-intents#intents-creating-intents-task) aus, um eine Absicht zu erstellen.

1.  Fügen Sie mindestens 5 Benutzerbeispiele hinzu, um die Bandbreite der Benutzeräußerungen für diese Absicht zu veranschaulichen.

    Anhand dieser grundlegenden Beispiele kann der Service einschätzen, welche Arten von Äußerungen in den hochgeladenen Dateien gesucht werden sollen.

1.  Klicken Sie auf **Empfehlungen anzeigen**.

    Die Quellendateien mit Benutzerbeispielen werden von den Skills in einer Serviceinstanz gemeinsam genutzt. Wenn Ihre Kollegen Eigner von Skills in derselben Instanz sind und ebenfalls Dateien hochladen, werden diese Dateien zu Ihrer gemeinsam genutzten Sammlung mit *Quellendateien für Benutzerbeispiele* hinzugefügt.

1.  **Nur beim ersten Mal**: Klicken Sie auf **Dateien hochladen** und anschließend auf **Datei auswählen**, um die CSV-Datei zu suchen und auszuwählen, die Sie zuvor erstellt haben.

    Die hochgeladene Datei kann Äußerungen für verschiedene Arten von Absichten enthalten. Der Service weiß, mit welcher Absicht Sie arbeiten möchten, und findet geeignete Empfehlungen für diese Absicht. 

    Nachdem die Datei hochgeladen und vom Service verarbeitet wurde, werden empfohlene Äußerungen angezeigt. Wenn keine Empfehlungen angezeigt werden, dann enthält die Datei keine geeigneten Beispiele für die betreffende Absicht. 

1.  Wenn die Datei keine hilfreichen Empfehlungen für eine Ihrer Absichten bereitstellen kann, können Sie eine weitere Datei hochladen, um eine andere Sammlung von Äußerungen zu verwenden. Weitere Details enthält der Abschnitt [Hochgeladene Dateien verwalten](#intent-recommendations-manage-files).

1.  Nachdem der Service Empfehlungen bereitgestellt hat, wählen Sie die Äußerungen aus, die Sie als Benutzerbeispiele für diese Absicht hinzufügen möchten, und klicken Sie anschließend auf **Hinzufügen**. Oder klicken Sie auf **Nächste Gruppe**, um weitere Äußerungen zu prüfen.
1.  Wenn Sie den Inhalt der CSV-Datei selbst nach Benutzerbeispielen durchsuchen möchten, klicken Sie auf die Registerkarte **Protokolle durchsuchen**, geben Sie ein Schlüsselwort für die Suche ein und drücken Sie anschließend die **Eingabetaste**.

    Befolgen Sie diese Syntaxrichtlinien für den Suchvorgang: 

    - Boolesche Operatoren (z. B. `AND` und `OR`) werden unterstützt.
    - Suchtext in Anführungszeichen hinzufügen, um exakte Übereinstimmungen zu finden ('diesezeichenfolgemussenthaltensein').
    - Reguläre Ausdrücke wie `*ly` verwenden, um alle Begriffe zu finden, die mit `ly` enden.
    - Die folgenden Zeichen können in regulären Ausdrücken als Operatoren verwendet werden:

      `+ - = && || > < ! ( ) { } [ ] ^ " ~ * ? : \ /`

      Wenn eines dieser Zeichen im Suchbegriff nicht als Operator behandelt werden soll, muss ein Backslash (`\`) vorangestellt werden.

Benutzerbeispiele, die Sie auf diese Weise hinzufügen, werden auf die Summe Ihrer Benutzerbeispiele für Absichten angerechnet, die in dem verwendeten Plan zulässig sind. Weitere Details enthält der Abschnitt [Begrenzungen für Absichten](/docs/services/assistant?topic=assistant-intents#intents-limits).

## Hochgeladene Dateien verwalten
{: #intent-recommendations-manage-files}

Nachdem Sie mindestens eine Datei hochgeladen haben, können Sie die Datensammlung mit *Quellendateien für Benutzerbeispiele* öffnen, um Ihre Dateien zu verwalten. Die hochgeladenen Dateien werden zu einer Dateisammlung hinzugefügt, die innerhalb der aktuellen {{site.data.keyword.conversationshort}}-Serviceinstanz gemeinsam genutzt wird. 

1.  Wenn Sie auf die Datensammlung zugreifen möchten, klicken Sie auf eine Absicht, um sie zu öffnen, klicken Sie auf **Empfehlungen anzeigen** und klicken Sie anschließend in der Seitenleiste auf **Dateien anzeigen**. 

1.  Sie können die folgenden Aktionen ausführen: 

    - Um eine Datei hinzuzufügen, klicken Sie auf **Dateien hinzufügen**, suchen Sie nach der gewünschten Datei und wählen Sie sie aus.
    - Um eine Datei zu löschen, müssen Sie alle hochgeladenen Dateien aus allen Skills entfernen, die der jeweiligen Serviceinstanz zugeordnet sind. Es ist nicht möglich, nur eine Datei zu löschen. Stellen Sie zuerst sicher, dass die Dateien von keinem anderen Benutzer verwendet werden, und klicken Sie anschließend auf **Alle löschen**, um alle hochgeladenen Dateien zu löschen.

1.  Schließen Sie die Seite *Quellendateien für Benutzerbeispiele*.
