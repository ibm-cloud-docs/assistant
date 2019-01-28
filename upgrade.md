---

copyright:
  years: 2015, 2019
lastupdated: "2019-01-22"

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

# Upgrading
{: #upgrade}

Learn how to upgrade your service plan.
{: shortdesc}

## Upgrading your plan
{: #plan-upgrade}

You can explore the {{site.data.keyword.conversationshort}} [service plan options ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/cloud/watson-assistant/pricing/){: new_window} to decide which plan is best for you.

You cannot upgrade a Cloud Foundry-based instance to a Plus plan. You must migrate the instance, so it is using a resource group before you can upgrade it. See [Migrating](migrate.html) for more details.
{: note}

To upgrade your plan, complete these steps:

1.  From the {{site.data.keyword.Bluemix_notm}} menu, select **Upgrade Plan**.
    From here, you can see your current plan and other available plan options, and make changes.

For answers to common questions about subscriptions, see the [Managing billing and usage ![External link icon](../../icons/launch-glyph.svg "External link icon")](/docs/billing-usage/how_charged.html){: new_window}.

Still have questions? Contact [IBM Sales ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www-01.ibm.com/marketing/iwm/dre/signup?source=urx-20970){: new_window}.

## Upgrading a dialog skill
{: #upgrade-skill}

The {{site.data.keyword.conversationshort}} service regularly adds and updates features. While some of these changes are automatically applied to your dialog skills, updates that have a large impact do require a manual update.

An upgrade is only available for your dialog skill if the upgrade icon (![upgrade icon](images/upgrade.png)) is displayed.

After you upgrade a dialog skill, you cannot revert your skill to a previous version.
{: note}

To upgrade your dialog skill, complete the following steps:

1.  [Download a copy of your dialog skill](create-skill.html#download-skill), and then import it as a new skill.
2.  Upgrade the new copy of your dialog skill.

    When you upgrade your skill, the latest version of the API is enabled in the tool, and the "Try it out" pane begins to use the newest features.
3.  Test the upgraded skill.
4.  After evaluating the upgraded skill to understand how the upgrade will impact your application, apply the upgrade to your primary dialog skill.
5.  Upgrade your application. To do so, change the message API calls it uses to specify the latest API version. For API version details, see the [release notes](release-notes.html).
