---

copyright:
  years: 2015, 2018
lastupdated: "2018-10-26"

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

# Learn from conversations
{: #logs}

To open a list of messages between users and the assistant that uses this dialog skill, select **User conversations** in the navigation bar.
{: shortdesc}

When you open the **User conversations** page, the default view lists results for the last day, with the newest results first. The top intent (#intent) and any recognized entity (@entity) values used in a message, and the message text are available. For intents that are not recognized, the value shown is *Irrelevant*. If an entity is not recognized, or has not been provided, the value shown is *No entities found*.
![Logs default page](images/logs_page1.png)

It is important to note that the **User conversations** page displays the total number of *messages* between users and your application. A message is a single utterance the user sends to the application. Each conversation may be made up of multiple messages. Thus, the number of results on this **User conversations** page is different than the number of conversations shown on the [Overview](logs_oview.html) page.

## Log limits
{: #log-limits}

The length of time for which messages are retained depends on your {{site.data.keyword.conversationshort}} service plan:

  Service plan                         | Chat message retention
  ------------------------------------ | ------------------------------------
  Premium                              | Last 90 days
  Standard                             | Last 30 days
  Lite                                 | Last 7 days

## Filtering messages

You can filter messages by *Search user statements*, *Intents*, *Entities*, and *Last* n *days*:

*Search user statements* - Type a word in the search bar. This searches the users' inputs, but not your application's replies.

*Intents* - Select the drop-down menu and type an intent in the input field, or choose from the populated list. You can select more than one intent, which filters the results using any of the selected intents, including *Irrelevant*.

![Intents drop-down menu](images/intents_filter.png)

*Entities* - Select the drop-down menu and type an entity name in the input field, or choose from the populated list. You can select more than one entity, which filters the results by any of the selected entities. If you filter by intent *and* entity, your results will include the messages that have both values. You can also filter for results with *No entities found*.

![Entities drop-down menu](images/entities_filter.png)

Messages may take some time to update. Allow at least 30 minutes after a user's interaction with your application before attempting to filter for that content.

## Viewing an individual message
You can expand each message entry to see what the user said in the whole conversation, and how your application answered. To do this, select **Open conversation**. You are automatically taken to the message you selected within that conversation.

**NOTE**: The time shown at the top of each conversation is localized to reflect the time zone of your browser. This may differ from the timestamp shown if you review the same conversation log via an API call; API log calls are always shown in UTC.

![Open conversation panel](images/open_convo.png)

You can then choose to show the classification(s) for the message you selected.

![Open conversation panel with classifications](images/open_convo_classes.png)

## Improving across assistants
{: #deploy_id}

Creating a dialog skill is an iterative process. While you develop your skill, you use the *Try it out* pane to verify that the service recognizes the correct intents and entities in test inputs, and to make corrections as needed.

From the User conversations page, you can analyze actual interactions between the assistant you used to deploy the skill and your users. Based on those interactions, you can make corrections to improve the accuracy with which intents and entities are recognized by your dialog skill. It is difficult to know exactly *how* your users will ask questions, or what random messages they might submit, so it is important to frequently analyze real conversations to improve your dialog skills.

For a {{site.data.keyword.conversationshort}} instance that includes multiple assistants, there may be times when it is useful to use message data from the dialog skill of one assistant to improve the dialog skill used by another assistant within that same instance.

![Premium plan only](images/premium0.png) If you are a {{site.data.keyword.conversationshort}} Premium user, your premium instances can optionally be configured to allow access to log data from assistants across your different premium instances.

As an example, say you have a {{site.data.keyword.conversationshort}} instance named *HelpDesk*. You may have both a Production assistant and a Development assistant in your HelpDesk instance. When working in the dialog skill for the Development assistant, you can use logs from the Production assistant messages to improve the Development assistant's dialog skill.

Any edits you then make within the dialog skill for the Development assistant will only affect the Development assistant's dialog skill, even though you’re using data from messages sent to the Production assistant.

### Picking a data source
{: #pick-data-source}

The term *data source* refers to the logs compiled from conversations between customers and the assistant or custom application by which a dialog skill was deployed.

When you open the *Improve* tab, metrics are shown that were generated by user interactions with the current dialog skill. No metrics are shown if the current skill has not been deployed and used by customers.

To populate the metrics with message data from a dialog skill that was added to a different assistant or custom application, one that has interacted with customers, complete these steps:

1.  Click the **Data source** field to see a list of assistants with log data that you might want to use.

    See [*Show deployment IDs* explained](#deployment-id-explained) for more information about that option.

1.  Choose a data source.

Statistical information for the selected data source is displayed.

### *Show deployment IDs* explained
{: #deployment-id-explained}

Applications that use the V1 version of the API must specify a deployment ID in each messages sent using the `/message` API. This ID identifies the deployed app that the call was made from. The Improve page can use this deployment ID to retrieve and display logs that are associated with a specific live application.

For assistants or custom apps that use the V2 version of the API, the service automatically includes a system id and skill id with each /message call, so you can choose a data source by assistant name instead of using a deployment ID.

To add the deployment ID, V1 API users include the deployment property inside the metadata of the [context ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#message){: new_window}, as in this example:

```
"context" : {
  "metadata" : {
       "deployment": "HelpDesk-Production"
  }
}
```
{: codeblock}

## Making training data improvements

Use insights from real user conversations to correct the model associated with your dialog skill.

If you use data from another data source, any improvements you make to the model are applied to the current dialog skill only. The **Data source** field shows the source of the messages you are using to improve this dialog skill, and the top of the page shows the dialog skill you are applying changes to.

### Correcting an intent

1.  To correct an intent, select the ![Edit](images/edit_icon.png) edit icon beside the chosen #intent.
1.  From the list provided, select the correct intent for this input.
    - Begin typing in the entry field and the list of intents is filtered.
    - You can also choose **Mark as irrelevant** from this menu. (For more information, see [Mark as irrelevant](intents.html#mark-irrelevant).) Or, you can choose **Do not train on intent**, which does not save this message as an example for training.

    ![Select intent](images/select_intent.png)
1.  Select **Save**.

    ![Save intent](images/save_intent.png)

    **Note**: The {{site.data.keyword.conversationshort}} service supports adding user input as an example to an intent *as-is*. If you are using @entity references as examples in your intent training data, and a user message that you want to save contains an entity value or synonym from your training data, then you must edit the message later. After you save it, edit the message from the Intents page to replace the entity that it references. For more information, see [Directly referencing an @Entity as an intent example](intents.html#entity-as-example).

### Adding an entity value or synonym

1.  To add an entity value or synonym, select the ![Edit](images/edit_icon.png) edit icon beside the chosen @entity.
1.  Select **Add entity**.

    ![Add entity](images/add_entity.png)
1.  Now, select a word or phrase in the underlined user input.

    ![Select entity](images/select_entity.png)
1.  Choose an entity to which the highlighted phrase will be added as a value.
    - Begin typing in the entry field and the list of entities and values is filtered.
    - To add the highlighted phrase as a synonym for an existing value, choose the `@entity:value` from the drop-down list.

    ![Add entity word](images/add_entity_word.png)
1.  Select **Save**.

    ![Save entity](images/add_entity_save.png)
