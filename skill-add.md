---

copyright:
  years: 2018, 2023
lastupdated: "2023-08-25"

subcollection: assistant

---

{{site.data.keyword.attribute-definition-list}}

{{site.data.content.newlink}}

# Adding skills to your assistant
{: #skill-add}

Customize your assistant by adding to it the skills it needs to satisfy your customers' goals.
{: shortdesc}

Conversational skills return responses that are authored by you to answer common questions, while a search skill searches for and returns passages from existing self-service content.

You can add the following types of skills to your assistant:

- *Conversational skills*: Understand and address questions or requests that your customers typically ask about. You provide information about the subjects or tasks that your users need help with, and how they ask about them, and the product dynamically builds a machine learning model that is tailored to understand the same and similar user requests.

   - **Actions skill**: Offers a simple interface where anyone can build a conversational flow for your assistant to follow. The complex process of training data creation occurs behind the scenes automatically.  [Learn more](#skill-add-actions-skill)

   - **Dialog skill**: Offers a set of editors that you use to define both your training data and the conversation. The conversation is represented as a dialog tree. You use the graphical dialog editor to create a script of sorts for your assistant to read from when it interacts with your customers. The dialog keys off the common customer goals that you teach it to recognize, and provides useful responses. [Learn more](#skill-add-dialog-skill)

- **Search skill** [Plus]{: tag-green}[Enterprise]{: tag-purple}: Leverages information from existing corporate knowledge bases or other collections of content authored by subject matter experts to address unanticipated or more nuanced customer inquiries. For a given user query, uses the {{site.data.keyword.discoveryfull}} service to search a data source of your self-service content and return an answer. [Learn more](#skill-add-search-skill)

## Actions skill
{: #skill-add-actions-skill}

An actions skill contains actions that represent the tasks you want your assistant to help your customers with.

Each action contains a series of steps that represent individual exchanges with a customer. Building the conversation that your assistant has with your customers is fundamentally about deciding which steps, or which user interactions, are required to complete an action. After you identify the list of steps, you can then focus on writing engaging content to turn each interaction into a positive experience for your customer.

![Diagram of a simple exchange between a customer and an actions skill step.](images/action-skill-explained.png)

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

## Search skill
{: #skill-add-search-skill}

[Plus]{: tag-green}[Enterprise]{: tag-purple}

When {{site.data.keyword.conversationshort}} doesn't have an explicit solution to a problem, it routes the user question to a search skill to find an answer from across your disparate sources of self-service content. The search skill interacts with the {{site.data.keyword.discoveryfull}} service to extract this information from a configured data collection.

If you already use the {{site.data.keyword.discoveryshort}} service, you can mine your existing data collections for source material that you can share with customers to address their questions.

The following diagram illustrates how user input is processed when both a dialog skill and a search skill are added to an assistant. Any questions that the dialog is not designed to answer are sent to the search skill, which finds a relevant response from a {{site.data.keyword.discoveryshort}} data collection.

![Diagram of how a question gets routed to the search skill.](images/search-skill-diagram.png)

## Skill limits
{: #skill-add-limits}

The number of skills you can create depends on your {{site.data.keyword.conversationshort}} plan type. Any sample dialog skills that are available for you to use do not count toward your limit unless you use them. A skill version does not count as a skill.

| Plan | Maximum number of skills of each type per service instance |
| --- | --- |
| Enterprise | 100 |
| Premium (legacy) | 100 |
| Plus | 50 |
| Trial | 50 |
| Standard (legacy) | 20 |
| Lite | 5 |
{: caption="Plan details" caption-side="bottom"}

After 30 days of inactivity, an unused skill in a Lite plan service instance might be deleted to free up space.
{: note}
