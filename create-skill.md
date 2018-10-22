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

# Creating a skill
{: #create-skill}

A skill is a container for the artificial intelligence that enables an assistant to help your customers.
{: shortdesc}

You can create one of the following types of skills:

- **Dialog skill**: Uses Watson natural language processing and machine learning technologies to understand user questions and requests, and respond to them with answers that are authored by you.
- **Search skill**: For a given user query, uses the {{site.data.keyword.discoveryfull}} service to retrieve information from a data source that you identify and shares any relevant information that it finds as the response to the user.

## Skill limits
{: #skill-limits}

The number of skills you can create in a single service instance depends on your {{site.data.keyword.conversationshort}} plan. Any sample dialog skills that are available in your service instance do not count toward your limit unless you edit or duplicate them.

| Service plan     | Skills per service instance |
|------------------|----------------------------:|
| Premium          |                         100 |
| Standard         |                          20 |
| Lite*            |                           5 |
{: caption="Service plan details" caption-side="top"}

*After 30 days of inactivity, an unused skill in a Lite plan service instance might be deleted to free up space.

## Creating a skill
{: #create-a-skill}

You can add one skill to an assistant.

To create a skill, complete the following steps:

1.  From the Skills tab, click **Add** for the type of skill that you want to add, and then follow the remaining steps in the appropriate procedure to complete the skill creation process.

      - [Dialog Skill](create-dialog-skill.html)
      - [Search Skill](create-search-skill.html)

## Deleting a skill
{: #delete-skill}

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
