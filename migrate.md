---

copyright:
  years: 2015, 2019
lastupdated: "2019-04-02"

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

# Migrating from Cloud Foundry
{: #migrate}

Migrate a {{site.data.keyword.conversationshort}} service instance to move it from its current Cloud Foundry org and space to a resource group.
{: shortdesc}

{{site.data.keyword.cloud_notm}} moved from using Cloud Foundry to using resource groups.

Resource groups offer these benefits over Cloud Foundry:

- Finer-grained access control by using IBM Cloud Identity and Access Management (IAM)
- Ability to connect service instances to apps and service across different regions
- Simplifies the capture of usage data per group

If you created service instances before November 2018, then depending on the location in which your instance is hosted, it might be using Cloud Foundry instead of resource groups. See [Data centers](/docs/services/assistant?topic=assistant-services-information#services-information-regions) for information about when each location started to use IAM with new instances.

## Migrating a service instance
{: #migrate-task}

If you want to learn more about the migration process before you begin, see the [IBM Cloud migration documentation ![External link icon](../../icons/launch-glyph.svg "External link icon")](/docs/resources?topic=resources-migrate).

Only the person or group that created the instance or was given `Developer` role access to the instance can migrate it.
{: note}

To migrate your service instance, complete these steps:

1.  Determine which resource group you want to move the service instance to first.

    See [Best practices for organizing resource in a resource group ![External link icon](../../icons/launch-glyph.svg "External link icon")](/docs/resources?topic=resources-bp_resourcegroups) for tips.

    You can use one service instance to develop, test, and deploy an assistant, so do not create distinct resource groups for different types of deployment environments.
    {:tip}

    With a Premium plan, you can create multiple instances. However, the instances must all be created in the same resource group.

1.  From the IBM Cloud Dashboard services list, click the migrate icon ![Migrate](images/migrate.svg) for the instance you want to migrate, and then click **Migrate** from the popup.

    Any service instances that were created in the Sydney data center before 7 May 2018 or in the London data center before 13 December 2018 were syndicated to the Dallas data center. When you migrate a Sydney- or London-based Cloud Foundry service instance, it is converted to a resource that is hosted in Dallas.
    {: note}

1.  Click **Continue**, and then choose a resource group.

    If you have not created a resource group, you can create one now. A **default** resource group is available. Take time to understand how the groups are used and create one if necessary. The resource group that you choose here *cannot* be changed later.

1.  Click **Migrate**.

    A message is displayed when the process is done. If you have other service instances to migrate, you can continue migrating other service instances, or click **Done**.

1.  **Premium plan one-time step**: If the service instance you are migrating was created as part of a Premium plan, you must inform the service team that you are migrating a Premium plan instance. To do so, create a case from [IBM Cloud Support ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://cloud.ibm.com/unifiedsupport/supportcenter){: new_window}.

    There are some additional steps that the service team needs to take on your behalf. Add the following information to the case:

    - Region in which the service instance is hosted, such as Dallas or Frankfurt.
    - Resource group name
    - Resource group ID

      If you don't know the ID, open the IBM Cloud command line interface (CLI) tool and enter the following command:

      ```bash
      ibmcloud resource groups
      ```
      {: codeblock}

      The response shows the ID of the resource group. For more information about the CLI command, see [Working with resources and resource groups ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://cloud.ibm.com/docs/cli?topic=cloud-cli-ibmcloud_commands_resource#ibmcloud_commands_resource).

      Give the support team up to 24 hours from the time you submit the case to complete the work they need to perform to support the Premium plan migration. There is no disruption of service during the migration process.

      You only need to perform this step the first time you migrate a service instance for the Premium plan. When you click *Migrate* for other service instances that belong to the same plan, they will be migrated to the same resource group without requiring any involvement from the service team.
      {: important}

The old (Cloud Foundry org-based) service instance that you migrated continues to be listed in the Cloud Foundry Services section of the Dashboard, and is now shown as an *alias* of the new (resource group-based) version of the instance.

![Shows current service instance is now an alias of a resource-based instance](images/alias.png)

For more information about aliases, see the [IBM Cloud Connections documentation ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://cloud.ibm.com/docs/resources/connecting_apps#what_is_alias).

You must open the new, resource group-based version of the service instance to access the **Launch tool** button. The new instance is listed in the Services section of the {{site.data.keyword.Bluemix_notm}} Dashboard.

## Authentication
{: #migrate-auth-support}

If you have existing applications that use basic authentication to access the service, then they can continue to pass a username and password to authenticate with the service instance after it is migrated. You can see the credential information from the **Service credentials** page of the alias.

Your new instance manages authentication with IBM Cloud Identity and Access Management (IAM), which is an enhanced mechanism that uses API keys instead of username and password credentials. You can see the API key information from the **Service credentials** page of the new service instance.

Consider updating your existing custom applications such that they use the new authentication method to take advantage of the improved security that it affords. Any changes that you make to the dialog skills in the new instance are reflected in applications that use basic authentication credentials also. After you update all of your apps to use the new API key approach, you won't need the alias and can delete it.
