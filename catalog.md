---

copyright:
  years: 2015, 2019
lastupdated: "2019-02-28"

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

# Using content catalogs
{: #catalog}

***Content Catalogs*** provide an easy way to add common intents to your {{site.data.keyword.conversationshort}} dialog skill.
{: shortdesc}

Intents you add from the catalog are meant to provide a starting point. Add to or edit the catalog intents to tailor them for your use case.

## Adding a content catalog to your dialog skill
{: #catalog-add}

Use the {{site.data.keyword.conversationshort}} tool to add content catalogs.

1.  In the {{site.data.keyword.conversationshort}} tool, open your dialog skill and then click the **Content Catalog** tab.

1.  Select a content catalog, such as *Banking*, to see the intents that are provided with it.

    ![Screen capture showing available catalogs](images/catalog_overview.png)

    You will see information about the intents that are included in the catalog.

    ![Screen capture showing Banking category intents](images/catalog_open.png)

    Intents that are added from a content catalog are distinguishable from other intents by their names. Each intent name is prepended with the content catalog name.

1.  Select ![Close arrow](images/close_arrow.png) to return to the **Content Catalog** tab.

1.  Next, add a content catalog to your dialog skill by clicking the `Add to skill` button.

1.  Now, select the **Intents** tab, and verify that the intents from the catalog were added and are available.

    ![Screen capture showing Banking intents listed on Intents tab](images/catalog_intents.png)

The system begins to train itself on the new data.

After you add a catalog to your skill, the intents become part of your training data. If IBM makes subsequent updates to a content catalog, the changes are not automatically applied to any intents you added from a catalog.
{: note}

## Editing content catalog examples
{: #catalog-edit-content}

Like any other intent, after you add content catalog intents to your skill, you can make the following changes to them:

- Rename the intent.
- Delete the intent.
- Add, edit, or delete examples.
- Move an example to a different intent.
