---

copyright:
  years: 2021
lastupdated: "2021-06-24"

subcollection: assistant

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:external: target="_blank" .external}
{:deprecated: .deprecated}
{:important: .important}
{:note: .note}
{:tip: .tip}
{:preview: .preview}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:swift: .ph data-hd-programlang='swift'}
{:video: .video}

{{site.data.content.newlink}}

# Deploying your assistant
{: #deploy-overview}

After you have built and tested your assistant, you can make it available for customers to use.
{: shortdesc}

There are multiple options for deploying your assistant, depending on how you want your customers to interact with it. A _channel_ is a communication platform through which you want to make your assistant available, such as a website or a telephone system; an assistant can be deployed to one or more channels. To deploy to a channel, you use an _integration_, which is essentially an adapter or interface that enables the assistant to communicate through a channel.

In most cases, an assistant is deployed using one of these integrations:

- [**Web chat integration**](/docs/assistant?topic=assistant-deploy-web-chat): The web chat integration provides a secure and highly customizable widget you can add to your website. You can configure how and where the web chat widget appears, and you can use theming to align it with your branding and website design. If a customer needs help from a person, the web chat integration can transfer the conversation to an agent.

- [**Phone integration**](/docs/assistant?topic=assistant-deploy-phone): The phone integration enables your assistant to converse with customers on the phone, using the {{site.data.keyword.ibmwatson_notm}} {{site.data.keyword.texttospeechshort}} and {{site.data.keyword.speechtotextshort}} services. If your customer asks to speak to a person, the phone integration can transfer the call to an agent.

## Other channels

In addition to the web chat and phone integrations, {{site.data.keyword.conversationshort}} provides integrations you can use to deploy your assistant to numerous other channels:

- [Facebook Messenger](/docs/assistant?topic=assistant-deploy-facebook)
- [Intercom](/docs/assistant?topic=assistant-deploy-intercom)
- [Slack](/docs/assistant?topic=assistant-deploy-slack)
- [SMS with Twilio](/docs/assistant?topic=assistant-deploy-sms)
- [WhatsApp](/docs/assistant?topic=assistant-deploy-whatsapp)
- A [custom client application](/docs/assistant?topic=assistant-deploy-custom-app)
