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

1.  If the "Service Details" page is not already open, click your {{site.data.keyword.conversationshort}} service instance from the console. (When you create a service instance, the "Service Details" page is displayed.)

1.  Click **Launch tool**.

1.  Click the **Skills** tab.

    **Note**: If you created or were given developer role access to any workspaces that were built with the Watson Conversation service, you will see them listed here as conversational skills.

1.  Do one of the following things:
    - To create a skill from scratch, click **Create**.
    - To use the sample as a starting point for your own skill, edit the sample skill.
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

## Exporting skills
{: #exporting-skills}

From the Skills page, you can export skills.

Exporting a skill saves a copy of all skill data in a JSON file. The skill data includes intents and entities that you defined as part of the training data. It also includes counter examples that are created when you identify inaccurate classifications by marking them as irrelevant.  Use this option if you want to import the skill into a different service instance or save a copy of the skill data offline. When you import a conversational skill, you can choose to import only the intents and entities, which can be useful if you want to build a new dialog using the same training data.

- To export an existing skill to a JSON file, click the menu icon in the skill tile and then select **Download JSON**.

## Sharing a conversational skill with team members
{: #invite-others}

After you create the service instance, you can give other people access to it. Together, you can define the training data and build the dialog.

**Important**: Only one person can edit an intent, entity, or a dialog node at a time. If multiple people work on the same item at the same time, then the changes made by the person who saves their changes last are the only changes applied. Changes that are made during the same time frame by someone else and are saved first are not retained. Coordinate the updates that you plan to make with your team members to prevent anyone from losing their work.

To share a conversational skill with other people, you must give them developer access to the service instance that hosts the conversational skill. If the instance you share hosts other conversational skills that you do not want others to edit, then be sure to make that clear to anyone that you invite.

1.  Go to the {{site.data.keyword.watson}} Developer Console [Projects ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://console.{DomainName}/developer/watson/projects) page, and log in. Click **Manage > Account > Users** from the menu.
1.  Click **Invite users**, and then enter the email addresses of the people on your team to whom you want to give access.
1.  In the *Cloud Foundry access* section, choose your organization from the *Organization* list.

    The *Organization roles* field is automatically filled with *Auditor*. You can keep the default value in the field.
1.  Optionally, limit the access you are granting to the conversational skills in a single region and space.
1.  In the *Space roles* field, choose **Developer**.

    See [Cloud Foundry roles ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://console.bluemix.net/docs/iam/cfaccess.html#cfroles) for more information about the roles.
1.  Click **Invite users**.

When the people you invite next log in to {{site.data.keyword.cloud_notm}}, your organization will be included in their list of accounts.

With more people contributing to conversational skill development, unintended changes can occur, including skill deletions. Consider creating backup copies of your conversational skill on a regular basis, so you can roll back to an earlier version if necessary. To create a backup, simply export the converational skill as a JSON file.