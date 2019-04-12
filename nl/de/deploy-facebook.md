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

# In Facebook Messenger integrieren
{: #deploy-facebook}

Facebook Messenger ist eine Messaging-Anwendung für Mobilgeräte, die eine direkte Kommunikation zwischen Unternehmen und Kunden ermöglicht.
{: shortdesc}

Nachdem Sie einen Dialogskill konfiguriert und zu einem Assistenten hinzugefügt haben, können Sie den Assistenten in Facebook Messenger integrieren.

Derzeit ist kein Verfahren zum Identifizieren der Benutzer verfügbar, die über Facebook Messenger mit dem Assistenten interagieren. Dies bedeutet, es gibt keine Möglichkeit, die einem bestimmten Benutzer zugeordneten Daten zu identifizieren oder zu löschen. Verwenden Sie diese Integrationsmethode nicht für Bereitstellungen, die die Datenschutz-Grundverordnung (DSGVO) einhalten müssen. Weitere Details enthält der Abschnitt [Informationssicherheit](/docs/services/assistant?topic=assistant-information-security).
{: important}

1.  Klicken Sie in der Registerkarte 'Assistenten' auf die entsprechende Kachel, um den Assistenten zu öffnen, den Sie bereitstellen möchten.

1.  Klicken Sie im Abschnitt 'Integrationen' auf **Integration hinzufügen**.

1.  Klicken Sie auf die Schaltfläche **Integration auswählen** für *Facebook Messenger*.

1.  Befolgen Sie die angezeigten Anweisungen, um den Integrationsprozess abzuschließen. 

Eine schrittweise Demonstration der einzelnen Schritte für die Bereitstellung enthält dieses Video.

<iframe class="embed-responsive-item" id="youtubeplayer" title="Walkthrough of the Facebook deployment steps" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/8o-FFU5sYNM?rel=0" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

Dauer: 8 Minuten

## Hinweise zu Dialogen
{: #deploy-facebook-dialog}

Die erweiterten Antworten, die Sie zu einem Dialog hinzufügen, werden in einer Facebook-App wie erwartet angezeigt. Dabei gelten jedoch die folgenden Ausnahmen: 

- **Servicemitarbeiter kontaktieren**: Dieser Antworttyp wird ignoriert.

- **Bild**: Dieser Antworttyp fügt ein Bild in die Antwort ein. Vor dem Bild wird weder ein Titel noch eine Beschreibung angezeigt (unabhängig davon, ob Sie diese Elemente angeben). 

- **Option**: Dieser Antworttyp zeigt eine Liste der Optionen, die für den Benutzer zur Auswahl stehen.

  - Ein Titel ist **erforderlich** und wird vor der Liste der Optionen angezeigt. 
  - Es wird keine Beschreibung angezeigt (unabhängig davon, ob Sie eine Beschreibung eingeben).
  - Wenn ein Benutzer auf eine der Schaltflächen klickt, werden die Auswahlmöglichkeiten ausgeblendet und durch die anhand der Benutzerauswahl generierte Benutzereingabe ersetzt. Wenn der Assistent oder Benutzer neue Eingaben angibt, wird die von der Schaltfläche generierte Eingabe ausgeblendet. Beim Einfügen von mehreren Antworttypen in einer einzigen Antwort sollten Sie daher den Ressourcentyp 'Option' an der letzten Stelle platzieren. Andernfalls wird der von der Schaltfläche generierte Text durch die Inhalte der nachfolgenden Antworten ersetzt. 

- **Pause**: Dieser Antworttyp unterbricht die Aktivität des Assistenten im Messenger. Die Aktivität wird nach der Unterbrechung jedoch nur fortgesetzt, wenn danach ein weiterer Antworttyp ausgelöst wird. Fügen Sie bei jeder Verwendung dieses Antworttyps einen weiteren, anderen Antworttyp (z. B. eine Textantwort) hinzu und positionieren Sie ihn nach diesem Antworttyp. 

Weitere Informationen zu Antworttypen enthält der Abschnitt [Erweiterte Antworten](/docs/services/assistant?topic=assistant-dialog-overview#dialog-overview-multimedia).

## Mit dem Assistenten chatten
{: #deploy-facebook-try}

So starten Sie einen Chat mit dem Assistenten: 

1.  Öffnen Sie Facebook Messenger.
1.  Geben Sie den Namen der Seite ein, die Sie zuvor erstellt haben. 
1.  Klicken Sie auf die Seite, sobald sie angezeigt wird, und beginnen Sie einen Chatdialog mit dem Assistenten.

Der Knoten 'Willkommen' Ihres Dialogs wird von der Facebook Messenger-Integration nicht verarbeitet. Die Willkommensnachricht wird im Facebook-Chat anders angezeigt als im Fenster 'Ausprobieren' des Tools oder auf der Integrationswebseite 'Linkvorschau'. Dies liegt daran, dass Knoten mit der Sonderbedingung `welcome` in den von Benutzern gestarteten Dialogabläufen übersprungen werden. Facebook Messenger wartet, bis der Benutzer den Dialog startet. Wenn Sie am Anfang des Dialogs Standardwerte für Kontextvariablen festlegen müssen, sollte dies nicht im Willkommensknoten geschehen. Weitere Informationen enthält der Abschnitt [Dialogmodul starten](/docs/services/assistant?topic=assistant-dialog-start).
{: note}

Der Dialogablauf für die aktuelle Sitzung wird nach einer Inaktivitätszeit von 60 Minuten erneut gestartet (5 Minuten bei Lite- und Standard-Plänen). Mit anderen Worten: Wenn ein Benutzer 60 bzw. 5 Minuten lang nicht mit dem Assistenten interagiert, werden alle während des vorherigen Dialogs gesetzten Kontextvariablen auf Null oder auf die jeweiligen Standardwerte zurückgesetzt.
