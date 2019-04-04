---

copyright:
  years: 2015, 2019
lastupdated: "2019-03-01"

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

# 与 Slack 集成
{: #deploy-slack}

Slack 是一种基于云的消息传递应用程序，可帮助人们相互进行协作。
{: shortdesc}

配置对话技能并将其添加到助手后，可以将助手与 Slack 集成。

1.  在“助手”选项卡中，单击以打开要部署的助手的磁贴。

1.  在“集成”部分中，单击**添加集成**。

1.  单击 *Slack* 的**选择集成**按钮。

1.  按照屏幕上提供的指示信息完成集成过程。

如果您希望像其他人一样执行部署步骤，请观看以下视频。

<iframe class="embed-responsive-item" id="youtubeplayer" title="Slack 部署步骤全程指导" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/RBGBPJ8h4HQ?rel=0" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

持续时间：9.5 分钟

## 对话注意事项
{: #deploy-slack-dialog}

添加到对话的富文本响应会按预期在 Slack 通道中显示，但有以下例外情况：

- **连接到人工座席**：此响应类型会被忽略。

- **图像**：图像标题是必需的。如果未提供标题，那么不会显示图像。

- **选项**：此响应类型显示用户可以选择的选项列表。

  - 标题是**必需**的，会显示在选项列表前面。
  - 描述不会显示，无论是否指定。
  - 用户单击其中一个按钮后，按钮选项会消失，并替换为用户所选选项生成的用户输入。因此，如果在单个响应中包含多个响应类型，请将“选项”响应类型放置在最后。否则，输出将包含响应与用户输入的混合内容，可能会使用户感到困惑。

- **暂停**：此响应类型会暂停助手在 Slack 通道中的活动。但是，不会显示任何可视指示符来指示助手已暂停，无论是否选择显示正在输入。

有关响应类型的更多信息，请参阅[富文本响应](/docs/services/assistant?topic=assistant-dialog-overview#dialog-overview-multimedia)。

## 与助手交谈
{: #deploy-slack-try}

要开始与助手进行交谈，请完成以下步骤：

1.  打开 Slack，然后转至与应用程序关联的工作空间。
1.  单击在“应用程序”部分中创建的应用程序。
1.  与助手进行交谈。

对话的“欢迎”节点不会由 Slack 集成进行处理。在 Slack 通道中，不会像在工具的“试用”窗格中或在“预览链接”集成 Web 页面中那样显示欢迎消息。由于在用户启动的对话流中跳过了具有 `welcome` 特殊条件的节点，因此不会在此处触发此节点。Slack 会等待用户启动会话。如果您需要在会话开始时设置上下文变量的缺省值，不要将这些变量设置在“欢迎”节点中。有关更多信息，请参阅[启动对话](/docs/services/assistant?topic=assistant-dialog-start)。
{: note}

当前会话的对话流在处于不活动状态 60 分钟（轻量和标准套餐为 5 分钟）后会重新启动。这意味着，如果用户停止与助手进行交互，那么在 60 分钟（或 5 分钟）后，先前会话期间设置的所有上下文变量值都会设置为 null 或恢复为其缺省值。
