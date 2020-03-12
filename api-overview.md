---

copyright:
  years: 2015, 2020
lastupdated: "2020-03-12"

subcollection: assistant


---

{:curl: .ph data-hd-programlang='curl'}
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:swift: data-hd-programlang='swift'}
{:new_window: target="_blank"}
{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:codeblock: .codeblock}
{:download: .download}
{:deprecated: .deprecated}
{:important: .important}
{:note: .note}
{:tip: .tip}

# {{site.data.keyword.conversationshort}} API overview
{: #api-overview}

You can use the {{site.data.keyword.conversationshort}} REST APIs, and the corresponding SDKs, to develop applications that interact with the service.

## Client applications

To build a virtual assistant or other client application that communicates with an assistant at run time, use the new v2 API. By using this API, you can develop a user-facing client that can be deployed for production use, an application that brokers communication between an assistant and another service (such as a chat service or back-end system), or a testing application.

By using the v2 runtime API to communicate with your assistant, your application can take advantage of the following features:

- **Automatic state management.** The v2 runtime API manages each session with an end user, storing and maintaining all of the context data your assistant needs for a complete conversation.

- **Ease of deployment using assistants.** In addition to supporting custom clients, an assistant can be easily deployed to popular messaging channels such as Slack and Facebook Messenger.

- **Versioning.** With dialog skill versioning, you can save a snapshot of your skill and link your assistant to that specific version. You can then continue to update your development version without affecting the production assistant.

- **Search capabilities.** The v2 runtime API can be used to receive responses from both dialog skills and search skills. When a query is submitted that your dialog skill cannot answer, the assistant can use a search skill to find the best answer from the configured data sources. (Search skills are a beta feature available only to Plus or Premium plan users.)

For details about the v2 API, see the {{site.data.keyword.conversationshort}} [v2 API Reference](https://{DomainName}/apidocs/assistant/assistant-v2){: external}.

**Note**: The {{site.data.keyword.conversationshort}} v1 API still supports the legacy `/message` method that sends user input directly to the workspace used by a dialog skill. The v1 runtime API is supported primarily for backward compatibility purposes. If you use the v1 `/message` method, you must implement your own state management, and you cannot take advantage of versioning or any of the other features of an assistant.

## Authoring applications

The v1 API provides methods that enable an application to create or modify dialog skills, as an alternative to building a skill graphically using the {{site.data.keyword.conversationshort}} user interface. An authoring application uses the API to create and modify skills, intents, entities, dialog nodes, and other artifacts that make up a dialog skill. For more information, see the [v1 API Reference](https://{DomainName}/apidocs/assistant/assistant-v1){: external}.

  **Note:** The v1 authoring methods create and modify workspaces rather than skills. A workspace is a container for the dialog and training data (such as intents and entities) within a dialog skill. If you create a new workspace using the API, it will appear as a new dialog skill in the {{site.data.keyword.conversationshort}} user interface.

For a list of the available API methods, see [API methods summary](/docs/assistant?topic=assistant-api-methods).
