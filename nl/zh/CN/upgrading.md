---

copyright:
  years: 2015, 2018
lastupdated: "2018-02-09"

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

# 升级

了解如何升级服务套餐。
{: shortdesc}

## 升级套餐
{: #plan-upgrade}

使用 Lite 套餐可以广泛试用 {{site.data.keyword.conversationshort}} 服务。但可以执行的操作存在限制。


### 工件限制
有关每个套餐的工件限制的信息，请参阅以下主题：

- [工作空间](configure-workspace.html#workspace-limits)
- [对话节点](dialog-build.html#dialog-node-limits)
- [意向](intents.html#intent-limits)
- [实体](entities.html#entity-limits)
- [日志](logs_convo.html#log-limits)

### API 调用限制
每个实例允许的 API 调用数取决于服务套餐。请参阅套餐描述以获取详细信息。

如果使用的是 Lite 套餐，并达到了 API 调用限制，但日志显示的调用数少于预期，请记住 Lite 套餐对日志信息仅存储 7 天。

要升级套餐，请完成以下步骤：

1.  在 {{site.data.keyword.Bluemix_notm}} 菜单中，选择**服务** > **仪表板**。
1.  选择要升级的服务实例以将其打开。
1.  在导航窗格中，单击**套餐**。在此，可以查看当前套餐和其他可用的套餐选项并进行更改。

有关预订的常见问题的解答，请参阅[管理帐单和使用情况 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](/docs/billing-usage/how_charged.html){: new_window}。

通过访问以下链接，可以了解有关 IBM Cloud 托管的服务的更多信息：

- [云服务条款](http://www.ibm.com/software/sla/sladb.nsf/sla/saas)
- [云服务数据安全与隐私](http://www.ibm.com/software/sla/sladb.nsf/sla/csdsp)

## 升级工作空间
{: #upgrade-workspace}

{{site.data.keyword.conversationshort}} 服务会定期添加和更新功能。虽然其中某些更改会自动应用于工作空间，但那些影响较大的更新仍需要手动进行。


仅当显示“升级”图标 (![“升级”图标](images/upgrade.png)) 时，才能对工作空间进行升级。

**注**：升级工作空间后，即无法将工作空间还原为先前版本。

要升级工作空间，请完成以下步骤：
1.  [复制工作空间](configure-workspace.html#exporting-and-copying-workspaces)。
2.  升级复制的工作空间。

    升级工作空间后，工具中会启用 API 的最新版本，并且“试用”窗格开始使用最新的功能。
3.  测试升级后的工作空间。
4.  在评估重复工作空间以了解升级将如何影响应用程序后，将升级应用到主工作空间。
5.  通过将消息 API 调用更改为使用最新的 API 版本来升级应用程序。
