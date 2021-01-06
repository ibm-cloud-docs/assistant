---

copyright:
  years: 2018, 2021
lastupdated: "2021-01-06"

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

# Adding a skill to an assistant
{: #skill-add}

Customize your assistant by adding to it the skills it needs to satisfy your customers' goals.
{: shortdesc}

You can add one skill of each type to your assistant:

- **Actions skill** ![Beta](images/beta.png): Offers a simple interface where you can build a conversational flow for your assistant to follow.

- **Dialog skill**: Uses Watson natural language processing and machine learning technologies to understand user questions and requests, and respond to them with answers that are authored by you.

- **Search skill** ![Plus or Premium plan only](images/plus.png): For a given user query, uses the {{site.data.keyword.discoveryfull}} service to search a data source of your self-service content and return an answer.

  Only users of Plus or Premium plans can create this type of skill.
  {: important}

If you add both a dialog skill and an actions skill to your assistant, the dialog skill is used. You can configure your dialog skill to process individual actions from your actions skill by following the steps in [Calling an actions skill from a dialog](/docs/assistant?topic=assistant-dialog-call-action).

If you can't decide between creating an actions or dialog skill, see [Choosing skills](/docs/assistant?topic=assistant-skills-choose).
{: tip}

## Actions skill
{: #skill-add-actions-skill}

An actions skill contains actions that represent the tasks you want your assistant to help your customers with.

Each action contains a series of steps that represent individual exchanges with a customer. Building the conversation that your assistant has with your customers is fundamentally about deciding which steps, or which user interactions, are required to complete an action. After you identify the list of steps, you can then focus on writing engaging content to turn each interaction into a positive experience for your customer.

## Dialog skill
{: #skill-add-dialog-skill}

A dialog skill contains the training data and logic that enables an assistant to help your customers. It contains the following types of artifacts:

- [**Intents**](/docs/assistant?topic=assistant-intents): An *intent* represents the purpose of a user's input, such as a question about business locations or a bill payment. You define an intent for each type of user request you want your application to support. The name of an intent is always prefixed with the `#` character. To train the dialog skill to recognize your intents, you supply lots of examples of user input and indicate which intents they map to.

  A *content catalog* is provided that contains prebuilt common intents you can add to your application rather than building your own. For example, most applications require a greeting intent that starts a dialog with the user. You can add the **General** content catalog to add an intent that greets the user and does other useful things, like end the conversation.

- [**Dialog**](/docs/assistant?topic=assistant-dialog-build): A *dialog* is a branching conversation flow that defines how your application responds when it recognizes the defined intents and entities. You use the dialog editor to create conversations with users, providing responses based on the intents and entities that you recognize in their input.

  ![Diagram of a basic implementation that uses intent and dialog only.](images/basic-impl.png)

To enable your dialog skill to handle more nuanced questions, define entities and reference them from your dialog.

- [**Entities**](/docs/assistant?topic=assistant-entities); An *entity* represents a term or object that is relevant to your intents and that provides a specific context for an intent. For example, an entity might represent a city where the user wants to find a business location, or the amount of a bill payment. The name of an entity is always prefixed with the `@` character.

  You can train the skill to recognize your entities by providing entity term values and synonyms, entity patterns, or by identifying the context in which an entity is typically used in a sentence. To fine tune your dialog, go back and add nodes that check for entity mentions in user input in addition to intents.

![Diagram of a more complex implementation that uses intent, entity, and dialog.](images/complex-impl.png)

As you add information, the skill uses this unique data to build a machine learning model that can recognize these and similar user inputs. Each time you add or change the training data, the training process is triggered to ensure that the underlying model stays up-to-date as your customer needs and the topics they want to discuss change.

## Search skill ![Plus or Premium plan only](images/plus.png)
{: #skill-add-search-skill}

When Watson Assistant doesn't have an explicit solution to a problem, it routes the user question to a search skill to find an answer from across your disparate sources of self-service content. The search skill interacts with the {{site.data.keyword.discoveryfull}} service to extract this information from a configured data collection.

If you already use the {{site.data.keyword.discoveryshort}} service, you can mine your existing data collections for source material that you can share with customers to address their questions.

However, you do not need to have a {{site.data.keyword.discoveryshort}} service instance. If you choose to create a search skill, a free instance of {{site.data.keyword.discoveryshort}} is provisioned for you. You can then create a collection from a data source and configure your search skill to search this collection to find answers to customer queries.

The following diagram illustrates how user input is processed when both a dialog skill and a search skill are added to an assistant. Any questions that the dialog is not designed to answer are sent to the search skill, which finds a relevant response from a {{site.data.keyword.discoveryshort}} data collection.

![Diagram of how a question gets routed to the search skill.](images/search-skill-diagram.png)

## Creating a skill
{: #skill-add-task}

You can add one skill of each skill type to an assistant.

- [Actions skill](/docs/assistant?topic=assistant-skill-actions-add)
- [Dialog Skill](/docs/assistant?topic=assistant-skill-dialog-add)
- [Search Skill](/docs/assistant?topic=assistant-skill-search-add)

## Skill limits
{: #skill-add-limits}

The number of skills you can create depends on your {{site.data.keyword.conversationshort}} plan type. Any sample dialog skills that are available for you to use do not count toward your limit unless you use them. A skill version does not count as a skill.

| Plan     | Maximum number of skills of each type per service instance |
|------------------|----------------------------:|
| Premium          |                         100 |
| Plus             |                          50 |
| Standard (legacy) |                         20 |
| Lite`*`, Plus Trial |                        5 |
{: caption="Plan details" caption-side="top"}

`*` After 30 days of inactivity, an unused skill in a Lite plan service instance might be deleted to free up space.

## Renaming a skill
{: #skill-add-rename}

You can change the name of a skill and its associated description after you create the skill.

To rename a skill, follow these steps:

1.  From the Skills page, find the skill that you want to rename.

1.  Click the ![open and close list of options](images/kebab.png) icon, and then choose **Rename**.

1.  Edit the name, and then click **Save**.

## Deleting a skill
{: #skill-add-delete}

You can delete any skill that you can access, unless it is being used by an assistant. If it is in use, you must remove it from the assistant that is using it before you can delete it.

Be sure to check with anyone else who might be using the skill before you delete it.
{: tip}

To delete a skill, complete the following steps:

1.  Find out whether the skill is being used by any assistants. From the Skills page, find the tile for the skill that you want to delete. The **Assistants** field lists the assistants that currently use the skill.

1.  If the skill you want to delete is associated with an assistant, then remove it from the assistant by completing the following steps:

    - Check with the owner of the assistant that is using the skill before you remove the skill from it.
    - Open the Assistants page, and then click to open the assistant tile.
    - Find the tile for the skill that you want to delete. Click the ![open and close list of options](images/kebab.png) icon, and then choose **Remove**.
    - Repeat the previous steps for any other assistants that use the skill.
    - Return to the Skills page and find the tile for the skill that you want to delete.

1.  Click the ![open and close list of options](images/kebab.png) icon, and then choose **Delete**. Confirm the deletion.
