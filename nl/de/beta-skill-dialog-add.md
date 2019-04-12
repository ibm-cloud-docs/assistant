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

# Dialogskill erstellen
{: #beta-skill-dialog-add}

Die Verarbeitung natürlicher Sprache für den {{site.data.keyword.conversationshort}}-Service wird in einem *Dialogskill* definiert. Der Dialogskill ist ein Container für alle Artefakte, die einen Dialogablauf definieren.
{: shortdesc}

Dieses Feature ist nur für Benutzer verfügbar, die am Betaprogramm teilnehmen. Informationen zum Anfordern des Zugriffs enthält der Abschnitt [Am Betaprogramm teilnehmen](/docs/services/assistant?topic=assistant-feedback#feedback-beta).

![Beta](images/beta.png) IBM gibt Services, Features und Sprachunterstützung für Sie zum Bewerten frei, die als Betaversion klassifiziert sind. Diese Services können unter Umständen instabil sein, häufig geändert werden und nach Ankündigung kurzfristig eingestellt werden. Außerdem bieten Beta-Features möglicherweise nicht dieselben Leistungswerte oder dieselbe Kompatibilität wie allgemein verfügbare Features und sind nicht für die Verwendung in einer Produktionsumgebung bestimmt. 

Sie können einen Dialogskill zu einem Assistenten hinzufügen. Informationen zu Begrenzungen in bestimmten Plänen enthält der Abschnitt [Begrenzungen für Skills](/docs/services/assistant?topic=assistant-skill-add#skill-add-limits).

## Dialogskill erstellen
{: #beta-skill-dialog-add-task}

Sie können einen Skill völlig neu erstellen, einen von IBM bereitgestellten Beispielskill verwenden oder einen Skill aus einer JSON-Datei importieren.

Falls noch nicht geschehen, führen Sie die vorausgesetzten Schritte aus dem [Lernprogramm 'Einführung'](/docs/services/assistant?topic=assistant-getting-started#getting-started-prerequisites) aus, um eine {{site.data.keyword.conversationshort}}-Serviceinstanz zu erstellen und starten Sie das Tool.

Mit dem {{site.data.keyword.conversationshort}}-Tool können Sie Skills erstellen. So erstellen Sie einen Dialogskill: 

1.  Klicken Sie auf die Registerkarte **Skills**.

    Wenn Sie Arbeitsbereiche erstellt haben oder als Inhaber der Entwicklerrolle auf Arbeitsbereiche zugreifen können, die mit der allgemein verfügbaren Version des {{site.data.keyword.conversationshort}}-Service (früher als Watson Conversation bezeichnet) erstellt wurden, werden die betreffenden Arbeitsbereiche hier als Dialogskills aufgelistet.{: note}

1.  Klicken Sie auf **Neu erstellen**.

1.  Klicken Sie auf **Hinzufügen**, um einen *Dialogskill* zu erstellen.

1.  Führen Sie eine der folgenden Aktionen aus:

    - Klicken Sie auf **Skill erstellen**, um einen Skill völlig neu zu erstellen.
    - Um einen mit dem Service bereitgestellten Beispielskill als Ausgangspunkt für die Erstellung eines eigenen Skills zu verwenden oder als Lernbeispiel vor dem Erstellen eines eigenen Skills, klicken Sie **Beispielskill verwenden** und anschließend auf das Beispiel, das Sie verwenden möchten.

      Der Beispielskill wird zu Ihrer Liste der Skills hinzugefügt. Der Beispielskill ist keinem Assistenten zugeordnet. Überspringen Sie die übrigen Schritte der vorliegenden Prozedur. 

    - Um einen vorhandenen Skill zu dieser Serviceinstanz hinzuzufügen, können Sie den Skill als JSON-Datei importieren. Klicken Sie auf **Skill importieren** und anschließend auf **JSON-Datei auswählen** und wählen Sie die JSON-Datei aus, die Sie importieren möchten. 

      **Wichtig:**

      - Die importierte JSON-Datei muss die UTF-8-Codierung verwenden.
      - Die maximale Größe einer JSON-Datei für einen Skill beträgt 10 MB. Falls Sie einen größeren Skill importieren müssen, kann es sinnvoll sein, die Absichten und Entitäten separat zu importieren, nachdem Sie den Skill importiert haben. (Für den Import von größeren Skills können Sie auch die REST-API nutzen. Weitere Informationen finden Sie in der [API-Referenz ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://www.ibm.com/watson/developercloud/conversation/api/v1/#create_workspace){: new_window}.)
      - Die JSON-Datei darf keine Tabstopps, Zeilenumbrüche oder Wagenrückläufe enthalten.

      Geben Sie die Daten an, die Sie einbeziehen möchten:

        - Wählen Sie **Alles (Absichten, Entitäten und Dialogmodul)** aus, um eine vollständige Kopie des exportierten Skills (einschließlich des Dialogmoduls) zu importieren.
        - Wählen Sie **Absichten und Entitäten** aus, wenn Sie die Absichten und Entitäten aus dem exportierten Skill verwenden und ein neues Dialogmodul erstellen möchten. 

      Klicken Sie auf **Importieren**.

      Falls beim Importieren eines Skills Fehler auftreten, finden Sie weitere Informationen im Abschnitt [Fehlerbehebung beim Importieren von Skills](#beta-skill-dialog-add-import-errors).

1.  Geben Sie die Details für den Skill an: 

    - **Name**: Der Name darf nicht länger als 100 Zeichen sein.. Diese Angabe ist erforderlich. 
    - **Beschreibung**: Die Beschreibung ist optional und darf nicht länger als 200 Zeichen sein. 
    - **Sprache**: Die Sprache der Benutzereingabe, für deren Erkennung der Skill trainiert wird. Der Standardwert ist 'English'.

Der erstellte Skill wird als Kachel auf der Seite 'Skills' angezeigt. Sie können nun angeben, welche Benutzerziele der Dialogskill adressieren soll.

- Informationen zum Hinzufügen vordefinierter Absichten zu Ihrem Skill enthält der Abschnitt [Inhaltskataloge verwenden](/docs/services/assistant?topic=assistant-catalog).
- Informationen zum Definieren eigener Absichten enthält der Abschnitt [Absichten](/docs/services/assistant?topic=assistant-intents).

Der Dialogskill kann erst mit Kunden interagieren, nachdem er zu einem Assistenten hinzugefügt und der Assistent bereitgestellt wurde. Weitere Informationen enthält der Abschnitt [Assistenten erstellen](/docs/services/assistant?topic=assistant-assistant-add).

### Fehlerbehebung beim Importieren von Skills
{: #beta-skill-dialog-add-import-errors}

Wenn eine Nachricht darauf hinweist, dass die Artefakte in dem Skill die Grenzwerte für Ihren Serviceplan überschreiten, führen Sie die folgenden Schritte aus, um den Skill erfolgreich zu importieren: 

1.  Erwerben Sie einen Plan mit einem höheren Grenzwert für Artefakte.
1.  Erstellen Sie eine Serviceinstanz in dem neu erworbenen Plan.
1.  Importieren Sie den Skill in die neue Serviceinstanz.
1.  Bearbeiten Sie den Skill so, dass die Artefaktgrenzwerte für den Plan eingehalten werden, den Sie verwenden möchten. Möglicherweise müssen Sie die Anzahl der Dialogmodulknoten verringern, die in der Baumstruktur des Dialogmoduls verwendet werden. 
1.  Exportieren Sie den bearbeiteten Skill, indem Sie ihn herunterladen.
1.  Versuchen Sie erneut, den bearbeiteten Skill in die ursprüngliche Serviceinstanz mit dem gewünschten Serviceplan zu importieren.

### Skill zu einem Assistenten hinzufügen
{: #beta-skill-dialog-add-to-assistant}

Sie können einen Skill in einem Assistenten hinzufügen. Dazu müssen Sie die Kachel des Assistenten öffnen und den Skill auf der Konfigurationsseite des Assistenten zum Assistenten hinzufügen. Auf der Konfigurationsseite für Skills können Sie nicht auswählen, von welchem Assistenten der Skill verwendet werden soll. Ein Dialogskill kann von mehreren Assistenten verwendet werden.

1.  Klicken Sie in der Registerkarte 'Assistenten' auf die entsprechende Kachel, um den Assistenten zu öffnen, dem Sie den Skill hinzufügen möchten.

1.  Klicken Sie auf **Dialogskill hinzufügen**.

1.  Klicken Sie auf **Vorhandenen Skill hinzufügen**.

    Klicken Sie in den angezeigten verfügbaren Skills auf den Skill, den Sie hinzufügen möchten. 

    Wenn Sie einen vorhandenen Dialogskill hinzufügen und Arbeitsbereiche erstellt haben oder als Inhaber der Entwicklerrolle auf Arbeitsbereiche zugreifen können, die mit der allgemein verfügbaren Version des {{site.data.keyword.conversationshort}}-Service (früher als Watson Conversation bezeichnet) erstellt wurden, werden die betreffenden Arbeitsbereiche hier als Dialogskills aufgelistet. {: note}

## Dialogskill herunterladen
{: #beta-skill-dialog-add-download}

Sie können einen Dialogskill im JSON-Format herunterladen. Möglicherweise möchten Sie einen Skill herunterladen, um ihn unverändert in einer anderen Instanz des {{site.data.keyword.conversationshort}}-Service zu verwenden. Sie können den betreffenden Dialogskill aus einer Instanz herunterladen und als neuen Dialogskill in eine andere Instanz importieren. 

So laden Sie einen Dialogskill herunter: 

1.  Lokalisieren Sie die Kachel für den Dialogskill auf der Seite 'Skills' oder auf der Konfigurationsseite eines Assistenten, der den Skill verwendet.

1.  Klicken Sie auf das Symbol ![Liste der Optionen öffnen bzw. schließen](images/kabob-beta.png) und wählen Sie anschließend **JSON-Datei herunterladen** aus.

1.  Geben Sie einen Namen und den Speicherort für die JSON-Datei an und klicken Sie anschließend auf **Speichern**.

Sie können einen Skill auch mithilfe der API exportieren. Geben Sie den Parameter `export=true` in der Anforderung an. Weitere Details finden Sie in der [API-Referenz](https://cloud.ibm.com/apidocs/assistant#get-information-about-a-workspace).

## Dialogskill mit Teammitgliedern gemeinsam nutzen
{: #beta-skill-dialog-add-invite-others}

Nachdem Sie die Serviceinstanz erstellt haben, können Sie anderen Personen Zugriff auf die Instanz gewähren. Danach können Sie in Teamarbeit die Trainingsdaten definieren und das Dialogmodul erstellen.

Eine Absicht, eine Entität oder ein Dialogmodulknoten kann nicht von mehreren Personen gleichzeitig bearbeitet werden. Andernfalls werden nur die Änderungen der Person angewendet, die ihre Änderungen als zuletzt speichert. Änderungen, die im selben Zeitraum von einer anderen Person vorgenommen und vorher gespeichert wurden, gehen verloren. Koordinieren Sie die geplanten Bearbeitungsvorgänge der einzelnen Teammitglieder so, dass keine Arbeitsergebnisse verloren gehen.
{: important}

Um einen Dialogskill mit anderen Personen gemeinsam zu nutzen, müssen Sie den betreffenden Personen Zugriff auf die Serviceinstanz gewähren, in der der Skill gehostet wird. Dabei ist zu beachten, dass die Person, die Sie einladen, auf jeden Skill zugreifen kann, der in dieser Serviceinstanz gehostet wird. 

1.  Notieren Sie den Namen der aktuellen Instanz, der im oberen Bereich der aktuellen Seite angezeigt wird. 
1.  Klicken Sie auf das Symbol ![Benutzer](images/user-icon2.png) in der Kopfzeile der Seite und wählen Sie in der Dropdown-Liste den Eintrag **Benutzer verwalten** aus.

1.  Klicken Sie auf **Benutzer einladen** und geben Sie die E-Mai-Adressen der Personen aus Ihrem Team ein, denen Sie Zugang gewähren möchten.

    Wenn Sie einer Person den Zugriff auf eine Serviceinstanz in Cloud Foundry erteilt haben, wird die betreffende Person möglicherweise bereits als eingeladener Benutzer aufgelistet. Klicken Sie auf den Namen der Person, um die Zugriffsmanagementeinstellungen für den Benutzer anzuzeigen. Klicken Sie auf **Zugriff zuweisen** und wählen Sie anschließend **Zugriff auf Ressourcen zuweisen** aus.
1.  Treffen Sie im Abschnitt *Services* mindestens die folgenden Auswahlen: 

    - **Services**: {{site.data.keyword.conversationshort}}
    - **Zugriffsrollen für Plattform zuweisen**: Operator

    Weitere Informationen zu Plattformmangagementrollen enthält der Abschnitt [IAM-Zugriff ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](/docs/iam?topic=iam-userroles). (Servicezugriffsrollen werden von {{site.data.keyword.conversationshort}} standardmäßig nicht genutzt.)

    Für ältere Instanzen, die von Cloud Foundry verwaltet werden, müssen Sie den Abschnitt *Cloud Foundry-Zugriff* erweitern, Ihre Organisation auswählen und anschließend der Person die Bereichsrolle **Entwickler** zuweisen.
    {: note}

1.  Klicken Sie auf **Benutzer einladen**.

    Wenn Sie den Zugriff für einen vorhandenen Benutzer bearbeiten, klicken Sie auf **Zugriff zuweisen**.

Bei der nächsten Anmeldung der eingeladenen Personen bei {{site.data.keyword.cloud_notm}} wird Ihr Konto in der Kontoliste dieser Personen angezeigt. Wenn diese Personen Ihr Konto auswählen, wird Ihre Serviceinstanz angezeigt und die betreffenden Personen können Ihre Skills öffnen und bearbeiten. 

Wenn mehrere Personen an der Entwicklung eines Dialogskills beteiligt sind, können unbeabsichtigte Änderungen (einschließlich Löschen von Skills) auftreten. Ziehen Sie in Betracht, regelmäßig Sicherungskopien Ihres Dialogskills zu erstellen, damit gegebenenfalls eine frühere Version wiederhergestellt werden kann. Um eine Sicherungskopie zu erstellen, können Sie einfach den [Skill als JSON-Datei herunterladen](#beta-skill-dialog-add-download).
