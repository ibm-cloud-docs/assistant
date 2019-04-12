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

# Daten sichern und wiederherstellen
{: #backup}

Zum Sichern und Wiederherstellen können Sie die betreffenden Daten exportieren und anschließend importieren.
{: shortdesc}

Sie können die folgenden Datentypen aus einer {{site.data.keyword.conversationshort}}-Serviceinstanz exportieren: 

- Trainingsdaten für Dialogskills (Absichten und Entitäten)
- Dialogskilldaten

Die folgenden Daten können nicht exportiert werden: 

<!--- Search skill -->
- Assistent (einschließlich der konfigurierten Integrationen)

## Protokolle aufbewahren
{: #backup-retain-logs}

Wenn Sie Protokolle von Dialogen zwischen Benutzern und Ihrem Assistenten speichern möchten, können Sie die API `/logs` verwenden, um Ihre Protokolldaten zu exportieren. Weitere Details finden Sie in der [API-Referenz ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://cloud.ibm.com/apidocs/assistant#list-log-events-in-a-workspace).

Um die Arbeitsbereichs-ID für einen Skill abzurufen, klicken Sie in der Kachel für den Skill auf das Symbol ![Liste der Optionen öffnen bzw. schließen](images/kabob-beta.png) und wählen Sie anschließend **API-Details anzeigen** aus.
{: tip}

Der Zeitraum, in dem Protokolle gespeichert werden, variiert je nach Serviceplan. Lite-Pläne stellen beispielsweise nur die Protokolle der letzten 7 Tage bereit. Weitere Informationen enthält der Abschnitt [Begrenzungen für Protokolle](/docs/services/assistant?topic=assistant-logs#logs-limits).

Protokolle können nicht aus einem Skill in einen anderen Skill importiert werden.

## Dialogskill exportieren
{: #backup-export-skill}

Wenn Sie die Daten für einen Dialogskill sichern möchten, exportieren Sie den Skill als JSON-Datei und speichern Sie die JSON-Datei. 

1.  Lokalisieren Sie die Kachel für den Dialogskill auf der Seite 'Skills' oder auf der Konfigurationsseite eines Assistenten, der den Skill verwendet.

1.  Klicken Sie auf das Symbol ![Liste der Optionen öffnen bzw. schließen](images/kabob-beta.png) und wählen Sie anschließend **JSON-Datei herunterladen** aus.

1.  Geben Sie einen Namen und den Speicherort für die JSON-Datei an und klicken Sie anschließend auf **Speichern**.

Alternativ können Sie die API `/workspaces` verwenden, um einen Dialogskill zu exportieren. Geben Sie den Parameter `export=true` in der GET-Anforderung für den Arbeitsbereich an. Weitere Details finden Sie in der [API-Referenz ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://cloud.ibm.com/apidocs/assistant#get-information-about-a-workspace).

## Dialogskill importieren
{: #backup-import-skill}

Um die Sicherungskopie eines Dialogskills wiederherzustellen, die Sie aus einer anderen Serviceinstanz oder Umgebung exportiert haben, erstellen Sie einen neuen Dialogskill, indem Sie die JSON-Datei mit dem exportierten Dialogskill importieren. 

Wenn der {{site.data.keyword.conversationshort}}-Service in der Zeit zwischen dem Exportieren und dem Importieren des Dialogskills geändert wird (z. B. durch funktionelle Aktualisierungen, die auf Instanzen in cloudbasierten Continuous Delivery-Umgebungen regelmäßig angewendet werden), funktioniert der importierte Skill möglicherweise anders als vorher.
{: important}

1.  Klicken Sie auf die Registerkarte **Skills**.

1.  Klicken Sie auf **Neu erstellen**.

1.  Klicken Sie auf **Skill importieren** und anschließend auf **JSON-Datei auswählen** und wählen Sie die JSON-Datei aus, die Sie importieren möchten. 

    **Wichtig:**

    - Die importierte JSON-Datei muss die UTF-8-Codierung ohne Byteanordnungsmarkierung verwenden.
    - Die maximale Größe einer JSON-Datei für einen Skill beträgt 10 MB. Falls Sie einen größeren Skill importieren müssen, kann es sinnvoll sein, die Absichten und Entitäten separat zu importieren, nachdem Sie den Skill importiert haben. (Für den Import von größeren Skills können Sie auch die REST-API nutzen. Weitere Informationen finden Sie in der [API-Referenz ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://cloud.ibm.com/apidocs/assistant#create-workspace){: new_window}.)
    - Die JSON-Datei darf keine Tabstopps, Zeilenumbrüche oder Wagenrückläufe enthalten. 

    Wählen Sie **Alles (Absichten, Entitäten und Dialogmodul)** aus, um eine vollständige Kopie des exportierten Skills zu importieren. 

    Klicken Sie auf **Importieren**.

    Falls beim Importieren eines Skills Fehler auftreten, finden Sie weitere Informationen im Abschnitt [Fehlerbehebung beim Importieren von Skills](/docs/services/assistant?topic=assistant-skill-add#skill-add-import-errors).

1.  Geben Sie die Details für den Skill an: 

    - **Name**: Der Name darf nicht länger als 100 Zeichen sein.. Diese Angabe ist erforderlich. 
    - **Beschreibung**: Die Beschreibung ist optional und darf nicht länger als 200 Zeichen sein. 
    - **Sprache**: Die Sprache der Benutzereingabe, für deren Erkennung der Skill trainiert wird. Der Standardwert ist 'English'.

Der erstellte Skill wird als Kachel auf der Seite 'Skills' angezeigt. 

## Assistent erneut erstellen
{: #backup-recreate-assistant}

Sie können Ihren Assistenten jetzt erneut erstellen. Anschließend können Sie den importierten Dialogskill mit dem Assistenten verknüpfen und zugehörige Integrationen konfigurieren.

Weitere Details enthalten die Abschnitte [Assistent erstellen](/docs/services/assistant?topic=assistant-assistant-add) und [Integrationen hinzufügen](/docs/services/assistant?topic=assistant-deploy-integration-add#deploy-integration-add-task).

## Clientanwendungen aktualisieren
{: #backup-update-api}

Beim Importieren eines Dialogskills, der zuvor exportiert wurde, wird ein neuer Skill erstellt. Der neue Skill verfügt über eine neue Arbeitsbereichs-ID. Wenn Sie mit vorhandenen Clientanwendungen arbeiten, die mithilfe der API der Version 1 auf den Skill zugreifen, müssen Sie alle Verweise auf Arbeitsbereichs-IDs so anpassen, dass die neue Arbeitsbereichs-ID verwendet wird.

Beim erneuten Erstellen Ihres Assistenten wird eine neue Assistenten-ID zugeordnet. Wenn Sie mit vorhandenen Clientanwendungen arbeiten, die mithilfe der API der Version 1 auf den Assistenten zugreifen, müssen Sie alle Verweise auf Assistenten-IDs so anpassen, dass die neue Assistenten-ID verwendet wird. 
