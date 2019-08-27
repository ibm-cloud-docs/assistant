---

copyright:
  years: 2015, 2019
lastupdated: "2019-07-26"

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

# 助手
{: #assistants}

助手是一种认知机器人，您可以根据自己的业务需求进行定制，并跨多个通道进行部署，以随时随地根据客户需要向客户提供帮助。
{: shortdesc}

![技能](images/skill-icon.png) 助手将客户查询路由到技能，然后该技能会提供相应的响应。对话技能返回的是您为了回答常见问题而编写的响应，而搜索技能是搜索现有自助服务内容并返回几段文本，以回答更复杂的查询。

## 对话技能
{: #assistants-dialog-skill}

对话技能可以理解和处理客户通常需要帮助的问题或请求。提供有关用户询问的主题或任务及其询问方式的信息后，本产品会动态构建定制机器学习模型，用于理解相同和类似的用户请求。

|对话树|图形用户界面|
|-------------|-------------------------:|
|可以使用图形对话编辑器创建各种类型的脚本，以供助手在与客户交互时从中读取。结果是得到模拟真实会话的对话。对话会从您指导其学会识别的常见客户目标中获取线索，并提供有用的响应。| ![包含示例内容的样本对话树](images/dialog-depiction.png) |

对话技能本身定义为文本，但您可以将其与 Watson Speech to Text 和 Watson Text to Speech 服务相集成，使用户能够与助手进行口头交互。

![开箱即用的训练数据](images/oob.png) 如果要快速入门，请将预构建的训练数据添加到对话技能，以便助手可以开始使用基本信息来帮助客户。

## 搜索技能 ![仅限增强版或高端套餐](images/plus.png)
{: #assistants-search-skill}

搜索技能仅可供增强版或高端套餐用户使用。
{: note}

搜索技能利用现有企业知识库或主题专家撰写的其他内容集合中的信息，解决意外或更细致的客户查询。

![IBM Cloud](images/cloud.png) 助手是由 {{site.data.keyword.Bluemix_notm}} 管理的全托管机器人，这意味着您无需担心支持助手的基础架构的设置和维护工作。


|集成|通道|
|--------------------|:----------|
|只需几个步骤，就可以通过多个接口来部署助手，包括现有消息传递通道（例如，Slack 和 Facebook Messenger）。或者，如果要设计包含助手的定制应用程序，那么可以直接调用底层 API 来实现。| ![集成方法，包括 Slack、Facebook Messenger、Web 应用程序或人工座席集成](images/integrations.png) |

请参阅[创建助手](/docs/services/assistant?topic=assistant-assistant-add)以开始使用。
