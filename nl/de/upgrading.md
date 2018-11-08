---

copyright:
  years: 2015, 2018
lastupdated: "2018-02-09"

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

# Upgrade durchführen

Erfahren Sie, wie ein Upgrade Ihres Serviceplans durchgeführt wird.
{: shortdesc}

## Upgrade Ihres Plans durchführen
{: #plan-upgrade}

Mit dem Plan des Typs 'Lite' können Sie den Service '{{site.data.keyword.conversationshort}}' nachhaltig ausprobieren. Ihre Möglichkeiten sind jedoch nicht unbegrenzt.

### Begrenzungen nach Artefakten
Informationen zu den Begrenzungen, die sich bei den einzelnen Plänen ergeben, finden Sie nach Artefakten aufgeschlüsselt in den folgenden Abschnitten:

- [Arbeitsbereiche](configure-workspace.html#workspace-limits)
- [Dialogmodulknoten](dialog-build.html#dialog-node-limits)
- [Absichten](intents.html#intent-limits)
- [Entitäten](entities.html#entity-limits)
- [Protokolle](logs_convo.html#log-limits)

### Begrenzungen für API-Aufrufe
Die Anzahl der zulässigen API-Aufrufe pro Instanz richtet sich nach Ihrem Serviceplan. Details können Sie der Beschreibung Ihres Plans entnehmen.

Falls Sie einen Lite-Plan verwenden und den Grenzwert für die API-Aufrufe erreichen, die Protokolle jedoch angeben, dass Sie weniger Aufrufe als erwartet durchgeführt haben, müssen Sie bedenken, dass der Lite-Plan Informationen nur für 7 Tage speichert.

So führen Sie ein Upgrade für Ihren Plan durch:

1.  Wählen Sie im {{site.data.keyword.Bluemix_notm}}-Menü die Optionen **Services** > **Dashboard** aus.
1.  Wählen Sie die Serviceinstanz aus, für die Sie das Upgrade durchführen wollen, um sie zu öffnen.
1.  Klicken Sie im Navigationsbereich auf **Plan**.
   Daraufhin werden Ihr aktueller Plan sowie andere verfügbare Planoptionen angezeigt und Sie können Änderungen vornehmen.

Antworten auf häufig gestellte Frage zu Abonnements enthält der Abschnitt [Abrechnung und Nutzung verwalten ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](/docs/billing-usage/how_charged.html){: new_window}.

Weitere Informationen zu Services, die von IBM Cloud gehostet werden, finden Sie über die folgenden Links:

- [Bedingungen für Cloud-Services](http://www.ibm.com/software/sla/sladb.nsf/sla/saas)
- [Datensicherheit und Datenschutz für Cloud-Services](http://www.ibm.com/software/sla/sladb.nsf/sla/csdsp)

## Upgrade Ihres Arbeitsbereichs durchführen
{: #upgrade-workspace}

Die Features des Service '{{site.data.keyword.conversationshort}}' werden regelmäßig erweitert und aktualisiert. Einige dieser Änderungen werden zwar automatisch auf Ihren Arbeitsbereich angewendet, aber Updates mit weitreichenden Auswirkungen machen eine manuelle Aktualisierung erforderlich.

Ein Upgrade für Ihren Arbeitsbereich ist nur verfügbar, wenn das Upgradesymbol (![Upgradesymbol](images/upgrade.png)) angezeigt wird.

**Hinweis**: Nachdem Sie ein Upgrade für einen Arbeitsbereich durchgeführt haben, können Sie den Arbeitsbereich nicht mehr auf eine Vorgängerversion zurücksetzen.

So führen Sie ein Upgrade für Ihren Arbeitsbereich durch:
1.  [Duplizieren Sie Ihren Arbeitsbereich](configure-workspace.html#exporting-and-copying-workspaces).
2.  Führen Sie das Upgrade für den duplizierten Arbeitsbereich durch.

    Sobald Sie ein Upgrade Ihres Arbeitsbereiches durchführen, wird die neueste Version der API im Tool aktiviert und in der Anzeige 'Ausprobieren' mit der Verwendung der neuen Features begonnen.
3.  Testen Sie den Arbeitsbereich nach dem Upgrade.
4.  Nachdem Sie im duplizierten Arbeitsbereich überprüft haben, wie sich die Änderung auf Ihre Anwendung auswirken wird, wenden Sie das Upgrade auf Ihren primären Arbeitsbereich an.
5.  Aktualisieren Sie Ihre Anwendung, indem Sie den Nachrichten-API-Aufruf so ändern, dass die neueste Version der API verwendet wird.
