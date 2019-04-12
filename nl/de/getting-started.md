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
{:hide-dashboard: .hide-dashboard}
{:download: .download}
{:gif: data-image-type='gif'}

# Lernprogramm 'Einführung'
{: #getting-started}

In diesem kurzen Lernprogramm stellen wir Ihnen das {{site.data.keyword.conversationshort}}-Tool vor und führen Sie durch die Erstellung Ihres ersten Assistenten.
{: shortdesc}

## Vorbemerkungen
{: #getting-started-prerequisites}
{: hide-dashboard}

Als Ausgangspunkt benötigen Sie eine Serviceinstanz.
{: hide-dashboard}

1.  {: hide-dashboard}Rufen Sie die  Seite für [{{site.data.keyword.conversationshort}} ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://cloud.ibm.com/catalog/services/watson-assistant) im {{site.data.keyword.cloud_notm}}-Katalog auf.

    Die Serviceinstanz wird in der Ressourcengruppe **Standard** erstellt, wenn Sie keine andere Gruppe auswählen, und sie kann später *nicht* geändert werden. Diese Gruppe reicht zum Ausprobieren des Service aus. 

    Wenn Sie eine Instanz für komplexere Einsatzzwecke erstellen möchten, finden Sie weitere Informationen unter [Ressourcengruppen ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://{DomainName}/docs/resources/bestpractice_rgs#bp_resourcegroups){: new_window}.
1.  {: hide-dashboard} Registrieren Sie sich für ein kostenloses {{site.data.keyword.cloud_notm}}-Konto oder melden Sie sich an.
1.  {: hide-dashboard} Klicken Sie auf **Erstellen**.

## Schritt 1: Tool öffnen
{: #getting-started-launch-tool}

Nachdem Sie eine {{site.data.keyword.conversationshort}}-Serviceinstanz erstellt haben, wird die Seite **Verwalten** des Service-Dashboards angezeigt.
{: hide-dashboard}

1.  Klicken Sie auf **Tool starten**. Geben Sie nach der Aufforderung zum Anmelden bei dem Tool Ihre {{site.data.keyword.cloud_notm}}-Berechtigungsnachweise an. 

<!-- Remove this text after dedicated instances have the developer console: begin -->

{{site.data.keyword.Bluemix_dedicated_notm}}: Wählen Sie Ihre Serviceinstanz im Dashboard aus, um das Tool zu starten.

<!-- Remove this text after dedicated instances have the Developer Console: end -->

## Schritt 2: Dialogskill erstellen
{: #getting-started-add-skill}

Als ersten Schritt im {{site.data.keyword.conversationshort}}-Tool erstellen Sie einen Skill. 

Ein *Dialogskill* ist ein Container für die Artefakte, die den Ablauf des Dialogs definieren, den Ihr Assistent mit Ihren Kunden führen kann.

1.  Klicken Sie auf der Homepage des {{site.data.keyword.conversationshort}}-Tools auf **Skill erstellen**.

    ![Zeigt die Schaltfläche 'Skill hinzufügen' auf der Homepage](images/gs-new-skill.png)

1.  Klicken Sie auf **Neu erstellen**.

    ![Zeigt die Schaltfläche 'Neu erstellen' auf der Seite 'Skills'](images/gs-click-create-new.png)

1.  Ordnen Sie dem Skill den Namen `Lernprogramm für Dialogskill` zu.
1.  **Optional**: Wenn das Dialogmodul, das Sie erstellen möchten, eine andere Sprache als Englisch verwenden soll, wählen Sie die gewünschte Sprache in der Liste aus.
1.  Klicken Sie auf **Erstellen**.

    ![Skillerstellung abschließen](images/gs-add-skill-done.png)

Die Seite 'Absichten' des Tools wird angezeigt.

## Schritt 3: Absichten aus einem Inhaltskatalog hinzufügen
{: #getting-started-add-catalog}

Fügen Sie von IBM erstellte Trainingsdaten zu Ihrem Skill hinzu, indem Sie Absichten aus einem Inhaltsdialog hinzufügen. Geben Sie Ihrem Assistenten insbesondere Zugriff auf den Inhaltskatalog **General**, damit Ihr Dialogmodul Benutzer begrüßen und Benutzerdialoge beenden kann.

1.  Klicken Sie im {{site.data.keyword.conversationshort}}-Tool auf die Registerkarte **Inhaltskatalog**.
1.  Suchen Sie in der Liste den Eintrag **General** und klicken Sie auf **Zu Skill hinzufügen**.

    ![Zeigt den Inhaltskatalog und die hervorgehobene Schaltfläche 'Zu Skill hinzufügen' für den Katalog 'General'](images/gs-add-general-catalog.png)
1.  Öffnen Sie die Registerkarte **Absichten**, um die Absichten und die zugehörigen Beispieläußerungen zu überprüfen, die zu Ihren Trainingsdaten hinzugefügt wurden. Sie sind daran zu erkennen, dass der Absichtsname mit dem Präfix `#General_` beginnt. Im nächsten Schritt fügen Sie die Absichten `#General_Greetings` und `#General_Ending` zu Ihrem Dialogmodul hinzu.

    ![Zeigt die in der Registerkarte 'Absichten' enthaltenen Absichten nach dem Hinzufügen des Katalogs 'General'](images/gs-general-added.png)

Sie haben erfolgreich mit dem Erstellen Ihrer Trainingsdaten begonnen, indem Sie vordefinierten Inhalt aus {{site.data.keyword.IBM_notm}} hinzugefügt haben.

## Schritt 4: Dialogmodul erstellen
{: #getting-started-build-dialog}

Ein [Dialogmodul](/docs/services/assistant?topic=assistant-dialog-overview) definiert den Ablauf des Dialogs in Form einer logischen Baumstruktur. Darin werden Absichten (Äußerungen der Benutzer) den Antworten (Antworten von dem Bot) zugeordnet. Für jeden Knoten der Baumstruktur gibt es eine Bedingung, die ihn auf Basis der Benutzereingabe auslöst.

Sie erstellen jetzt ein einfaches Dialogmodul, das die Absichten für Begrüßung und Beendigung des Dialogs jeweils durch einen einzelnen Knoten verarbeitet. 

### Startknoten hinzufügen

1.  Klicken Sie im {{site.data.keyword.conversationshort}}-Tool auf die Registerkarte **Dialogmodul**.
1.  Klicken Sie auf **Erstellen**. Die beiden folgenden Knoten werden angezeigt:
    - **Welcome**: Enthält eine Begrüßung, die Ihren Benutzern angezeigt wird, wenn sie Kontakt zu dem Assistenten aufnehmen.
    - **Anything else**: Enthält Ausdrücke, die als Antworten für die Benutzer ausgegeben werden, wenn ihre Eingabe nicht erkannt wird.

    ![Neues Dialogmodul mit zwei integrierten Knoten](images/gs-new-dialog.png)
1.  Klicken Sie auf den Knoten **Welcome**, um ihn in der Editoransicht zu öffnen.
1.  Ersetzen Sie die Standardantwort durch den Text `Willkommen beim Lernprogramm für Watson Assistant!`.

    ![Antwort des Begrüßungsknotens bearbeiten](images/gs-edit-welcome.png)
1.  Klicken Sie auf ![Schließen](images/close.png), um die Bearbeitungsansicht zu schließen.

Sie haben einen Dialogmodulknoten erstellt, der durch die Bedingung `welcome` ausgelöst wird. (`welcome` ist eine Sonderbedingung, die zwar wie eine Absicht funktioniert, jedoch nicht mit dem Zeichen `#` beginnt.) Sie wird ausgelöst, wenn ein neuer Dialog beginnt. Ihr Knoten gibt an, dass das System beim Beginn eines neuen Dialogs mit der Begrüßungsnachricht antworten soll, die Sie im Antwortabschnitt für diesen ersten Knoten hinzufügen.

### Startknoten testen

Sie können Ihr Dialogmodul jederzeit testen, um es zu überprüfen. Diesen Test führen Sie jetzt aus.

- Klicken Sie auf das Symbol ![Ausprobieren](images/ask_watson.png), um die Anzeige 'Ausprobieren' zu öffnen. Daraufhin sollte Ihre Willkommensnachricht angezeigt werden.

### Knoten zur Verarbeitung von Absichten hinzufügen

Als Nächstes fügen Sie zwischen dem Knoten `Welcome` und dem Knoten `Anything else` weitere Knoten zur Verarbeitung von Absichten hinzu. 

1.  Klicken Sie auf das Symbol 'Mehr' ![Mehr Optionen](images/kabob.png) im Knoten **Welcome** und wählen Sie dann die Option **Knoten darunter hinzufügen** aus.
1.  Geben Sie `#General_Greetings` in das Feld **Bedingung eingeben** für diesen Knoten ein. Wählen Sie dann die Option **`#General_Greetings`** aus.
1.  Fügen Sie die Antwort `Good day to you!` hinzu.
1.  Klicken Sie auf ![Schließen](images/close.png), um die Bearbeitungsansicht zu schließen.

   ![Ein allgemeiner Begrüßungsknoten wurde zum Dialogmodul hinzugefügt](images/gs-add-greeting-node.png)

1.  Klicken Sie auf das Symbol 'Mehr' ![Mehr Optionen](images/kabob.png) in diesem Knoten und wählen Sie dann die Option **Knoten darunter hinzufügen** aus. Geben Sie im Peerknoten `#General_Ending` als Bedingung an und geben Sie `OK. See you later.` als Antwort ein.

   ![Zeigt das Hinzufügen eines Endknotens zum Dialogmodul](images/gs-add-ending-node.png)

1.  Klicken Sie auf ![Schließen](images/close.png), um die Bearbeitungsansicht zu schließen.

   ![Zeigt, dass auch ein allgemeiner Endknoten zum Dialogmodul hinzugefügt wurde](images/gs-ending-added.png)

### Erkennung der Absicht testen

Sie haben ein einfaches Dialogmodul erstellt, um Eingaben für die Begrüßung und für die Verabschiedung zu erkennen und zu beantworten. Nun ermitteln Sie, wie gut es funktioniert.

1.  Klicken Sie auf das Symbol ![Ausprobieren](images/ask_watson.png), um die Anzeige 'Ausprobieren' zu öffnen. Dort wird Ihre Willkommensnachricht angezeigt.
1.  Geben Sie unten im Fenster das Wort `Hello` ein und drücken Sie die Eingabetaste. Die Ausgabe macht deutlich, dass die Absicht '#hello' erkannt wurde, und die passende Antwort (`Good day to you.`) wird angezeigt.
1.  Testen Sie die folgenden Eingaben:
    - `bye`
    - `howdy`
    - `see ya`
    - `good morning`
    - `sayonara`

![Testen des Dialogmoduls im Fenster 'Ausprobieren'](images/gs-try-it.gif){: gif}

{{site.data.keyword.watson}} kann Ihre Absichten sogar dann erkennen, wenn die Eingabe nicht exakt mit den von Ihnen angegebenen Beispielen übereinstimmt. Das Dialogmodul ermittelt anhand von Absichten den Zweck der Benutzereingabe unabhängig vom eigentlichen Wortlaut und antwortet dann auf die von Ihnen vorgegebene Weise.

### Ergebnis der Dialogmodulerstellung

Das war's schon. Sie haben einen einfachen Dialog mit zwei Absichten und ein Dialogmodul zu ihrer Erkennung erstellt.

## Schritt 5: Einen Assistenten erstellen
{: #getting-started-create-assistant}

Ein [*Assistent*](/docs/services/assistant?topic=assistant-assistants) ist ein kognitiver Bot, den Sie durch das Hinzufügen eines Skills in die Lage versetzen, mit Ihren Kunden zu interagieren und hilfreiche Antworten zu geben.

1.  Klicken Sie auf die Registerkarte **Assistenten**.
1.  Klicken Sie auf **Neu erstellen**.

    ![Zeigt die Schaltfläche 'Neu erstellen' auf der Registerkarte 'Assistenten'](images/gs-create-assistant.png)
1.  Ordnen Sie dem Assistenten den Namen `Lernprogramm für Watson Assistant` zu.
1.  Geben Sie in das Feld 'Beschreibung' Folgendes ein: `Dies ist ein Beispielassistent, den ich als praktische Übung erstelle.`
1.  Klicken Sie auf **Erstellen**.

    ![Erstellung des neuen Assistenten abschließen](images/gs-create-assistant-done0.png)

## Schritt 6: Erstellten Skill im Assistenten hinzufügen
{: #getting-started-add-skill-to-assistant}

Fügen Sie den von Ihnen erstellten Dialogskill zu dem Assistenten hinzu, den Sie erstellt haben.

1.  Klicken Sie auf der Seite für den neu erstellten Assistenten auf **Skill hinzufügen**.

    Wenn Sie Arbeitsbereiche erstellt haben oder als Inhaber der Entwicklerrolle auf Arbeitsbereiche zugreifen können, die mit der allgemein verfügbaren Version des {{site.data.keyword.conversationshort}}-Service erstellt wurden, werden die betreffenden Skills auf der Seite 'Skills' als Dialogskills aufgelistet.
    {: tip}

    ![Zeigt die Schaltfläche 'Skill hinzufügen' auf der Seite für den Assistenten](images/gs-add-skill.png)
1.  Wählen Sie aus, dass der Skill, den Sie zuvor erstellt haben, zum Assistenten hinzugefügt wird. 

## Schritt 7: Den Assisten integrieren
{: #getting-started-integrate-assistant}

Sie verfügen jetzt über einen Assistenten, der an einem einfachen Dialog mitwirken kann. Veröffentlichen Sie den Assistenten zum Testen auf einer öffentlichen Webseite. Der Service stellt eine Integration bereit, die als Vorschaulink bezeichnet wird. Beim Erstellen dieses Integrationstyps wird Ihr Assistent in ein Chat-Widget eingefügt, das auf einer Webseite mit IBM Schutzmarke gehostet wird. Sie können die Webseite öffnen und mit Ihrem Assistenten chatten, um ihn zu testen.

1.  Klicken Sie auf die Registerkarte **Assistenten**, suchen und öffnen Sie den von Ihnen erstellten Assistenten `Lernprogramm für Watson Assistant`.
1.  Klicken Sie im Bereich *Integrationen* auf **Integration hinzufügen**.
1.  Suchen Sie **Vorschaulink** und klicken Sie auf **Integration hinzufügen**.

1.  Klicken Sie auf die URL, die auf der Seite angezeigt wird.

    Die Seite wird in einer neuen Registerkarte geöffnet.
1.  Geben Sie `hello` ein und beobachten Sie, wie Ihr Assistent reagiert. Sie können die URL mit anderen Benutzern teilen, die Ihren Assistenten testen sollen.

## Nächste Schritte
{: #getting-started-next-steps}

Dieses Lernprogramm hat ein einfaches Beispiel behandelt. Für eine echte Anwendung müssen Sie einige weitere relevante Absichten und einige Entitäten definieren sowie ein komplexeres Dialogmodul, das beide verwendet. Wenn Sie eine optimierte Version des Assistenten fertiggestellt haben, können Sie sie in Kanäle integrieren, die von Ihren Kunden genutzt werden (z. B. Slack). Wenn der Datenverkehr zwischen dem Assistenten und Ihren Kunden zunimmt, können Sie mit den verfügbaren Tools auf der Registerkarte **Analyse** echte Dialoge analysieren und Bereiche für weitere Verbesserungen identifizieren.

- Arbeiten Sie die nachfolgenden Lernprogramme zum Erstellen von komplexeren Dialogmodulen durch: 
    - Fügen Sie mit dem Lernprogramm [Komplexes Dialogmodul erstellen](/docs/services/assistant?topic=assistant-tutorial) Standardknoten hinzu.
    - Erfahren Sie mehr über Slots mit dem Lernprogramm [Knoten mit Slots hinzufügen](/docs/services/assistant?topic=assistant-tutorial-slots).
- Probieren Sie weitere [Beispielapps](/docs/services/assistant?topic=assistant-sample-apps) aus, um mehr Anregungen zu erhalten.
