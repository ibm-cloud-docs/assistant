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

# Skills
{: #skills}

Ein Dialogskill enthält die Trainingsdaten und die Logik, die es einem virtuellen Assistenten ermöglichen, Ihren Kunden zu helfen.
{: shortdesc}

Ein Dialogskill enthält die folgenden Typen von Artefakten: 

- [**Absichten**](/docs/services/assistant?topic=assistant-intents): Eine *Absicht* bildet den Zweck einer Benutzereingabe ab, z. B. die Frage nach Filialstandorten oder einer Rechnungszahlung. Sie definieren eine Absicht für jeden Typ einer Benutzeranforderung, den Ihre Anwendung unterstützen soll. Im Tool wird für den Namen einer Absicht immer das Zeichen `#` als Präfix verwendet. Um den Dialogskill für die Erkennung Ihrer Absichten zu trainieren, stellen Sie viele Beispiele für Benutzereingaben bereit und geben an, welchen Absichten sie zugeordnet sind. 

  Ein bereitgestellter *Inhaltskatalog* enthält vordefinierte, häufig vorkommende Absichten, die Sie zu Ihrer Anwendung hinzufügen können, anstatt eigene Absichten zu erstellen. Beispielsweise ist für die meisten Anwendungen eine Begrüßungsabsicht erforderlich, um einen Dialog mit dem Benutzer einzuleiten. Sie können den Inhaltsdialog **General** hinzufügen, um eine Absicht einzufügen, die den Benutzer begrüßt und weitere hilfreiche Aktionen wie das Beenden des Dialogs enthält. 

- [**Dialogmodul**](/docs/services/assistant?topic=assistant-dialog-build): Ein *Dialogmodul* ist ein Dialogablauf mit Verzweigungen, der definiert, wie Ihre Anwendung antwortet, wenn die definierten Absichten und Entitäten erkannt werden. Mit dem tooleigenen Dialogmoduleditor können Sie Dialoge mit Benutzern erstellen und Antworten geben, die auf den in der Benutzereingabe erkannten Absichten und Entitäten basieren. 

  ![Diagramm einer Basisimplementierung, die nur Absicht und Dialogmodul verwendet](images/basic-impl.png)

Damit Ihr Dialogskill auf differenziertere Fragen antworten kann, definieren Sie Entitäten, auf die in Ihrem Dialogmodul verwiesen werden soll. 

- [**Entitäten**](/docs/services/assistant?topic=assistant-entities): Eine *Entität* stellt einen Term oder ein Objekt dar, der/das für die Absichten relevant ist und einen speziellen Kontext für die Absicht bereitstellt. Eine Entität kann beispielsweise eine Stadt sein, in der ein Benutzer einen Filialstandort sucht, oder der Betrag einer Rechnungszahlung. Im Tool wird für den Namen einer Entität immer das Zeichen `@` als Präfix verwendet.

  Sie können den Skill für die Erkennung Ihrer Entitäten trainieren, indem Sie Werte für Entitätsbegriffe und -synonyme sowie Entitätsmuster angeben oder den Kontext, in dem eine Entität typischerweise in einem Satz erwähnt wird. Fügen Sie für die Feinabstimmung Ihres Dialogmoduls weitere Knoten hinzu, die in der Benutzereingabe neben den Absichten zusätzlich nach Entitätserwähnungen suchen. 

![Diagramm einer komplexeren Implementierung mit Absicht, Entität und Dialogmodul](images/complex-impl.png)

Wenn Sie Informationen hinzufügen, werden diese eindeutigen Daten von dem Skill verwendet, um ein Modell für maschinelles Lernen zu erstellen, das diese und ähnliche Benutzereingaben erkennen kann. Bei jedem Hinzufügen oder Ändern der Trainingsdaten wird der Trainingsprozess ausgelöst, zum sicherzustellen, dass das zugrunde liegende Modell stets mit den wechselnden Anforderungen und Diskussionsthemen der Kunden Schritt hält.

Hilfe zum Erstellen eines Dialogskills finden Sie unter [Skill erstellen](/docs/services/assistant?topic=assistant-skill-add).
