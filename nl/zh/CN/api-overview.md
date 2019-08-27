---

copyright:
  years: 2015, 2019
lastupdated: "2019-07-16"

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
{:tip: .tip}

# {{site.data.keyword.conversationshort}} API 概述
{: #api-overview}

您可以使用 {{site.data.keyword.conversationshort}} REST API 和相应的 SDK 来开发与服务进行交互的应用程序。

## 客户机应用程序

要在运行时构建与助手通信的虚拟助手或其他客户机应用程序，请使用新的 V2 API。使用此 API 时，可以开发面向用户的客户机（可部署用于生产用途），开发用于代理助手与其他服务（例如，交谈服务或后端系统）之间通信的应用程序，或者开发测试应用程序。

通过使用 V2 运行时 API 与助手进行通信，应用程序可以利用以下功能：

- **自动状态管理**。V2 运行时 API 会管理与最终用户的每个会话，以存储并维护助手完成完整会话所需的所有上下文数据。

- **使用助手轻松部署**。除了支持定制客户机外，还可以轻松地将助手部署到 Slack 和 Facebook Messenger 等常用消息传递通道。

- **版本控制**。通过对话技能版本控制，可以保存技能的快照，并将助手链接到该特定版本。然后，可以继续更新开发版本，而不会影响生产助手。

- **搜索功能**。V2 运行时 API 可用于接收来自对话技能和搜索技能的响应。如果提交的查询是对话技能无法回答的，那么助手可以使用搜索技能在配置的数据源中查找最佳回答。（搜索技能是仅可供增强版或高端套餐用户使用的 Beta 功能。）

有关 V2 API 的详细信息，请参阅 {{site.data.keyword.conversationshort}} [V2 API ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://{DomainName}/apidocs/assistant-v2){: new_window}。

**注**：{{site.data.keyword.conversationshort}} V1 API 仍支持旧的 `/message` 方法，此方法可将用户输入直接发送到对话技能使用的工作空间。支持 V1 运行时 API 的主要目的是为了实现向后兼容性。如果使用 V1 `/message` 方法，那么必须实现您自己的状态管理，而无法利用助手的版本控制或其他任何功能。

## 编写应用程序

V1 API 提供了多种方法，用于支持应用程序创建或修改对话技能，这些方法可作为使用 {{site.data.keyword.conversationshort}} 用户界面以图形方式构建技能的替代方法。编写应用程序使用该 API 来创建和修改技能、意向、实体、对话节点和其他构成对话技能的工件。有关更多信息，请参阅 [V1 API 参考 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://{DomainName}/apidocs/assistant){: new_window}。

  **注：**V1 编写方法创建和修改的是工作空间，而不是技能。工作空间是用于对话技能中对话和训练数据（如意向和实体）的容器。如果使用该 API 创建新的工作空间，那么该工作空间将在 {{site.data.keyword.conversationshort}} 用户界面中显示为新的对话技能。

有关可用 API 方法的列表，请参阅 [API 方法摘要](/docs/services/assistant?topic=assistant-api-methods)。
