---

copyright:
  years: 2018, 2021
lastupdated: "2021-01-06"

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

{{site.data.content.newlink}}

# Working with skills 
{: #skill-tasks}

Perform common tasks, such as renaming or deleting a skill.
{: shortdesc}

## Renaming a skill
{: #skill-tasks-rename}

You can change the name of a skill and its associated description after you create the skill.

To rename a skill, follow these steps:

1.  From the Skills page, find the skill that you want to rename.

1.  Click the ![open and close list of options](images/kebab.png) icon, and then choose **Rename**.

1.  Edit the name, and then click **Save**.

## Deleting a skill
{: #skill-tasks-delete}

You can delete any skill that you can access, unless it is being used by an assistant. If it is in use, you must remove it from the assistant that is using it before you can delete it.

Be sure to check with anyone else who might be using the skill before you delete it.
{: tip}

To delete a skill, complete the following steps:

1.  Find out whether the skill is being used by any assistants. From the Skills page, find the tile for the skill that you want to delete. The **Assistants** field lists the assistants that currently use the skill.

1.  If the skill you want to delete is associated with an assistant, then remove it from the assistant by completing the following steps:

    - Check with the owner of the assistant that is using the skill before you remove the skill from it.
    - Open the Assistants page, and then click to open the assistant tile.
    - Find the tile for the skill that you want to delete. Click the ![open and close list of options](images/kebab.png) icon, and then choose **Remove**.
    - Repeat the previous steps for any other assistants that use the skill.
    - Return to the Skills page and find the tile for the skill that you want to delete.

1.  Click the ![open and close list of options](images/kebab.png) icon, and then choose **Delete**. Confirm the deletion.

## Downloading a skill
{: #skill-tasks-download}

You can download a dialog or actions skill in JSON format. You might want to download a skill if you want to use the same skill in a different instance of the {{site.data.keyword.conversationshort}} service. You can download a dialog skill from one instance and upload it to another instance as a new skill, for example.

You cannot download a search skill.

To download a skill, complete the following steps:

1.  Find the skill tile on the Skills page or on the configuration page of an assistant that uses the skill.

1.  Click the ![open and close list of options](images/kebab.png) icon, and then choose **Download**.

1.  Specify a name for the JSON file and where to save it, and then click **Save**.

For dialog skills only:

- You can download a dialog skill by using the API also. Include the `export=true` parameter with the request. See the [API reference](https://cloud.ibm.com/apidocs/assistant/assistant-v1#getworkspace){: external} for more details.
- For information about how to download a specific dialog skill version, see [Downloading a skill version](/docs/assistant?topic=assistant-versions#versions-export).

## Overwriting a skill
{: #skill-tasks-overwrite}

To overwrite or replace an existing skill, upload the new version of the skill as a JSON file into the existing skill. 

You can overwrite a dialog or actions skill. You cannot overwrite a search skill.

To overwrite a skill, complete the following steps:

1.  Click the **Skills** icon ![Skills menu icon](images/nav-skills-icon.png) to open the Skills page.

1.  Find the skill that you want to replace.

1.  Click the ![open and close list of options](images/kebab.png) icon, and then choose **Upload**. 

1.  Drag a file or click **Drag and drop file here or click to select a file** and select the JSON file you want to overwrite your skill with.

    The uploaded JSON file must use UTF-8 encoding, without byte order mark (BOM) encoding. The JSON cannot contain tabs, newlines, or carriage returns.
    {: important}

    For dialog skills only: The maximum size for a skill JSON file is 10 MB. If you need to upload a larger skill, consider using the REST API. For more information, see the [API Reference](https://cloud.ibm.com/apidocs/assistant/assistant-v1?curl=#createworkspace){: external}.
    {: tip}

1.  Click **Upload and overwrite**.

    If you have trouble uploading a dialog skill, see [Troubleshooting skill upload issues](#skill-dialog-add-import-errors).
