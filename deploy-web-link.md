---

copyright:
  years: 2015, 2018
lastupdated: "2018-04-12"

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

# Testing the assistant from a web-hosted chat widget
{: #deploy-web-link}

After you configure a conversational skill and add it to an assistant, the assistant is immediately available for testing from a web page.
{: shortdesc}

The assistant is implemented as a chat widget embedded in a simple IBM-branded web page automatically. You can test the conversational skill that you added to the assistant by entering text into the chat widget. You can also share the URL of the page with others to enlist help in testing and getting feedback about the assistant.

To test the assistant from the web-hosted chat widget, complete the following steps:

1.  From the Assistants tab, click to open the assistant tile that you want to test.

1.  From the Integrations section, click the *Shareable Link* tile.

1.  **Optional**: Click the **Edit Integration name** ![Edit Integration](images/edit-integration.png) icon to change the integration name, and the **Edit Integration description** ![Edit Integration](images/edit-integration.png) icon to change the description.

1.  Click **Create Shareable Link**.

1.  Click **Visit public link**.

1.  Submit a test utterance to see how the assistant responds.

**Note**: The welcome message you defined for the Welcome node of your dialog is not displayed in this chat widget. It is displayed in the 'Try it out' pane when you test the assistant within the tool. However, it is not triggered from here because nodes with the `welcome` special condition are skipped in dialog flows that are started by users. The chat widget on this page waits for the user to initiate the conversation. If you need to set default values for context variables at the start of your conversation, see [Starting the dialog](add-integrations.html#dialog-start) for tips.

The dialog flow is restarted after 60 minutes of inactivity. Meaning if you stop testing the assistant, after 60 minutes any context variable values that were set during the previous conversation are reset to null or to their default values.
