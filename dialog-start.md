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

# Starting the dialog
{: #dialog-start}

You cannot use the built-in welcome node to start a dialog in the same way for all integrations. The response you define for the welcome node in the dialog is displayed in the 'Try it out' pane when you test the assistant within the tool. However, it is not displayed from channel integrations because nodes with the `welcome` special condition are skipped in dialog flows that are started by users. Deployed assistants typically wait for users to initiate conversations with them, not the other way around. The `conversation_start` special condition is always triggered at the start of a dialog. You can use a combination of nodes with these two special conditions to manage the start of your dialog in a consistent way.

Complete the following steps to manage the dialog start:

1.  Add a dialog node above the Welcome node that is provided when you initially create the dialog.

1.  Set the `conversation_start` special condition as the node condition for this newly added node.

1.  Define any context variables that you want to set with default values for the dialog in the `conversation_start` node.

1.  Do not define a text response for this node.

1.  Configure this node to jump to the `Welcome` node directly below it in the dialog tree, and choose **If bot recognizes (condition)**.

![Screenshot of the dialog tree with a conversation_start node jumping to a welcome node below it.](images/dialog-start.png)

This design results in a dialog that works like this:

- Whatever the integration type, the `conversation_start` node is processed, which means any context variables that you define in it are initialized.
- In deployments where the assistant starts the dialog flow, the `Welcome` node is triggered and its text response is displayed.
- In deployments where the user starts the dialog flow, the user's first input is evaluated and then processed by the node that can provide the best response.