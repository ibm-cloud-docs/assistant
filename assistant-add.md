---

copyright:
  years: 2018, 2023
lastupdated: "2022-01-11"

subcollection: assistant

---

{{site.data.keyword.attribute-definition-list}}

Documentation for the **classic {{site.data.keyword.assistant_classic_short}}** experience has moved. For the most up-to-date version, see [Adding more assistants](/docs/watson-assistant?topic=watson-assistant-assistant-add){: external} and [Renaming or deleting assistants](/docs/watson-assistant?topic=watson-assistant-assistant-rename-delete){: external}. To see all documentation for the new {{site.data.keyword.conversationshort}}, please go [here](https://cloud.ibm.com/docs/watson-assistant){: external}.
{: attention}

# Creating an assistant
{: #assistant-add}

Create an assistant with the skills it needs to address the business goals of your customers.
{: shortdesc}

To learn more about what an assistant is first, see [Assistants](/docs/assistant?topic=assistant-assistants).

Follow these steps to create an assistant:

1.  Click the **Assistants** icon ![Assistants menu icon](images/nav-ass-icon.png).

1.  Click **Create assistant**.

1.  Add details about the new assistant:

    - **Name**: A name no more than 100 characters in length. A name is required.
    - **Description**: An optional description no more than 200 characters in length.

1.  Click **Create assistant**.

1.  Add a skill to the assistant.

    **Note**: You can choose to add an existing skill or create a new one.

    See [Adding a skill to an assistant](/docs/assistant?topic=assistant-skill-add).

## Assistant limits
{: #assistant-add-limits}

The number of assistants you can create depends on your {{site.data.keyword.assistant_classic_short}} [plan type](https://www.ibm.com/products/watsonx-assistant/pricing){: external}.

After 30 days of inactivity, an unused assistant in a Lite plan service instance might be deleted to free up space. See [Changing the inactivity timeout setting](/docs/assistant?topic=assistant-assistant-settings) for more information on the subject.

You can connect one skill of each type to your assistant. The number of skills you can build differs depending on the plan you have. See [Skill limits](/docs/assistant?topic=assistant-skill-add#skill-add-limits) for more details.

If you want to create an assistant in a different service instance, you can switch to another service instance. See [Switching the service instance](/docs/assistant?topic=assistant-assistant-settings#assistant-settings-switch-instance).

## Deleting an assistant
{: #assistant-add-delete}

When you delete an assistant, any integrations that you defined for the assistant are automatically deleted also. Skills that you added to the assistant are not deleted.

To delete an assistant, follow these steps:

1.  From the Assistants page, find the assistant that you want to delete.

1.  Click the ![open and close list of options](images/kebab.png) icon, and then choose **Delete**. Confirm the deletion.

## Renaming an assistant
{: #assistant-add-rename}

You can change the name of an assistant and its associated description after you create the assistant.

To rename an assistant, follow these steps:

1.  From the Assistants page, find the assistant that you want to rename.

1.  Click the ![open and close list of options](images/kebab.png) icon, and then choose **Rename**.

1.  Edit the name, and then click **Save**.

### Changing the skill that is associated with the assistant
{: #assistant-add-swap-skill}

You can add one conversation skill (actions or dialog) and one search skill to an assistant. If you want to change a skill that your assistant is using, you can swap one conversation skill or one search skill for another skill.

1.  From the Assistants page, open the assistant.

1.  Click the ![open and close list of options](images/kebab.png) icon for the skill you want to swap, and then choose **Swap skill**.

    To swap the current dialog skill for a different version of the skill, choose **Change skill version**.

1.  Choose an existing skill to use instead or [create a skill](/docs/assistant?topic=assistant-skill-add).
