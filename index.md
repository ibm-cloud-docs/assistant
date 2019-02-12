---

copyright:
  years: 2015, 2019
lastupdated: "2019-02-12"

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

# About
{: #index}

{{site.data.keyword.conversationfull}} is a cognitive bot that you can customize for your business needs, and deploy across multiple channels to bring help to your customers where and when they need it.
{: shortdesc}

## Where are my workspaces?
{: #index-existing-customers}

If you created a *workspace* in a previous version of the service, have no fear; you can still get to it. Workspaces are now called *skills*. To get to your workspace, click the **Skills** tab.

## How it works
{: #index-how-it-works}

This diagram shows the overall architecture:

![Flow diagram of the service](images/arch-overview.png)

- Users interact with the assistant through one or more of these **integration** points:

  - A chat bot that you publish directly to an existing social media messaging platform, such as Slack or Facebook Messenger.
  - A simple chat bot user interface that is hosted by IBM Cloud.
  - Custom application that you develop, such as a mobile app or a robot with a voice interface.

- The **assistant** receives user input and routes it to the dialog skill.

- The dialog **skill** interprets the user input further, then directs the flow of the conversation and gathers any information that it needs to respond or perform a transaction on the user's behalf.

## Implementation
{: #index-mplementation}

Here's how you will implement your assistant:

- **Create a dialog skill**. Use the intuitive graphical tool to define the training data and dialog for the conversation between your assistant and your customers.

  The training data consists of the following artifacts:

  - **Intents**: Goals that you anticipate your users will have when they interact with the service. Define one intent for each goal that can be identified in a user's input. For example, you might define an intent named *store_hours* that answers questions about store hours. For each intent, you add sample utterances that reflect the input customers might use to ask for the information they need, such as, `What time do you open?`

    Or use prebuilt **content catalogs** provided by IBM to get started with data that addresses common customer goals.

  - **Entities**: An entity represents a term or object that provides context for an intent. For example, an entity might be a city name that helps your dialog to distinguish which store the user wants to know store hours for.

    As you add training data, a natural language classifier is automatically added to the skill, and is trained to understand the types of requests that you have indicated the service should listen for and respond to.

  - **Dialog**: Use the dialog tool to build a dialog flow that incorporates your intents and entities. The dialog flow is represented graphically in the tool as a tree. You can add a branch to process each of the intents that you want the service to handle. You can then add branch nodes that handle the many possible permutations of a request based on other factors, such as the entities found in the user input or information that is passed to the service from an external service.

- **Create an assistant**.

- **Add the dialog skill to your assistant.**

- **Integrate your assistant.** Create a channel integration to deploy the configured assistant directly to a social media or messaging channel.

  Your deployed assistant is hosted by {{site.data.keyword.cloud_notm}}, the IBM cloud computing platform. (See [Platform overview ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://cloud.ibm.com/docs/overview/ibm-cloud.html#overview) for more information.)

Read more about these implementation steps by following these links:

- [Intent creation overview](/docs/services/assistant/intents.html#intents-described)
- [Dialog overview](/docs/services/assistant/dialog-overview.html)
- [Entity creation overview](/docs/services/assistant/entities.html#entities-described)
- [Assistant overview](/docs/services/assistant/assistant-add.html)
- [Adding integrations](/docs/services/assistant/deploy-integration-add.html)

## Browser support
{: #index-browser-support}

The {{site.data.keyword.conversationshort}} service tool requires the same level of browser software as is required by {{site.data.keyword.Bluemix_notm}}. See the {{site.data.keyword.Bluemix_notm}} [Prerequisites ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://cloud.ibm.com/docs/overview/prereqs.html#browsers){: new_window} topic for details.

## Language support
{: #index-lang-support}

Language support by feature is detailed in the [Supported languages](/docs/services/assistant/language-support.html) topic.

## Terms and notices
{: #index-notices}

See [IBM Cloud Terms and Notices ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://cloud.ibm.com/docs/overview/terms-of-use/notices.html) for information about the terms of service.

## Next steps
{: #index-next-steps}

- [Get started](/docs/services/assistant/getting-started.html) with the service.
- View the list of [developer resources ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/watson/developer-resources/){: new_window}.

Still have questions? Contact [IBM Sales ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www-01.ibm.com/marketing/iwm/dre/signup?source=urx-20970){: new_window}.
