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

# Erweiterte Tasks
{: #logs-resources}

Informationen zu APIs und zu anderen Tools zum Abrufen und Analysieren von Protokolldaten.
{: shortdesc}

## API
{: #logs-resources-api}

Mit der API `/logs` können Sie Ereignisse aus den aufgezeichneten Dialogen zwischen Ihren Benutzern und Ihrem Assistenten auflisten. Ausführliche Referenzinformationen zur API finden Sie in [List log events in a workspace ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://cloud.ibm.com/apidocs/assistant#list-log-events-in-a-workspace).

Für welche Anzahl von Tagen Protokolle gespeichert werden, variiert je nach Typ des Serviceplans. Weitere Details enthält der Abschnitt [Begrenzungen für Protokolle](/docs/services/assistant?topic=assistant-logs#logs-limits).

Ein Python-Script zum Exportieren von Protokollen und zum Umwandeln der Protokolle in das CSV-Format enthält die Datei `export_logs.py`, die Sie aus dem [Watson Assistant GitHub ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/watson-developer-cloud/community/blob/master/watson-assistant/export_logs.py)-Repository herunterladen können.

## Terminologie für Protokolle
{: #logs-resources-terminology}

Machen Sie sich zuerst mit zugehörigen Begriffsdefinitionen für {{site.data.keyword.conversationshort}}-Protokolle vertraut: 

- ***Assistent***: Eine Anwendung (auch als Chat-Bot bezeichnet), die Ihre {{site.data.keyword.conversationshort}}-Inhalte bereitstellt.
- ***Dialog***: Eine Nachrichtenaustausch zwischen einem einzelnen Benutzer und Ihrem Assistenten. 
- ***Dialog-ID***: Eine eindeutige Kennung, die einzelnen Nachrichtenaufrufen hinzugefügt wird, um die Nachrichten in einem bestimmten Nachrichtenaustausch miteinander zu verknüpfen. Anwendungsentwickler, die die Version 1 der Service-API verwenden, fügen diesen Wert zu den Nachrichtenaufrufen in einem Dialog hinzu, indem die ID in die Metadaten des Kontextobjekts eingefügt wird.
- ***Kunden-ID***: Eine eindeutige ID, die zum Kennzeichnen von Kundendaten verwendet werden kann, damit die Daten später gelöscht werden können, wenn Kunden das Löschen ihrer personenbezogenen Daten verlangen.
- ***Bereitstellungs-ID***: Ein eindeutiger Kennsatz, den Anwendungsentwickler, die mit Version 1 der Service-API arbeiten, zusammen mit jeder Benutzernachricht übertragen können, um die Bereitstellungsumgebung zu identifizieren, in der die Nachricht erstellt wurde. 
- ***Instanz***: Ihre Bereitstellung von {{site.data.keyword.conversationshort}}, die mit eindeutigen Berechtigungsnachweisen aufgerufen werden kann. Eine {{site.data.keyword.conversationshort}}-Instanz kann mehrere Assistenten enthalten. 
- ***Nachricht***: Eine einzelne Äußerung, die ein Benutzer an den Assistenten sendet. 
- ***Skill-ID***: Die eindeutige Kennung für einen Skill. 
- ***Benutzer***: Eine Person, die mit Ihrem Assistenten interagiert. Dies ist häufig einer Ihrer Kunden.
- ***Benutzer-ID***: Ein eindeutiger Kennsatz, der zum Überwachen des Umfangs der Servicenutzung durch einen bestimmten Benutzer verwendet wird.
- ***Arbeitsbereichs-ID***: Die eindeutige Kennung für einen Arbeitsbereich. Alle Arbeitsbereiche, die Sie vor dem 9. November erstellt haben, werden im Tool als Skills dargestellt. Dies bedeutet jedoch nicht, dass die Begriffe 'Skill' und 'Arbeitsbereich' synonym sind. Ein Skill ist im Grunde eine Oberfläche für einen Arbeitsbereich aus Version 1. 

**Wichtig**: Die Eigenschaft **Benutzer-ID** ist *nicht* gleichbedeutend mit der Eigenschaft **Kunden-ID**, obwohl beide Eigenschaften an den Service übergeben werden können. Das Feld **Benutzer-ID** wird zum Überwachen der Nutzungsstufe für Abrechnungszwecke verwendet, während das Feld **Kunden-ID** zum Kennzeichnen und späteren Löschen von Nachrichten verwendet wird, die Endbenutzern zugeordnet sind. Die Kunden-ID wird durchgängig in allen Watson-Services verwendet und im Header `X-Watson-Metadata` angegeben. Die Benutzer-ID wird ausschließlich vom {{site.data.keyword.conversationshort}}-Service verwendet und im Kontextobjekt für jeden Aufruf der API '/message' übergeben. 

## Benutzermetriken aktivieren
{: #logs-resources-user-id}

Anhand von Benutzermetriken können Sie auf der [Übersichtsseite](/docs/services/assistant?topic=assistant-logs-overview) beispielsweise erkennen, wie viele eindeutige Benutzer mit Ihrem Assistenten in Kontakt getreten sind oder wie viele Dialoge in einem bestimmten Zeitraum durchschnittlich pro Benutzer ausgeführt wurden. Benutzermetriken werden durch die Verwendung eines eindeutigen Parameters `Benutzer-ID` aktiviert. 

Um die `Benutzer-ID` für eine mithilfe der API `/message` gesendete Nachricht anzugeben, fügen Sie die Eigenschaft `user_id` in das Metadatenobjekt in Ihren [Kontext ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://cloud.ibm.com/apidocs/assistant?curl=#get-response-to-user-input){: new_window} ein, wie im folgenden Beispiel gezeigt:

```
"context" : {
  "metadata" : {
       "user_id": "{UserID}"
  }
}
```
{: codeblock}

## Nachrichtendaten zum Löschen mit einem Benutzer verknüpfen
{: #logs-resources-customer_id}

Es kann erforderlich werden, bestimmte Benutzerdaten vollständig aus einer {{site.data.keyword.conversationshort}}-Instanz zu löschen. Wenn die Löschfunktion verwendet wird, werden die gelöschten Nachrichten in der Metrikübersicht möglicherweise nicht mehr berücksichtigt (z. B. wird eine geringerer Wert für 'Dialoge insgesamt' angezeigt). 

### Vorbemerkungen
{: #logs-resources-delete-customer-id-prereqs}

Um Nachrichten für mindestens eine Einzelperson zu löschen, muss eine Nachricht zunächst einer bestimmten **Kunden-ID** zugeordnet werden. Um die **Kunden-ID** für eine Nachricht anzugeben, die mithilfe der API `/message` gesendet wurde, fügen Sie die Eigenschaft `X-Watson-Metadata: customer_id` in Ihren Header ein. Sie können mehrere **Kunden-ID**-Einträge mit durch Semikolon getrennten `field=value`-Paaren unter Verwendung von `customer_id` übergeben, wie im folgenden Beispiel gezeigt:

```
curl -X POST -u "apikey:3Df... ...Y7Pc9"
 --header
   'Content-Type: application/json'
   'X-Watson-Metadata: customer_id={erste_kunden-ID};customer_id={zweite_kunden-ID}'
 --data '{"input":{"text":"hello"}}' 'https:// gateway-us-south.watsonplatform.net/assistant/api/v1/workspaces/{workspaceID}/message?version=2018-09-20'
```
{: codeblock}

Die Zeichenfolge `customer_id` darf kein Semikolon (`;`) und kein Gleichheitszeichen (`=`) enthalten. Sie müssen sicherstellen, dass jeder Parameter `Kunden-ID` für alle Ihre Kunden eindeutig ist.
{: note}

Informationen zum Löschen von Nachrichten unter Verwendung der Eigenschaft `customer_id` enthält der Abschnitt [Informationssicherheit](/docs/services/assistant?topic=assistant-information-security#information-security-gdpr-wa).

## Jupyter-Notizbücher
{: #logs-resources-jupyter-notebooks}

IBM stellt vordefinierte Jupyter-Notizbücher bereit, mit denen Sie Ihre Protokolldaten genauer analysieren können. Ein Jupyter-Notizbuch ist eine webbasierte Umgebung für interaktive Datenverarbeitung. Sie können kleine Codesequenzen ausführen, um Ihre Daten zu verarbeiten. Die Ergebnisse der Verarbeitung werden sofort angezeigt. 

Eine Gruppe der bereitgestellten Notizbücher kann mit Python-Standardtools verwendet werden; eine andere Gruppe ist für die Verwendung mit {{site.data.keyword.DSX_full}} optimiert. Das Produkt {{site.data.keyword.DSX_short}} bietet eine Umgebung, in der Sie die Tools zum Analysieren und Darstellen von Daten, zum Bereinigen und Formen von Daten, zum Einpflegen von Streaming-Daten sowie zum Erstellen, Trainieren und Bereitstellen von Modellen für maschinelles Lernen auswählen können. Weitere Details finden Sie in der [Produktdokumentation ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://dataplatform.cloud.ibm.com/docs/content/getting-started/welcome-main.html){:new_window}.

Die folgenden Notizbücher sind verfügbar: 

- **Measure**: Stellt Messwerte für den Wirkungsgrad (wie oft der Assistent mit Konfidenz auf Benutzeranfragen antwortet) und die Effektiviät (ob die Antworten des Assistenten die Benutzeranforderungen erfüllen) zusammen.

- **Effectiveness**: Führt eine tiefere Analyse Ihrer Protokolle durch, um potenzielle Schritte zur Optimierung Ihres Assistenten aufzuzeigen. 

Informationen zur optimalen Nutzung der Notizbücher finden Sie in der Veröffentlichung [Watson Assistant Continuous Improvement Best Practices Guide ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://www.ibm.com/common/ssi/cgi-bin/ssialias?htmlfid=54022554USEN&).

### Notizbücher mit {{site.data.keyword.DSX}} verwenden
{: #logs-resources-notebooks-studio}

Wenn Sie sich für die Verwendung der angepassten Notizbücher für {{site.data.keyword.DSX}} entscheiden, führen Sie die folgenden grundlegenden Schritte aus: 

1.  Erstellen Sie ein {{site.data.keyword.DSX}}-Konto, [erstellen Sie ein Projekt ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://dataplatform.cloud.ibm.com/docs/content/getting-started/projects.html?context=analytics){:new_window} und fügen Sie ein Cloud Object Storage-Konto zu dem Projekt hinzu.
1.  Rufen Sie in der {{site.data.keyword.DSX}}-Community das [Notizbuch 'Measure Watson Assistant Performance' ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")]( https://dataplatform.cloud.ibm.com/exchange/public/entry/view/133dfc4cd1480bbe4eaa78d3f635e568) ab.
1.  Führen Sie die mit dem Notizbuch bereitgestellten Anweisungen Schritt für Schritt aus, um eine Teilmenge der Dialogaufzeichnungen aus den Protokollen zu analysieren. 

    Die Darstellung der resultierenden Erkenntnisse erleichtert das Verständnis des Wirkungsgrads und der Effektivität des Assistenten.
1.  Exportieren Sie eine Stichprobe der dargestellten Protokolle mit Beispielen für unwirksame Dialoge und analysieren und annotieren Sie die Stichprobe.

    Geben Sie beispielsweise an, ob eine Antwort zutreffend ist. Wenn die Antwort zutrifft, geben Sie an, ob die Antwort hilfreich ist. Wenn eine Antwort falsch ist, geben Sie den Grund dafür an (z. B. dass die falsche Absicht bzw. Entität ermittelt oder der falsche Dialogmodulknoten ausgelöst wurde). Nachdem Sie die Fehlerursache identifiziert haben, geben Sie an, wie die richtige Auswahl lauten sollte.
1.  Importieren Sie das mit Anmerkungen versehene Arbeitsblatt in das [Notizbuch 'Analyze Watson Assistant Effectiveness'](https://dataplatform.cloud.ibm.com/exchange/public/entry/view/133dfc4cd1480bbe4eaa78d3f636921c).

Durch diese Vorgehensweise gewinnen Sie ein besseres Verständnis für potenzielle Schritte zum Verbessern Ihres Assistenten. Es gibt keine Methode, um gewonnene Erkenntnisse automatisch auf Ihre Serviceinstanz anzuwenden. Verfolgen Sie alle Änderungen, die beim Optimieren des Systems vorgenommen werden, damit Sie diese Änderungen später direkt auf die Trainingsdaten für Ihren Dialogskill anwenden können.

### Notizbücher mit Python-Standardtools verwenden
{: #logs-resources-notebooks-python}

Wenn Sie sich für die Verwendung der Notizbücher für Python-Standardtools entscheiden, können Sie diese Notizbücher aus dem [IBM GitHub-Repository](https://github.com/watson-developer-cloud/assistant-improve-recommendations-notebook/tree/master/notebook) abrufen. Stellen Sie sicher, dass die Notizbücher in dieser Reihenfolge ausgeführt werden: 

1.  Measure Notebook.ipynb
1.  Effectiveness Notebook.ipynb
