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

# 关于
{: #index}

{{site.data.keyword.conversationfull}} 是一种认知机器人，您可以根据自己的业务需求进行定制，并跨多个通道进行部署，以随时随地根据客户需要向客户提供帮助。
{: shortdesc}

## 工作方式
{: #index-how-it-works}

下图显示了总体体系结构：

![服务的流程图](images/arch-overview.png)

- 用户通过以下一个或多个**集成**点与助手进行交互：

  - 直接发布到现有社交媒体消息传递平台（例如，Slack 或 Facebook Messenger）的聊天机器人。
  - IBM Cloud 托管的简单聊天机器人用户界面。
  - 您开发的定制应用程序，例如移动应用程序或具有语音界面的机器人。

- **助手**接收用户输入，并将其路由到对话技能。

- 对话**技能**会进一步解释用户输入，然后定向会话流，并收集作出响应或代表用户执行事务所需的任何信息。

## 实现
{: #index-mplementation}

下面说明了如何实现助手：

- **创建对话技能**。使用直观的图形工具为助手与客户之间的会话定义训练数据和对话。

  训练数据包含以下工件：

  - **意向**：预期用户与服务交互时有何目标。为可以在用户输入中识别到的每个目标定义一个意向。例如，可以定义名为 *store_hours* 的意向，用于回答有关门店营业时间的问题。对于每个意向，都可以添加样本发声，以反映客户可能用于询问其所需信息的输入，例如`你们几点开门？`

    或者，使用 IBM 提供的预构建**内容目录**，开始使用用于处理常见客户目标的数据。

  - **对话**：使用对话工具可构建包含意向的对话流。对话流在工具中以图形方式表示为树。可以添加分支来处理希望服务处理的每个意向。

  - **实体**：实体表示用于为意向提供上下文的词汇或对象。例如，实体可能是城市名称，用于帮助对话区分用户希望了解哪个门店的营业时间。添加实体后，请更新对话以使用这些实体。添加多个对话节点，用于根据用户输入中找到的实体来处理请求的许多可能的排列组合。

    在添加训练数据时，会自动向技能添加自然语言分类器，并对该分类器进行训练，使其理解您已指示服务应该听取并响应的请求类型。

- **创建助手**。

- **向助手添加对话技能。**

- **集成助手。**创建通道集成，以将配置的助手直接部署到社交媒体或消息传递通道。

  部署的助手由 IBM 云计算平台 {{site.data.keyword.cloud_notm}} 进行托管。（有关更多信息，请参阅[平台概述 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://cloud.ibm.com/docs/overview/ibm-cloud#overview)。）

通过访问以下链接，阅读有关这些实施步骤的更多信息：

- [意向创建概述](/docs/services/assistant?topic=assistant-intents#intents-described)
- [对话概述](/docs/services/assistant?topic=assistant-dialog-overview)
- [实体创建概述](/docs/services/assistant?topic=assistant-entities#entities-described)
- [助手概述](/docs/services/assistant?topic=assistant-assistant-add)
- [添加集成](/docs/services/assistant?topic=assistant-deploy-integration-add)

## 我的工作空间在哪里?
{: #index-existing-customers}

如果在服务的先前版本中创建了*工作空间*，您不必担心；您仍可以对其进行访问。现在，工作空间称为*技能*。要访问您的工作空间，请单击**技能**选项卡。

## 浏览器支持
{: #index-browser-support}

{{site.data.keyword.conversationshort}} 服务工具需要的浏览器软件级别与 {{site.data.keyword.Bluemix_notm}} 所需的相同。有关详细信息，请参阅 {{site.data.keyword.Bluemix_notm}} [先决条件 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://cloud.ibm.com/docs/overview/prereqs#browsers){: new_window} 主题。

## 语言支持
{: #index-lang-support}

[支持的语言](/docs/services/assistant?topic=assistant-language-support)主题中详细描述了功能部件支持的语言。

## 条款和声明
{: #index-notices}

有关服务条款的信息，请参阅 [IBM Cloud 条款和声明 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](/docs/overview/terms-of-use?topic=overview-terms)。

## 后续步骤
{: #index-next-steps}

- 服务[入门](/docs/services/assistant?topic=assistant-getting-started)。
- 查看[开发者资源 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://www.ibm.com/watson/developer-resources/){: new_window} 列表。

仍然有疑问？请联系 [IBM 销售 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://www-01.ibm.com/marketing/iwm/dre/signup?source=urx-20970){: new_window}。
