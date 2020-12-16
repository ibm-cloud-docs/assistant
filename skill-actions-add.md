---

copyright:
  years: 2020
lastupdated: "2020-12-16"

subcollection: assistant

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:external: target="_blank" .external}
{:deprecated: .deprecated}
{:important: .important}
{:note: .note}
{:tip: .tip}
{:pre: .pre}
{:preview: .preview}
{:codeblock: .codeblock}
{:screen: .screen}
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:swift: .ph data-hd-programlang='swift'}
{:table: .aria-labeledby="caption"}

# Adding an actions skill ![Beta](images/beta.png)
{: #skill-actions-add}

Add an actions skill to design a content-rich dialog for your assistant.
{: shortdesc}

## Add the actions skill
{: #skill-actions-add-task}

An actions skill contains actions that represent the tasks you want your assistant to help your customers with.

Each action contains a series of steps that represent individual exchanges with a customer. Building the conversation that your assistant has with your customers is fundamentally about deciding which steps, or which user interactions, are required to complete an action. After you identify the list of steps, you can then focus on writing engaging content to turn each interaction into a positive experience for your customer.

To add an actions skill, complete the following steps:

1.  Click **Create Skill**.

1.  Select the **Actions skill**, and then click **Next**.

1.  Take one of the following actions:

    - To add an actions skill that was downloaded previously to this service instance, you can import it as a JSON file. Click **Import skill**. Drag a file or click **Drag and drop file here or click to select a file** and select the actions skill JSON file you want to import.

      The imported JSON file must use UTF-8 encoding, without byte order mark (BOM) encoding. The JSON cannot contain tabs, newlines, or carriage returns.
      {: important}

      Click **Import**.

    - To create a new actions skill, stay on the *Create skill* tab.

1.  Specify the details for the skill:

    - **Name**: A name no more than 64 characters in length. A name is required.
    - **Description**: An optional description no more than 128 characters in length.
    - **Language**: The language of the user input the skill will be trained to understand. The default value is English.

      For more information about supported languages, see [Supported languages](/docs/assistant?topic=assistant-language-support).

1.  Click **Create actions skill**.

If you add only an actions skill to the assistant, the action skill starts the conversation. If you add both a dialog skill and actions skill to an assistant, the dialog skill starts the conversation. And actions are recognized only if you configure the dialog skill to call them.

Add actions to your skill. For more information, see [Adding actions](/docs/assistant?topic=assistant-actions).