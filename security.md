---

copyright:
  years: 2015, 2019
lastupdated: "2019-12-09"

subcollection: assistant

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:external: target="_blank" .external}
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

# Protecting sensitive information
{: #security}

Learn more about strategies that are available for you to use to ensure that your {{site.data.keyword.conversationshort}} data remains secure and protected in the cloud.
{: shortdesc}

![Premium plan only](images/premium0.png) The following features are available with Premium plans only.

## Bring your own key
{: #security-byok}

When you integrate with {{site.data.keyword.keymanagementservicefull}}, you can encrypt {{site.data.keyword.conversationshort}} data in Premium plan instances with ecryption keys that you create or import.

For implementation details, see [Protecting sensitive information in your {{site.data.keyword.watson}} service](/docs/services/watson?topic=watson-keyservice){: external}.

### Important BYOK notes
{: #security-byok-notes}

- Do not delete your key or your {{site.data.keyword.keymanagementserviceshort}} instance. The key is required to access your data and only you manage it. IBM cannot help you retrieve your data if you accidentally delete your key.
- You cannot delete the first {{site.data.keyword.conversationshort}} service instance that you create with your Premium plan. The access policies for your Premium instances are derived from the first instance that you add. In fact, each time you create an instance in this Premium plan, you must set the **Encrypted with a Key Protect root key** option to **Yes**, and then specify the key protect instance details for this plan.

For existing customers, a new slot must be provisioned for you before you can use {{site.data.keyword.keymanagementserviceshort}}, and your data must be migrated to it. You also must create a new resource group and use it with your new Premium instances.

## Private network endpoints
{: #security-private-endpoints}

You can set up a private network for {{site.data.keyword.conversationshort}} instances that are part of a Premium service plan. Using a private network prevents data from being transferred over the public internet, and ensures greater data isolation.

Private network endpoints support routing services over the {{site.data.keyword.cloud_notm}} private network instead of the public network. A private network endpoint provides a unique IP address that is accessible to you without a VPN connection.

For implementation details, see [Public and private network endpoints](/docs/services/watson?topic=watson-public-private-endpoints){: external}.

### Important private network endpoint notes
{: #security-private-endpoint-notes}

- The integrations that are provided with the product require endpoints that are available on the public internet. Therefore, any built-in integrations you add to your assistant will have public endpoints. If you only want to connect to a client application or messaging channel over the private network, then you must build your own custom client application or channel integration.
- Before you can create a search skill, you must create a {{site.data.keyword.discoveryshort}} instance with a private network endpoint. The list of {{site.data.keyword.discoveryshort}} instances that are displayed for you to connect to includes only instances with private network endpoints. Normally, when you create a search skill and do not have any {{site.data.keyword.discoveryshort}} instances, a Lite plan instance is provisioned for you automatically. No instance is provisioned for you when you are using a private network. You must create a {{site.data.keyword.discoveryshort}} instance with a private network endpoint first.

## Related topic
{: #security-related}

- **[Information security](/docs/services/assistant?topic=assistant-information-security)**: Describes strategies for complying with data protection regulations, such as GDPR and HIPAA. 