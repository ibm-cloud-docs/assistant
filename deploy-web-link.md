---

copyright:
  years: 2015, 2018
lastupdated: "2018-10-23"

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

# Integrating with a web-hosted chat widget
{: #deploy-web-link}

After you configure a dialog skill and add it to an assistant, the assistant is immediately available for testing from a web page.
{: shortdesc}

The assistant is implemented as a chat widget embedded in a simple IBM-branded web page automatically. You can test the dialog skill that you added to the assistant by entering text into the chat widget. You can also share the URL of the page with others to enlist help in testing and getting feedback about the assistant.

Unlike when you test using the "Try it out" pane in the tool, any API calls that result from your interactions with the assistant hosted by the shareable URL do incur charges.

## Using the shareable link integration to test your assistant
{: #use-provided-widget}

To test the assistant from a web-hosted chat widget, complete the following steps:

1.  From the Assistants tab, click to open the assistant tile that you want to test.

1.  From the Integrations section, click the *Shareable Link* tile.

1.  **Optional**: Click the ![Edit integration name](images/edit-integration.png) icon to change the integration name, and the ![Edit integration description](images/edit-integration.png) icon to change the description.

1.  Click the public page URL that is displayed to open the test page.

    A separate web browser tab opens that contains a chat widget implementation of your assistant.

1.  Submit test utterances to see how the assistant responds. (No responses are returned until after you build a dialog in the attached skill.)

    **Note**: The welcome message you defined for the Welcome node of your dialog is not displayed in this chat widget. It is displayed in the "Try it out" pane when you test the assistant within the tool. However, it is not triggered from here because nodes with the `welcome` special condition are skipped in dialog flows that are started by users. The chat widget on this page waits for the user to initiate the conversation. If you need to set default values for context variables at the start of your conversation, see [Starting the dialog](dialog-start.html) for tips.

    The dialog flow is restarted after 60 minutes of inactivity. This means that if you stop testing the assistant, after 60 minutes, any context variable values that were set during the previous conversation are reset to null or to their default values.

    **Note**: The inactivity period is 5 minutes for Lite and Standard plans.

1.  After testing, you can close the browser tab to exit the public web page.

1.  From the tool, click **Save Changes** to save any edits you made to the shareable link integration and close the page or **X** to close the page without saving.

## Dialog considerations
{: #weblink-dialog}

The rich responses that you add to a dialog are displayed in the web-hosted chat widget as expected, with the following exceptions:

- **Connect to human agent**: This response type is ignored.

- **Pause**: This response type pauses the assistant's activity in the chat widget. However, activity does not resume after the pause until another response is triggered. Whenever you include a `pause` response type, add another, different response type, such as `text`, after it.

See [Rich responses](dialog-overview.html#multimedia) for more information about response types.

## Adding a shareable link integration
{: #add-another-widget}

If you accidentally deleted the shareable link integration that is created automatically for you or want to create another, separate public URL, you can add a shareable link integration.

1.  From the Assistant tab, click to open the tile for the assistant that you want to deploy.

1.  Go to the Integrations section, and then click **Add Integration**.

1.  Find *Shareable Link* in the list, and then click **Select Integration**.

1.  Click **Create Shareable Link**.

1.  **Optional**: To test the assistant from the newly created public web page, you can click **Visit public link**.

    A separate web browser tab opens that contains a chat widget implementation of your assistant. You can submit test utterances to see how the assistant responds. After testing, close the browser tab to exit the public web page.

1.  From the tool, click **Close** to close the Shareable Link integration page.
