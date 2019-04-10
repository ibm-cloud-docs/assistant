---

copyright:
  years: 2015, 2019
lastupdated: "2019-04-09"

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

# Integrating with a web-hosted chat widget
{: #deploy-web-link}

If you do not disable the preview link, then the assistant is immediately available for testing from a web page.
{: shortdesc}

The assistant is implemented as a chat widget embedded in a simple IBM-branded web page automatically. You can test the dialog skill that you added to the assistant by entering text into the chat widget. You can also share the URL of the page with others to enlist help in testing and getting feedback about the assistant.

Unlike when you test using the "Try it out" pane in the tool, any API calls that result from your interactions with the assistant hosted by the Preview Link URL do incur charges.

## Using the Preview Link integration to test your assistant
{: #deploy-web-link-try}

To test the assistant from a web-hosted chat widget, complete the following steps:

1.  From the Assistants tab, click to open the assistant tile that you want to test.

1.  From the *Integrations* section, click the **Preview Link** tile.

    If you did not enable the preview link when you created the assistant, then click **Add integration**, and then click the **Preview Link** integration tile.

1.  **Optional**: Change the preview web page name and description.

1.  Click the URL link that is displayed to open the test page.

    A separate web browser tab opens that contains a chat widget implementation of your assistant.

    If your service instance was created in the United Kingdom data center before 13 December 2018 or in Sydney before 7 May 2018, then you must edit the preview link URL. The URL includes a location code for the data center where the instance is hosted. Because instances in London and Sydney were syndicated to Dallas prior to the specified dates, you must replace the `eu-gb` or `au-syd` reference in the URL with `us-south` for the web page to render properly.
    {: important}

1.  Submit test utterances to see how the assistant responds.

    No responses are returned until after you create a dialog skill and add it to the assistant.

    The dialog flow for the current session is restarted after 60 minutes of inactivity (5 minutes for Lite and Standard plans). This means that if a user stops interacting with the assistant, after 60 (or 5) minutes, any context variable values that were set during the previous conversation are set to null or back to their default values.

1.  After testing, you can close the browser tab to exit the public web page.

1.  From the tool, click **Save Changes** to save any edits you made to the preview link integration and close the page, or click **X** to close the page without saving.

## Dialog considerations
{: #deploy-web-link-dialog}

The rich responses that you add to a dialog are displayed in the web-hosted chat widget as expected, with the following exceptions:

- **Connect to human agent**: This response type is ignored.

- **Pause**: This response type pauses the assistant's activity in the chat widget. However, activity does not resume after the pause until another response is triggered. Whenever you include a `pause` response type, add another, different response type, such as `text`, after it.

See [Rich responses](/docs/services/assistant?topic=assistant-dialog-overview#dialog-overview-multimedia) for more information about response types.

## Adding a Preview Link integration
{: #deploy-web-link-add-more}

If you accidentally deleted the preview link integration that is created automatically for you or want to create another, separate public web page, you can add a preview link integration.

1.  From the Assistant tab, click to open the tile for the assistant that you want to deploy.

1.  Go to the **Integrations** section, and then click **Add integration**.

1.  Click the **Preview Link** integration tile.

1.  Optionally edit the preview link integration name and description, and then click **Create**.

    A URL is added to the page. Click it to open the preview web page.
