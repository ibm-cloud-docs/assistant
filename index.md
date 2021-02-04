---

copyright:
  years: 2015, 2021
lastupdated: "2021-02-04"

keywords: chatbot, live chatbot, omnichannel

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

# About Watson Assistant
{: #index}

Use {{site.data.keyword.conversationfull}} to build your own branded live chatbot into any device, application, or channel. Your chatbot, which is also known as an *assistant*, connects to the customer engagement resources you already use to deliver an engaging, unified problem-solving experience to your customers.
{: shortdesc}

| | |
|------------|-------------|
| *Create AI-driven conversational flows* | Your assistant leverages industry-leading AI capabilities to understand questions that your customers ask in natural language. It uses machine learning models that are custom built from your data to deliver accurate answers in real time. |
| *Embed existing help content* | You already know the answers to customer questions? Put your subject matter expertise to work. Add a search skill to give your assistant access to corporate data collections that it can mine for answers. |
| *Connect to your customer service teams* | If customers need more help or want to discuss a topic that requires a personal touch, connect them to human agents from your existing service desk provider. |
| *Bring the assistant to your customers, where they are* | Configure one or more built-in integrations to quickly publish your assistant in popular social media channels like Slack, Facebook Messenger, or Intercom. Add your assistant as a chat widget to your company website, or build your own custom app. |
| *Track customer engagement and satisfaction* | Use built-in metrics to analyze logs from conversations between customers and your assistant to gauge how well it's doing and identify areas for improvement. |

## How it works
{: #index-how-it-works}

This diagram illustrates how the product delivers an exceptional, omnichannel customer experience:

![Flow diagram of the service](images/arch-detail.png)

- Customers interact with the assistant through one or more of these channels:

  - An existing social media messaging platform, such as Slack, Facebook Messenger, or WhatsApp
  - A phone call or text message
  - A web chat that you embed in your company website and that can transfer complex requests to a customer support representative.
  - A custom application that you develop, such as a mobile app or a robot with a voice interface

- The **assistant** receives a message from a customer and sends it down the appropriate resolution path. 

  If you want to preprocess incoming messages, this is where you would inject logic to call an external service that can process the messages before the assistant routes them. Likewise, you can process responses from the assistant before they are returned to the customer.

- The assistant chooses the appropriate resolution from among these options:

  - A **conversational skill** interprets the customer's message further, then directs the flow of the conversation. The skill gathers any information it needs to respond or perform a transaction on the customer's behalf.

  - A **search skill** leverages existing FAQ or other curated content that you own to find relevant answers to customer questions.

  - If a customer wants more personalized help or wants to discuss a sensitive subject, the assistant can connect the customer with someone from your support team through the web chat integration.

For more information about the architecture, read the [How to Make Chatbot Orchestration Easier](https://medium.com/ibm-watson/how-to-make-chatbot-orchestration-easier-c8ed61620b8d){: external} blog on Medium.com.

To see how {{site.data.keyword.conversationshort}} is helping enterprises cut costs and improve customer satisfaction today, read the [Independent study finds IBM Watson Assistant customers can accrue $23.9 million in benefits](https://www.ibm.com/blogs/watson/2020/03/independent-study-finds-ibm-watson-assistant-customers-accrued-23-9-million-in-benefits/){: external} blog on ibm.com.

This documentation describes managed instances of {{site.data.keyword.conversationshort}} that are offered in IBM Cloud or in Cloud Pak for Data as a Service. If you are interested in on-premises or installed deployments, see [this documentation](/docs/assistant-data?topic=assistant-data-index).
{: note}

Read more about these implementation steps by following these links:

- [Creating an assistant](/docs/assistant?topic=assistant-assistant-add)
- [Adding skills to your assistant](/docs/assistant?topic=assistant-skill-add)
- [Deploying your assistant](/docs/assistant?topic=assistant-deploy-integration-add)

## Browser support
{: #index-browser-support}

The {{site.data.keyword.conversationshort}} application (where you create assistants and skills) requires the same level of browser software as is required by {{site.data.keyword.Bluemix_notm}}. For more information, see {{site.data.keyword.Bluemix_notm}} [Prerequisites](/docs/overview?topic=overview-prereqs-platform#browsers-platform){: external}. 

For information about the web browsers that are supported by the web chat integration, see [Browser Support](https://web-chat.global.assistant.watson.cloud.ibm.com/docs.html?to=key-concepts#browsersupport){: external}.

## Language support
{: #index-lang-support}

Language support by feature is detailed in the [Supported languages](/docs/assistant?topic=assistant-language-support) topic.

## Terms and notices
{: #index-notices}

See [IBM Cloud Terms and Notices](/docs/overview/terms-of-use?topic=overview-terms){: external} for information about the terms of service.

US Health Insurance Portability and Accountability Act (HIPAA) support is available for Premium plans that are hosted in the Washington, DC location created on or after 1 April 2019. For more information, see [Enabling EU and HIPAA supported settings](/docs/account?topic=account-eu-hipaa-supported#eu-hipaa-supported){: external}.

To learn more about service terms and data security, read the following information:

- [Service terms](https://www-03.ibm.com/software/sla/sladb.nsf/sla/saas?OpenDocument){: external} (Search for the {{site.data.keyword.conversationshort}} offering)
- [Data Processing and Protection Datasheet](https://www.ibm.com/software/reports/compatibility/clarity-reports/report/html/softwareReqsForProduct?deliverableId=DF7F84500FA711E69DCADF455C6AF151){: external}
- [Information security](/docs/assistant?topic=assistant-information-security)
- [IBM Cloud Data security and privacy](https://www.ibm.com/software/sla/sladb.nsf/sla/csdsp?OpenDocument){: external}

## Next steps
{: #index-next-steps}

- [Get started](/docs/assistant?topic=assistant-getting-started) with the product.

Have questions? Contact [IBM Sales](https://www.ibm.com/account/reg/us-en/signup?formid=urx-20970){: external}.
