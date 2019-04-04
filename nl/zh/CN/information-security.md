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

# 信息安全
{: #information-security}

IBM 致力于为客户和合作伙伴提供创新的数据隐私、安全和监管解决方案。
{: shortdesc}

**声明：**
客户负责确保自身遵守各种法律和法规，包括《欧盟一般数据保护条例》(GDPR)。客户须自行负责从合格的法律顾问那里，就可能会影响客户业务和客户为了遵守此类法律和法规需要采取的任何行动，获得关于任何相关法律和法规的认定和解释的意见。

此处描述的产品、服务和其他功能并不适用于所有客户情形，并且在可用性方面可能有限制。IBM 不会提供法律、会计或审计建议，也不会表述或保证其服务或产品将确保客户遵守任何法律或法规。

如果需要为创建的 {{site.data.keyword.cloud}} {{site.data.keyword.watson}} 资源请求 GDPR 支持

- 在欧盟，请参阅 [Requesting support for IBM Cloud Watson resources created in the European Union ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://cloud.ibm.com/docs/services/watson/getting-started-gdpr-sar#request-EU){: new_window}。
- 在欧盟以外的地区，请参阅 [Requesting support for resources outside the European Union ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://cloud.ibm.com/docs/services/watson/getting-started-gdpr-sar#request-non-EU){: new_window}。

## 欧盟一般数据保护条例 (GDPR)
{: #information-security-gdpr}

IBM 致力于为客户和合作伙伴提供创新的数据隐私、安全和监管解决方案，以协助他们完成 GDPR 合规旅程。

在[此处 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](../../icons/launch-glyph.svg "外部链接图标")](http://www.ibm.com/gdpr) 了解有关 IBM 自己的 GDPR 就绪性旅程以及支持您合规旅程的 GDPR 功能和产品的更多信息{: new_window}。

## 标注和删除 {{site.data.keyword.conversationshort}} 中的数据
{: #information-security-gdpr-wa}

不要向创建的训练数据（实体和意向，包括用户示例）添加个人数据。特别是，确保从上传用于挖掘用户示例建议的包含实际用户发声的文件中，除去所有个人可标识信息。

**注：**试验性和 Beta 功能不适用于生产环境，因此在标注和删除数据时，不保证能按预期起作用。在实现需要标注和删除数据的解决方案时，不应使用试验性和 Beta 功能。

如果需要从 {{site.data.keyword.conversationshort}} 实例中除去客户的消息数据，可以根据客户的客户标识来执行此操作，但条件是在消息发送到服务时，将消息与客户标识相关联。

**注：**“预览链接”和自动 Facebook 集成功能不支持标记，因此会根据客户标识来删除数据。在需要能够根据客户标识进行删除的解决方案中，不应使用这些功能。

### 开始之前
{: #information-security-delete-user-data-prereqs}

要能够删除与特定用户关联的消息数据，必须首先将所有消息与每个用户的唯一**客户标识**相关联。要为使用 `/message` API 发送的任何消息指定**客户标识**，请在头中包含 `X-Watson-Metadata: customer_id` 属性。例如：

```
curl -X POST -u "apikey:3Df... ...Y7Pc9"
 --header
   'Content-Type: application/json'
   'X-Watson-Metadata: customer_id=abc'
 --data
   '{"input":{"text":"hello"}}'
  'https://gateway-eu-de.watsonplatform.net/assistant/api/v1/workspaces/{workspaceID}/message?version=2018-09-20'
```
{: codeblock}

`customer_id` 字符串不能包含分号 (`;`) 或等号 (`=`) 字符。确保每个`客户标识`属性在您的客户中唯一是您的责任。
{: note}

可以使用分号分隔的 `customer_id={value}` 对来传递多个**客户标识**值。例如：`'X-Watson-Metadata: customer_id=abc;customer_id=xyz'`

如果向助手添加了搜索技能，那么提交给助手的用户输入将作为搜索查询传递到 {{site.data.keyword.discoveryshort}} 服务。如果 {{site.data.keyword.conversationshort}} 集成提供了客户标识，那么生成的 /message API 请求将在头中包含该客户标识，并且会将该标识传递到 {{site.data.keyword.discoveryshort}} /query API 请求。要删除与特定客户关联的任何查询数据，必须将删除请求直接发送到与助手链接的 {{site.data.keyword.discoveryshort}} 服务实例。有关详细信息，请参阅 {{site.data.keyword.discoveryshort}} [信息安全](https://cloud.ibm.com/docs/services/discovery/information-security#gdpr-discovery)主题。

### 查询用户数据
{: #information-security-query-customer-id}

使用 V1 `/logs` 方法 `filter` 参数来搜索应用程序日志以查找特定用户数据。例如，要搜索特定于与 `my_best_customer` 匹配的 `customer_id` 的数据，查询可能如下：

``` curl
curl -X GET -u "apikey:3Df... ...Y7Pc9"
'https://gateway-eu-de.watsonplatform.net/assistant/api/v1/workspaces/{workspaceID}/logs?version=2018-09-20&filter=customer_id::my_best_customer'
```
{: codeblock}

有关其他详细信息，请参阅[过滤器查询参考](/docs/services/assistant?topic=assistant-filter-reference)。

### 删除数据
{: #information-security-delete-data}

要删除服务可能已存储的与特定用户关联的任何消息日志数据，请使用 `DELETE /user_data` V1 API 方法。通过随请求一起传递 `customer_id` 参数，指定用户的客户标识。

使用此删除方法只能删除通过包含关联客户标识的 `POST /message` API 端点添加的数据。无法根据客户标识删除其他方法添加的数据。例如，无法通过这种方式来删除从客户会话中添加的实体和意向。这些方法不支持个人数据。

**重要信息**：指定 `customer_id` 将删除整个 {{site.data.keyword.conversationshort}} 实例中在删除请求之前收到的包含该 `customer_id` 的*所有*消息，而不仅仅是删除一个技能中的消息。

例如，要从 {{site.data.keyword.conversationshort}} 实例中删除与客户标识为 `abc` 的用户关联的所有消息数据，请发送以下 cURL 命令：

```
curl -X DELETE -u "apikey:3Df... ...Y7Pc9"
 'https://gateway-eu-de.watsonplatform.net/assistant/api/v1/user_data?customer_id=abc&version=2018-09-20'
```
{: codeblock}

这将返回空的 JSON 对象 `{}`。

有关更多信息，请参阅 [API 参考](https://cloud.ibm.com/apidocs/assistant?curl=#delete-labeled-data)。

**注：**删除请求是批量处理的，因此可能最长需要 24 小时才能完成。
