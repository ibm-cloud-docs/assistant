---

copyright:
  years: 2015, 2020
lastupdated: "2020-11-13"

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

# Integrating with WhatsApp ![Beta](images/beta.png)
{: #deploy-whatsapp}

Integrate with Whatsapp messaging so your assistant can exchange messages with your customers where they are.
{: shortdesc}

Many customers use WhatsApp because it provides fast, simple, secure messaging for free, and is available on phones all over the world. WhatsApp uses the phone Internet connection to send messages so customers can avoid SMS fees.

This integration creates a connection between your assistant and WhatsApp by using Twilio as a provider.

This integration is available as a beta feature.
{: note}

## Before you begin
{: #deploy-whatsapp-twilio-setup}

If you don't have one, set up a Twilio messaging account and get a phone number.

1.  Go to the [Twilio website](https://www.twilio.com/){: external}.
1.  Create an account.
1.  From the *All Products and Services* menu ![Twilio All products and services icon](images/twilio-products.png), click *Phone numbers*.
1.  Follow the instructions to get a phone number.

    When you get a Twilio phone number, it supports voice, SMS, and MMS automatically. Your new phone number is listed as an active number.

## Ask WhatsApp for permission to enable your Twilio number for WhatsApp.
{: #deploy-whatsapp-prereqs}

WhatsApp has a rigorous process that they use to review all businesses that want to interact with customers over their network. WhatsApp, which is owned by Facebook, requires that you register your business with the Facebook business directory.

1.  To register, go to the [Facebook Business Manager](https://business.facebook.com/overview) website, and click *Create account* and follow the instructions to create an account.

1.  Make a note of your Facebook Business Manager ID. You will need this in the next step.

1.  Submit the *Request to enable your Twilio numbers for WhatsApp* form from the [Twilio API for WhatsApp](https://www.twilio.com/whatsapp/request-access) web page.

    Tips for specifying the following values:

    - *Phone Number*: Specify the Twilio phone number that you created earlier. 
    
      Consider provisioning more than one phone number and going through the process of getting permission for the numbers in parallel. If your number was used by a different business previously (because Twilio assigned you a number that was used before, for example), WhatsApp will reject it.
      {: tip}
    - *Are you working with an ISV*: No
    - *Twilio Account SID*: From the Twilio site, click the home icon to go to your project dashboard to find the SID.
    - *Facebook Business Manager ID*: Add the ID for the account that you created in the previous step.

1.  Click *Request Now*.

Give WhatsApp time to evaluate and approve your request. It can take up to 7 days for your request to be approved.

## Set up the integration
{: #deploy-whatsapp-setup}

To set up the integration, complete the following steps:

1.  From the Assistants page, click to open the assistant tile that you want to deploy.

1.  From the Integrations section, click **Add integration**.

1.  Click **WhatsApp with Twilio**.

1.  Click **Create**.

1.  From the Twilio site, click the home icon to go to your project dashboard. 

    Copy the following values and store them temporarily, so you can paste them into the phone integration setup page in the next step.

    - Account SID
    - Auth token

1.  Return to the WhatsApp integration setup page. 

    Paste the values that you copied in the previous step into the fields with the corresponding names in the *Twilio account information* section.

    - **Account SID**
    - **Auth token**

1.  Click **Sync account**.

1.  Add the Twilio messaging phone number that you created previously to the **Company phone number** field.

    Specify the number by using the international phone number format: `+1 958 555 0123`. Do *not* include parentheses (`(958)`).

    The phone number must be unique per WhatsApp integration.

1.  Copy the value from the **WhatsApp Webhook** field.

1.  Click **Save and exit**.

## Testing the integration
{: #deploy-whatsapp-sandbox}

While you wait for WhatsApp to approve your request, you can test the integration by using the Twilio sandbox. With the sandbox, you can send and receive preapproved template messages to numbers that join your sandbox, using a shared Twilio test number.

Do not use the Twilio sandbox in production. Sandbox sessions expire after 3 days.

1.  To create a sandbox, go to the [Twilio Try WhatsApp](https://www.twilio.com/console/sms/whatsapp/learn) web page. An *Activate your sandbox* prompt is displayed. Agree to have a sandbox created, and confirm your choice.

1.  Follow the instructions to create the sandbox.

1.  Connect to the sandbox by sending a WhatsApp message from your device to the Sandbox phone number.

1.  From the *Programmable Messaging* menu, expand *Settings*, and then click *WhatsApp Sandbox Settings*. 

1.  In the *Sandbox Configuration* section, paste the webhook URI that you copied earlier into the *when a message comes in* field, and then save the configuration.

1.  You can test the integration by sending a message from WhatsApp to the shared phone number that is assigned to your Twilio sandbox.

## Finish the product integration
{: #deploy-whatsapp-finish}

After WhatsApp grants permission for your Twilio phone number or number to access the WhatsApp network, update the integration to use your dedicated Twilio phone number instead of the sandbox number. 

1.  From the *WhatsApp with Twilio* integration setup page, scroll to the *Setup instructions* section, and then copy the value from the **Webhook URI (uniform resource identifier)** field.

1.  Go to your Twilio account web page and add the webhook you copied to the Twilio configuration to complete the connection to the WhatsApp integration in Twilio.

## Give your customers fast access to your assistant
{: #deploy-whatsapp-click-to-chat}

You can add an icon to your web page that customers can click to start a conversation over WhatsApp with your assistant.

To add an icon to your web page, complete the following steps:

1.  From the *WhatsApp with Twilio* integration setup page, click the **Click to chat** tab.

1.  In the **Prefilled message** field, add text that you want WhatsApp to send to your assistant on the customer's behalf to get the conversation started.

    Specify a message that you know your assistant can answer in a useful way. 

1.  Copy the embed link and add it to your web page. Consider adding text in front of the icon that explains what the icon does. For example, you might add a `<span>` HTML tag in front of the icon's `<span>` element that says `Have a question? Ask Watson Assistant for help`.

    When a user clicks the icon in your web page, it opens a WhatsApp messaging session that is connected to your assistant, and adds the text you specify into the user's text field, ready to be submitted.

## Dialog considerations
{: #deploy-whatsapp-dialog}

For the best customer experience, design your dialog with the capabilities of the WhatsApp integration in mind:

- A text response that contains more than 1,600 characters is broken up into multiple responses.
- Do not include HTML elements in your text responses.
- The WhatsApp with Twilio integration does not support chat transfers that are initiated with the *Connect to human agent* response type.
- If you use Markdown syntax, see the *Supported Markdown syntax* table.
- To include a hypertext link in a text response, specify the url directly. Do not use markdown syntax for links. For example, specify `Contact us at https://www.ibm.com.`

| Format | Syntax | Example |
|------------|--------|---------|
| Italics | `We're talking about _practice_.` | We're talking about *practice*. |
| Bold | `There's *no* crying in baseball.` | There's **no** crying in baseball. |
{: caption="Supported Markdown syntax" caption-side="top"}

If you want to use the same dialog for an assistant that you deploy to many different platforms, add custom responses per integration type. You can add a conditioned response that tells the assistant to show the response only when the *WhatsApp with Twilio* integration is being used. For more information, see [Building integration-specific responses](/docs/assistant?topic=assistant-dialog-integrations#dialog-integrations-condition-by-type).
