---

copyright:
  years: 2015, 2018
lastupdated: "2018-02-13"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:swift: .ph data-hd-programlang='swift'}

# 关于改进组件

{{site.data.keyword.conversationshort}} 的“改进”组件提供了用户与工作空间进行的交互的历史记录。可以使用此历史记录来提高工作空间对用户输入的理解能力。
{: shortdesc}

**改进**面板的各个部分：

* [概述](logs_oview.html)：用户与工作空间的交互摘要。
* [用户会话](logs_convo.html)：用户发声的列表。可以在查看单个用户发声时更新意向和实体。**注**：单个用户会话可能由多个发声组成。
* [建议](logs_recommend.html)：改进系统的方法。仅可供高级用户使用。

## 跨工作空间进行改进
{: #deploy_id}

要了解如何使用发声数据来跨工作空间进行改进，查看与 {{site.data.keyword.conversationshort}} 服务关联的以下定义会非常有用：

* ***实例***：{{site.data.keyword.conversationshort}} 的部署，可使用唯一凭证进行访问。一个 {{site.data.keyword.conversationshort}} 实例可能由多个工作空间组成
* ***工作空间***：一个工作空间是 {{site.data.keyword.conversationshort}} 内容的一个模型；这通常可等同于一个机器人
* ***工作空间标识***：工作空间的唯一标识
* ***部署标识***：部署标识是与用户发声一起传递的唯一标签，以帮助识别发声来自的部署环境
* ***发声***：一个发声是用户向工作空间发送的单条消息

创建工作空间是一个迭代性很强的过程。在开发工作空间时，使用*试用*窗格可验证工作空间是否能识别到测试输入中的正确意向和实体，然后根据需要进行更正。

在**改进**面板中，可以查看有关与用户的实际交互的信息，并进行类似更正，以提高工作空间识别意向和实体的准确性。很难确切知道用户将*如何*提问或者他们可能进行哪些随机发声，因此请务必经常访问**改进**面板以便改进工作空间。

对于包含多个工作空间的 {{site.data.keyword.conversationshort}} 实例，有时使用来自一个工作空间的发声数据来改进同一个实例中的另一个工作空间可能会非常有用。**注**：如果您是 {{site.data.keyword.conversationshort}} 高级用户，那么可以选择将高级实例配置为允许访问跨不同高级实例的工作空间中的日志数据。

例如，假设您有一个名为 *HelpDesk* 的 {{site.data.keyword.conversationshort}} 实例。在 HelpDesk 实例中您可能同时拥有“生产”工作空间和“开发”工作空间。在“开发”工作空间中工作时，可以根据“生产”工作空间的`部署标识`来过滤发声，以便使用“生产”工作空间的发声来改进“开发”工作空间。

![数据源链接](images/data_source_1.png)

在“开发”工作空间中进行的任何编辑只会影响“开发”工作空间，即便您正在使用“生产”工作空间发声。有关其他信息，请参阅[选择数据源](logs_convo.html#select-source)。

要为使用 `/message` API 发送的发声指定部署标识，请在 [context ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://www.ibm.com/watson/developercloud/conversation/api/v1/#send_message){: new_window} 中的 metadata 对象中包含 deployment 属性，如以下示例中所示：

```
"context" : {
            "username" : "jane_doe@myemail.com",
  "member_type" : "gold",
  "metadata" : {
       "deployment": "HelpDesk-Production"
  }
}
```
{: #codeblock}
