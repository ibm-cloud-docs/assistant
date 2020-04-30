---

copyright:
  years: 2015, 2020
lastupdated: "2020-04-30"

keywords: integration settings

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
{:table: .aria-labeledby="caption"}

# Adding custom dialog flows for integrations
{: #dialog-integrations}

Use the JSON editor in dialog to access information that is submitted from the Web Chat integration.
{: shortdesc}

The `context` object that is passed as part of the v2 `/message` API request contains an `integrations` object. This object makes it possible to pass information that is specific to a single integration type in the context. For more information about context variables, see [Context variables](/docs/assistant?topic=assistant-dialog-runtime#dialog-runtime-context-variables).

The `integrations` object is available from the v2 API in version `2020-04-01` or later only.
{: important} 

## Web Chat: Accessing sensitive data
{: #dialog-integrations-chat-private}

If you enable security for the Web Chat, you can configure your Web Chat implementation to send encrypted data to the dialog. Payload data that is sent from Web Chat is stored in a private context variable named `context.integrations.chat.private.user_payload`. No private variables are sent from the dialog to any integrations. For more information about how to pass data, see [Passing sensitive data](/docs/assistant?topic=assistant-deploy-web-chat#deploy-web-chat-security-encrypt).

To access the payload data, you can reference the `context.integrations.chat.private.user_payload` object from the dialog node condition. 

You must know the structure of the JSON object that is sent in the payload.
{: note}

For example, if you passed the value `"mvp:true"` in the JSON payload, you can add a dialog flow that checks for this value to define a response that is meant for VIP customers only. Add a dialog node with a condition like this:

| Field | Value |
|-------|-------|
| If assistant recognizes | `$integrations.chat.private.user_payload.mvp` |
| Assistant responds | `I can help you reserve box seats at the upcoming conference!` |
{: caption="Private variable as node condition" caption-side="top"}

## Web Chat: Reusing the JWT token for webhook authentication
{: #dialog-integrations-chat-jwt}

You can use the same JSON Web Token that was used to secure your Web Chat to authenticate webhook calls. If you specify a token in the `identityToken` property when you add the Web Chat to your web page, the token is stored in a private variable named `context.integrations.chat.private.jwt`. For more information about passing a JWT, see [Enable security](/docs/assistant?topic=assistant-deploy-web-chat#deploy-web-chat-security-task).

1.  From your dialog skill, open the **Options>Webhooks** page.
1.  Click **Add authentication**. 
1.  Without adding any values to the fields in the *Basic authorization* modal that is displayed, click **Save**.

    An Authorization header is added for you.
1.  In the **Header value** field, add `$integrations.chat.private.jwt`.
