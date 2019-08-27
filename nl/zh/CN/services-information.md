---

copyright:
  years: 2015, 2019
lastupdated: "2019-07-19"

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

# IBM Cloud 服务信息
{: #services-information}

助手是由 {{site.data.keyword.cloud}} 管理的完全托管机器人，这意味着您无需担心用于支持助手的基础架构的设置或维护工作。
{: shortdesc}

## 服务套餐信息
{: #services-information-plans}

探索 {{site.data.keyword.conversationshort}} [服务套餐选项 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://www.ibm.com/cloud/watson-assistant/pricing/){: new_window}。

创建服务实例之前，请决定您要如何在 {{site.data.keyword.cloud_notm}} 帐户中组织资源。如果您未定义自己的资源组，那么将使用**缺省**资源组，并且日后*不能*对其进行更改。有关更多详细信息，请参阅[使用资源组来组织资源的最佳实践 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://cloud.ibm.com/docs/resources/bestpractice_rgs#bp_resourcegroups){: new_window}。所有用户都必须具有操作员平台访问角色。（{{site.data.keyword.conversationshort}} 不会利用服务访问角色。）

要查找当前实例所属的服务套餐，请完成以下步骤：

1.  记录当前正在使用的实例的名称。（可以从“技能”或“助手”主页面中查找和更改实例。）
1.  转至 [IBM Cloud 资源列表 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://cloud.ibm.com/resources) 页面。
1.  展开**服务**部分，找到您先前记录的实例名称，然后单击该名称以查看关联的套餐信息。

### 套餐限制（按工件类型）
{: #services-information-limits}

有关每个套餐的工件限制的信息，可从描述如何创建工件的主题中获取，以便您可以在需要了解这些限制时进行参考。下面是主题的链接：

- [助手](/docs/services/assistant?topic=assistant-assistant-add#assistant-add-limits)
- [对话节点](/docs/services/assistant?topic=assistant-dialog-build#dialog-build-node-limits)
- [实体](/docs/services/assistant?topic=assistant-entities#entities-limits)
- [不活动状态超时](/docs/services/assistant?topic=assistant-assistant-settings#assistant-settings-session-limits)
- [意向](/docs/services/assistant?topic=assistant-intents#intents-limits)
- [集成](/docs/services/assistant?topic=assistant-deploy-integration-add#deploy-integration-add-limits)
- [日志](/docs/services/assistant?topic=assistant-logs#logs-limits)
- [技能](/docs/services/assistant?topic=assistant-skill-add#skill-add-limits)
- [版本](/docs/services/assistant?topic=assistant-versions#versions-limits)

### API 调用限制
{: #services-information-api-limits}

每个实例允许的 API 调用数取决于服务套餐。请参阅套餐描述以获取详细信息。

如果使用的是轻量套餐，并达到了 API 调用限制，但日志显示您执行的调用数比预期少，请记住轻量套餐对日志信息仅存储 7 天。

如果要从一个套餐升级为另一个套餐，请参阅[升级](/docs/services/assistant?topic=assistant-upgrade)。

### 增强版和高端套餐功能 ![仅限增强版或高端套餐](images/plus.png)
{: #services-information-premium}

以下功能仅可供增强版或高端套餐的用户使用。

- [消歧](/docs/services/assistant?topic=assistant-dialog-runtime#dialog-runtime-disambiguation)
- [意向冲突解决](/docs/services/assistant?topic=assistant-intents#intents-resolve-conflicts)
- [意向建议和意向用户示例建议](/docs/services/assistant?topic=assistant-intent-recommendations)
- [Intercom 集成](/docs/services/assistant?topic=assistant-deploy-intercom)
- [搜索技能](/docs/services/assistant?topic=assistant-skill-search-add)

### 基于用户的套餐
{: #services-information-user-based-plans}

基于 API 的套餐根据在指定时间范围内发出的 API 调用数来度量使用量，与之不同的是，新的增强版套餐和更新的高端套餐使用基于用户的计费。它们根据在指定时间范围内与助手进行交互的唯一用户数来度量使用量。

{{site.data.keyword.conversationshort}} 将在 API 请求中检查以下信息（按所列顺序）以进行计费：

  1.  **user_id**：在 API 中定义的属性，用于在 /message API 调用的 context 对象中发送。使用此属性是确保准确地将 /message API 调用归于唯一用户的最佳方法。有关用户标识属性的更多信息，请参阅 API 参考文档：
  
    - `context.global.system.user_id`：[V2 API](https://cloud.ibm.com/apidocs/assistant-v2#send-user-input-to-assistant)
    - `context.metadata.user_id`：[V1 API](https://cloud.ibm.com/apidocs/assistant#get-response-to-user-input)

  1.  **session_id**：V2 API 中定义的属性，用于标识用户与助手之间的单个会话。会话标识在内置集成生成的 /message API 调用中提供。 用户关闭交谈窗口时或交谈处于不活动状态 60 分钟后，会话会结束。

  1.  **conversation_id**：在 V1 API 中定义的属性，存储在 /message API 调用的 context 对象中。此属性可用于标识与一个用户的单个会话交流关联的多个 /message API 调用。但是，仅当显式保留标识并将其随作为同一会话的一部分发出的每个请求传回时，才会使用相同的标识。否则，将为每个新的 /message API 调用生成新标识。

要最充分地利用新的基于用户的服务套餐，请将用来部署助手的任何定制应用程序设计成可捕获唯一用户标识或会话标识，并将这些信息传递到 {{site.data.keyword.conversationshort}}。

## 对 API 调用进行认证
{: #services-information-authenticate-api-calls}

服务实例使用的认证机制会影响在发出 API 调用时必须提供凭证的方式。

1.  获取服务凭证。

    - 在 [{{site.data.keyword.Bluemix_notm}} 资源列表 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://cloud.ibm.com){: new_window} 中查找并单击服务实例。

    - 单击以打开服务实例，单击**服务凭证**，然后单击**查看凭证**。

      **Cloud Foundry 凭证**

      ![显示 Cloud Foundry 托管的实例的“服务凭证”页面。](images/cf-cred-ui.png)

      **IAM 凭证**

      ![显示 IAM 托管的实例的“服务凭证”页面。](images/iam-creds.png)

1.  在 API 调用中使用这些凭证。

    **Cloud Foundry API 调用**

    提供您的用户名和密码凭证。

    ```curl
    curl -X GET \
    --user {username}:{password} \
    'https://gateway.watson.net/assistant/api/v1/workspaces?version=2018-09-20'
    ```
    {: codeblock}

     **IAM API 调用**

    - 基本 URL 必须包含位置。使用语法 `gateway-<location>.watsonplatform.net` 来指定在其中创建服务实例的位置。位置码列在*数据中心位置*表中。
    - 在头中提供相应类型的令牌。您可以传递不记名令牌或 API 密钥。

      - 令牌支持已认证的请求，而无需在每次调用中都嵌入服务凭证。以下示例显示的是使用不记名令牌的情况。

        ```curl
        curl -X GET \
        'https://gateway-syd.watsonplatform.net/assistant/api/v1/workspaces?version=2018-09-20' \
        --header 'Authorization: Bearer eyJhbGciOiJIUz......sgrKIi8hdFs'
        ```
        {: codeblock}

      - API 密钥使用基本认证。以下示例显示的是使用 API 密钥的情况。

        ```curl
        curl -X GET -u "apikey:3Df... ...Y7Pc9" \
        'https://gateway-us-east.watsonplatform.net/assistant/api/v1/workspaces?version=2018-09-20' \
        ```
        {: codeblock}

        使用任何 Watson SDK 时，可以传递 API 密钥，并让 SDK 来管理令牌的生命周期。
        {: note}

        IAM 资源无法使用 Cloud Foundry 命令行界面 (CLI) 进行管理。例如，用于创建或管理服务实例的 Cloud Foundry CLI 命令（以 `cf` 开头）无法使用在利用 IAM 的位置中托管的实例。必须改为使用 {{site.data.keyword.cloud_notm}} CLI 及其关联的命令。有关更多详细信息，请参阅[使用资源和资源组 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](/docs/cli/reference/ibmcloud?topic=cloud-cli-ibmcloud_commands_resource)。

        有关更多信息，请参阅[使用 IAM 令牌进行认证 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](/docs/services/watson?topic=watson-iam){: new_window}。

    有关示例，请参阅 API 参考中适用于您的语言的[认证 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://{DomainName}/apidocs/assistant-v2#authentication){: new_window}。

### 数据中心
{: #services-information-regions}

{{site.data.keyword.cloud_notm}} 拥有由全球数据中心构成的网络，这为其云服务提供了性能优势。有关更多详细信息，请参阅 [{{site.data.keyword.cloud_notm}} 全球数据中心 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://www.ibm.com/cloud/data-centers/){: new_window}。

{{site.data.keyword.cloud_notm}} 已从使用 Cloud Foundry 来管理用户访问权更改为使用基于令牌的 Identity and Access Management (IAM) 认证。IAM 在不同位置的应用时间不同。您可以迁移服务实例，以将其从当前 Cloud Foundry 组织和空间移至资源组。有关更多详细信息，请参阅[迁移](/docs/services/watson?topic=watson-migrate)。

可以创建在以下数据中心位置托管的 {{site.data.keyword.conversationshort}} 服务实例：

|位置|位置码|认证类型|IAM 采用日期|注释|
|-------------|---------------|---------------------|-------------------|-------|
|达拉斯|us-south|IAM|2018 年 10 月 30 日|不适用|
|法兰克福|eu-de|IAM|2018 年 10 月 30 日|不适用|
|悉尼  |au-syd|IAM|2018 年 5 月 7 日|在 5 月 7 日之前创建的实例已联合到达拉斯|
|东京|jp-tok|IAM|2018 年 11 月 8 日|不适用|
|伦敦|eu-gb，lon|IAM|2018 年 12 月 13 日|12 月 13 日之前在英国区域创建的实例已联合到美国南部区域|
|华盛顿|us-east|IAM|2018 年 6 月 14 日|不适用|
{: caption="数据中心位置" caption-side="top"}

有关托管其他 {{site.data.keyword.cloud_notm}} 服务的数据中心的信息，请参阅[服务（按区域）![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://cloud.ibm.com/docs/resources/services_region#services_region){: new_window}。

## 条款和安全性
{: #services-information-terms}

要了解有关服务条款和数据安全性的更多信息，请阅读以下信息：

- [服务条款 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://www-03.ibm.com/software/sla/sladb.nsf/sla/home?OpenDocument){: new_window}
- [Data security and privacy ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://www.ibm.com/software/sla/sladb.nsf/sla/csdsp?OpenDocument){: new_window}
- [信息安全](/docs/services/assistant?topic=assistant-information-security)

有关 {{site.data.keyword.cloud_notm}} 的更多信息，请参阅[平台概述 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](/docs/overview?topic=overview-whatis-platform){: new_window}。

## 仍然有疑问？ 
{: #services-information-sales}

请联系 [IBM 销售人员 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://www-01.ibm.com/marketing/iwm/dre/signup?source=urx-20970){: new_window}。
