---

copyright:
  years: 2020, 2021
lastupdated: "2022-04-25"

subcollection: assistant

---

{{site.data.keyword.attribute-definition-list}}

Documentation for the **classic {{site.data.keyword.assistant_classic_short}}** experience has moved. For the most up-to-date version, see [Integrating with SMS](/docs/watson-assistant?topic=watson-assistant-deploy-sms){: external}.
{: attention}

# Integrating with *SMS with Twilio*
{: #deploy-sms}

Add a text messaging integration so your assistant can exchange messages with your customers.
{: shortdesc}

The Short Messaging Service (SMS) supports text-only messages. Typically, SMS restricts the text message length to 160 characters. The Multimedia Messaging Service (MMS) supports sending images and text messages that are over 160 characters in length. When you create a phone number with Twilio, MMS message support is included automatically.

Customers send text messages to your Twilio-hosted phone number. Twilio uses a messaging webhook that you set up to send a POST request with the text message body to your assistant. Each response from the assistant is sent back to Twilio to be converted to an outbound SMS message that is sent to the customer. The responses are sent to the Twilio API for processing. You provide your Twilio account SID and project authentication token information, which serve as your Twilio API access credentials.

This feature is available only to Plus plan users.
{: note}

## Before you begin
{: #deploy-sms-service-setup}

If you don't have a text messaging phone number, set up a SMS with Twilio account and get a phone number.

1.  Go to the [Twilio website](https://www.twilio.com/){: external}.
1.  Create an account or start a free trial.
1.  From the *All Products and Services* menu ![Twilio All products and services icon](images/twilio-products.png), click *Phone numbers*.
1.  Follow the instructions to get a phone number.

    When you get a Twilio phone number, it supports voice, SMS, and MMS automatically. Your new phone number is listed as an active number.

Keep the Twilio web page open in a web browser tab so you can refer to it again later.
{: tip}

### Migrating from Voice Agent with Watson
{: #deploy-sms-migrate-from-va}

If you created an {{site.data.keyword.iva_full}} service instance in IBM Cloud to enable customers to exchange text messages with an assistant, use the *SMS with Twilio* integration instead.

The *SMS with Twilio* integration provides a more seamless integration with your assistant and supports as many Twilio phone numbers as needed. However, the integration currently does not support the following functions:

- Starting an SMS-only interaction with an outgoing text
- Configuring backup locations
- Reviewing the usage summary page. Use IBM Log Analysis instead. For more information, see [Viewing logs](/docs/assistant?topic=assistant-deploy-phone#deploy-phone-logs).

To migrate from {{site.data.keyword.iva_short}} to the {{site.data.keyword.assistant_classic_short}} *SMS with Twilio* integration, complete the following step:

1.  Do one of the following things:

    - If your {{site.data.keyword.iva_short}} service instance uses an SMS service provider other than Twilio, you cannot continue to use it. You must create an SMS account with Twilio first. Complete the [Before you begin](#deploy-sms-service-setup) steps to create the account. Next, set up the integration.
    
    - If your {{site.data.keyword.iva_short}} service instance uses Twilio as its SMS provider, you can go directly to setting up the integration.

## Set up the integration
{: #deploy-sms-setup}

To set up the integration, complete the following steps:

1.  From the Assistants page, click to open the assistant tile that you want to deploy.

1.  From the Integrations section, click **Add integration**.

1.  Click **SMS with Twilio**.

1.  Click **Create**.

1.  From the Twilio site, click the home icon to go to your project dashboard. 

    Copy the following values and store them temporarily, so you can paste them into the *SMS with Twilio* integration setup page in the next step.

    - Account SID
    - Auth token

1.  Return to the *SMS with Twilio* integration setup page. 

    Paste the values that you copied in the previous step into the fields with the corresponding names in the *Twilio account information* section.

    - **Account SID**
    - **Auth token**

1.  Scroll to the *Setup instructions* section, and then copy the value from the **Webhook URI (uniform resource identifier)** field.

    You will add this URI to the webhook configuration in Twilio. If you want to support more than one phone number, you must add the URI to the webhook for each phone number separately.

1.  Go to your Twilio account web page. From the *All Products and Services* menu, click *Phone Numbers*. 

1.  From the *Active Numbers* page, click one of your phone numbers. Scroll to the *Messaging* section, and then find the *Webhook* field that defines what to do when *a message comes in*. 

    Paste the value that you copied from the *Webhook URI* field into it.

1.  If you want to support multiple phone numbers, repeat the previous step for each phone number that you want to use.

1.  Click **Save and exit**.

To watch a video that walks through the setup process, see [Phone and SMS Integration](https://community.ibm.com/community/user/watsonapps/viewdocument/phone-and-sms-integration?CommunityKey=7a3dc5ba-3018-452d-9a43-a49dc6819633&tab=librarydocuments){: external} in the *IBM Watson Apps Community*.

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
- You can send media files or even documents in your response. These files can be intercepted and processed by a configured premessage webhook. For more details, see [Processing input attachments](/docs/assistant?topic=assistant-api-input-attachments).
- The SMS with Twilio integration does not support chat transfers that are initiated with the *Connect to human agent* response type.
- The **pause** response type is ignored. If you want to add a pause, use a [vgwConversationResponseTimeout](/docs/assistant?topic=assistant-commands-sms#commands-sms-context-variables) context variable instead.
- **Image**, **Audio**, **Video** response types allow sending a message containing media. A title and description are sent along with the attachment. Note that depending on the carrier and device of the end user these messages may not be successfully received. For a list of the supported content types, see [Accepted Content Types for Media](https://www.twilio.com/docs/sms/accepted-mime-types){: external}.
- You can include search skill response types in dialog nodes that the phone integration will send as a message. The message includes the introductory text (*I searched my knowledge base* and so on), and then the body of only the first search result.

If you want to use the same dialog for an assistant that you deploy to many different platforms, add custom responses per integration type. You can add a conditioned response that tells the assistant to show the response only when the SMS with Twilio integration is being used. For more information, see [Building integration-specific responses](/docs/assistant?topic=assistant-dialog-integrations#dialog-integrations-condition-by-type).

For reference documentation, see [Handling SMS with Twilio interactions](/docs/assistant?topic=assistant-dialog-sms-actions).

## Troubleshooting
{: #deploy-sms-troubleshoot}

Find solutions to problems that you might encounter while using the integration.

- If you get a *Forbidden* message, it means the phone number that you specified when you configured the integration cannot be verified. Make sure the number fully matches the SMS phone number.
