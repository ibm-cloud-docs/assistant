---

copyright:
  years: 2015, 2018
lastupdated: "2018-01-26"

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

# Informationen
{: #index}

Mit dem Service '{{site.data.keyword.conversationfull}}' können Sie eine Lösung erstellen, die Eingabe in natürlicher Sprache versteht und mittels maschinellem Lernen auf Kunden in einer Weise reagiert, die einen Dialog zwischen Personen simuliert.
{: shortdesc}

## Funktionsweise

Das folgende Diagramm zeigt die Gesamtarchitektur einer vollständigen Lösung:![Ablaufdiagramm des Service](images/conversation_arch_overview.png)

- Benutzer interagieren mit Ihrer Anwendung über die von Ihnen implementierte **Benutzerschnittstelle**. Dies kann beispielsweise ein einfaches Chatfenster oder eine mobile App, aber auch ein Roboter mit einer Sprachschnittstelle sein.

- Die **Anwendung** sendet die Benutzereingabe an den Service '{{site.data.keyword.conversationshort}}'.
    - Die Anwendung stellt eine Verbindung zu einem *Arbeitsbereich* her. Dies ist ein Container für den Dialogmodulablauf und die Trainingsdaten.
    - Der Service interpretiert die Benutzereingabe, steuert den Ablauf des Dialogs und erfasst die benötigten Informationen.
    - Sie können Verbindungen zu weiteren {{site.data.keyword.watson}}-Services herstellen, um die Benutzereingabe zu analysieren, beispielsweise zum Service '{{site.data.keyword.toneanalyzershort}}' oder '{{site.data.keyword.speechtotextshort}}'.

- Die Anwendung kann mit Ihren **Back-End-Systemen** auf der Grundlage der Benutzerabsicht und weiterer Informationen interagieren. Beispiele hierfür sind die Beantwortung von Fragen, die Erstellung von Tickets, die Aktualisierung von Kontoinformationen oder die Aufgabe von Bestellungen. Ihre Möglichkeiten sind hier unbegrenzt.

## Implementierung

So implementieren Sie Ihren Dialog:

- **Konfigurieren Sie einen Arbeitsbereich.** Hierzu richten Sie in der komfortablen grafischen Umgebung die Trainingsdaten und das Dialogmodul für Ihren Dialog ein.

    Die Trainingsdaten bestehen aus den folgenden Artefakten:
    - **Absichten**: Dies sind die Ziele, die Sie für die Benutzer antizipieren, die mit dem Service interagieren. Definieren Sie eine Absicht pro Zielsetzung, die in der Eingabe eines Benutzers ermittelt werden kann. Sie können beispielsweise eine Absicht namens *öffnungszeiten* erstellen, die Fragen zu Ladenöffnungszeiten beantwortet. Für jede Absicht fügen Sie Beispieläußerungen hinzu, die von Kunden zum Fragen nach den benötigten Informationen eingegeben werden könnten (z. B. `Wann öffnen Sie?`).
    - **Entitäten**: Eine Entität stellt einen Term oder ein Objekt dar, der/das Kontext für eine Absicht bereitstellt. Eine Entität könnte beispielsweise ein Ortsname sein, mit dem in Ihrem Dialogmodul ermittelt wird, für welches Geschäft ein Benutzer die Öffnungszeiten wissen möchte.

      Wenn Sie Trainingsdaten hinzufügen, wird automatisch ein Klassifikationsmerkmal für natürliche Sprache zum Arbeitsbereich hinzugefügt und trainiert, um die Arten von Anforderungen zu verstehen, die der Service überwachen und beantworten soll.

    Mit dem Dialogmodultool erstellen Sie einen Dialogmodulablauf, der Ihre Absichten und Entitäten enthält. Der Dialogmodulablauf wird im Tool grafisch als Baumstruktur dargestellt. Durch das Hinzufügen von Verzweigungen können Sie alle Absichten verarbeiten, die der Service abwickeln soll. Anschließend können Sie Verzweigungsknoten hinzufügen, die die vielen möglichen Permutationen einer Anforderung auf der Grundlage von weiteren Faktoren verarbeiten, z. B. den in der Benutzereingabe gefundenen Entitäten oder den von einer Anwendung bzw. einem anderen externen Service an Ihren Service übergebenen Informationen.

- **Stellen Sie Ihren Arbeitsbereich bereit.** Zur Bereitstellung Ihres konfigurierten Arbeitsbereichs verbinden Sie ihn mit einer Front-End-Benutzerschnittstelle, einer Social-Media-Plattform oder einem Nachrichtenkanal. Ihre bereitgestellte Instanz des Service '{{site.data.keyword.conversationshort}}' wird von der IBM Cloud-Computing-Plattform {{site.data.keyword.cloud_notm}} gehostet. (Weitere Informationen finden Sie unter [Übersicht über die Plattform![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://console.bluemix.net/docs/overview/ibm-cloud.html#overview).)

Weitere Informationen zu diesen Implementierungsschritten finden Sie über die folgenden Links:

- [Absichten und Entitäten planen](intents-entities.html#planning-your-entities)
- [Dialogmodule im Überblick](dialog-overview.html)
- [Bereitstellung im Überblick](deploy.html)

## Browserunterstützung

Für das Tool des Service '{{site.data.keyword.conversationshort}}'  ist der gleiche Versionsstand der Browsersoftware erforderlich wie für {{site.data.keyword.Bluemix_notm}}. Weitere Details enthält der Abschnitt {{site.data.keyword.Bluemix_notm}} [Voraussetzungen ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://console.bluemix.net/docs/overview/prereqs.html#browsers){: new_window}.

## Sprachunterstützung

Die Sprachunterstützung für die einzelnen Features ist detailliert im Abschnitt [Unterstützte Sprachen](lang-support.html) erläutert.

## Nächste Schritte

- Lesen Sie die [Einführung](getting-started.html) in den Service.
- Probieren Sie einige [Demos](sample-applications.html) aus.
- Rufen Sie die Liste der [SDKs ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://www.ibm.com/watson/developercloud/developer-tools.html){: new_window} auf.

Sie haben noch Fragen? Wenden Sie sich an [IBM Sales ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://www-01.ibm.com/marketing/iwm/dre/signup?source=urx-20970){: new_window}.
