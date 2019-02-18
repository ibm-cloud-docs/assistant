---

copyright:
  years: 2015, 2019
lastupdated: "2019-02-18"

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

# Backing up and restoring data
{: #backup}

Back up and restore your data by exporting, and then importing the data.
{: shortdesc}

You can export the following data from a {{site.data.keyword.conversationshort}} service instance:

- Dialog skill training data (intents and entities)
- Dialog skill dialog

You cannot export the following data:

<!--- Search skill -->
- Assistant, including any configured integrations

## Retaining logs
{: #backup-retain-logs}

If you want to store logs of conversations that users have had with your assistant, you can use the `/logs` API to export your log data. See [API reference ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://cloud.ibm.com/apidocs/assistant#list-log-events-in-a-workspace) for details.

To get the workspace ID for a skill, from the skill tile, click the ![open and close list of options](images/kabob-beta.png) icon, and then choose **View API Details**.
{: tip}

Logs are stored for a different amount of time depending on your service plan. For example, Lite plans provide logs from the past 7 days only. See [Log limits](/docs/services/assistant/logs.html#logs-limits) for more information.

You cannot import logs from one skill into another skill.

## Exporting a dialog skill
{: #backup-export-skill}

To back up dialog skill data, export the skill as a JSON file, and store the JSON file.

1.  Find the dialog skill tile on the Skills page or on the configuration page of an assistant that uses the skill.

1.  Click the ![open and close list of options](images/kabob-beta.png) icon, and then choose **Download JSON**.

1.  Specify a name for the JSON file and where to save it, and then click **Save**.

Alternatively, you can use the `/workspaces` API to export a dialog skill. Include the `export=true` parameter with the GET workspace request. See the [API reference ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://cloud.ibm.com/apidocs/assistant#get-information-about-a-workspace) for more details.

## Importing a dialog skill
{: #backup-import-skill}

To reinstate a backup copy of a dialog skill that you exported from another service instance or environment, create a new dialog skill by importing the JSON file of the dialog skill you exported.

If the {{site.data.keyword.conversationshort}} service changes between the time you export the skill and import it, due to functional updates which are regularly applied to instances in cloud-hosted continuous delivery environments, your imported skill might function differently than it did before.
{: important}

1.  Click the **Skills** tab.

1.  Click **Create new**.

1.  Click **Import skill**, and then click **Choose JSON File**, and select the JSON file you want to import.

    **Important:**

    - The imported JSON file must use UTF-8 encoding, without byte order mark (BOM) encoding.
    - The maximum size for a skill JSON file is 10MB. If you need to import a larger skill, consider importing the intents and entities separately after you have imported the skill. (You can also import larger skills using the REST API. For more information, see the [API Reference ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://cloud.ibm.com/apidocs/assistant#create-workspace){: new_window}.)
    - The JSON file cannot contain tabs, newlines, or carriage returns.

    Select **Everything (Intents, Entities, and Dialog)** to import a complete copy of the exported skill.

    Click **Import**.

    If you have trouble importing a skill, see [Troubleshooting skill import issues](/docs/services/assistant/skill-add.html#skill-add-import-errors).

1.  Specify the details for the skill:

    - **Name**: A name no more than 100 characters in length. A name is required.
    - **Description**: An optional description no more than 200 characters in length.
    - **Language**: The language of the user input the skill will be trained to understand. The default value is English.

After you create the skill, it appears as a tile on the Skills page.

## Re-creating your assistant
{: #backup-recreate-assistant}

You can now re-create your assistant. You can then link your imported dialog skill to the assistant, and configure integrations for it.

See [Creating assistants](/docs/services/assistant/assistant-add.html) and [Adding integrations](/docs/services/assistant/deploy-integration-add.html#deploy-integration-add-task) for more details.

## Update your client applications
{: #backup-update-api}

When you import a dialog skill that you exported, a new skill is created. The new skill has a new workspace ID. If you have existing client applications that use the v1 API to access this skill, then you must update any workspace ID references to use the new worskpace ID instead.

When you re-create your assistant, it is given a new assistant ID. If you have existing client applications that use the v2 API to access the assistant, then you must update any assistant ID references to use the new assistant ID instead.
