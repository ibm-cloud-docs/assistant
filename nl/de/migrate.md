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

# Migration
{: #migrate}

Durch Migration können Sie eine Instanz des {{site.data.keyword.conversationshort}}-Service aus der aktuellen Cloud Foundry-Organisation und dem zugehörigen Bereich in eine Ressourcengruppe verschieben.
{: shortdesc}

{{site.data.keyword.cloud_notm}} verwendet anstelle von Cloud Foundry jetzt Ressourcengruppen.

Ressourcengruppen bieten die folgenden Vorteile gegenüber Cloud Foundry:

- Differenziertere Zugriffssteuerung durch IBM Cloud Identity and Access Management (IAM)
- Funktionalität zum Verbinden von Serviceinstanzen mit Apps und Services in verschiedenen Regionen
- Leichteres Erfassen von Nutzungsdaten für einzelne Gruppen

Wenn Sie Serviceinstanzen vor November 2018 erstellt haben, verwendet Ihre Instanz je nach Hosting-Standort möglicherweise Cloud Foundry anstelle von Ressourcengruppen. Informationen zum Startdatum der Verwendung von IAM für neue Instanzen an bestimmten Standorten enthält der Abschnitt [Rechenzentren](/docs/services/assistant?topic=assistant-services-information#services-information-regions).

## Serviceinstanz migrieren
{: #migrate-task}

Vorbereitende Informationen zum Migrationsprozess finden Sie in der [IBM Cloud-Migrationsdokumentation![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](/docs/resources?topic=resources-migrate).

Eine Instanz kann nur von der Person oder Gruppe migriert werden, von der sie erstellt wurde oder die über die Zugriffsrolle `Entwickler` für die Instanz verfügt.
{: note}

So migrieren Sie Ihre Serviceinstanz: 

1.  Legen Sie zuerst fest, in welche Ressourcengruppe die Serviceinstanz verschoben werden soll. 

    Tipps für diese Entscheidung enthält der Abschnitt [Bewährte Verfahren zum Organisieren von Ressourcen in Ressourcengruppen ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](/docs/resources?topic=resources-bp_resourcegroups).

1.  Klicken Sie im IBM Cloud-Dashboard in der Serviceliste auf das Migrationssymbol ![Migrieren](images/migrate.svg) für die Instanz, die Sie migrieren möchten, und klicken Sie anschließend im Popup-Fenster auf **Migrieren**.

    Alle Serviceinstanzen, die im Rechenzentrum Sydney vor dem 7. Mai 2018 erstellt wurden oder im Rechenzentrum London vor dem 13. Dezember 2018, wurden in das Rechenzentrum Dallas syndiziert. Beim Migrieren einer Cloud Foundry-Serviceinstanz, die sich im Rechenzentrum Sydney oder London befindet, wird die betreffende Instanz als Ressource an den Hosting-Standort Dallas versetzt. {: note}

1.  Klicken Sie auf **Weiter** und wählen Sie anschließend eine Ressourcengruppe aus.

    Falls Sie noch keine Ressourcengruppe erstellt haben, können Sie dies jetzt nachholen. Eine Ressourcengruppe **default** steht zur Verfügung. Informieren Sie sich über die Verwendung von Gruppen und erstellen Sie eine Gruppe, falls erforderlich. Die Ressourcengruppe, die Sie hier auswählen, kann später *nicht* mehr geändert werden. 

1.  Klicken Sie auf **Migrieren**.

    Nachdem der Vorgang abgeschlossen ist, wird eine Nachricht angezeigt. Anschließend können Sie bei Bedarf weitere Serviceinstanzen migrieren oder auf **Fertig** klicken.

Die alte (in Cloud Foundry gehostete) Serviceinstanz, die Sie migriert haben, wird weiterhin im Abschnitt für Cloud Foundry-Services des Dashboards aufgelistet, jedoch als *Alias* der neuen Version der Instanz, die in einer Ressourcengruppe gehostet wird.

![Zeigt, dass die aktuelle Serviceinstanz jetzt ein Alias der ressourcenbasierten Instanz ist](images/alias.png)

Weitere Informationen zu Aliasnamen finden Sie in der [IBM Cloud-Dokumentation zu Verbindungen![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://cloud.ibm.com/docs/resources/connecting_apps#what_is_alias).

Öffnen Sie die neue, ressourcengruppenbasierte Version der Serviceinstanz, um auf die Schaltfläche **Tool starten** zuzugreifen. Die neue Instanz wird im Abschnitt 'Services' des {{site.data.keyword.Bluemix_notm}}-Dashboards aufgelistet.

## Authentifizierung
{: #migrate-auth-support}

Wenn Sie über Anwendungen verfügen, die mithilfe der Basisauthentifizierung auf den Service zugreifen. können diese Anwendungen nach der Migration weiterhin einen Benutzernamen mit Kennwort übergeben, um sich bei der Serviceinstanz zu authentifizieren. Berechtigungsinformationen werden auf der Seite **Serviceberechtigungsnachweise** für den Aliasnamen angezeigt. 

Die neue Instanz verwaltet die Authentifizierung mit IBM Cloud Identity and Access Management (IAM), einem erweiterten Mechanismus, der API-Schlüssel anstelle von Kombinationen aus Benutzername und Kennwort verwendet. Informationen zu den API-Schlüsseln werden au der Seite **Serviceberechtigungsnachweise** der neuen Serviceinstanz angezeigt. 

Sie sollten in Erwägung ziehen, Ihre vorhandenen angepassten Anwendungen so zu aktualisieren, dass sie die neue Authentifizierungsmethode verwenden, um von der verbesserten Sicherheit dieser Methode zu profitieren. Alle Änderungen, die Sie an den Dialogskills in der neuen Instanz vornehmen, werden auch in die Anwendungen übernommen, die Berechtigungsnachweise für die Basisauthentifizierung verwenden. Nachdem Sie alle Ihre Apps auf die neue Methode mit API-Schlüsseln umgestellt haben, wird der Aliasname nicht mehr benötigt und kann gelöscht werden. 
