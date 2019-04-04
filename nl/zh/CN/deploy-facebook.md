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

# 与 Facebook Messenger 集成
{: #deploy-facebook}

Facebook Messenger 是一种移动消息传递应用程序，可帮助企业和客户直接相互通信。
{: shortdesc}

配置对话技能并将其添加到助手后，可以将助手与 Facebook Messenger 集成。

目前没有机制可通过 Facebook Messenger 来识别与助手进行交互的用户，这意味着无法识别或删除与特定用户关联的数据。因此，不要将此集成方法用于必须符合 GDPR 的部署。有关更多详细信息，请参阅[信息安全](/docs/services/assistant?topic=assistant-information-security)。
{: important}

1.  在“助手”选项卡中，单击以打开要部署的助手的磁贴。

1.  在“集成”部分中，单击**添加集成**。

1.  单击**选择集成**按钮以选择 *Facebook Messenger*。

1.  按照屏幕上提供的指示信息完成集成过程。

如果您希望像其他人一样执行部署步骤，请观看以下视频。

<iframe class="embed-responsive-item" id="youtubeplayer" title="Facebook 部署步骤全程指导" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/8o-FFU5sYNM?rel=0" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

持续时间：8 分钟

## 对话注意事项
{: #deploy-facebook-dialog}

添加到对话的富文本响应会按预期在 Facebook 应用程序中显示，但有以下例外情况：

- **连接到人工座席**：此响应类型会被忽略。

- **图像**：此响应类型会在响应中嵌入图像。在图像前面不会显示标题和描述，无论是否指定标题和描述。

- **选项**：此响应类型显示用户可以选择的选项列表。

  - 标题是**必需**的，会显示在选项列表前面。
  - 描述不会显示，无论是否指定。
  - 用户单击其中一个按钮后，按钮选项会消失，并替换为用户所选选项生成的用户输入。
如果助手或用户输入新的输入，那么按钮生成的输入会消失。因此，如果在单个响应中包含多个响应类型，请将“选项”响应类型放置在最后。否则，后续响应中的内容（例如，文本响应类型中的文本）将替换按钮生成的文本。

- **暂停**：此响应类型会暂停助手在 Messenger 中的活动。但是，活动在暂停后，要到触发其后的另一个响应类型时才会恢复。每当包含此响应类型时，请添加另一种不同的响应类型（例如，文本响应），并将其放置在此响应类型之后。

有关响应类型的更多信息，请参阅[富文本响应](/docs/services/assistant?topic=assistant-dialog-overview#dialog-overview-multimedia)。

## 与助手交谈
{: #deploy-facebook-try}

要开始与助手进行交谈，请完成以下步骤：

1.  打开 Facebook Messenger。
1.  输入先前创建的页面的名称。
1.  在该页面出现后，单击该页面，然后开始与助手进行交谈。

对话的“欢迎”节点不会由 Facebook Messenger 集成进行处理。在 Facebook 交谈中，不会像在工具的“试用”窗格中或在“预览链接”集成 Web 页面中那样显示欢迎消息。由于在用户启动的对话流中跳过了具有 `welcome` 特殊条件的节点，因此不会在此处触发此节点。Facebook Messenger 会等待用户启动会话。如果您需要在会话开始时设置上下文变量的缺省值，不要将这些变量设置在“欢迎”节点中。有关更多信息，请参阅[启动对话](/docs/services/assistant?topic=assistant-dialog-start)。
{: note}

当前会话的对话流在处于不活动状态 60 分钟（轻量和标准套餐为 5 分钟）后会重新启动。这意味着，如果用户停止与助手进行交互，那么在 60 分钟（或 5 分钟）后，先前会话期间设置的所有上下文变量值都会设置为 null 或恢复为其缺省值。
