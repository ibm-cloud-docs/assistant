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

# Beta
{: #beta}

![Beta](images/beta.png) IBM gibt Services, Features und Sprachunterstützung für Sie zum Bewerten frei, die als Betaversion klassifiziert sind. Diese Services können unter Umständen instabil sein, häufig geändert werden und nach Ankündigung kurzfristig eingestellt werden. Außerdem bieten Beta-Features möglicherweise nicht dieselben Leistungswerte oder dieselbe Kompatibilität wie allgemein verfügbare Features und sind nicht für die Verwendung in einer Produktionsumgebung bestimmt. Betaversionsfeatures werden nur in [IBM Developer Answers ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://developer.ibm.com/answers/topics/watson-assistant/){: new_window} unterstützt.

## Verfügbare Features der Betaversion
{: #beta-features}

Die folgenden Features sind nur für Benutzer verfügbar, die am Betaprogramm teilnehmen. Informationen zum Anfordern des Zugriffs enthält der Abschnitt [Am Betaprogramm teilnehmen](/docs/services/assistant?topic=assistant-feedback#feedback-beta).

- Die Vorgehensweise beim Arbeiten mit Suchskills wurde geändert. Sie können jetzt einen Suchskill und einen Dialogskill zum selben Assistenten hinzufügen. Wenn Sie beide Skills hinzufügen, wird der Suchvorgang ausgelöst, wenn die Benutzereingabe von keinem der Knoten im Dialogmodul des Dialogskills verarbeitet werden kann. Weitere Informationen enthalten die folgenden Abschnitte: 

  - [Suchskill](/docs/services/assistant?topic=assistant-skill-search-add)
  - [Dialogskill](/docs/services/assistant?topic=assistant-beta-skill-dialog-add)

  Nach der Freigabe ist dieses Feature nur für Benutzer des Plus- oder des Premium-Plans verfügbar. 

- Die Benutzerschnittstelle des Dialogmodulbuilders wurde aktualisiert, sodass die JavaScript-Bibliothek 'React' verwendet werden kann. Dialogfunktionen werden jetzt in eingebundenen Komponenten bereitgestellt, die ihren Status selbst verwalten. Dies führt zu einem besonders responsiven Benutzererlebnis. 

- Sie können einen Skill konfigurieren, um Rechtschreibfehler in der Benutzereingabe zu korrigieren. Weitere Details enthält der Abschnitt [Benutzereingabe korrigieren](/docs/services/assistant?topic=assistant-beta-spell-check).

- Nutzen Sie vorhandene Chataufzeichnungen der Kundenunterstützung, um geeignete Absichten und Benutzerbeispiele zum Trainieren Ihres Assistenten zu finden. Weitere Details enthält der Abschnitt [Hilfe zum Erstellen von Absichten anfordern](/docs/services/assistant?topic=assistant-beta-intent-recommendations).
