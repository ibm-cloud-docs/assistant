---

copyright:
  years: 2015, 2019
lastupdated: "2019-03-01"

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

# In Slack integrieren
{: #deploy-slack}

Slack ist eine cloudbasierte Messaging-Anwendung zur Unterstützung der Onlinezusammenarbeit von Benutzern.
{: shortdesc}

Nachdem Sie einen Dialogskill konfiguriert und zu einem Assistenten hinzugefügt haben, können Sie den Assistenten in Slack integrieren.

1.  Klicken Sie in der Registerkarte 'Assistenten' auf die entsprechende Kachel, um den Assistenten zu öffnen, den Sie bereitstellen möchten.

1.  Klicken Sie im Abschnitt 'Integrationen' auf **Integration hinzufügen**.

1.  Klicken Sie auf die Schaltfläche **Integration auswählen** für *Slack*.

1.  Befolgen Sie die angezeigten Anweisungen, um den Integrationsprozess abzuschließen. 

Eine schrittweise Demonstration der einzelnen Schritte für die Bereitstellung enthält dieses Video.

<iframe class="embed-responsive-item" id="youtubeplayer" title="Walkthrough of the Slack deployment steps" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/RBGBPJ8h4HQ?rel=0" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

Dauer: 9,5 Minuten

## Hinweise zu Dialogen
{: #deploy-slack-dialog}

Die erweiterten Antworten, die Sie zu einem Dialogmodul hinzufügen, werden in einem Slack-Kanal wie erwartet angezeigt. Dabei gelten jedoch die folgenden Ausnahmen: 

- **Servicemitarbeiter kontaktieren**: Dieser Antworttyp wird ignoriert.

- **Bild**: Ein Bildtitel ist erforderlich. Wenn Sie keinen Titel angeben, wird das Bild nicht angezeigt. 

- **Option**: Dieser Antworttyp zeigt eine Liste der Optionen, die für den Benutzer zur Auswahl stehen.

  - Ein Titel ist **erforderlich** und wird vor der Liste der Optionen angezeigt. 
  - Eine Beschreibung wird nicht angezeigt (unabhängig davon, ob Sie eine Beschreibung angeben). 
  - Wenn ein Benutzer auf eine der Schaltflächen klickt, werden die Auswahlmöglichkeiten ausgeblendet und durch die anhand der Benutzerauswahl generierte Benutzereingabe ersetzt. Wenn Sie mehrere Antworttypen in eine einzige Antwort einfügen, sollten Sie den Antworttyp 'Option' an der letzten Stelle platzieren. Andernfalls enthält die Ausgabe eine Mischung aus Antworten und Benutzereingaben, die für den Benutzer verwirrend sein kann.

- **Pause**: Dieser Antworttyp unterbricht die Aktivität des Assistenten im Slack-Kanal. Es wird jedoch kein Indikator angezeigt, der darauf hinweist (unabhängig davon, ob das Anzeigen von Eingaben aktiviert ist oder nicht).

Weitere Informationen zu Antworttypen enthält der Abschnitt [Erweiterte Antworten](/docs/services/assistant?topic=assistant-dialog-overview#dialog-overview-multimedia).

## Mit dem Assistenten chatten
{: #deploy-slack-try}

So starten Sie einen Chat mit dem Assistenten: 

1.  Öffnen Sie Slack und rufen Sie den Arbeitsbereich auf, der Ihrer App zugeordnet ist.
1.  Klicken Sie im Abschnitt für Anwendungen auf die Anwendung, die Sie erstellt haben.
1.  Chatten Sie mit dem Assistenten.

Der Knoten 'Willkommen' Ihres Dialogmoduls wird von der Slack-Integration nicht verarbeitet. Die Willkommensnachricht wird im Slack-Kanal anders angezeigt als im Fenster 'Ausprobieren' des Tools oder auf der Integrationswebseite 'Linkvorschau'. Dies liegt daran, dass Knoten mit der Sonderbedingung `welcome` in den von Benutzern gestarteten Dialogabläufen übersprungen werden. Slack wartet, bis der Benutzer das Dialogmodul startet. Wenn Sie am Anfang des Dialogs Standardwerte für Kontextvariablen festlegen müssen, sollte dies nicht im Willkommensknoten geschehen. Weitere Informationen enthält der Abschnitt [Dialogmodul starten](/docs/services/assistant?topic=assistant-dialog-start).
{: note}

Der Dialogablauf für die aktuelle Sitzung wird nach einer Inaktivitätszeit von 60 Minuten erneut gestartet (5 Minuten bei Lite- und Standard-Plänen). Mit anderen Worten: Wenn ein Benutzer 60 bzw. 5 Minuten lang nicht mit dem Assistenten interagiert, werden alle während des vorherigen Dialogs gesetzten Werte für Kontextvariablen auf Null oder auf die jeweiligen Standardwerte zurückgesetzt.
