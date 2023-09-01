---

copyright:
  years: 2019, 2021
lastupdated: "2021-06-16"

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

{{site.data.content.newlink}}

# Securing your connection
{: #security}

Learn more about strategies that are available for you to use to ensure that your {{site.data.keyword.assistant_classic_short}} data remains secure and protected in the cloud.
{: shortdesc}

For more information about {{site.data.keyword.Bluemix_notm}} security, see [Security architecture](https://www.ibm.com/cloud/architecture/architecture/practices/watson-security-architecture/){: external}.



## Private network endpoints
{: #security-private-endpoints}

You can set up a private network for {{site.data.keyword.assistant_classic_short}} instances that are part of a Plus or Enterprise service plan. Using a private network prevents data from being transferred over the public internet, and ensures greater data isolation.

![Plus or higher plans only](images/plus.png) This feature is available only to users of paid plans.

Private network endpoints support routing services over the {{site.data.keyword.cloud_notm}} private network instead of the public network. A private network endpoint provides a unique IP address that is accessible to you without a VPN connection.

For implementation details, see [Public and private network endpoints](/docs/watson?topic=watson-public-private-endpoints){: external}.

### Important private network endpoint notes
{: #security-private-endpoint-notes}

- The integrations that are provided with the product require endpoints that are available on the public internet. Therefore, any built-in integrations you add to your assistant will have public endpoints. If you only want to connect to a client application or messaging channel over the private network, then you must build your own custom client application or channel integration.
- Before you can create a search skill, you must create a {{site.data.keyword.discoveryshort}} instance with a private network endpoint. The list of {{site.data.keyword.discoveryshort}} instances that are displayed for you to connect to includes only instances with private network endpoints. Normally, when you create a search skill and do not have any {{site.data.keyword.discoveryshort}} instances, a Lite plan instance is provisioned for you automatically. No instance is provisioned for you when you are using a private network. You must create a {{site.data.keyword.discoveryshort}} instance with a private network endpoint first.

## Related topics
{: #security-related}

- [Information security](/docs/assistant?topic=assistant-information-security): Describes strategies for complying with data protection regulations, such as GDPR and HIPAA. 
[Security architecture](https://www.ibm.com/cloud/architecture/architecture/practices/watson-security-architecture/){: external}: Describes the security components that are needed for secure cloud development, deployment, and operations.
- [IBM Cloud compliance programs](https://www.ibm.com/cloud/compliance){: external}: Describes how to manage regulatory compliance and internal governance requirements with IBM Cloud services.