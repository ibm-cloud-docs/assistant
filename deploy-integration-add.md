---

copyright:
  years: 2015, 2019
lastupdated: "2019-06-27"

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

# Adding integrations
{: #deploy-integration-add}

To deploy your skill, add it to an assistant, and then add integrations to the assistant that publish your bot to the channels where your customers go for help.
{: shortdesc}

## How service desk platform integrations work
{: #deploy-integration-service-desk-integrations}

Watson this 3-minute video to learn more about integrating your assistant with a service desk platform, such as Intercom.

<iframe class="embed-responsive-item" id="youtubeplayer" title="Overview of how service desk integrations work" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/pJSCZLQVgCY?rel=0" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

To learn about how service desk integrations with your assistant can benefit your business, [read this blog post ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://medium.com/ibm-watson/contact-center-post-394dff427c8){: new_window}.

## Add an integration
{: #deploy-integration-add-task}

Follow these steps to add integrations to your assistant:

1.  Click the **Assistants** tab.

    If you don't see the **Assistants** tab, click the breadcrumb menu link in the page header.
    {: note}

1.  Click to open the tile for the assistant that you want to deploy.

1.  Go to the Integrations section.

    **What is the Preview Link integration?** When you create an assistant, a test web site is provisioned for you automatically (unless you choose not to enable the preview link). It has a simple chat widget interface that you can use to interact with your assistant for testing purposes. You can also share the URL to this IBM-branded site with your teammembers.

1.  Click **Add integration**.

1.  Click the tile for the channel with which you want to integrate the assistant. The options include:

    - [Custom application](/docs/services/assistant?topic=assistant-deploy-custom-app)
    - [Facebook Messenger](/docs/services/assistant?topic=assistant-deploy-facebook)
    - [Intercom](/docs/services/assistant?topic=assistant-deploy-intercom)  ![Plus or Premium plan only](images/premium.png)
    - [Preview Link](/docs/services/assistant?topic=assistant-deploy-web-link)
    - [Slack](/docs/services/assistant?topic=assistant-deploy-slack)
    - [Voice Agent (Telephony)  ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://cloud.ibm.com/catalog/services/voice-agent-with-watson){: new_window}

      Opens the **Voice Agent with Watson** service overview page on {{site.data.keyword.cloud_notm}}.
    - [WordPress plug-in ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://wordpress.org/plugins/conversation-watson/){: new_window}

      Opens the IBM Watson Assistant plug-in overview page on the WordPress site.

1.  Follow the instructions that are provided on the screen to complete the integration process.

After you integrate the assistant, test it from the target channel to ensure that the assistant works as expected.

For tips on how to start the dialog in a consistent way across all integration types, see [Starting the dialog](/docs/services/assistant?topic=assistant-dialog-start).

## Integration limits
{: #deploy-integration-add-limits}

The number of integrations you can create in a single service instance depends on your {{site.data.keyword.conversationshort}} plan.

| Service plan     | Integrations per assistant |
|------------------|---------------------------:|
| Premium          |                        100 |
| Plus             |                        100 |
| Standard         |                        100 |
| Plus Trial       |                        100 |
| Lite             |                        100 |
{: caption="Service plan details" caption-side="top"}
