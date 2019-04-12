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

# Informationssicherheit
{: #information-security}

IBM ist bestrebt, seinen Kunden und Business Partners innovative Datenschutz-, Sicherheits- und Governance-Lösungen zur Verfügung zu stellen.
{: shortdesc}

**Hinweis:**
Jeder Kunde ist für die Einhaltung der geltenden Gesetze und Verordnungen, einschließlich der Datenschutz-Grundverordnung (DSGVO) der Europäischen Union, selbst verantwortlich. Es obliegt allein dem Kunden, sich von kompetenter juristischer Stelle zu Inhalt und Auslegung aller relevanten Gesetze und gesetzlichen Bestimmungen beraten zu lassen, die seine Geschäftstätigkeit und die von ihm eventuell einzuleitenden Maßnahmen zur Einhaltung dieser Gesetze und Bestimmungen betreffen. 

Die hier beschriebenen Produkte, Services und weiteren Leistungsmerkmale sind nicht für alle Situationen des Kunden geeignet und möglicherweise nur eingeschränkt verfügbar. IBM erteilt keine Rechts- oder Steuerberatung und gibt keine Garantie bezüglich der Konformität von IBM Produkten oder Services mit den für die Kunden geltenden Gesetzen und gesetzlichen Bestimmungen. 

DSGVO-Unterstützung für erstellte {{site.data.keyword.cloud}} {{site.data.keyword.watson}}-Ressourcen anfordern

- Innerhalb der Europäischen Union: [Requesting support for IBM Cloud Watson resources created in the European Union![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://cloud.ibm.com/docs/services/watson/getting-started-gdpr-sar#request-EU){: new_window}
- Außerhalb der Europäischen Union: [Requesting support for resources outside the European Union![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://cloud.ibm.com/docs/services/watson/getting-started-gdpr-sar#request-non-EU){: new_window}

## Datenschutz-Grundverordnung der Europäischen Union (DSGVO)
{: #information-security-gdpr}

IBM ist bestrebt, seinen Kunden und  Business Partners innovative Datenschutz-, Sicherheits- und Governance-Lösungen zur Verfügung zu stellen, die ihnen bei der Einhaltung der DSGVO behilflich sind.

Weitere Informationen zu den IBM Bestrebungen und Angeboten für Ihren Weg zur Einhaltung der DSGVO finden Sie [hier ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](../../icons/launch-glyph.svg "Symbol für externen Link")](http://www.ibm.com/gdpr){: new_window}.

## In {{site.data.keyword.conversationshort}} Daten kennzeichnen und löschen
{: #information-security-gdpr-wa}

Nehmen Sie keine personenbezogenen Daten in die Trainingsdaten (Entitäten und Absichten sowie Benutzerbeispiele) auf, die Sie erstellen. Stellen Sie vielmehr sicher, dass alle personenbezogenen Daten aus Dateien mit Äußerungen realer Benutzer entfernt werden, die zum Suchen nach Empfehlungen für Benutzerbeispiele hochgeladen werden.

**Hinweis:** Experimentelle Funktionen und Betafunktionen sind nicht für die Verwendung in einer Produktionsumgebung bestimmt und funktionieren daher beim Kennzeichnen und Löschen von Daten möglicherweise nicht wie erwartet. Experimentelle Funktionen und Betafunktionen sollten nicht beim Implementieren einer Lösung verwendet werden, die das Kennzeichnen und Löschen von Daten erfordert. 

Wenn die Nachrichtendaten eines Kunden in einer {{site.data.keyword.conversationshort}}-Instanz gelöscht werden müssen, kann dies in Abhängigkeit von der ID des Kunden erfolgen, sofern Sie die Nachricht beim Senden an den Service mit einer Kunden-ID verknüpfen. 

**Hinweis:** Die Funktionen für Vorschaulink-Integration und automatische Facebook-Integration unterstützen nicht das Kennzeichnen und Löschen von Daten in Abhängigkeit von einer Kunden-ID. Diese Funktionen sollten nicht in einer Lösung verwendet werden, die das Löschen in Abhängigkeit von einer Kunden-ID erfordert. 

### Vorbemerkungen
{: #information-security-delete-user-data-prereqs}

Damit Nachrichtendaten gelöscht werden können, die einem bestimmten Benutzer zugeordnet sind, müssen Sie zunächst allen Nachrichten eine eindeutige **Kunden-ID** für jeden Benutzer zuordnen. Um die **Kunden-ID** für beliebige Nachrichten anzugeben, die mithilfe der API `/message` gesendet wurden, fügen Sie die Eigenschaft `X-Watson-Metadata: customer_id` in Ihren Header ein. Beispiel:

```
curl -X POST -u "apikey:3Df... ...Y7Pc9"
 --header
   'Content-Type: application/json'
   'X-Watson-Metadata: customer_id=abc'
 --data
   '{"input":{"text":"hello"}}'
  'https://gateway-eu-de.watsonplatform.net/assistant/api/v1/workspaces/{workspaceID}/message?version=2018-09-20'
```
{: codeblock}

Die Zeichenfolge `customer_id` darf kein Semikolon (`;`) und kein Gleichheitszeichen (`=`) enthalten. Sie müssen sicherstellen, dass jede Eigenschaft `customer ID` für alle Ihre Kunden eindeutig ist.
{: note}

Sie können mehrere Werte für die **Kunden-ID** in durch Semikolon getrennten `customer_id={value}`-Paaren übergeben. Beispiel: `'X-Watson-Metadata: customer_id=abc;customer_id=xyz'`.

Wenn Sie einen Suchskill zu  einem Assistenten hinzufügen, werden an den Assistenten übergebene Benutzereingaben als Suchabfrage an den {{site.data.keyword.discoveryshort}}-Service übergeben. Wenn die {{site.data.keyword.conversationshort}}-Integration eine Kunden-ID bereitstellt, ist im Header der resultierenden Anforderung der API '/message' die Kunden-ID enthalten und wird an die Anforderung der {{site.data.keyword.discoveryshort}}-API '/query' übergeben. Um alle Abfragedaten zu löschen, die einem bestimmten Kunden zugeordnet sind, müssen Sie eine Löschanforderung direkt an die {{site.data.keyword.discoveryshort}}-Serviceinstanz senden, die mit Ihrem Assistenten verknüpft ist. Weitere Informationen enthält der Abschnitt [Informationssicherheit](https://cloud.ibm.com/docs/services/discovery/information-security#gdpr-discovery) für {{site.data.keyword.discoveryshort}}.

### Benutzerdaten abfragen
{: #information-security-query-customer-id}

Verwenden Sie den Parameter `filter` für die Methode der API `/logs` der Version 1, um in einem Anwendungsprotokoll nach bestimmten Benutzerdaten zu suchen. Mit der folgenden Abfrage kann beispielsweise nach zugehörigen Daten für eine Kunden-ID (`customer_id`) gesucht werden, die mit der Angabe `my_best_customer` übereinstimmen: 

``` curl
curl -X GET -u "apikey:3Df... ...Y7Pc9"
'https://gateway-eu-de.watsonplatform.net/assistant/api/v1/workspaces/{workspaceID}/logs?version=2018-09-20&filter=customer_id::my_best_customer'
```
{: codeblock}

Weitere Details enthält der Abschnitt [Referenzinformationen zu Filterabfragen](/docs/services/assistant?topic=assistant-filter-reference).

### Daten löschen
{: #information-security-delete-data}

Verwenden Sie zum Löschen der einem bestimmten Benutzer zugeordneten und im Service gespeicherten Nachrichtenprotokolldaten die API-Methode `DELETE /user_data` der Version 1. Geben Sie die Kunden-ID des Benutzers an, indem Sie mit der Anforderung einen Parameter `customer_id` übergeben. 

Nur Daten mit zugeordneter Kunden-ID, die unter Verwendung des API-Endpunkts `POST /message` hinzugefügt wurden, können mit dieser Methode gelöscht werden. Daten, die mit anderen Methoden hinzugefügt wurden, können nicht anhand der Kunden-ID gelöscht werden. Beispiel: Entitäten und Absichten, die aus Kundendialogen hinzugefügt wurden, können nicht auf diese Weise gelöscht werden. Diese Methoden unterstützen keine personenbezogenen Daten.

**WICHTIG**: Bei Angabe einer Kunden-ID (`customer_id`) werden *alle* Nachrichten mit dieser Kunden-ID (`customer_id`), die vor der Löschanforderung empfangen wurden, in der gesamten {{site.data.keyword.conversationshort}}-Instanz gelöscht, und nicht nur innerhalb eines Skills.

Beispiel: Um die zugeordneten Nachrichtendaten für einen Benutzer mit der Kunden-ID `abc` in Ihrer {{site.data.keyword.conversationshort}}-Instanz zu löschen, senden Sie den folgenden Befehl cURL:

```
curl -X DELETE -u "apikey:3Df... ...Y7Pc9"
 'https://gateway-eu-de.watsonplatform.net/assistant/api/v1/user_data?customer_id=abc&version=2018-09-20'
```
{: codeblock}

Ein leeres JSON-Objekt `{}` wird zurückgegeben.

Weitere Informationen finden Sie in der [API-Referenz](https://cloud.ibm.com/apidocs/assistant?curl=#delete-labeled-data).

**Hinweis:** Löschanforderungen werden im Stapelbetrieb verarbeitet. Dies kann bis zu 24 Stunden dauern.
