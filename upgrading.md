---

copyright:
  years: 2015, 2018
lastupdated: "2018-04-26"

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

# Upgrading

Learn how to upgrade your service plan.
{: shortdesc}

## Upgrading your plan
{: #plan-upgrade}

You can explore the {{site.data.keyword.conversationshort}} [service plan options ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://console.bluemix.net/catalog/services/watson-assistant-formerly-conversation){: new_window} to decide which plan is best for you.

To upgrade your plan, complete these steps:

1.  From the {{site.data.keyword.Bluemix_notm}} menu, select **Services** > **Dashboard**.
1.  Select the service instance that you want to upgrade to open it.
1.  Click **Plan** from the navigation pane.
   From here, you can see your current plan and other available plan options, and make changes.

For answers to common questions about subscriptions, see the [Managing billing and usage ![External link icon](../../icons/launch-glyph.svg "External link icon")](/docs/billing-usage/how_charged.html){: new_window}.

Still have questions? Contact [IBM Sales ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www-01.ibm.com/marketing/iwm/dre/signup?source=urx-20970){: new_window}.

## Upgrading a conversational skill
{: #upgrade-skill}

The {{site.data.keyword.conversationshort}} service regularly adds and updates features. While some of these changes are automatically applied to your conversational skills, updates that have a large impact do require a manual update.

An upgrade is only available for your conversational skill if the upgrade icon (![upgrade icon](images/upgrade.png)) is displayed.

**Note**: After you upgrade a conversational skill, you cannot revert your skill to a previous version.

To upgrade your conversational skill, complete the following steps:

1.  [Duplicate your conversational skill](create-convo-skill.html#exporting-and-copying-skills).
2.  Upgrade the duplicate conversational skill.

    When you upgrade your skill, the latest version of the API is enabled in the tool, and the "Try it out" pane begins to use the newest features.
3.  Test the upgraded skill.
4.  After evaluating the duplicate skill to understand how the upgrade will impact your application, apply the upgrade to your primary conversational skill.
5.  Upgrade your application. To do so, change the message API calls it uses to specify the latest API version. For API version details, see the [release notes](release-notes.html).
