---

copyright:
  years: 2015, 2019
lastupdated: "2019-02-21"

subcollection: assistant

---

{:curl: #curl .ph data-hd-programlang='curl'}
{:javascript: #javascript .ph data-hd-programlang='javascript'}
{:java: #java .ph data-hd-programlang='java'}
{:python: #python .ph data-hd-programlang='python'}
{:swift: data-hd-programlang='swift'}
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:codeblock: .codeblock}
{:download: .download}
{:deprecated: .deprecated}
{:important: .important}
{:note: .note}
{:deprecated: .deprecated}
{:important: .important}
{:note: .note}
{:tip: .tip}

# {{site.data.keyword.conversationshort}}-API im Überblick
{: #api-overview}

Mit den {{site.data.keyword.conversationshort}}-REST-APIs und den entsprechenden SDKs können Sie Anwendungen entwickeln, die mit dem Service interagieren. Die beiden Versionen 1 und 2 der {{site.data.keyword.conversationshort}}-API unterstützen jeweils eine andere Gruppe von Funktionen. Welche API Sie verwenden sollten, ist vom Typ der für Ihre Anwendung erforderlichen Methoden abhängig: 

- **Laufzeitmethoden**: Diese Methoden ermöglichen einer Clientanwendung die Interaktion mit (und nicht das Ändern von) einem vorhandenen Assistenten oder Skill. Mit diesen Methoden können Sie einen Client mit Benutzerinteraktion entwickeln, der für den Produktionseinsatz bereitgestellt werden kann, oder eine Anwendung, die als Broker zwischen einem Assistenten und einem anderen Service (z. B. ein Chatservice oder Back-End-System) oder einer Testanwendung vermittelt.

  Die API für {{site.data.keyword.conversationshort}} Version 2 bietet Zugriff auf Methoden für die Interaktion mit einem Assistenten während der Laufzeit (z. B. `/message`). Diese API-Version eignet sich besonders für die Entwicklung neuer Clientanwendungen. Weitere Informationen zur API der Version 2 finden Sie in {{site.data.keyword.conversationshort}} [Version 2 - API-Referenz![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://{DomainName}/apidocs/assistant-v2){: new_window}.

  Die API für {{site.data.keyword.conversationshort}} Version 1 enthält eine Methode `/message`, die Benutzereingaben unter Umgehung des Assistenten direkt an den von einem Dialogskill verwendeten Arbeitsbereich sendet. Die Laufzeit-API für Version 1 wird primär aus Gründen der Abwärtskompatibilität unterstützt. Wenn Sie die Methode `/message` der Version 1 verwenden, können Sie nicht auf die Vorteile der Orchestrierungs- und Statusmanagementfunktionen eines Assistenten zurückgreifen. Weitere Informationen finden Sie unter [Laufzeit-API der Version 1 verwenden](/docs/services/assistant?topic=assistant-api-client#v1-api).

- **Authoring-Methoden**: Diese Methoden ermöglichen einer Anwendung das Erstellen oder Ändern von Dialogskills als Alternative zur grafikgestützten Skillerstellung mit dem {{site.data.keyword.conversationshort}}-Tool. Eine Authoring-Anwendung nutzt verschiedene Methoden zum Erstellen und Ändern von Skills, Absichten, Entitäten, Dialogmodulknoten und anderen Artefakten, aus denen ein Dialogskill besteht.

  Verwenden Sie zum Erstellen einer Authoring-Anwendung die API der Version 1. Weitere Informationen enthält die [Version 1 - API-Referenz ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://{DomainName}/apidocs/assistant){: new_window}.

  **Hinweis:** Die Authoring-Methoden der Version 1 interagieren mit Arbeitsbereichen anstatt mit Skills. Ein Arbeitsbereich ist ein Container für die Dialog- und Trainingsdaten (z. B. Absichten und Entitäten) innerhalb eines Dialogskills. Beim Arbeiten mit der API können die Begriffe 'Arbeitsbereich' und 'Dialogskill' in den meisten Fällen synonym verwendet werden. Wenn Sie beispielsweise mithilfe der API einen neuen Arbeitsbereich erstellen, wird er im {{site.data.keyword.conversationshort}}-Tool als neuer Dialogskill angezeigt.

Eine Liste der verfügbaren API-Methoden finden Sie unter [Zusammenfassung der API-Methoden](/docs/services/assistant?topic=assistant-api-methods).
