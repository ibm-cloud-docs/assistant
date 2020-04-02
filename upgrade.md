---

copyright:
  years: 2015, 2020
lastupdated: "2020-04-02"

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

# Upgrading
{: #upgrade}

Learn how to upgrade your service plan.
{: shortdesc}

## Upgrading your plan
{: #upgrade-plan}

You can explore the {{site.data.keyword.conversationshort}} [service plan options](https://www.ibm.com/cloud/watson-assistant/pricing/){: external} to decide which plan is best for you.

You cannot upgrade a Cloud Foundry-based instance to a Plus plan. You must migrate the instance, so it is using a resource group before you can upgrade it. See [Migrating from Cloud Foundry](/docs/watson?topic=watson-migrate) for more details.
{: note}

To upgrade your plan, complete these steps:

1. From the header of any page in the current instance, click the User icon ![user icon](images/user-icon.png).
1.  Choose **Upgrade Plan** from the menu.

    From here, you can see your current plan and other available plan options. Depending on the plan type, you can make changes yourself or inform IBM that you want to change your plan.

    - If you upgrade to a Premium plan, you cannot do an in-place upgrade of your service instance. A Premium plan instance must be provisioned for you first. After the service instance is provisioned, you must export your dialog skills from the old plan instance, and import them into the new plan instance. For more information, see [Backing up and restoring data](/docs/assistant?topic=assistant-backup).

    - When you upgrade from a Standard plan to a Plus plan, you change the metrics that are used for billing purposes. Instead of basing billing on API usage, the Plus plan bases billing on the number of monthly active users. If you built a custom app to deploy your assistant, evaluate whether you need to update the app to ensure that the API calls pass user ID information. For more information, see [User-based plans explained](/docs/assistant?topic=assistant-services-information#services-information-user-based-plans).

For answers to common questions about subscriptions, see the [How you're charged](/docs/billing-usage?topic=billing-usage-charges){: external}.

Still have questions? Contact [IBM Sales](https://www.ibm.com/account/reg/us-en/subscribe?formid=urx-20970){: external}.
