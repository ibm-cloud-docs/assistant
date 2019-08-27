---

copyright:
  years: 2015, 2019
lastupdated: "2019-07-26"

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

要首先了解有关什么是助手的更多信息，请参阅[助手](/docs/services/assistant?topic=assistant-assistants)。

要创建助手，请执行以下步骤：

1.  单击**创建助手**。

1.  添加有关新助手的详细信息：

    - **名称**：名称，长度不超过 100 个字符。名称是必填的。
    - **描述**：可选的描述，长度不超过 200 个字符。

    这将自动创建 IBM 品牌的公共 Web 页面，您和团队可以使用该页面来测试助手。如果不希望创建预览 Web 页面，请取消选中**启用预览链接**复选框。

1.  单击**创建助手**。

1.  通过选择要添加的下列其中一种技能类型，向助手添加技能。

    **注**：可以选择添加现有技能，也可以创建新技能。

    - **添加对话技能**：使用 Watson 自然语言处理和机器学习技术来理解用户的问题和请求，并使用您编写的回答对其进行响应。

      从此处添加对话技能时，可获取开发版本。如果要添加特定对话技能版本，请改为在技能的*版本*页面中进行添加。

    - **添加搜索技能** ![仅限增强版或高端套餐](images/plus.png)：对于给定的用户查询，使用 {{site.data.keyword.discoveryfull}} 服务在您标识的数据源中检索信息，并将其发现的任何相关信息作为对用户的响应进行共享。

      仅当您是增强版或高端套餐用户时，此选项才会显示。
      {: note}

    请参阅[创建技能](/docs/services/assistant?topic=assistant-skill-add)。

## 助手限制
{: #assistant-add-limits}

可以创建的助手数取决于 {{site.data.keyword.conversationshort}} 套餐类型。

|套餐|每个服务实例的助手数        |
|--------------|--------------------------------:|
|高端                                 |100 |
|增强版                               |100 |
|标准                                 |100 |
|增强试用版                           |5 |
|轻量*            |100 |
{: caption="套餐详细信息" caption-side="top"}

* 轻量套餐服务实例中未使用的助手处于不活动状态的时间达到 30 天后，可能会将其删除以释放空间。

有关主题的更多信息，请参阅[更改不活动状态超时设置](/docs/services/assistant?topic=assistant-assistant-settings)。

您可以将每种类型的一个技能连接到助手。根据您所拥有的套餐，可以构建的技能数有所不同。有关更多详细信息，请参阅[技能限制](/docs/services/assistant?topic=assistant-skill-add#skill-add-limits)。

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

您可以将每种技能类型的一个技能添加到助手。如果要更改助手使用的技能，可以将一个技能交换为另一个技能。

1.  在“助手”选项卡中，打开助手。

1.  单击要交换的技能的 ![打开和关闭选项列表](images/kabob-beta.png) 图标，然后选择**交换技能**。

    要将当前对话技能交换为该技能的不同版本，请选择**更改技能版本**。

1.  选择要改为使用的现有技能，或者[创建技能](/docs/services/assistant?topic=assistant-skill-add)。

### 在服务实例之间切换
{: #assistant-add-switch-instance}

如果您有多个服务实例，那么可以查看页面标题来了解当前正在使用的实例。如果正在技能中工作，请首先单击**技能**面包屑链接。条幅会显示当前实例名称。要切换到其他服务实例，请单击**更改**，然后选择相应的实例。
