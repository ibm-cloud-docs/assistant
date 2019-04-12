---

copyright:
  years: 2015, 2019
lastupdated: "2019-02-11"

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

# In webbasiertes Chat-Widget integrieren
{: #deploy-web-link}

Wenn Sie den Vorschaulink nicht inaktivieren, ist der Assistent sofort auf einer Webseite zum Testen verfügbar.
{: shortdesc}

Der Assistent wird als Chat-Widget automatisch auf einer Webseite mit IBM Schutzmarke implementiert. Sie können den von Ihnen zum Assistenten hinzugefügten Dialogskill testen, indem Sie Text in das Chat-Widget eingeben. Außerdem können Sie die URL der Seite mit anderen Benutzern teilen, um Unterstützung beim Testen sowie Feedback zum Assistenten zu erhalten.

Im Unterschied zum Testen mithilfe der Anzeige 'Ausprobieren' des Tools, fallen Gebühren für alle API-Aufrufe an, die aus Ihren Interaktionen mit dem auf der Vorschaulink-URL gehosteten Assistenten resultieren. 

## Assistenten mit der Vorschaulink-Integration testen
{: #deploy-web-link-try}

So testen Sie den Assistenten mit einem webbasierten Chat-Widget:

1.  Klicken Sie in der Registerkarte 'Assistenten' auf die Kachel für den Assistenten, den Sie testen möchten.

1.  Klicken Sie im Abschnitt *Integrationen* auf die Kachel **Vorschaulink**.

    Wenn Sie den Vorschaulink beim Erstellen des Assistenten nicht aktiviert haben, klicken Sie auf **Integration hinzufügen**, klicken Sie auf die Kachel für die **Preview Link**-Integration und klicken Sie anschließend auf **Erstellen**.

1.  **Optional**: Ändern Sie den Namen und die Beschreibung der Webseite für die Vorschau.

1.  Klicken Sie auf den angezeigten URL-Link, um die Testseite zu öffnen.

    Im Web-Browser wird eine neue Registerkarte mit einer Chat-Widget-Implementierung Ihres Assistenten geöffnet. 

    Wenn Ihre Serviceinstanz im Rechenzentrum des Vereinigten Königreichs vor dem 13. Dezember 2018 oder in Sydney vor dem 7. Mai 2018 erstellt wurde, müssen Sie die URL für den Vorschaulink bearbeiten. Die URL enthält einen Standortcode für das Rechenzentrum, in dem die Instanz gehostet wird. Da Instanzen in London und Sydney vor den angegebenen Stichtagen aus Dallas syndiziert wurden, müssen Sie die Angabe `eu-gb` oder `au-syd` in der URL durch `us-south` ersetzen, damit die Webseite korrekt dargestellt wird.
    {: important}

1.  Übergeben Sie Testnachrichten, um zu prüfen, wie der Assistent reagiert.

    Antworten werden erst zurückgegeben, nachdem Sie einen Dialogskill erstellt und im Assistenten hinzugefügt haben.

    Der Dialogablauf für die aktuelle Sitzung wird nach einer Inaktivitätszeit von 60 Minuten erneut gestartet (5 Minuten bei Lite- und Standard-Plänen). Mit anderen Worten: Wenn ein Benutzer 60 bzw. 5 Minuten lang nicht mit dem Assistenten interagiert, werden alle während des vorherigen Dialogs gesetzten Werte für Kontextvariablen auf Null oder auf die jeweiligen Standardwerte zurückgesetzt.

1.  Nach dem Test können Sie die Registerkarte im Browser schließen, um die öffentlich zugängliche Webseite zu verlassen. 

1.  Klicken Sie im Tool auf **Änderungen speichern**, um alle Änderungen zu speichern, die Sie in der Vorschaulink-Integration vorgenommen haben, und die Seite zu schließen. Alternativ können Sie auf **X** klicken, um die Seite zu schließen, ohne die Änderungen zu speichern.

## Hinweise zu Dialogen
{: #deploy-web-link-dialog}

Die erweiterten Antworten, die Sie zu einem Dialogmodul hinzufügen, werden in dem webbasierten Chat-Widget wie erwartet angezeigt. Dabei gelten jedoch die folgenden Ausnahmen: 

- **Servicemitarbeiter kontaktieren**: Dieser Antworttyp wird ignoriert.

- **Pause**: Dieser Antworttyp unterbricht die Aktivität des Assistenten im Chat-Widget an. Die Aktivität wird nach der Unterbrechung jedoch nur fortgesetzt, wenn danach ein weiterer Antworttyp ausgelöst wird. Fügen Sie bei jeder Verwendung des Antworttyps `pause` nach diesem Antworttyp einen weiteren, anderen Antworttyp (z. B. `text`) hinzu.

Weitere Informationen zu Antworttypen enthält der Abschnitt [Erweiterte Antworten](/docs/services/assistant?topic=assistant-dialog-overview#dialog-overview-multimedia).

## Vorschaulink-Integration hinzufügen
{: #deploy-web-link-add-more}

Wenn Sie die automatisch erstellte Vorschaulink-Integration versehentlich gelöscht haben oder eine andere öffentliche Webseite erstellen möchten, können Sie eine Vorschaulink-Integration hinzufügen. 

1.  Klicken Sie in der Registerkarte 'Assistenten' auf die entsprechende Kachel, um den Assistenten zu öffnen, den Sie bereitstellen möchten. 

1.  Klicken Sie im Abschnitt **Integrationen** auf **Integration hinzufügen**.

1.  Klicken Sie auf die Kachel für die **Vorschaulink**-Integration.

1.  Optional können Sie den Namen und die Beschreibung der Vorschaulink-Integration bearbeiten. Klicken Sie anschließend auf **Erstellen**.

    Auf der Seite wird eine URL hinzugefügt. Klicken Sie auf diese URL, um die Webseite mit der Vorschau zu öffnen.
