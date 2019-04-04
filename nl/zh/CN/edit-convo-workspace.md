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

# 访问工作空间
{: #edit-convo-workspace}

如果已使用较低版本的服务（甚至在它还称为 {{site.data.keyword.watson}} Conversation 的时候）创建了工作空间，那么可以继续使用该工作空间。
{: shortdesc}

## 更改会话工作空间
{: #edit-convo-workspace-task}

工作空间仍然可用；只是现在工作空间称为*技能*。要对原有工作空间进行更改，请完成以下步骤：

1.  在 {{site.data.keyword.conversationshort}} 主页中，单击**技能**。

    使用 {{site.data.keyword.conversationshort}} 服务的一般可用版本创建的任何工作空间都会显示为对话技能磁贴。
1.  单击表示要编辑的工作空间的对话技能。
1.  对要更改或添加的训练数据或对话执行任何更改或添加操作。
1.  保存更改。

训练完成后，更新可供要调用服务的定制应用程序使用。有关更多信息，请参阅 [Watson Assistant API 概述](/docs/services/assistant?topic=assistant-api-overview)。

## 限制
{: #edit-convo-workspace-cons}

使用最新版本的服务时，可以继续执行通过原有服务所能执行的所有操作，但具有更高的灵活性。或许您使用较低版本的服务创建了工作空间，并且将从不想替换的现有应用程序来调用该工作空间？它已经在管理状态，并可对要继续控制的各轮对话执行有用的功能。您仍可以在使用最新版本的 /message API 发出的调用之间进行编排。优点是此操作不是必需的。在最新版本中，可以通过同一底层对话技能一次支持多个集成通道。

如果选择跳过创建助手并向其添加对话技能的步骤，那么您将错过助手提供的简化功能。也就是说，您**无法**使用单个会话，一次通过多个集成通道与客户进行交互，并快速扩展或切换到用户常用的新通道。

## 技能和工作空间
{: #edit-convo-workspace-names}

在工具中显示为对话技能的内容实际上是 V1 工作空间的包装器。目前，没有用于使用 V2 API 编写技能的 API 方法，您可以继续使用 V1 API 来编写工作空间。

- 打开工具时，在 11 月 9 日之前创建的任何工作空间都会显示为技能。技能名称采用原工作空间名称。不过，如果随后更改了技能名称或描述，这些更改并不会影响工作空间。同样，如果使用 API 来编辑工作空间名称或描述，这些更改也不会影响技能。
- 在工具中，技能的 API 详细信息将同时显示技能标识和工作空间标识。
