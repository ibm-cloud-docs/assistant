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

# 创建技能版本
{: #versions}

版本可帮助您管理对话技能开发项目的工作流程。
{: shortdesc}

创建技能版本可捕获开发过程中关键点的技能中的训练数据（意向和实体）和对话的快照。开始优化助手时，能够保存特定时间点的进行中技能特别有用。您通常需要进行更改并实时查看更改产生的影响，然后才能确定更改是提高还是降低了助手的有效性。根据来自测试环境部署的发现结果，可以就是否将给定更改部署到生产环境中所部署的助手做出明智的决策。

如果您有免费（轻量）套餐，那么无法创建技能版本。
{: note}

这一为时 2 分半钟的视频描述了使用版本对您有哪些帮助。

<iframe class="embed-responsive-item" id="youtubeplayer" title="创建技能版本" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/FDolnBxvcZ8" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

## 创建版本
{: #versions-create}

使用 {{site.data.keyword.conversationshort}} 工具一次只能编辑对话技能的一个版本。进行中的版本称为*开发*版本。

保存版本时，还会保存应用于开发版本的任何技能设置。

要创建对话技能版本，请执行以下步骤：

1.  在技能的标题中，单击**保存新版本**，然后描述技能的当前状态。

    添加适当的描述可帮助您日后区分多个版本。

1.  单击**保存**。

将创建当前技能的快照，并另存为新版本。您将保持在技能的开发版本中。您所做的任何更改都将继续应用于开发版本，而不会应用于保存的版本。要访问保存的版本，请转至**版本历史记录**页面。

## 部署技能版本
{: #versions-deploy}

1.  在技能的标题中，单击**版本历史记录**选项卡。
1.  单击要部署的版本中的 ![单击以查看操作](images/kebab-react.png) 图标，然后选择**分配给助手**。

    这将显示可以将此版本链接到的助手的列表。该列表仅限于没有关联任何技能的助手或关联有此技能其他版本的助手。
1.  单击一个或多个助手的复选框，然后单击**分配**。

跟踪此版本何时部署到助手以及持续了多长时间。您可能希望分析用户与此特定技能版本之间发生的用户会话。可以从**分析**页面获取此信息。但是，选取数据源时，不会列出版本。您必须选择已部署此版本的助手的名称。然后，可以过滤度量值数据，以仅显示此技能版本部署到助手期间的时间范围内开始日期和结束日期之间发生的会话。
{: important}

## 技能版本限制
{: #skill-version-limits}

可以为单个技能创建的版本数取决于 {{site.data.keyword.conversationshort}} 套餐。

|服务套餐          |每个技能的版本数    |
|------------------|-------------------:|
|高级                                 |50|
|增强版            |10|
|标准                                 |10|
|Lite              |0 |
{: caption="服务套餐详细信息" caption-side="top"}

## 交换技能
{: #versions-swap-skills}

如果发现技能的较早版本比后来的版本能更好地识别和解决客户需求，那么可以交换链接到助手的技能以改为使用较早的版本。

执行用于[部署技能版本](#versions-deploy)的相同步骤来更改链接到助手的版本。

## 访问保存的版本
{: #versions-view}

查看已保存版本的唯一方法是使用技能的已保存版本来覆盖其进行中开发版本。（但不要在保存在当前开发版本中完成的任何工作之前覆盖。）

您无法编辑保存的版本。要实现这一目标，可以基于保存的版本创建新版本，在新版本中包含要进行的任何更改。要从保存的版本开始开发工作，请使用技能的已保存版本来覆盖其进行中开发版本。

1.  保存自上次创建版本以来对技能所做的任何更改。

    请立即保存技能的版本。否则，执行以下步骤时，您的工作将丢失。
    {: important}

1.  在要编辑的版本中，单击**技能操作** ![技能操作](images/kebab-react.png) 图标，然后选择**复制到开发**并确认操作。

    页面将刷新以还原为创建版本时技能所处于的状态。

如果要保存对此版本所做的任何更改，必须将技能保存为新版本。不能将更改应用于已保存的版本。

通过单击助手页面中的技能磁贴打开技能时，将显示该技能的开发版本。即便已将更高版本与助手相关联，访问该技能时，也仍会打开其开发版本。
{: important}
