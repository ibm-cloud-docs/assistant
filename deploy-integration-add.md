---

copyright:
  years: 2015, 2019
lastupdated: "2019-02-11"

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

# Adding integrations
{: #deploy-integration-add}

To deploy your skill, add it to an assistant, and then add integrations to the assistant that publish your bot to the channels where your customers go for help.
{: shortdesc}

## Add an integration
{: #deploy-integration-add-task}

Follow these steps to add integrations to your assistant:

1.  Click the **Assistants** tab.

1.  Click to open the tile for the assistant that you want to deploy.

1.  Go to the Integrations section.

    **What is the Preview Link integration?** When you create an assistant, a test web site is provisioned for you automatically (unless you disable it). It has a simple chat widget interface that you can use to interact with your assistant for testing purposes. You can also share the URL to this IBM-branded site with your teammembers.

1.  Click **Add Integration**.

1.  Click the tile for the channel with which you want to integrate the assistant. The options include:

    - [Custom application](deploy-custom-app.html)
    - [Facebook Messenger](deploy-facebook.html)
    - [Intercom](deploy-intercom.html) ![Beta](images/beta.png) 
    - [Preview Link](deploy-web-link.html)
    - [Slack](deploy-slack.html)
    - [Voice Agent (Telephony)  ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://console.bluemix.net/catalog/services/voice-agent-with-watson){: new_window}

      Opens the **Voice Agent with Watson** service overview page on {{site.data.keyword.cloud_notm}}.
    - [WordPress plug-in ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://wordpress.org/plugins/conversation-watson/){: new_window}

      Opens the IBM Watson Assistant plug-in overview page on the WordPress site.

1.  Follow the instructions that are provided on the screen to complete the integration process.

After you integrate the assistant, test it from the target channel to ensure that the assistant works as expected.

For tips on how to start the dialog in a consistent way across all integration types, see [Starting the dialog](dialog-start.html).

## Integration limits
{: #deploy-integration-add-limits}

The number of integrations you can create in a single service instance depends on your {{site.data.keyword.conversationshort}} plan.

| Service plan     | Integrations per assistant |
|------------------|---------------------------:|
| Premium          |                        100 |
| Plus             |                        100 |
| Standard         |                        100 |
| Lite             |                        100 |
{: caption="Service plan details" caption-side="top"}
