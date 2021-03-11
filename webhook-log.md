---

copyright:
  years: 2019, 2021
lastupdated: "2021-03-11"

keywords: log webhook

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

# Logging messages with a webhook ![Enterprise plan only](images/enterprise.png)
{: #webhook-log}

Make a call to an external service or application every time a customer submits input to log the conversation.
{: shortdesc}

A webhook is a mechanism that allows you to call out to an external program based on events in your program. 

Add a log webhook to your assistant if you want to log each incoming message and its corresponding response. The log webhook is triggered after the `/message` response is returned.

This feature is available only to Enterprise plan users. You can log messages that are exchanged in conversations that are defined by a dialog skill only.
{: important}

The log webhook works only with the v2 `/message` API (stateless and stateful). For more information, see the [API reference](https://cloud.ibm.com/apidocs/assistant/assistant-v2#message){: external}.

The log webhook does not return anything to your assistant. You can use the log webhook method as an alternative to the built-in analytics feature to handle logging yourself. For more information about the built-in analytics support, see [Metrics overview](/docs/assistant?topic=assistant-logs-overview).

For environments where private endpoints are in use, keep in mind that a webhook sends traffic over the internet. For more information, see [Private network endpoints](/docs/assistant?topic=assistant-security#security-private-endpoints).
{: note}

## Defining the webhook
{: #webhook-log-create}

You can define one webhook URL to use for logging every incoming message.

The programmatic call to the external service must meet these requirements:

- The call must be a POST HTTP request.

To add the webhook details, complete the following steps:

1.  From your assistant, click the ![Overflow menu](images/kebab.png) icon, and then choose **Settings**.

1.  Click **Webhooks > Log webhook**.

1.  Set the *Log webhook* switch to **Enabled**.

    If you cannot enable the webhook, you might need to upgrade your service plan.

1.  In the **URL** field, add the URL for the external application to which you want to send HTTP POST request callouts.

    For example, to write the message to the 

    ```bash
    https://www.mycompany.com/my_log_service
    ```
    {: codeblock}

    You must specify a URL that uses the SSL protocol, so specify a URL that begins with `https`.

1.  In the **Secret** field, add a token to pass with the request that can be used to authenticate with the external service.

    The secret must be specified as a text string, such as `purple unicorn`.  Maximum length is 1,024 characters. You cannot specify a context variable.

    It is the responsibility of the external service to check for and verify the secret. If the external service does not require a secret, you can leave this field empty.

    It is the responsibility of the external service to check for and verify the secret. If the external service does not require a token, specify any string you want. You cannot leave this field empty.

1.  In the Headers section, add any headers that you want to pass to the service one at a time by clicking **Add header**.

    The service automatically sends an `Authorization` header with a JWT; you do not need to add one. If you want to handle authorization yourself, add your own authorization header and it will be used instead.

Your webhook details are saved automatically.

## Webhook security
{: #webhook-log-security}

To authenticate the webhook request, verify the JSON Web Token (JWT) that is sent with the request. The webhook microservice automatically generates a JWT and sends it in the `Authorization` header with each webhook call. It is your responsibility to add code to the external service that verifies the JWT.

For example, if you specify `purple unicorn` in the **Secret** field, you might add code similar to this:

```javascript
const jwt = require('jsonwebtoken');
...
const token = request.headers.authentication; // grab the "Authentication" header
try {
  const decoded = jwt.verify(token, 'purple unicorn');
} catch(err) {
  // error thrown if token is invalid
}
```
{: codeblock}

## Example request body
{: #webhook-log-request-body}

It is useful to know the format of the request body so that your external code can process it. 

The payload contains the same information that would be returned by a GET request to the v2 `/logs` API. The event name `message_logged` indicates that the request is generated by the log webhook. For more information about the log payload, see the [API reference](https://cloud.ibm.com/apidocs/assistant/assistant-v2#listlogs).

Any fields not shown in the API reference documentation are subject to change.
{: important}

```
{
  "payload" : { GET /v2/assistants/{id}/logs copy }
  "event": {
      "name": "message_logged"
   }
}
```
{: codeblock}

## Removing the webhook
{: #webhook-log-delete}

If you decide you do not want to log messages programmatically, complete the following steps:

1.  From the assistant overview page, click the ![Overflow menu](images/kebab.png) icon, and then choose **Settings**.

1.  Click **Webhooks > Log webhook**.

1.  Do one of the following things:

    - To change the webhook that you want to call, click **Delete webhook** to delete the currently specified URL and secret. You can then add a new URL and other details.
    - To stop calling a webhook to log every message and response, click the *Log webhook* switch to disable the webhook altogether.
