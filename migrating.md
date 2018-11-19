---

copyright:
  years: 2015, 2018
lastupdated: "2018-11-19"

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

# Migrating

Migrate a {{site.data.keyword.conversationshort}} service instance to move it from its current Cloud Foundry org and space to a resource group.
{: shortdesc}

{{site.data.keyword.cloud_notm}} moved from using Cloud Foundry to using resource groups.

Resource groups offer these benefits over Cloud Foundry:

- Finer-grained access control by using IBM Cloud Identity and Access Management (IAM)
- Ability to connect service instances to apps and service across different regions
- Simplifies the capture of usage data per group

If you created service instances before November 2018, then depending on the location in which your instance is hosted, it might be using Cloud Foundry instead of resource groups. See [Data centers](services-information.html#regions) for information about when each location started to use IAM with new instances.

## Migrating a service instance
{: #migrate-steps}

To migrate your service instance, complete these steps:

1.  Determine which resource group you want to move the service instance to first.

    See [Best practices for organizing resource in a resource group ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://console.bluemix.net/docs/resources/bestpractice_rgs.html) for tips.

1.  From the IBM Cloud Dashboard services list, click the migrate icon ![Migrate](images/migrate.svg) for the instance you want to migrate, and then click **Migrate** from the popup.

    **Note**: Any service instances that were created in Sydney before 7 May 2018 were syndicated to Dallas. When you migrate a Sydney-based Cloud Foundry service instance, it will become a resource that is hosted in the Dallas data center. Any service instances created in Sydney on or after 7 May 2018 use resource groups already and do not need to be migrated.

1.  Click **Continue**, and then choose a resource group.

    You can create a resource group if you did not do so already. A **default** resource group is available. Take time to understand how the groups are used and create one if necessary. The resource group that you choose here *cannot* be changed later.

1.  Click **Migrate**.

For more information, see the [IBM Cloud migration documentation ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://console.bluemix.net/docs/resources/instance_migration.html).

## Authentication
{: #auth-support}

If you have existing applications that use basic authentication to access the service, then they can continue to pass a username and password to authenticate with the service instance after it is migrated.

Your new instance also supports authenticating with IBM Cloud Identity and Access Management (IAM), which is an enhanced mechanism that uses API keys instead of username and password credentials. Consider moving to using the new authentication method to take advantage of the improved security that it affords.
