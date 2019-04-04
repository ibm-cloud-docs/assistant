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

# 技能
{: #skills}

对话技能包含训练数据和逻辑，用于支持助手为客户提供帮助。
{: shortdesc}

对话技能包含以下类型的工件：

- [**意向**](/docs/services/assistant?topic=assistant-intents)：*意向*表示用户输入的目的，例如有关业务位置或帐单支付的问题。您可为希望应用程序支持的每种类型的用户请求定义一个意向。在工具中，意向的名称始终以 `#` 字符为前缀。要训练对话技能来识别意向，请提供大量用户输入示例，并指示其映射到的意向。

  提供的*内容目录*包含预构建的常用意向，您可以将这些意向添加到应用程序，而不用构建自己的意向。例如，大多数应用程序需要一个问候意向，用于启动与用户的对话。您可以添加**常规**内容目录，以添加用于问候用户以及执行其他有用操作（如结束会话）的意向。

- [**对话**](/docs/services/assistant?topic=assistant-dialog-build)：*对话*是分支会话流，用于定义应用程序识别到所定义的意向和实体时如何进行响应。使用工具中的对话编辑器可创建与用户的会话，并根据在其输入中识别到的意向和实体来提供响应。

  ![仅使用意向和对话的基本实现的图。](images/basic-impl.png)

要支持对话技能处理更细致的问题，请定义实体，并在对话中引用这些实体。

- [**实体**](/docs/services/assistant?topic=assistant-entities)：*实体*表示与意向相关的术语或对象，并且为意向提供了特定上下文。例如，实体可以表示用户要在其中查找业务位置的城市或表示帐单支付金额。在工具中，实体的名称始终以 `@` 字符为前缀。

  可以通过提供实体词汇值和同义词，添加实体模式或通过识别实体通常在语句中使用时的上下文，以训练技能来识别实体。要优化对话，请返回并添加节点，用于在用户输入中除了检查意向外，还检查实体提及项。

![使用意向、实体和对话的更复杂实现的图。](images/complex-impl.png)

添加信息时，技能会使用这些独特的数据来构建机器学习模型，该模型可以识别这些用户输入和类似的用户输入。每次添加或更改训练数据时，都会触发训练过程，以确保底层模型保持最新，因为客户的需求以及客户希望讨论的主题会更改。

有关创建对话技能的帮助，请参阅[创建技能](/docs/services/assistant?topic=assistant-skill-add)。
