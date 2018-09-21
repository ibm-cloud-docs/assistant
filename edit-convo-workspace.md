---

copyright:
  years: 2015, 2018
lastupdated: "2018-09-04"

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

# Accessing workspaces
{: #edit-convo-workspace}

If you created a workspace with the generally available version of the {{site.data.keyword.conversationfull}} service (formerly known as {{site.data.keyword.watson}} Conversation), you can continue to use it.
{: shortdesc}

## Changing the conversation workspace
{: #edit-convo}

To make changes to a legacy conversation workspace, complete the following steps:

1.  From the {{site.data.keyword.conversationshort}} home page, click **Skills**.

    Any workspaces that you created with the generally available version of the {{site.data.keyword.conversationshort}} service are displayed as dialog skill tiles.
1.  Click the dialog skill that represents the workspace you want to edit.
1.  Make any changes or additions to the training data or dialog that you want to make.
1.  Save your changes.

Once training is completed, your updates are available to the custom application from which you are calling the service. See the **v1 API reference** for more information.

## Limitations
{: #workspace-cons}

With the latest version of the service, you can continue to do everything you could do with the legacy service but with more flexibility. Maybe you created a workspace with an earlier version of the service and are calling it from an existing application that you do not want to replace? It already manages state and performs useful functions on individual dialog turns that you want to continue to control. You can still orchestrate between calls with the latest version of the /message API. The advantage is that you don't have to. In the latest version, you can support more than one integration channel at a time with the same underlying dialog skill.

If you choose to skip the step of creating an assistant and adding your dialog skill to it, you will miss out on the simplicity that assistants provide. Namely, you **cannot** use a single conversation to interact with customers through multiple integration channels at once, and quickly expand to new channels that become popular with users.
