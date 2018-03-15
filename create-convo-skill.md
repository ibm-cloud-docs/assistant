---

copyright:
  years: 2015, 2018
lastupdated: "2018-03-15"

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

    **Note**: If you created or were given developer role access to any workspaces that were built with the generally available version of the  {{site.data.keyword.conversationshort}} service, you will see them listed here as conversational skills.

1.  Click **Create new**.

1.  Specify the details for the new skill:
    - **Name**: A name no more than 64 characters in length that contains alphabetical characters (a-z), number characters (0-9), underscores (_), dashes (-) periods (.), and spaces. You cannot include an apostrophe in the name, for example. This value is required.
    - **Description**: A description no more than 128 characters in length.
    - **Language**: The language of the user input the skill will be trained to understand.

After you create the skill, it appears as a tile on the Skills page. Now, you can start identifying the user goals that you want the assistant to address. See [Planning your intents and entities](intents-entities.html) for more details.
