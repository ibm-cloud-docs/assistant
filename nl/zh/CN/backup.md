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

# 备份和复原数据
{: #backup}

通过先导出数据，然后再导入数据来备份和复原数据。
{: shortdesc}

可以从 {{site.data.keyword.conversationshort}} 服务实例导出以下数据：

- 对话技能训练数据（意向和实体）
- 对话技能对话

无法导出以下数据：

<!--- Search skill -->
- 助手，包括任何配置的集成

## 保留日志
{: #backup-retain-logs}

如果要存储用户与助手之间的会话的日志，可以使用 `/logs` API 来导出日志数据。有关详细信息，请参阅 [API 参考 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://cloud.ibm.com/apidocs/assistant#list-log-events-in-a-workspace)。

要获取技能的工作空间标识，请在技能磁贴中，单击 ![打开和关闭选项列表](images/kabob-beta.png) 图标，然后选择**查看 API 详细信息**。
{: tip}

根据服务套餐，所存储日志的时间量不同。例如，轻量套餐仅提供过去 7 天的日志。有关更多信息，请参阅[日志限制](/docs/services/assistant?topic=assistant-logs#logs-limits)。

无法将日志从一个技能导入到其他技能中。

## 导出对话技能
{: #backup-export-skill}

要备份对话技能数据，请将技能导出为 JSON 文件，并存储该 JSON 文件。

1.  在“技能”页面上或在使用技能的助手的配置页面上，找到对话技能磁贴。

1.  单击 ![打开和关闭选项列表](images/kabob-beta.png) 图标，然后选择**下载 JSON**。

1.  指定 JSON 文件的名称以及保存该文件的位置，然后单击**保存**。

或者，可以使用 `/workspaces` API 来导出对话技能。请在 GET workspace 请求中包含 `export=true` 参数。有关更多详细信息，请参阅 [API 参考 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://cloud.ibm.com/apidocs/assistant#get-information-about-a-workspace)。

## 导入对话技能
{: #backup-import-skill}

要恢复从其他服务实例或环境中导出的对话技能备份副本，请通过导入已导出对话技能的 JSON 文件来创建新的对话技能。

如果 {{site.data.keyword.conversationshort}} 服务在从导出技能到将其导入的这段时间内发生更改，那么由于功能更新会定期应用于云托管的持续交付环境中的实例，因此所导入技能的作用方式可能与之前有所不同。
{: important}

1.  单击**技能**选项卡。

1.  单击**新建**。

1.  单击**导入技能**，然后单击**选择 JSON 文件**，并选择要导入的 JSON 文件。

    **重要信息：**

    - 导入的 JSON 文件必须使用 UTF-8 编码，而不使用字节顺序标记 (BOM) 编码。
    - 技能 JSON 文件的最大大小为 10 MB。如果需要导入更大的技能，请考虑在导入技能后单独导入意向和实体。（您还可以使用 REST API 导入更大的技能。有关更多信息，请参阅 [API 参考 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://cloud.ibm.com/apidocs/assistant#create-workspace){: new_window}。）
    - JSON 文件不能包含制表符、换行符或回车符。

    要导入已导出技能的完整副本，请选择**所有内容（意向、实体和对话）**。

    单击**导入**。

    如果导入技能时遇到问题，请参阅[有关技能导入问题的故障诊断](/docs/services/assistant?topic=assistant-skill-add#skill-add-import-errors)。

1.  指定技能的详细信息：

    - **名称**：名称，长度不超过 100 个字符。名称是必填的。
    - **描述**：可选的描述，长度不超过 200 个字符。
    - **语言**：将对技能进行训练以理解用户输入的语言。缺省值为“英语”。

创建技能后，该技能即会在“技能”页面上显示为磁贴。

## 重新创建助手
{: #backup-recreate-assistant}

现在，可以重新创建助手。然后，可以将导入的对话技能链接到助手，并为其配置集成。

有关更多详细信息，请参阅[创建助手](/docs/services/assistant?topic=assistant-assistant-add)和[添加集成](/docs/services/assistant?topic=assistant-deploy-integration-add#deploy-integration-add-task)。

## 更新客户机应用程序
{: #backup-update-api}

导入已导出的对话技能时，将创建新的技能。新技能具有新的工作空间标识。如果您有使用 V1 API 来访问此技能的现有客户机应用程序，那么必须更新任何工作空间标识引用，以改为使用新的工作空间标识。

重新创建助手时，将向其提供新的助手标识。如果您有使用 V2 API 来访问此助手的现有客户机应用程序，那么必须更新任何助手标识引用，以改为使用新的助手标识。
