---

copyright:
  years: 2015, 2018
lastupdated: "2018-06-21"

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

# Defining intents
{: #intents}

***Intents*** are purposes or goals expressed in a customer's input, such as answering a question or processing a bill payment. By recognizing the intent expressed in a customer's input, the {{site.data.keyword.conversationshort}} service can choose the correct dialog flow for responding to it.
{: shortdesc}

<iframe class="embed-responsive-item" id="youtubeplayer" title="Working with intents" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/WbXTZBq-Xlk" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

## Intent limits
{: #intent-limits}

The number of intents and examples you can create depends on your {{site.data.keyword.conversationshort}} service plan:

| Service plan     | Intents per skill | Examples per skill |
|------------------|------------------:|-------------------:|
| Premium          |             2,000 |             25,000 |
| Standard         |             2,000 |             25,000 |
| Lite             |               100 |             25,000 |
{: caption="Service plan details" caption-side="top"}

## Creating intents
{: #creating-intents}

Use the {{site.data.keyword.conversationshort}} tool to create intents.

1.  In the {{site.data.keyword.conversationshort}} tool, open your conversational skill and then select the **Intents** tab in the navigation bar. If **Intents** is not visible, use the ![Menu](images/Menu_16.png) menu to open the page.

1.  Select **Create new**.

1.  In the **Intent name** field, type a name for the intent.
    - The intent name can contain letters (in Unicode), numbers, underscores, hyphens, and periods.
    - The name cannot consist of `..` or any other string of only periods.
    - Intent names cannot contain spaces and must not exceed 128 characters. The following are examples of intent names:
        - `#weather_conditions`
        - `#pay_bill`
        - `#escalate_to_agent`

    The tool automatically includes the `#` character in the intent names, so you do not have to add one.
    {: tip}

    Add a description of the intent in the **Description** field.

1.  Select **Create intent** to save your intent name.

    ![Screen capture showing new intent definition](images/create_intent.png)

1.  Next, in the **Add user examples** field, type the text of a user example for the intent. An example can be any string up to 1024 characters in length. The following might be examples for the `#pay_bill` intent:
    - `I need to pay my bill.`
    - `Pay my account balance`
    - `make a payment`

    **Referencing entities and synonyms as intent examples**

    If you have defined, or plan to define, entities that correspond to this intent, refer to the entities, or their associated synonyms, in some of the examples. Doing so helps to establish a relationship between the intent and entities.

    ![Screen capture showing intent definition](images/define_intent.png)
    {: #entity-as-example}

    You can also add entity annotations directly from user examples. See [Creating entity annotations from the **Intents** tab](entities.html#create-open-entities).

    *Important*:

      - Intent example data should be representative and typical of data that end users will provide. Examples can be collected from actual user data, or from people who are experts in your specific field. The representative and accurate nature of the data is important.
      - Both training and test data (for evaluation purposes) should reflect the distribution of intents in real usage. Generally, more frequent intents have relatively more examples, and better response coverage.
      - You can include punctuation in the example text, as long as it appears naturally. If you believe that some users will express their intents with examples that include punctuation, and some users will not, include both versions. Generally, the more coverage for various patterns, the better the response.

    **Directly referencing an @Entity as an intent example**

    You may also choose to directly reference entities in your intent examples. For instance, say you have an entity called `@PhoneModelName`, which contains values *Galaxy S8*, *Moto Z2*, *LG G6*, and *Google Pixel 2*. When you create an intent, for example `#order_phone`, you could then provide training data as follows:
    - Can I get a `@PhoneModelName`?
    - Help me order a `@PhoneModelName`.
    - Is the `@PhoneModelName` in stock?
    - Add a `@PhoneModelName` to my order.

    ![Screen capture showing intent definition](images/define_intent_entity.png)

    **Note**: Currently, you can only directly reference closed entities that you define (pattern values will be ignored). You cannot use [system entities](system-entities.html).

    If you choose to reference an entity as an intent example (for example, `@PhoneModelName`) *anywhere* in your training data it cancels out the value of using a direct reference (for example, *Galaxy S8*) in an intent example anywhere else. All intents will then use the entity-as-an-intent-example approach; you cannot select this approach for a specific intent only.

    In practice, this means that if you have previously trained most of your intents based on direct references (*Galaxy S8*), and you now use entity references (`@PhoneModelName`) for just one intent, that would impact all your previous training. If you do choose to use `@Entity` references, you need to be careful to replace all previous direct references with `@Entity` references.

    **Note**: Defining one example intent with an `@Entity` that has 10 values defined for it **does not** equate to specifying that example intent 10 times. The {{site.data.keyword.conversationshort}} service does not give that much weight to that one example intent syntax.

    **Important**: Intent names and example text can be exposed in URLs when an application interacts with the service. Do not include sensitive or personal information in these artifacts.

    Press **Add example** to save the example.

1.  Repeat the same process to add more examples. You can tab between each example. Provide at least 5 examples for each intent. The more examples you provide, the more accurate your application can be.

1.  When you have finished adding examples, select ![Close arrow](images/close_arrow.png) to finish creating the intent.

### Results
{: #creating-intents-results}

The intent you created is added to the Intents tab, and the system begins to train itself on the new data.

## Editing intents

You can select any intent in the list to open it for editing. You can make the following changes:

- Rename the intent.
- Delete the intent.
- Add, edit, or delete examples.
- Move an example to a different intent.

You can tab from the intent name to each example, editing the examples if you choose.

To move or delete an example, select the example by selecting the check box and then select **Move** or **Delete**.

  ![Screen capture showing how to move or delete an example](images/move_example.png)

## Searching intents

**BETA** The Searching intents feature described in this documentation is a beta feature that has been made available for your evaluation. Beta features might be unstable, might change frequently, and might be discontinued with short notice. Beta features also might not provide the same level of performance or compatibility that generally available features provide, and are not intended for use in a production environment.

Use the Search feature to find user examples, intent names, and descriptions.

1.  Select the **Intents** tab in the navigation bar.

    ![Intent tab overview](images/intent_oview.png)

1.  Select the Search icon: ![Search icon](images/search_icon.png)

1.  Enter a search term or phrase.

    ![Intent search term](images/searchint_1.png)

### Results
{: #searching-intents-results}

Intents containing your search term, with corresponding examples, are shown.

  ![Intent search return](images/searchint_2.png)

## Importing intents and examples

If you have a large number of intents and examples, you might find it easier to import them from a comma-separated value (CSV) file than to define them one by one in the {{site.data.keyword.conversationshort}} tool.

1.  Collect the intents and examples into a CSV file, or export them from a spreadsheet to a CSV file. The required format for each line in the file is as follows:

    ```
    <example>,<intent>
    ```
    {: screen}

    where `<example>` is the text of a user example, and `<intent>` is the name of the intent you want the example to match. For example:

    ```
    Tell me the current weather conditions.,weather_conditions
    Is it raining?,weather_conditions
    What's the temperature?,weather_conditions
    Where is your nearest location?,find_location
    Do you have a store in Raleigh?,find_location
    ```
    {: screen}

    > **Important:** Save the CSV file with UTF-8 encoding and no byte order mark (BOM).

1.  In the {{site.data.keyword.conversationshort}} tool, open your conversational skill and then select the **Intents** tab in the navigation bar. If **Intents** is not visible, use the ![Menu](images/Menu_16.png) menu to open the page.

1.  Select the *Import* icon ![Import icon](images/importGA.png). Then, drag a file or browse to select a file from your computer. The file is validated and imported, and the system begins to train itself on the new data.

    ![Import option](images/ImportIntent.png)

    > **Important:** The maximum CSV file size is 10MB. If your CSV file is larger, consider splitting it into multiple files and importing them separately.

### Results
{: #importing-intents-results}

You can view the imported intents and the corresponding examples on the **Intents** tab. You might need to refresh the page in order to see the new intents and examples.

## Exporting intents
{: #export_intents}

You can export a number of intents to a CSV file, so you can then import and reuse them for another {{site.data.keyword.conversationshort}} application.

1.  On the Intents tab, select the intents you want from the list and choose *Export*.

    ![Export option](images/ExportIntent.png)

## Resolving intent conflicts
{: #conflict-intents}

The {{site.data.keyword.conversationshort}} application detects a conflict when two or more intent examples in *separate* intents are so similar that {{site.data.keyword.conversationshort}} is confused as to which intent to use.

To resolve conflicts:

1.  On the **Intents** tab, review any intents with conflicts.

    ![Conflicts in intent list](images/ConflictIntent1.png)

    Toggle the *Show only conflicts* button to see a list of just your intents with conflicts.
    {: tip}

    ![Conflicts only view](images/ConflictIntent2.png)

1.  Open an intent conflict. For the intent example that is causing the conflict, select the *Resolve conflict...* button.

    ![Conflicting intent example](images/ConflictIntent3.png)

1.  Now, you have the option to either move a conflicting example to another intent, or delete a conflicting example entirely.

    In this case, the examples `Cancel my order` and `I want to cancel my order` appear in both the `#cancel` intent and in the `#eCommerce_Cancel_Product_Order` intent.

    ![Conflicting intent example](images/ConflictIntent4.png)

    **NOTE**: Similar examples are training examples that are not necessarily in conflict, but are similar to the examples in conflict. They are shown to provide context to help resolve the conflict.

1.  Select the examples `Cancel  my order` and `I want to cancel my order`, and move them from the `#cancel` intent to the `#eCommerce_Cancel_Product_Order` intent:

    ![Conflicting intent example](images/ConflictIntent5.png)

1.  When deciding where to place an example, look for the intent that has synonymous, or nearly synonymous examples.

    **NOTE**: Keep each intent as distinct and focused on one goal as possible. If you have two intents with multiple user examples that overlap, maybe you don't need two separate intents. You can move or delete user examples that don't directly overlap into one intent, and then delete the other.

    Select the other examples in the `#cancel` intent, and delete them:

    ![Conflicting intent example](images/ConflictIntent6.png)

1.  Click the *Submit* button to resolve the conflicts:

    ![Conflicting intent example](images/ConflictIntent7.png)

    **NOTE**: The *Reset* option allows you to start over with moving the conflict example among intents. *Cancel* returns you to the intent page.

### Results
{: #resolving-intent-conflict-results}

You have resolved a conflict, and can continue your review of other intents with conflicts.

## Deleting intents
{: #delete_intents}

You can select a number of intents for deletion.

**IMPORTANT**: By deleting intents you are also deleting all associated examples, and these items cannot be retrieved later. All dialog nodes that reference these intents must be updated manually to no longer reference the deleted content.

1.  On the Intents tab, select the intents you want from the list and choose *Delete*.

    ![Delete option](images/DeleteIntent.png)

## Testing your intents
{: #testing-your-intents}

After you have finished creating new intents, you can test the system to see if it recognizes your intents as you expect.

1.  In the {{site.data.keyword.conversationshort}} tool, select the ![Ask Watson](images/ask_watson.png) icon.

1.  In the *Try it out* pane, enter a question or other text string and press Enter to see which intent is recognized. If the wrong intent is recognized, you can improve your model by adding this text as an example to the correct intent.

    If you have recently made changes in your skill, you might see a message indicating that the system is still retraining. If you see this message, wait until training completes before testing:
    {: tip}

    ![Screen capture showing retraining message](images/training.png)

    The response indicates which intent was recognized from your input.

    ![Screen capture of testing intents](images/test_intents.png)

1.  If the system did not recognize the correct intent, you can correct it. To correct the recognized intent, select the displayed intent and then select the correct intent from the list. After your correction is submitted, the system automatically retrains itself to incorporate the new data.

    ![Screen capture of correcting a recognized intent](images/correct_intent.png)

1.  If the input is unrelated to your application, you can indicate that. Select the displayed intent and choose **Mark as irrelevant**.

    ![Mark as irrelevant screen capture](images/irrelevant.png)

If your intents are not being correctly recognized, consider making the following kinds of changes:

- Add the unrecognized text as an example to the correct intent.
- Move existing examples from one intent to another.
- Consider whether your intents are too similar, and redefine them as appropriate.

## Absolute scoring

The {{site.data.keyword.conversationshort}} service now scores each intent’s confidence on its own, not in relation to other intents. This allows the flexibility to have multiple intents returned. It also means the system may not return an intent at all. If the top intent has low confidence that any intents relate to the user’s input (less than 0.2), it will still get included in the intents array output by the API, but conditioning on that #intent will return false. If you want to detect the case when no intents were detected with good confidence, you can condition on `irrelevant`.

As intent confidence scores change, your dialogs may need restructuring. For example, if you conditioned your dialog with an intent that now has low confidence, the system’s response will no longer be correct.

## Mark as irrelevant
{: #mark-irrelevant}

Refer to [supported languages](lang-support.html) for the availability of this feature.

After you upgrade your conversational skill, you can [test input](#testing-your-intents) in the *Try it out* pane to see the changes. You can use **Mark as irrelevant** to indicate that the input is not related to your application.

If you have an intent, such as #off_topic, for those inputs that are out of scope or off topic, delete the intent and test your skill by marking the inputs as irrelevant.

**Important**: Intents that are marked as irrelevant are saved as counterexamples in the JSON workspace, and are included as part of the training data. Be sure that you want to make any changes.

- The inputs cannot be accessed or changed later in the tool.
- The only way to remove the **Irrelevant** tag is to use the same input in the *Try it out* pane, and then change the intent.
