---

copyright:
  years: 2015, 2018
lastupdated: "2018-06-01"

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

# Defining entities

***Entities*** represent a class of object or a data type that is relevant to a user's purpose. By recognizing the entities that are mentioned in the user's input, the {{site.data.keyword.conversationshort}} service can choose the specific actions to take to fulfill an intent.

<iframe class="embed-responsive-item" id="youtubeplayer" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/wyWgsF9eYc8" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

## Entity limits
{: #entity-limits}

The number of entities, entity values, and synonyms that you can create depends on your {{site.data.keyword.conversationshort}} service plan:

| Service plan      | Entities per skill | Entity values per skill | Entity synonyms per skill |
|-------------------|-------------------:|------------------------:|--------------------------:|
| Premium           |               1000 |                 100,000 |                   100,000 |
| Standard          |               1000 |                 100,000 |                   100,000 |
| Lite              |                 25 |                 100,000 |                   100,000 |
{: caption="Service plan details" caption-side="top"}

System entities that you enable for use count toward your plan usage totals.

## Creating entities
{: #creating-entities}

Use the {{site.data.keyword.conversationshort}} tool to create entities.

1.  In the {{site.data.keyword.conversationshort}} tool, open your conversational skill and then click the **Entities** tab. If **Entities** is not visible, use the ![Menu](images/Menu_16.png) menu to open the page.

1.  Click **Add entity**.

    You can also click **Use System Entities** to select from a list of common entities, provided by {{site.data.keyword.IBM_notm}}, that can be applied to any use case. See [Enabling system entities](#enable_system_entities) for more detail.

1.  In the **Entity name** field, type a descriptive name for the entity.

    The entity name can contain letters (in Unicode), numbers, underscores, and hyphens. For example:
    - `@location`
    - `@menu_item`
    - `@product`

    Don't include the `@` character in the entity names when you create them in the {{site.data.keyword.conversationshort}} tool. Entity names can't contain spaces or be longer than 64 characters. And entity names can't begin with the string `sys-`, which is reserved for system entities.
    {: tip}

1.  Select **Create entity**.

    ![Screen capture of creating an entity](images/create_entity.png)

1.  In the **Value name** field, type the text of a possible value for the entity and hit the `Enter` key. An entity value can be any string up to 64 characters in length.

    > **Important:** Don't include sensitive or personal information in entity names or values. The names and values can be exposed in URLs in an app.

1.  For **Fuzzy Matching**, click the button to select either on or off; fuzzy matching is off by default. This feature is available for languages noted in the [Supported languages](lang-support.html) topic.

    **Fuzzy matching**
    {: #fuzzy-matching}

    You can turn on fuzzy matching to improve the ability of the service to recognize user input terms with syntax that is similar to the entity, but without requiring an exact match. There are three components to fuzzy matching - stemming, misspelling, and partial matching:
    - *Stemming* - The feature recognizes the stem form of entity values that have several grammatical forms. For example, the stem of 'bananas' would be 'banana', while the stem of 'running' would be 'run'.
    - *Misspelling* - The feature is able to map user input to the appropriate corresponding entity despite the presence of misspellings or slight syntactical differences. For example, if you define *giraffe* as a synonym for an animal entity, and the user input contains the terms *giraffes* or *girafe*, the fuzzy match is able to map the term to the animal entity correctly.
    - *Partial match* - With partial matching, the feature automatically suggests substring-based synonyms present in the user-defined entities, and assigns a lower confidence score as compared to the exact entity match.

    **Note** - For English, fuzzy matching prevents the capturing of some common, valid English words as fuzzy matches for a given entity. This feature uses standard English dictionary words. You can also define an English entity value/synonym, and fuzzy matching will match only your defined entity value/synonym. For example, fuzzy matching may match the term `unsure` with `insurance`; but if you have `unsure` defined as a value/synonym for an entity like `@option`, then `unsure` will always be matched to `@option`, and not to `insurance`.

1.  Once you have entered a value name, you can then add any synonyms, or define specific patterns, for that entity value by selecting either `Synonyms` or `Patterns` from the *Type* drop-down menu.

    ![Type selector for Value](images/value_type.png)

    > **Note:** You can add *either* synonyms or patterns for a single entity value, you cannot add both.

    ***Synonyms***
    {: #synonyms}

    - In the **Synonyms** field, type any synonym for the entity value. A synonym can be any string up to 64 characters in length.

      ![Screen capture of defining an entity](images/define_entity.png)

      The {{site.data.keyword.conversationshort}} service can also recommend synonyms for your entity values. The recommender finds related synonyms based on contextual similarity extracted from a vast body of existing information, including large sources of written text, and uses natural language processing techniques to identify words similar to the existing synonyms in your entity value.

    - Select `Show recommendations`

      ![Synonym recommendation screen 1](images/synonym_1.png)

    - The {{site.data.keyword.conversationshort}} service will make several recommendations for synonyms

      **NOTE**: The more coherent your entity value synonyms are, the more relevant and better focused your recommendations will be. For example, if you have several words that are focused on a theme, you will get better suggestions than if you have one or two random words.

      ![Synonym recommendation screen 2](images/synonym_2.png)

    - Select any synonyms you want to include, and click `Add selected`

      ![Synonym recommendation screen 3](images/synonym_3.png)

    - The {{site.data.keyword.conversationshort}} service adds those synonyms to your entity, and suggests additional synonyms

      **NOTE**: If you receive no additional synonym recommendations, it could be because your entity is already well defined, or it contains content that the recommender is not currently able to expand upon.

      **NOTE**: If you choose not to select a recommended synonym, the system will treat that as a term you are not interested in, and will alter the next set of recommendations you see when you press `Add selected` or `Next set`.

      ![Synonym recommendation screen 4](images/synonym_4.png)

      Continue adding synonyms as desired. When you're finished accepting recommendations, click the `X` to close.

    ***Patterns***
    {: #patterns}

    - The **Patterns** field lets you define specific patterns for an entity value. A pattern **must** be entered as a regular expression in the field.

      - For each entity value, there can be a maximum of up to 5 patterns.
      - Each pattern (regular expression) is limited to 128 characters.

      ![Screen capture of defining a pattern entity](images/patternents1.png)
      {: #pattern-entities}

      As in this example, for entity *ContactInfo*, the patterns for phone, email, and website values can be defined as follows:
      - Phone
        - `localPhone`: `(\d{3})-(\d{4})`, e.g. 426-4968
        - `fullUSphone`: `(\d{3})-(\d{3})-(\d{4})`, e.g. 800-426-4968
        - `internationalPhone`: `^(\(?\+?[0-9]*\)?)?[0-9_\- \(\)]*$`, e.g., +44 1962 815000
      - `email`: `\b[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Za-z]{2,}\b`, e.g. name@ibm.com
      - `website`: `(https?:\/\/)?([\da-z\.-]+)\.([a-z\.]{2,6})([\/\w \.-]*)*\/?$`, e.g. https://www.ibm.com

      Often when using pattern entities, it will be necessary to store the text that matches the pattern in a context variable (or action variable), from within your dialog tree.

      Imagine a case where you are asking a user for their email address. The dialog node condition will contain a condition similar to `@contactInfo:email`. In order to assign the user-entered email as a context variable, the following syntax can be used to capture the pattern match within the dialog node's response section:

      ```json
      {
          "context" : {
              "email": "<? @contactInfo.literal ?>"
          }
      }
      ```
      {: screen}
      {: #capture-group}

      *Capture groups* - For regular expressions, any part of a pattern inside a pair of normal parentheses will be captured as a group. For example, the entity value `fullUSphone` contains three captured groups:

        - `(\d{3})` - US area code
        - `(\d{3})` - Prefix
        - `(\d{4})` - Line number

      Grouping can be helpful if, for example, you wanted the {{site.data.keyword.conversationshort}} service to ask users for their phone number, and then use only the area code of their provided number in a response.

      In order to assign the user-entered area code as a context variable, the following syntax can be used to capture the group match within the dialog node's response section:

        ```json
        {
            "context" : {
                "area_code": "<? @fullUSphone.groups[1] ?>"
            }
        }
        ```
       {: screen}

      For additional information about using capture groups in dialog runtime, see [Storing pattern entity values](dialog-runtime.html#context-pattern-entities).

      The pattern matching engine employed by the {{site.data.keyword.conversationshort}} service has some syntax limitations, which are necessary in order to avoid performance concerns which can occur when using other regular expression engines.
        - Entity patterns may not contain:
          - Positive repetitions (for example `x*+`)
          - Backreferences (for example `\g1`)
          - Conditional branches (for example `(?(cond)true)`)
        - When a pattern entity starts or ends with a Unicode character, and includes word boundaries, for example `\bš\b`, the pattern match does not match the word boundary correctly. In this example, for input `š zkouška`, the match returns `Group 0: 6-7 š` (`š zkou`_**`š`**_`ka`), instead of the correct `Group 0: 0-1 š` (_**`š`**_ `zkouška`).

      The regular expression engine is loosely based on the Java regular expression engine. The {{site.data.keyword.conversationshort}} service will produce an error if you try to upload an unsupported pattern, either via the API or from within the {{site.data.keyword.conversationshort}} service Tooling UI.

1.  Click **Add value** and repeat the process to add more entity values.

1.  When you are finished adding entity values, select ![Close arrow](images/close_arrow.png) to finish creating the entity.

### Results

The entity you created is added to the **Entities** tab, and the system begins to train itself on the new data.

## Defining annotations for entities (BETA)

When you define specific values for an entity, the service finds entity mentions only when a term in the user input exactly matches (or closely matches if fuzzy matching is enabled) a value or synonym defined. When you define annotations for an entity, a model is trained on both the entity *value* and the *context* in which the entity is used in sentences that you annotate. This new entity context model enables the service to calculate a confidence score that identifies how likely a word or phrase is to be an instance of an entity, based on how it is used in the user input.

**Note**: This feature is Beta only. This feature is currently available for English only.

### Creating entity annotations from the **Intents** tab
{: #create-open-entities}

In order to train an annotated entity model, you can take advantage of your intent examples, which provide readily-available sentences to annotate. Using intent user examples to define entity annotations does not affect the classification of an intent.

1.  In the {{site.data.keyword.conversationshort}} tool, open your skill and then click the **Intents** tab. If **Intents** is not visible, use the ![Menu](images/Menu_16.png) menu to open the page.

1.  Select an intent. For this example, the intent `#place_order` defines the order function for an online retailer.

    ![Select #place_order intent](images/oe-intent.png)

1.  Review the intent examples for potential entity values. Highlight a potential entity value from the intent examples, in this case `computer`.

    ![Review intent examples](images/oe-intent-review.png)

    **Note**: To directly edit an intent example, select the Edit icon ![Edit icon](images/oe-intent-edit.png) instead of highlighting a value for annotation.

1.  A Search box opens, allowing you to search for an appropriate entity for the highlighted entity value.

    ![Search box initial state](images/oe-intent-search1.png)

1.  In this example, searching `prod` brings up matches for both the `@product` entity, and for entity values `shirt` and `pens`. This is an important distinction - `@product` is an entity that can contain multiple entity values, in this case `@product:pencil`, `@product:shirt` and `@product:pens`

    ![Search box with search parameter prod](images/oe-intent-search2.png)

    You can also create a new entity by choosing `@(create new entity)`.

1.  Select `@product` to add `computer` as a value for that entity.
1.  Now, click the annotation you just created. A box will appear with the words `Go to: @product` at the bottom.  Clicking that link will take you directly to the entity.

    ![Verify value computer for product entity](images/oe-verify-value.png)

### Viewing annotations from the **Entities** tab
{: #annotate-open-entities}

To see the intent examples you have used in annotating your entities:

1.  From the **Entities** tab, open an entity, for example, `@cuisine`.

    ![Cuisine entity highlighted in list](images/oe-annotate1.png)

1.  Select the *Annotation* view.

    ![Annotation view selector highlighted](images/oe-annotate2.png)

    If you have already [created annotated entities from the Intents tab](entities.html#create-open-entities), you will see a list of user examples with their associated intents.

    If you want your entity model to understand additional values, values that you have not explicitly defined, turn on the `Value expansion` option. With this option, the system makes an educated guess about additional entity values from the user examples for the listed intents, and uses those values to train other entities. Any similar user examples are added to the *Annotation* view, so you can see how this option impacts training.

    ![Examples and intents list](images/oe-annotate3a.png)

    **Note**: You can also delete annotations from this view by selecting user examples, and choosing **Delete**.

## Editing entities

You can click any entity in the list to open it for editing. You can rename or delete entities, and you can add, edit, or delete values, synonyms, or patterns.

> **Note**: If you change the entity type from `synonym` to `pattern`, or vice versa, the existing values are converted, but might not be useful as-is.

## Searching entities

Use the Search feature to find entity names, values and synonyms.

1.  Select the **Entities** tab in the navigation bar, then *My Entities*.

    ![Entity tab overview](images/entity_oview.png)

    **Note**: System entities are not searchable.

1.  Select the Search icon: ![Search icon](images/search_icon.png)

1.  Enter a search term or phrase.

    ![Entity search term](images/searchent_1.png)

### Results

Entities containing your search term, with corresponding examples, are shown.

  ![Entity search return](images/searchent_2.png)

## Importing entities

If you have a large number of entities, you might find it easier to import them from a comma-separated value (CSV) file than to define them one by one in the {{site.data.keyword.conversationshort}} tool.

1.  Collect the entities into a CSV file, or export them from a spreadsheet to a CSV file. The required format for each line in the file is as follows:

    ```
    <entity>,<value>,<synonyms>
    ```
    {: screen}

    where &lt;entity&gt; is the name of an entity, &lt;value&gt; is a value for the entity, and &lt;synonyms&gt; is a comma-separated list of synonyms for that value.

    ```
    weekday,Monday,Mon
    weekday,Tuesday,Tue,Tues
    weekday,Wednesday,Wed
    weekday,Thursday,Thur,Thu,Thurs
    weekday,Friday,Fri
    weekday,Saturday,Sat
    weekday,Sunday,Sun
    month,January,Jan
    month,February,Feb
    month,March,Mar
    month,April,Apr
    month,May
    ```
    {: screen}

    Importing a CSV file also supports patterns. Any string wrapped with `/` will be considered a pattern (as opposed to a synonym).

    ```
    ContactInfo,localPhone,/(\d{3})-(\d{4})/
    ContactInfo,fullUSphone,/(\d{3})-(\d{3})-(\d{4})/
    ContactInfo,internationalPhone,/^(\(?\+?[0-9]*\)?)?[0-9_\- \(\)]*$/
    ContactInfo,email,/\b[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Za-z]{2,}\b/
    ContactInfo,website,/(https?:\/\/)?([\da-z\.-]+)\.([a-z\.]{2,6})([\/\w \.-]*)*\/?$/
    ```
    {: screen}

    Save the CSV file with UTF-8 encoding and no byte order mark (BOM). The maximum CSV file size is 10MB. If your CSV file is larger, consider splitting it into multiple files and importing them separately.  In the {{site.data.keyword.conversationshort}} tool, open your conversational skill and then click the **Entities** tab.
    {: tip}

1.  Click ![Import](images/importGA.png) and then drag a file, or browse to select a file from your computer. The file is validated and imported, and the system begins to train itself on the new data.

### Results

You can view the imported entities on the Entities tab. You might need to refresh the page to see the new entities.

## Exporting entities
{: #export_entities}

You can export a number of entities to a CSV file, so you can then import and reuse them for another {{site.data.keyword.conversationshort}} application.

Exporting a CSV file supports patterns. Any string wrapped with `/` will be considered a pattern (as opposed to a synonym).
{: tip}

1.  Select the entities you want, then select **Export**.

    ![Export entity button](images/ExportEntity.png)

## Deleting entities
{: #delete_entities}

You can select a number of entities for deletion.

**IMPORTANT**: By deleting entities you are also deleting all associated values, synonyms, or patterns, and these items cannot be retrieved later. All dialog nodes that reference these entities or values must be updated manually to no longer reference the deleted content.

1.  Select the entities you want, then select **Delete**.

    ![Delete entity button](images/DeleteEntity.png)

## Enabling system entities
{: #enable_system_entities}

The {{site.data.keyword.conversationshort}} service provides a number of *system entities*, which are common entities that you can use for any application. Enabling a system entity makes it possible to quickly populate your skill with training data that is common to many use cases.

System entities can be used to recognize a broad range of values for the object types they represent. For example, the `@sys-number` system entity matches any numerical value, including whole numbers, decimal fractions, or even numbers written out as words.

System entities are centrally maintained, so any updates are available automatically. You cannot modify system entities.

1.  On the Entities tab, click **System entities**.

    ![Screen capture of "System entities" tab](images/system_entities_1.png)

1.  Browse through the list of system entities to choose the ones that are useful for your application.
    - To see more information about a system entity, including examples of matching input, click the entity in the list.
    - For details about the available system entities, see [System entities](system-entities.html).

1.  Click the toggle switch next to a system entity to enable or disable it.

### Results

After you enable system entities, the {{site.data.keyword.conversationshort}} service begins retraining. After training is complete, you can use the entities.
