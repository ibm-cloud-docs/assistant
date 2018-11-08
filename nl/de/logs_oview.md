---

copyright:
  years: 2015, 2018
lastupdated: "2018-01-25"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:swift: .ph data-hd-programlang='swift'}

# Seite 'Übersicht'

Die Seite 'Übersicht' der Anzeige **Verbessern** bietet eine Zusammenfassung der Interaktionen zwischen Benutzern und Ihrem Arbeitsbereich. Sie können den Umfang des Datenverkehrs für einen bestimmten Zeitraum ebenso einsehen wie die Absichten und Entitäten, die in Benutzerdialogen am häufigsten erkannt wurden.
{: shortdesc}

Die auf der Seite 'Übersicht' angezeigte Statistik deckt einen längeren Zeitraum als die Dauer ab, für die Protokolle von Benutzerdialogen aufbewahrt werden. Diese Statistik stellt externen Datenverkehr (Benutzer- oder API-Aufrufe) dar, der mit Ihrem Arbeitsbereich interagiert hat. Interaktionen, die über die Anzeige *Ausprobieren* im Tool erfolgt sind, werden nicht berücksichtigt.

Anhand der Angaben auf der Seite 'Übersicht' können Sie Fragen wie beispielsweise die Folgenden beantworten:

* In welchen Monaten des letzten Jahres gab es am meisten und am wenigsten Dialoge?
* Wie hoch war die durchschnittliche Zahl der Dialoge pro Woche im ersten Quartal?
* Welche Absichten traten in der vergangenen Woche am häufigsten auf?
* Welche Entitätswerte wurden im September am häufigsten erkannt?

Zum Öffnen der Seite 'Übersicht' wählen Sie in der Navigationsleiste die Option **Übersicht** aus. Falls die Option **Übersicht** nicht angezeigt wird, öffnen Sie die Seite über das Menü ![Menü](images/Menu_16.png).

  ![Seite 'Übersicht'](images/oview.png)

Der obere Teil der Seite enthält die folgenden Steuerelemente:

* *Daten aktualisieren*: Hiermit können Sie die Statistik auf der Seite 'Übersicht' sofort aktualisieren. Auf der Seite 'Übersicht' ist angegeben, wann die angezeigten Daten zum letzten Mal aktualisiert worden sind. Sie können die Option **Daten aktualisieren** auswählen, wenn Sie annehmen, dass möglicherweise neuere Daten verfügbar sind.
* Steuerelement für Zeitraum: Mit diesem Steuerelement können Sie den Zeitraum auswählen, für den Daten angezeigt werden.  Dieses Steuerelement wirkt sich auf alle Daten aus, die auf der Seite angezeigt werden, also nicht nur auf die Anzahl der Dialoge, die im Diagramm dargestellt sind, sondern auch auf die mit dem Diagramm angezeigte Statistik und die Listen der häufigsten Absichten und Entitäten.

  ![Steuerelement für Zeitraum](images/oview-time.png)

Sie können auswählen, ob Daten für einen einzelnen Tag, eine Woche, einen Monat, ein Quartal oder ein Jahr angezeigt werden sollen.  In jedem Fall werden die Datenpunkte im Diagramm an einen passenden Messzeitraum angepasst.  Wenn Sie beispielsweise ein Diagramm für einen Tag anzeigen, werden die Daten in stündlichen Werten dargestellt. Zeigen Sie hingegen ein Diagramm für eine Woche an, werden die Daten jeweils nach Tag angezeigt.  Eine Woche reicht von Sonntag bis Samstag.  Zeitperioden können nicht angepasst werden; Sie können also keine Woche definieren, die von Donnerstag bis zum folgenden Mittwoch reicht, oder einen Monat, der an einem anderen Datum als dem Monatsersten beginnt.

## Alle Dialoge

In einem Diagramm wird die Gesamtzahl der Dialoge für den ausgewählten Datumsbereich angezeigt.

