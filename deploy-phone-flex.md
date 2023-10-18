---

copyright:
  years: 2021
lastupdated: "2022-05-26"

subcollection: assistant

---

{{site.data.keyword.attribute-definition-list}}

Documentation for the **classic {{site.data.keyword.assistant_classic_short}}** experience has moved. For the most up-to-date version, see [Integrating with phone and Twilio Flex](/docs/watson-assistant?topic=watson-assistant-deploy-phone-flex){: external}.
{: attention}

# Integrating with phone and Twilio Flex
{: #deploy-phone-flex}

You can use the phone integration to help your customers over the phone and transfer them to live agents inside of Twilio Flex. If, in the course of a conversation with your assistant, a customer asks to speak to a person, you can transfer the conversation directly to a Twilio Flex agent.

## Before you begin

To use this integration pattern, make sure you have the following:

- {{site.data.keyword.assistant_classic_short}} Plus or Enterprise Plan (required for phone integration)
- A Twilio account with the following products:
    - Twilio Flex
    - Twilio Voice with Programmable Voice API
    - Twilio Studio

## Adding the {{site.data.keyword.assistant_classic_short}} phone integration

If you have not already added the phone integration to your assistant, follow these steps:

1. From the Assistants page, click to open the assistant tile that you want to deploy.

1. From the Integrations section, click **Add integration**.

1. Click **Phone**.

1. Click **Create**.

1. Click **Close**.

For now, this is all you need to do. For more information about configuring the phone integration, see [Integrating with phone](/docs/assistant?topic=assistant-deploy-phone).

## Adding the Twilio Flex Project

If you don't already have a Twilio Flex project, you can create one by following these steps:

1. From the project drop-down menu, click **Create New Project**. Specify a name for the project and verify your account information.

1. On the welcome page, select Flex as the Twilio product for your new project. Fill out the questionnaire and then click **Get Started with Twilio**.

    After your Flex project has been provisioned, return to the [Twilio console](https://www.twilio.com/console){: external}. Make sure you have selected the correct project from the drop-down list.

1. In the navigation menu, click the **All Products & Services** icon.

1. Click **Twilio Programmable Voice > Settings > General**.

1. Under **Enhanced Programmable SIP Features**, toggle the switch to **Enabled**.

## Creating the call flow

After you have your phone integration and Twilio Flex project configured, you must create a call flow with Twilio Studio and provision (or port) the phone number you want your assistant to work with.

To create the call flow:

1. In the navigation menu, click the **All Products & Services** icon.

1. Click **Studio**.

1. Click **+** to create a new flow.

1. Name the new flow and then click **Next**.

1. Select **Start From Scratch** and then click **Next**.

1. At this point you should have a **Trigger** widget at the top of your flow canvas.

1. Click the **Trigger** widget.

1. Make note of the value from the **WEBHOOK URL** field. You will need this value in a subsequent step.


## Configuring the phone number

1. In the navigation menu, click the **All Products & Services** icon.

1. Click **Phone Numbers**.

1. Under **Manage Numbers**, configure the phone number you want your assistant to use. Select **Buy a Number** to buy a new number, or **Port & Host** to port an existing phone number.

1. In the **Active Numbers** list, click the new phone number.

1. Under **Voice and Fax**, configure the following settings:

    - For **CONFIGURE WITH** field, select **Webhook, TwiML Bins, Functions, Studio, or Proxy**.

    - For **A CALL COMES IN**, select **Studio Flow**. Select your flow from the drop-down list.

    - For **PRIMARY HANDLER FAILS**, select **Studio Flow**. Select your flow from the drop-down list.

1. Go to the {{site.data.keyword.assistant_classic_short}} user interface, open the phone integration settings for your assistant.

1. In the **Phone number** field, type the phone number you configured in Flex Studio.

1. Click **Save and exit**.

## Test your phone number

You can now test that your phone number is connected to your flow by triggering a **Say/Play** widget in the Twilio Flex Flow editor.

1. Drag a **Say/Play** widget onto your flow canvas.

1. Configure the Say/Play widget with a simple phrase like `I'm alive.`.

1. Connect the **Incoming call** node on your **Trigger** widget to your **Say/Play** widget.

1. Call your phone number. You should hear your Twilio flow respond with your test phrase.

1. Delete the **Say/Play** widget and continue to the next step. 

1. If this test did not work as expected, double check your phone number configuration to make sure its attached to your flow.

## Creating a Twilio function to handle incoming calls

Now we need to configure the call flow to direct inbound calls to the assistant using a Twilio function. Follow these steps:

1. In the navigation menu, click the **All Products & Services** icon.

1. Click **Services**.

1. Click **Create Service**. Specify a service name and then click **Next**.

1. Click **Add** > **Add Function** to add a new function to your service. Name the new function `/receive-call`.

1. Replace the template in your `/receive-call` function with the following code:

    ```javascript
    exports.handler = function(context, event, callback) {
      const VoiceResponse = require('twilio').twiml.VoiceResponse;  
      const response = new VoiceResponse();
      const dial = response.dial({
        answerOnBridge: "true",
        referUrl: "/refer-handler"
      });
      const calledPhoneNumber = event.Called;
      dial.sip(`sip:${calledPhoneNumber}@{sip_uri_hostname};secure=true`);  
      return callback(null, response);
    }
    ```

    - Replace `{sip_uri_hostname}` with the hostname portion of your  assistant's phone integration SIP URI (everything that comes after `sips:`).. Note that Twilio does not support `SIPS` URIs, but does support secure SIP trunking by appending `;secure=true` to the SIP URI.

1. Click **Save**.

1. Click **Deploy All**.

## Redirecting to the incoming call handler 

In this section you will use a TwiML **Redirect**** widget in your Studio Flow editor to call out to the `/call-recieve` function created in the previous section.

1. Add a **TwiML Redirect** widget to your Studio Flow canvas. 

1. Connect the Incoming Call trigger to your **TwiML Redirect** widget.

1. Configure the **TwiML Redirect** widget with the URL for the `/receive-call` function you created in the previous section.

1. Your flow should now redirect to {{site.data.keyword.assistant_classic_short}} when receiving an inbound call. 

1. If the redirect fails, make sure you deployed your `/receive-call` function.

## Creating a Twilio function to handle transfers from assistant

We also need to configure the call flow to handle calls being transferred from the assistant back to Twilio Flex, for cases when when customers ask to speak to an agent. To show this, we will use a **Say/Play** after the **TwiML Redirect** widget to show that the call is transferred back to the flow from {{site.data.keyword.assistant_classic_short}}. Note that there are many things like queuing the call for a live agent that can happen at this point. These will be discussed below.

1. Add a new **Say/Play** widget to your canvas and configure it with a phrase like `Transfer from Watsom complete`.

1. Connect the **Return** node on the **TwiML Redirect** widget to your **Say/Play** widget.

1. Click the **Trigger** widget.

1. Copy the value from the **WEBHOOK URL** field. You will need this value in a subsequent step.

1. On the Twilio Functions page, click **Add** > **Add Function** to add another new function to your service. Name this new function `/refer-handler`.

1. Replace the template in your `/refer-handler` function with the following code:

    ```javascript
    exports.handler = function(context, event, callback) {
      // This function handler will handle the SIP REFER back from the Watson Assistant Phone Integration.
      // Before handing the call back to Twilio, it will extract the session history key from the
      // User-to-User header that's part of the SIP REFER Refer-To header. This session history key
      // is a string that is used to load the agent application in order to share the transcripts of the caller
      // with Watson Assistant to the agent.
      // See https://github.com/watson-developer-cloud/assistant-web-chat-service-desk-starter/blob/main/docs/AGENT_APP.md
      const VoiceResponse = require('twilio').twiml.VoiceResponse;
      
      const STUDIO_WEBHOOK_URL = '{webhook_url}';
      
      let studioWebhookReturnUrl = `${STUDIO_WEBHOOK_URL}?FlowEvent=return`;
      
      const response = new VoiceResponse();
      console.log("ReferTransferTarget: " + event.ReferTransferTarget);
      
      const referToSipUriHeaders = event.ReferTransferTarget.split("?")[1];
      console.log(referToSipUriHeaders);
      if (referToSipUriHeaders) {
        const sanitizedReferToSipUriHeaders = referToSipUriHeaders.replace(">", "");
        console.log("Custom Headers: " + sanitizedReferToSipUriHeaders);
        
        const sipHeadersList = sanitizedReferToSipUriHeaders.split("&");
        
        const sipHeaders = {};
        for (const sipHeaderSet of sipHeadersList) {
          const [name, value] = sipHeaderSet.split('=');
          sipHeaders[name] = value;
        }

        const USER_TO_USER_HEADER = 'User-to-User';
        
        // Extracts the User-to-User header value
        const uuiData = sipHeaders[USER_TO_USER_HEADER];
        
        if (uuiData) {
          const decodedUUIData = decodeURIComponent(uuiData);
          const sessionHistoryKey = decodedUUIData.split(';')[0];
          // Passes the session history key back to Twilio Studio through a query parameter.
          studioWebhookReturnUrl = `${studioWebhookReturnUrl}&SessionHistoryKey=${sessionHistoryKey}`;
        }    
      }

      response.redirect(
        { method: 'POST' },
        studioWebhookReturnUrl
      );

      // This callback is what is returned in response to this function being invoked.
      // It's really important! E.g. you might respond with TWiML here for a voice or SMS response.
      // Or you might return JSON data to a studio flow. Don't forget it!
      return callback(null, response);
    }
    ```

    Replace `{webhook_url}` with the **WEBHOOK URL** value you copied from the **Trigger** widget in your Studio Flow.

1. Click **Save**.

1. Click **Deploy All**.

1. After you create this refer-handler, copy the function URL back into the `/receive-call` handler's **referUrl** field.

## Configuring the assistant to transfer calls to Twilio Flex

Now we need to configure the assistant to transfer calls to Twilio Flex when a customer asks to speak to an agent. Follow these steps:

1. In the {{site.data.keyword.assistant_classic_short}} user interface, open the dialog skill of your assistant.

1. Add a node with the condition you want to trigger your assistant to transfer customers to an agent.

1. Add a text response to the node, and specify the text you want your assistant to say to your customers before it transfers them to an agent.

1. Open the JSON editor for the response.

1. In the JSON editor, add a [`connect_to_agent` response](https://cloud.ibm.com/docs/assistant?topic=assistant-phone-context), specifying your phone number as the `sip.uri` (replace `{phone_number}` with the phone number of your SIP trunk):

```json
{
  "output": {
    "generic": [
      {
          "response_type": "connect_to_agent",
          "transfer_info": {
            "target": {
              "service_desk": {
                "sip": {
                  "uri": "sip:+{phone_number}@flex.twilio.com",
                  "transfer_headers_send_method": "refer_to_header"
                }
              }
            }
          },
          "agent_available": {
            "message": "Ok, I'm transferring you to an agent"
          },
          "agent_unavailable": {
            "message": ""
          }
      }
    ]
  }
}
```

Note that this example does not show how to use the context passed from {{site.data.keyword.assistant_classic_short}} to Twilio Flex. You can reference the User-to-User information from within the Twilio Flex flow as follows:

```json
{
  "context": {
    "widgets": {
      "redirect_1": {
        "User-to-User": "value",
      }
    }
  }
}
```

where `redirect_1` is the name of your redirect widget. For example, if you set up multiple queues, you might want to use a Twilio Split widget to pick a queue based on the returned context.

## Test your assistant

Your assistant should now be able to answer phone calls to your phone number and transfer calls back to your Twilio Flex flow. To test your assistant:

1. Call your phone number. When the assistant responds, ask for an agent.

1. At this point you should hear the phrase configured in the **Say/Play** widget (such as "Transfer from Watson complete").

1. If the transfer fails, use the console log to follow the flow of the call as it moves from the flow to the `/call-receive` handler, to {{site.data.keyword.assistant_classic_short}}, to the refer-handler and back to your Twilio Flex flow.
 

###  Share the conversation history with service desk agents 
To enable the service desk agent to get a quick view of the conversation history between the visitor and the assistant, set up the Watson Assistant Agent App app for your Twilio Flex environment. For more information, see the documentation for the [Twilio Flex Watson Assistant Agent App](https://github.com/watson-developer-cloud/assistant-web-chat-service-desk-starter/tree/main/src/flex/agentApp){: external}.
