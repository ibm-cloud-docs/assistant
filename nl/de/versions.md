---

copyright:
  years: 2015, 2019
lastupdated: "2019-02-28"

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

# Skillversionen erstellen
{: #versions}

Mithilfe von Versionen können Sie den Workflow eines Entwicklungsprojekts für Dialogskills verwalten.
{: shortdesc}

Erstellen Sie eine Skillversion, um einen Snapshot der Trainingsdaten (Absichten und Entitäten) an entscheidenden Stellen im Entwicklungsprozess aufzuzeichnen. Das Speichern eines in Bearbeitung befindlichen Skills zu einem bestimmten Zeitpunkts ist besonders hilfreich bei der Optimierung Ihres Assistenten. Auf diese Weise können die Auswirkungen der vorgenommenen Änderungen in Echtzeit überprüft werden, um festzustellen, ob die Änderungen sich vor- oder nachteilig auf die Effektivität des Assistenten auswirken. Anhand Ihrer Ergebnisse nach der Bereitstellung in einer Testumgebung können Sie eine fundierte Entscheidung darüber treffen, ob eine bestimmte Änderung auf die Bereitstellung des Assistenten in einer Produktionsumgebung angewendet werden sollte. 

Wenn Sie einen kostenlosen Plan (Lite-Plan) verwenden, können Sie keine Skillversionen erstellen.
{: note}

Dieses Video dauert zweieinhalb Minuten und beschreibt, welchen Nutzen die Verwendung von Versionen bietet.

<iframe class="embed-responsive-item" id="youtubeplayer" title="Skillversionen erstellen" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/FDolnBxvcZ8" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

## Version erstellen
{: #versions-create}

Mit dem {{site.data.keyword.conversationshort}}-Tool können Sie nur jeweils eine Version des Dialogskills auf einmal erstellen. Die in Bearbeitung befindliche Version wird als *Entwicklungsversion* bezeichnet.

Beim Speichern einer Version werden auch alle Skilleinstellungen gespeichert, die Sie auf die Entwicklungsversion angewendet haben.

So erstellen Sie eine Dialogskillversion:

1.  Klicken Sie im Header des Skills auf **Neue Version speichern** und beschreiben Sie anschließend den aktuellen Status des Skills.

    Das Hinzufügen einer aussagefähigen Beschreibung hilft Ihnen, später mehrere Versionen voneinander zu unterscheiden.

1.  Klicken Sie auf **Speichern**.

Ein Snapshot des aktuellen Skills wird erstellt und als neue Version gespeichert. Im Tool wird weiterhin die Entwicklungsversion des Skills angezeigt. Alle Änderungen, die Sie vornehmen, werden auf die Entwicklungsversion angewendet und nicht auf die gespeicherte Version. Wenn Sie eine gespeicherte Version aufrufen möchten, öffnen Sie die Seite **Versionsverlauf**.

## Skillversion bereitstellen
{: #versions-deploy}

1.  Klicken Sie im Header des Skills auf die Registerkarte **Versionsverlauf**.
1.  Klicken Sie auf das Symbol ![Klicken, um Aktionen anzuzeigen](images/kebab-react.png) für die Version, die Sie bereitstellen möchten, und wählen Sie anschließend **Assistenten zuweisen** aus.

    Eine Liste der Assistenten, mit denen Sie diese Version verlinken können, wird angezeigt. Diese Liste enthält nur Assistenten, denen keine Skills zugeordnet sind oder die einer anderen Version dieses Skills zugeordnet sind.
1.  Klicken Sie auf das Kontrollkästchen für mindestens einen Assistenten und anschließend auf **Zuweisen**.

Behalten Sie die Übersicht, wann und wie lange diese Version für einen Assistenten bereitgestellt wird. Sehr wahrscheinlich werden Sie Dialoge zwischen Benutzern und dieser bestimmten Version des Skills analysieren. Die entsprechenden Informationen finden Sie auf der Seite **Analyse**. Wenn Sie jedoch eine Datenquelle auswählen, werden keine Versionen aufgelistet. Sie müssen den Namen des Assistenten auswählen, in dem Sie diese Version bereitgestellt haben. Anschließend können Sie die Metrikdaten so filtern, dass nur die Dialoge angezeigt werden, die zwischen dem Anfang und dem Ende des Zeitraums aufgetreten sind, in dem diese Skillversion im Assistenten implementiert war.
{: important}

## Grenzwerte für Skillversionen
{: #skill-version-limits}

Wie viele Versionen Sie für einen einzelnen Skill erstellen können, richtet sich nach Ihrem {{site.data.keyword.conversationshort}}-Plan.

| Serviceplan     | Versionen pro Skill |
|------------------|-------------------:|
| Premium          |                 50 |
| Plus             |                 10 |
| Standard         |                 10 |
| Lite             |                  0 |
{: caption="Details des Serviceplans" caption-side="top"}

## Zwischen Skillversionen wechseln
{: #versions-swap-skills}

Wenn Sie feststellen, dass eine frühere Version des Skills zu besseren Ergebnissen bei der Erkennung und Umsetzung von Kundenanforderungen geführt hat als eine spätere Version, können Sie stattdessen die frühere Skillversion mit dem Assistenten verlinken. 

Führen Sie die im Abschnitt [Skillversion bereitstellen](#versions-deploy) beschriebenen Schritte aus, um eine andere Version mit dem Assistenten zu verlinken. 

## Gespeicherte Version aufrufen
{: #versions-view}

Die einzige Methode zum Anzeigen einer gespeicherten Version besteht darin, die in Bearbeitung befindliche Entwicklungsversion des Skills durch die gespeicherte Version zu überschreiben. (Vergessen Sie nicht, vorher die Änderungen zu speichern, die Sie an der aktuellen Entwicklungsversion vorgenommen haben.)

Eine gespeicherte Version kann nicht bearbeitet werden. Um dies zu erreichen, können Sie eine gespeicherte Version als Grundlage für eine neue Version verwenden, in der Sie die gewünschten Änderungen vornehmen. Um eine gespeicherte Version als Ausgangspunkt für die weitere Entwicklungsarbeit zu nutzen, überschreiben Sie die in Bearbeitung befindliche Entwicklungsversion des Skills durch die gespeicherte Version. 

1.  Speichern Sie alle Änderungen, die Sie seit dem letzten Erstellen einer Version an dem Skill vorgenommen haben. 

    Speichern Sie jetzt eine Version des Skills. Andernfalls geht Ihre Arbeit bei den nächsten Schritten verloren.
    {: important}

1.  Klicken Sie in der Version, die Sie bearbeiten möchten auf das Symbol **Skillaktionen** ![Skillaktionen](images/kebab-react.png), wählen Sie anschließend **In Entwicklung kopieren** aus und bestätigen Sie diese Aktion.

    Die Seite wird erneut aufgebaut und zeigt den Skillstatus an, der beim Erstellen der Version vorhanden war.

Wenn Sie die Änderungen speichern möchten, die Sie an der aktuellen Version vorgenommen haben, müssen Sie den Skill als neue Version speichern. An einer bereits gespeicherten Version können keine weiteren Änderungen vorgenommen werden. 

Wenn Sie auf die Kachel für einen Skill auf der Seite des Assistenten klicken, wird die Entwicklungsversion des Skills angezeigt. Selbst wenn Sie dem Assistenten eine spätere Version zugeordnet haben, wird beim Aufrufen des Skills stets die Entwicklungsversion des Skills geöffnet.
{: important}
