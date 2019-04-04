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

# {{site.data.keyword.conversationshort}} API 概述
{: #api-overview}

您可以使用 {{site.data.keyword.conversationshort}} REST API 和相应的 SDK 来开发与服务进行交互的应用程序。{{site.data.keyword.conversationshort}} API 有两个版本，每个版本支持一组不同的函数：V1 和 V2。应该使用的 API 取决于应用程序需要的方法类型：

- **运行时方法**：使客户机应用程序能够与现有助手或技能交互（但不对其进行修改）的方法。可以使用这些方法来开发面向用户的客户机（可部署用于生产用途）、开发用于代理助手与其他服务（例如，交谈服务或后端系统）之间通信的应用程序，或者开发测试应用程序。

  {{site.data.keyword.conversationshort}} V2 API 提供可用于在运行时与助手进行交互的方法（例如，`/message`）。这是用于开发新的客户机应用程序的首选 API。有关 V2 API 的详细信息，请参阅 {{site.data.keyword.conversationshort}} [V2 API ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://{DomainName}/apidocs/assistant-v2){: new_window}。

  {{site.data.keyword.conversationshort}} V1 API 包含的 `/message` 方法用于绕过助手，将用户输入直接发送到对话技能使用的工作空间。支持 V1 运行时 API 的主要目的是为了实现向后兼容性。如果使用 V1 `/message` 方法，那么应用程序无法利用助手的编排和状态管理功能。有关更多信息，请参阅[使用 V1 运行时 API](/docs/services/assistant?topic=assistant-api-client#v1-api)。

- **编写方法**：使应用程序能够创建或修改对话技能的方法，可作为使用 {{site.data.keyword.conversationshort}} 工具以图形方式构建技能的替代方法。编写应用程序使用各种方法来创建和修改技能、意向、实体、对话节点和其他构成对话技能的工件。

  要构建编写应用程序，请使用 V1 API。有关更多信息，请参阅 [V1 API 参考 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://{DomainName}/apidocs/assistant){: new_window}。

  **注：**V1 编写方法与工作空间（而不是技能）进行交互。工作空间是用于对话技能中对话和训练数据（如意向和实体）的容器。对于大多数用途，使用 API 时，可以将工作空间和对话技能视为可互换；例如，如果使用 API 创建新的工作空间，那么该工作空间将在 {{site.data.keyword.conversationshort}} 工具中显示为新的对话技能。

有关可用 API 方法的列表，请参阅 [API 方法摘要](/docs/services/assistant?topic=assistant-api-methods)。
