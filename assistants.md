---

copyright:
  years: 2015, 2019
lastupdated: "2019-07-26"

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

# Assistants
{: #assistants}

An assistant is a cognitive bot that you can customize for your business needs, and deploy across multiple channels to bring help to your customers where and when they need it.
{: shortdesc}

![Skills](images/skill-icon.png)  An assistant routes your customer queries to a skill, which then provides the appropriate response. Dialog skills return responses that are authored by you to answer common questions, while search skills search for and return passages from existing self-service content to answer more complex inquiries.

## Dialog skill
{: #assistants-dialog-skill}

A dialog skill can understand and address questions or requests that your customers typically need help with. You provide information about the subjects or tasks your users ask about, and how they ask about them, and the product dynamically builds a machine learning model that is tailored to understand the same and similar user requests.

| Dialog tree | Graphical user interface |
|-------------|-------------------------:|
| You can use the graphical dialog editor to create a script of sorts for your assistant to read from when it interacts with your customers. The result is a dialog that simulates a real conversation. The dialog keys off the common customer goals that you teach it to recognize, and provides useful responses. | ![A sample dialog tree with example content](images/dialog-depiction.png) |

The dialog skill itself is defined in text, but you can integrate it with Watson Speech to Text and Watson Text to Speech services that enable users to interact with your assistant verbally.

![Out-of-the-box training data](images/oob.png)  If you want to get started quickly, add prebuilt training data to your dialog skill so your assistant can start helping your customers with the basics.

## Search skill ![Plus or Premium plan only](images/plus.png)
{: #assistants-search-skill}

Search skill is available to Plus or Premiums users only.
{: note}

A search skill leverages information from existing corporate knowledge bases or other collections of content authored by subject matter experts to address unanticipated or more nuanced customer inquiries.

![IBM Cloud](images/cloud.png)  The assistant is a fully hosted bot that is managed by {{site.data.keyword.Bluemix_notm}}, which means you do not need to worry about setting up or maintaining infrastructure to support it.

| Integrations       | Channels  |
|--------------------|:----------|
| You can deploy the assistant through multiple interfaces, including existing messaging channels, such as Slack and Facebook Messenger, in just a few steps. Or, if you want to design a custom application that incorporates it, you can make direct calls to the underlying APIs to do so. | ![Integration methods including Slack, Facebook Messenger, a web application or human agent integration](images/integrations.png) |

See [Creating assistants](/docs/services/assistant?topic=assistant-assistant-add) to get started.