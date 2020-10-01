---

copyright:
  years: 2015, 2020
lastupdated: "2020-10-01"

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

# Integrating with *Twilio messaging* ![Beta](images/beta.png)
{: #deploy-sms}

Add a text messaging integration so your assistant can exchange messages with your customers.
{: shortdesc}

The Short Messaging Service (SMS) supports text-only messages. Typically, SMS restricts the text message length to 160 characters. The Multimedia Messaging Service (MMS) supports sending images and text messages that are over 160 characters in length. When you create a phone number with Twilio, MMS message support is included automatically.

Customers send text messages to your Twilio-hosted phone number. Twilio uses a messaging webhook that you set up to send a POST request with the text message body to your assistant. Each response from the assistant is sent back to Twilio to be converted to an outbound SMS message that is sent to the customer. The responses are sent to the Twilio API for processing. You provide your Twilio account SID and project authentication token information, which serve as your Twilio API access credentials.

This integration type is available as a beta feature in the Dallas, Frankfurt, and Washington, DC locations.
{: note}

## Before you begin
{: #deploy-sms-service-setup}

- You must have Manager service level access to the instance. For more information about access levels, see [Managing access to resources](/docs/assistant?topic=assistant-access-control).

If you don't have a text messaging phone number, set up a Twilio messaging account and get a phone number.

1.  Go to the [Twilio website](https://www.twilio.com/){: external}.
1.  Create an account or start a free trial.
1.  From the *All products and Services* menu, click *Phone numbers*.
1.  Follow the instructions to get a phone number.

    When you get a Twilio phone number, it supports voice, SMS, and MMS automatically. Your new phone number is listed as an active number.

Keep the Twilio web page open in a web browser tab so you can refer to it again later.
{: tip}

## Set up the integration
{: #deploy-sms-setup}

1.  From the Assistants page, click to open the assistant tile that you want to deploy.

1.  From the Integrations section, click **Add integration**.

1.  Click **Twilio messaging**.

1.  Click **Create**.

1.  Add the messaging phone number that you created previously to the **Phone number** field.

    Specify the number by using the international phone number format: `+1 958 555 0123`. Do *not* include parentheses (`(958)`).

    The phone number must be unique per phone integration. If you use Twilio as the SIP trunk provider, you can use the same phone number for your phone and text messaging integrations.

    If you get a *Forbidden* message, it means the phone number cannot be verified. Make sure the number fully matches the SMS phone number.

1.  Scroll to the *Setup instructions* section, and then copy the value from the **Webhook URI (uniform resource identifier)** field.

1.  Go to your Twilio account web page. From the *All products and Services* menu, click *Phone Numbers*. 

1.  From the *Active Numbers* page, click your phone number. In the *Messaging* section, find the *Webhook* field that defines what to do when *a message comes in*. Paste the value that you copied from the *Webhook URI* field into it.

1.  From the Twilio site, click the home icon to go to your project dashboard. 

    Copy the following values and store them temporarily, so you can paste them into the phone integration setup page in the next step.

    - Account SID
    - Auth token

1.  Return to the phone integration setup page. 

    Paste the values that you copied in the previous step into the fields with the corresponding names in the *Twilio account information* section.

    - **Account SID**
    - **Auth token**

1.  Click **Save and exit**.

If you want your assistant to be able to switch between voice and text during a customer interaction, enable both the phone and text messaging integrations. The integrations do not need to use the same third-party service provider. For more information, see [Integrating with phone](/docs/assistant?topic=assistant-deploy-phone).

## Advanced configuration options
{: #deploy-sms-advanced}

Click the *Advanced options* tab to make any of the following customizations to the messaging behavior:

- **Initiate conversation from inbound messages**: Disable this option if you want to limit messaging support to allow messages that are sent in the context of an ongoing phone integration conversation only, and not allow customers to start a message exchange with the assistant outside of a phone call.
- **Default failure message**: Add a message to send to the customer if the SMS connection fails.
- **Base URL**: This URL is the REST API endpoint for the SMS service you are using.

## Optimize your dialog for messaging
{: #deploy-sms-dialog}

For the best customer experience, design your dialog with the capabilities of the Twilio integration in mind:

- Do not include HTML elements in your text responses.
- You can include search skill response types in dialog nodes that the phone integration will send as a message. The message includes the introductory text (*I searched my knowledge base* and so on), and then the body of only the first search result.

For reference documentation, see [Handling Twilio messaging interactions](/docs/assistant?topic=assistant-dialog-sms-actions).
