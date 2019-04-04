---

copyright:
  years: 2015, 2019
lastupdated: "2019-02-12"

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

# 获取有关构建意向的帮助 ![Beta](images/beta.png) ![仅限增强版或高端套餐](images/premium.png)
{: #beta-intent-recommendations}

如果您有现有的企业客户支持交谈抄本数据，请让 Watson 分析这些数据，以了解支持团队将大部分时间用于处理的客户需求。
{: shortdesc}

此功能仅可供 Beta 程序参与者使用。要了解如何请求访问权，请参阅[参与 Beta 程序](/docs/services/assistant?topic=assistant-feedback#feedback-beta)。

![Beta](images/beta.png) IBM 发布的供您评估的服务、功能和语言支持分类为 Beta。这些功能可能不太稳定，可能经常会更改，还可能会临时通知中断使用。此外，Beta 功能可能无法提供与一般可用功能所提供级别相同的性能或兼容性，并且 Beta 功能不适用于生产环境。

此功能发布后，可供增强版或高端套餐用户使用，但只能处理英语语言发声。
{: tip}

客户需求在 {{site.data.keyword.conversationshort}} 中表示为*意向*。如果您尚未定义意向，那么可以通过向 Watson 请求帮助来更快上手。将呼叫中心抄本中包含用户发声的文件上传到 {{site.data.keyword.conversationshort}} 服务进行分析。该服务会根据所发现的洞察，建议您应构建的一组基本意向，以涵盖客户最常发生的需求。

随着客户希望讨论的主题发生变化，您可以使用意向用户示例建议功能来帮助意向随时间变化保持最新且相关。

挖掘现有数据以执行下列其中一个任务：

- [获取意向建议](#beta-intent-recommendations-get-intent-recommendations)
- [获取意向用户示例建议](#beta-intent-recommendations-get-example-recommendations)

## 上传文件
{: #beta-intent-recommendations-log-files-add}

开始之前，请创建要提供给服务的文件。该文件必须是逗号分隔值 (CSV) 文件，其中一行一个用户发声。理想情况下，发声是从呼叫中心抄本中抽取的简短用语，其中包含真实世界的客户问题和请求。每个用户发声文件的最大大小可以为 20 MB。

请遵循以下更多准则：

  - 从文件中包含的发声中除去任何敏感数据。

    敏感数据包括与可识别的自然人相关的任何信息（例如，姓名、电子邮件地址和客户标识）以及受监管的数据（例如，受保护的健康信息）。
  - 不要包含长度超过 1024 个字符的用户发声。超过此长度的发声会被截断。
  - 文件必须至少包含 100 个发声；包含 500 个或更多发声的文件将提供更好的结果。
  - 如果发声包含逗号，请用引号将发声括起。
  - CSV 只能包含一列。
  - 除去不属于用户生成的发声的所有内容，包括任何人工座席响应或注释。

  例如：

  ```
  What happens to my coverage if I trade in my car?
  i'd like to buy a house.
  How do I add a dependent to my plan?
  "first, i want to know if i am already registered."
  ```

上传的任何文件都将在当前服务实例中的所有技能之间共享。当您询问意向建议和意向用户示例建议时，系统将从所有可用文件中挖掘发声。

## 从 Watson 获取意向建议
{: #beta-intent-recommendations-get-intent-recommendations}

通过让服务分析呼叫中心交谈抄本，并为您建议一些初始意向以开始使用，您可以快速入门。如果您已创建了某些意向，请让服务分析日志，并将其发现结果与现有意向进行比较，以识别训练数据中的缺漏之处，并建议新的意向来填补这些缺漏。

要使用此功能，请上传一个或多个文件，其中包含真实用户在请求帮助时提交的发声。例如，文件可能会列出从呼叫中心日志中抽取的用户请求。服务会对这些发声进行评估，并识别客户经常提及的常见问题方面。然后，{{site.data.keyword.conversationshort}} 工具会显示一组不同的意向，用于捕获这些热门用户需求。您可以查看每个建议的意向及其对应的用户示例，以选择要添加到训练数据中的内容。

## 获取意向建议
{: #beta-intent-recommendations-get-intent-recommendations-task}

要获取意向建议，请完成以下步骤：

1.  在 {{site.data.keyword.conversationshort}} 工具中，打开对话技能，然后单击导航栏中的**意向**选项卡。如果**意向**选项卡未显示，请使用 ![菜单](images/Menu_16.png) 菜单来打开该页面。

1.  单击**获取建议**。

1.  **仅限首次**：单击**添加文件**，然后单击**选择文件**以浏览查找先前创建的 CSV 文件，并选择该文件。

    给服务一些时间来分析数据并对发声分组。

1.  复查服务建议的意向。

    最先列出的意向中捕获的是最常发生的客户需求。可以将鼠标悬停在意向上以查看其关联发声的若干示例。如果要查看所有意向分组的列表，请单击**显示所有建议**。

1.  单击意向以查看与其关联的一组完整用户示例，然后执行下列其中一个操作：

    取消选择不想添加为用户示例的任何发声。

    - 要将包含所选发声的建议意向添加为用户示例，请单击**创建意向**。
    - 要改为将建议意向中的所选发声作为用户示例添加到某个现有意向，请单击**添加到现有意向**，选择该意向，然后单击**添加**。

以这种方式添加的意向和意向用户示例会计入意向和意向用户示例总数，每个套餐对此都有限制。有关更多详细信息，请参阅[意向限制](/docs/services/assistant?topic=assistant-intents#intents-limits)。

## 获取意向用户示例建议
{: #beta-intent-recommendations-get-example-recommendations}

以下视频提供了有关如何获取意向用户示例建议的为时 2 分钟的概述。

<iframe class="embed-responsive-item" id="youtubeplayer" title="意向用户示例建议" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/L3FI8KeZfsc?rel=0" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

要获取现有意向的用户示例，可以使用早先上传的相同文件。您无需将示例与意向相关联。只需提供原始用户发声，然后让服务来执行选择适合当前意向的发声的工作即可。对于每个意向，服务会分析相同的数据，以查找适合该意向的用户示例。

1.  执行[创建意向](/docs/services/assistant?topic=assistant-intents#intents-creating-intents-task)中的步骤来创建意向。

1.  至少添加 5 个用户示例，以说明您预期用户为了触发此意向而可能会说出的典型发声的完整范围。

    这些种子用户示例指导服务学习要在上传的文件中查找的发声类型。

1.  单击**显示建议**。

    用户示例源文件会在服务实例中的技能之间共享。如果您的同事拥有同一实例中的技能并上传了文件，那么会将他们的文件添加到您的共享*用户示例源文件*集合中。

1.  **仅限首次**：单击**上传文件**，然后单击**选择文件**以浏览查找先前创建的 CSV 文件，并选择该文件。

    如果上传的文件包含所有类型的意向的发声，那非常好。服务会知道您在处理的意向，并查找针对此特定意向要建议的合适示例。

    在文件上传并由服务进行处理后，将显示建议的发声。如果未提出任何建议，说明该文件不包含适用于此意向的示例。

1.  如果文件无法为任何意向提供有用的建议，那么可以通过上传其他文件来尝试一组不同的发声。有关更多详细信息，请参阅[管理上传的文件](#beta-intent-recommendations-manage-files)。

1.  服务显示建议后，选择要添加为此意向的用户示例的发声，然后单击**添加**。或者，单击**下一组**以查看更多发声。
1.  如果您希望自行在 CSV 文件的内容中搜索用户示例，请单击**搜索日志**选项卡，输入要将搜索基于的关键字，然后按 **Enter** 键。

    遵循以下搜索查询语法准则：

    - 支持布尔运算符（例如，`AND` 和 `OR`）。
    - 添加用引号括起的文本可搜索完全匹配的文本 ("thisstringmustbepresent")。
    - 可以使用正则表达式（例如，`*ly`）来查找以 `ly` 结尾的所有词汇。
    - 以下字符用作正则表达式运算符：

      `+ - = && || > < ! ( ) { } [ ] ^ " ~ * ? : \ /`

      如果要将其中一个字符包含在搜索项中而不使其作为运算符进行处理，那么必须以反斜杠 (`\`) 作为该字符的前缀。

以这种方式添加的用户示例会计入意向用户示例总数，每个套餐对此都有限制。有关更多详细信息，请参阅[意向限制](/docs/services/assistant?topic=assistant-intents#intents-limits)。

## 管理上传的文件
{: #beta-intent-recommendations-manage-files}

至少上传一个文件后，可以打开*用户示例源文件*集合来管理文件。上传的文件将添加到在当前 {{site.data.keyword.conversationshort}} 服务实例中共享的文件集合。

1.  要访问该集合，请单击以打开意向，单击**显示建议**，然后单击侧边栏中的**查看文件**。

    如果还未添加任何意向，请在主“意向”页面中展开*意向建议*部分，单击**获取建议**，单击**显示所有建议**，然后单击**查看文件**。

1.  可以执行以下操作：

    - 要添加文件，请单击**添加文件**，然后浏览查找文件并选择该文件。
    - 要删除文件，必须除去已从与此服务实例关联的任何技能上传的所有文件；不能仅删除一个文件。首先，确保没有其他人在使用这些文件，然后单击**全部删除**以删除所有上传的文件。

1.  关闭*用户示例源文件*页面。
