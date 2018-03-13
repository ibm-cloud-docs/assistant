---

copyright:
  years: 2015, 2018
lastupdated: "2018-03-12"

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

# Building a conversational skill
{: #create-convo-skill}

The natural-language processing for the {{site.data.keyword.conversationshort}} service is defined in a *conversational skill*, which is a container for all of the artifacts that define a conversation flow.
{: shortdesc}

## Skill limits
{: #skill-limits}

Currently, you can add one conversational skill to an assistant.

The number of skills you can create in a single service instance depends on your {{site.data.keyword.conversationshort}} plan. The sample skills do not count toward your limit unless you edit or duplicate them:

| Service plan     | Skills per service instance |
|------------------|----------------------------:|
| Premium          |                         100 |
| Standard         |                          20 |
| Lite*            |                           5 |
{: caption="Service plan details" caption-side="top"}

*After 30 days of inactivity, an unused Lite skill might be deleted to free up space.

## Creating skills
{: #creating-skills}

You can create a skill from scratch, use a sample skill that is provided by IBM, or import a skill from a JSON file.

You use the {{site.data.keyword.conversationshort}} tool to build skills. Follow these steps to create a skill:

1.  Click the **Skills** tab.

    **Note**: If you created or were given developer role access to any workspaces that were built with the Watson Conversation service, you will see them listed here as conversational skills.

1.  Do one of the following things:
    - To create a skill from scratch, click **Create**.
    - To import a skill from a JSON file, click the ![Import skill](images/workspace_import.png) icon, and select the JSON file you want to import from.

        **Important:**

        - The imported JSON file must use UTF-8 encoding.
        - The maximum size for a skill JSON file is 10MB. If you need to import a larger skill, consider importing the intents and entities separately after you have imported the skill. (You can also import larger skills using the REST API. For more information, see the [API Reference ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/watson/developercloud/conversation/api/v1/#create_workspace){: new_window}.)
        - The JSON cannot contain tabs, newlines, or carriage returns.

        Specify the data you want to include:

        - Select **Everything (Intents, Entities, and Dialog)** if you want to import a complete copy of the exported skill, including the dialog.
        - Select **Intents and Entities** if you want to use the intents and entities from the exported skill, but you plan to build a new dialog.

1.  Specify the details for the new skill:
    - **Name**: A name no more than 64 characters in length that contains alphabetical characters (a-z), number characters (0-9), underscores (_), dashes (-) periods (.), and spaces. You cannot include an apostrophe in the name, for example. This value is required.
    - **Description**: A description no more than 128 characters in length.
    - **Language**: The language of the user input the skill will be trained to understand.

After you create the skill, it appears as a tile on the Skills page. To put the conversation to work helping your users, integrate your assistant with a messaging platform or custom application. See [Adding integrations](add-integrations.html).
