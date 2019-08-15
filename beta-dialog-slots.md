---

copyright:
  years: 2015, 2019
lastupdated: "2019-08-13"

subcollection: assistant

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:deprecated: .deprecated}
{:important: .important}
{:note: .note}
{:tip: .tip}
{:preview: .preview}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:swift: .ph data-hd-programlang='swift'}
{:table: .aria-labeledby="caption"}

# Adding rich responses to slots
{: #dialog-slots-multimedia}

Add slots to a dialog node to gather multiple pieces of information from a user within that node. Slots collect information at the user's pace. Details that a user provides up front are saved, and your assistant asks only for the missing details it needs to fulfill the request. 

This feature is available for use by participants in the early access program only. To find out how to request access, see [Participate in the early access program](/docs/services/assistant?topic=assistant-feedback#feedback-beta).
{: preview}

IBM releases services, features, and language support for your evaluation that are classified as beta. These features might be unstable, might change frequently, and might be discontinued with short notice. Beta features also might not provide the same level of performance or compatibility that generally available features provide and are not intended for use in a production environment.

In addition to adding rich responses to the typical dialog node response, you can add rich responses to a node with slots in the following places:

- Slot prompt
- Slot Found condition response
- Slot Not found condition response
- Slot node response

To add slots with rich responses, complete the following steps:

1.  Click the drop-down menu in the response field to choose a response type, and then provide any required information:

    - **Connect to human agent**. ![Plus or Premium plan only](images/plus.png) You can optionally add a message to share with the human agent to whom the conversation is transferred.

        This response type is supported with Intercom and custom application integrations only. For custom applications, you must program the client application to recognize when this response type is triggered.
        {: note}

    - **Image**. Add the full URL to the hosted image file into the **Image source** field. The image must be in .jpg, .gif, or .png format. The image file must be stored in a location that is publicly addressable by URL.

        For example: `https://www.example.com/assets/common/logo.png`.

        If you want to display an image title and description above the embedded image in the response, then add them in the fields provided.

        To access an image that is stored in {{site.data.keyword.cloud}} {{site.data.keyword.cos_short}}, enable public access to the individual image storage object, and then reference it by specifying the image source with syntax like this: `https://s3.eu.cloud-object-storage.appdomain.cloud/your-bucket-name/image-name.png`.

        Slack integrations require a title. Other integration channels ignore titles or descriptions.
        {: note}

    - **Option**. Complete the following steps:

      1.  Click **Add option**.
      1.  In the **List label** field, enter the option to display in the list. The label must be less than 64 characters in length.
      1.  In the corresponding **Value** field, enter the user input to pass to your assistant when this option is selected. The value must be less than 2,048 characters in length.

          Specify a value that you know will trigger the correct intent when it is submitted. For example, it might be a user example from the training data for the intent.
      1.  Repeat the previous steps to add more options to the list.

          You can add up to 20 options.
      1.  Add a list introduction in the **Title** field. The title can ask the user to pick from the list of options.

          Some integration channels do not display the title.
          {: note}

      1.  Optionally, add additional information in the **Description** field. If specified, the description is displayed after the title and before the option list.

      Some integration channels do not display the description.
      {: note}

      For example, you can construct a response like this:

        <table>
        <caption>Response options</caption>
        <tr>
          <th>List title</th>
          <th>List description</th>
          <th>Option label</th>
          <th>User input submitted when clicked</th>
        </tr>
        <tr>
          <td>Insurance types</td>
          <td>Which of these items do you want to insure?</td>
          <td></td>
          <td></td>
        </tr>
        <tr>
          <td></td>
          <td></td>
          <td>Boat</td>
          <td>I want to buy boat insurance</td>
        </tr>
        <tr>
          <td></td>
          <td></td>
          <td>Car</td>
          <td>I want to buy car insurance</td>
        </tr>
         <tr>
          <td></td>
          <td></td>
          <td>Home</td>
          <td>I want to buy home insurance</td>
        </tr>
        </table>

    - **Pause**. Add the length of time for the pause to last as a number of milliseconds (ms) to the **Duration** field.

        The value cannot exceed 10,000 ms. Users are typically willing to wait about 8 seconds (8,000 ms) for someone to enter a response. To prevent a typing indicator from being displayed during the pause, choose **Off**.

        Add another response type, such as a text response type, after the pause to clearly denote that the pause is over.
        {: tip}

    - **Text**. Add the text to return to the user in the text field. Optionally, choose a variation setting for the text response. See [Simple text response](#dialog-overview-simple-text) for more details.

    - **Search skill**. ![Plus or Premium plan only](images/plus.png) Indicates that you want to search an external data source for a relevant response.

      This response type is only visible to Plus or Premium plan users.
      {: note}

      To edit the search query to pass to the {{site.data.keyword.discoveryshort}} service, click **Customize**, and then fill in the following fields:

        - **Query**: Optional. You can specify a specific query in natural language to pass to {{site.data.keyword.discoveryshort}}. If you do not add a query, then the customer's exact input text is passed as the query.

          For example, you can specify `What cities do you fly to?`. This query value is passed to {{site.data.keyword.discoveryshort}} as a search query. {{site.data.keyword.discoveryshort}} uses natural language understanding to understand the query and to find an answer or relevant information about the subject in the data collection that is configured for the search skill.

          You can include specific information provided by the user by referencing entities that were detected in the user's input as part of the query. For example, `Tell me about @product`. Or you can reference a context variable, such as `Do you have flights to $destination?`. Just be sure to design your dialog such that the search is not triggered unless any entities or context variables that you reference in the query have been set to valid values.

          This field is equivalent to the {{site.data.keyword.discoveryshort}} `natural_language_query` parameter. For more information, see [Query parameters ![External link icon](../../icons/launch-glyph.svg "External link icon")](/docs/services/discovery?topic=discovery-query-parameters#nlq){: new_window}.

        - **Filter**: Optional. Specify a text string that defines information that must be present in any of the search results that are returned.

          - To indicate that you want to return only documents with positive sentiment detected, for example, specify `enriched_text.sentiment.document.label:positive`.

          - To filter results to includes only documents that the ingestion process identified as containing the entity `Boston, MA`, specify `enriched_text.entities.text:"Boston, MA"`.

          - To filter results to includes only documents that the ingestion process identified as containing a product name provided by the customer, you can specify `enriched_text.entities.text:@product`.

          - To filter results to includes only documents that the ingestion process identified as containing a city name that you saved in a context variable named `$destination`, you can specify `enriched_text.entities.text:$destination`.

        If you add both a query and a filter value, then the filter parameter is applied first to filter the data collection documents and cache the results. The query parameter then ranks the cached results. 

        This field is equivalent to the {{site.data.keyword.discoveryshort}} `filter` parameter. For more information, see [Query parameters ![External link icon](../../icons/launch-glyph.svg "External link icon")](/docs/services/discovery?topic=discovery-query-parameters#filter){: new_window}.

      This response type only returns a valid response if the assistant to which you added this dialog skill also has a search skill associated with it. Test this response type from the preview link or another assistant-level integration. You cannot test it from the dialog skill's "Try it out" pane.

1.  Click **Add response type** to add another response type to the current response.

    You might want to add multiple response types to a single response to provide a richer answer to a user query. For example, if a user asks for store locations, you could show a map and display a button for each store location that the user can click to get address details. To build that type of response, you can use a combination of image, options, and text response types. Another example is using a text response type before a pause response type so you can warn users before pausing the dialog.

    You cannot add more than 5 response types to a single response. Meaning, if you define three conditional responses for a dialog node, each conditional response can have no more than 5 response types added to it.
    {: note}

    A single response cannot have more than one **Connect to human agent** or more than one **Search skill** response type.
    {: note}

1.  If you added more than one response type, you can click the **Move** up or down arrows to arrange the response types in the order you want your assistant to process them.
