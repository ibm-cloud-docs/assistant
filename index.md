---

copyright:
  years: 2015, 2018
lastupdated: "2018-09-20"

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

# About
{: #about}

{{site.data.keyword.conversationfull}} is a cognitive bot that you can customize for your business needs, and deploy across multiple channels to bring help to your customers where and when they need it.
{: shortdesc}

**BETA** The features described in this documentation are beta features that have been made available to a small group of users for evaluation. Beta features might be unstable, might change frequently, and might be discontinued with short notice. Beta features also might not provide the same level of performance or compatibility that generally available features provide and are not intended for use in a production environment. Go [here](https://console.bluemix.net/docs/services/conversation/index.html) to see the product documentation for the generally available version of this service. Beta features are supported only on [IBM Developer Answers ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://developer.ibm.com/answers/search.html?f=&sort=relevance&q=%5Bwatson-conversation%5D+%5Bwatson-assistant%5D+%24).

## How it works

This diagram shows the overall architecture:

![Flow diagram of the service](images/arch-overview.png)

- Users interact with the assistant through one or more of these **integration** points:

  - A chat bot that you publish directly to an existing social media messaging platform, such as Slack or Facebook Messenger.
  - A simple chat bot user interface that is hosted by IBM Cloud.
  - Custom application that you develop, such as a mobile app or a robot with a voice interface.

- The **assistant** receives user input and routes it to the dialog skill.

- The dialog **skill** interprets the user input further, then directs the flow of the conversation and gathers any information that it needs to respond or perform a transaction on the user's behalf.

## Implementation

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

  Your deployed assistant is hosted by {{site.data.keyword.cloud_notm}}, the IBM cloud computing platform. (See [Platform overview ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://console.bluemix.net/docs/overview/ibm-cloud.html#overview) for more information.)

Read more about these implementation steps by following these links:

- [Intent creation overview](intents.html#intent-described)
- [Dialog overview](dialog-overview.html)
- [Entity creation overview](entities.html#entity-described)
- [Assistant overview](create-assistant.html)
- [Adding integrations](add-integrations.html)

## Browser support

The {{site.data.keyword.conversationshort}} service tool requires the same level of browser software as is required by {{site.data.keyword.Bluemix_notm}}. See the {{site.data.keyword.Bluemix_notm}} [Prerequisites ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://console.bluemix.net/docs/overview/prereqs.html#browsers){: new_window} topic for details.

## Language support

Language support by feature is detailed in the [Supported languages](lang-support.html) topic.

## Next steps

- [Get started](getting-started.html) with the service.
- View the list of [SDKs ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/watson/developercloud/developer-tools.html){: new_window}.

Still have questions? Click the ![Help](images/help_icon.png) icon from the tooling header, and then click **Chat with support** or contact [IBM Sales ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www-01.ibm.com/marketing/iwm/dre/signup?source=urx-20970){: new_window}.
