---

copyright:
  years: 2019, 2021
lastupdated: "2021-01-15"

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

# Logging messages with a webhook ![Premium only](images/premium0.png)
{: #webhook-log}

Make a call to an external service or application every time a customer submits input to log the conversation.
{: shortdesc}

The log webhook is available for users of Premium plans only.
{: note}

A webhook is a mechanism that allows you to call out to an external program based on events in your program. Add a log webhook to your assistant if you want to log each incoming message and its corresponding response.

The log webhook works with the v2 `/message` API only (stateless and stateful). For more information, see the [API reference](https://cloud.ibm.com/apidocs/assistant/assistant-v2#message).
{: important}

The log webhook does not return anything. You can use it as an alternative to the built-in analytics feature to handle logging yourself. For more information about the built-in analytics support, see [Metrics overview](/docs/assistant?topic=assistant-logs-overview).

![Plus or Premium plan only](images/plus.png) For environments where private endpoints are in use, keep in mind that a webhook sends traffic over the internet. For more information, see [Private network endpoints](/docs/assistant?topic=assistant-security#security-private-endpoints).
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

1.  In the **URL** field, add the URL for the external application to which you want to send HTTP POST request callouts.

    For example, to write the message to the 

    ```bash
    https://www.mycompany.com/my_log_service
    ```
    {: codeblock}

    You must specify a URL that uses the SSL protocol, so specify a URL that begins with `https`.

1.  In the **Secret** field, add a token to pass with the request that can be used to authenticate with the external service.

    The secret must be specified as a text string, such as `purpleunicorn`. It cannot be a context variable.

    It is the responsibility of the external service to check for and verify the secret. If the external service does not require a secret, you can leave this field empty.

1.  In the Headers section, add any headers that you want to pass to the service one at a time by clicking **Add header**.

Your webhook details are saved automatically.

## Webhook security
{: #webhook-log-security}

To authenticate the webhook request, verify the JSON Web Token (JWT) that is sent with the request. The webhook microservice automatically generates a JWT and sends it in the `Authorization` header with each webhook call. It is your responsibility to add code to the external service that verifies the JWT.

For example, if you specify `purpleunicorn` in the **Secret** field, you might add code similar to this:

```javascript
token = request.headers.authentication
jwt.verify(token, 'purpleunicorn')
```
{: codeblock}

## Removing the webhook
{: #webhook-log-delete}

If you decide you do not want to log messages programmatically, complete the following steps:

1.  From the assistant overview page, click the ![Overflow menu](images/kebab.png) icon, and then choose **Settings**.

1.  Click **Webhooks > Log webhook**.

1.  Set the *Log webhook* switch to **Disabled**.
