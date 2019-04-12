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

# Produktinfo
{: #index}

{{site.data.keyword.conversationfull}} ist ein kognitiver Bot, den Sie an Ihre Geschäftsandorderungen anpassen und in mehreren Kanälen bereitstellen können, um Ihren Kunden nach Bedarf kontextbezogene Hilfe anzubieten.
{: shortdesc}

## Funktionsweise
{: #index-how-it-works}

Das folgende Diagramm zeigt die Gesamtarchitektur: 

![Ablaufdiagramm für den Service](images/arch-overview.png)

- Benutzer interagieren über mindestens einen der folgenden **Integrationspunkte** mit dem Assistenten:

  - Chat-Bot, den Sie direkt in einer Messaging-Plattform der Social Media wie Slack oder Facebook Messenger veröffentlichen
  - Einfache Chat-Bot-Benutzerschnittstelle, die in IBM Cloud gehostet wird
  - Von Ihnen entwickelte, angepasste Anwendung (z. B. eine mobile App oder ein Roboter mit Sprachschnittstelle)

- Der **Assistent** empfängt Benutzereingaben und leitet sie zum Dialogskill weiter.

- Der **Dialogskill** interpretierte die Benutzereingabe, steuert den Ablauf des Dialogs und erfasst die erforderlichen Informationen zum Antworten oder zum Ausführen einer Transaktion im Auftrag des Benutzers.

## Implementierung
{: #index-mplementation}

So implementieren Sie Ihren Assistenten: 

- **Erstellen Sie einen Dialogskill**. Definieren Sie mithilfe des intuitiven Grafiktools die Trainingsdaten und das Dialogmodul für den Datenaustausch zwischen Ihrem Assistenten und Ihren Kunden. 

  Die Trainingsdaten bestehen aus den folgenden Artefakten:

  - **Absichten**: Dies sind die Ziele, die Sie für die Benutzer antizipieren, die mit dem Service interagieren. Definieren Sie eine Absicht pro Zielsetzung, die in der Eingabe eines Benutzers ermittelt werden kann. Sie können beispielsweise eine Absicht namens *öffnungszeiten* erstellen, die Fragen zu Ladenöffnungszeiten beantwortet. Für jede Absicht fügen Sie Beispieläußerungen hinzu, die von Kunden zum Fragen nach den benötigten Informationen eingegeben werden könnten (z. B. `Wann öffnen Sie?`).

    Alternativ können Sie die vordefinierten **Inhaltskataloge** von IBM als Ausgangspunkt verwenden, die Daten für häufig geäußerte Kundenwünsche bereitstellen. 

  - **Dialogmodul**: Erstellen Sie mit dem Dialogmodultool einen Dialogmodulablauf, der Ihre Absichten enthält. Der Dialogmodulablauf wird im Tool grafisch als Baumstruktur dargestellt. Durch das Hinzufügen von Verzweigungen können Sie alle Absichten verarbeiten, die der Service abwickeln soll.

  - **Entitäten**: Eine Entität stellt einen Term oder ein Objekt dar, der/das Kontext für eine Absicht bereitstellt. Eine Entität könnte beispielsweise ein Ortsname sein, mit dem in Ihrem Dialogmodul ermittelt wird, für welches Geschäft ein Benutzer die Öffnungszeiten wissen möchte. Aktualisieren Sie Ihr Dialogmodul nach dem Hinzufügen von Entitäten so, dass die Entitäten verwendet werden. Fügen Sie Dialogmodulknoten hinzu, um die zahlreichen möglichen Permutationen einer Anfrage basierend auf den in der Benutzereingabe gefundenen Entitäten zu verarbeiten. 

    Wenn Sie Trainingsdaten hinzufügen, wird automatisch ein Klassifikationsmerkmal für natürliche Sprache zum Skill hinzugefügt und trainiert, um die Arten von Anfragen zu verstehen, die der Service überwachen und beantworten soll.

- **Erstellen Sie einen Assistenten**.

- **Fügen Sie den Dialogskill zu Ihrem Assistenten hinzu**.

- **Integrieren Sie Ihren Assistenten**. Erstellen Sie eine Kanalintegration, um den konfigurierten Assistenten direkt in einem Social-Media- oder Nachrichtenkanal bereitzustellen. 

  Ihr bereitgestellter Assistent wird in {{site.data.keyword.cloud_notm}}, der Cloud-Computing-Plattform von IBM gehostet. (Weitere Informationen finden Sie unter [Übersicht über die Plattform![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://cloud.ibm.com/docs/overview/ibm-cloud#overview).)

Weitere Informationen zu diesen Implementierungsschritten finden Sie über die folgenden Links:

- [Absichtserstellung im Überblick](/docs/services/assistant?topic=assistant-intents#intents-described)
- [Dialogmodule im Überblick](/docs/services/assistant?topic=assistant-dialog-overview)
- [Entitätserstellung im Überblick](/docs/services/assistant?topic=assistant-entities#entities-described)
- [Assistenten im Überblick](/docs/services/assistant?topic=assistant-assistant-add)
- [Integrationen hinzufügen](/docs/services/assistant?topic=assistant-deploy-integration-add)

## Wo sind meine Arbeitsbereiche?
{: #index-existing-customers}

Wenn Sie mit einer früheren Version des Service einen *Arbeitsbereich* erstellt haben, können Sie unbesorgt sein: der Arbeitsbereich bleibt für Sie erreichbar. Arbeitsbereiche werden jetzt als *Skills* bezeichnet. Um Ihren Arbeitsbereich aufzurufen, klicken Sie auf die Registerkarte **Skills**.

## Browserunterstützung
{: #index-browser-support}

Für das Tool des {{site.data.keyword.conversationshort}}-Service ist der gleiche Versionsstand der Browsersoftware erforderlich wie für {{site.data.keyword.Bluemix_notm}}. Weitere Details enthält der Abschnitt {{site.data.keyword.Bluemix_notm}} [Voraussetzungen ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://cloud.ibm.com/docs/overview/prereqs#browsers){: new_window}.

## Sprachunterstützung
{: #index-lang-support}

Die Sprachunterstützung für die einzelnen Features ist detailliert im Abschnitt [Unterstützte Sprachen](/docs/services/assistant?topic=assistant-language-support) erläutert.

## Nutzungsbedingungen
{: #index-notices}

Informationen zu den Nutzungsbedingungen für den Service finden Sie unter [IBM Cloud - Bedingungen und Bemerkungen ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](/docs/overview/terms-of-use?topic=overview-terms).

## Nächste Schritte
{: #index-next-steps}

- [Einführung](/docs/services/assistant?topic=assistant-getting-started) in den Service.
- Rufen Sie die Liste der [Entwicklerressourcen ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://www.ibm.com/watson/developer-resources/){: new_window} auf.

Sie haben noch Fragen? Wenden Sie sich an [IBM Sales ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://www-01.ibm.com/marketing/iwm/dre/signup?source=urx-20970){: new_window}.
