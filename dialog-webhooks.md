---

copyright:
  years: 2019, 2021
lastupdated: "2021-05-12"

subcollection: assistant

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:external: target="_blank" .external}
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
{:video: .video}

{{site.data.content.newlink}}

# Making a programmatic call from dialog
{: #dialog-webhooks}

To make a programmatic call, define a webhook that sends a POST request callout to an external application that performs a programmatic function. You can then invoke the webhook from one or more dialog nodes.

A webhook is a mechanism that allows you to call out to an external program based on something happening in your program. When used in a dialog skill, a webhook is triggered when the assistant processes a node that has a webhook enabled. The webhook collects data that you specify or that you collect from the user during the conversation and save in context variables. It sends the data as part of a HTTP POST request to the URL that you specify as part of your webhook definition. The URL that receives the webhook is the listener. It performs a predefined action using the information that you pass to it as specified in the webhook definition, and can optionally return a response.

Watch this video to learn more, including how to set up orchestration to call more than one external service from a dialog.

![Webhooks demo](https://www.youtube.com/embed/j8TBqD2rx2o){: video output="iframe" id="youtubeplayer" frameborder="0" width="560" height="315" webkitallowfullscreen mozallowfullscreen allowfullscreen}

To read a transcript of the video, [open the video on YouTube.com](https://www.youtube.com/watch?v=j8TBqD2rx2o&feature=emb_imp_woyt), click the *More actions* icon, and then choose *Open transcript*.

You can use a webhook to do the following types of things:

- Validate information that you collected from the user.
- Interact with an external web service to get information. For example, you might check on the expected arrival time for a flight from an air traffic service or get a forecast from a weather service.
- Send requests to an external application, such as a restaurant reservation site, to complete a simple transaction on the user's behalf.
- Trigger a SMS notification.
- Trigger a {{site.data.keyword.openwhisk}} web action.

You cannot use a webhook to call a {{site.data.keyword.openwhisk_short}} action that uses token-based Identity and Access Management (IAM) authentication. However, you can make a call to a secured {{site.data.keyword.openwhisk_short}} web action. For more information, see [Calling an IBM {{site.data.keyword.openwhisk_short}} web action](#dialog-webhooks-cf-web-action).
{: important}

For information about how to call a client application, see [Calling a client application from a dialog node](/docs/assistant?topic=assistant-dialog-actions-client).

![Plus or higher plans only](images/plus.png) For environments where private endpoints are in use, keep in mind that a webhook sends traffic over the internet. For more information, see [Private network endpoints](/docs/assistant?topic=assistant-security#security-private-endpoints).
{: note}

## Defining the webhook
{: #dialog-webhooks-create}

You can define one webhook URL for a dialog skill, and then call the webhook from one or more dialog nodes.

The programmatic call to the external service must meet these requirements:

- The call must be a POST HTTP request.
- The request body must be a JSON object (`Content-Type: application/json`).
- The response must be a JSON object (`Accept: application/json`).
- The call must return in **8 seconds or less**.

If your external service supports only GET requests, or if you need to specify URL parameters dynamically at run time, consider creating an intermediate service that accepts a POST request with a JSON payload containing any runtime values. The intermediate service can then make a request to the target service, passing these values as URL parameters, and then return the response to the dialog.
{: tip}

If you need to call a service that might not return within 8 seconds, you can manage the call through a custom client application and pass the information to the dialog as a separate step. For more information, see [Calling a client application from a dialog node](/docs/assistant?topic=assistant-dialog-actions-client).
{: tip}

To add the webhook details, complete the following steps:

1.  From the skill where you want to add the webhook, click the **Options** tab.

1.  Click **Webhooks**.

1.  In the **URL** field, add the URL for the external application to which you want to send HTTP POST request callouts.

    For example, to call the Language Translator service, specify the URL for your service instance.

    ```bash
    https://api.us-south.language-translator.watson.cloud.ibm.com/v3/translate?version=2018-05-01
    ```
    {: codeblock}

    If the external application that you call returns a response, it must be able to send back a response in JSON format. For the Language Translator service, for example, you must specify the format in which you want the result to be returned. You can do so by passing a header to the service.

1.  In the Headers section, add any headers that you want to pass to the service one at a time by clicking **Add header**.

    For example, this header indicates that the request is in JSON format.

    <table>
    <caption>Header example</caption>
      <tr>
      <th>Header name</th>
      <th>Header value</th>
      </tr>
      <tr>
      <td>Content-Type</td>
      <td>application/json</td>
      </tr>
    </table>
    
1.  If the external service requires that you pass basic authentication credentials with the request, then provide them. Click **Add authorization**, add your credentials to the **User name** and **Password** fields, and then click **Save**. 

    The product creates a base-64 encoded ASCII string from the credentials and generates a header that it adds to the page for you. 

    <table>
    <caption>Header example</caption>
      <tr>
      <th>Header name</th>
      <th>Header value</th>
      </tr>
      <tr>
      <td>Authorization</td>
      <td>Basic `<encoded-credentials>`</td>
      </tr>
    </table>

    If you use the web chat integration and enable security, you can use the same token you use to secure the web chat in the Authorization header. For more information, see [Web chat: Reusing the JWT for webhook authentication](/docs/assistant?topic=assistant-dialog-integrations#dialog-integrations-chat-jwt).
    {: tip}

Your webhook details are saved automatically.

## Adding a webhook callout to a dialog node
{: #dialog-webhooks-dialog-node-callout}

To use a webhook from a dialog node, you must enable webhooks on the node, and then add details for the callout.

1.  Click the **Dialog** tab.

1.  Find the dialog node where you want to add a callout. The callout to the webhook will occur whenever this node is triggered during a conversation with a user.

    For example, you might want to send a callout to the webhook from the `#General_Greetings` node.

1.  Click to open the dialog node, and then click **Customize**.

1.  Scroll down to the webhook section. Set the **Callout to webhooks/actions skill** switch to **On**.

1.  Select **Call a webhook**, and then click **Apply**.

    If you did not have it enabled already, the **Multiple conditioned responses** switch is set to **On** automatically and you cannot disable it. This setting is enabled to support adding different responses depending on the success or failure of the Webhook call. If you had a response specified for the node already, it becomes the first conditional response.
    {: note}

1.  Add any data that you want to pass to the external application as key and value pairs in the *Parameters* section.

    For example, if you call the Language Translator service, you must provide values for the following parameters:

    <table>
    <caption>Parameter example</caption>
      <tr>
        <th>Key</th>
        <th>Value</th>
        <th>Description</th>
      </tr>
      <tr>
        <td>model_id</td>
        <td>"en-es"</td>
        <th>Identifies the input and output languages. In this example, the request is for text in English (en) to be translated into Spanish (es).</th>
      </tr>
      <tr>
        <td>text</td>
        <td>"How are you?"</td>
        <th>This parameter contains the text string that you want the service to translate. You can hard code this value, pass a context variable, such as $saved_text, or pass the user input to the service directly, by specifying `<? input.text ?>` as this value.</th>
      </tr>
    </table>

    In more complex use cases, you might collect information during a conversation with a user about their travel plans, for example. You can collect dates and destination information and save it in context variables that you can pass to an external application as parameters.

    <table>
    <caption>Travel parameters example</caption>
      <tr>
        <th>Key</th>
        <th>Value</th>
      </tr>
      <tr>
        <td>depart_date</td>
        <td>$departure</td>
      </tr>
    <tr>
        <td>arrive_date</td>
        <td>$arrival</td>
      </tr>
    <tr>
        <td>origin</td>
        <td>$origin</td>
      </tr>
    <tr>
        <td>destination</td>
        <td>$destination</td>
      </tr>
    </table>

1.  Any response made by the callout is saved to the return variable. You can rename the variable that is automatically added to the **Return variable** field for you. If the callout results in an error, this variable is set to `null`.

    The generated variable name has the syntax `webhook_result_n`, where the appended `_n` is incremented each time you add a webhook callout to a dialog node. This naming convention ensures that context variable names are unique across the dialog skill. If you change the name, be sure to use a unique name.

1.  In the conditional responses section, two response conditions are added automatically, one response to show when the webhook callout is successful and a return variable is sent back. And one response to show when the callout fails. You can edit these responses, and add more conditional responses to the node.

    - If the callout returns a response, and you know the format of the JSON response, then you can edit the dialog node response to include only the section of the response that you want to share with users. 
    
      For example, the Language Translator service returns an object like this:
    
      ```json
         {
         "translations":[
            {"translation":"¿Cómo estás?"}
         ],
         "word_count":3,
         "character_count":12
         }
      ```
      {: codeblock}
    
      Use a SpEL expression that extracts only the translated text value.

      <table>
      <caption>Conditional responses example</caption>
        <tr>
          <th>Condition</th>
          <th>Response</th>
        </tr>
        <tr>
          <td>$webhook_result_1</td>
          <td>Your words in Spanish: <? $webhook_result_1.translations[0].translation ?>.</td>
        </tr>
        <tr>
          <td>anything_else</td>
          <td>The call to the external application failed. Please try again later.</td>
        </tr>
      </table>

      If you use the recommended format for the response and the translation response shown earlier is returned, the assistant's response to the user would be: `Your words in Spanish: ¿Cómo estás?`

    - If you want to provide a specific response if the callout returns an empty string, meaning the call is successful, but the value returned is an empty string, you can add a conditional response that has a condition with syntax like this:

      ```
      $webhook_result_1.size() == 0
      ```
      {: codeblock}

1.  When you are done, click the X to close the node. Your changes are automatically saved.

## Testing webhooks
{: #dialog-webhooks-test}

When you first add a webhook callout, it can be useful to see exactly what is returned in the response from the external application, the data and its format. To do this, add this expression as the text response for the successful callout conditional response: `$webhook_result_n` where `n` is the appropriate number for the webhook you are testing.

This response returns the full body of the return variable, so you can see what the callout is sending back and decide what to share with the user. You can then use the methods documented in [Expression language methods](/docs/assistant?topic=assistant-dialog-methods) to extract only the information you care about from the response.

Test whether certain user inputs can generate errors in the callout, and build in ways to handle those situations. Errors generated by the external application are stored in `output.webhook_error.<result_variable>`. You can use a conditional response like this while you are testing to capture such errors:

| Condition | Response |
|-----------|----------|
| output.webhook_error | The callout generated this error: <? output.webhook_error.webhook_result_1 ?>. |

For example, you might not be authenticating the request properly (401), or you might be trying to pass a parameter with a name that is already in use by the external application. Test the webhook to discover and fix these types of errors before you deploy the webhook.

## Removing a webhook
{: #dialog-webhooks-delete}

If you decide you do not want to make a webhook call from a dialog node, open the node's *Customize* page, and then switch Webhooks **Off**.

The *Parameters* section and the **Return variable** field are removed from the dialog node editor. However, any conditional responses that were added for you or that you added yourself remain.

The *Multiple conditioned responses* section is editable again. You can choose to switch the feature off. If you do so, then only the first conditional response is saved as the node's only text response.

To change the external service that you call from dialog nodes, edit the webhook details defined on the Webhooks page of the **Options** tab. If the new service expects different parameters to be passed to it, be sure to update any dialog nodes that call it.

## Calling an IBM Cloud Functions web action
{: #dialog-webhooks-cf-web-action}

Typically, web actions can be run without requiring the caller to authenticate first. However, you can secure a web action that requires any callers to pass an ID with the request. For more information about how to secure a web action, see [Securing web actions](/docs/openwhisk?topic=openwhisk-actions_web#actions_web_secure){: external}.

The following tips will help you call a {{site.data.keyword.openwhisk_short}} web action from your dialog. 

1.  Open the **Options** page for the skill, and then click **Webhooks**.

1.  In the **URL** field, add the URL for the external application to which you want to send HTTP POST request callouts.

    For example, to call a {{site.data.keyword.openwhisk_short}} web action, specify the URL for the public web action. For example:

    ```bash
    https://us-south.functions.cloud.ibm.com/api/v1/web/my_org_dev/default/Hello%20World.json
    ```
    {: codeblock}

    If the external application that you call returns a response, it must be able to send back a response in JSON format.

    Notice the request URL in this example ends in `.json`. By specifying this extension, you take advantage of a feature of web actions that lets you specify the desired content type of the response. Specifying this extension type ensures that, if the web actions can return responses in more than one format, a JSON response will be returned. See [Extra features](/docs/openwhisk?topic=openwhisk-actions_web#actions_web_extra){: external} for more details.
    {: tip}

1.  If the web action is secured, specify any headers, such as `X-Require-Whisk-Auth`, that are required to call the web action.

    <table>
    <caption>Header example</caption>
      <tr>
      <th>Header name</th>
      <th>Header value</th>
      </tr>
      <tr>
      <td>`X-Require-Whisk-Auth`</td>
      <td>`{my-secret}`</td>
      </tr>
    </table>

1.  Click the **Dialog** tab.

1.  Click to open the dialog node from which you want to call the web action, and then click **Customize**.

1.  Scroll down to the webhook section. Set the **Callout to webhooks** switch to **On**, and then click **Apply**.

1.  Add any data that you want to pass to the external application as key and value pairs in the *Parameters* section.

    For example, if you call the Hello World {{site.data.keyword.openwhisk_short}} web action, you might want to add the following information to pass in the message parameter that is accepted by that application:

    <table>
    <caption>Parameter example</caption>
      <tr>
        <th>Key</th>
        <th>Value</th>
      </tr>
      <tr>
        <td>message</td>
        <td>"hello"</td>
      </tr>
    </table>

    When calling a {{site.data.keyword.openwhisk_short}} web action, you cannot pass parameters with the same key as parameters that are defined as part of the web action. See [Protected parameters](/docs/openwhisk?topic=openwhisk-actions_web#actions_web_protect_parameters){: external} for more details.
    {: note}

1.  You can edit the dialog node response to include only the section of the response that you want to display to users. 

    The Hello World {{site.data.keyword.openwhisk_short}} web action includes a message name and value pair in its response, along with other information. To show only the content of the message, you can use `<return-variable>.message` syntax to extract the message section only from the response object.

    <table>
    <caption>Conditional responses example</caption>
      <tr>
        <th>Condition</th>
        <th>Response</th>
      </tr>
      <tr>
        <td>$webhook_result_1</td>
        <td>The application returned "$webhook_result_1.message".</td>
      </tr>
      <tr>
        <td>anything_else</td>
        <td>The call to the external application failed. Please try again later.</td>
      </tr>
    </table>

1.  When you are done, click the X to close the node. Your changes are automatically saved.

## Updating output.generic with a webhook
{: #dialog-webhooks-outputgeneric}

You can use a webhook to update `output.generic` and provide dynamic responses. The blog article [How to Dynamically Add Response Options to Dialog Nodes in Watson Assistant](https://medium.com/ibm-data-ai/how-to-dynamically-add-response-options-to-dialog-nodes-in-watson-assistant-e14c5e08beca) shows you how.
