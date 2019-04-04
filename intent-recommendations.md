---

copyright:
  years: 2015, 2019
lastupdated: "2019-04-04"

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

# Get help building intents ![Plus or Premium plan only](images/premium.png)
{: #intent-recommendations}

If you have existing enterprise customer support chat transcript data, let Watson analyze that data to understand the customer needs that your support team spends most of its time addressing. Watson can then recommend intents and user examples you can use to train your assistant so it can recognize the same and similar requests in future.
{: shortdesc}

This feature is available to Plus or Premium plan users.
{: note}

Customer needs are represented in {{site.data.keyword.conversationshort}} as *intents*. If you have not defined intents yet, you can get started faster by asking Watson for help. Upload files with customer utterances from call center transcripts for the {{site.data.keyword.conversationshort}} service to analyze. Based on the insights it uncovers, the service recommends a base set of intents you should build to cover the most commonly occurring needs of your customers.

As the subjects that your customers want to discuss change, you can use the intent user example recommendations feature to help keep your intents up-to-date and relevant over time.

The following video provides a 3 1/2-minute overview of intent and intent user example recommendations.

<iframe class="embed-responsive-item" id="youtubeplayer" title="Intent recommendations overview" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/64h59KqDY98?rel=0" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

To learn more about how intent recommendations can help you build a useful bot faster, [read this blog post ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://medium.com/ibm-watson/let-watson-train-your-virtual-assistant-57bd53b12bc3).

Mine your existing data to do one of the following things:

- [Get intent recommendations](#intent-recommendations-get-intent-recommendations)
- [Get intent user example recommendations](#intent-recommendations-get-example-recommendations)

See [Supported languages](/docs/services/assistant?topic=assistant-language-support) for information about the language support for this feature.

## Creating a user example source file
{: #intent-recommendations-log-files-add}

Before the service can analyze your data, you must provide the data in the correct format. Create a comma-separated value (CSV) file with one customer utterance per line. Ideally, the utterances are short phrases which are extracted from your call center transcripts that contain real-world customer questions and requests. Each user example source file can be a maximum size of 20 MB.

Follow these additional guidelines:

  - Remove any sensitive data from the utterances that you include in the file.

    Sensitive data includes any information relating to an identifiable natural person such as names, email addresses, and customer IDs, and regulated data such as protected health information.
  - Do not include utterances that exceed 1,024 characters in length. Longer utterances are truncated.
  - The file must contain at least 100 utterances; a file with 500 or more utterances will give you better results.
  - If an utterance contains a comma, surround the utterance in quotation marks.
  - The CSV must include only one column.
  - Remove everything that is not a customer-generated utterance, including any human agent responses or notes.

  For example:

  ```
  What happens to my coverage if I trade in my car?
  i'd like to buy a house.
  How do I add a dependent to my plan?
  "first, i want to know if i am already registered."
  ```

Any files you upload are shared across all of the skills in the current service instance. The utterances from all of the available files are mined when you ask for both intent recommendations and intent user example recommendations.

## Get intent recommendations from Watson
{: #intent-recommendations-get-intent-recommendations}

Let the service analyze your call center chat transcripts and recommend some initial intents for you to start with. If you already created some intents, let the service analyze your logs and compare its findings with your existing intents to identify gaps in your training data and suggest new intents that can fill them in.

To use this feature, upload a file with customer utterances. The service evaluates these utterances and identifies common problem areas that customers mention frequently. The {{site.data.keyword.conversationshort}} tool then displays a set of discrete intents that capture the trending customer needs. You can review each recommended intent and its corresponding user examples to choose the ones you want to add to your training data.

## Getting intent recommendations
{: #intent-recommendations-get-intent-recommendations-task}

Before you begin, create a CSV file with your data. See [Creating a user example source file](#intent-recommendations-log-files-add).

To get intent recommendations, complete the following steps:

1.  In the {{site.data.keyword.conversationshort}} tool, open your dialog skill. The skill opens to the **Intents** page.

1.  Click **Get recommendations**.

1.  **First time only**: Click **Add file**, and then click **Choose a file** to browse for the CSV file you created earlier and select it.

    Give the service time to analyze your data and group the utterances.

1.  Review the intents that are recommended by the service.

    The intent recommendations are ordered by:

    1.  Intents with the most frequently occuring utterances. The frequency of the utterances associated with these intents suggests they identify the most common customer pain points.
    1.  How different the intents are from intents you have already added to your skill. Their uniqueness suggests that they can address customer needs that are not being met currently.
    1.  How similar the user examples in the intent are to one another, which indicates the strength of the intent.

    You can hover over an intent to see a few examples of its associated utterances. If you want to see a list of all of the intent groupings, click **Show all recommendations**.

1.  Click an intent to see the full set of user examples that are associated with it, and to take one of the following actions:

    Deselect any utterances that you do not want to add as user examples.

    - To add the recommended intent with the selected utterances as user examples, click **Create intent**.
    - To add the selected utterances from the recommended intent to one of your existing intents as user examples instead, click **Add to existing intent**, choose the intent, and then click **Add**.

The intents and intent user examples that you add in this way do count toward your intent and intent user example totals for which there are limits per plan. See [Intent limits](/docs/services/assistant?topic=assistant-intents#intents-limits) for more details.

As the subjects that your customers want to discuss change, you can use the intent user example recommendations feature to help keep your intents up-to-date and revelant over time.

## Get intent user example recommendations
{: #intent-recommendations-get-example-recommendations}

The data you provide to the service for it to find intent user examples does not need to associate the examples with intents. You simply provide the raw customer utterances and let the service do the work of choosing the ones that are appropriate for the current intent. For every intent, the service analyzes the same data to find user examples that are appropriate for that intent.

Before you begin, create a CSV file with your data. See [Creating a user example source file](#intent-recommendations-log-files-add).

1.  Follow the steps in [Creating intents](/docs/services/assistant?topic=assistant-intents#intents-creating-intents-task) to create an intent.

1.  Add at least 5 user examples that illustrate the full range of typical utterances that you anticipate customers might say to trigger this intent.

    These seed user examples teach the service about the kinds of utterances to look for in the files you upload.

1.  Click **Show recommendations**.

    User example source files are shared between the skills in a service instance. If your coworkers own skills in the same instance and upload files, then their files are added to your shared *User examples source files* collection.

1.  **First time only**: Click **Upload files**, and then click **Choose a file** to browse for the CSV file you created earlier and select it.

    It's fine if the file you uploaded contains utterances for all types of intents. The service knows which intent you are working on and finds suitable examples to recommend for this specific intent.

    After the file is uploaded and processed by the service, recommended utterances are displayed. If no recommendations are made, then the file does not contain examples that are suitable for this intent.

1.  If the file cannot provide useful recommendations for any of your intents, you can try a different set of utterances by uploading another file. See [Managing uploaded files](#intent-recommendations-manage-files) for more details.

1.  After the service shows you recommendations, select the utterances that you want to add as user examples for this intent, and then click **Add**. Or click **Next set** to review more utterances.
1.  If you want to search the content of the CSV file for user examples yourself, click the **Search Logs** tab, enter a keyword on which to base the search, and then press **Enter**.

    Follow these search query syntax guidelines:

    - Boolean operators (such as `AND` and `OR`) are supported.
    - Add quoted text to search for an exact text match ("thisstringmustbepresent").
    - You can use regular expressions, such as `*ly` to find all terms that end with `ly`.
    - The following characters are used as regular expression operators:

      `+ - = && || > < ! ( ) { } [ ] ^ " ~ * ? : \ /`

      If you want to include one in a search term without it being processed as an operator, you must prefix it with a backslash (`\`).

The user examples that you add in this way do count toward your intent user example totals for which there are limits per plan. See [Intent limits](/docs/services/assistant?topic=assistant-intents#intents-limits) for more details.

## Managing uploaded files
{: #intent-recommendations-manage-files}

After you upload at least one file, you can open the *User example source files* collection to manage your files. Uploaded files are added to a file collection that is shared across the current {{site.data.keyword.conversationshort}} service instance.

1.  To access the collection, click to open an intent, click **Show recommendations**, and then click **View Files** from the sidebar.

    If you don't have any intents added yet, from the main Intents page, expand the *Intent recommendations* section, click **Get recommendations**, click **Show all recommendations**, and then click **View Files**.

1.  You can take the following actions:

    - To add a file, click **Add Files**, and then browse for a file and select it.
    - To delete a file, you must remove all of the files that have been uploaded from any skill that is associated with this service instance; you cannot delete only one file. First, make sure nobody else is using the files, then click **Delete all** to delete all of the uploaded files.

1.  Close the *User example source files* page.
