---

copyright:
  years: 2015, 2019
lastupdated: "2019-01-31"

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

# {{site.data.keyword.conversationshort}} API overview
{: #api-overview}

You can use the {{site.data.keyword.conversationshort}} REST APIs, and the corresponding SDKs, to develop applications that interact with the service. There are two versions of the {{site.data.keyword.conversationshort}} API, each supporting a different set of functions: v1 and v2. The API you should use depends on the type of methods your application requires:

- **Runtime methods**: Methods that enable a client application to interact with (but not modify) an existing assistant or skill. You can use these methods to develop a user-facing client that can be deployed for production use, an application that brokers communication between an assistant and another service (such as a chat service or back-end system), or a testing application.

  The {{site.data.keyword.conversationshort}} v2 API provides access to methods you can use to interact with an assistant at run time (such as `/message`). This is the preferred API to use for developing new client applications. For details about the v2 API, see the {{site.data.keyword.conversationshort}} [v2 API Reference ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://{DomainName}/apidocs/assistant-v2){: new_window}.

  The {{site.data.keyword.conversationshort}} v1 API includes a `/message` method that sends user input directly to the workspace used by a dialog skill, bypassing the assistant. The v1 runtime API is supported primarily for backward compatibility purposes. If you use the v1 `/message` method, your app cannot take advantage of the orchestration and state-management capabilities of an assistant. For more information, see [Using the v1 runtime API](api-client.html#v1-api).

- **Authoring methods**: Methods that enable an application to create or modify dialog skills, as an alternative to building a skill graphically using the {{site.data.keyword.conversationshort}} tool. An authoring application uses various methods to create and modify skills, intents, entities, dialog nodes, and other artifacts that make up a dialog skill.

  To build an authoring application, use the v1 API. For more information, see the [v1 API Reference ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://{DomainName}/apidocs/assistant){: new_window}.

  **Note:** The v1 authoring methods interact with workspaces rather than skills. A workspace is a container for the dialog and training data (such as intents and entities) within a dialog skill. For most purposes, when using the API you can think of workspaces and dialog skills as interchangeable; for example, if you create a new workspace using the API, it will appear as a new dialog skill in the {{site.data.keyword.conversationshort}} tool.

For a list of the available API methods, see [API methods summary](api-methods.html).