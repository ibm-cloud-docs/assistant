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

# Upgrade durchführen
{: #upgrade}

Erfahren Sie, wie ein Upgrade Ihres Serviceplans durchgeführt wird.
{: shortdesc}

## Upgrade Ihres Plans durchführen
{: #upgrade-plan}

Erkunden Sie die {{site.data.keyword.conversationshort}}-[Serviceplanoptionen ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://www.ibm.com/cloud/watson-assistant/pricing/){: new_window}, um zu entscheiden, welcher Plan für Sie am besten ist. 

Für eine Cloud Foundry-basierte Instanz kann kein Upgrade auf einen Plus-Plan durchgeführt werden. Vor dem Upgrade müssen Sie die Instanz migrieren, damit sie eine Ressourcengruppe verwendet. Weitere Details enthält der Abschnitt [Migration](/docs/services/assistant?topic=assistant-migrate).{: note}

So führen Sie ein Upgrade für Ihren Plan durch:

1.  Wählen Sie im {{site.data.keyword.Bluemix_notm}}-Menü die Option **Planupgrade durchführen** aus.
    Daraufhin werden Ihr aktueller Plan sowie andere verfügbare Planoptionen angezeigt und Sie können Änderungen vornehmen.

Antworten auf häufig gestellte Frage zu Abonnements enthält der Abschnitt [Abrechnung und Nutzung verwalten ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](/docs/billing-usage?topic=billing-usage-charges){: new_window}.

Sie haben noch Fragen? Wenden Sie sich an [IBM Sales ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://www-01.ibm.com/marketing/iwm/dre/signup?source=urx-20970){: new_window}.

## Upgrade für Dialogskill durchführen
{: #upgrade-skill}

Die Features des Service '{{site.data.keyword.conversationshort}}' werden regelmäßig erweitert und aktualisiert. Einige dieser Änderungen werden zwar automatisch auf Ihre Dialogskills angewendet, aber für Updates mit weitreichenden Auswirkungen ist eine manuelle Aktualisierung des Modells für maschinelles Lernen erforderlich, das von Ihrem Skill verwendet wird.

Ein Upgrade für Ihren Dialogskill ist nur verfügbar, wenn das Upgradesymbol (![Upgradesymbol](images/upgrade.png)) angezeigt wird. 

So führen Sie ein Upgrade für Ihren Dialogskill durch: 

1.  [Laden Sie eine Kopie Ihres Dialogskills herunter](/docs/services/assistant?topic=assistant-skill-add#skill-add-download-skill) und importieren Sie die Kopie als neuen Skill.
2.  Führen Sie das Upgrade für die Kopie Ihres Dialogskills durch.

    Sobald Sie ein Upgrade Ihres Skills durchführen, wird die neueste Version der API im Tool aktiviert und im Fenster 'Ausprobieren' wird mit der Verwendung der neuen Features begonnen.
3.  Testen Sie den aktualisierten Skill. 
4.  Nachdem Sie in dem aktualisierten Skill überprüft haben, wie sich das Upgrade auf Ihre Anwendung auswirken wird, wenden Sie das Upgrade auf Ihren primären Dialogskill an. 
5.  Führen Sie ein Upgrade für Ihre Anwendung durch. Ändern Sie dazu die von der Anwendung verwendeten Aufrufe der Nachrichten-API so, dass die neueste API-Version angegeben wird. Details zur API-Version finden Sie in den [Releaseinformationen](/docs/services/assistant?topic=assistant-release-notes).
