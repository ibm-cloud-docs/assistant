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

# 开发流程
{: #dev-process}

构建和部署会话助手以及通过增量方式对其进行改进时，可通过 {{site.data.keyword.conversationshort}} 工具来利用 AI。
{: shortdesc}

![显示从开发训练数据开始一直到部署至生产环境结束的开发步骤流程](images/dev-process.png)

## 工作流程
{: #dev-process-workflow}

助手项目的典型工作流程包括以下步骤：

1.  定义希望助手代表您处理的一系列范围狭窄的关键客户需求，包括助手可以为客户启动或完成的任何业务流程。
1.  创建意向，用于表示在上一步中识别到的客户需求。例如，`About_company` 或 `#Place_order` 等意向。

    使用意向示例建议功能来挖掘现有呼叫中心日志，以查找最佳意向用户示例。
    {: tip}

1.  构建一个对话，用于检测定义的意向并对其进行处理，对话可具有简单的响应，或具有可收集更多信息的对话流。
1.  定义为了更明确了解用户意图而需要的任何实体。

    挖掘现有意向用户示例，以获取常见实体值提及项。通过使用注释来定义实体，不仅可捕获实体值的文本，还能捕获实体值通常在语句中使用时的上下文。

    对于基于字典的实体，请使用同义词建议来扩展实体定义。
    {: tip}

1.  在“试用”窗格中，对于向助手添加的每个功能，请一边添加功能，一边以递增方式对其进行测试。
1.  拥有能够成功处理关键任务的有效助手后，添加用于将该助手部署到开发环境的集成。测试部署的助手并进行优化。

1.  构建有效的助手后，创建对话技能的快照，并将其另存为版本。

    通过在达到开发里程碑时保存一个版本，让您可以在对技能进行的后续更改降低了其有效性时，恢复为原先较好的内容。请参阅[创建技能版本](/docs/services/assistant?topic=assistant-versions)。
1.  将助手版本部署到测试环境中，并对其进行测试。

    如果使用的是 Web 托管的交谈窗口小部件，那么可以与他人共享 URL，由他们帮助进行测试。
1.  使用“分析”选项卡中的度量值来查找需要改进的方面，并进行调整。

    如果需要测试用于解决某个问题的替代方法，请为每个解决方案创建一个版本，以便可以独立部署和测试每个解决方案，并比较结果。
1.  对助手的性能感到满意后，将最佳版本的助手部署到生产环境中。
1.  监视用户与部署的助手之间会话的日志。

    可以在技能的开发版本的“分析”选项卡中，查看正在生产环境中运行的技能版本的日志。发现分类不正确或其他问题时，可以在技能的开发版本中对其进行更正，然后对改进的版本进行测试后，将其部署到生产环境。有关更多详细信息，请参阅[跨助手进行改进](/docs/services/assistant?topic=assistant-logs#logs-deploy-id)。

分析日志和改进对话技能的过程会持续进行。随着时间的推移，您可能希望扩展助手可以处理的任务。客户需求也会变化。随着新需求的出现，部署的助手生成的度量值可以帮助您在后续迭代中识别并处理这些情况。
