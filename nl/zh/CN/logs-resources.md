---

copyright:
  years: 2015, 2019
lastupdated: "2019-05-28"

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

# 高级任务
{: #logs-resources}

了解可用于访问和分析日志数据的 API 和其他工具。
{: shortdesc}

## API
{: #logs-resources-api}

可以使用 `/logs` API 来列出用户与助手之间的会话记录中的事件。有关详细的 API 参考文档，请参阅[列出日志事件 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://cloud.ibm.com/apidocs/assistant#list-log-events-in-a-workspace)。

日志存储天数因服务套餐类型而有所不同。有关详细信息，请参阅[日志限制](/docs/services/assistant?topic=assistant-logs#logs-limits)。

要获取为了导出日志并将其转换为 CSV 格式而可以运行的 Python 脚本，请从 [Watson Assistant GitHub ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://github.com/watson-developer-cloud/community/blob/master/watson-assistant/export_logs.py) 存储库中下载 `export_logs.py` 文件。

## 与日志相关的术语
{: #logs-resources-terminology}

首先，查看与 {{site.data.keyword.conversationshort}} 日志关联的术语的定义：

- ***助手***：用于实现 {{site.data.keyword.conversationshort}} 内容的应用程序，有时称为“聊天机器人”。
- ***会话***：一组消息，包含单个用户向助手发送的消息以及助手发回的消息。
- ***会话标识***：添加到单个消息调用的唯一标识，用于将相关的消息交换链接在一起。使用 V1 版本 {{site.data.keyword.conversationshort}} API 的应用程序开发者通过将标识包含在 context 对象的 metadata 中，将此值添加到会话中的消息调用。
- ***客户标识***：可用于标注客户数据的唯一标识，以便随后客户请求除去其数据时，可以将这些数据删除。
- ***部署标识***：应用程序开发者使用 V1 版本的 {{site.data.keyword.conversationshort}} API 随每个用户消息一起传递的唯一标签，用于帮助标识生成该消息的部署环境。
- ***实例***：{{site.data.keyword.conversationshort}} 的部署，可使用唯一凭证进行访问。一个 {{site.data.keyword.conversationshort}} 实例可能包含多个助手。
- ***消息***：消息是用户向助手发送的单条话语。
- ***技能标识***：技能的唯一标识。
- ***用户***：用户是与助手进行交互的任何人；通常用户是您的客户。
- ***用户标识***：用于跟踪特定用户的服务使用级别的唯一标签。
- ***工作空间标识***：工作空间的唯一标识。虽然在 11 月 9 日之前创建的任何工作空间都会在产品用户界面中显示为技能，但技能和工作空间并不是一回事。技能实际上是 V1 工作空间的包装器。

**重要信息**：**用户标识**属性*不*等同于**客户标识**属性，但两者都可以随消息一起传递。**用户标识**字段用于跟踪使用量级别以进行计费，而**客户标识**字段用于支持标记和后续删除与最终用户关联的消息。客户标识在所有 Watson 服务中以一致的方式使用，并在 `X-Watson-Metadata` 头中进行指定。用户标识由 {{site.data.keyword.conversationshort}} 服务专用，并在每个 /message API 调用的 context 对象中传递。

## 启用用户度量值
{: #logs-resources-user-id}

例如，通过用户度量值，可以在[概述页面](/docs/services/assistant?topic=assistant-logs-overview)上查看与助手互动的唯一用户数，或者在给定时间间隔内每个用户的平均会话数。使用唯一`用户标识`参数启用用户度量值。

要为使用 `/message` API 发送的消息指定`用户标识`，请在 [context ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://cloud.ibm.com/apidocs/assistant?curl=#get-response-to-user-input){: new_window} 中的 metadata 对象内包含 `user_id` 属性，如以下示例中所示：

```
"context" : {
            "metadata" : {
       "user_id": "{UserID}"
  }
}
```
{: codeblock}

## 将消息数据与用户关联以进行删除
{: #logs-resources-customer_id}

有时您可能希望从 {{site.data.keyword.conversationshort}} 实例中完全除去用户的数据集。使用删除功能后，“概述”度量值不会再反映出这些已删除的消息；例如，“会话总数”将减少。

### 开始之前
{: #logs-resources-delete-customer-id-prereqs}

要删除一个或多个个人的消息，首先需要将消息与每个人的唯一**客户标识**相关联。要为使用 `/message` API 发送的任何消息指定**客户标识**，请在头中包含 `X-Watson-Metadata: customer_id` 属性。可以使用 `customer_id` 以分号分隔的 `field=value` 对传递多个**客户标识**条目，如以下示例中所示：

```
curl -X POST -u "apikey:3Df... ...Y7Pc9"
 --header
   'Content-Type: application/json'
   'X-Watson-Metadata: customer_id={first-customer-ID};customer_id={second-customer-ID}'
 --data '{"input":{"text":"hello"}}' 'https:// gateway-us-south.watsonplatform.net/assistant/api/v1/workspaces/{workspaceID}/message?version=2018-09-20'
```
{: codeblock}

`customer_id` 字符串不能包含分号 (`;`) 或等号 (`=`) 字符。确保每个`客户标识`参数在您的客户中唯一是您的责任。
{: note}

要使用 `customer_id` 值来删除消息，请参阅[信息安全](/docs/services/assistant?topic=assistant-information-security#information-security-gdpr-wa)主题。

## Jupyter 笔记本
{: #logs-resources-jupyter-notebooks}

IBM 创建了 Jupyter 笔记本，可用于更详细地分析日志数据。Jupyter 笔记本是基于 Web 的交互式计算环境。您可以运行少量代码来处理数据，并可以立即查看计算的结果。

有一组笔记本可以用于标准 Python 工具，还有一组笔记本设计为用于 {{site.data.keyword.DSX_full}} 时效果最佳。{{site.data.keyword.DSX_short}} 产品提供了一个环境，在其中可以选取和选择所需工具来对数据进行分析和可视化，清理和塑造数据，摄入流数据，或者创建、训练和部署机器学习模型。有关更多详细信息，请参阅[产品文档 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://dataplatform.cloud.ibm.com/docs/content/getting-started/welcome-main.html){:new_window}。

要了解有关笔记本如何帮助改进助手的更多信息，请[阅读此博客帖子 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://medium.com/ibm-watson/continuously-improve-your-watson-assistant-with-jupiter-notebooks-60231df4f01f)。

提供了以下笔记本：

- **度量**：收集专注于覆盖率（助手有足够信心响应用户的频率）和有效性（助手响应时，响应是否满足用户需求）的度量值。

- **有效性**：对日志执行更深入的分析，以帮助您了解可以执行哪些步骤来改进助手。

[Watson Assistant Continuous Improvement Best Practices ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://www.ibm.com/common/ssi/cgi-bin/ssialias?htmlfid=54022554USEN&) 指南描述了如何最充分地利用笔记本。

### 将笔记本用于 {{site.data.keyword.DSX}}
{: #logs-resources-notebooks-studio}

如果选择使用设计为用于 {{site.data.keyword.DSX}} 的笔记本，那么步骤大致如下：

1.  创建 {{site.data.keyword.DSX}} 帐户，[创建项目 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://dataplatform.cloud.ibm.com/docs/content/getting-started/projects.html?context=analytics){:new_window}，然后向其添加 Cloud Object Storage 帐户。
1.  在 {{site.data.keyword.DSX}} 社区中，获取 [Measure Watson Assistant Performance ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")]( https://dataplatform.cloud.ibm.com/exchange/public/entry/view/133dfc4cd1480bbe4eaa78d3f635e568) 笔记本。
1   遵循随该笔记本一起提供的逐步指示信息来分析日志中的对话交流子集。

    这些洞察可以通过多种方式进行可视化，让您更容易理解助手的覆盖率和有效性。
1.  从效率低下的会话中导出日志的样本集，然后对其进行分析和注释。

    例如，指示响应是否正确。如果正确，标记它是否有用。如果响应不正确，请确定根本原因，例如检测到错误的意向或实体，或者触发了错误的对话节点。确定根本原因后，指示正确的选择是什么。
1.  将注释的电子表格发送到 [Analyze Watson Assistant Effectiveness](https://dataplatform.cloud.ibm.com/exchange/public/entry/view/133dfc4cd1480bbe4eaa78d3f636921c) 笔记本。

此过程可帮助您了解可以执行哪些步骤来改进助手。无法将您了解的内容自动应用于服务实例。请跟踪为了改进系统所做的任何更改，以便随后可以直接将其应用于对话技能的训练数据。

### 将笔记本用于标准 Python 工具
{: #logs-resources-notebooks-python}

如果选择使用标准 Python 工具来运行笔记本，那么可以从 [IBM GitHub 存储库](https://github.com/watson-developer-cloud/assistant-improve-recommendations-notebook/tree/master/notebook)中获取笔记本。请确保按以下顺序运行笔记本：

1.  Measure Notebook.ipynb
1.  Effectiveness Notebook.ipynb
