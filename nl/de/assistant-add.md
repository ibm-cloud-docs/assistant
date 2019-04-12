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

# Assistent erstellen
{: #assistant-add}

Erstellen Sie einen Assistenten mit den erforderlichen Skills, um die Geschäftsziele Ihrer Kunden umzusetzen.
{: shortdesc}

So erstellen Sie einen Assistenten:

1.  Klicken Sie auf die Registerkarte **Assistenten**.

1.  Führen Sie eine der folgenden Aktionen aus:

    - Um für Lernzwecke einen Beispielassistenten zu erstellen, den Sie untersuchen und auswerten können, klicken Sie auf **Beispiel hinzufügen** und wählen Sie den Beispielassistenten aus, der erstellt werden soll.

      Der Beispielassistent wird hinzugefügt. Überspringen Sie die übrigen Schritte der vorliegenden Prozedur.

      Mit dem Beispielassistenten wird ein Beispielskill bereitgestellt und zu Ihrer Liste der Skills hinzugefügt. Wenn Sie bereits einen Beispielskill dieses Typs erstellt haben, wird der vorhandene Skill automatisch dem neuen Beispielassistenten zugeordnet.{: note}

    - Um einen Assistenten völlig neu zu erstellen, klicken Sie auf **Neu erstellen** und führen Sie die weiteren Schritte der vorliegenden Prozedur aus.

1.  Geben Sie die Details für den neuen Assistenten an: 
    - **Name**: Der Name darf nicht länger als 100 Zeichen sein.. Diese Angabe ist erforderlich. 
    - **Beschreibung**: Die Beschreibung ist optional und darf nicht länger als 200 Zeichen sein. 

    Eine öffentliche Webseite mit IBM Schutzmarke wird automatisch für Sie erstellt. Diese Webseite können Sie und Ihr Team verwenden, um Ihren Assistenten zu testen. Wenn keine Webseite als Vorschau erstellt werden soll, wählen Sie das Kontrollkästchen **Vorschaulink aktivieren** ab.

1.  Klicken Sie auf **Erstellen**.

1.  Fügen Sie einen Skill zum Assistenten hinzu, indem Sie auf **Skill hinzufügen** klicken. Sie können auswählen, ob Sie einen vorhandenen Skill hinzufügen oder einen neuen Skill erstellen möchten.

    Wenn Sie auf diesem Weg einen Skill hinzufügen, wird die Entwicklungsversion aufgerufen. Eine bestimmte Skillversion können Sie über die Registerkarte *Versionsverlauf* hinzufügen.

    Wenn Sie Arbeitsbereiche erstellt haben oder als Inhaber der Entwicklerrolle auf Arbeitsbereiche zugreifen können, die mit der allgemein verfügbaren Version des {{site.data.keyword.conversationshort}}-Service (früher als Watson Conversation bezeichnet) erstellt wurden, werden die betreffenden Arbeitsbereiche als vorhandene Dialogskills aufgelistet.{: note}

    Weitere Informationen zum Erstellen eines Skills enthält der Abschnitt [Skill erstellen](/docs/services/assistant?topic=assistant-skill-add).

## Begrenzungen für Assistenten
{: #assistant-add-limits}

Wie viele Assistenten Sie in einer einzelnen Serviceinstanz erstellen können, richtet sich nach Ihrem {{site.data.keyword.conversationshort}}-Plan.

| Serviceplan | Assistenten pro Serviceinstanz | Integrationen pro Assistent | Inaktivät der Chatsitzung |
|--------------|--------------------------------:|----------------------------:|-----------------:|
| Premium      |                             100 |                         100 |       60 Minuten |
| Plus         |                             100 |                         100 |       60 Minuten |
| Standard     |                             100 |                         100 |        5 Minuten |
| Lite*        |                             100 |                         100 |        5 Minuten |
{: caption="Details des Serviceplans" caption-side="top"}

*Nach 30 Tagen Inaktivität kann ein nicht verwendeter Assistent in einer Serviceinstanz unter dem Lite-Plan gelöscht werden, um Speicherplatz freizugeben.

Sie können einen Skill zu Ihrem Assistenten hinzufügen. Wie viele Skills Sie erstellen können, richtet sich nach dem verwenden Serviceplan. Weitere Details enthält der Abschnitt [Begrenzungen für Skills](/docs/services/assistant?topic=assistant-skill-add#skill-add-limits).

## Assistent löschen
{: #assistant-add-delete}

Wenn Sie einen Assistenten löschen, werden automatisch auch alle Integrationen gelöscht, die Sie für den Assistenten definiert haben. Skills, die Sie zu dem Assistenten hinzugefügt haben, werden nicht gelöscht.

So löschen Sie einen Assistenten:

1.  Suchen Sie auf der Registerkarte 'Assistenten' den Assistenten, den Sie löschen möchten.

1.  Klicken Sie auf das Symbol ![Liste der Optionen öffnen bzw. schließen](images/kabob-beta.png) und wählen Sie anschließend **Löschen** aus. Bestätigen Sie den Löschvorgang. 

## Assistent umbenennen
{: #assistant-add-rename}

Sie können den Namen und die Beschreibung eines Assistenten ändern, nachdem Sie den Assistenten erstellt haben.

So benennen Sie einen Assistenten um:

1.  Suchen Sie auf der Registerkarte 'Assistenten' den Assistenten, den Sie umbenennen möchten. 

1.  Klicken Sie auf das Symbol ![Liste der Optionen öffnen bzw. schließen](images/kabob-beta.png) und wählen Sie anschließend **Umbenennen** aus.

1.  Bearbeiten Sie den Namen und klicken Sie anschließend auf **Umbenennen**, damit Ihre Änderungen gespeichert werden.

### Zugeordneten Skill für Assistent ändern
{: #assistant-add-swap-skill}

Sie können einen Skill in einem Assistenten hinzufügen. Wenn Sie den von Ihrem Assistenten verwendeten Skill ändern möchten, können Sie einen Skill gegen einen anderen Skill tauschen.

1.  Klicken Sie in der Registerkarte 'Assistenten' auf die entsprechende Kachel, um den Assistenten zu öffnen, für den Sie den Skill ändern möchten.

1.  Klicken Sie auf das Symbol ![Liste der Optionen öffnen bzw. schließen](images/kabob-beta.png) und wählen Sie anschließend **Skill tauschen** aus. Um den aktuellen Skill gegen eine andere Version des Skills zu tauschen, wählen Sie **Skillversion ändern** aus.

1.  Sie können entweder einen vorhandenen Skill auswählen, der stattdessen verwendet werden soll, oder einen [Skill erstellen](/docs/services/assistant?topic=assistant-skill-add).

### Zwischen Serviceinstanzen umschalten
{: #assistant-add-switch-instance}

Wenn Sie über mehrere Serviceinstanzen verfügen, können Sie in der Seitenkopfzeile prüfen, welche Instanz momentan verwendet wird. Wenn Sie gerade mit einem Skill arbeiten, klicken Sie zuerst auf den Breadcrumb-Link **Skills**. Im Banner wird der Name der aktuellen Instanz angezeigt. Um zu einer anderen Serviceinstanz umzuschalten, klicken Sie auf **Ändern** und wählen Sie anschließend die gewünschte Instanz aus.
