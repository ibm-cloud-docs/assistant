---

copyright:
  years: 2015, 2018
lastupdated: "2018-06-21"
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

# Release notes

## Changes
{: #change-log}

The following new features and changes to the service are available.

### 6 August 2018
{: #6August2018}

- **Digression return message**: You can now specify text to display when the user returns to a node after a digression. The user will have seen the prompt for the node already. You can change the message slightly to let users know they are returning to where they left off. For example, specify a response like, `Where were we? Oh, yes...` See [Digressions](dialog-runtime.html#digressions) for more details.

- **Jump-to fix**: Fixed a bug in the Dialogs tool which prevented you from being able to configure a jump-to that targets the response of a node with the `anything_else` special condition.

### 20 June 2018
{: #20June2018}

- **Intent conflict resolution**: The {{site.data.keyword.conversationshort}} can now help you to resolve conflicts when two or more intent examples in separate intents are so similar that {{site.data.keyword.conversationshort}} is confused as to which intent to use. See [Resolving intent conflicts](intents.html#conflict-intents) for details.

- **Disambiguation**: Enable disambiguation to allow your assistant to ask the user for help when it needs to decide between two or more viable dialog nodes to process for a response. See [Disambiguation](dialog-runtime.html#disambiguation) for more details.

### 12 June 2018
{: #12June2018}

- **Pattern limit expanded**: When using the **Patterns** field to [define specific patterns for an entity value](entities.html#patterns), the pattern (regular expression) is now limited to 512 characters.

### 4 June 2018
{: #4June2018}

- **Rich responses**: You can now add rich responses, that include elements such as images or buttons in addition to text, to your dialog. See [Rich responses](dialog-overview.html#multimedia) for more information.

- **Entity annotations**: Entity annotations teach the service about the context in which entities are typically mentioned in speech. Add annotations by labeling entity mentions that occur in your intent user examples. This feature is currently available for English only. See [Defining annotations for entities](entities.html#defining-annotations-for-entities) for more information.

### 7 May 2018
{: #7May2018}

- You can now chat with a live IBM representative. Click the ![Help](images/help_icon.png) icon from the tooling header, and then click **Chat with support**.

### 12 April 2018
{: #12April2018}

- A group of select customers were given access to a private beta build of {{site.data.keyword.conversationfull}} for evaluation. The Beta introduces customers to a new way of building assistants, one that manages the orchestration of information from one dialog call to the next, and that simplifies the deployment process so the assistant can start helping customers faster. Read [this blog post ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/blogs/watson/2018/03/the-future-of-watson-conversation-watson-assistant/) for more details.
