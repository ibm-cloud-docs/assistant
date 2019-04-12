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

# Auf Arbeitsbereiche zugreifen
{: #edit-convo-workspace}

Wenn Sie einen Arbeitsbereich mit einer früheren Version des Service erstellt haben (auch mit der Vorgängerversion, die als {{site.data.keyword.watson}} Conversation bezeichnet wurde), können Sie den Arbeitsbereich weiter verwenden.
{: shortdesc}

## Conversation-Arbeitsbereich ändern
{: #edit-convo-workspace-task}

Ihr Arbeitsbereich steht weiterhin zur Verfügung; er wird jetzt als *Skill* bezeichnet. So können Sie einen traditionellen Arbeitsbereich ändern: 

1.  Klicken Sie auf der Homepage von {{site.data.keyword.conversationshort}} auf **Skills**.

    Alle Arbeitsbereiche, die Sie mit der allgemein verfügbaren Version des {{site.data.keyword.conversationshort}}-Service erstellt haben, werden als Kacheln für Dialogskills angezeigt.
1.  Klicken Sie auf den Dialogskill. der den Arbeitsbereich repräsentiert, den Sie bearbeiten möchten. 
1.  Ändern oder ergänzen Sie die Traningsdaten oder das Dialogmodul nach Bedarf. 
1.  Speichern Sie die Änderungen.

Sobald das Training abgeschlossen ist, stehen Ihre Aktualisierungen für die angepasste Anwendung zur Verfügung, in der Sie den Service aufrufen. Weitere Informationen enthält der Abschnitt [Watson Assistant-API im Überblick](/docs/services/assistant?topic=assistant-api-overview).

## Einschränkungen
{: #edit-convo-workspace-cons}

Die aktuelle Version des Service bietet den gleichen Funktionsumfang wie die Vorgängerversion, jedoch mit größerer Flexibilität. Angenommen, Sie haben einen Arbeitsbereich mit einer früheren Version des Service erstellt und rufen diesen Arbeitsbereich über eine vorhandene Anwendung auf, die Sie nicht ersetzen möchten. Die Anwendung verwaltet den Status und führt hilfreiche Funktionen für einzelne Abschnitte des Dialogmoduls aus, die Sie weiterhin steuern möchten. Mit der aktuellen Version der API '/message' können Sie die Aufrufe auch weiterhin koordinieren. Der Mehrwert der aktuellen Version besteht darin, dass Sie dies nicht tun müssen. In der aktuellen Version können Sie gleichzeitig mehrere Integrationskanäle mit demselben zugrunde liegenden Dialogskill unterstützen. 

Wenn Sie keinen Assistenten und keinen eigenen Dialogskill hinzufügen, verzichten Sie damit auf die Vereinfachung, die durch Assistenten möglich wird. Das bedeutet vor allem, dass Sie **nicht** durch einen einzelnen Dialog gleichzeitig über mehrere Integrationskanäle mit Kunden interagieren und ohne großen Aufwand neue Kanäle hinzufügen bzw. nutzen können, die bei den Kunden immer beliebter werden.

## Skills und Arbeitsbereiche
{: #edit-convo-workspace-names}

Was in den Tools als Dialogskill dargestellt wird, ist im Grunde eine Oberfläche für einen Arbeitsbereich aus Version 1. Da in der API von Version 2 derzeit keine API-Methoden für das Authoring von Skills verfügbar sind, können Sie weiterhin die API von Version 1 für das Authoring von Arbeitsbereichen verwenden. 

- Beim Öffnen des Tools werden alle Arbeitsbereiche, die Sie vor dem 9. November erstellt haben, als Skills angezeigt. Als Skillname wird der Name des Arbeitsbereichs übernommen. Wenn Sie den Namen oder die Beschreibung des Skills ändern, haben diese Änderungen jedoch keine Auswirkung auf den Arbeitsbereich. Umgekehrt haben Änderungen, die Sie mithilfe der API am Namen oder an der Beschreibung eines Arbeitsbereichs vornehmen, keine Auswirkung auf den Skill. 
- Im Tool wird in den API-Details für den Skill sowohl die Skill-ID als auch die Arbeitsbereichs-ID angezeigt. 
