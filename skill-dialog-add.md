---

copyright:
  years: 2015, 2020
lastupdated: "2020-03-02"

keywords: import workspace, import JSON, export JSON

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
{:codeblock: .codeblock}
{:screen: .screen}
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:swift: .ph data-hd-programlang='swift'}

# Creating a dialog skill
{: #skill-dialog-add}

The natural-language processing for the {{site.data.keyword.conversationshort}} service is defined in a *dialog skill*, which is a container for all of the artifacts that define a conversation flow.
{: shortdesc}

You can add one dialog skill to an assistant. See [Skill limits](/docs/assistant?topic=assistant-skill-add#skill-add-limits) for information about limits per plan.

## Create the dialog skill
{: #skill-dialog-add-task}

You can create a skill from scratch, use a sample skill that is provided by IBM, or import a skill from a JSON file.

To add a skill, complete the following steps:

1.  Click the **Skills** icon ![Skills menu icon](images/nav-skills-icon.png), and then click **Create skill**.

1.  Click the *Dialog skill* tile, and then click **Next**.

1.  Take one of the following actions:

    - To create a skill from scratch, click **Create skill**.
    - To add a sample skill that is provided with the product as a starting point for your own skill or as an example to explore before you create one yourself, click **Use sample skill**, and then click the sample you want to use.

      The sample skill is added to your list of skills. It is not associated with any assistants. Skip the remaining steps in this procedure.

    - To add an existing skill to this service instance, you can import it as a JSON file. Click **Import skill**, and then click **Choose JSON File** and select the JSON file you want to import.

      **Important:**

      - The imported JSON file must use UTF-8 encoding, without byte order mark (BOM) encoding.
      - The JSON cannot contain tabs, newlines, or carriage returns.

      The maximum size for a skill JSON file is 10 MB. If you need to import a larger skill, consider using the REST API. For more information, see the [API Reference](https://cloud.ibm.com/apidocs/assistant/assistant-v1?curl=#create-workspace){: external}.
      {: tip}

      Click **Import**.

      If you have trouble importing a skill, see [Troubleshooting skill import issues](#skill-dialog-add-import-errors).

1.  Specify the details for the skill:

    - **Name**: A name no more than 100 characters in length. A name is required.
    - **Description**: An optional description no more than 200 characters in length.
    - **Language**: The language of the user input the skill will be trained to understand. The default value is English.

After you create the dialog skill, it appears as a tile on the Skills page. Now, you can start identifying the user goals that you want the dialog skill to address.

- To add prebuilt intents to your skill, see [Using content catalogs](/docs/assistant?topic=assistant-catalog).
- To define your own intents, see [Defining intents](/docs/assistant?topic=assistant-intents).

The dialog skill cannot interact with customers until it is added to an assistant and the assistant is deployed. See [Creating an assistant](/docs/assistant?topic=assistant-assistant-add).

### Troubleshooting skill import issues
{: #skill-dialog-add-import-errors}

If you get the message, `Error. Should NOT be shorter than 1 character`, then check whether your skill has a name. If not, add one.

If you receive a message that says the skill contains artifacts that exceed the limits imposed by your service plan, complete the following steps to import the skill successfully:

1.  Purchase a plan with higher artifact limits.
1.  Create a service instance in the new plan.
1.  Import the skill to the new service instance.
1.  Make edits to the skill such that it meets the artifact limit requirements for the plan you want to use going forward. 

    For information about how to decrease the number of dialog nodes, see [How many nodes are in my dialog?](/docs/assistant?topic=assistant-dialog-build#dialog-build-count-nodes).
1.  Export the edited skill by downloading it.
1.  Try again to import the edited skill into the original service instance on the plan you want.

### Adding the skill to an assistant
{: #skill-dialog-add-to-assistant}

You can add one dialog skill to an assistant. You must open the assistant tile and add the skill to the assistant from the assistant configuration page; you cannot choose the assistant that will use the skill from within the skill configuration page. One dialog skill can be used by more than one assistant.

1.  From the Assistants page, click to open the tile for the assistant to which you want to add the skill.

1.  Click **Add Dialog Skill**.

1.  Click **Add existing skill**.

    Click the skill that you want to add from the available skills that are displayed.

When you add a dialog skill from here, you get the development version. If you want to add a specific skill version, add it from the skill's *Versions* page instead.

## Downloading a dialog skill
{: #skill-dialog-add-download}

You can download a dialog skill in JSON format. You might want to download a skill if you want to use the same dialog skill in a different instance of the {{site.data.keyword.conversationshort}} service, for example. You can download it from one instance and import it to another instance as a new dialog skill.

To download a dialog skill, complete the following steps:

1.  Find the dialog skill tile on the Skills page or on the configuration page of an assistant that uses the skill.

1.  Click the ![open and close list of options](images/kabob-beta.png) icon, and then choose **Export**.

1.  Specify a name for the JSON file and where to save it, and then click **Save**.

You can export a skill by using the API also. Include the `export=true` parameter with the request. See the [API reference](https://cloud.ibm.com/apidocs/assistant/assistant-v1#get-information-about-a-workspace){: external} for more details.

For information about how to download a specific skill version, see [Downloading a skill version](/docs/assistant?topic=assistant-versions-export).

## Sharing a dialog skill with team members
{: #skill-dialog-add-invite-others}

After you create the service instance, you can give other people access to it. Together, you can define the training data and build the dialog.

Only one person can edit an intent, entity, or a dialog node at a time. If multiple people work on the same item at the same time, then the changes made by the person who saves their changes last are the only changes applied. Changes that are made during the same time frame by someone else and are saved first are not retained. Coordinate the updates that you plan to make with your team members to prevent anyone from losing their work.
{: important}

To share a dialog skill with other people, you must give them access to the service instance that hosts the skill. Note that the person you invite will be able to access any skill or assistant in this service instance.

1.  Click the User ![User](images/user-icon2.png) icon in the page header.

1.  Make a note of the current service instance name, and then select **Manage Users** from the drop-down.

1.  From the navigation pane, click **Users**.

    If you gave someone access to a service instance previously, then the person might be listed as an invited user already. To change the person's level of access to the instance, click the menu next to their name, choose **Assign access**, and then click **Assign access to resources**.
    
1.  Click **Invite users**. 

1.  Add the email address of the person with whom you want to share the instance. You can add more than one person.

1.  Expand the *Assign users additional resources* section, and then click **IAM Services**.

1.  In the *No access* field, choose {{site.data.keyword.conversationshort}}.

    The *Account* resource group is used unless you change it.

1.  Select at least one region and at least one service instance to share with this user.

    Remember, you made a note of the name of the current service instance in an earlier step. You can select it from the list of service instances if you want to give the person access to that service instance only.

1.  Give this user the following assignments at a minimum:
 
    - **Platform access roles**: Operator
    - **Service access roles**: Writer

    The service access role assignment is currently ignored in all data centers except Tokyo and Seoul. Any person you invite is given Manager access to the instance and all of its skills and assistants, regardless of the *Service access role* you assign to them. When service access roles are supported in all data centers, your choose will matter.
    {: important}

1.  Click **Add**.

    You can repeat the previous step to invite other users and apply different roles to them. 

1.  Click **Invite** to finish the process.

    If you are editing access for an existing user, click **Assign access**.

When the people you invite next log in to {{site.data.keyword.cloud}}, your account will be included in their list of accounts. If they select your account, they can see your service instance, and open and edit your skills.

With more people contributing to dialog skill development, unintended changes can occur, including skill deletions. Consider creating backup copies of your dialog skill on a regular basis, so you can roll back to an earlier version if necessary. To create a backup, simply [download the skill as a JSON file](#skill-dialog-add-download).
