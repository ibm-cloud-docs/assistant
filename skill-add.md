---

copyright:
  years: 2015, 2019
lastupdated: "2019-02-21"

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

# Creating a skill
{: #skill-add}

The natural-language processing for the {{site.data.keyword.conversationshort}} service is defined in a *dialog skill*, which is a container for all of the artifacts that define a conversation flow.
{: shortdesc}

You can add one skill to an assistant.

You can create a skill from scratch, use a sample skill that is provided by IBM, or import a skill from a JSON file. To add a skill, complete the following steps:

1.  If you have not created a {{site.data.keyword.conversationshort}} service instance, perform this one-time procedure first. Otherwise, skip this step.

    1.  Go to the [{{site.data.keyword.conversationshort}} ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://{DomainName}/catalog/services/watson-assistant) page in the {{site.data.keyword.cloud_notm}} catalog.

    1.  Sign up for an {{site.data.keyword.cloud_notm}} account or log in.

        The service instance will be created in the **default** resource group if you do not choose a different one, and it *cannot* be changed later. *Default* is typically sufficient. But if you want to learn more about resource groups, see the [{{site.data.keyword.Bluemix_notm}} documentation ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://{DomainName}/docs/resources?topic=resources-bp_resourcegroups#bp_resourcegroups){: new_window}.

        You can use one service instance to develop, test, and deploy an assistant, so do not create distinct resource groups for different types of deployment environments.
        {:tip}

        With a Premium plan, you can create multiple instances. The instances must all be created in the same resource group.

    1.  Click **Create**.

    1.  Click **Launch tool**. If you're prompted to log in to the tool, provide your {{site.data.keyword.cloud_notm}} credentials.

1.  Click the **Skills** tab.

    If you created or were given developer role access to any workspaces that were built with the generally available version of the  {{site.data.keyword.conversationshort}} service (formerly Watson Conversation), you will see them listed here as dialog skills.
    {: note}

1.  Click **Create new**.

1.  Do one of the following things:

    - To create a skill from scratch, click **Create skill**.
    - To add a sample skill that is provided with the service as a starting point for your own skill or as an example to explore before you create one yourself, click **Use sample skill**, and then click the sample you want to use.

      The sample skill is added to your list of skills. It is not associated with any assistants. Skip the remaining steps in this procedure.

    - To add an existing skill to this service instance, you can import it as a JSON file. Click **Import skill**, and then click **Choose JSON File**, and select the JSON file you want to import.

      **Important:**

      - The imported JSON file must use UTF-8 encoding, without byte order mark (BOM) encoding.
      - The maximum size for a skill JSON file is 10MB. If you need to import a larger skill, consider importing the intents and entities separately after you have imported the skill. (You can also import larger skills using the REST API. For more information, see the [API Reference ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://cloud.ibm.com/apidocs/assistant?curl=#create-workspace){: new_window}.)
      - The JSON cannot contain tabs, newlines, or carriage returns.

      Specify the data you want to include:

        - Select **Everything (Intents, Entities, and Dialog)** if you want to import a complete copy of the exported skill, including the dialog.
        - Select **Intents and Entities** if you want to use the intents and entities from the exported skill, but you plan to build a new dialog.

      Click **Import**.

      If you have trouble importing a skill, see [Troubleshooting skill import issues](#skill-add-import-errors).

1.  Specify the details for the skill:

    - **Name**: A name no more than 100 characters in length. A name is required.
    - **Description**: An optional description no more than 200 characters in length.
    - **Language**: The language of the user input the skill will be trained to understand. The default value is English.

After you create the skill, it appears as a tile on the Skills page. Now, you can start identifying the user goals that you want the dialog skill to address.

- To add prebuilt intents to your skill, see [Using content catalogs](/docs/services/assistant?topic=assistant-catalog).
- To define your own intents, see [Defining intents](/docs/services/assistant?topic=assistant-intents).

The dialog skill cannot interact with customers until it is added to an assistant and the assistant is deployed. See [Creating an assistant](/docs/services/assistant?topic=assistant-assistant-add).

### Troubleshooting skill import issues
{: #skill-add-import-errors}

If you receive a message that says the skill contains artifacts that exceed the limits imposed by your service plan, complete the following steps to import the skill successfully:

1.  Purchase a plan with higher artifact limits.
1.  Create a service instance in the new plan.
1.  Import the skill to the new service instance.
1.  Make edits to the skill such that it meets the artifact limit requirements for the plan you want to use going forward. You might need to decrease the number of dialog nodes used in the dialog tree, for example.
1.  Export the edited skill by downloading it.
1.  Try again to import the edited skill into the original service instance on the plan you want.

### Adding the skill to an assistant
{: #skill-add-to-assistant}

You can add one skill to an assistant. You must open the assistant tile and add the skill to the assistant from the assistant configuration page; you cannot choose the assistant that will use the skill from within the skill configuration page. One dialog skill can be used by more than one assistant.

1.  From the Assistants tab, click to open the tile for the assistant to which you want to add the skill.

1.  Click **Add Dialog Skill**.

1.  Click **Add existing skill**.

    Click the skill that you want to add from the available skills that are displayed.

    When you add a skill from here, you get the development version. If you want to add a specific skill version, add it from the skill's *Version History* tab instead.

## Skill limits
{: #skill-add-limits}

The number of skills you can create in a single service instance depends on your {{site.data.keyword.conversationshort}} plan. Any sample dialog skills that are available in your service instance do not count toward your limit unless you edit or duplicate them.

| Service plan     | Skills per service instance |
|------------------|----------------------------:|
| Premium          |                          50 |
| Plus             |                          50 |
| Standard         |                          20 |
| Lite*            |                           5 |
{: caption="Service plan details" caption-side="top"}

*After 30 days of inactivity, an unused skill in a Lite plan service instance might be deleted to free up space.

## Downloading a dialog skill
{: #skill-add-download}

You can download a dialog skill in JSON format. You might want to download a skill if you want to use the same dialog skill in a different instance of the {{site.data.keyword.conversationshort}} service, for example. You can download it from one instance and import it to another instance as a new dialog skill.

To download a dialog skill, complete the following steps:

1.  Find the dialog skill tile on the Skills page or on the configuration page of an assistant that uses the skill.

1.  Click the ![open and close list of options](images/kabob-beta.png) icon, and then choose **Download JSON**.

1.  Specify a name for the JSON file and where to save it, and then click **Save**.

You can export a skill by using the API also. Include the `export=true` parameter with the request. See the [API reference](https://cloud.ibm.com/apidocs/assistant#get-information-about-a-workspace) for more details.

## Deleting a skill
{: #skill-add-delete}

You can delete any skill that you can access, unless it is being used by an assistant. If it is in use, you must remove it from the assistant that is using it before you can delete it.

Be sure to check with anyone else who might be using the skill before you delete it.
{: tip}

To delete a skill, complete the following steps:

1.  Find out whether the skill is being used by any assistants. From the Skills tab, find the tile for the skill that you want to delete. The **Assistants** field lists the assistants that currently use the skill.

1.  If the skill you want to delete is associated with an assistant, then remove it from the assistant by completing the following steps:

    - Check with the owner of the assistant that is using the skill before you remove the skill from it.
    - Open the Assistants tab, and then click to open the assistant tile.
    - Find the tile for the skill that you want to delete. Click the ![open and close list of options](images/kabob-beta.png) icon, and then choose **Remove**.
    - Repeat the previous steps for any other assistants that use the skill.
    - Return to the Skills tab and find the tile for the skill that you want to delete.

1.  Click the ![open and close list of options](images/kabob-beta.png) icon, and then choose **Delete**. Confirm the deletion.

## Sharing a dialog skill with team members
{: #skill-add-invite-others}

After you create the service instance, you can give other people access to it. Together, you can define the training data and build the dialog.

**Important**: Only one person can edit an intent, entity, or a dialog node at a time. If multiple people work on the same item at the same time, then the changes made by the person who saves their changes last are the only changes applied. Changes that are made during the same time frame by someone else and are saved first are not retained. Coordinate the updates that you plan to make with your team members to prevent anyone from losing their work.

To share a dialog skill with other people, you must give them access to the service instance that hosts the skill. Note that the person you invite will be able to access any skill that is hosted by this service instance.

1.  Make a note of the current instance name, which is displayed at the top of the current page.
1.  Click the User ![User](images/user-icon2.png) icon in the page header, and select **Manage Users** from the drop-down.

1.  Click **Invite users**, and then enter the email addresses of the people on your team to whom you want to give access.

    If you gave someone access to a service instance when it was managed by Cloud Foundry, then the person might be listed as an invited user already. Click the person's name to open the access management settings for the user. Click **Assign access**, and then choose **Assign access to resources**.

1.  In the *Services* section, make the following selections at a minimum:

    - **Services**: {{site.data.keyword.conversationshort}}
    - **Assign platform access roles**: Operator

    For more information about platform management roles, see [IAM access ![External link icon](../../icons/launch-glyph.svg "External link icon")](/docs/iam?topic=iam-userroles). (Service access roles are not leveraged by {{site.data.keyword.conversationshort}} by default.)

    For older instances that are managed by Cloud Foundry, you must expand the *Cloud Foundry access* section, choose your organization, and then assign the person to the **Developer** space role.
    {: note}

1.  Click **Invite users**.

    If you are editing access for an existing user, click **Assign access**.

When the people you invite next log in to {{site.data.keyword.cloud_notm}}, your account will be included in their list of accounts. If they select your account, they can see your service instance, and open and edit your skills.

With more people contributing to dialog skill development, unintended changes can occur, including skill deletions. Consider creating backup copies of your dialog skill on a regular basis, so you can roll back to an earlier version if necessary. To create a backup, simply [download the skill as a JSON file](#skill-add-download).
