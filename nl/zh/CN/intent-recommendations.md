---

copyright:
  years: 2015, 2019
lastupdated: "2019-07-19"

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

# 获取定义意向的帮助 ![仅限增强版或高端套餐](images/plus.png)
{: #intent-recommendations}

如果您已有企业客户支持交谈记录数据，请让 Watson 分析这些数据，以了解支持团队将大部分时间用于处理哪些客户需求。
然后，Watson 可以建议可用于训练助手的意向和用户示例，以便助手未来可识别到相同和类似的请求。
{: shortdesc}

此功能可供增强版或高端套餐用户使用。
{: note}

客户需求在 {{site.data.keyword.conversationshort}} 中表示为*意向*。如果您尚未定义意向，那么可以通过向 Watson 请求帮助来更快上手。将包含呼叫中心交谈记录中的客户话语的文件上传到 {{site.data.keyword.conversationshort}} 服务进行分析。Watson 会根据所发现的洞察，建议您应构建的一组基本意向，以涵盖客户最常发生的需求。

随着客户希望讨论的主题发生变化，您可以使用意向用户示例建议功能来帮助意向随时间变化保持最新且相关。

以下视频提供了有关意向和意向用户示例建议的为时三分半钟的概述。

<iframe class="embed-responsive-item" id="youtubeplayer" title="意向建议概述" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/64h59KqDY98?rel=0" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

要了解有关意向建议如何帮助更快构建有用的机器人的更多信息，请[阅读此博客帖子 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://medium.com/ibm-watson/let-watson-train-your-virtual-assistant-57bd53b12bc3){: new_window}。

挖掘现有数据以执行下列其中一个操作：

- [获取意向建议](#intent-recommendations-get-intent-recommendations)
- [获取意向用户示例建议](#intent-recommendations-get-example-recommendations)

有关此功能的语言支持的信息，请参阅[支持的语言](/docs/services/assistant?topic=assistant-language-support)。

## 创建用户示例源文件
{: #intent-recommendations-log-files-add}

您必须以正确的格式提供数据后，Watson 才能对数据进行分析。创建逗号分隔值 (CSV) 文件，其中一行一条客户话语。理想情况下，话语是从包含现实世界客户问题和请求的呼叫中心交谈记录中抽取的简短用语。每个用户示例源文件的最大大小可以为 20 MB。

请遵循以下更多准则：

  - 从文件中包含的话语中除去任何敏感数据。

    敏感数据包括与可识别的自然人相关的任何信息（例如，姓名、电子邮件地址和客户标识）以及受监管的数据（例如，受保护的健康信息）。
  - 不要包含长度超过 1024 个字符的话语。超过此长度的话语会被截断。
  - 文件必须至少包含 100 条话语；包含 500 条或更多话语的文件将提供更好的结果。
  - 如果话语包含逗号，请用引号将话语括起。
  - CSV 只能包含一列。
  - 除去不属于客户生成的话语的所有内容，包括任何人工座席响应或注释。

  例如：

  ```
  如果我卖车，我的车险责任范围会怎样？
  我想买一套房子。
  如何将被扶养人添加到我的保险计划？
  "首先，我想知道我是否已注册。"
  ```

上传的任何文件都将在当前服务实例中的所有技能之间共享。当您询问意向建议和意向用户示例建议时，系统将从所有可用文件中挖掘话语。

## 从 Watson 获取意向建议
{: #intent-recommendations-get-intent-recommendations}

首先，让 Watson 分析呼叫中心交谈记录，并为您建议一些初始意向以开始使用。如果您已创建了某些意向，请让 Watson 分析日志，并将其发现结果与现有意向进行比较，以识别训练数据中的缺漏之处，并建议可以填补这些缺漏的新意向。

要使用此功能，请上传包含客户话语的文件。Watson 会对这些话语进行评估，并识别客户经常提及的常见问题方面。然后，{{site.data.keyword.conversationshort}} 会显示一组不同的意向，用于捕获这些热门的客户需求。您可以查看每个建议的意向及其对应的用户示例，以选择要添加到训练数据中的内容。

## 获取意向建议
{: #intent-recommendations-get-intent-recommendations-task}

开始之前，请使用数据创建 CSV 文件。请参阅[创建用户示例源文件](#intent-recommendations-log-files-add)。

要获取意向建议，请完成以下步骤：

1.  打开对话技能。技能将打开到**意向**页面。

1.  单击**获取建议**。

1.  **仅限首次**：单击**添加文件**，然后单击**选择文件**以浏览查找先前创建的 CSV 文件，并选择该文件。

    给 Watson 一些时间来分析数据并将话语分组。

1.  复查 Watson 建议的意向。

    意向建议的排序方式如下：

    1.  具有最频繁出现的话语的意向。与这些意向关联的话语的频率表明这些意向识别到最常见的客户痛点。
    1.  这些意向与已添加到技能的意向之间的差异程度。意向的唯一性表明这些意向可以满足目前未满足的客户需求。
    1.  意向中用户示例的彼此相似程度，这指示意向的强度。

    可以将鼠标悬停在意向上以查看其关联话语的若干示例。如果要查看所有意向分组的列表，请单击**显示所有建议**。

1.  单击意向以查看与其关联的一组完整用户示例，然后执行下列其中一个操作：

    取消选择不想添加为用户示例的任何话语。

    - 要将包含所选话语的建议意向添加为用户示例，请单击**创建意向**。
    - 要改为将建议意向中的所选话语作为用户示例添加到某个现有意向，请单击**添加到现有意向**，选择该意向，然后单击**添加**。

以这种方式添加的意向和意向用户示例会计入意向和意向用户示例总数，每个套餐对此都有限制。有关更多详细信息，请参阅[意向限制](/docs/services/assistant?topic=assistant-intents#intents-limits)。

随着客户希望讨论的主题发生变化，您可以使用意向用户示例建议功能来帮助意向随时间变化保持最新且相关。

## 获取意向用户示例建议
{: #intent-recommendations-get-example-recommendations}

向 Watson 提供用于查找意向用户示例的数据无需将这些示例与意向相关联。只需提供原始客户话语，然后让 Watson 来执行选择适合当前意向的话语的工作即可。对于每个意向，Watson 都会分析相同的数据，以查找适合该意向的用户示例。

开始之前，请使用数据创建 CSV 文件。请参阅[创建用户示例源文件](#intent-recommendations-log-files-add)。

1.  执行[创建意向](/docs/services/assistant?topic=assistant-intents#intents-creating-intents-task)中的步骤来创建意向。

1.  至少添加 5 个用户示例，以说明您预期客户为了触发此意向而可能会说出的典型话语的完整范围。

    这些种子用户示例指导 Watson 学习要在上传的文件中查找的话语类型。

1.  单击**显示建议**。

    用户示例源文件会在服务实例中的技能之间共享。如果您的同事拥有同一实例中的技能并上传了文件，那么会将他们的文件添加到您的共享*用户示例源文件*集合中。

1.  **仅限首次**：单击**上传文件**，然后单击**选择文件**以浏览查找先前创建的 CSV 文件，并选择该文件。

    如果上传的文件包含所有类型的意向的话语，那非常好。Watson 会知道您在处理的意向，并查找针对此特定意向要建议的合适示例。

    在文件上传并由 Watson 进行处理后，将显示建议的话语。如果未提出任何建议，说明该文件不包含适用于此意向的示例。

1.  如果文件无法为任何意向提供有用的建议，那么可以通过上传其他文件来尝试一组不同的话语。有关更多详细信息，请参阅[管理上传的文件](#intent-recommendations-manage-files)。

1.  Watson 显示建议后，请选择要添加为此意向的用户示例的话语，然后单击**添加**。或者，单击**下一组**以查看更多话语。
1.  如果您希望自行在 CSV 文件的内容中搜索用户示例，请单击**搜索日志**选项卡，输入要作为搜索基础的关键字，然后按 **Enter** 键。

    遵循以下搜索查询语法准则：

    - 支持布尔运算符（例如，`AND` 和 `OR`）。
    - 添加用引号括起的文本可搜索完全匹配的文本 ("thisstringmustbepresent")。
    - 可以使用正则表达式（例如，`*ly`）来查找以 `ly` 结尾的所有词汇。
    - 以下字符用作正则表达式运算符：

      `+ - = && || > < ! ( ) { } [ ] ^ " ~ * ? : \ /`

      如果要将其中一个字符包含在搜索项中而不使其作为运算符进行处理，那么必须以反斜杠 (`\`) 作为该字符的前缀。

以这种方式添加的用户示例会计入意向用户示例总数，每个套餐对此都有限制。有关更多详细信息，请参阅[意向限制](/docs/services/assistant?topic=assistant-intents#intents-limits)。

## 管理上传的文件
{: #intent-recommendations-manage-files}

至少上传一个文件后，可以打开*用户示例源文件*集合来管理文件。上传的文件将添加到在当前 {{site.data.keyword.conversationshort}} 服务实例中共享的文件集合。

1.  要访问该集合，请单击以打开意向，单击**显示建议**，然后单击侧边栏中的**查看文件**。

    如果还未添加任何意向，请在主“意向”页面中展开*意向建议*部分，单击**获取建议**，单击**显示所有建议**，然后单击**查看文件**。

1.  可以执行以下操作：

    - 要添加文件，请单击**添加文件**，然后浏览查找文件并选择该文件。
    - 要删除文件，必须除去已从与此服务实例关联的任何技能上传的所有文件；不能仅删除一个文件。首先，确保没有其他人在使用这些文件，然后单击**全部删除**以删除所有上传的文件。

1.  关闭*用户示例源文件*页面。
