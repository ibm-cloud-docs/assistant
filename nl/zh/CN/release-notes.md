---

copyright:
  years: 2015, 2019
lastupdated: "2019-02-28"

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

# 发行说明
{: #release-notes}

## 服务 API 版本控制
{: #release-notes-api-version}

API 请求需要采用 `version=YYYY-MM-DD` 格式的日期的 version 参数。每当我们以向后不兼容方式更改 API 时，都会发布次要版本的新 API。

version 参数随每个 API 请求一起发送。服务会将该 API 版本用于您指定的日期，或使用该日期之前的最新版本。不要缺省使用当前日期。请改为指定与应用程序兼容的版本匹配的日期，但在应用程序准备好用于更高版本之前，不要对其进行更改。

- V1 的当前版本为 `2019-02-28`。
- V2 的当前版本为 `2019-02-28`。
- {{site.data.keyword.conversationshort}} 工具中的“试用”窗格使用的是版本 `2018-07-10`。

## Beta 功能
{: #release-notes-beta}

IBM 发布分类为 Beta 的服务、功能和语言支持供您评估。这些功能可能不太稳定，可能经常会更改，还可能会临时通知中断使用。此外，Beta 功能可能无法提供与一般可用功能所提供级别相同的性能或兼容性，并且 Beta 功能不适用于生产环境。Beta 功能仅在 [IBM Developer Answers ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://developer.ibm.com/answers/topics/watson-assistant/){: new_window} 上受支持。

## 更新的模型
{: #release-notes-updated-models}

{{site.data.keyword.conversationshort}} 算法可能会根据反馈、科学增强功能和其他因素定期优化和更新，以便不断提高性能。以下发行说明中将阐述对模型的更新。

您已训练的现有模型不会立即受到影响，但在新模型可用 60 天之后，到期模型会更新到当前模型（如果尚未这样做）。

**注：**此更新声明仅适用于一般可用性 (GA) 语言和功能。

为服务提供了以下新功能和更改。请查看我们的[博客 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://medium.com/ibm-watson/assistant/home)，以深入了解有关最新功能如何使您的企业受益的信息。

## 2019 年 3 月 1 日
{: #1March2019}

- **简化了导航**：除去了包含单独的*构建*、*改进*和*部署*选项卡的侧边栏导航。现在，在主技能页面中就能获取构建对话技能所需的全部工具。

- **“改进”页面现在称为“分析”**：服务根据用户与助手之间的会话生成的参考度量值已从侧边栏的*改进*选项卡移至主技能页面上名为**分析**的新选项卡。

## 2019 年 2 月 28 日
{: #28February2019}

- **新 API 版本**：现在，当前 API 版本为 `2019-02-28`。此版本进行了以下更改：

    - 更改了在带槽的节点中对条件求值的顺序。先前，如果您有允许离题的带槽节点，那么要在触发了 `anything_else` 根节点后，才能对任何槽级别的“找不到”条件求值。通过更改操作顺序，解决了此行为的问题。现在，用户从带槽的节点离题时，将先处理除 `anything_else` 节点以外的其他所有根节点。接下来，对槽级别的“找不到”条件求值。最后，处理根级别的 `anything_else` 节点。要更好地了解带槽节点的完整操作顺序，请参阅[槽用法提示](/docs/services/assistant?topic=assistant-dialog-slots#dialog-slots-node-level-handler)。

    - 在消息的 `context` 或 `output` 对象中，以井号 (#) 开头的字符串不再视为意向引用。
  
      先前，这些字符串会自动视为意向。例如，如果指定了上下文变量，如 `"color":"#FFFFFF"`，那么十六进制颜色代码 (#FFFFFF) 将视为意向。服务会检查是否在用户的输入中检测到名为 #FFFFFF 的意向，如果没有检测到，那么会将 #FFFFFF 替换为 `false`。现在，不会再发生此替换。
  
      与此类似，如果在节点响应的文本字符串中包含井号 (#)，以前必须在其前面加反斜杠 (`\`) 以对其进行转义。例如，`我们是缅因州排名第一 (\#1) 的龙虾卷销售商。`现在，您不再需要对文本响应中的 `#` 号进行转义。

      此更改不适用于节点或条件响应条件。在条件中指定的以井号 (#) 开头的任何字符串都将继续视为意向引用。此外，可以使用 SpEL 表达式语法来强制系统将消息的 `context` 或 `output` 对象中的字符串视为意向。例如，将意向指定为 `<? #intent-name ?>`。

## 2019 年 2 月 25 日
{: #25February2019}

**Slack 集成增强功能**：现在，可以选择用于在 Slack 通道中触发助手的事件的类型。先前，将助手与 Slack 集成后，助手通过直接消息通道与用户进行交互。现在，可以将助手配置为侦听提及项，并在其他通道中被提及时进行响应。您可以选择使用一种或两种事件类型作为助理与用户进行交互的机制。

## 2019 年 2 月 11 日
{: #11February2019}

**与 Intercom 集成**：Intercom 是一个领先的客户服务消息传递平台，通过与 IBM 合作，向团队添加了新的座席，即虚拟 Watson Assistant。您可以将助手与 Intercom 应用程序相集成，以支持应用程序在助手和人工支持座席之间无缝地传递用户会话。此集成仅可供增强版和高端套餐用户使用。有关更多详细信息，请参阅[与 Intercom 集成](/docs/services/assistant?topic=assistant-deploy-intercom)。

## 2019 年 2 月 8 日
{: #8February2019}

- **控制技能版本**：现在，可以在开发过程中的关键点捕获技能的意向、实体、对话和配置设置的快照。通过版本，可以安全地发挥创意。您可以在测试环境中部署新的设计方法以验证这些方法，然后再将任何更新应用于助手的生产部署。有关更多详细信息，请参阅[创建技能版本](/docs/services/assistant?topic=assistant-versions)。

- **阿拉伯语内容目录**：现在，阿拉伯语技能的用户可以将预构建的意向添加到对话。有关更多信息，请参阅[使用内容目录](/docs/services/assistant?topic=assistant-catalog)。

## 2019 年 1 月 17 日
{: #17January2019}

- **捷克语语言支持一般可用**：对捷克语语言的支持不再分类为 Beta；现在此支持已一般可用。有关更多信息，请参阅[支持的语言](/docs/services/assistant?topic=assistant-language-support)。

- **语言支持改进**：更新了语言理解组件，以改进以下功能：

  - 德语和韩国语系统实体
  - 对阿拉伯语、荷兰语、法语、意大利语、日语、葡萄牙语和西班牙语进行意向分类记号化

## 2019 年 1 月 4 日
{: #4January2019}

- **华盛顿和伦敦位置的 IBM Cloud Functions**：现在，可以从在伦敦和华盛顿数据中心托管的服务实例中的助手对话，对 IBM Cloud Functions 发起程序化调用。请参阅[从对话节点发起程序化调用](/docs/services/assistant?topic=assistant-dialog-actions)。

- **使用数组的新方法**：可以使用以下 SpEL 表达式方法来更轻松地在对话中使用数组值：

  - **JSONArray.filter**：通过将数组中的每个值与可以根据用户输入变化的值进行比较来过滤数组。
  - **JSONArray.includesIntent**：检查 `intents` 数组是否包含特定意向。
  - **JSONArray.indexOf**：获取数组中特定值的下标号。
  - **JSONArray.joinToArray**：对从数组返回的值应用格式设置。

   有关更多详细信息，请参阅[数组方法文档](/docs/services/assistant?topic=assistant-dialog-methods#dialog-methods-arrays)。

## 2018 年 12 月 13 日
{: #13December2018}

- **伦敦数据中心**：现在，无需联合，就可以创建在伦敦数据中心托管的 {{site.data.keyword.conversationshort}} 服务实例。有关更多详细信息，请参阅[数据中心](/docs/services/assistant?topic=assistant-services-information#services-information-regions)。

- **对话节点限制更改**：对于新的标准套餐实例，对话节点限制曾临时从 100,000 个更改为 500 个。但后来此限制更改被撤销。如果在该限制生效的时间范围内创建了标准套餐实例，那么对话可能会受到影响。该限制对 2018 年 12 月 10 日至 12 月 12 日之间创建的技能有效。2019 年 1 月，将从所有受影响实例中除去该下限。如果您需要在此之前提升下限，请开具支持凭单。

## 2018 年 12 月 1 日
{: #1December2018}

   要确定对话技能中的对话节点数，请执行下列其中一个操作：

   - 在工具中，如果对话技能尚未与助手关联，请向助手添加对话技能，然后在该助手的主页中查看“技能”磁贴。*训练数据*部分会列出对话节点数。

   - 向 /dialog_nodes API 端点发送 GET 请求，并包含 `include_count=true` 参数。例如：

     ```curl
     curl -u "apikey:{apikey}" "https://gateway.watsonplatform.net/assistant/api/v1/workspaces/{workspace_id}/dialog_nodes?version=2018-09-20&include_count=true"
     ```

     在响应中，`pagination` 对象中的 `total` 属性包含对话节点数。

     有关如何编辑要继续使用的技能的信息，请参阅[有关技能导入问题的故障诊断](/docs/services/assistant?topic=assistant-skill-add#skill-add-import-errors)。

## 2018 年 11 月 27 日
{: #27November2018}

- **新的服务套餐（增强版套餐）可用**：新套餐以更低的价格点提供了高端级别的功能。与先前的套餐不同的是，增强版套餐是基于用户的收费套餐。它会根据在给定时间段内与助手进行交互的唯一用户数来度量使用情况。为了最充分地利用套餐，如果您构建自己的客户机应用程序，请将应用程序设计为能为每个用户定义唯一标识，并在每个 /message API 调用中传递用户标识。对于内置集成，将使用会话标识来标识用户与助手的交互。有关更多信息，请参阅[基于用户的套餐](/docs/services/assistant?topic=assistant-services-information#services-information-user-based-plans)。

  <table>
  <caption>增强版套餐限制</caption>
    <tr>
      <th>工件</th>
      <th>限制</th>
    </tr>
    <tr>
      <td>助手</td>
      <td>100 </td>
    </tr>
    <tr>
       <td>上下文实体</td>
       <td>20                            </td>
    </tr>
    <tr>
       <td>上下文实体注释</td>
       <td>2,000 </td>
    </tr>
    <tr>
       <td>对话节点</td>
       <td>100,000 </td>
    </tr>
    <tr>
       <td>实体</td>
       <td>1,000</td>
    </tr>
    <tr>
       <td>实体同义词</td>
       <td>100,000 </td>
    </tr>
    <tr>
       <td>实体值</td>
       <td>100,000 </td>
    </tr>
    <tr>
       <td>意向</td>
       <td>2,000 </td>
    </tr>
    <tr>
       <td>意向用户示例</td>
       <td>25,000 </td>
    </tr>
    <tr>
       <td>集成</td>
       <td>100 </td>
    </tr>
    <tr>
       <td>日志</td>
       <td>30 天</td>
    </tr>
    <tr>
       <td>技能</td>
       <td>50</td>
    </tr>
  </table>

- **基于用户的高端套餐**：现在，高端套餐基于活动的唯一用户数进行计费。如果选择使用此套餐，请将您构建的任何定制应用程序设计为能正确识别生成 /message API 调用的用户。有关更多信息，请参阅[基于用户的套餐](services-information#user-based-plans)。

  现有高端套餐服务实例不受此更改的影响；这些实例将继续使用基于 API 的计费方法。仅现有高端套餐用户会看到基于 API 的套餐作为*高端 (API)* 套餐选项列出。
  {: note}

  有关所有可用服务套餐的更多信息，请参阅 {{site.data.keyword.conversationshort}} [服务套餐选项 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://www.ibm.com/cloud/watson-assistant/pricing/){: new_window}。

- **意向用户示例建议 ![仅限增强版或高端套餐](images/premium.png)**：可以上传包含原始用户输入的文件（例如，呼叫中心日志的用户查询），服务可以分析和挖掘这些输入，以获取意向用户示例候选项。请参阅[通过日志文件添加示例](intent-recommendations#intent-recommendations-get-example-recommendations)。

## 2018 年 11 月 20 日
{: #20November2018}

**停止提供建议**：除去了“改进”选项卡上的“建议”部分。建议是仅可供高端套餐用户使用的 Beta 功能。它建议用户可执行哪些操作来改进其训练数据。现在，是在工具中实际更改训练数据的部分中提供建议，而不是将建议合并到一个位置。例如，在添加实体同义词时，现在可以选择查看服务建议的同义词列表。如果您要寻找其他方法来更详细地分析用户会话日志，请考虑使用 Jupyter 笔记本。有关更多详细信息，请参阅[高级任务](/docs/services/assistant?topic=assistant-logs-resources)。

## 2018 年 11 月 9 日
{: #9November2018}

- **主用户界面修订版**：{{site.data.keyword.conversationshort}} 服务采用了新的外观并新增了功能。

  此版本的工具在过去几个月内由 Beta 程序参与者进行了评估。

  - **技能**：您视为*工作空间*的内容现在称为*技能*。*对话技能*是用于自然语言处理训练数据和工件的容器，使助手能够了解用户问题，并对其进行响应。

    **我的工作空间在哪里？**您先前创建的任何工作空间现在都作为技能在服务实例中列出。单击**技能**选项卡可进行查看。有关更多信息，请参阅[技能](/docs/services/assistant?topic=assistant-skills)。

  - **助手**：现在，您只需执行两个步骤就能发布技能。第一步是将技能添加到助手，第二步是设置一个或多个用于部署技能的集成。助手在技能的基础上添加了一个功能层，使服务能够编排和管理信息流。请参阅[助手](/docs/services/assistant?topic=assistant-assistants)。

  - **内置集成**：您不用转至**部署**选项卡来部署工作空间，只要将对话技能添加到助手，然后再将集成添加到使技能可供用户使用所经由的助手即可。您无需构建定制前端应用程序，也无需管理从一个调用到下一个调用的会话状态。但是，如果您愿意，也仍然可以执行这些操作。有关更多信息，请参阅[添加集成](/docs/services/assistant?topic=assistant-deploy-integration-add)。

  - **新的主 API 版本**：API V2 版本可用。通过此版本，可以访问可用于在运行时与助手进行交互的方法。不用在每个 API 调用中传递上下文；会话状态会作为助手层的一部分代您进行管理。
  
    在工具中显示为对话技能的内容实际上是 V1 工作空间的包装器。目前，没有用于使用 V2 API 编写技能和助手的 API 方法。不过，您可以继续使用 V1 API 来编写工作空间。有关更多详细信息，请参阅 [API 概述](/docs/services/assistant?topic=assistant-api-overview)。
    {: note}

  - **切换数据源**：现在，可以更轻松地使用来自不同技能的用户会话日志来改进一个技能中的模型。您无需依赖部署标识，而只需选取添加并部署了技能的助手的名称即可使用其数据。请参阅[跨助手进行改进](/docs/services/assistant?topic=assistant-logs#logs-deploy-id)。

  以下视频提供了更新后的 {{site.data.keyword.conversationshort}} 工具为时 2 分钟的概述。

  <iframe class="embed-responsive-item" id="youtubeplayer" title="产品概述" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/OkW7gnHrw30?rel=0" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

  - **伦敦实例的预览链接**：如果服务实例是在伦敦托管的，那么必须编辑预览链接 URL。该 URL 包含托管实例的区域的区域代码。由于伦敦的实例已联合到达拉斯，因此必须将该 URL 中的 `eu-gb` 引用替换为 `us-south`，以便预览 Web 页面正确呈现。

## 2018 年 11 月 8 日
{: #8November2018}

- **日本数据中心**：现在，可以创建在东京数据中心托管的 {{site.data.keyword.conversationshort}} 服务实例。有关更多详细信息，请参阅[数据中心](/docs/services/assistant?topic=assistant-services-information#services-information-regions)。

## 2018 年 10 月 30 日
{: #30October2018}

- **新的 API 认证流程**：在以下区域中，{{site.data.keyword.conversationshort}} 服务已从使用 Cloud Foundry 转换为使用基于令牌的 Identity and Access Management (IAM) 认证：

  - 达拉斯 (us-south)
  - 法兰克福 (eu-de)

  对于新的服务实例，将使用 IAM 进行认证。您可以传递不记名令牌或 API 密钥。令牌支持已认证的请求，而无需在每次调用中都嵌入服务凭证。API 密钥使用基本认证。

  对于所有现有服务实例，您可继续使用服务凭证 (`{username}:{password}`) 进行认证。

  有关更多信息，请参阅[对 API 调用进行认证](/docs/services/assistant?topic=assistant-services-information#services-information-authenticate-api-calls)。

## 2018 年 10 月 25 日
{: #25October2018}

- **实体同义词建议以更多语言提供**：为法语、日语和西班牙语语言添加了同义词建议支持。

## 2018 年 9 月 26 日
{: #26September2018}

- **{{site.data.keyword.conversationfull}} 在 {{site.data.keyword.icpfull}}** 中可用：有关更多信息，请参阅 [{{site.data.keyword.icpfull}} 文档 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_2.1.0.3/featured_applications/watson_assistant.html)。

## 2018 年 9 月 21 日
{: #21September2018}

- **新 API 版本**：现在，当前 API 版本为 `2018-09-20`。在此版本中，API 返回的错误对象的 `errors[].path` 属性表示为 [JSON 指针 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://tools.ietf.org/html/rfc6901)，而不是采用点表示法形式。
- **Web 操作支持**：现在，可以从对话节点调用 {{site.data.keyword.openwhisk_short}} Web 操作。有关更多详细信息，请参阅[从对话节点发起程序化调用](/docs/services/assistant?topic=assistant-dialog-actions)。

## 2018 年 8 月 15 日
{: #15August2018}

- **实体模糊匹配支持改进**：英语实体完全支持模糊匹配，并且对于其他许多语言，拼写错误检查功能不再是仅 Beta 功能。有关详细信息，请参阅[支持的语言](/docs/services/assistant?topic=assistant-language-support)。

## 2018 年 8 月 6 日
{: #6August2018}

- **意向冲突解决 ![仅限增强版或高端套餐](images/premium.png)**：现在，不同意向中的两个或多个用户示例彼此类似时，工具可以帮助您解决冲突。类似的用户示例可能会削弱训练数据，并使服务在运行时更难将用户输入映射到相应的意向。有关详细信息，请参阅[解决意向冲突](/docs/services/assistant?topic=assistant-intents#intents-resolve-conflicts)。

- **消歧** ![仅限增强版或高端套餐](images/premium.png)：启用消歧，以允许助手在需要从两个或多个可行的对话节点中做出选择以用于处理响应时，向用户请求帮助。有关更多详细信息，请参阅[消歧](/docs/services/assistant?topic=assistant-dialog-runtime#dialog-runtime-disambiguation)。

- **“跳转至”修订**：修复了“对话”工具中的错误，此错误使您无法配置以具有 `anything_else` 特殊条件的节点的响应为目标的“跳转至”。

- **离题返回消息**：现在，可以指定用户在离题后返回到节点时要显示的文本。届时用户已看到该节点的提示。您可以对消息略作更改，以便让用户知道他们将返回到上次离开的位置。例如，指定类似下面的响应：`Where were we? Oh, yes...` 有关更多详细信息，请参阅[离题](dialog-runtime#digressions)。

## 2018 年 7 月 12 日
{: #12July2018}

- **富文本响应类型**：现在，除了文本外，还可以向对话添加包含图像或按钮等元素的富文本响应。有关更多信息，请参阅[富文本响应](/docs/services/assistant?topic=assistant-dialog-overview#dialog-overview-multimedia)。

- **上下文实体 (Beta)**：上下文实体是通过标注在意向用户示例中出现的实体类型的提及项而定义的实体。这些实体类型不仅会指导服务学习相关项，还会指导服务学习在用户发声中通常显示相关项的上下文，使服务能够仅基于在用户输入中引用实体提及项的方式，就识别出从未见过的实体提及项。例如，如果通过将“Boston”标记为 @destination 实体来注释意向用户示例“I want a flight to Boston”，那么服务就可以将用户输入“I want a flight to Chicago.”中的“Chicago”识别为 @destination。此功能目前仅可用于英语。有关更多信息，请参阅[添加上下文实体](/docs/services/assistant?topic=assistant-entities#entities-create-annotation-based)。

  使用 Internet Explorer Web 浏览器访问工具时，无法对意向用户示例中的实体提及项进行标注，也无法编辑用户示例文本。
  {: note}

- **实体建议**：现在，服务可以为实体值建议同义词。建议者基于从大量现有信息（包括大量书面文本来源）中抽取的上下文相似度来查找相关同义词，然后使用自然语言处理技术来识别与实体值中现有同义词类似的词。有关更多信息，请参阅[同义词](/docs/services/assistant?topic=assistant-entities#entities-synonyms)。

- **新 API 版本**：现在，当前 API 版本为 `2018-07-10`。此版本引入了以下更改：

  - /message `output` 对象的内容已从 `text` JSON 对象更改为支持多种富文本响应类型（包括 `image`、`option`、`pause` 和 `text`）的 `generic` 数组。
  - 添加了对上下文实体的支持。

- **概述页面的日期过滤器**：使用新的日期过滤器来选择要显示其中数据的时间段。这些过滤器会影响页面上显示的所有数据：不仅仅是图形中显示的会话数，还会影响与图形一起显示的统计信息以及最热门意向和实体的列表。有关更多信息，请参阅[控件](logs-overview#controls)。

- **扩展了模式限制**：使用**模式**字段[为实体值定义特定模式](/docs/services/assistant?topic=assistant-entities#entities-patterns)时，现在模式（正则表达式）限制为 512 个字符。

## 2018 年 7 月 2 日
{: #2July2018}

- **从条件响应跳转至**：现在，可以将条件响应配置为直接跳转至另一个节点。有关更多详细信息，请参阅[条件响应](/docs/services/assistant?topic=assistant-dialog-overview#dialog-overview-multiple)。

## 2018 年 6 月 21 日
{: #21June2018}

- **系统实体的语言更新**：现在，荷兰语和简体中文语言支持已一般可用。荷兰语语言支持包括用于拼写错误检查的模糊匹配。繁体中文语言支持在 Beta 发行版中提供了[系统实体](/docs/services/assistant?topic=assistant-system-entities)。有关详细信息，请参阅[支持的语言](/docs/services/assistant?topic=assistant-language-support)。

## 2018 年 6 月 14 日
{: #14June2018}

- **华盛顿数据中心正式运行**：现在，可以创建在华盛顿数据中心托管的 {{site.data.keyword.conversationshort}} 服务实例。有关更多详细信息，请参阅[数据中心](/docs/services/assistant?topic=assistant-services-information#services-information-regions)。

- **新的 API 认证流程**：对于在以下区域中托管的服务实例，{{site.data.keyword.conversationshort}} 服务采用了新的 API 认证流程：

  - 华盛顿 (us-east)，自 2018 年 6 月 14 日起
  - 澳大利亚悉尼 (au-syd)，自 2018 年 5 月 7 日起

  {{site.data.keyword.cloud_notm}} 将迁移到基于令牌的 Identity and Access Management (IAM) 认证。

  对于上面所列区域中的新服务实例，将使用 IAM 进行认证。您可以传递不记名令牌或 API 密钥。令牌支持已认证的请求，而无需在每次调用中都嵌入服务凭证。API 密钥使用基本认证。

  对于其他区域中的所有新的和现有服务实例，您都继续使用服务凭证 (`{username}:{password}`) 进行认证。

  使用任何 Watson SDK 时，可以传递 API 密钥，并让 SDK 来管理令牌的生命周期。有关更多信息和示例，请参阅 API 参考中的[认证 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")]https://{DomainName}/apidocs/assistant-v2#authentication){: new_window}。

  如果不确定要使用哪种类型的认证，请通过单击 [{{site.data.keyword.Bluemix_notm}} 资源列表 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://cloud.ibm.com/resources){: new_window} 上的服务实例来查看服务凭证。

## 2018 年 5 月 25 日
{: #25May2018}

- **新的样本工作空间**：更改了供您探索或用作您自己工作空间的起点的样本工作空间。**汽车仪表板**样本已替换为**客户服务**样本。新的样本说明了如何使用内容目录意向和其他更新的功能来构建机器人。它可以回答常见问题，例如，有关门店营业时间和所在位置的查询，并说明了如何使用带槽的节点来安排店内预约。

- **向“试用”添加了 HTML 呈现**：现在，“试用”窗格会呈现响应文本中包含的 HTML 格式。先前，如果在文本响应中将超文本链接包含为 HTML 锚点标记，那么在测试期间，在“试用”窗格中会看到 HTML 源代码。之前类似于以下内容：

  `Contact us at <a href="https://www.ibm.com">ibm.com</a>.`

  现在，超文本链接会像在 Web 页面上一样呈现。现在显示为类似于以下内容：

  `Contact us at` [ibm.com](https://www.ibm.com){: new_window}.

    请记住，针对要将会话部署到的客户机应用程序，您必须在响应中使用相应的语法类型。仅当客户机应用程序可以正确解释 HTML 语法时，才可使用 HTML 语法。其他集成通道可能需要其他格式。

- **部署更改**：除去了**在 Slack 中测试**选项。

## 2018 年 5 月 11 日
{: #11May2018}

- **信息安全**：本文档包含有关数据隐私的一些新的详细信息。请阅读[信息安全](/docs/services/assistant?topic=assistant-information-security)以了解更多信息。

## 2018 年 5 月 7 日
{: #7May2018}

- **澳大利亚悉尼数据中心正式运行**：现在，可以创建在澳大利亚悉尼数据中心托管的 {{site.data.keyword.conversationshort}} 服务实例。有关更多详细信息，请参阅 [IBM Cloud 全球数据中心 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标"](https://www.ibm.com/cloud/data-centers/)。

## 2018 年 4 月 4 日
{: #4April2018}

- **搜索对话**：现在，可以[搜索对话节点](/docs/services/assistant?topic=assistant-dialog-build#dialog-build-search)，以查找给定的词或短语。

## 2018 年 3 月 15 日
{: #15March2018}

- **引入 {{site.data.keyword.conversationfull}}**：{{site.data.keyword.ibmwatson}} Conversation 已重命名。现在，它名为 {{site.data.keyword.conversationfull}}。该名称更改反映了服务在不断扩展，以提供预构建的内容和工具，帮助您更轻松地共享构建的虚拟助手。有关更多详细信息，请参阅[此博客帖子 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://www.ibm.com/blogs/watson/2018/03/the-future-of-watson-conversation-watson-assistant/)。

- **新的 REST API 和 SDK 可用于 Watson Assistant**：新的 API 在功能上与现有会话 API 完全相同，这些 API 将继续受支持。有关 Watson Assistant API 的更多信息，请参阅 [API 参考 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://{DomainName}/apidocs/assistant){: new_window}。

- **对话增强功能**：向对话工具添加了以下功能：

  - 现在，提供了简单变量名称和值字段，可用于添加上下文变量或更新上下文变量值。您无需打开 JSON 编辑器，除非您希望这样做。有关更多详细信息，请参阅[定义上下文变量](/docs/services/assistant?topic=assistant-dialog-runtime#dialog-runtime-context-var-define)。
  - 通过使用文件夹将相关对话节点分组在一起来组织对话。有关更多详细信息，请参阅[使用文件夹组织对话](dialog-build#folders)。
  - 添加了对以下功能的支持：定制每个对话节点如何参与用户启动的从指定对话流执行的离题。有关更多详细信息，请参阅[离题](dialog-runtime#digressions)。

- **搜索意向和实体**：添加了新的搜索功能，支持[搜索意向](intents#searching-intents)以查找用户示例、意向名称或描述，或者[搜索实体](/docs/services/assistant?topic=assistant-entities#entities-search)值和同义词。

- **内容目录**：新的[内容目录](/docs/services/assistant?topic=assistant-catalog#catalog-add)包含可以添加到应用程序的单个类别的预构建通用意向和实体。例如，大多数应用程序需要一个常规 #greeting-type 意向，用于启动与用户的对话。您可以从内容目录进行添加，而不用构建自己的意向。

- **增强了用户度量值**：通过其他用户度量值和日志记录统计信息增强了“改进”组件的功能。例如，“概述”页面包括多个新的详细图形，概述了用户与应用程序之间的交互、给定时间段内的流量，以及在用户会话中最常识别到的意向和实体。

## 2018 年 3 月 12 日
{: #12March2018}

- **新的日期和时间方法**：添加了方法，能更轻松地从对话执行日期计算。有关更多详细信息，请参阅[日期和时间计算](/docs/services/assistant?topic=assistant-dialog-methods#dialog-methods-date-and-time-calculations)。

## 2018 年 2 月 16 日
{: #16February2018}

- **对话节点跟踪**：使用“试用”窗格测试对话时，每个响应旁边会显示一个“位置”图标。可以单击该图标来突出显示服务在对话树中遍历至该响应的路径。有关详细信息，请参阅[构建对话](/docs/services/assistant?topic=assistant-dialog-build#dialog-build-test)。

- **新 API 版本**：现在，当前 API 版本为 `2018-02-16`。此版本引入了以下更改：

  - 现在，大多数 GET 请求上都支持新的 `include_audit` 参数。这是可选的布尔参数，用于指定响应是否应该包含审计属性（`created` 和 `updated` 时间戳记）。缺省值为 `false`。（如果使用的 API 版本早于 `2018-02-16`，那么缺省值为 `true`。）有关更多信息，请参阅 [API 参考 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://{DomainName}/apidocs/assistant){: new_window}。

  - 来自使用新版本的 API 调用的响应仅包含具有非 `null` 值的属性。

  - 现在，消息响应的 `output.nodes_visited` 和 `output.nodes_visited_details` 属性包含具有以下类型的节点，这些类型先前是省略的：

    - `type`=`response_condition` 的节点
    - `type`=`event_handler` 并且 `event_name`=`input` 的节点

## 2018 年 2 月 9 日
{: #9February2018}

- **荷兰语系统实体 (Beta)**：增强了荷兰语支持，以在 Beta 发行版中包含[系统实体](/docs/services/assistant?topic=assistant-system-entities)的可用性。有关详细信息，请参阅[支持的语言](/docs/services/assistant?topic=assistant-language-support)。

## 2018 年 1 月 29 日
{: #29January2018}

- 现在，{{site.data.keyword.conversationshort}} REST API 支持新的请求参数：
  - 更新工作空间时使用 `append` 参数来指示是否应该将新工作空间数据添加到现有数据，而不是替换现有数据。有关更多信息，请参阅[更新工作空间 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://cloud.ibm.com/apidocs/assistant?curl=#update-workspace){: new_window}。
  - 发送消息时使用 `nodes_visited_details` 参数来指示响应是否应该包含有关在处理消息期间访问的节点的其他诊断信息。有关更多信息，请参阅[发送消息 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://cloud.ibm.com/apidocs/assistant?curl=#get-response-to-user-input){: new_window}。

## 2018 年 1 月 23 日
{: #23January2018}

- **无法检索工作空间列表**：如果在工具中工作时看到此错误消息或类似错误消息，可能意味着您的会话已到期。通过从**用户信息**图标 ![“用户信息”图标菜单](images/user-icon.png) 中选择**注销**以注销，然后重新登录。

## 2017 年 12 月 8 日
{: #8December2017}

- **跨实例日志数据访问（仅限高级用户）**：如果您是 {{site.data.keyword.conversationshort}} 高级用户，那么可以选择将高级实例配置为允许访问跨不同高级实例的工作空间中的日志数据。

- **复制节点**：现在，可以复制节点以生成该节点及其子代的副本。如果使用有用的逻辑构建了节点，并且希望在对话中的其他位置复用该逻辑，那么此功能非常有用。有关更多信息，请参阅[复制对话节点](dialog-build#copy-node)。

- **捕获模式实体中的组**：您可以标识为实体定义的正则表达式模式中的组。如果希望日后能够引用模式的子部分，那么标识组非常有用。例如，实体可能有一个用于捕获美国电话号码的正则表达式模式。如果将号码模式的区号分段标识为一个组，那么后续可以引用该组来访问电话号码的该区号分段。有关更多信息，请参阅[定义实体](/docs/services/assistant?topic=assistant-entities#entities-creating-task)。

## 2017 年 12 月 6 日
{: #6December2017}

- **{{site.data.keyword.openwhisk}} 集成 (Beta)**：直接从对话节点调用 {{site.data.keyword.openwhisk}}（以前称为 IBM OpenWhisk）操作。例如，此功能支持调用操作以在对话节点中检索天气信息，然后以对话响应中返回的信息为条件。目前，可以从美国南部区域中托管的 {{site.data.keyword.openwhisk_short}} 实例调用操作。有关更多详细信息，请参阅[从对话节点发起程序化调用](/doc/services/assistant?topic=assistant-dialog-actions)。

## 2017 年 12 月 5 日
{: #5December2017}

- **重新设计了用于意向和实体的 UI**：`意向`和`实体`选项卡已经过重新设计，可在创建和编辑实体和意向时，提供更轻松、更高效的工作流程。有关使用这两个选项卡的信息，请参阅[定义意向](intents#creating-intents)和[定义实体](/docs/services/assistant?topic=assistant-entities#entities-creating-task)。

## 2017 年 11 月 30 日
{: #30November2017}

- **支持东阿拉伯数字**：现在，阿拉伯语系统实体中支持东阿拉伯数字。

## 2017 年 11 月 29 日
{: #29November2017}

- **提高了对跨工作空间的用户输入的理解**：现在，可以使用发送到实例中的其他工作空间的发声来改进工作空间。例如，您可能有多个版本的生产工作空间和开发工作空间；可以使用相同的发声数据来改进其中任一工作空间。请参阅[跨工作空间进行改进](/docs/services/assistant?topic=assistant-logs#logs-deploy-id)。

## 2017 年 11 月 20 日
{: #20November2017}

- **GB18030 合规性**：GB18030 是一项中国标准，规定了用于中国市场的扩展代码页。此代码页标准对于软件行业来说非常重要，因为中国国家信息技术标准化技术委员会要求，在 2001 年 9 月 1 日之后向中国市场发布的任何软件应用程序都应支持 GB18030。{{site.data.keyword.conversationshort}} 服务支持此编码，并且已通过 GB18030 合规性认证。

## 2017 年 11 月 9 日
{: #9November2017}

- **意向示例可以直接引用实体**：现在，可以直接在意向示例中指定实体引用。{{site.data.keyword.conversationshort}} 服务分类器使用实体引用及其所有值或同义词来训练意向。有关更多信息，请参阅[意向](/docs/services/assistant?topic=assistant-intents)主题中的[*实体作为示例*](/docs/services/assistant?topic=assistant-intents#intents-entity-as-example)。

  目前，您只能直接引用定义的封闭实体。不能直接引用[模式实体](/docs/services/assistant?topic=assistant-entities#entities-pattern)或[系统实体](/docs/services/assistant?topic=assistant-system-entities)。
  {: note}

## 2017 年 11 月 8 日
{: #8November2017}

- **{{site.data.keyword.conversationshort}} 连接器**：可以使用新的 {{site.data.keyword.conversationshort}} 连接器工具将工作空间连接到您拥有的 Slack 或 Facebook Messenger 应用程序，使其成为 Slack 或 Facebook Messenger 用户可以与其交互的聊天机器人。此工具仅可用于 {{site.data.keyword.Bluemix_notm}} 美国南部区域。

## 2017 年 11 月 3 日
{: #3November2017}

- **对话更新**：通过以下更新，您可以更轻松地构建对话。（有关详细信息，请参阅[构建对话](/docs/services/assistant?topic=assistant-dialog-build)。）

    - 可以向槽添加条件，使该槽仅在满足特定条件时才为必需。例如，仅当要求提供婚姻状况的先前（必需）槽指示用户已婚时，才能使要求提供配偶姓名的槽为必需。

    - 现在，可以选择**跳过用户输入**作为节点的下一步。选择此选项时，在处理当前节点之后，服务会直接跳转至当前节点的第一个子节点。此选项与现有的*跳转至*下一步选项类似，不同之处在于前者的灵活性更高。您无需指定要跳转至的确切节点。在运行时，即便在定义下一步行为之后对子节点重新排序或添加了新节点，服务也始终会跳转至作为第一个子节点的任何节点。

    - 可以为槽添加条件响应。对于“已找到”和“找不到”响应，都可以定制服务根据是否满足特定条件的响应方式。此功能支持在将用户提供的值保存在槽的上下文变量中之前，检查是否存在可能的错误解释并进行更正。例如，如果槽保存了用户的年龄，并使用*检查对象*字段中的 `@sys-number` 来捕获此信息，那么可以添加条件来检查是否有超过 100 的数字，然后用类似*请提供有效年龄（岁）*的内容予以响应。有关更多详细信息，请参阅[向“已找到”和“找不到”响应添加条件](/docs/services/assistant?topic=assistant-dialog-slots#dialog-slots-handler-next-steps)。

    - 用于向节点添加条件响应的界面已经过重新设计，能更轻松地列出每个条件及其响应。要添加节点级别的条件响应，请单击**定制**，然后启用**多个响应**选项。

     **多个响应**切换控件仅针对节点级别的响应将此功能设置为开启或关闭。它无法控制为槽定义条件响应的能力。槽的多个响应设置是单独控制的。
     {: note}

    - 为了使用于编辑槽的页面保持简洁，现在请选择菜单选项以 a.) 添加必须满足后才能处理槽的条件，以及 b.) 为槽的“已找到”和“找不到”条件添加条件响应。除非选择添加此额外功能，否则不会显示槽条件和“多个响应”字段，这会使页面得到简化，使用起来更轻松。

## 2017 年 10 月 25 日
{: #25October2017}

- **更新了简体中文**：增强了对简体中文的语言支持。这包括使用字符级文字嵌入来改进意向分类以及系统实体的可用性。请注意，{{site.data.keyword.conversationshort}} 服务学习模型可能已在此增强功能中进行更新；因此重新训练模型时，将应用所有更改；有关更多信息，请参阅[更新的模型](#release-notes-updated-models)。
- **更新了西班牙语** - 针对超大数据集，改进了西班牙语意向分类。

## 2017 年 10 月 11 日
{: #11October2017}

- **更新了韩国语**：增强了对韩国语的语言支持。请注意，{{site.data.keyword.conversationshort}} 服务学习模型可能已在此增强功能中进行更新；因此重新训练模型时，将应用所有更改；有关更多信息，请参阅[更新的模型](#release-notes-updated-models)。

## 2017 年 10 月 3 日
{: #3October2017}

- **模式定义的实体 (Beta)**：现在，可以使用正则表达式来定义实体的特定模式。这可以帮助您识别符合所定义模式的实体，例如 SKU 或部件号、电话号码或电子邮件地址。有关其他详细信息，请参阅[模式定义的实体](/docs/services/assistant?topic=assistant-entities#entities-pattern)。
    - 可以为单个实体值添加同义词或模式；但不能同时添加这两者。
    - 对于每个实体值，最多可以有 5 个模式。
    - 每个模式（正则表达式）限制为 128 个字符。
    - 目前，通过 CSV 文件导入或导出操作不支持模式。
    - REST API 不支持直接访问模式，但可以使用 `/values` 端点来检索或修改模式。

- **通过字典过滤模糊匹配（仅限英语）**：改进后的实体模糊匹配版本现已可用，适用于英语环境。此改进会阻止捕获某些常用、有效的英语单词作为给定实体的模糊匹配。例如，模糊匹配不会将实体值 `like` 与 `hike` 或 `bike` 匹配（因为 like 是有效的英语单词），而是会继续匹配诸如 `lkie` 或 `oike` 这样的实体。

## 2017 年 9 月 27 日
{: #27September2017}

- **更新了条件构建器**：更新了显示用于帮助定义对话节点中条件的控件。增强功能包括支持在输入 $ 以开始添加上下文变量后，列出可用的上下文变量名称。

## 2017 年 8 月 31 日
{: #31August2017}

- **改进了部分回滚**：在“改进”部分的“概述”页面中，临时除去了中值会话时间度量值和对应的过滤器。除去这些功能后，就可以避免计算某些度量值，从而避免生成中值会话时间度量值以及随时间变化的会话图，避免由此导致显示不准确的信息。虽然从工具中除去功能让 IBM 感到遗憾，但目的是为了致力于确保向用户传递准确的信息。
- **对话节点名**：现在，可以为对话节点指定任何名称；名称不必唯一。而且随后可以更改节点名，而不影响节点在内部的引用方式。指定的名称会保存为工作空间 JSON 文件中该节点的 title 属性，而系统是使用 name 属性中存储的唯一标识来引用该节点。

## 2017 年 8 月 23 日
{: #23August2017}

- **更新了韩国语、日语和意大利语**：增强了对韩国语、日语和意大利语的语言支持。请注意，{{site.data.keyword.conversationshort}} 服务学习模型可能已在此增强功能中进行更新；因此重新训练模型时，将应用所有更改；有关更多信息，请参阅[更新的模型](#release-notes-updated-models)。

## 2017 年 8 月 10 日
{: #10August2017}

- **重音符规范化**：在会话式设置中，用户在与 {{site.data.keyword.conversationshort}} 服务进行交互时，有可能会使用重音符。因此，更新了算法，现在对于意向检测和实体识别，对单词的有重音和无重音版本进行相同的处理。

  但是，对于某些语言（例如，西班牙语），一些重音符可以改变实体的含义。因此，对于实体检测，尽管原始实体可能隐式具有重音符，但服务还是可以与同一实体的无重音版本相匹配，但置信度分数略低。

  例如，对于单词 `barrió`（该词具有重音符，并且对应于动词 `barrer`（打扫）的过去时），服务还可能匹配单词 `barrio`（邻居），但置信度略低。

  系统将在具有完全匹配项的实体中提供最高置信度分数。例如，如果训练集内有 `barrió`，那么不会检测到 `barrio`；如果训练集内有 `barrio`，那么不会检测到 `barrió`。

  您应该使用正确的字符和重音符来训练系统。例如，如果期望 `barrió` 作为响应，那么应该将 `barrió` 放入训练集。

  同样的规则也适用于带有重音符之外的标记的单词，例如西班牙语字母 `ñ` 和字母 `n`（如 `uña` 与 `una`）。在此例中，字母 `ñ` 并非只是带有重音符的 `n`；它是一个特定于西班牙语的独特字母。

  如果您认为客户不会使用正确的重音符或者单词会拼写错误（例如，包括输入 `n` 而不是 `ñ`），那么可以启用模糊匹配，或者在训练示例中显式包含这些情况。

  **注：**葡萄牙语、西班牙语、法语和捷克语支持重音符规范化。

- **工作空间选择退出标志**：现在，{{site.data.keyword.conversationshort}} REST API 支持工作空间的选择退出标志。此标志指示 IBM 不会使用意向和实体等工作空间训练数据进行常规服务改进。有关更多信息，请参阅 [API 参考 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://cloud.ibm.com/apidocs/assistant?curl=#data-collection){: new_window}

## 2017 年 8 月 7 日
{: #7August2017}

- **`Next` 和 `last` 日期解释**：{{site.data.keyword.conversationshort}} 服务将 `last` 和 `next` 日期视为是指所引用的最近的上一天或下一天（可能是同一周，也可能是前一周）。有关其他信息，请参阅[系统实体](/docs/services/assistant?topic=assistant-system-entities#system-entities-sys-datetime)主题。

## 2017 年 8 月 3 日
{: #3August2017}

- **其他语言的模糊匹配 (Beta)**：现在，实体模糊匹配可用于其他语言，如[支持的语言](/docs/services/assistant?topic=assistant-language-support)主题中所注明。
- **部分匹配（Beta - 仅限英语）**：现在，模糊匹配将自动建议用户定义的实体中存在基于子字符串的同义词，并为其分配低于完全实体匹配的置信度分数。有关详细信息，请参阅[模糊匹配](/docs/services/assistant?topic=assistant-entities#entities-fuzzy-matching)。

## 2017 年 7 月 28 日
{: #28July2017}

- 现在，为工具设置双向首选项时，可以指定图形用户界面方向。
- 工具的颜色方案已更新为与其他 Watson 服务和产品保持一致。

## 2017 年 7 月 19 日
{: #19July2017}

- {{site.data.keyword.conversationshort}} REST API 现在支持访问对话节点。有关更多信息，请参阅 [API 参考 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://cloud.ibm.com/apidocs/assistant?curl=#list-dialog-nodes){: new_window}。

## 2017 年 7 月 14 日
{: #14July2017}

- 增强了对话的槽功能。例如，添加了 *slot_in_focus* 属性，可以用于定义仅应用于单个槽的条件。有关详细信息，请参阅[使用槽收集信息](/docs/services/assistant?topic=assistant-dialog-slots)。

## 2017 年 7 月 12 日
{: #12July2017}

- **支持捷克语**：引入了捷克语语言支持；有关其他详细信息，请参阅[支持的语言](/docs/services/assistant?topic=assistant-language-support)主题。

## 2017 年 7 月 11 日
{: #11July2017}

- **在 Slack 中测试**：可以使用新的**在 Slack 中测试**工具，将工作空间快速部署为 Slack 机器人用户以进行测试。此工具仅可用于 {{site.data.keyword.Bluemix_notm}} 美国南部区域。
- **更新了阿拉伯语**：增强了对阿拉伯语的语言支持，以包括按意向进行绝对评分，以及将意向标记为不相关的功能；有关其他详细信息，请参阅[支持的语言](/docs/services/assistant?topic=assistant-language-support)主题。请注意，{{site.data.keyword.conversationshort}} 服务学习模型可能已在此增强功能中进行更新；因此重新训练模型时，将应用所有更改；有关更多信息，请参阅[更新的模型](#release-notes-updated-models)。

## 2017 年 6 月 23 日
{: #23June2017}

- **更新了韩国语**：增强了对韩国语的语言支持；有关其他详细信息，请参阅[支持的语言](/docs/services/assistant?topic=assistant-language-support)主题。请注意，{{site.data.keyword.conversationshort}} 服务学习模型可能已在此增强功能中进行更新；因此重新训练模型时，将应用所有更改；有关更多信息，请参阅[更新的模型](#release-notes-updated-models)。

## 2017 年 6 月 22 日
{: #22June2017}

- **引入了槽**：现在，通过添加槽，可以更轻松地从单个节点中的用户那里收集多条信息。以前，必须创建多个对话节点，才能涵盖用户提供信息时可能采用的所有方式的组合。通过槽，可以配置单个节点来保存用户提供的所有信息，并且系统会提示提供用户未曾输入的任何必需的详细信息。有关更多详细信息，请参阅[使用槽收集信息](/docs/services/assistant?topic=assistant-dialog-slots)。
- **简化了对话树** - 对话树已重新设计，提高了易用性。树形视图更紧凑，因此可以更轻松地查看您在其中的位置。节点之间的链接表示方式让您更容易了解节点之间的关系。

## 2017 年 6 月 21 日
{: #21June2017}

- **支持阿拉伯语**：现在，对阿拉伯语的语言支持已一般可用。有关详细信息，请参阅[配置双向语言](/docs/services/assistant?topic=assistant-language-support#language-support-configuring-bi-directional)。
- **更新了语言**：更新了 {{site.data.keyword.conversationshort}} 服务算法，以改进总体语言支持。有关详细信息，请参阅[支持的语言](/docs/services/assistant?topic=assistant-language-support)主题。

## 2017 年 6 月 16 日
{: #16June2017}

- **建议（Beta - 仅限高级用户）**：“改进”面板还包含**建议**页面，用于分析用户与聊天机器人之间的会话，考虑系统当前的训练数据和响应确定性，从而建议系统改进方法。

## 2017 年 6 月 14 日
{: #14June2017}

- **其他语言的模糊匹配 (Beta)**：现在，实体模糊匹配可用于其他语言，如[支持的语言](/docs/services/assistant?topic=assistant-language-support)主题中所注明。可以启用按实体模糊匹配来提高服务识别用户输入中语法类似于实体，但不需要完全匹配的项的能力。此功能可将用户输入映射到对应的正确实体，就算存在拼写错误或语法方面略有差异也能正确映射。例如，如果将 giraffe 定义为动物实体的同义词，而用户输入包含词汇 giraffes 或 girafe，那么模糊匹配能够正确地将该词汇映射到动物实体。有关详细信息，请参阅[模糊匹配](/docs/services/assistant?topic=assistant-entities#entities-fuzzy-matching)。

## 2017 年 6 月 13 日
{: #13June2017}

- **用户会话**：现在，“改进”面板包含**用户会话**页面，可提供用户与聊天机器人的交互列表，并可按关键字、意向、实体或天数进行过滤。可以打开单个会话来更正意向，或添加实体值或同义词。
- **更改了正则表达式**：更改了 SpEL 函数（如 find、matches、extract、replaceFirst、replaceAll 和 split）支持的正则表达式。不再允许使用一组正则表达式构造，包括向前查找、向后查找、占有式重复和反向引用构造。此更改是为了避免原始正则表达式库中存在的安全隐患。

## 2017 年 6 月 12 日
{: #12June2017}

- 可以使用 **Lite** 套餐（以前称为“免费套餐”）创建的最大工作空间数已从 3 个更改为 5 个。
- 现在，可以为对话节点指定任何名称；名称不必唯一。而且随后可以更改节点名，而不影响节点在内部的引用方式。指定的名称会视为别名，而系统使用其自己的内部标识来引用节点。
- 通过编辑工作空间详细信息来创建工作空间后，即无法再更改该工作空间的语言。如果需要更改语言，可以将工作空间导出为 JSON 文件，更新 language 属性，然后将 JSON 文件作为新的工作空间导入。

## 2017 年 6 月 6 日
{: #6June2017}

- **了解**：提供了新的 *了解 {{site.data.keyword.conversationfull}}* 页面，其中包含入门信息以及服务文档和其他有用资源的链接。要打开该页面，请单击页面标题中的 ![i 表示信息](images/info.png) 图标。
- **批量导出和删除**：现在，可以将多个意向或实体同时导出到一个 CSV 文件，以便以后可以将其导入并复用于其他 {{site.data.keyword.conversationshort}} 应用程序。您还可以同时选择多个实体或意向以批量删除。
- **更新了韩国语**：更新了韩国语记号化器，以处理非正式语言支持。IBM 会继续改进实体识别和分类。
- **支持表情符号**：现在，将对添加到意向示例或添加为实体值的表情符号进行正确分类/抽取。

  只有训练数据中包含的表情符号才会正确并一致地得到识别；表情符号支持可能无法正确对色调不同的类似表情符号或其他变体进行分类。
  {: note}

- **实体词干提取（Beta - 仅限英语）**：此模糊匹配 Beta 功能可识别实体，并根据实体值的词干形式来匹配实体。例如，此功能可将“bananas”正确识别为类似于“banana”，“run”类似于“running”，因为它们共享公共词干形式。有关更多信息，请参阅[模糊匹配](/docs/services/assistant?topic=assistant-entities#entities-fuzzy-matching)。
- **工作空间导入进度**：从 JSON 文件导入工作空间时，将立即显示工作空间的磁贴，其中会显示有关导入进度的信息。
- **缩短了训练时间**：现在，可并行训练多个模型，从而显著缩短大型工作空间的训练时间。

## 2017 年 5 月 26 日
{: #26May2017}

- 当前 API 版本为 `2017-05-26`。此版本引入了以下更改：
    - 更改了 ErrorResponse 对象的模式。此更改会影响所有端点和方法。有关更多信息，请参阅 [API 参考 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://{DomainName}/apidocs/assistant){: new_window}。
    - 更改了用于表示已导出工作空间 JSON 中对话节点的内部模式。如果使用 `2017-05-26` API 来导入使用较早版本导出的工作空间，那么某些对话节点可能无法正确导入。为了获得最佳结果，请始终使用导出工作空间的同一版本来导入该工作空间。

## 2017 年 5 月 25 日
{: #25May2017}

- 现在，可以在“试用”窗格中管理上下文变量。单击**管理上下文**链接可打开一个新的窗格，在测试对话时，可在此窗格中设置和检查上下文变量值。有关更多信息，请参阅[测试对话](/docs/services/assistant?topic=assistant-dialog-build#dialog-build-test)。

## 2017 年 5 月 16 日
{: #16May2017}

- 现在，打开工具时，提供了**汽车仪表板**样本工作空间。要将此样本用作自己工作空间的起始点，请编辑此工作空间。如果要将其用于多个工作空间，请改为复制样本。除非使用样本工作空间，否则样本工作空间不会计入工作空间总计。
- 现在，浏览工具更轻松。导航菜单选项在主页的侧面（而不是顶部）提供。在页面顶部，会显示面包屑链接，表明您所在的位置。现在，可以在“工作空间”页面中切换服务实例。要快速访问该页面，请单击导航菜单中的**返回到工作空间**。如果有多个服务实例，那么将显示当前实例的名称。可以单击其旁边的**更改**链接来选择其他实例。
- 现在，创建对话时，将添加两个节点：1) 对话树顶部的**欢迎**节点，用于包含要向用户显示的问候语，以及 2)**其他**节点，用于捕获对话中其他节点无法识别的任何用户查询并对其做出响应。有关更多详细信息，请参阅[创建对话](/docs/services/assistant?topic=assistant-dialog-build)。
- 现在，在“试用”窗格中测试对话时，可以通过按“向上”键来循环浏览先前的输入，以找到并重新提交最近的测试发声。
- 现在，为 5 个系统实体（`@sys-date`、`@sys-time`、`@sys-currency`、`@sys-number` 和 `@sys-percentage`）提供了试验性韩国语语言支持。某些数字实体存在已知问题，并且对非正式语言输入的支持有限。
- “改进”选项卡中提供了“概述”页面。该页面提供与机器人的交互摘要。可以查看给定时间段内的流量以及在用户会话中最常识别到的意向和实体。
有关其他信息，请参阅[使用“概述”页面](/docs/services/assistant?topic=assistant-logs-overview)。

## 2017 年 4 月 27 日
{: #27April2017}

- 现在，以下系统实体仅作为英文环境中的 Beta 功能提供：
    - sys-location：识别用户发声中对位置（例如，城镇、城市和国家/地区）的引用。
    - sys-person：识别用户发声中对人员姓名（名字和姓氏）的引用。

有关更多信息，请参阅[系统实体引用](/docs/services/assistant?topic=assistant-system-entities)。
- 实体的模糊匹配是 Beta 功能，目前在英语环境中可用。可以启用按实体模糊匹配来提高服务识别用户输入中语法类似于实体，但不需要完全匹配的项的能力。此功能可将用户输入映射到对应的正确实体，就算存在拼写错误或语法方面略有差异也能正确映射。例如，如果将 **giraffe** 定义为动物实体的同义词，而用户输入包含词汇 *giraffes* 或 *girafe*，那么模糊匹配能够正确地将该词汇映射到动物实体。有关详细信息，请参阅[定义实体](/docs/services/assistant?topic=assistant-entities#entities-fuzzy-matching)并搜索`模糊匹配`。

## 2017 年 4 月 18 日
{: #18April2017}

- 现在，{{site.data.keyword.conversationshort}} REST API 支持访问以下资源：
    - 实体
    - 实体值
    - 实体值同义词
    - 日志

    有关更多信息，请参阅 [API 参考 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://{DomainName}/apidocs/assistant){: new_window}。
- /messages `POST` 方法的行为更改了处理实体和意向的方式（实体和意向是作为消息输入一部分指定的）。
    - 如果在输入上指定意向，那么服务将使用指定的意向，但会使用自然语言处理来检测用户输入中的实体。
    - 如果在输入上指定实体，那么服务将使用指定的实体，但会使用自然语言处理来检测用户输入中的意向。

         对于同时指定意向和实体的消息，或者这两项均未指定的消息，行为尚未更改。
- 现在，用于将用户输入标记为不相关的选项可用于所有支持的语言。这是 Beta 功能。
- 新的“凭证”选项卡提供了单一位置，在其中可以找到将应用程序连接到工作空间（例如，服务凭证和工作空间标识）所需的所有信息以及其他部署选项。要访问工作空间的“凭证”选项卡，请单击 ![菜单](images/Menu_16.png) 图标，然后选择**凭证**。

## 2017 年 3 月 9 日
{: #9March2017}

现在，{{site.data.keyword.conversationshort}} REST API 支持访问以下资源：

- 工作空间
- 意向
- 示例
- 反例

有关更多信息，请参阅 [API 参考 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://{DomainName}/apidocs/assistant){: new_window}。

## 2017 年 3 月 7 日
{: #7March2017}

- 将 `.` 或 `..` 用作意向名称会导致问题，因此不再支持。

    无法对此名称的意向执行重命名或删除；要更改名称，请将意向导出到文件，重命名文件中的意向，然后将更新后的文件导入到工作空间。

    付费客户可以联系支持人员进行数据库更改。

## 2017 年 3 月 1 日
{: #1March2017}

- 现在，德语环境中支持系统实体。

## 2017 年 2 月 22 日
{: #22February2017}

- 现在，消息限制为 2,048 个字符。

## 2017 年 2 月 3 日
{: #3February2017}

- 更改了意向评分方式，并添加了将输入标记为与应用程序不相关的功能。有关详细信息，请参阅[定义意向](/docs/services/assistant?topic=assistant-intents)并搜索`标记为不相关`。

- 此发行版引入了对工作空间的重大更改。要从这些更改中获益，您必须手动升级工作空间。

- 更改了**跳转至**操作的处理，以防止在某些条件下可能发生的循环。先前，如果跳转至节点的条件，但是该节点或其任何对等节点都没有求值为 true 的条件，那么系统将跳转至根级别节点，并查找其条件与输入匹配的节点。在某些情境中，此处理会产生循环，导致无法继续处理对话。

  在新过程下，如果目标节点或其同级都未求值为 true，那么对话将结束。要重新实施旧模型，请添加条件为 `true` 的最终对等节点。在响应中，使用将对话树根级别的第一个节点条件作为目标的**跳转至**操作。

## 2017 年 1 月 11 日
{: #11January2017}

- 在此发行版中，可以在对话中定制节点标题。

## 2016 年 12 月 22 日
{: #22December2016}

- 在此发行版中，对话节点将显示用于`节点标题`的新部分。定制`节点标题`的功能不可用。折叠时，`节点标题`会显示对话节点的`节点条件`。如果没有`节点条件`，那么会将“无标题节点”显示为标题。

## 2016 年 12 月 19 日
{: #19December2016}

对对话编辑器进行了多项更改，使其使用起来更容易、更直观：

- 编辑视图更大，在处理节点时更容易查看节点的所有详细信息。
- 一个节点可以包含多个响应，每个响应由单独的条件触发。有关更多信息，请参阅[多个响应](/docs/services/assistant?topic=assistant-dialog-overview#dialog-overview-responses)。

## 2016 年 12 月 5 日
{: #5December2016}

- 支持新语言，所有新语言都为“试验性”方式：德语、繁体中文、简体中文和荷兰语。
- 提供了两个新的系统实体：@sys-date 和 @sys-time。有关详细信息，请参阅[系统实体](/docs/services/assistant?topic=assistant-system-entities)。

## 2016 年 10 月 21 日
{: #21October2016}

- 现在，{{site.data.keyword.conversationshort}} 服务提供了系统实体，这些是可以在任何用例中使用的公共实体。有关详细信息，请参阅[定义实体](/docs/services/assistant?topic=assistant-entities)并搜索`启用系统实体`。
- 现在，可以在“改进”页面上查看与用户的会话历史记录。可以借此来了解机器人的行为。有关详细信息，请参阅[改进技能](/docs/services/assistant?topic=assistant-logs-intro)。
- 现在，可以通过逗号分隔值 (CSV) 文件导入实体，如果您有大量实体，此功能会很有用。有关详细信息，请参阅[定义实体](/docs/services/assistant?topic=assistant-entities)并搜索`导入实体`。

## 2016 年 9 月 20 日
{: #20September2016}

**新版本**：2016-09-20

要利用新版本中的更改，请将 `version` 参数的值更改为新的日期。如果尚未准备好更新到此版本，请不要更改版本日期。

- version **2016-09-20**：`dialog_stack` 从字符串数组更改为 JSON 对象数组。

## 2016 年 8 月 29 日
{: #29August2016}

- 可以将对话节点从一个分支移至另一个分支以作为同代或同级。有关详细信息，请参阅[移动对话节点](/docs/services/assistant?topic=assistant-dialog-build#dialog-build-move-node)。
- 可以展开 JSON 编辑器窗口。
- 可以查看机器人会话的交谈日志，以帮助了解其行为。可以按意向、实体、日期和时间进行过滤。有关详细信息，请参阅[改进技能](/docs/services/assistant?topic=assistant-logs-intro)。

## 2016 年 7 月 11 日
{: #21July2016}

- 此一般可用性发行版支持使用实体和对话来创建全功能机器人。

## 2016 年 5 月 18 日
{: #18May2016}

- {{site.data.keyword.conversationshort}} 的此试验发行版引入了用户界面，支持使用工作空间、意向和示例。
