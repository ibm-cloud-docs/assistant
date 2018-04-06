---

copyright:
  years: 2015, 2018
lastupdated: "2018-04-06"

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

# Adding integrations
{: #add-integrations}

Add integrations to bring the assistant to your customers where they are when they need help.
{: shortdesc}

## Integration limits
{: #integration-limits}

The number of integrations you can create in a single service instance depends on your {{site.data.keyword.conversationshort}} plan.

| Service plan     | Integrations per assistant |
|------------------|---------------------------:|
| Premium          |                        100 |
| Standard         |                        100 |
| Lite*            |                        100 |
{: caption="Service plan details" caption-side="top"}

## Add an integration
{: #add-integration}

Follow these steps to add integrations to your assistant:

1.  Click the **Assistants** tab.

1.  Click to open the tile for the assistant that you want to deploy.

1.  Go to the Integrations section.

    **What is the Shareable Link integration?** After you add a conversational skill to an assistant, a test web site is provisioned for you automatically. It has a simple chat widget interface that you can use to interact with your assistant for testing purposes. You can also share the URL to this IBM-branded site with your teammembers.

1.  Click **Add Integration**.

1.  Click the **Select Integration** button for the channel with which you want to integrate the assistant. The options include:

    - [Facebook Messenger](deploy-facebook.html)
    - [Slack](deploy-slack.html)
    - [Shareable Link*](deploy-web-link.html)

      *One shareable link integration is already available. Only choose this option if you want to create another shareable link instance. To access the instance that was provisioned for you automatically, click the **Shareable Link** integration tile.

1.  Follow the instructions that are provided on the screen to complete the integration process.

After you integrate the assistant, test it from the target channel to ensure that the assistant works as expected.
