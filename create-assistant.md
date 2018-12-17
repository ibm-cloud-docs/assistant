---

copyright:
  years: 2015, 2018
lastupdated: "2018-12-17"

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

Create an assistant with the skills it needs to address the business goals of your customers.
{: shortdesc}

## Assistant limits
{: #assistant-limits}

The number of assistants you can create in a single service instance depends on your {{site.data.keyword.conversationshort}} plan.

| Service plan | Assistants per service instance | Integrations per assistant  | Chat session duration |
|--------------|--------------------------------:|----------------------------:|-----------------:|
| Premium      |                             100 |                         100 |       60 minutes |
| Plus         |                             100 |                         100 |       60 minutes |
| Standard     |                             100 |                         100 |        5 minutes |
| Lite*        |                             100 |                         100 |        5 minutes |
{: caption="Service plan details" caption-side="top"}

*After 30 days of inactivity, an unused assistant in a Lite plan service instance might be deleted to free up space.

You can connect one skill to your assistant. The number of skills you can build differs depending on the plan you have. See [Skill limits](create-skill.html#skill-limits) for more details.

## Create an assistant
{: #create-assistant}

Follow these steps to create an assistant:

1.  Click the **Assistants** tab.

1.  Do one of the following things:

    - To create a sample assistant that you can review and learn from, click **Add a sample**, and then choose the sample assistant to create.

      The sample assistant is added. You can skip the remaining steps in this procedure.

      **Note**: A sample skill is provided with the sample assistant and is added to your list of skills. If you created a sample skill of the same type already, then the existing skill is associated with this new sample assistant automatically.
    - To create an assistant from scratch, click **Create new**, and then complete the remaining steps in this procedure.

1.  Specify the details for the new assistant:
    - **Name**: A name no more than 100 characters in length. A name is required.
    - **Description**: An optional description no more than 200 characters in length.

    An IBM-branded public web page is created for you automatically that you and your team can use to test your assistant. If you do not want the preview web page to be created, deselect the **Enable Preview Link** checkbox.

1.  Click **Create**.

1.  Add a skill to the assistant by clicking **Add skill**. You can choose to add an existing skill or create a new one.

    **Note**: If you created or were given developer role access to any workspaces that were built with the generally available version of the {{site.data.keyword.conversationshort}} service (formerly Watson Conversation), you will see them listed as existing dialog skills.

    See [Creating a skill](create-skill.html) for more information about how to create a skill.

## Deleting an assistant
{: #delete-assistant}

When you delete an assistant, any integrations that you defined for the assistant are automatically deleted also. Skills that you added to the assistant are not deleted.

To delete an assistant, follow these steps:

1.  From the Assistants tab, find the assistant that you want to delete.

1.  Click the ![open and close list of options](images/kabob-beta.png) icon, and then choose **Delete**. Confirm the deletion.

## Renaming an assistant
{: #rename-assistant}

You can change the name of an assistant and its associated description after you create the assistant.

To rename an assistant, follow these steps:

1.  From the Assistants tab, find the assistant that you want to rename.

1.  Click the ![open and close list of options](images/kabob-beta.png) icon, and then choose **Rename**.

1.  Edit the name, and then click **Rename** to save your changes.

### Changing the skill that is associated with the assistant
{: #swap-skill}

You can add one skill to an assistant. If you want to change the skill that your assistant uses, you can swap one skill for another skill.

1.  From the Assistants tab, click to open the tile for the assistant for which you want to change the skill.

1.  Click the ![open and close list of options](images/kabob-beta.png) icon on the skill tile, and then choose **Swap**.

1.  Choose an existing skill to use instead or [create a skill](create-skill.html).

### Switching between service instances
{: #switch-instance}

If you have more than one service instance, you can check the page header to find out which instance you are currently using. If you are working in a skill, click the **Skills** breadcrumb link first. The banner displays the current instance name. To switch to a different service instance, click **change**, and then choose the appropriate instance.
