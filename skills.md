---

copyright:
  years: 2015, 2018
lastupdated: "2018-10-19"

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

# Skills
{: #skills}

A skill is a container for the artificial intelligence that enables an assistant to help your customers.
{: shortdesc}

## Skill types

You can add one of the following types of skills to your assistant:

- **Dialog skill**: Understands typical questions or requests from users and answers or fulfills them by following a dialog that is scripted by you.
- **Search skill**: Answers a user's question by searching for relevant information from an external data source, extracting the passage, and returning it as the assistant's response.

### Dialog skill
{: #dialog-skill}

A dialog skill contains the following types of artifacts:

- [**Intents**](intents.html): An *intent* represents the purpose of a user's input, such as a question about business locations or a bill payment. You define an intent for each type of user request you want your application to support. In the tool, the name of an intent is always prefixed with the `#` character. To train the dialog skill to recognize your intents, you supply lots of examples of user input and indicate which intents they map to.
- [**Entities**](entities.html); An *entity* represents a term or object that is relevant to your intents and that provides a specific context for an intent. For example, an entity might represent a city where the user wants to find a business location, or the amount of a bill payment. In the tool, the name of an entity is always prefixed with the `@` character. To train the dialog skill to recognize your entities, you list the possible values for each entity and synonyms that users might enter.
- [**Dialog**](dialog-build.html): A *dialog* is a branching conversation flow that defines how your application responds when it recognizes the defined intents and entities. You use the dialog builder in the tool to create conversations with users, providing responses based on the intents and entities that you recognize in their input.

The *content catalog* contains prebuilt common intents and entities that you can add to your application rather than building your own. For example, most applications require a greeting intent that starts a dialog with the user. You can add the **General** content catalog to add a group of intents that recognize and respond to utterances that are commonly used to start and end a conversation, among other things.

As you add information, the dialog skill uses this unique data to build a machine learning model that can recognize these and similar user inputs. Each time you add or change the training data, the training process is triggered to ensure that the underlying model stays up-to-date as your customer needs and the topics they want to discuss change.

For help creating a dialog skill, see [Building a dialog skill](create-dialog-skill.html).

### Search skill
{: #search-skill}

The search skill interacts with the {{site.data.keyword.discoveryfull}} service to extract information that is relevant to a customer query from a configured data collection.

If you already use the {{site.data.keyword.discoveryshort}} service, you can mine your existing data collections for source material that you can share with customers to address common questions.

However, you do not need to have a {{site.data.keyword.discoveryshort}} service instance. If you choose to create a search skill, a Lite plan instance is provisioned for you. You can then create a collection by choosing a data source, and the configuration to use to ingest documents from the data source into the collection. Lastly, configure your search skill to search this collection to find answers to queries that are typically asked by your customers.

**Attention**: Currently, a Lite plan instance of {{site.data.keyword.discoveryshort}} cannot be provisioned automatically. Create a free Lite plan instance yourself from the [IBM Cloud Catalog ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://console.bluemix.net/catalog/services/discovery).

For help creating a search skill, see [Building a search skill](create-search-skill.html).