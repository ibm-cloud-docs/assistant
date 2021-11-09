---

copyright:
  years: 2015, 2021
lastupdated: "2021-04-23"

keywords: deploy, deploying

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
{:video: .video}

{{site.data.content.newlink}}

# Adding integrations
{: #deploy-integration-add}

To deploy your skill, add it to an assistant, and then add integrations to the assistant that publish your bot to the channels where your customers go for help.
{: shortdesc}

## Add an integration
{: #deploy-integration-add-task}

Follow these steps to add integrations to your assistant:

1.  Click the **Assistants** icon ![Assistants menu icon](images/nav-ass-icon.png).

1.  Click to open the tile for the assistant that you want to deploy.

1.  Go to the Integrations section.

    **Why do I have the *Web chat* integration?** This integration is provisioned and added automatically to your first assistant only.

    The web chat is a chat window that you can embed in one or more pages on your website to share your assistant with your customers.

1.  Click **Add integration**.

1.  Click the tile for the channel with which you want to integrate the assistant. The options include:

    - [Web chat](/docs/assistant?topic=assistant-deploy-web-chat)
    - [Facebook Messenger](/docs/assistant?topic=assistant-deploy-facebook)
    - [Slack](/docs/assistant?topic=assistant-deploy-slack)
    - [Phone](/docs/assistant?topic=assistant-deploy-phone)
    - [SMS with Twilio](/docs/assistant?topic=assistant-deploy-sms)
    - [WhatsApp with Twilio](/docs/assistant?topic=assistant-deploy-whatsapp)
    - [Custom application](/docs/assistant?topic=assistant-deploy-custom-app)

    Built-in service desk integrations:

    - [Intercom](/docs/assistant?topic=assistant-deploy-intercom)
    - [Web chat with Salesforce support](/docs/assistant?topic=assistant-deploy-salesforce)
    - [Web chat with Zendesk support](/docs/assistant?topic=assistant-deploy-zendesk)

    Web chat service desk reference implementations:

    - [Genesys Cloud](https://github.com/watson-developer-cloud/assistant-web-chat-service-desk-starter/tree/main/src/genesys/webChat){: external}
    - [NICE inContact](https://github.com/watson-developer-cloud/assistant-web-chat-service-desk-starter/tree/main/src/incontact/webChat){: external}
    - [Twilio Flex](https://github.com/watson-developer-cloud/assistant-web-chat-service-desk-starter/tree/main/src/flex/webChat){: external}
    - [Bring your own (starter kit)](https://github.com/watson-developer-cloud/assistant-web-chat-service-desk-starter){: external}

1.  Follow the instructions that are provided on the screen to complete the integration process.

After you integrate the assistant, test it from the target channel to ensure that the assistant works as expected.

![Plus or higher plans only](images/plus.png) For environments where private endpoints are in use, keep in mind that these integrations send traffic over the internet. For more information, see [Private network endpoints](https://cloud.ibm.com/docs/assistant?topic=assistant-security#security-private-endpoints).
{: note}

## How service desk platform integrations work
{: #deploy-integration-service-desk-integrations}

Watch this 3-minute video to learn more about integrating your assistant with a service desk platform, such as Intercom.

![Overview of how service desk integrations work](https://www.youtube.com/embed/pJSCZLQVgCY){: video output="iframe" id="youtubeplayer" frameborder="0" width="560" height="315" webkitallowfullscreen mozallowfullscreen allowfullscreen}

To read a transcript of the video, [open the video on YouTube.com](https://www.youtube.com/watch?v=pJSCZLQVgCY&feature=emb_imp_woyt), click the *More actions* icon, and then choose *Open transcript*.

To learn about how service desk integrations with your assistant can benefit your business, [read this blog post](https://medium.com/ibm-watson/contact-center-post-394dff427c8){: external}.

## Integration limits
{: #deploy-integration-add-limits}

The number of integrations you can create in a single service instance depends on your {{site.data.keyword.conversationshort}} plan.

| Service plan     | Integrations per assistant |
|------------------|---------------------------:|
| Enterprise       |                        100 |
| Premium (legacy) |                        100 |
| Plus             |                        100 |
| Trial            |                        100 |
| Lite             |                        100 |
{: caption="Service plan details" caption-side="top"}
