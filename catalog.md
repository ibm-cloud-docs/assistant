---

copyright:
  years: 2015, 2018
lastupdated: "2018-03-08"

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

# Using content catalogs

***Content Catalogs*** provide an easy way to add common intents to your {{site.data.keyword.conversationshort}} conversational skill.
{: shortdesc}

**IMPORTANT**: Content Catalog intents are meant to provide a starting point, and not meant to be fully built-out for production use. It is recommended that you review and expand on these intents, to make them better suited to how your application will use them.

## Adding a content catalog to your conversational skill
{: #add-catalog}

Use the {{site.data.keyword.conversationshort}} tool to add content catalogs.

1.  In the {{site.data.keyword.conversationshort}} tool, open your conversational skill and then select the **Content Catalog** tab in the navigation bar.

1.  Select a content catalog, such as *Banking*, to see the intents that are provided with it.

    ![Screen capture showing available catalogs](images/catalog_overview.png)

    You will see information about the intents that are defined in the *Banking* category.

    ![Screen capture showing Banking category intents](images/catalog_open.png)

    Intents that are added from a content catalog are distinguishable from other intents by their naming convention; in this case, `#Banking_ . . .`

1.  Select ![Close arrow](images/close_arrow.png) to return to the **Content Catalog** tab.

1.  Next, add the *Banking* content catalog to your conversational skill by clicking the `Add to workspace` button. You will see a message indicating that the *Banking* intents have been added.

    ![Screen capture showing Add to workspace button](images/catalog_addtobot.png)

1.  Now, select the **Intents** tab, and verify that the *Banking* intents have been added to your conversational skill.

    ![Screen capture showing Banking intents listed on Intents tab](images/catalog_intents.png)

### Results

The intents from the *Banking* content catalog have been added to the **Intents** tab of your conversational skill, and the system begins to train itself on the new data.

## Editing content catalog examples

Like any other intent, once the *Banking* content catalog intents have been added, you can make the following changes:

- Rename the intent.
- Delete the intent.
- Add, edit, or delete examples.
- Move an example to a different intent.

You can tab from the intent name to each example, editing the examples if you choose.

To move or delete an example, select the example by selecting the check box and then select **Move** or **Delete**.

  ![Screen capture showing how to move or delete an example](images/catalog_edit.png)
