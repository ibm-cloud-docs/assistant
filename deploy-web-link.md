---

copyright:
  years: 2015, 2020
lastupdated: "2020-10-19"

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

# Testing your assistant with the preview link
{: #deploy-web-link}

If you do not disable the preview link when you create an assistant, the assistant is immediately available for testing from a web page.
{: shortdesc}

The preview link renders your assistant as a web chat integration that is embedded in a simple IBM-branded web page. You can test the skills that you added to the assistant by entering text into the chat window. You can also share the URL of the page with others to enlist help in testing and getting feedback about the assistant.

Unlike when you test using the "Try it out" pane, any API calls that result from your interactions with the assistant hosted by the preview link URL do incur charges.

## Using the preview link integration to test your assistant
{: #deploy-web-link-try}

To test the assistant from a web-hosted chat widget, complete the following steps:

1.  From the Assistants page, click to open the assistant tile that you want to test.

1.  From the *Integrations* section, click the **Preview link** tile.

    If you did not enable the preview link when you created the assistant, then click **Add integration**, and then click the **Preview link** integration tile.

1.  **Optional**: Change the preview web page name and description.

1.  Click the URL link that is displayed to open the test page.

    A separate web browser tab opens that contains a chat window where you can interact with your assistant.

1.  Submit test utterances to see how the assistant responds.

    No responses are returned until after you create a dialog skill and add it to the assistant.

    The dialog flow for the current session is restarted after the inactivity period is passed. The inactivity period can range from 5 minutes to 7 days depending on what you configure for your service plan type. This means that if a user stops interacting with the assistant, after the intactivity time frame passes, any context variable values that were set during the previous conversation are set to null or back to their default values. For more information, see [Changing the inactivity timeout setting](/docs/assistant?topic=assistant-assistant-settings#assistant-settings-change-timeout).

1.  After testing, you can close the browser tab to exit the public web page.

1.  Click **Save Changes** to save any edits you made to the preview link integration and close the page, or click **X** to close the page without saving.

## Dialog considerations
{: #deploy-web-link-dialog}

The rich responses that you add to a dialog are displayed in the web-hosted chat widget as expected, with the following exceptions:

- **Connect to human agent**: This response type is ignored.

- **Pause**: This response type pauses the assistant's activity in the chat widget. However, activity does not resume after the pause until another response is triggered. Whenever you include a `pause` response type, add another, different response type, such as `text`, after it.

See [Rich responses](/docs/assistant?topic=assistant-dialog-overview#dialog-overview-multimedia) for more information about response types.

## Adding a preview link integration
{: #deploy-web-link-add-more}

If you accidentally deleted the preview link integration that is created automatically for you or want to create another, separate public web page, you can add a preview link integration.

1.  From the Assistant tab, click to open the tile for the assistant that you want to deploy.

1.  Go to the **Integrations** section, and then click **Add integration**.

1.  Click the **Preview link** integration tile.

1.  Optionally edit the preview link integration name and description, and then click **Create**.

    A URL is added to the page. Click it to open the preview web page.
