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

# 助手
{: #assistants}

助手是一种认知机器人，您可以根据自己的业务需求进行定制，并跨多个通道进行部署，以随时随地根据客户需要向客户提供帮助。
{: shortdesc}

![技能](images/skill-icon.png) 通过向助手添加满足客户目标所需的技能，可对助手进行定制。

添加可以理解和处理客户通常需要帮助的问题或请求的对话技能。提供有关用户询问的主题或任务及其询问方式的信息后，服务会动态构建定制机器学习模型，用于理解相同和类似的用户请求。

|对话树|图形用户界面|
|-------------|-------------------------:|
|可以使用图形工具来创建助手与用户进行交互时，从中进行读取的对话，即模拟真实会话的对话。对话会从您指导其学会识别的常见客户目标中获取线索，并提供有用的响应。| ![包含示例内容的样本对话树](images/dialog-depiction.png) |

对话技能本身定义为文本，但您可以将其与 Watson Speech to Text 和 Watson Text to Speech 服务相集成，使用户能够与助手进行口头交互。

![开箱即用的训练数据](images/oob.png) 如果要快速入门，请将预构建的训练数据添加到对话技能，以便助手可以开始使用基本信息来帮助客户。

![IBM Cloud](images/cloud.png) 助手是由 {{site.data.keyword.cloud_notm}} 管理的全托管机器人，这意味着您无需担心支持助手的基础架构的设置和维护工作。


|集成|通道|
|--------------------|:----------|
|只需几个步骤，就可以通过多个接口来部署助手，包括现有消息传递通道（例如，Slack 和 Facebook Messenger）。或者，如果要设计包含助手的定制应用程序，那么可以直接调用底层 API 来实现。| ![集成方法，包括 Slack、Facebook Messenger、Web 应用程序或人工座席集成](images/integrations.png) |

首先，请参阅[创建助手](/docs/services/assistant?topic=assistant-assistant-add)。
