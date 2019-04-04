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

# Beta
{: #beta}

![Beta](images/beta.png) IBM 发布的供您评估的服务、功能和语言支持分类为 Beta。这些功能可能不太稳定，可能经常会更改，还可能会临时通知中断使用。此外，Beta 功能可能无法提供与一般可用功能所提供级别相同的性能或兼容性，并且 Beta 功能不适用于生产环境。Beta 功能仅在 [IBM Developer Answers ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://developer.ibm.com/answers/topics/watson-assistant/){: new_window} 上受支持。

## 可用的 Beta 功能
{: #beta-features}

以下功能仅可供 Beta 程序参与者使用。要了解如何请求访问权，请参阅[参与 Beta 程序](/docs/services/assistant?topic=assistant-feedback#feedback-beta)。

- 使用搜索技能的方式已更改。现在，可以向同一助手添加一个搜索技能和一个对话技能。同时添加这两种技能后，如果对话技能的对话中任何节点都无法处理用户输入，才会触发搜索。可以从以下主题中了解更多信息：

  - [搜索技能](/docs/services/assistant?topic=assistant-skill-search-add)
  - [对话技能](/docs/services/assistant?topic=assistant-beta-skill-dialog-add)

  此功能发布后，仅可供增强版或高端套餐用户使用。

- 对话构建器的用户界面已更新为使用 React JavaScript 库。现在，对话函数在自行管理状态的封装组件中提供，因此提供了响应性更好的用户体验。

- 可以配置技能来更正用户输入中的拼写错误。有关更多详细信息，请参阅[更正用户输入](/docs/services/assistant?topic=assistant-beta-spell-check)。

- 利用现有客户支持交谈抄本来查找用于训练助手的相应意向和用户示例集。有关更多详细信息，请参阅[获取构建意向的帮助](/docs/services/assistant?topic=assistant-beta-intent-recommendations)。
