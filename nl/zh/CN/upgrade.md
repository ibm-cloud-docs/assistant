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

# 升级
{: #upgrade}

了解如何升级服务套餐。
{: shortdesc}

## 升级套餐
{: #upgrade-plan}

您可以探索 {{site.data.keyword.conversationshort}} [服务套餐选项 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://www.ibm.com/cloud/watson-assistant/pricing/){: new_window} 来决定哪个套餐最适合您。

不能将基于 Cloud Foundry 的实例升级为增强版套餐。您必须迁移实例，使其使用资源组后，才能对实例进行升级。有关更多详细信息，请参阅[迁移](/docs/services/assistant?topic=assistant-migrate)。
{: note}

要升级套餐，请完成以下步骤：

1.  从 {{site.data.keyword.Bluemix_notm}} 菜单中，选择**升级套餐**。
    在此，可以查看当前套餐和其他可用的套餐选项并进行更改。

有关预订的常见问题的解答，请参阅[管理帐单和使用情况 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](/docs/billing-usage?topic=billing-usage-charges){: new_window}。

仍然有疑问？请联系 [IBM 销售 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://www-01.ibm.com/marketing/iwm/dre/signup?source=urx-20970){: new_window}。

## 升级对话技能
{: #upgrade-skill}

{{site.data.keyword.conversationshort}} 服务会定期添加和更新功能。虽然其中某些更改会自动应用于对话技能，但影响较大的更新仍需要对技能使用的机器学习模型进行手动更新。


仅当显示“升级”图标 (![“升级”图标](images/upgrade.png)) 时，才能对对话技能进行升级。

要升级对话技能，请完成以下步骤：

1.  [下载对话技能的副本](/docs/services/assistant?topic=assistant-skill-add#skill-add-download-skill)，然后将其作为新技能导入。
2.  升级对话技能的新副本。

    升级技能后，工具中会启用 API 的最新版本，并且“试用”窗格开始使用最新的功能。
3.  测试升级后的技能。
4.  在评估升级后的技能以了解升级将如何影响应用程序后，将升级应用到主对话技能。
5.  升级应用程序。要执行此操作，请更改应用程序用于指定最新 API 版本的消息 API 调用。有关 API 版本的详细信息，请参阅[发行说明](/docs/services/assistant?topic=assistant-release-notes)。
