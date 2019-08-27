---

copyright:
  years: 2015, 2019
lastupdated: "2019-08-02"

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

# 创建搜索技能 ![仅限增强版或高端套餐](images/plus.png)
{: #skill-search-add}

助手使用*搜索技能*将复杂的客户查询路由到 {{site.data.keyword.discoveryfull}} 服务。{{site.data.keyword.discoveryshort}} 将用户输入视为搜索查询。它会在外部数据源中查找与查询相关的信息，并将其返回给助手。
{: shortdesc}

此功能仅可供增强版或高端套餐用户使用。
{: note}

向助手添加搜索技能，这样助手就不必说类似下面的内容：`很抱歉，我帮不上忙`。助手可以改为查询现有公司文档或数据，以了解是否可以找到任何有用信息并与客户共享。

![在预览链接集成中显示搜索结果](images/search-skill-preview-link.png)

以下 4 分钟的视频提供了搜索技能概述。

<iframe class="embed-responsive-item" id="youtubeplayer" title="搜索技能概述" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/ZcgGf8J2Cfw?rel=0" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

要了解有关搜索技能对您的企业有何助益的更多信息，请[阅读此博客帖子 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://medium.com/ibm-watson/adding-search-to-watson-assistant-99e4e81839e5){: new_window}。

## 工作方式
{: #skill-search-add-how}

搜索技能将搜索使用 {{site.data.keyword.discoveryshort}} 服务创建的数据集合中的信息。

{{site.data.keyword.discoveryshort}} 是一种用于搜寻、转换和规范化非结构化数据的服务。该产品会应用数据分析和认知直觉来扩充数据，以便您日后可以更轻松地在其中查找和检索有意义的信息。要阅读有关 {{site.data.keyword.discoveryshort}} 的更多信息，请参阅[产品文档 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](/docs/services/discovery?topic=discovery-about){: new_window}。

通常，添加到 {{site.data.keyword.discoveryshort}} 以及通过助手访问的数据集合的类型包含您公司拥有的信息。这些专有信息可能包括常见问题、销售宣传材料、技术手册或主题专家撰写的文章。挖掘这种密集型专有信息集合，可快速找到客户问题的答案。

下图说明了向助手添加对话技能和搜索技能后如何处理用户输入。

![显示对话如何回答某些用户输入以及如何通过搜索回答其他问题的图。](images/search-skill-diagram.png)

## 开始之前
{: #skill-search-add-prereqs}

如果您没有 {{site.data.keyword.discoveryshort}} 服务实例，那么在此过程中将为您供应免费轻量套餐实例。如果您有现有的 {{site.data.keyword.discoveryshort}} 服务实例，请连接到该服务实例；在此过程中，不会要求您创建新实例。

如果首先创建 Discovery 实例，请不要向实例添加名为 *Watson Discovery News* 的预扩充数据源。此数据源不是可以通过 {{site.data.keyword.conversationshort}} 进行搜索的数据类型。
{: tip}

## 创建搜索技能
{: #skill-search-add-task}

1.  单击**技能**选项卡，然后单击**创建技能**。

1.  单击*搜索技能*磁贴，然后单击**下一步**。

    仅当您是增强版或高端套餐用户时，才能选择“搜索技能”。
    {: note}

1.  指定新技能的详细信息：
    - **名称**：名称，长度不超过 100 个字符。名称是必填的。
    - **描述**：可选的描述，长度不超过 200 个字符。

1.  单击**继续**。

其余步骤根据您是否有权访问具有已创建集合的现有 {{site.data.keyword.discoveryshort}} 服务实例而有所不同。针对您的情况，请遵循相应的过程：

- [连接到现有 Watson Discovery 实例](#skill-search-add-connect-discovery)
- [创建 Watson Discovery 实例](#skill-search-add-create-discovery)

## 连接到现有 Watson Discovery 服务实例
{: #skill-search-add-connect-discovery}

1.  选择要从中抽取信息的 {{site.data.keyword.discoveryshort}} 服务实例。
{: #choose-d-instance}

    您有权访问的所有 {{site.data.keyword.discoveryshort}} 服务实例都会显示在列表中。

    如果看到警告称某些 {{site.data.keyword.discoveryshort}} 服务实例没有设置凭证，这意味着您可以访问您本人从未直接在 {{site.data.keyword.cloud_notm}} 仪表板中打开过的至少一个实例。您必须访问服务实例，才能为该实例创建凭证。凭证必须存在后，{{site.data.keyword.conversationshort}} 才能代表您与 {{site.data.keyword.discoveryshort}} 服务实例建立连接。如果您认为列表中缺少某个 {{site.data.keyword.discoveryshort}} 服务实例，请直接在 {{site.data.keyword.cloud}} 仪表板中打开该实例以生成其凭证。
    {: note}

1.  通过执行下列其中一个操作，指示要使用的数据集合：
{: #pick-data-collection}

    - 选择现有数据集合。

      在决定要使用的集合之前，可以单击*在 Discovery 中打开*链接以查看数据集合的配置。

      转至[配置搜索](#search-skill-add-configure)。

    - 如果您没有集合或不想使用列出的任何数据集合，请单击**创建新集合**以添加集合。遵循[创建数据集合](#search-skill-add-create-discovery-collection)中的过程。

      如果已达到基于 {{site.data.keyword.discoveryshort}} 服务套餐允许您创建的集合数的限制，那么不会显示**创建新集合**按钮。有关套餐限制详细信息，请参阅 [{{site.data.keyword.discoveryshort}} 价格套餐 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](/docs/services/discovery/discovery-about?topic=discovery-discovery-pricing-plans){: new_window}。
      {: note}

## 创建 Watson Discovery 服务实例
{: #skill-search-add-create-discovery}

1.  要创建 {{site.data.keyword.discoveryshort}} 服务实例，请单击**创建新集合**。

    如果您没有现有的 {{site.data.keyword.discoveryshort}} 服务实例，那么会为您创建 {{site.data.keyword.discoveryshort}} 服务的免费实例。

    服务的轻量套餐实例是在 {{site.data.keyword.Bluemix_notm}} 中供应的，不管您拥有的是哪种类型的 {{site.data.keyword.conversationshort}} 服务套餐。
    {: note}

1.  查看使用实例的条款和条件，然后单击**接受**以继续。

1.  [创建数据集合](#skill-search-add-create-discovery-collection)。

## 创建数据集合
{: #skill-search-add-create-discovery-collection}

如果您有 Discovery 服务轻量套餐，那么您有机会升级套餐。如果现在不想升级，请单击**开始使用**。

1.  要创建 {{site.data.keyword.discoveryshort}} 集合，请执行下列其中一个操作：

      - 要通过在 {{site.data.keyword.discoveryshort}} 为其提供内置支持的数据源类型中存储的数据来创建集合，请选取数据源类型。

        1.  提供所选数据源的必需信息，然后单击**连接**。

            有关更多详细信息，请参阅[连接到数据源 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](/docs/services/discovery?topic=discovery-sources){: new_window}。
        1.  指示希望数据源中的数据与要在 {{site.data.keyword.discoveryshort}} 中创建的集合进行同步的频率。
        1.  指定要从数据源中抽取并包含在 {{site.data.keyword.discoveryshort}} 集合中的信息。

            显示的选项因数据源类型而有所不同。

            - 对于 Salesforce 数据源，请选择要从源文档中抽取的对象类型。例如，可选择表示*案例*（即客户问题）的 [Case 对象类型 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://developer.salesforce.com/docs/atlas.en-us.object_reference.meta/object_reference/sforce_api_objects_case.htm#!)。
            - 对于 Sharepoint 数据源，请指定路径。
            - 对于文件存储库，请指定目录或文件。
            - 对于 Web 搜寻数据源，请指定要搜寻的 Web 站点的基本 URL。这将搜寻指定的 Web 页面及其链接到的任何页面，并且每个 Web 页面会创建一个文档。

            请给 Watson 几分钟时间来开始创建文档。开始摄入源后，{{site.data.keyword.discoveryshort}} 详细信息页面上显示的文档数会增加。您可能需要刷新该页面。
            
            要获取有关创建数据源的帮助，请参阅[故障诊断](#skill-search-add-troubleshoot)。

        1.  单击**保存并同步对象**。

            这将创建数据集合。此过程完成后，在单独的 Web 浏览器选项卡中托管的 {{site.data.keyword.discoveryshort}} 中将显示摘要页面。

      - 要通过上传文档来创建集合，请单击**上传文档**。

        1.  首先定义集合，然后上传文档。请提供以下信息：

            - 集合名称。名称对于此服务实例必须唯一。
            - 语言。选择要添加到此集合的文件的语言。有关 {{site.data.keyword.discoveryshort}} 支持的语言的信息，请参阅[语言支持 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](/docs/services/discovery?topic=discovery-language-support){: new_window}。

              如果要上传 PDF 文档并希望从中提取当事方、性质和类别信息，请展开**高级**部分，然后单击**将 Default Contract Configuration 用于此集合**。有关更多详细信息，请参阅[集合需求 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](/docs/services/discovery?topic=discovery-element-classification#element-collection){: new_window}。
        1.  上传文档。

            支持的文件类型包括 PDF、HTML、JSON 和 DOC 文件。有关更多详细信息，请参阅[添加内容 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](/docs/services/discovery?topic=discovery-addcontent){: new_window}。
            {: note}

            无法对上传的文档持续同步。如果要选取对文档进行的更改，请上传该文档的更高版本。

等待集合完全摄入后，再返回到 {{site.data.keyword.conversationshort}}。

### 数据集合创建示例
{: #skill-search-add-json-collection-example}

例如，您可能具有类似于以下内容的 JSON 文件：

```bash
{
  "Title": "About",
  "Shortdesc": "IBM Watson Assistant is a cognitive bot that you can customize for your business needs, and deploy across multiple channels to bring help to your customers where and when they need it.",
  "Topics": "overview",
  "url": "https://cloud.ibm.com/docs/services/assistant?topic=assistant-index"
}
```
{: codeblock}

如果上传的 JSON 文件含重复名称值，那么搜索仅对第一次出现的名称和值对建立索引并返回此对。请将该文件拆分成多个 JSON 文件，然后上传该集。
{: tip}

## 配置搜索
{: #skill-search-add-configure}

1.  在 {{site.data.keyword.discoveryshort}} 实例中，单击**在 Watson Assistant 中完成设置**。

1.  在 {{site.data.keyword.conversationshort}} 搜索技能页面中，单击**配置**。

1.  选择要从中抽取文本的 {{site.data.keyword.discoveryshort}} 集合字段，抽取的文本将包含在返回给用户的搜索结果中。

    可用的字段根据摄入的数据而有所不同。

    每个搜索结果可由以下各部分组成：

    - **标题**：搜索结果标题。使用集合中字段的标题、名称或类似类型的字段作为搜索结果标题。

      必须为标题选择某些内容，否则在 Facebook 和 Slack 集成中不会显示搜索结果响应。
    - **主体**：搜索结果描述。使用集合中的梗概、摘要或要点字段作为搜索结果主体。

       必须为主体选择某些内容，否则在 Facebook 和 Slack 集成中不会显示搜索结果响应。
    - **URL**：可以使用要包含在搜索结果末尾的任何页脚内容来填充此字段。

       例如，您可能希望包含指向其本机数据源中的原始数据对象的超文本链接。大多数联机数据源提供了用于存储中对象的自引用公共 URL，以支持直接访问。如果添加 URL，该 URL 必须有效且可访问。否则，Slack 集成不会在其响应中包含该 URL，并且 Facebook 集成不会返回任何响应。

       URL 字段为空时，Facebook 和 Slack 集成可以成功显示搜索结果响应。
  
    必须至少为其中一个搜索结果部分选择值。
    {: important}

    要获取帮助，请参阅[选择集合字段的技巧](#skill-search-add-field-tips)。

    如果下拉字段中没有可用的选项，请给 {{site.data.keyword.discoveryshort}} 更多时间来完成创建集合的操作。等待一段时间后，如果仍未创建集合，说明集合可能不包含任何文档，或者集合可能有需要首先解决的摄入错误。

    要继续使用[上传的 JSON 文件示例](#skill-search-add-json-collection-example)，映射最好使用*标题*、*简短描述*和 *URL* 字段。

    ![显示已选择“标题”、“简短描述”和 URL 字段，并且预览搜索卡已使用这些字段中的信息进行填充](images/search-skill-configure-fields.png)

    添加字段映射后，将显示搜索结果的预览，其中包含数据集合的相应字段中的信息。此预览显示返回给用户的搜索结果响应中包含的内容。

    要获取有关配置搜索的帮助，请参阅[故障诊断](#skill-search-add-troubleshoot)。

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
      <td>由于某种原因，无法完成搜索</td>
      <td>我可能有信息可以帮助处理您的查询，但目前我无法搜索知识库。</td>
    </tr>
    </table>

1.  单击**试用**，以打开“试用”窗格进行测试。输入测试消息以查看将配置选项应用于搜索时会返回的结果。根据需要进行调整。

1.  单击**创建**。

如果日后要更改搜索结果卡的配置，请再次打开搜索技能并进行编辑。您无需边更改边保存；更改会自动应用。对搜索结果感到满意后，单击**保存**以完成配置搜索技能的操作。

如果您决定要连接到其他 {{site.data.keyword.discoveryshort}} 服务实例或数据集合，请创建新的搜索技能，并将其配置为连接到其他实例。**无法**在创建搜索技能后为其更改服务实例或数据集合详细信息。
{: important}

### 选择集合字段的技巧
{: #skill-search-add-field-tips}

哪些集合字段适合用来抽取数据，取决于集合的数据源以及数据源的扩充方式。选择数据集合类型后，集合字段值会使用源字段预填充，这些源字段被视为最有可能包含给定集合数据源类型的有用信息。但是，您比任何人都更了解您的数据。因此，您可以将源字段更改为包含能满足您需求的最佳信息的字段。

要了解有关集合中的文档结构的更多信息（包括含有可能要抽取的信息的字段的名称），请在 {{site.data.keyword.discoveryshort}} 中打开该集合，然后单击“查看数据模式”图标 ![“查看数据模式”图标](images/icon-view-data-schema.png)。

源字段是在创建集合时创建的。要了解有关生成的字段（例如，`enriched_text.concepts.text`）的更多信息，请参阅[配置服务 > 添加扩充项 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](/docs/services/discovery?topic=discovery-configservice#adding-enrichments){: new_window}。

## 故障诊断
{: #skill-search-add-troubleshoot}

查看以下信息以获取有关执行常见任务的帮助。

- **创建 Web 搜寻数据集合**：创建 Web 搜寻数据源时要了解的事项：

    - 对于 {{site.data.keyword.discoveryshort}} 轻量套餐，创建的文档数不能超过 1,000 个。 
    - 要增加可用于数据集合的文档数，请单击“添加 URL 组”，在其中会列出要搜寻但并未从初始种子 URL 链接到的页面的 URL。
    - 要减少可用于数据集合的文档数，请指定基本 URL 的子域。或者，在 Web 搜寻设置中，限制 Watson 可以从原始页面跳转的次数。您还可以指定要从搜寻中显式排除的子域。
    - 如果等待了几分钟并刷新页面后仍未列出任何文档，请确保要摄入的内容可从 URL 的页面源中获取。某些 Web 页面内容是动态生成的，因此无法进行搜寻。

- **为上传的文档配置搜索结果**：如果使用的是已上传文档的集合，并且无法获取正确的搜索结果或结果不够简明，请考虑在创建数据集合时使用*智能文档理解*。 

  此功能支持根据文本格式设置对文档进行注释。例如，可以指导 {{site.data.keyword.discoveryshort}} 学会任何 28 磅粗体字体的文本都是文档标题。如果在摄入时将此信息应用于集合，那么日后可以使用 *title* 字段作为搜索结果的标题部分的源。 
  
  您还可以使用智能文档理解将大文档拆分成更易于搜索的分段。有关更多信息，请参阅 {{site.data.keyword.discoveryshort}} 文档中的[智能文档理解 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](/docs/services/discovery?topic=discovery-sdu) 主题。

- **改进搜索结果**：如果您对所看到的结果不满意，请查看以下信息以获取帮助。

  - 从对话节点调用搜索技能，并指定过滤器详细信息。 

    在对话节点搜索技能响应中，可以指定完整的 {{site.data.keyword.discoveryshort}} 查询语法过滤器以帮助缩小结果范围。 
    
    例如，可以定义过滤器，用于过滤掉数据集合中未在文档标题或其他某个元数据字段中提及意向的任何文档。或者，过滤器可以过滤掉未将某个实体识别为数据集合元数据中的已知实体的文档，或过滤掉未在文档全文中的任何位置提及该实体的文档。有关如何添加搜索技能响应类型的详细信息，请参阅[添加富文本响应](https://cloud.ibm.com/docs/services/assistant?topic=assistant-dialog-overview#dialog-overview-multimedia-add)。

    有关改进结果的更多技巧，请阅读 [Improve your natural language query results from Watson Discovery ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://developer.ibm.com/blogs/improving-your-natural-language-query-results-from-watson-discovery/) 博客帖子。

## 后续步骤
{: #skill-search-add-next-steps}

创建技能后，该技能即会在“技能”页面上显示为磁贴。

在将搜索技能添加到助手并部署助手之前，搜索技能无法与客户交互。请参阅[创建助手](/docs/services/assistant?topic=assistant-assistant-add)。

### 向助手添加技能
{: #skill-search-add-to-assistant}

您可以向一个助手添加一个技能。打开助手磁贴，并在其中向该助手添加技能。无法在技能配置页面中选择将使用该技能的助手。

一个搜索技能可以由多个助手使用。

1.  在“助手”选项卡中，单击以打开要向其添加技能的助手的磁贴。

1.  单击**添加搜索技能**。

1.  单击**添加现有技能**。

    在显示的可用技能中，单击要添加的技能。

向助手添加搜索技能后，将自动为该助手启用该技能，如下所示：

- 如果助手只有搜索技能，那么向助手的其中一个集成通道提交的任何用户输入都将触发该搜索技能。

- 如果助手既有对话技能也有搜索技能，那么任何用户输入都将首先触发对话技能。对话会处理自己对于可以正确进行回答有高置信度的任何用户输入。通常会触发对话树中 `anything_else` 节点的任何查询都将改为发送到搜索技能。

  通过遵循[禁用搜索](#search-skill-add-disable)中的步骤，可以阻止从 `anything_else` 节点触发搜索。
  {: note}

- 如果要对特定问题触发特定搜索查询，请将搜索技能响应类型添加到相应的对话节点。有关更多详细信息，请参阅[响应](/docs/services/assistant?topic=assistant-dialog-overview#dialog-overview-multimedia)。

## 搜索触发器
{: #skill-search-add-trigger}

搜索技能通过以下方式触发：

- **其他**节点：任何对话节点都无法处理用户的查询时，搜索外部数据源以查找相关回答。

  助手不会显示标准消息，例如`我不知道如何帮助您。`，而可能会显示`也许以下信息有所帮助：`，后跟搜索返回的一段文本。如果搜索技能链接到助手，那么无论何时触发 `anything_else` 节点，都会改为执行搜索，而不会显示该节点的响应。助手会将用户输入作为查询传递给搜索技能，并将搜索结果作为响应返回。

  通过遵循[禁用搜索](#search-skill-add-disable)中的步骤，可以阻止从 `anything_else` 节点触发搜索。
  {: note}

- **搜索响应类型**：如果向对话节点添加了搜索响应类型，那么助手将从外部数据源检索一段文本，并将其作为对特定问题的响应返回。仅当处理单个对话节点时，才会执行此类型的搜索。

  如果您希望在触发搜索之前缩小用户查询的范围，那么此方法很有用。例如，对话分支可能会收集有关客户想要购买的设备类型的信息。了解品牌和型号后，可以在提交到搜索技能的查询中发送型号关键字，以获得更好的结果。
- **仅搜索技能**：如果只有搜索技能链接到助手，而没有对话技能链接到该助手，那么从该助手的其中一个集成通道收到任何用户输入时，会向 {{site.data.keyword.discoveryshort}} 服务发送搜索查询。

## 测试搜索技能
{: #search-skill-add-test}

配置搜索后，可以使用搜索技能的“试用”窗格来发送测试查询，以查看从 {{site.data.keyword.discoveryshort}} 返回的搜索结果。

要测试客户提问并由对话回答或触发搜索时的完整体验，请使用通道集成，例如预览链接。

无法在对话的“试用”窗格中测试完整的端到端用户体验。搜索技能是单独配置并连接到助手的。对话技能无法了解搜索的详细信息，因此无法在其“试用”窗格中显示搜索结果。
{: important}

配置至少一个集成通道来测试搜索技能。在通道中，输入用于触发搜索的查询。如果从对话启动了任何类型的搜索，请测试对话以确保搜索按预期触发。如果未使用搜索响应类型，请测试是否仅当没有现有对话节点可以处理用户输入时，才会触发搜索。在任何时候触发搜索时，确保该搜索会返回有意义的结果。

## 向搜索技能发送更多请求
{: #search-skill-add-increase-flow}

如果希望降低对话技能的响应频率，而改为向搜索技能发送更多查询，那么可以配置对话来实现这一点。

您必须向助手添加了对话技能和搜索技能，此方法才有效。

遵循以下过程，通过将置信度级别阈值从缺省设置 0.2 重置为 0.5，以降低对话的响应可能性。将置信度级别阈值更改为 0.5，即指示助手不要使用对话中的回答进行响应，除非助手对于确信对话可以理解用户意向并可以进行处理的置信度超过 50%。

1.  在对话技能的*对话*页面中，确保对话树中的最后一个节点具有 `anything_else` 条件。

    每当处理此节点时，都会触发搜索技能。

1.  向对话添加一个文件夹。使该文件夹位于要取消强调的第一个对话节点的上方。向该文件夹添加以下条件：

    `intents[0].confidence > 0.5`

    此条件将应用于该文件夹中的所有节点。仅当助手对于确信自己理解用户意向的置信度至少为 50% 时，此条件才会指示助手处理该文件夹中的节点。

1.  将不希望助手经常处理的任何对话节点移入该文件夹中。

更改对话后，测试助手以确保搜索技能的触发频率与您希望的相符。

另一种方法是指导对话学习要忽略的主题。为此，可以将希望助手立即发送到搜索技能的话语添加为对话技能的“试用”窗格中的测试话语。然后，可以选择“试用”窗格中的**标记为不相关**选项，以指导对话学会不对此话语或类似的其他话语进行响应。有关更多信息，请参阅[指导助手学习要忽略的主题](/docs/services/assistant?topic=assistant-logs#logs-mark-irrelevant)。

## 禁用搜索
{: #search-skill-add-disable}

可以禁止触发搜索技能。

设置集成时，您可能希望暂时禁止触发搜索技能。或者，您可能希望仅针对可以在对话中识别到的特定用户查询触发搜索，并使用搜索技能响应类型进行回答。

要阻止触发搜索技能，请完成以下步骤：

1.  在**助手**页面中，单击助手的菜单，然后选择**设置**。
1.  打开*搜索技能*页面，然后单击以将相应切换控件切换为**已禁用**。
