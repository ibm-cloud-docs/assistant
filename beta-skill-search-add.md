---

copyright:
  years: 2015, 2019
lastupdated: "2019-01-30"

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

# Building a search skill
{: #beta-skill-search-add}

An assistant uses a *search skill* to route complex customer inquiries to the {{site.data.keyword.discoveryfull}} service. {{site.data.keyword.discoveryshort}} treats the user input as a search query. It finds information that is relevant to the query from an external data source and returns it to the assistant.
{: shortdesc}

This feature is available for use by participants in the beta program only. To find out how to request access, see [Particpate in the beta program](feedback.html#feedback-beta).

![Beta](images/beta.png) IBM releases services, features, and language support for your evaluation that are classified as beta. These features might be unstable, might change frequently, and might be discontinued with short notice. Beta features also might not provide the same level of performance or compatibility that generally available features provide and are not intended for use in a production environment. 

You can add one search skill to an assistant. See [Skill limits](/docs/services/assistant/skill-add.html#skill-add-limits) for information about limits per plan.

The search skill searches for information from a data collection that you create by using the {{site.data.keyword.discoveryshort}} service. {{site.data.keyword.discoveryshort}} is a service that crawls, converts, and normalizes your unstructured data. The service applies data analysis and cognitive intuition to enrich your data such that you can more easily find and retrieve meaningful information from it later. To read more about {{site.data.keyword.discoveryshort}}, see the [product documentation ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://cloud.ibm.com/docs/services/discovery/aboutlhtml).

The {{site.data.keyword.discoveryfull}} service is triggered in the following ways:

- **Anything else node**: Searches an external data source for a relevant answer when none of the dialog nodes can address the user's query. Instead of showing a standard message, such as `I don't know how to help you with that.` the assistant can say, `Maybe this information can help:` followed by the passage returned by the search. If a search skill is linked to your assistant, then whenever the `anything_else` node is triggered, rather than displaying the node response, a search is performed instead. The assistant passes the user input as the query to your search skill, and returns the search results as the response.
- **Search response type**: If you add a search response type to a dialog node, then the service retrieves a passage from an external data source and returns it as the response to a particular question. This type of search occurs only when the individual dialog node is processed. This approach is useful if you have want to narrow down a user query before you perform a search. For example, the dialog branch might collect information about the type of device the customer wants to buy. When you know the make and model, you can then send a model keyword in the query that is submitted to the search skill, and get better results.
- **Search skill only**: If only a search skill is linked to an assistant, and no dialog skill is linked to the assistant, then a search query is submitted to the {{site.data.keyword.discoveryshort}} service when any user input is received from one of the assistant's integration channels.

## Creating a search skill
{: #beta-skill-search-add-task}

If you have not done so, complete the prerequisite steps in the [getting started tutorial](/docs/services/assistant/getting-started.html#getting-started-prerequisites) to create a {{site.data.keyword.conversationshort}} service instance and launch the {{site.data.keyword.conversationshort}} tool.

You use the {{site.data.keyword.conversationshort}} tool to create skills. Follow these steps to create a search skill:

1.  Click the **Skills** tab.

1.  Click **Create new**.

1.  Click **Add** to create a *search skill*.

1.  Specify the details for the new skill:
    - **Name**: A name no more than 100 characters in length. A name is required.
    - **Description**: An optional description no more than 200 characters in length.

1.  Click **Create**.

The remaining steps differ depending on whether you have access to an existing {{site.data.keyword.discoveryshort}} service instance with collections created or not. Follow the appropriate procedure for your situation:

- [Connect to an existing Watson Discovery instance](#beta-skill-search-add-connect-discovery)
- [Create a Watson Discovery instance](#beta-skill-search-add-create-discovery)

## Connect to an existing Watson Discovery service instance
{: #beta-skill-search-add-connect-discovery}

1.  Choose the {{site.data.keyword.discoveryshort}} service instance that you want to extract information from.
{: #choose-d-instance}

    Any {{site.data.keyword.discoveryshort}} service instances that you have access to are displayed in the list.

    If you see a warning that some of your {{site.data.keyword.discoveryshort}} service instances do not have credentials set, it means that you have access to at least one instance that you have never opened from the {{site.data.keyword.cloud_notm}} dashboard directly yourself. You must access a service instance for credentials to be created for it. And credentials must exist before {{site.data.keyword.conversationshort}} can establish a connection to the {{site.data.keyword.discoveryshort}} service instance on your behalf. If you think a {{site.data.keyword.discoveryshort}} service instance should be listed that is not, open the instance from the {{site.data.keyword.cloud_notm}} dashboard directly to generate credentials for it.

1.  Indicate the data collection to use, by doing one of the following things:
{: #pick-data-collection}

    - Choose an existing data collection.

      You can click the *Open in Discovery* link to review the configuration of a data collection before you decide which one to use.

      Go to [Configure the search](#search-skill-configure).

    - If you do not have a collection or do not want to use any of the data collections that are listed, click **Create a new collection** to add one. Follow the procedure in [Create a data collection](#search-skill-create-discovery-collection).

      The **Create a new collection** button is not displayed if you have reached the limit to the number of collections you are allowed to create based on your {{site.data.keyword.discoveryshort}} service plan. See [{{site.data.keyword.discoveryshort}} pricing plans ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://cloud.ibm.com/docs/services/discovery/discovery-pricing-plans.html) for plan limit details.

## Create a Watson Discovery service instance
{: #beta-skill-search-add-create-discovery}

1.  To create a {{site.data.keyword.discoveryshort}} service instance, click **Create new collection**.

    An instance of the {{site.data.keyword.discoveryshort}} service is created for you, and a configuration page opens to the new {{site.data.keyword.discoveryshort}} service instance.

    A Lite plan instance of the service is provisioned in {{site.data.keyword.Bluemix_notm}}, no matter what {{site.data.keyword.conversationshort}} service plan you use. If you want to create the {{site.data.keyword.discoveryshort}} service instance as part of a different plan, stop here. Create the service instance directly from the [{{site.data.keyword.Bluemix_notm}} catalog ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://console.bluemix.net/catalog/services/discovery), and follow the [Connect to an existing {{site.data.keyword.discoveryshort}} service instance ](#beta-skill-search-add-connect-discovery) procedure instead.
    {: important}

1.  Review the terms and conditions for using the instance, and then click **Accept** to continue.

1.  [Create a data collection](#beta-skill-search-add-create-discovery-collection).

## Create a data collection
{: #beta-skill-search-add-create-discovery-collection}

Do not try to add the pre-enriched data source named *Watson Discovery News* to your instance. It is not a data type that can be searched from {{site.data.keyword.conversationshort}}.
{: important}

1.  To create a {{site.data.keyword.discoveryshort}} collection, do one of the following things:

      - To create a collection from data that is stored in a type of data source for which  {{site.data.keyword.discoveryshort}} provides built-in support, click **Connect to data source**.

        1.  Pick a data source type.
        1.  Provide the required information for the data source you choose, and then click **Connect**.

            See [Connecting to data sources ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://cloud.ibm.com/docs/services/discovery/sources.html) for more details.
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
            - Configuration. You can choose to use a default configuration template or a saved configuration. See [Configuring your service ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://cloud.ibm.com/docs/services/discovery/configuring-your-service.html) for more information about configurations.
            - Language. Select the language of the files that you will add to this collection. See [Language support ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://cloud.ibm.com/docs/services/discovery/language-support.html) for information about the languages supported by {{site.data.keyword.discoveryshort}}.
        1.  Upload documents.

            Supported file types include PDF, HTML, JSON, and DOC files. See [Adding content ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://cloud.ibm.com/docs/services/discovery/adding-content.html) for more details.
            {: note}

            No ongoing synchronization of uploaded documents is available. If you want to pick up changes made to a document, upload a later version of the document.

Wait for the collection to be fully ingested before you return to {{site.data.keyword.conversationshort}}.

## Configure the search
{: #beta-skill-search-add-configure}

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

1.  Choose the {{site.data.keyword.discoveryshort}} collection fields that you want to extract text from.

    The fields that are available differ based on the data you ingested and the configuration you used to ingest it.

    Each search result can consist of these pieces of information:

    - **Title**: Search result title. Use the title, name, or similar type of field from the collection as the search result title.

      Something other than `None` must be selected for the Facebook and Slack integrations to display the response at all.
    - **Body**: Search result description. Use an abstract, summary, or highlight field from the collection as the search result body.

      Something other than `None` must be selected for the Facebook and Slack integrations to display the response at all.
    - **Url**: A hypertext link to the original data object in its native data source. Most online data sources provide self-referencing public URLs for objects in the store to support direct access.

      The resulting URL must be valid and reachable for the Slack integration to include the URL in the response and for the Facebook integration to display the response at all. `None` is an acceptable selection for the Facebook and Slack integrations.

    See [Tips for collection field selection](#beta-skill-search-add-field-tips) for help.
  
    You must choose a value other than `None` for at least one of the options.

    If no options are available from the drop-down fields, you might need to give {{site.data.keyword.discoveryshort}} more time to finish creating the collection. Otherwise, your collection might not contain any documents or might have ingestion errors that need to be addressed first.

1.  In the preview pane, enter a test message to see the results that are returned when your configuration choices are applied to the search. Make adjustments as necessary.

1.  Click **Create**.

If you want to change the configuration later, open the search skill again, and make edits. You do not need to save changes as you make them; they are automatically applied. When you are happy with the search results, click **Save** to finish configuring the search skill.

## Next steps
{: #beta-skill-search-add-next-steps}

After you create the skill, it appears as a tile on the Skills page.

The search skill cannot interact with customers until it is added to an assistant and the assistant is deployed. See [Creating assistants](/docs/services/assistant/assistant-add.html).

When you link both a dialog skill and search skill to an assistant, the search skill is automatically triggered if user input is processed by the dialog skill and cannot be addressed by any of its dialog nodes. Rather than replying with a generic response from the `anything_else` node, a search that uses the user input as its query string is initiated.

If you want, you can define a specific search query to call in response to a particular node condition. To do so, add a search response type to the dialog node. See [Responses](/docs/services/assistant/dialog-overview.html#multimedia) for more details.

If you initiate any type of search from your dialog skill, test the dialog to ensure that the search is being triggered as expected. For example, if you are not using search response types, test that a search is triggered only when no existing dialog nodes can address the user input. And any time a search is triggered, ensure that it returns meaningful results.

### Tips for collection field selection
{: #beta-skill-search-add-field-tips}

The appropriate collection fields to extract data from vary depending on your collection's data source and the configuration you used to ingest the data source. To learn more about the structure of the documents in your collection, including the names of fields that contain information you might want to extract, open the collection in the {{site.data.keyword.discoveryshort}} tool, and then click **View data schema**.

The following table provides collection fields you can try as you get started. These suggestions assume that you used the default configuration template when creating the collection.

| Data source type   | Title | Body | Url |
|--------------------|-------|------|-----|
| Uploaded PDF documents | enriched_text.concepts.text | text | None |
| Box                | name | description | listing_url |

The collection fields are created when the collection is created. To learn more about fields that are generated when you use the default configuration template for ingestion, such as `enriched_text.concepts.text`, see [Configuring your service > Adding enrichments ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://cloud.ibm.com/docs/services/discovery/configuring-your-service.html#adding-enrichments).

### Adding the skill to an assistant
{: #beta-skill-search-add-to-assistant}

You can add one skill to an assistant. You must open the assistant tile and add the skill to the assistant from the assistant configuration page; you cannot choose the assistant that will use the skill from within the skill configuration page.

One search skill can be used by more than one assistant.

1.  From the Assistants tab, click to open the tile for the assistant to which you want to add the skill.

1.  Click **Add Search Skill**.

1.  Click **Add existing skill**.

    Click the skill that you want to add from the available skills that are displayed.

Configure at least one test integration channel. You cannot test the search skill from the Try it out pane. Test the skill from an integration channel by entering queries that trigger the search. Ensure that the search is being triggered properly, and is returning relevant results.

The shareable link integration does not work currently for assistants with a search skill.
{: important}
