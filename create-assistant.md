---

copyright:
  years: 2015, 2018
lastupdated: "2018-03-07"

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

# Creating assistants
{: #creating-assistants}

Create an assistant with the right set of skills to address the business needs of your customers.
{: shortdesc}

## Assistant limits
{: #assistant-limits}

The number of assistants you can create in a single service instance depends on your {{site.data.keyword.conversationshort}} plan.

| Service plan     | Assistants per service instance | Integrations per assistant  |
|------------------|--------------------------------:|----------------------------:|
| Premium          |                             100 |                         100 |
| Standard         |                             100 |                         100 |
| Lite*            |                             100 |                         100 |
{: caption="Service plan details" caption-side="top"}

*After 30 days of inactivity, an unused Lite assistant might be deleted to free up space.

Currently, you can connect one skill to your assistant. The number of skills you can build differs depending on the plan you have. See [Skill limits](create-convo-skill.html#skill-limits) for more details.

## Create an assistant
{: #create-assistant}

If you have not done so, follow the [getting started](getting-started.html#prerequisites) steps to create a {{site.data.keyword.conversationshort}} service instance and launch the tool.

Follow these steps to create an assistant:

1.  From the home page of the {{site.data.keyword.conversationshort}} tool, click the **Assistants** tab.

1.  Click **Create new**.

1.  Specify the details for the new assistant:
    - **Name**: A name no more than 64 characters in length that contains alphabetical characters (a-z), number characters (0-9), underscores (_), dashes (-) periods (.), and spaces. You cannot include an apostrophe in the name, for example. This value is required.
    - **Description**: A description no more than 128 characters in length.
    - **Language**: The language the assistant will use to communicate with your customers.

    After you create the assistant, it appears as a tile on the Assistants page.

1.  Add a skill to the assistant. See [Building a conversational skill](create-convo-skill.html) for details.

    **Note**: You must add a skill to the assistant for the creation of the assistant to be fully completed.
