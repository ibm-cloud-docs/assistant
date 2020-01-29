---

copyright:
  years: 2015, 2020
lastupdated: "2020-01-29"

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

# About
{: #index}

Use {{site.data.keyword.conversationfull}} to build your own branded assistant into any device, application, or channel. Your assistant connects to the customer engagement resources you already use to deliver an engaging, unified problem-solving experience to your customers.
{: shortdesc}

## How it works
{: #index-how-it-works}

This diagram illustrates how the product delivers an omnichannel customer experience:

![Flow diagram of the service](images/simple-overview.png)

- Users interact with the assistant through one or more of these **integration** points:

  - A virtual assistant that you publish directly to an existing social media messaging platform, such as Slack or Facebook Messenger.
  - A custom application that you develop, such as a mobile app or a robot with a voice interface.

- The **assistant** receives user input and routes it to the dialog skill.

- The **dialog skill** interprets the user input further, then directs the flow of the conversation. The dialog gathers any information it needs to respond or perform a transaction on the user's behalf.

- Any questions that cannot be answered by the dialog skill are sent to the **search skill**, which finds relevant answers by searching the company knowledge bases that you configure for the purpose.

## Implementation
{: #index-implementation}

This diagram shows the implementation in more detail:

![Flow diagram of the service](images/arch-overview-search.png)

Here's how you implement your assistant:

- **Create an assistant**.

- **Add a skill to your assistant**.

  Depending on your service plan, you can add the following types of skills:

  - **Add a dialog skill**.  
  
    Use the intuitive graphical product to define the training data and dialog for the conversation between your assistant and your customers. The training data consists of the following artifacts:

    - **Intents**: Goals that you anticipate your users have when they interact with your assistant. Define one intent for each goal that can be identified in a user's input. For example, you might define an intent that is named *store_hours* that answers questions about store hours. For each intent, you add sample utterances that reflect the input customers might use to ask for the information they need, such as, `What time do you open?`

      Or use prebuilt **content catalogs** that are provided by IBM to get started with data that addresses common customer goals.

    - **Dialog**: Use the dialog editor to build a dialog flow that incorporates your intents. The dialog flow is represented graphically as a tree. You can add a branch to process each of the intents that you want your assistant to handle.

    - **Entities**: An entity represents a term or object that provides context for an intent. For example, an entity might be a city name that helps your dialog to distinguish which store the user wants to know store hours for. After you add entities, update your dialog to use them. Add dialog nodes that handle the many possible permutations of a request based on the entities that are found in the user input.

    As you add training data, a natural language classifier is automatically added to the skill. The classifier model is trained to understand the types of requests that you teach your assistant to listen for and respond to.

  - **Add a search skill**. ![Plus or Premium plan only](images/plus.png)

    Take advantage of data collections that you create in {{site.data.keyword.discoveryfull}} to provide answers to customer questions. When a customer asks a question that the dialog is not designed to answer, your assistant can search for relevant information from the configured data sources, extract the information, and return it as the assistant's response.

- **Integrate your assistant.** Add a built-in channel integration to deploy the configured assistant directly to a social media or messaging channel. Or build your own client application as the user interface for the assistant.

  Your deployed assistant is hosted by {{site.data.keyword.cloud}}, the IBM cloud computing platform. (For more information, see [Platform overview](/docs/overview?topic=overview-whatis-platform){: external}.) 

  If you are interested in on-premises deployments, see the [{{site.data.keyword.conversationfull}} for {{site.data.keyword.icp4dfull}} documentation](/docs/assistant-data?topic=assistant-data-index).

Read more about these implementation steps by following these links:

- [Assistant overview](/docs/assistant?topic=assistant-assistants)
- [Search skill overview](/docs/assistant?topic=assistant-skill-search-add)
- [Intent creation overview](/docs/assistant?topic=assistant-intents#intents-described)
- [Dialog overview](/docs/assistant?topic=assistant-dialog-overview)
- [Entity creation overview](/docs/assistant?topic=assistant-entities#entities-described)
- [Adding integrations](/docs/assistant?topic=assistant-deploy-integration-add)

## Browser support
{: #index-browser-support}

The {{site.data.keyword.conversationshort}} application (where you create assistants and skills) requires the same level of browser software as is required by {{site.data.keyword.Bluemix_notm}}. For more information, see {{site.data.keyword.Bluemix_notm}} [Prerequisites](/docs/overview?topic=overview-prereqs-platform#browsers-platform){: external}. 

For information about the web browsers that are supported by the Web Chat integration, see [Browser Support](https://watson-developer-cloud.github.io/assistant-web-chat/documentation.html#browser-support){: external}.

## Language support
{: #index-lang-support}

Language support by feature is detailed in the [Supported languages](/docs/assistant?topic=assistant-language-support) topic.

## Terms and notices
{: #index-notices}

See [IBM Cloud Terms and Notices](/docs/overview/terms-of-use?topic=overview-terms){: external} for information about the terms of service.

US Health Insurance Portability and Accountability Act (HIPAA) support is available for Premium plans that are hosted in the Washington, DC location created on or after 1 April 2019. For more information, see [Enabling EU and HIPAA supported settings](/docs/account?topic=account-eu-hipaa-supported#eu-hipaa-supported){: external}.

## Next steps
{: #index-next-steps}

- [Get started](/docs/assistant?topic=assistant-getting-started) with the product.
- View the list of [developer resources)](https://www.ibm.com/watson/developer-resources/){: external}.

Have questions? Contact [IBM Sales](https://www.ibm.com/account/reg/us-en/signup?formid=urx-20970){: external}.
