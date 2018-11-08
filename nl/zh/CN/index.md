---

copyright:
  years: 2015, 2018
lastupdated: "2018-01-26"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:swift: .ph data-hd-programlang='swift'}

# 关于

通过 {{site.data.keyword.conversationfull}} 服务，可以构建一个解决方案，用于理解自然语言输入，并使用机器学习如何以模拟人类会话的方式对客户进行响应。
{: shortdesc}

## 工作方式

下图显示了完整解决方案的总体体系结构：![服务流程图](images/conversation_arch_overview.png)

- 用户通过您实现的用户**界面**与应用程序进行交互。例如，简单的交谈窗口或移动应用程序，甚至是具有语音接口的机器人。

- **应用程序**将用户输入发送到 {{site.data.keyword.conversationshort}} 服务。
    - 应用程序连接到*工作空间*，工作空间是用于对话流和培训数据的容器。
    - 服务解释用户输入，引导会话流并收集其所需的信息。
    - 可以连接其他 {{site.data.keyword.watson}} 服务来分析用户输入，例如 {{site.data.keyword.toneanalyzershort}} 或 {{site.data.keyword.speechtotextshort}}。

- 应用程序可以根据用户的意向和其他信息与**后端系统**进行交互。例如，回答问题、开具凭单、更新帐户信息或下单。您可以执行的操作没有限制。

## 实现

下面是实现会话的方式：

- **配置工作空间**。通过易于使用的图形环境，为您的会话设置培训数据和对话。

    培训数据包含以下工件：
    - **意向**：预期用户与服务交互时有何目标。为可以在用户输入中识别到的每个目标定义一个意向。例如，可以定义名为 *store_hours* 的意向，用于回答有关门店营业时间的问题。对于每个意向，都可以添加样本发声，以反映客户可能用于询问其所需信息的输入，例如`你们几点开门？`
    - **实体**：实体表示用于为意向提供上下文的词汇或对象。例如，实体可能是城市名称，用于帮助对话区分用户希望了解哪个门店的营业时间。

      在添加培训数据时，会自动向工作空间添加自然语言分类器，并对该分类器进行培训，使其理解您已指示服务应该听取并响应的请求类型。

    使用对话工具可构建包含意向和实体的对话流。对话流在工具中以图形方式表示为树。可以添加分支来处理希望服务处理的每个意向。然后，可以添加多个分支节点，以根据其他因素（例如，用户输入中找到的实体或从应用程序或其他外部服务传递到服务的信息）添加用于处理请求的许多可能的排列。

- **部署工作空间**。通过将已配置的工作空间连接到前端用户界面、社交媒体或消息传递通道，将该工作空间部署到用户。您部署的 {{site.data.keyword.conversationshort}} 服务实例由 IBM 云计算平台 {{site.data.keyword.cloud_notm}} 进行托管。（有关更多信息，请参阅[平台概述 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://console.bluemix.net/docs/overview/ibm-cloud.html#overview)。）

通过访问以下链接，阅读有关这些实施步骤的更多信息：

- [计划意向和实体](intents-entities.html#planning-your-entities)
- [对话概述](dialog-overview.html)
- [部署概述](deploy.html)

## 浏览器支持

{{site.data.keyword.conversationshort}} 服务工具需要的浏览器软件级别与 {{site.data.keyword.Bluemix_notm}} 所需的相同。有关详细信息，请参阅 {{site.data.keyword.Bluemix_notm}} [先决条件 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://console.bluemix.net/docs/overview/prereqs.html#browsers){: new_window} 主题。

## 语言支持

[支持的语言](lang-support.html)主题中详细描述了功能部件支持的语言。

## 后续步骤

- 服务[入门](getting-started.html)
- 试用某些[演示](sample-applications.html)。
- 查看 [SDK ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://www.ibm.com/watson/developercloud/developer-tools.html){: new_window} 列表。

仍然有疑问？请联系 [IBM 销售 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://www-01.ibm.com/marketing/iwm/dre/signup?source=urx-20970){: new_window}。
