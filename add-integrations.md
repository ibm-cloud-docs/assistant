---

copyright:
  years: 2015, 2018
lastupdated: "2018-11-02"

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

To deploy your skill, add it to an assistant, and then add integrations to the assistant that publish your bot to the channels where your customers go for help.
{: shortdesc}

## Integration limits
{: #integration-limits}

The number of integrations you can create in a single service instance depends on your {{site.data.keyword.conversationshort}} plan.

| Service plan     | Integrations per assistant |
|------------------|---------------------------:|
| Premium          |                        100 |
| Standard         |                        100 |
| Lite             |                        100 |
{: caption="Service plan details" caption-side="top"}

## Add an integration
{: #add-integration}

Follow these steps to add integrations to your assistant:

1.  Click the **Assistants** tab.

1.  Click to open the tile for the assistant that you want to deploy.

1.  Go to the Integrations section.

    **What is the Preview Link integration?** When you create an assistant, a test web site is provisioned for you automatically (unless you disable it). It has a simple chat widget interface that you can use to interact with your assistant for testing purposes. You can also share the URL to this IBM-branded site with your teammembers.

1.  Click **Add Integration**.

1.  Click the tile for the channel with which you want to integrate the assistant. The options include:

    - [Custom application](deploy-custom-app.html)
    - [Facebook Messenger](deploy-facebook.html)
    - [Intercom](deploy-intercom.html)  ![Premium plan only](images/premium0.png)
    - [Preview Link](deploy-web-link.html)
    - [Slack](deploy-slack.html)
    - [WordPress plug-in ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://wordpress.org/plugins/conversation-watson/){: new_window}

1.  Follow the instructions that are provided on the screen to complete the integration process.

After you integrate the assistant, test it from the target channel to ensure that the assistant works as expected.

For tips on how to start the dialog in a consistent way across all integration types, see [Starting the dialog](dialog-start.html).
