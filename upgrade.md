---

copyright:
  years: 2015, 2021
lastupdated: "2021-09-21"

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

# Upgrading
{: #upgrade}

Learn how to upgrade your service plan.
{: shortdesc}

## Upgrading your plan
{: #upgrade-plan}

You can explore the {{site.data.keyword.conversationshort}} [service plan options](https://www.ibm.com/cloud/watson-assistant/pricing/){: external} to decide which plan is best for you.

The page header shows the plan you are using today.

![Shows that the Trial plan is displayed in the page header](images/plan-in-header.png)

To upgrade your plan, complete these steps:

1. Do one of the following things:

   - **Trial plan only**: The number of days that are left in your trial is displayed in the page header. To upgrade your plan, click **Upgrade** from the page header before your trial period ends.
   - For all other plan types, click the *User* icon ![user icon](images/user-icon.png) from the page header, and then choose **Upgrade Plan** from the menu.

1. From here, you can see other available plan options. For most plan types, you can step through the upgrade process yourself.

    - If you upgrade to an Enterprise with Data Isolation plan, you cannot do an in-place upgrade of your service instance. An Enterprise with Data Isolation plan instance must be provisioned for you first. After the service instance is provisioned, you must export your dialog skills from the old plan instance, and import them into the new plan instance. For more information, see [Backing up and restoring data](/docs/assistant?topic=assistant-backup).

    - When you upgrade from a legacy Standard plan, you change the metrics that are used for billing purposes. Instead of basing billing on API usage, the Plus plan bases billing on the number of monthly active users. If you built a custom app to deploy your assistant, you might need to update the app. Ensure that the API calls from the app include user ID information. For more information, see [User-based plans explained](/docs/assistant?topic=assistant-services-information#services-information-user-based-plans).

    - You cannot change from a Trial plan to a Lite plan.

For answers to common questions about subscriptions, see the [How you're charged](/docs/billing-usage?topic=billing-usage-charges){: external}.
