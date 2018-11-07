---

copyright:
  years: 2015, 2018
lastupdated: "2018-02-13"

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

# Informationen zur Komponente 'Verbessern'

Die Komponente 'Verbessern' von {{site.data.keyword.conversationshort}} stellt ein Protokoll der Interaktionen mit Benutzern Ihres Arbeitsbereichs zur Verfügung. Mithilfe dieses Protokolls können Sie die Erkennung der Benutzereingaben durch den Arbeitsbereich verbessern.
{: shortdesc}

Die Anzeige **Verbessern** besteht aus den folgenden Abschnitten:

* [Übersicht](logs_oview.html): Hier sehen Sie eine Zusammenfassung der Interaktionen von Benutzern mit einem Arbeitsbereich.
* [Benutzerdialoge](logs_convo.html): Dies ist eine Liste der Äußerungen von Benutzern. Sie können Absichten und Entitäten aktualisieren, während eine einzelne Äußerung eines Benutzers angezeigt wird.**Hinweis**: Ein Benutzerdialog kann mehrere Äußerungen enthalten.
* [Empfehlungen](logs_recommend.html): Hier sind Möglichkeiten für die Verbesserung Ihres Systems aufgeführt. Diese Option ist nur für Benutzer der Kategorie 'Premium' verfügbar.

## Verbesserungen für mehrere Arbeitsbereiche
{: #deploy_id}

Um zu verstehen, wie Daten über Äußerungen für Verbesserungen in mehreren Arbeitsbereichen verwendet werden können, sollten Sie die folgenden Definitionen im Zusammenahng mit dem {{site.data.keyword.conversationshort}}-Service kennen:

* ***Instanz***: Ihre Bereitstellung von {{site.data.keyword.conversationshort}}, die mit eindeutigen Berechtigungsnachweisen aufgerufen werden kann. Eine {{site.data.keyword.conversationshort}}-Instanz kann mehrere Arbeitsbereiche enthalten.
* ***Arbeitsbereich***: Ein Arbeitsbereich ist ein Modell Ihres {{site.data.keyword.conversationshort}}-Inhalts und entspricht häufig einem Bot.
* ***Arbeitsbereichs-ID***: Die eindeutige Kennung für einen Arbeitsbereich.
* ***Bereitstellungs-ID***: Bereitstellungs-IDs sind eindeutige Bezeichnungen, die zusammen mit Äußerungen der Benutzer übermittelt werden und die Bereitstellungsumgebung identifizieren, aus der die Äußerungen stammen.
* ***Äußerung***: Eine Äußerung ist eine einzelne Nachricht, die ein Benutzer an den Arbeitsbereich sendet.

Das Erstellung eines Arbeitsbereichs ist ein iterativer Prozess. Beim Entwickeln Ihres Arbeitsbereichs überprüfen Sie in der Anzeige *Ausprobieren*, dass der Arbeitsbereich die Absichten und Entitäten in den Testeingaben richtig erkennt, und nehmen bei Bedarf Korrekturen vor.

In der Anzeige **Verbessern** können Sie Informationen zu konkreten Interaktionen mit den Benutzern anzeigen und ähnliche Korrekturen vornehmen, um die Erkennungsgenauigkeit für Absichten und Entitäten in Ihrem Arbeitsbereich zu verbessern. Es ist schwer vorauszusehen, *wie* Ihre Benutzer Fragen formulieren oder welche Äußerungen sie vielleicht eingeben werden. Aus diesem Grund sollten Sie die Anzeige **Verbessern** häufig aufrufen, um Ihre Arbeitsbereiche zu verbessern.

Bei einer {{site.data.keyword.conversationshort}}-Instanz, die mehrere Arbeitsbereiche enthält, kann es durchaus hilfreich sein, Äußerungsdaten aus einem Arbeitsbereich zu verwenden, um einen anderen Arbeitsbereich in derselben Instanz zu verbessern.**Hinweis**: Wenn Sie ein Benutzer der Premiumversion von {{site.data.keyword.conversationshort}} sind, können Ihre Premiuminstanzen optional so konfiguriert werden, dass der Zugriff auf die Protokolldaten der jeweils anderen Premiuminstanzen möglich ist.

Angenommen, Sie verfügen über eine {{site.data.keyword.conversationshort}}-Instanz namens *HelpDesk*, die einen Arbeitsbereich 'Production' (Produktion) und einen Arbeitsbereich 'Development' (Entwicklung) enthält. Beim Arbeiten im Arbeitsbereich 'Development' können Sie Äußerungen nach der `Bereitstellungs-ID` für den Arbeitsbereich 'Production' filtern, d. h., Sie können Äußerungen aus dem Arbeitsbereich 'Production' verwenden, um Ihren Arbeitsbereich 'Development' zu verbessern.

![Link zur Datenquelle](images/data_source_1.png)

Alle Bearbeitungsvorgänge, die Sie im Arbeitsbereich 'Development' ausführen, wirken sich allein auf den Arbeitsbereich 'Development' aus, selbst wenn Sie Äußerungen aus dem Arbeitsbereich 'Production' verwenden. Weitere Informationen enthält der Abschnitt [Datenquelle auswählen](logs_convo.html#select-source).

Um die Bereitstellungs-ID für eine Äußerung anzugeben, die mithilfe der API `/message` übermittelt wurde, schließen Sie die Eigenschaft 'deployment' in das Metadatenobjekt in Ihrem [-Kontext ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://www.ibm.com/watson/developercloud/conversation/api/v1/#send_message){: new_window} ein, wie in diesem Beispiel gezeigt:

```
"context" : {
  "username" : "jane_doe@myemail.com",
  "member_type" : "gold",
  "metadata" : {
       "deployment": "HelpDesk-Production"
  }
}
```
{: #codeblock}
