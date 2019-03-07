---

copyright:
  years: 2015, 2019
lastupdated: "2019-02-28"

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

# Creating skill versions
{: #versions}

Versions help you manage the workflow of a dialog skill development project.
{: shortdesc}

Create a skill version to capture a snapshot of the training data (intents and entities) and dialog in the skill at key points during the development process. Being able to save an in-progress skill at a specific point in time is especially useful as you start to fine tune your assistant. You often need to make a change and see the impact of the change in real time before you can determine whether or not the change improves or lessens the effectiveness of the assistant. Based on your findings from a test environment deployment, you can make an informed decision about whether to deploy a given change to an assistant that is deployed in a production environment.

If you have a free (Lite) plan, you cannot create skill versions.
{: note}

This 2 1/2 minute video describes how using versions can help you.

<iframe class="embed-responsive-item" id="youtubeplayer" title="Creating skill versions" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/FDolnBxvcZ8" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

## Creating a version
{: #versions-create}

You can use the {{site.data.keyword.conversationshort}} tool to edit only one version of the dialog skill at a time. The in-progress version is called the *development* version.

When you save a version, any skill settings that you applied to the development version are saved also.

To create a dialog skill version, follow these steps:

1.  From the header of the skill, click **Save new version**, and then describe the current state of the skill.

    Adding a good description will help you to distinguish multiple versions from one another later.

1.  Click **Save**.

A snapshot is taken of the current skill and saved as a new version. You remain in the development version of the skill. Any changes you make continue to be applied to the development version, not the version you saved. To access the version you saved, go to the **Version History** page.

## Deploying a skill version
{: #versions-deploy}

1.  From the header of the skill, click the **Version History** tab.
1.  Click the ![Click to view actions](images/kebab-react.png) icon from the version you want to deploy, and then choose **Assign to assistant**.

    A list of assistants to which you can link this version is displayed. The list is limited to those assistants that don't have any skills associated with them, or that are associated with a different version of this skill.
1.  Click the checkbox for one or more of the assistants, and then click **Assign**.

Keep track of when this version is deployed to an assistant and for how long. It is likely that you will want to analyze user conversations that occur between users and this specific version of the skill. You can get this information from the **Analytics** page. However, when you pick a data source, versions are not listed. You must choose the name of the assistant to which you deployed this version. You can then filter the metrics data to show only those conversations that occurred between the start and end dates of the time frame during which this skill version was deployed to the assistant.
{: important}

## Skill version limits
{: #skill-version-limits}

The number of versions you can create for a single skill depends on your {{site.data.keyword.conversationshort}} plan.

| Service plan     | Versions per skill |
|------------------|-------------------:|
| Premium          |                 50 |
| Plus             |                 10 |
| Standard         |                 10 |
| Lite             |                  0 |
{: caption="Service plan details" caption-side="top"}

## Swapping skills
{: #versions-swap-skills}

If you find that an earlier version of the skill did a better job of recognizing and addressing customer needs than a later version, you can swap the skill that is linked to the assistant to use the earlier version instead.

Follow the same steps you use to [deploy a skill version](#versions-deploy) to change the version that is linked to an assistant.

## Accessing a saved version
{: #versions-view}

The only way to view a saved version is to overwrite the in-progress development version of the skill with the saved version. (But not before you have saved any work you have done in the current development version.)

You cannot edit a saved version. To achieve the same goal, you can use a saved version as the basis for a new version in which you incorporate any changes you want to make. To start development work from a saved version, overwrite the in-progress development version of the skill with the saved version.

1.  Save any changes you made to the skill since the last time you created a version.

    Save a version of the skill now. Otherwise, your work will be lost when you follow these steps.
    {: important}

1.  From the version you want to edit, click the **Skill actions** ![Skill actions](images/kebab-react.png) icon, and then choose **Copy to development** and confirm the action.

    The page refreshes to revert to the state that the skill was in when the version was created.

If you want to save any changes that you make to this version, you must save the skill as a new version. You cannot apply changes to an already-saved version.

When you open a skill by clicking the skill tile from the assistant page, the development version of the skill is displayed. Even if you associated a later version with the assistant, when you access the skill, its development version opens.
{: important}