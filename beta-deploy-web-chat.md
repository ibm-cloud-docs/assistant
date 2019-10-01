---

copyright:
  years: 2015, 2019
lastupdated: "2019-10-01"

subcollection: assistant

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:external: target="_blank" .external}
{:deprecated: .deprecated}
{:important: .important}
{:note: .note}
{:tip: .tip}
{:preview: .preview}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:swift: .ph data-hd-programlang='swift'}

# Integrating with your own website
{: #deploy-web-chat}

Add your assistant to your company website as a web chat widget.
{: shortdesc}

This feature is available for use by participants in the early access program only. To find out how to request access, see [Participate in the early access program](/docs/services/assistant?topic=assistant-feedback#feedback-beta).
{: preview}

When you participate in the early access program, IBM gives you early access to features for your evaluation. These features are classified as beta, which means they might be unstable, might change frequently, and might be discontinued with short notice. Beta features also might not provide the same level of performance or compatibility that generally available features provide and are not intended for use in a production environment. Beta features are supported only on the [IBM Developer Answers](https://developer.ibm.com/answers/topics/watson-assistant/){: external}.

When you create a Web Chat integration, code is generated that calls a script written in JavaScript which instantiates a unique instance of your assistant. You can then copy and paste the HTML `<script>` element into any page or pages on your website where you want users to be able to ask your assistant for help.

![Plus or Premium plan only](images/plus.png) When the feature is generally available, it will be available to Plus or Premium plan users only.

## Create a web chat instance to add to your website
{: #deploy-web-chat-task}

To add the assistant to a web page on your company website, complete the following steps:

1.  From the Assistants tab, click to open the assistant tile that you want to deply to your site.

1.  From the *Integrations* section, click the **Web Chat** tile.

1.  **Optional**: Change the web chat integration name from *Web Chat* to something more descriptive.

1.  Click **Create** to generate the script.

    A code snippet is created and added to the bottom of the page that contains an HTML `<script>` element. The script tag calls JavaScript code that is hosted on an IBM site. The code instantiates an instance of your assistant within a web chat widget. The generated code includes a region and unique integration ID. Do not change these parameter values.

1.  **Optional**: Customize the chat. You can make the following changes:

    - **Public assistant name**. Name by which the assistant is known to users. This name is displayed in the header of the chat window. 
    - **Font family**. List one or more font styles that you want applied to the text that is displayed in the chat window. Separate multiple font names with commas. The first font family in the list is used unless the web page does not support it, in which case, the next font family in the list is used. Surround font family names that include spaces with single quotation marks. For example, `Georgia,'Helvetica Neue'`. 
    
      If you don't specify a font, these fonts are used: `IBMPlexSans, Arial, Helvetica, sans-serif`. 

    - **Accent color**. This color is applied to the following elements:

      - Chat launcher button that is embedded in your web page
      - Send button associated with the input text field
      - Input text field border when in focus
      - Marker that shows the start of the assistant’s response
      - Border of a button after it is clicked
      - Typing indicator that is shown to repesent a pause response
      - Active state for dropdown color of border 

    - **Secondary access color**. This color is applied to the chat launcher button after it is clicked.
  
    Use HTML color codes to specify the colors. For example, `#FF33FC` is pink and `#329A1D` is green.
    {: note}

1.  Copy the `<script>` HTML element.

1.  Open the HTML source for a web page on your website where you want the chat window to be displayed. Paste the code snippet into the page.

    Paste the code as close to the closing `</body>` tag as possible to prevent the script from blocking the rest of the page from rendering.
    {: tip}

    The following HTML snippet is the source for a test page that you can copy and save as a file with a .html extension for testing purposes. You would replace the script element block here with the script elements you copied from the Web Chat integration setup page.

    ```html
    <html>
    <head></head>
    <body>
        <title>My Test Page</title>
        <p>The body of my page.</p>
        <!-- copied script elements -->
        </body>
    </html>
    ```
    {: codeblock}

1.  Refresh the web page.

    ![Chat icon](images/web-chat-icon.png) 
    
    The web chat icon is displayed at the end of the page. Its placement is always the same regardless of where you paste the script element into the web page source. The chat launcher icon is blue unless you customize the accent color.
    {: important}

1.  Click the icon to open the chat window and talk to your assistant.

    ![Web chat window](images/web-chat-window.png)

1.  Paste the code snippet into each web page where you want the assistant to be available to your customers.

    You can paste the same script tag into as many pages on your website as you want. Add it anywhere where you want users to be able to reach your assistant for help. However, be sure to add it only one time per page.
    {: tip}

1.  Submit test utterances from the chat widget that is displayed on your web page to see how the assistant responds.

    No responses are returned until after you create a dialog skill and add it to the assistant.
    {: note}

    If you don't extend the session timeout setting for the assistant, the dialog flow for the current session is restarted after 60 minutes of inactivity. This means that if a user stops interacting with the assistant, after 60 minutes, any context variable values that were set during the previous conversation are set to null or back to their initial values.

1.  Click **Save changes** to save the web chat name and description information that you added and close the integration page. Alternatively, you can click the **X** to close the page. 

    The web chat integration instance is created as soon as you click the *Create* button, and does not need to be saved.

## Extending the web chat
{: #deploy-web-chat-extend}

You can make more advanced customizations and extend the capability of the web chat by using the {site.data.keyword.conversationshort}} Web Chat toolkit on [GitHub](https://github.com/watson-developer-cloud/assistant-web-chat){: external}.

If you choose to use the provided methods, you implement them by editing the code snippet that was generated earlier. You then embed the updated code snippet into your web page.

For example, the following updated script preserves the context for the conversation. In addition, it adds an `$ismember` context variable and sets it to `true`.

```html
<script src="https://assistant-web.watsonplatform.net/loadWatsonAssistantChat.js"></script>
<script>
  // Following the v2 message API, we add some items to context.
  function preSendhandler(event) {
    event.data.context = event.data.context || {};
    event.data.context.skills = event.data.context.skills || {};
    event.data.context.skills['main skill'] = event.data.context.skills.main_skill || {};
    event.data.context.skills['main skill'].user_defined = event.data.context.skills['main skill'].user_defined || {};
    event.data.context.skills['main skill'].user_defined.ismember = true;
  }
</script>
<script>
  window.loadWatsonAssistantChat({
    integrationID: '{INTEGRATION ID}',
    region: '{REGION}'
  }).then(function(instance){
    // When this promise returns, we know WatsonAssistantChat is ready.
    instance.on({ type: "pre:send", handler: preSendhandler });
    instance.render();
  });
</script>
```
{: codeblock}

You can reference the `$ismember` context variable from your dialog. For example, you might add a dialog node that conditions on the `#General_Greetings` intent that is defined in the **General** content catalog. Add multiple conditioned response to the node. In the condition for the first response, add `$ismember`, and draft a response to show to customers who are members of your Rewards Program, such as `Hello, valued Rewards Program member!` The next conditioned response can condition on `true` and specify the response to show to everyone else, such as `Hello!`.
