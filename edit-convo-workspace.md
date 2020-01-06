---

copyright:
  years: 2015, 2020
lastupdated: "2019-04-12"

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

# Accessing workspaces
{: #edit-convo-workspace}

If you created a workspace with an earlier version of {{site.data.keyword.conversationshort}} (even back when it was known as {{site.data.keyword.watson}} Conversation), you can continue to use the workspace.
{: shortdesc}

## Changing the conversation workspace
{: #edit-convo-workspace-task}

Your workspace is still available; it is just referred to as a *skill* now. To make changes to a legacy workspace, complete the following steps:

1.  From the {{site.data.keyword.conversationshort}} home page, click **Skills**.

    Any workspaces that you created with the generally available version of the {{site.data.keyword.conversationshort}} service are displayed as dialog skill tiles.
1.  Click the dialog skill that represents the workspace you want to edit.
1.  Make any changes or additions to the training data or dialog that you want to make.
1.  Save your changes.

Once training is completed, your updates are available to the custom application from which you are calling {{site.data.keyword.conversationshort}}. See [Watson Assistant API overview](/docs/services/assistant?topic=assistant-api-overview) for more information.

## Limitations
{: #edit-convo-workspace-cons}

With the latest version of {{site.data.keyword.conversationshort}}, you can continue to do everything you could do with the legacy service but with more flexibility. Maybe you created a workspace with an earlier version of {{site.data.keyword.conversationshort}} and are calling it from an existing application that you do not want to replace? It already manages state and performs useful functions on individual dialog turns that you want to continue to control. You can still orchestrate between calls with the latest version of the /message API. The advantage is that you don't have to. In the latest version, you can support more than one integration channel at a time with the same underlying dialog skill.

If you choose to skip the step of creating an assistant and adding your dialog skill to it, you will miss out on the simplicity that assistants provide. Namely, you **cannot** use a single conversation to interact with customers through multiple integration channels at once, and quickly expand or switch to new channels that become popular with users.

## Skills and workspaces
{: #edit-convo-workspace-names}

What is presented in the product user interface as a dialog skill is effectively a wrapper for a V1 workspace. While there are currently no API methods for authoring skills with the V2 API, you can continue to use the V1 API for authoring workspaces.

- When you open the product user interface, any workspaces that you created before 9 November 2018 are shown as skills. The skill name is taken from the workspace name. However, if you subsequently change the skill name or description, these changes do not affect the workspace. Likewise, if you use the APIs to edit the workspace name or description, these changes do not affect the skill.
- From the product user interface, the API details for the skill show you both the skill ID and the workspace ID.
