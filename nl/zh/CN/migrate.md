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

# 迁移
{: #migrate}

迁移 {{site.data.keyword.conversationshort}} 服务实例，以将其从当前 Cloud Foundry 组织和空间移至资源组。
{: shortdesc}

{{site.data.keyword.cloud_notm}} 已从使用 Cloud Foundry 移至使用资源组。

相对于 Cloud Foundry，资源组具有以下优点：

- 使用 IBM Cloud Identity and Access Management (IAM) 可实现更细颗粒度的访问控制
- 能够将服务实例连接到不同区域的应用程序和服务
- 简化了对每个组的使用情况数据的捕获

如果您在 2018 年 11 月之前创建了服务实例，那么根据托管实例的位置，该实例可能使用的是 Cloud Foundry，而不是资源组。有关每个位置何时开始将 IAM 用于新实例的信息，请参阅[数据中心](/docs/services/assistant?topic=assistant-services-information#services-information-regions)。

## 迁移服务实例
{: #migrate-task}

如果要在开始之前了解有关迁移过程的更多信息，请参阅 [IBM Cloud 迁移文档 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](/docs/resources?topic=resources-migrate)。

只有创建实例的人员或组，或被授予了对实例的`开发者`角色访问权的人员或组，才能迁移该实例。
{: note}

要迁移服务实例，请完成以下步骤：

1.  首先确定要将服务实例移至的资源组。

    有关提示，请参阅[使用资源组来组织资源的最佳实践 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](/docs/resources?topic=resources-bp_resourcegroups)。

1.  在 IBM Cloud“仪表板”的服务列表中，单击要迁移的实例的“迁移”图标 ![迁移](images/migrate.svg)，然后单击弹出窗口中的**迁移**。

    2018 年 5 月 7 日前在悉尼数据中心创建的任何服务实例，或 2018 年 12 月 13 日之前在伦敦数据中心内创建的任何服务实例，都已联合到达拉斯数据中心。迁移基于悉尼或基于伦敦的 Cloud Foundry 服务实例时，会将其转换为在达拉斯托管的资源。
    {: note}

1.  单击**继续**，然后选择资源组。

    如果尚未创建资源组，那么现在可以创建资源组。提供了**缺省**资源组。请花时间来了解如何使用资源组，并根据需要创建资源组。在此处选择的资源组日后*不能*更改。

1.  单击**迁移**。

    迁移过程完成时，将显示一条消息。如果有其他服务实例要迁移，可以继续迁移其他服务实例，否则请单击**完成**。

已迁移的旧服务实例（基于 Cloud Foundry 组织的服务实例）会继续列在仪表板的“Cloud Foundry 服务”部分中，但现在会显示为新版本（基于资源组的版本）实例的*别名*。

![显示当前服务实例现有为基于资源的实例的别名](images/alias.png)

有关别名的更多信息，请参阅 [IBM Cloud 连接文档 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://cloud.ibm.com/docs/resources/connecting_apps#what_is_alias)。

您必须打开服务实例的基于资源组的新版本，才能访问**启动工具**按钮。新实例会列在 {{site.data.keyword.Bluemix_notm}}“仪表板”的“服务”部分中。

## 认证
{: #migrate-auth-support}

如果您有使用基本认证来访问服务的现有应用程序，那么在服务实例迁移后，这些应用程序可以继续通过传递用户名和密码来向服务实例进行认证。您可以在别名的**服务凭证**页面中查看凭证信息。

新实例会管理通过 IBM Cloud Identity and Access Management (IAM) 进行的认证，这是一种增强的机制，使用的是 API 密钥，而不是用户名和密码凭证。您可以在新服务实例的**服务凭证**页面中查看 API 密钥信息。

请考虑更新现有定制应用程序，使这些应用程序可使用新的认证方法，以利用它所能提供的改进后的安全性。对新实例中对话技能所做的任何更改，都会同时反映在使用基本认证凭证的应用程序中。将所有应用程序都更新为使用新的 API 密钥方法后，就不再需要别名，因此可以将其删除。
