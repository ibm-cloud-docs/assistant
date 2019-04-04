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

# 构建搜索技能
{: #skill-search-add}

助手使用*搜索技能*将复杂的客户查询路由到 {{site.data.keyword.discoveryfull}} 服务。{{site.data.keyword.discoveryshort}} 将用户输入视为搜索查询。它会在外部数据源中查找与查询相关的信息，并将其返回给助手。
{: shortdesc}

此功能仅可供 Beta 程序参与者使用。要了解如何请求访问权，请参阅[参与 Beta 程序](/docs/services/assistant?topic=assistant-feedback#feedback-beta)。

![Beta](images/beta.png) IBM 发布的供您评估的服务、功能和语言支持分类为 Beta。这些功能可能不太稳定，可能经常会更改，还可能会临时通知中断使用。此外，Beta 功能可能无法提供与一般可用功能所提供级别相同的性能或兼容性，并且 Beta 功能不适用于生产环境。

您可以向一个助手添加一个搜索技能。有关每个套餐的限制的信息，请参阅[技能限制](/docs/services/assistant?topic=assistant-skill-add#skill-add-limits)。

搜索技能将搜索使用 {{site.data.keyword.discoveryshort}} 服务创建的数据集合中的信息。{{site.data.keyword.discoveryshort}} 是一种用于搜寻、转换和规范化非结构化数据的服务。该服务会应用数据分析和认知直觉来扩充数据，以便您日后可以更轻松地在其中查找和检索有意义的信息。要阅读有关 {{site.data.keyword.discoveryshort}} 的更多信息，请参阅[产品文档 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](/docs/services/discovery?topic=discovery-about)。

{{site.data.keyword.discoveryfull}} 服务通过以下方式触发：

- **其他**节点：任何对话节点都无法处理用户的查询时，搜索外部数据源以查找相关回答。助手不会显示标准消息，例如`我不知道如何帮助您。`，而可能会显示`也许以下信息有所帮助：`，后跟搜索返回的一段文本。如果搜索技能链接到助手，那么无论何时触发 `anything_else` 节点，都会改为执行搜索，而不会显示该节点的响应。助手会将用户输入作为查询传递给搜索技能，并将搜索结果作为响应返回。
- **搜索响应类型**：如果向对话节点添加了搜索响应类型，那么服务将从外部数据源检索一段文本，并将其作为对特定问题的响应返回。仅当处理单个对话节点时，才会执行此类型的搜索。如果您希望在执行搜索之前缩小用户查询的范围，那么此方法非常有用。例如，对话分支可能会收集有关客户想要购买的设备类型的信息。了解品牌和型号后，可以在提交到搜索技能的查询中发送型号关键字，以获得更好的结果。
- **仅搜索技能**：如果只有搜索技能链接到助手，而没有对话技能链接到该助手，那么从该助手的其中一个集成通道收到任何用户输入时，会向 {{site.data.keyword.discoveryshort}} 服务提交搜索查询。

## 创建搜索技能
{: #skill-search-add-task}

如果尚未完成[入门教程](/docs/services/assistant?topic=assistant-getting-started#getting-started-prerequisites)中的先决条件步骤以创建 {{site.data.keyword.conversationshort}} 服务实例并启动 {{site.data.keyword.conversationshort}} 工具，请完成这些步骤。

您可使用 {{site.data.keyword.conversationshort}} 工具来创建技能。要创建搜索技能，请执行以下步骤：

1.  单击**技能**选项卡。

1.  单击**新建**。

1.  单击**添加**以创建*搜索技能*。

1.  指定新技能的详细信息：
    - **名称**：名称，长度不超过 100 个字符。名称是必填的。
    - **描述**：可选的描述，长度不超过 200 个字符。

1.  单击**创建**。

其余步骤根据您是否有权访问具有已创建集合的现有 {{site.data.keyword.discoveryshort}} 服务实例而有所不同。针对您的情况，请遵循相应的过程：

- [连接到现有 Watson Discovery 实例](#skill-search-add-connect-discovery)
- [创建 Watson Discovery 实例](#skill-search-add-create-discovery)

## 连接到现有 Watson Discovery 服务实例
{: #skill-search-add-connect-discovery}

1.  选择要从中抽取信息的 {{site.data.keyword.discoveryshort}} 服务实例。
{: #choose-d-instance}

    您有权访问的所有 {{site.data.keyword.discoveryshort}} 服务实例都会显示在列表中。

    如果看到警告称某些 {{site.data.keyword.discoveryshort}} 服务实例没有设置凭证，这意味着您有权访问至少一个您本人从未直接从 {{site.data.keyword.cloud_notm}} 仪表板打开的实例。您必须访问服务实例，才能为该实例创建凭证。凭证必须存在后，{{site.data.keyword.conversationshort}} 才能代表您与 {{site.data.keyword.discoveryshort}} 服务实例建立连接。如果您认为 {{site.data.keyword.discoveryshort}} 服务实例应该列出，但并未列出，请直接从 {{site.data.keyword.cloud_notm}} 仪表板打开该实例以生成其凭证。

1.  通过执行下列其中一个操作，指示要使用的数据集合：
{: #pick-data-collection}

    - 选择现有数据集合。

      在决定要使用的集合之前，可以单击*在 Discovery 中打开*链接以查看数据集合的配置。

      转至[配置搜索](#beta-search-skill-add-configure)。

    - 如果您没有集合或不想使用列出的任何数据集合，请单击**创建新集合**以添加集合。遵循[创建数据集合](#beta-search-skill-add-create-discovery-collection)中的过程。

      如果已达到基于 {{site.data.keyword.discoveryshort}} 服务套餐允许您创建的集合数的限制，那么不会显示**创建新集合**按钮。有关套餐限制详细信息，请参阅 [{{site.data.keyword.discoveryshort}} 价格套餐 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](/docs/services/discovery/discovery-about?topic=discovery-discovery-pricing-plans)。

## 创建 Watson Discovery 服务实例
{: #skill-search-add-create-discovery}

1.  要创建 {{site.data.keyword.discoveryshort}} 服务实例，请单击**创建新集合**。

    这将创建 {{site.data.keyword.discoveryshort}} 服务的实例，并且配置页面会打开到新的 {{site.data.keyword.discoveryshort}} 服务实例。

    该服务的轻量套餐实例是在 {{site.data.keyword.Bluemix_notm}} 中供应的，不管您使用的是什么 {{site.data.keyword.conversationshort}} 服务套餐。如果要在其他套餐中创建 {{site.data.keyword.discoveryshort}} 服务实例，请就此停止。改为直接通过 [{{site.data.keyword.Bluemix_notm}} 目录 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://cloud.ibm.com/catalog/services/discovery) 创建该服务实例，并遵循[连接到现有 {{site.data.keyword.discoveryshort}} 服务实例](#skill-search-add-connect-discovery)过程。
    {: important}

1.  查看使用实例的条款和条件，然后单击**接受**以继续。

1.  [创建数据集合](#skill-search-add-create-discovery-collection)。

## 创建数据集合
{: #skill-search-add-create-discovery-collection}

不要尝试将名为 *Watson Discovery News* 的预扩充数据源添加到实例。此数据源不是可以通过 {{site.data.keyword.conversationshort}} 进行搜索的数据类型。
{: important}

1.  要创建 {{site.data.keyword.discoveryshort}} 集合，请执行下列其中一个操作：

      - 要通过在 {{site.data.keyword.discoveryshort}} 为其提供内置支持的数据源类型中存储的数据来创建集合，请单击**连接到数据源**。

        1.  选取数据源类型。
        1.  提供所选数据源的必需信息，然后单击**连接**。

            有关更多详细信息，请参阅[连接到数据源 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](/docs/services/discovery?topic=discovery-sources)。
        1.  指示希望数据源中的数据与要在 {{site.data.keyword.discoveryshort}} 中创建的集合进行同步的频率。
        1.  指定要从数据源中抽取并包含在 {{site.data.keyword.discoveryshort}} 集合中的信息。

            显示的选项因数据源类型而有所不同。

            - 对于 Salesforce 数据源，请选择要从源文档中抽取的对象类型。例如，可选择表示*案例*（即客户问题）的 [Case 对象类型 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://developer.salesforce.com/docs/atlas.en-us.object_reference.meta/object_reference/sforce_api_objects_case.htm#!)。
            - 对于 Sharepoint 数据源，请指定路径。
            - 对于文件存储库，请指定目录或文件。

        1.  单击**保存并同步数据**。

            这将创建数据集合。此过程完成后，将在单独的 Web 浏览器选项卡中显示 {{site.data.keyword.discoveryshort}} 工具中的摘要页面。
        1.  单击**在 {{site.data.keyword.conversationshort}} 中配置技能**以返回到 {{site.data.keyword.conversationshort}} 工具。

      - 要通过上传文档来创建集合，请单击**上传您自己的数据**。

        1.  首先定义集合，然后上传文档。请提供以下信息：

            - 集合名称。名称对于此服务实例必须唯一。
            - 配置。可以选择使用缺省配置模板或保存的配置。有关配置的更多信息，请参阅[配置服务 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](/docs/services/discovery?topic=discovery-configservice)。
            - 语言。选择要添加到此集合的文件的语言。有关 {{site.data.keyword.discoveryshort}} 支持的语言的信息，请参阅[语言支持 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](/docs/services/discovery?topic=discovery-language-support)。
        1.  上传文档。

            支持的文件类型包括 PDF、HTML、JSON 和 DOC 文件。有关更多详细信息，请参阅[添加内容 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](/docs/services/discovery?topic=discovery-addcontent)。
            {: note}

            无法对上传的文档持续同步。如果要选取对文档进行的更改，请上传该文档的更高版本。

等待集合完全摄入后，再返回到 {{site.data.keyword.conversationshort}}。

## 配置搜索
{: #skill-search-add-configure}

1.  在 {{site.data.keyword.conversationshort}} 搜索技能页面中，单击**配置**。

1.  根据搜索的成功与否，起草不同的消息与用户共享。

    <table>
    <caption>搜索结果消息</caption>
    <tr>
      <th>字段名称</th>
      <th>场景</th>
      <th>示例消息</th>
    </tr>
    <tr>
      <td>消息</td>
      <td>返回了搜索结果</td>
      <td>我发现以下信息可能很有用：</td>
    </tr>
    <tr>
      <td>找不到结果</td>
      <td>找不到任何搜索结果</td>
      <td>我搜索了知识库来查找可处理查询的信息，但找不到任何有用的信息可共享。</td>
    </tr>
    <tr>
      <td>错误消息</td>
      <td>由于某种原因，服务无法执行搜索</td>
      <td>我可能有信息可以帮助处理您的查询，但目前我无法搜索知识库。</td>
    </tr>
    </table>

1.  选择要从中抽取文本的 {{site.data.keyword.discoveryshort}} 集合字段。

    可用的字段根据摄入的数据以及用于摄入数据的配置而有所不同。

    每个搜索结果都会包含以下信息部分：

    - **标题**：搜索结果标题。使用集合中字段的标题、名称或类似类型的字段作为搜索结果标题。

      必须为 Facebook 和 Slack 集成选择除`无`以外的其他选项才能显示响应。
    - **主体**：搜索结果描述。使用集合中的梗概、摘要或要点字段作为搜索结果主体。

      必须为 Facebook 和 Slack 集成选择除`无`以外的其他选项才能显示响应。
    - **URL**：指向其本机数据源中的原始数据对象的超文本链接。大多数联机数据源提供了用于存储中对象的自引用公共 URL，以支持直接访问。

      生成的 URL 必须有效且可访问，Slack 集成才能在响应中包含该 URL，Facebook 集成才能显示该响应。对于 Facebook 和 Slack 集成，`无`是可接受的选项。

    要获取帮助，请参阅[有关集合字段选项的提示](#skill-search-add-field-tips)。
  
    必须对于至少一个选项，选择除`无`以外的值。

    如果下拉字段中没有可用的选项，那么可能需要给 {{site.data.keyword.discoveryshort}} 更多时间来完成创建集合的操作。否则，集合可能不会包含任何文档，或者可能有需要首先解决的摄入错误。

1.  在预览窗格中，输入测试消息以查看将配置选项应用于搜索时会返回的结果。根据需要进行调整。

1.  单击**创建**。

如果日后要更改配置，请再次打开搜索技能并进行编辑。您无需边更改边保存；更改会自动应用。对搜索结果感到满意后，单击**保存**以完成配置搜索技能的操作。

## 后续步骤
{: #skill-search-add-next-steps}

创建技能后，该技能即会在“技能”页面上显示为磁贴。

在将搜索技能添加到助手并部署助手之前，搜索技能无法与客户交互。请参阅[创建助手](/docs/services/assistant?topic=assistant-assistant-add)。

将对话技能和搜索技能链接到助手后，如果用户输入已由对话技能处理，但无法由其任何对话节点处理，那么会自动触发搜索技能。这将启动使用用户输入作为其查询字符串的搜索，而不是使用来自 `anything_else` 节点的通用响应进行回复。

如果需要，可以定义为了响应特定节点条件而要调用的特定搜索查询。为此，请向对话节点添加搜索响应类型。有关更多详细信息，请参阅[响应](/docs/services/assistant?topic=assistant-dialog-overview#dialog-overview-multimedia)。

如果从对话技能启动了任何类型的搜索，请测试对话以确保搜索按预期触发。例如，如果未使用搜索响应类型，请测试是否仅当没有现有对话节点可以处理用户输入时，才会触发搜索。在任何时候触发搜索时，确保该搜索会返回有意义的结果。

### 有关集合字段选项的提示
{: #skill-search-add-field-tips}

用于从中抽取数据的相应集合字段根据集合的数据源和用于摄入数据源的配置而有所不同。要了解有关集合中文档结构的更多信息，包括含有可能要抽取的信息的字段的名称，请在 {{site.data.keyword.discoveryshort}} 工具中打开该集合，然后单击**查看数据模式**。

下表提供了可以在开始时尝试的集合字段。这些建议假定您在创建集合时使用的是缺省配置模板。

|数据源类型|标题|主体|URL|
|--------------------|-------|------|-----|
|上传的 PDF 文档|enriched_text.concepts.text|text          |无|
|Box|name|description   |listing_url|

集合字段是在创建集合时创建的。要了解有关使用缺省配置模板进行摄入时生成的字段（例如，`enriched_text.concepts.text`）的更多信息，请参阅[配置服务 > 添加扩充项 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](/docs/services/discovery?topic=discovery-configservice#adding-enrichments)。

### 向助手添加技能
{: #skill-search-add-to-assistant}

您可以向一个助手添加一个技能。必须打开助手磁贴，并在助手配置页面中向该助手添加技能；无法在技能配置页面中选择将使用该技能的助手。

一个搜索技能可以由多个助手使用。

1.  在“助手”选项卡中，单击以打开要向其添加技能的助手的磁贴。

1.  单击**添加搜索技能**。

1.  单击**添加现有技能**。

    在显示的可用技能中，单击要添加的技能。

配置至少一个测试集成通道。不能在“试用”窗格中测试搜索技能。在该集成通道中，通过输入用于触发搜索的查询来测试技能。确保搜索会正确触发，并且返回相关的结果。

目前，可共享的链接集成不适用于使用搜索技能的助手。
{: important}
