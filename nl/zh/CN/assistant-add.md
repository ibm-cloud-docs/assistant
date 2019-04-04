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

# 创建助手
{: #assistant-add}

创建具有解决客户业务目标所需技能的助手。
{: shortdesc}

要创建助手，请执行以下步骤：

1.  单击**助手**选项卡。

1.  执行下列其中一个操作：

    - 要创建可以查看并从中学习的样本助手，请单击**添加样本**，然后选择要创建的样本助手。

      这将添加样本助手。您可以跳过此过程中的其余步骤。

      随样本助手一起提供了样本技能，并且该技能已添加到技能列表中。如果已创建相同类型的样本技能，那么现有技能会自动与此新样本助手相关联。
      {: note}

    - 要从头开始创建助手，请单击**新建**，然后完成此过程中的其余步骤。

1.  指定新助手的详细信息：
    - **名称**：名称，长度不超过 100 个字符。名称是必填的。
    - **描述**：可选的描述，长度不超过 200 个字符。

    这将自动创建 IBM 品牌的公共 Web 页面，您和团队可以使用该页面来测试助手。如果不希望创建预览 Web 页面，请取消选中**启用预览链接**复选框。

1.  单击**创建**。

1.  通过单击**添加技能**向助手添加技能。可以选择添加现有技能，也可以创建新技能。

    从此处添加技能时，可获取开发版本。如果要添加特定技能版本，请改为在技能的*版本历史记录*选项卡中进行添加。

    对于使用 {{site.data.keyword.conversationshort}} 服务（先前称为 Watson Conversation）的一般可用版本构建的任何工作空间，如果已创建或已被授予其开发者角色访问权，那么将看到这些工作空间作为现有对话技能列出。
    {: note}

    有关如何创建技能的更多信息，请参阅[创建技能](/docs/services/assistant?topic=assistant-skill-add)。

## 助手限制
{: #assistant-add-limits}

可以在单个服务实例中创建的助手数取决于 {{site.data.keyword.conversationshort}} 套餐。

|服务套餐          |每个服务实例的助手数        |每个助手的集成数  |交谈会话不活动时间段          |
|--------------|--------------------------------:|----------------------------:|-----------------:|
|高级                                 |100 |100 |60 分钟|
|增强版                               |100 |100 |60 分钟|
|标准                                 |100 |100 |5 分钟|
|Lite*            |100 |100 |5 分钟|
{: caption="服务套餐详细信息" caption-side="top"}

* 轻量套餐服务实例中未使用的助手处于不活动状态的时间达到 30 天后，可能会将其删除以释放空间。

您可以将一个技能连接到您的助手。根据您所拥有的套餐，可以构建的技能数有所不同。有关更多详细信息，请参阅[技能限制](/docs/services/assistant?topic=assistant-skill-add#skill-add-limits)。

## 删除助手
{: #assistant-add-delete}

删除助手时，还会自动删除为该助手定义的所有集成。但不会删除添加到该助手的技能。

要删除助手，请执行以下步骤：

1.  在“助手”选项卡中，找到要删除的助手。

1.  单击 ![打开和关闭选项列表](images/kabob-beta.png) 图标，然后选择**删除**。确认此删除操作。

## 重命名助手
{: #assistant-add-rename}

可以在创建助手后，更改助手的名称及其关联的描述。

要重命名助手，请执行以下步骤：

1.  在“助手”选项卡中，找到要重命名的助手。

1.  单击 ![打开和关闭选项列表](images/kabob-beta.png) 图标，然后选择**重命名**。

1.  编辑名称，然后单击**重命名**以保存更改。

### 更改与助手关联的技能
{: #assistant-add-swap-skill}

您可以向一个助手添加一个技能。如果要更改助手使用的技能，可以将一个技能交换为另一个技能。

1.  在“助手”选项卡中，单击以打开要更改其技能的助手的磁贴。

1.  单击 ![打开和关闭选项列表](images/kabob-beta.png) 图标，然后选择**交换技能**。要将当前技能交换为该技能的不同版本，请选择**更改技能版本**。

1.  选择要改为使用的现有技能，或者[创建技能](/docs/services/assistant?topic=assistant-skill-add)。

### 在服务实例之间切换
{: #assistant-add-switch-instance}

如果您有多个服务实例，那么可以查看页面标题来了解当前正在使用的实例。如果正在技能中工作，请首先单击**技能**面包屑链接。条幅会显示当前实例名称。要切换到其他服务实例，请单击**更改**，然后选择相应的实例。
