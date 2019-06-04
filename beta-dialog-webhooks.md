---

copyright:
  years: 2015, 2019
lastupdated: "2019-04-03"

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

Define a webhook that sends a POST request callout to an external application to perform a programmatic function. You can then invoke the webhook from one or more dialog nodes.

A webhook is a mechanism that allows you to call out to an external program based on something happening in your program. When used in a dialog skill, a webhook is triggered when the assistant processes a node that has a webhook enabled. The webhook collects data that you specify or that you collect from the user during the conversation and save in context variables, and sends the data to the Webhook request URL as an HTTP POST request. The URL that receives the webhook is the listener. It performs a predefined action using the information that is provided by the webhook as specified in the webhook definition, and can optionally return a response.

You can use a webhook to do the following types of things:

- Validate information that you collected from the user.
- Do calculations or string manipulations on user input that are too complex for supported SpEL expression methods to handle.
- Interact with an external web service to get information. For example, you might check on the expected arrival time for a flight from an air traffic service or get a forecast from a weather service.
- Send requests to an external application, such as a restaurant reservation site, to complete a simple transaction on the user's behalf.
- Trigger a SMS notification
- Trigger a {{site.data.keyword.openwhisk_short}} action or web action.

## Defining the webhook
{: #dialog-webhooks-create}

You can define one webhook for a dialog skill, and then call the webhook from one or more dialog nodes.

The programmatic call to the external service must meet these requirements:

- The call must be a POST HTTP request.
- The format of the request and response must be in JSON.
- Only make a call that you know can return in **under 5 seconds**.

  For less efficient services that you need to call, manage the call through your client application and pass the information to the dialog as a separate step.

To add the webhook details, complete the following steps:

1.  From the skill where you want to add the webhook, click the **Options** tab.

1.  Click **Webhooks**.

1.  In the **Request URL** field, add the URL for the external application to which you want to send HTTP POST request callouts.

    For example, to call a {{site.data.keyword.openwhisk_short}} web action, specify the URL for the public web action. For example:

    ```bash
    https://us-south.functions.cloud.ibm.com/api/v1/web/my_org_dev/default/Hello%20World.json
    ```
    {: codeblock}

    If the external application that you call returns a response, it must be able to send back a response in JSON format.

    Notice the request URL in this example ends in `.json`. By specifying this extension, you take advantage of a feature of web actions that lets you specify the desired content type of the response. Specifying this extension type ensures that, if the web actions can return responses in more than one format, a JSON response will be returned. See [Extra features](/docs/openwhisk?topic=cloud-functions-actions_web#actions_web_extra){: new_window} for more details.
    {: tip}

1.  If the external service requires that you pass authentication credentials or other parameters with the POST request, then add them as headers.

    ({{site.data.keyword.openwhisk_short}} web actions typically do not require that you pass an API key.)

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

Your webhook details are saved automatically.

## Adding a webhook callout to a dialog node
{: #dialog-webhooks-dialog-node-callout}

To use a webhook from a dialog node, you must enable webhooks on the node, and then add details for the callout.

1.  Click the **Dialog** tab.

1.  Find the dialog node where you want to add a callout, click to open the dialog node, and then click **Customize**.

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

    If you are calling a {{site.data.keyword.openwhisk_short}} web action, you cannot pass parameters with the same key as parameters that are defined as part of the web action. See [Protected parameters](/docs/openwhisk?topic=cloud-functions-actions_web#actions_web_protect){: new_window} for more details.
    {: note}

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

1.  Any response made by the callout is saved to the return variable. You can rename the variable that is automatically added to the **Return variable** field for you.

    The generated variable name has the syntax `webhook_result_n`, where the appended `_n` is incremented each time you add a webhook callout to a dialog node to ensure that context variable names are unique across the dialog skill. If you change the name, be sure to use a unique name.

1.  In the conditional responses section, two response conditions are added automatically, one response to show when the webhook callout is successful and a return variable is sent back. And one response to show when the callout fails. You can edit these responses, and add more conditional responses to the node.

    If the callout returns a response, and you know the format of the JSON response, then you can edit the dialog node response to include only the section of the response that you want to share with users. For example, the Hello World {{site.data.keyword.openwhisk_short}} web action includes a message name and value pair in its response, along with other information. To show only the content of the message, you can use `<response>.message` syntax to extract the message section only from the response object.

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

## Testing webhooks
{: #dialog-webhooks-test}

When you first add a webhook callout, it can be useful to see exactly what is returned in the response from the external application, the data and its format. To do this, add this expression as the text response for the successful callout conditional response: `$webhook_result_n` where `n` is the appropriate number for the webhook you are testing.

This response returns the full body of the return variable, so you can see what the callout is sending back and decide what to share with the user. You can then use the methods documented in [Expression language methods](/docs/services/assistant?topic=assistant-dialog-methods) to extract only the information you care about from the response.

Test whether certain user inputs can generate errors in the callout, and build in ways to handle those situations. Errors generated by the external application are stored in `output.webhook_error.<result_variable>`. You can use a conditional response like this while you are testing to capture such errors:

| Condition | Response |
|-----------|----------|
| output.webhook_error | The callout generated this error: <? output.webhook_error.webhook_result_1 ?>. |

For example, you might not be authenticating the request properly (401), or you might be trying to pass a parameter with a name that is already in use by the external application. Test the webhook to discover and fix these types of errors before you deploy the webhook.

## Defining a {{site.data.keyword.openwhisk_short}} action webhook
{: #dialog-webhooks-cf-action}

Typically, you use a webhook to make a standard POST request to a REST HTTP endpoint. However, if you want to use a webhook to call a {{site.data.keyword.openwhisk_short}} action, and not a {{site.data.keyword.openwhisk_short}} web action, then you can do so.

You must make a synchronous call to the {{site.data.keyword.openwhisk_short}} action. The webhook implementation does not support asynchronous calls to {{site.data.keyword.openwhisk_short}} actions because when you send an asynchronous request, an activation ID is returned first. You must send a `GET` request with this activition ID to trigger the action and get data back. However, the webhook implementation supports sending `POST` requests only.

To make a synchronous call to a {{site.data.keyword.openwhisk_short}} action, complete the following steps:

1.  From the skill where you want to add the webhook, click the **Options** tab.

1.  Click **Webhooks**.

1.  In the **Request URL** field, specify the URL for the action. You must append a `?blocking=true` parameter to the action URL to force a synchronous call to be made.

    This syntax sends a synchronous request:

    ```bash
    https://us-south.functions.cloud.ibm.com/api/v1/namespaces/my_org_dev/actions/Hello%20World?blocking=true
    ```
    {: codeblock}

    When you call an action, you must provide the API Key that is associated with the action as a header, which is described in the next step.

1.  If the external service requires that you pass authentication credentials or other parameters with the POST request, then add them as headers.

    To add an authorization header for the request, complete these steps:

    1.  Get the API Key. From the {{site.data.keyword.openwhisk_short}} Endpoint page, find the CURL section and click the eye icon to show the key details. Copy the `user ID:password` key that is listed after the `-u` parameter in the curl command.
    1.  Convert the `user ID:password` key into a base64-encoded String. You can use a basic authentication header generator tool to encode it, for example.
    1.  Pass the encoded key as an Authorization header.

    ![Shows the Request URL field and Headers section of the Options page.](images/webhook-to-cfaction.png)

    Your webhook details are saved automatically.

1.  Follow the [same steps](#dialog-webhooks-dialog-node-callout) to add a webhook callout from a dialog node.

    When you call a {{site.data.keyword.openwhisk_short}} action, you *can* pass parameters with the same key as parameters that are defined as part of the action. The parameters are not reserved like they are for web actions.

    You can edit the conditional response that is shown when the webhook callout is successful to limit what is shown to users. Use an expression with the syntax `$webhook_result.response.result.message` to extract the returned message only.

    <table>
    <caption>Conditional responses example</caption>
      <tr>
        <th>Condition</th>
        <th>Response</th>
      </tr>
      <tr>
        <td>$webhook_result_1</td>
        <td>The application returned "$webhook_result.response.result.message".</td>
      </tr>
      <tr>
        <td>anything_else</td>
        <td>The call to the external application failed. Please try again later.</td>
      </tr>
    </table>

## Removing a webhook
{: #dialog-webhooks-delete}

If you decide you do not want to make a webhook call from a dialog node, open the node's *Customize* page, and then switch Webhooks **Off**.

The *Parameters* section and the **Return variable** field are removed from the dialog node editor. However, any conditional responses that were added for you or that you added yourself remain.

The *Multiple conditioned responses* section is editable again. You can choose to switch the feature off. If you do so, then only the first conditional response is saved as the node's only text response.

To change the external service that you call from dialog nodes, edit the webhook details defined on the Webhook page of the **Options** tab. If the new service expects different parameters to be passed to it, be sure to update any dialog nodes that call it.
