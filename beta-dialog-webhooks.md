---

copyright:
  years: 2015, 2019
lastupdated: "2019-03-15"

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

# Beta: Adding a webhook callout to a dialog node
{: #dialog-webhooks}

This feature is available for use by participants in the beta program only. To find out how to request access, see [Participate in the beta program](/docs/services/assistant?topic=assistant-feedback#feedback-beta).

![Beta](images/beta.png) IBM releases services, features, and language support for your evaluation that are classified as beta. These features might be unstable, might change frequently, and might be discontinued with short notice. Beta features also might not provide the same level of performance or compatibility that generally available features provide and are not intended for use in a production environment.

Define a webhook that sends a POST request to an external application to perform a programmatic function. You can then invoke the webhook from one or more dialog nodes.

A webhook is basically a user-defined HTTP callback or a code snippet that is linked to a web application. When a webhook is triggered by an event, it collects data, and sends it to the URL of the external application as an HTTP request that asks for a programmatic action to occur.

When used in your dialog skill, the webhook is triggered when a node that calls a webhook is processed. The webhook collects data that you specify or that you collect from the user during the conversation and save in context variables, and sends the data to the Webhook request URL as an HTTP POST request. The URL that receives the webhook is the listener. It performs a predefined action using any information that is provided by the webhook, and can optionally return a response.

You can use an external service to do the following types of things:

- Validate information that you collected from the user.
- Do calculations or string manipulations on user input that are too complex for supported SpEL expression methods to handle.
- Interact with an external web service to get information. For example, you might check on the expected arrival time for a flight from an air traffic service or get a forecast from a weather service.
- Send requests to an external application, such as a restaurant reservation site, to complete a simple transaction on the user's behalf.
- Trigger a SMS notification
- Trigger a {{site.data.keyword.openwhisk_short}} action or web action.

You can define one webhook for a dialog skill, and then call the webhook from one or more dialog nodes.

**Time limits**: Only make a call that you know can return in **under 5 seconds**. If your dialog makes more than one webhook call, the total amount of time allowed for the calls to complete is 7 seconds. If the first three calls complete in 2 seconds each, and the fourth takes more than 1 second, then the fourth call is stopped, and the error message for the call indicates that the call was not completed. For less efficient services that you need to call, manage the call through your client application and pass the information to the dialog as a separate step.

## Defining the webhook
{: #dialog-webhooks-create}

Before you can invoke a webhook, you must provide details about the external service that you want to call. To add the webhook details, complete the following steps:

1.  From the skill where you want to add the webhook, click the **Options** tab.

1.  Click **Webhooks**.

1.  In the **Request URL** field, add the URL for the external application to which you want to send HTTP POST requests.

    **Web action**

    For example, to call a {{site.data.keyword.openwhisk_short}} web action, specify the URL for the public web action. For example:

    ```bash
    https://us-south.functions.cloud.ibm.com/api/v1/web/my_org_dev/default/Hello%20World.json
    ```
    {: codeblock}

    **Synchronous action**

    To call a {{site.data.keyword.openwhisk_short}} action (not a web action), specify the URL for the action. You must append a `?blocking=true` parameter to the action URL to force a synchronous call to be made.

    This syntax sends a synchronous request:

    ```bash
    https://us-south.functions.cloud.ibm.com/api/v1/namespaces/my_org_dev/actions/Hello%20World?blocking=true
    ```
    {: codeblock}

    When you call an action, you must provide the API Key that is associated with the action as a header, which is described in the next step.

    The webhook implementation does not support asynchronous calls to {{site.data.keyword.openwhisk_short}} actions because when you send an asynchronous request, an activation ID is returned first. You must send a `GET` request with this activition ID to trigger the action and potentially get data back. However, the webhook implementation supports sending `POST` requests only.
    {: note}

1.  If the external service requires that you pass authentication credentials or other parameters with the POST request, then add them as headers.

    For example, if you call a {{site.data.keyword.openwhisk_short}} action, then you must add an authorization header for the request.

    1.  Get the API Key. From the {{site.data.keyword.openwhisk_short}} Endpoint page, find the CURL section and click the eye icon to show the key details. Copy the `user ID:password` key that is listed after the `-u` parameter in the curl command.
    1.  Convert the `user ID:password` key into a base64-encoded String. You can use a basic authentication header generator tool to encode it, for example.
    1.  Pass the encoded key as an Authorization header by using the following syntax:

    <table>
    <caption>Header example</caption>
      <tr>
        <th>Header name</th>
        <th>Header value</th>
      </tr>
      <tr>
        <td>Authorization</td>
        <td>Basic `<encoded-api-key>`</td>
      </tr>
    </table>

    ![Shows the Request URL field and Headers section of the Options page.](images/webhook-to-cfaction.png)

## Adding a webhook callout to a dialog node
{: #dialog-webhooks-dialog-node-callout}

To send a callout to a Webhook from a dialog node, you must enable Webhooks on the node, and then add details for the callout.

1.  Click the **Dialog** tab.

1.  Find the dialog node where you want to add a callout, click to open it, and then click **Customize**.

1.  Scroll down to the *Webhooks* section, and switch the toggle to **On**, and then click **Apply**.

    If you did not have it enabled already, the *Multiple conditional responses* setting is switched on automatically and you cannot disable it. This setting is enabled to support adding different responses depending on the success or failure of the Webhook call. If you had a response specified for the node already, it becomes the response for the first conditional response.
    {: note}

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

1.  Any response made by the application that you call is saved to the return variable. You can rename this variable.

    The variable name has the syntax `webhook_result_n`, where the appended `_n` is incremented each time you add a webhook callout to a dialog node to ensure that context variable names are unique across the dialog skill. If you change the name, be sure to use a unique name.

1.  In the conditional responses section, two response conditions are added automatically, one response to show when the webhook callout is successful and a return variable is sent back. And one response to show when the callout fails. You can edit these responses, and add more conditional responses to the node.

    For example, to show the message returned by the Hello World {{site.data.keyword.openwhisk_short}} web action, you might specify the following conditional responses:

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

    If you make a {{site.data.keyword.openwhisk_short}} action call, then the JSON object that is returned contains lots of information that you don't need to share with the user. You can use this expression in the response text to access only the message from the response: `$webhook_result.response.result.message`.

1.  When you are done, click the X to close the node. Your changes are automatically saved.

## Testing webhooks
{: #dialog-webhooks-test}

When you first add a callout, it can be useful to see exactly what is returned in the response from the external application, the data and its format. Add this expression as the text response for the successful callout conditional response: `$webhook_result_n` where `n` is the appropriate number for the webhook you are testing.

This response returns the full context variable, so you can see if the external application is sending back a JSON Object or a JSON Array that includes information you don't need to share with the user. You can then use the methods documented in [Expression language methods](/docs/services/assistant?topic=assistant-dialog-methods) to extract only the information you care about from the response.

Be sure to test whether certain user inputs can generate errors in the external application, and build in ways to handle those situations. Any errors generated by the external application are stored in `output.webhook_error.<result_variable>`. You can use a conditional response like this while you are testing to capture such errors in the external application:

| Condition | Response |
|-----------|----------|
| output.webhook_error | The external application generated this error: <? output.webhook_error.webhook_result_1 ?>. |

For example, you might not be authenticating the request properly (401), or you might be trying to pass a parameter with a name that is already in use by the external application. You can discover and fix these types of errors before you deploy the webhook.

## Removing a webhook
{: #dialog-webhooks-delete}

If you decide you do not want to make a webhook call from a dialog node, open the node's Customize page, and then switch Webhooks **Off**.

The *Parameters* section and the **Return variable** field are removed from the dialog node editor. However, any conditional responses that were added for you or that you added yourself remain.

The *Multiple conditioned responses* section is editable again. You can choose to switch the feature off. If you do so, then only the first conditional response is saved as the node's only text response.

To change the external service that you call from dialog nodes, edit the webhook details defined on the Webhook page of the **Options** tab. If the new service expects different parameters to be passed to it, be sure to update any dialog nodes that call it.