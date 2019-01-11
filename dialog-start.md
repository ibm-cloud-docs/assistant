---

copyright:
  years: 2015, 2019
lastupdated: "2019-01-11"

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

# Starting the dialog
{: #dialog-start}

You cannot use the built-in welcome node to start a dialog in the same way for all integrations. Use this workaround instead.
{: shortdesc}

The response you define for the welcome node in the dialog is displayed to initiate a conversation from the "Try it out" pane within the tool, and from the chat widget in the Preview Link integration. However, it is not displayed from many of the other channel integrations because nodes with the `welcome` special condition are skipped in dialog flows that are started by users. And deployed assistants typically wait for users to initiate conversations with them, not the other way around.

Unlike the `welcome` special condition, the `conversation_start` special condition is always triggered at the start of a dialog. You can use a combination of nodes with these two special conditions (`welcome` and `conversation_start`) to manage the start of your dialog in a consistent way.

Complete the following steps to manage the dialog start:

1.  Add a dialog node above the Welcome node that is automatically added to the top of the dialog tree when you create the dialog.

1.  Set the node condition for this newly added node to `conversation_start`, which is a special condition as described earlier.

1.  Define any context variables that you want to set with default values for the dialog in the `conversation_start` node.

1.  Do not define a text response for this node.

1.  Configure this node to jump to the `Welcome` node directly below it in the dialog tree, and choose **If bot recognizes (condition)**.

![Screenshot of the dialog tree with a conversation_start node jumping to a welcome node below it.](images/dialog-start.png)

This design results in a dialog that works like this:

- Whatever the integration type, the `conversation_start` node is processed, which means any context variables that you define in it are initialized.
- In integrations where the assistant starts the dialog flow, the `Welcome` node is triggered and its text response is displayed.
- In integrations where the user starts the dialog flow, the user's first input is evaluated and then processed by the node that can provide the best response.
