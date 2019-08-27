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

# 创建技能
{: #skill-add}

通过向助手添加满足客户目标所需的技能，可对助手进行定制。
{: shortdesc}

可以创建以下类型的技能：

- **对话技能**：使用 Watson 自然语言处理和机器学习技术来理解用户的问题和请求，并使用您编写的回答对其进行响应。

- **搜索技能** ![仅限增强版或高端套餐](images/plus.png)：对于给定的用户查询，使用 {{site.data.keyword.discoveryfull}} 服务来搜索自助服务内容的数据源并返回回答。

  只有增强版或高端套餐的用户才能创建此类型的技能。
  
  通常，可首先为每种技能类型创建一个技能。然后，为对话技能构建对话时，可决定何时启动搜索技能。对于一些问题或请求，使用硬编码或以编程方式派生的响应（在对话技能中定义）已足够。对于另一些问题或请求，您可能希望通过返回一整段相关信息（即使用搜索技能从外部数据源中抽取的信息）来提供更充分的响应。

## 创建技能
{: #skill-add-task}

您可以将每种技能类型的一个技能添加到助手。

要创建技能，请完成以下步骤：

1.  单击**技能**选项卡，然后单击**创建技能**。

1.  选择要创建的技能类型，然后单击**下一步**。

    遵循相应过程中的剩余步骤来完成技能创建过程。

      - [对话技能](/docs/services/assistant?topic=assistant-skill-dialog-add)
      - [搜索技能](/docs/services/assistant?topic=assistant-skill-search-add)

## 技能限制
{: #skill-add-limits}

可以创建的技能数取决于 {{site.data.keyword.conversationshort}} 套餐类型。可供您使用的任何样本对话技能都不会计入限制，除非您使用这些技能。技能版本不作为技能进行计数。

|套餐|每个服务实例的技能数|
|------------------|----------------------------:|
|高端          |50|
|增强版            |50|
|标准                                 |20                            |
|轻量`*` 和增强试用版|5 |
{: caption="套餐详细信息" caption-side="top"}

`*` 轻量套餐服务实例中未使用的技能处于不活动状态的时间达到 30 天后，可能会将其删除以释放空间。

## 删除技能
{: #skill-add-delete}

可以删除可访问的任何技能，但助手正在使用的技能除外。如果技能正在使用中，那么必须从正在使用该技能的助手中将其除去，然后才能对其执行删除操作。

确保在删除技能之前，核实是否有其他任何人可能正在使用该技能。
{: tip}

要删除技能，请完成以下步骤：

1.  了解是否有任何助手正在使用该技能。在“技能”选项卡中，找到要删除的技能的磁贴。**助手**字段会列出当前使用技能的助手。

1.  如果要删除的技能与助手相关联，请通过完成以下步骤将其从助手中除去：

    - 从正在使用技能的助手中除去该技能之前，请与该助手的所有者核实。
    - 打开“助手”选项卡，然后单击以打开助手磁贴。
    - 找到要删除的技能的磁贴。单击 ![打开和关闭选项列表](images/kabob-beta.png) 图标，然后选择**除去**。
    - 对使用该技能的其他任何助手重复上述步骤。
    - 返回到“技能”选项卡，然后找到要删除的技能的磁贴。

1.  单击 ![打开和关闭选项列表](images/kabob-beta.png) 图标，然后选择**删除**。确认此删除操作。