**Hinweis**: Als 'Dialog' gilt jede beliebige Interaktion mit dem Arbeitsbereich. Falls Dialoge vom Service mit `Hallo, wie kann ich Ihnen helfen?` eingeleitet wurden und der Benutzer den Browser beendet hat, ohne zu antworten, werden diese Dialoge in die Gesamtzahl der Dialoge einbezogen.

Durch Auswahl der Option **Protokolle anzeigen** können Sie die Seite [Benutzerdialoge](logs_convo.html) öffnen. Der gefilterte Datumsbereich stimmt hierbei mit dem Zeitraum überein, den Sie für die Seite 'Übersicht' ausgewählt haben. Auf der Seite [Benutzerdialoge](logs_convo.html) wird die Gesamtzahl der *Äußerungen* angezeigt. Eine Äußerung ist eine einzelne Nachricht, die ein Benutzer an den Arbeitsbereich sendet. Jeder Dialog kann aus vielen verbalen Äußerungen bestehen. Daher weicht die Zahl der Ergebnisse auf der Seite [Benutzerdialoge](logs_convo.html) von der Anzahl der Dialoge ab, die auf dieser Seite 'Übersicht' angezeigt wird.

**Hinweis**: Je nach Ihrem Plan und dem ausgewählten Datumsbereich kann es sein, dass keine Daten angezeigt werden. Beispielsweise bewahrt der {{site.data.keyword.conversationshort}}-[Standardserviceplan](logs_convo.html#log-limits) Dialoge nur 30 Tage lang auf. Wenn Sie einen Datumsbereich auswählen, der mehr als 30 Tage zurückliegt, werden daher keine Daten angezeigt.

Während das Diagramm angezeigt wird, können Sie auf einzelne Datenpunkte klicken, um den numerischen Wert anzuzeigen. Dies ist in der folgenden Abbildung dargestellt:

![Einzelner Datenpunkt](images/oview-point.png)

Unter dem Diagramm wird die Statistik für die angezeigten Daten angezeigt:

* *Dialoge insgesamt*: Die Gesamtzahl der Dialoge, die während dieses Zeitraums stattfanden.
* *Dialoge maximal*: Die maximale Anzahl von Dialogen für einen einzelnen Datenpunkt innerhalb des Zeitraums.
* *Vermindertes Verständnis*: Die Anzahl der einzelnen Äußerungen, bei denen das Verständnis verringert war. Solche Äußerungen werden nicht durch eine Absicht klassifiziert und enthalten keine bekannten Entitäten. Diese Angabe kann zur Erkennung von potenziellen Problemen bei Dialogen hilfreich sein.

## Häufigste Absichten und Entitäten

Sie können auch die Absichten und Entitäten anzeigen, die während des angegebenen Zeitraums am häufigsten erkannt wurden. Standardmäßig werden jeweils die drei Elemente mit der größten Häufigkeit angezeigt; dies können Sie jedoch in eine größere Anzahl wie 5 oder 10 ändern.

* *Häufigste Absichten*: Absichten werden in einer einfachen Liste angezeigt.  Hier sehen Sie nicht nur, wie oft eine Absicht erkannt wurde, sondern können über den Link **Protokolle anzeigen** auch die Seite 'Benutzerdialoge' öffnen, die dann für den Datumsbereich der angezeigten Daten und die ausgewählte Absicht gefiltert ist.

* *Häufigste Entitäten*: Entitäten werden in Form eines Balkendiagramms dargestellt. Für jede Entität können Sie den Balken auswählen, um die vom Balken dargestellte Anzahl anzuzeigen.

  ![Kurzinfo für Entitätsdaten](images/oview-entity.png)

  Wählen Sie die Option **Höchstwerte anzeigen** aus, um eine Liste der häufigsten Werte anzuzeigen, die während des Zeitraums für diese Entität erkannt wurden. Wählen Sie die Option **Protokolle anzeigen** aus, um die Seite [Benutzerdialoge](logs_convo.html) zu öffnen, die dann für den Datumsbereich der angezeigten Daten und die ausgewählte Entität gefiltert ist.
