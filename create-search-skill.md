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

# Building a search skill
{: #create-skill}

The *search skill* interacts with the {{site.data.keyword.discoveryfull}} service to extract information that is relevant to a customer query from a configured data source.
{: shortdesc}

You can add one search skill to an assistant. See [Skill limits](create-skill.html#skill-limits) for information about limits per plan.

The search skill searches for information from a data collection that you create by using the {{site.data.keyword.discoveryshort}} service. To read more about {{site.data.keyword.discoveryshort}}, see the [product documentation](https://console.bluemix.net/docs/services/discovery/index.html).

The {{site.data.keyword.discoveryshort}} service is triggered when user input is submitted through one of the assistant's integration channels.

## Creating a search skill
{: #creating-search-skill}

If you have not done so, complete the prerequisite steps in the [getting started tutorial](getting-started.html#prerequisites) to create a {{site.data.keyword.conversationshort}} service instance and launch the tool.

You use the {{site.data.keyword.conversationshort}} tool to create skills. Follow these steps to create a search skill:

1.  Click the **Skills** tab.

1.  Click **Create new**.

1.  Click **Add** to create a *search skill*.

1.  Specify the details for the new skill:
    - **Name**: A name no more than 100 characters in length. A name is required.
    - **Description**: An optional description no more than 200 characters in length.

1.  Click **Create**.

The remaining steps differ depending on whether you have access to an existing {{site.data.keyword.discoveryshort}} service instance with collections created or not. Follow the appropriate procedure for your situation:

- [Connect to an existing instance](#connect)
- [Create an instance](#create)

## Connect to an existing service instance
{: #connect}

1.  Choose the {{site.data.keyword.discoveryshort}} service instance that you want to extract information from.

    Any {{site.data.keyword.discoveryshort}} service instances that you have access to are displayed in the list.

1.  Indicate the data collection to use, by doing one of the following things:

    - Choose an existing data collection. Go to [Configure the search](#configure).

      You can click the *Open in Discovery* link to review the configuration of a data collection before you decide which one to use.

    - If you do not want to use any of the data collections that are listed, click **create new collection** to add one, and then [create a data collection](#create-collection).

## Create an instance
{: #create}

1.  To create a {{site.data.keyword.discoveryshort}} service instance, click **Create new collection**.

    An instance of the {{site.data.keyword.discoveryshort}} service is created for you, and a configuration page opens to the new {{site.data.keyword.discoveryshort}} service instance.

    **Attention**: Currently, a Lite plan instance of {{site.data.keyword.discoveryshort}} cannot be provisioned automatically. Create a free Lite plan instance yourself from the [IBM Cloud Catalog ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://console.bluemix.net/catalog/services/discovery).

1.  Review the terms and conditions for using the instance, and then click **Accept** to continue.

1.  [Create a data collection](#create-collection).

## Create a data collection
{: #create-collection}

Do not try to add the pre-enriched data source named *Watson Discovery News* to your instance. It is not a data type that can be searched from {{site.data.keyword.conversationshort}}.

1.  To create a {{site.data.keyword.discoveryshort}} collection, do one of the following things:

      - To create a collection from data that is stored in a data source that {{site.data.keyword.discoveryshort}} provides built-in support for, click **Connect to data source**.

        1.  Pick a data source type.
        1.  Provide the required information for the data source you choose, and then click **Connect**.

            See [Connecting to data sources ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://console.bluemix.net/docs/services/discovery/connect.html#sources) for more details.
        1.  Indicate the frequency with which you want data from the data source to be synchronized with the collection you are creating in {{site.data.keyword.discoveryshort}}.
        1.  Specify the information that you want to extract from the data source and include in your {{site.data.keyword.discoveryshort}} collection.

            The options that are displayed differ depending on the data source type.

            - For a Salesforce data source, you select the object types that you want to extract from the source documents. You might select a [Case object type ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://developer.salesforce.com/docs/atlas.en-us.object_reference.meta/object_reference/sforce_api_objects_case.htm#!) that represents a *case*, which is a customer issue or problem, for example.
            - For a Sharepoint data source, you specify paths.
            - For file repositories, you specify directories or files.

        1.  Click **Save and sync data**.

            The data collection is created. After the process completes, a summary page is displayed in the {{site.data.keyword.discoveryshort}} tool in a separate web browser tab.
        1.  Click **Configure your skill in {{site.data.keyword.conversationshort}}** to return to the {{site.data.keyword.conversationshort}} tool.

      - To create a collection by uploading documents, click **Upload your own data**.

        1.  First you define the collection, and then you upload the documents. Provide the following information:

            - Collection name. The name must be unique for this service instance.
            - Configuration. You can choose to use a default configuration template or a saved configuration. See [Configuring your service ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://console.bluemix.net/docs/services/discovery/building.html) for more information about configurations.
            - Language. Select the language of the files that you will add to this collection. See [Language support ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://console.bluemix.net/docs/services/discovery/language-support.html) for information about the languages supported by {{site.data.keyword.discoveryshort}}.
        1.  Upload documents.

            **Note**: Supported file types include PDF, HTML, JSON, and DOC files. See [Adding content ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://console.bluemix.net/docs/services/discovery/adding-content.html) for more details.

            No ongoing synchronization of uploaded documents is available. If you want to pick up changes made to a document, upload a later version of the document.

## Configure the search
{: #configure}

1.  From the {{site.data.keyword.conversationshort}} search skill page, click **Configure**.

1.  Draft different messages to share with users depending on the successfulness of the search.

    <table>
    <caption>Search result messages</caption>
    <tr>
      <th>Field name</th>
      <th>Scenario</th>
      <th>Example message</th>
    </tr>
    <tr>
      <td>Message</td>
      <td>Search results are returned</td>
      <td>I found this information which might be helpful: </td>
    </tr>
    <tr>
      <td>No results found</td>
      <td>No search results are found</td>
      <td>I searched my knowledge base for information that might address your query, but did not find anything useful to share.</td>
    </tr>
    <tr>
      <td>Error message</td>
      <td>The service was unable to perform the search for some reason</td>
      <td>I might have information that could help address your query, but am unable to search my knowledge base at the moment.</td>
    </tr>
    </table>

1.  Choose the {{site.data.keyword.discoveryshort}} collection fields that you want to extract text from. The fields that are available differ based on the data you ingested and the configuration you used to ingest it.

    Each search result can consist of these pieces of information:

    - **Title**: Search result title. Use the title, name, or similar type of field from the collection as the search result title.

      Something other than `None` must be selected for the Facebook and Slack integrations to display the response at all.
    - **Body**: Search result description. Use an abstract, summary, or highlight field from the collection as the search result body.

      Something other than `None` must be selected for the Facebook and Slack integrations to display the response at all.
    - **Url**: A hypertext link to the original data object in its native data source. Most online data sources provide self-referencing public URLs for objects in the store to support direct access.

      The resulting URL must be valid and reachable for the Slack integration to include the URL in the response and for the Facebook integration to display the response at all. `None` is an acceptable selection for the Facebook and Slack integrations.

    See [Tips for collection field selection](#field-tips) for help.
  
    You must choose a value other than `None` for at least one of the options.

1.  In the preview pane, enter a test message to see the results that are returned when your configuration choices are applied to the search. Make adjustments as necessary.

1.  Click **Create**.

If you want to change the configuration later, open the search skill again, and make edits. You do not need to save changes as you make them; they are automatically applied. When you are happy with the search results, click **Save** to finish configuring the search skill.

## Next steps
{: #next-steps}

After you create the skill, it appears as a tile on the Skills page.

The search skill cannot interact with customers until it is added to an assistant and the assistant is deployed. See [Creating assistants](create-assistant.html).

### Tips for collection field selection
{: #field-tips}

The appropriate collection fields to extract data from vary depending on your collection's data source and the configuration you used to ingest the data source. The following table provides collection fields you can try as you get started. They suggestions assume that you used the default configuration template when creating the collection.

| Data source type   | Title | Body | Url |
|--------------------|-------|------|-----|
| Uploaded documents | enriched_text.concepts.text | text | None |
| Box                | name | description | listing_url |

See [Configuring your service > Adding enrichments ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://console.bluemix.net/docs/services/discovery/building.html#adding-enrichments) to understand where some of the collection fields, such as `enriched_text.concepts.text`, come from.

### Adding the skill to an assistant
{: #add-search-to-assistant}

You can add one skill to an assistant. You must open the assistant tile and add the skill to the assistant from the assistant configuration page; you cannot choose the assistant that will use the skill from within the skill configuration page. 

One search skill can be used by more than one assistant.

1.  From the Assistants tab, click to open the tile for the assistant to which you want to add the skill.

1.  Click **Add Search Skill**.

1.  Click **Add existing skill**.

    Click the skill that you want to add from the available skills that are displayed.

Configure at least one test integration channel. Test the skill by entering queries that trigger the search. Ensure that the search is being triggered properly, and is returning relevant results.
