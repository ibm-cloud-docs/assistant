---

copyright:
  years: 2020, 2021
lastupdated: "2021-07-28"

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

# Integrating with phone ![Plus or higher plans only](images/plus.png)
{: #deploy-phone}

Add a phone integration so your assistant can answer when your customers call.
{: shortdesc}

When your customer makes a phone call through a Session Initiation Protocol (SIP) trunk that you configure, the phone integration answers. The integration converts output from your dialog from text to voice by using the {{site.data.keyword.texttospeechfull}} service. The audio is sent to the telephone network through the SIP trunk. When the customer replies, voice is converted to text by using the {{site.data.keyword.speechtotextfull}} service.

This feature is available only to Plus or Enterprise plan users. Note that {{site.data.keyword.speechtotextshort}} and {{site.data.keyword.texttospeechshort}} charges are included in the cost of a [monthly active user](/docs/assistant?topic=assistant-services-information#services-information-user-based-plans) (MAU).

Depending on the architecture of your existing telephony infrastructure, there are multiple ways you might integrate it with Watson Assistant. For more information about common integration patterns, read the blog post [Hey Watson, can I have your number?](https://medium.com/ibm-watson/hey-watson-can-i-have-your-number-7de8fc7621ed) on Medium.
{: tip}

## Set up the integration
{: #deploy-phone-setup}

You must have Manager service level access to the instance. For more information about access levels, see [Managing access to resources](/docs/assistant?topic=assistant-access-control).

To watch a video that walks through the setup process, see [Phone and SMS Integration](https://community.ibm.com/community/user/watsonapps/viewdocument/phone-and-sms-integration?CommunityKey=7a3dc5ba-3018-452d-9a43-a49dc6819633&tab=librarydocuments){: external} in the *IBM Watson Apps Community*.

To set up the integration, complete the following steps:

1. From the Assistants page, click to open the assistant tile that you want to deploy.

1. From the Integrations section, click **Add integration**.

1. Click **Phone**.

1. Click **Create**.

1. If you don't have a SIP trunk configured, [work with a SIP trunk provider](#deploy-phone-sip-providers) to set up a SIP trunk now. Otherwise, skip this step. 

    A SIP trunk is equivalent to an analog telephone line, except it uses Voice over Internet Protocol (VoIP) to transmit voice data and can support multiple concurrent calls. The trunk can connect to the public switched telephone network (PSTN) or your company's on-premises private branch exchange (PBX).

    - Scroll to the *SIP Trunking configuration* section, and then copy the value from the **SIP uniform resource identifier (URI)** field. 

      You will need to provide the SIP URI for your assistant during the SIP trunk setup process.
    
    - Create a phone number through your SIP provider, and then assign it to your SIP trunk. 

1.  In the *Phone number* section of the phone integration setup page, add one or more phone numbers.

    You might have only the one phone that you created through your SIP trunk provider in the previous step, or you might have a set of numbers.

    - If you have one phone number, add it to the field.

      Specify the number by using the international phone number format: `+1 958 555 0123`. Do *not* surround the area code with parentheses, such as (958).

    - If you have multiple phone numbers, click **Add**.

      Add a phone number and an optional description, and then click the checkmark icon ![checkmark icon](images/phone-checkmark-save.png) to save the number.

      - To add more phone numbers one by one, click the *add phone number* icon (![Add phone number][images/phone-integ-add-number.png]), and then specify the phone number and an optional description. Repeat to add more numbers.

      - To import a set of phone numbers that are stored in a comma-separated values (CSV) file, click the *Upload a CSV file* icon (![Add phone number][images/phone-integ-import-number.png]), and then find the CSV file that contains the list of phone numbers.

      The phone numbers you upload will replace any existing numbers in the table.
      {: important}

     The phone numbers must be unique per phone integration. If you use Twilio as the SIP trunk provider, you can use the same phone number for the phone and text messaging integrations.

     Click **Save**.

1. Review the speech services that will be used by the phone integration.

    If you don't have existing instances of the speech services, a Plus plan instance of each of the following services is created for you:

    - {{site.data.keyword.speechtotextshort}}
    - {{site.data.keyword.texttospeechshort}}

    If you have instances of the {{site.data.keyword.speechtotextshort}} and {{site.data.keyword.texttospeechshort}} services in paid plans, they are configured for use with your phone integration automatically.

    Stop and create speech service instances yourself before you finish setting up the integration in the following cases:

    - If you have Lite plan instances of the speech services, the automatic creation process is not started. Consider deleting the lite plan instances or create Plus plan instances of the services.
    - If you want to use instances that are dedicated to handling speech services for your assistant only, and your existing instance is already in use by other applciations.
    - If you want to use a paid plan other than Plus.
    - If you have an Enterprise with Data Isolation {{site.data.keyword.conversationshort}} plan, create Premium plan instances of the speech services so you can select them during the setup process.
    
    Create the speech instances in the same data center location before you set up the integration. Then, you can choose the existing instances from the list.
    
    The models that are chosen automatically use the same language as the assistant to which you are adding the integration. You can choose to use different models if you want.
    
    If you want to use a model that was created in a different service instance, click **More options** to show all service instances that you can access as options. <!--For example, if you created specialized custom models that you want your assistant to use, you can find and select them.-->

    If you created specialized custom models that you want your assistant to use, choose the service instance that hosts the custom models now, and you can configure your dialog to use them later. The service instance must be hosted in the same location as your {{site.data.keyword.conversationshort}} service instance. For more information, see [Using a custom language model](/docs/assistant?topic=assistant-dialog-voice-actions#dialog-voice-actions-custom-language).
    {: note}

    - **{{site.data.keyword.speechtotextshort}}**: Optionally choose a different {{site.data.keyword.speechtotextshort}} service language model to use to define the language your assistant will use when it transcribes what customers say.

      For example, indicate whether to use British or American English. The list shows options from {{site.data.keyword.speechtotextshort}} service instances that you can access. 
      
      For more information about language models, see [Languages and models](https://cloud.ibm.com/docs/speech-to-text?topic=speech-to-text-models){: external} in the {{site.data.keyword.speechtotextshort}} documentation.

    - **{{site.data.keyword.texttospeechshort}}**: Optionally choose a different {{site.data.keyword.texttospeechshort}} service voice model to use a different voice for your assistant. 

      The list shows options from {{site.data.keyword.texttospeechshort}} service instances that you can access. 
      
      For more information about voice options, and to listen to audio samples, see [Languages and voices](https://cloud.ibm.com/docs/text-to-speech?topic=text-to-speech-voices){: external} in the {{site.data.keyword.texttospeechshort}} documentation.

    Regardless of the instances you choose to use, any speech service charges that are incurred by the phone integration are billed with the {{site.data.keyword.conversationshort}} service plan as *voice add-on* charges. After the instances are created, you can access them directly from the IBM Cloud dashboard. Any use of the speech instances that occurs outside of your assistant are charged separately as speech service usage costs.
    {: important}

1. Click **Save and exit**.

If you want your assistant to be able to switch between voice and text during a customer interaction, enable both the phone and SMS with Twilio integrations. The integrations do not need to use the same third-party service provider. For more information, see [Integrating with *SMS with Twilio*](/docs/assistant?topic=assistant-deploy-sms).

## Advanced configuration options
{: #deploy-phone-advanced}

Click the *Advanced options* tab to make any of the following customizations to the call behavior:

### Handle call and transfer failures
{: #deploy-phone-advanced-failure}

You can configure the phone integration to transfer the caller to a human agent if the phone connection fails for any reason. To transfer the caller to a human agent automatically, make the following configuration selections:

- **SIP target when a call fails**: Add the SIP endpoint for your support agent service. Specify a SIP or telephone URI for a general call queue that can redirect requests to other queues. For more information, see [Configuring backup support](#deploy-phone-transfer-service).
- **Call failure message**: Add the message to say to a caller before you transfer them to a human agent.

If, after you transfer the caller to a human agent, the connection to the human agent fails for any reason, you can configure what to do.

- **Transfer failure message**: Add the message to stream to callers if the transfer to a human agent fails. The message can be up to 1,024 characters in length.
- **Disconnect call on transfer failure**: Choose whether to disconnect the call after sharing the failure message. This option is enabled by default. If disabled, when a call transfer fails, your assistant can disconnect or process a different dialog node.

### Secure the phone connection
{: #deploy-phone-security}

You can add security to the phone connection by selecting one or both of the following configuration options:

- **Force secure trunking**: Select this option to use Secure Real-Time Transfer Protocol (SRTP) to secure the audio that is transmitted over the phone. For more information about RTP, see [Call routing details](#deploy-phone-route).
- **Enable SIP authentication**: Select this option if you want to require SIP digest authentication. 

  When SIP authentication is required, all inbound traffic (meaning requests from the SIP provider to your assistant) are authenticated using SIP digest authentication, and must be sent using Transport Layer Security (TLS). If this option is selected, the SIP digest user name and password must be configured, and the SIP trunk being used to connect to Assistant must be configured to use only TLS.

  If you use Twilio as your SIP trunk provider, you cannot enable SIP authentication for outbound SIP trunks to Watson Assistant.
  {: important}

### Apply advanced SIP trunk configuration settings
{: #deploy-phone-sip-trunk-config}

- **SIP INVITE headers to extract**: List headers that you want to use in your dialog.

  The SIP request often sends INVITE headers with information about the request that is used by the SIP network. For example, many companies use Interactive Voice Response (IVR) systems that pass information about an incoming call by using headers. If you want to make use of any of these headers, list the header names here.
  
  The specified headers, if present in the request, are stored in the context variable `vgwSIPCustomInviteHeaders`. This variable is an array in which each key/value pair represents a header from the request, as in this example:

    ```json
    "vgwSIPCustomInviteHeaders": {
      "X-customer-name": "my_name",
      "X-account-number": "12345"
    }
    ```

  You can then reference these headers in your dialog. For example, you might check the header value in a dialog node condition to determine whether to process a branch. You can also use these headers when searching the Assistant logs; for example, you might search for a custom header to find all the messages associated with particular account.

- **Disable the ring that callers will hear while the assistant is contacted**: Choose whether you want the caller to hear a signal that indicates that the assistant is being contacted. 

  A `180 Ringing` response is sent from the assistant back to the SIP trunk provider while your assistant processes the incoming call invitation. The ringing response is sent by default.

- **Don't place callers on hold while transferring to a live agent**: Choose whether the phone integration puts the caller on hold. 

  If your SIP trunk provider manages holds, disable this feature. For example, some SIP trunk providers prefer to have the assistant send a SIP REFER request, so they can put the call on hold themselves.

For more information about the SIP protocol, see [RFC 3261](https://tools.ietf.org/html/rfc3261){: external} and about the RTP protocol, see [RFC 3550](https://tools.ietf.org/html/rfc3550){: external}.

## Configuring backup call center support
{: #deploy-phone-transfer-service}

When you use the phone integration as the first line of assistance for customers, it's a good idea to have human backup available. You can design your assistant to be able to transfer a call to a human in case the phone connection fails, or a user asks to speak to someone.

Your company might already have one or more phone numbers that connect to an automatic call dispatcher (ACD) that can queue callers until an appropriate agent is available. If not, choose a call center service to use as your backup.

A conversation cannot be transferred from one integration type to another. If you use the web chat integration with service desk support, there's no way to transfer the phone call to the existing service desk that is set up for the web chat, for example.

For whichever call center service you use, you will need to provide the call center SIP URI. You must specify this information in your dialog when you enable a call transfer from a dialog node. For more information, see [Transfer a call to a human agent](/docs/assistant?topic=assistant-dialog-voice-actions#dialog-voice-actions-transfer).

## Optimize your dialog for voice
{: #deploy-phone-dialog}

For the best customer experience, design your dialog with the capabilities of the phone integration in mind:

- Do not include HTML elements in your dialog text responses. To add formatting, use Markdown. For more information, see [Simple text response](https://cloud.ibm.com/docs/assistant?topic=assistant-dialog-overview#dialog-overview-simple-text).
- The phone integration does not support chat transfers that are initiated with the *Connect to human agent* response type. Use the [vgwActTransfer](/docs/assistant?topic=assistant-dialog-voice-actions#dialog-voice-actions-transfer) command instead.
- The pause response type is ignored. If you want to add a pause, use a [vgwConversationResponseTimeout](/docs/assistant?topic=assistant-commands-voice#commands-voice-context-variables) context variable instead.
- You can include search skill response types in dialog nodes that the phone integration will read. The introductory message (*I searched my knowledge base* and so on), and then the body of only the first search result is read.

  The search skill response (meaning the introductory message plus the body of the first search result) must be less than 5,000 characters long or the response will not be read at all. Be sure to test the search results that are returned and curate the data collection that you use as necessary.

For more information about dialog response types, see [Rich responses](/docs/assistant?topic=assistant-dialog-overview#dialog-overview-multimedia).

For more information about how to implement common actions from your dialog, see [Handling phone integrations](/docs/assistant?topic=assistant-dialog-voice-actions).

If you want to use the same dialog for an assistant that you deploy to many different platforms, you can add custom responses per integration type. Add a conditioned response that tells the assistant to show the response only when the phone integration is being used. For more information, see [Building integration-specific responses](/docs/assistant?topic=assistant-dialog-integrations#dialog-integrations-condition-by-type).

## Setting up a SIP trunk
{: #deploy-phone-sip-providers}

You are responsible for setting up the SIP trunk that will be used by the phone integration. Find a provider and create a SIP trunk account. Your account will be charged for the phone integration's use of the SIP trunk.

You can set up a SIP trunk in the following ways:

- [Create a Twilio SIP trunk](#deploy-phone-twilio-setup)
- [Use other third-party providers](#deploy-phone-request-setup)
- [Bring your own SIP trunk](#deploy-phone-byost)
- [Migrate from Voice Agent with Watson](#deploy-phone-migrate-from-va)

### Creating a Twilio SIP trunk
{: #deploy-phone-twilio-setup}

To set up a Twilio SIP trunk, complete the following steps: 

1.  Create a Twilio account on the [Twilio website](https://www.twilio.com/sip-trunking){: external}.

1.  From the Twilio website, go to the *Elastic SIP Trunking* dashboard.

1.  Select *Trunks* from the navigation bar and create a SIP trunk. If you already have a SIP trunk, click the plus sign (+). Enter a name for your SIP trunk and click *Create*.

1.  From the Elastic SIP Trunks page, select your SIP trunk.

1.  Select *Origination* from the navigation bar for your SIP trunk and configure the origination SIP URI.

    You can get the SIP URI for your phone integration from the phone integration configuration page.

1.  If you plan to support call transfers, enable Call Transfer (SIP REFER) in your SIP trunk. If you expect to transfer calls to the public switched telephone network (PSTN), also enable PSTN Transfer on your trunk.

1.  Select *Numbers* from the navigation bar for your SIP trunk, and then do one of the following things:
  
    - Click *Buy a Number*.
    - If you already have a number, you can click the plus sign (+) to provision a new phone number in your region.
  
1.  Assign the number to the SIP trunk you created by going back to the SIP trunk and clicking the number sign (#) icon.

If you use a Lite or Trial Twilio account for testing purposes, then be sure to verify the transfer target. For more information, see the [Twilio documentation](https://support.twilio.com/hc/en-us/articles/223136107-How-does-Twilio-s-Free-Trial-work-){: external}.

You cannot enable SIP authentication if you choose Twilio as your SIP trunk provider. Twilio doesn't support SIPS for originating calls.

### Using other third-party providers
{: #deploy-phone-request-setup}

You can ask for help setting up an account with another SIP trunk provider by opening a support request.

IBM has established relationships with the following SIP trunk providers:

- [Five9](https://www.five9.com/connect/voip-call-center-solution)
- [Genesys](https://www.genesys.com/en-sg/definitions/what-is-a-trunk)
- [Vonage](https://www.vonage.com/communications-apis/sip-trunking/)
- [Voximplant](https://voximplant.com/)

The SIP trunk provider sets up a SIP trunk for your voice traffic, and manages access from allowed IP addresses. Most of the major SIP trunk providers have existing relationships with IBM. Therefore, the network configuration that is required to support the SIP trunk connection typically can be handled for you with minimal effort.

1. Create a [{{site.data.keyword.Bluemix_notm}} case](https://cloud.ibm.com/unifiedsupport/cases/form){: external}.

1. Click **Customer success** as the case type.

1. For **Subject**, enter `SIP trunk provider setup for Watson Assistant`.

1. Include the following information in the description:

   - Company Name
   - Your {{site.data.keyword.Bluemix_notm}} account ID
   - Your {{site.data.keyword.conversationshort}} service name
   - Network diagram with IP address or SIP trunk provider information

### Bring your own SIP trunk
{: #deploy-phone-byost}

If you choose to use a SIP trunk carrier that IBM does not have an established relationship with, you can do so.

The following table lists the fully qualified domain names that are used for SIP connections.

| Location | Domain name |
|----------|------|------------|
| Dallas | public.0001.voip.us-south.assistant.watson.cloud.ibm.com |
| Dallas | public.0002.voip.us-south.assistant.watson.cloud.ibm.com |
| Dallas | public.0003.voip.us-south.assistant.watson.cloud.ibm.com |
| Frankfurt | public.0001.voip.eu-de.assistant.watson.cloud.ibm.com |
| Frankfurt | public.0002.voip.eu-de.assistant.watson.cloud.ibm.com |
| Frankfurt | public.0003.voip.eu-de.assistant.watson.cloud.ibm.com |
| London | public.0001.voip.eu-gb.assistant.watson.cloud.ibm.com |
| London | public.0002.voip.eu-gb.assistant.watson.cloud.ibm.com |
| London | public.0003.voip.eu-gb.assistant.watson.cloud.ibm.com |
| Seoul | public.0001.voip.kr-seo.assistant.watson.cloud.ibm.com |
| Sydney | public.0001.voip.au-syd.assistant.watson.cloud.ibm.com |
| Sydney | public.0002.voip.au-syd.assistant.watson.cloud.ibm.com |
| Sydney | public.0003.voip.au-syd.assistant.watson.cloud.ibm.com |
| Tokyo | public.0001.voip.jp-tok.assistant.watson.cloud.ibm.com |
| Tokyo | public.0002.voip.jp-tok.assistant.watson.cloud.ibm.com |
| Tokyo | public.0003.voip.jp-tok.assistant.watson.cloud.ibm.com |
| Washington, DC | public.0001.voip.us-east.assistant.watson.cloud.ibm.com |
| Washington, DC | public.0002.voip.us-east.assistant.watson.cloud.ibm.com |
| Washington, DC | public.0003.voip.us-east.assistant.watson.cloud.ibm.com |
{: caption="SIP network information" caption-side="top"}

### Migrating from Voice Agent with Watson
{: #deploy-phone-migrate-from-va}

If you created an {{site.data.keyword.iva_full}} service instance in IBM Cloud to enable customers to connect to an assistant over the phone, use the phone integration instead. You can use the same SIP account and phone number that you configured for use with {{site.data.keyword.iva_short}} in the phone integration.

The phone integration provides a more seamless integration with your assistant. However, the integration currently does not support the following functions:

- Outbound calling
- Configuring backup locations
- Event forwarding to save call detail reports in the IBM Cloudant for IBM Cloud database service <!-- Use the CDR API instead. -->
- Reviewing the usage summary page. Use IBM Log Analysis instead. For more information, see [Viewing logs](#deploy-phone-logs).

To migrate from {{site.data.keyword.iva_short}} to the {{site.data.keyword.conversationshort}} phone integration, complete the following steps:

1.  From the {{site.data.keyword.iva_short}} page, copy the phone number or numbers that you used for your SIP account.
1.  When you set up the {{site.data.keyword.conversationshort}} phone integration, add the phone number or set of numbers that you copied in the previous step.
1.  From the phone integration setup page, copy the *SIP uniform resource identifier (URI)*. 
1.  In your SIP trunk account, replace the {{site.data.keyword.iva_short}} URI that you specified previously with the URI that you copied from the phone integration setup page in the previous step. 

    For example, if you use a Twilio SIP trunk, you would add the assistant's *SIP uniform resource identifier (URI)* to the Twilio *Origination SIP URI* field.

### Call routing details
{: #deploy-phone-route}

Incoming calls to your assistant follow this path: 

1.  A customer calls the customer support phone number that is managed by your Session Initiation Protocol (SIP) trunk provider.
1.  The SIP trunk service sends a SIP `INVITE` HTTP request to your assistant's phone integration to establish a connection. 
1.  The phone integration connects to the speech services that are required to support the interaction.
1.  After the services are ready, the connection is established, and audio is sent over the Real-time Transport Protocol (RTP). 

    RTP is a network protocol for delivering audio and video over IP networks.
1.  The welcome node of the dialog is processed. The response text is sent to the {{site.data.keyword.texttospeechshort}} service to be converted to audio and the audio is sent to the caller.
1.  When the customer says something, the audio is converted to text by the {{site.data.keyword.speechtotextshort}} service and is sent to your assistant's dialog skill for evaluation.
1.  The dialog processes the input and calculates the best response. The response text from the dialog node is sent to the {{site.data.keyword.texttospeechshort}} service to be converted to audio and the audio is sent back to the caller over the existing connection.
1.  If the caller asks to speak to a person, the assistant can transfer the person to a call center. A SIP `REFER` request is sent to the SIP trunk provider so it can transfer the call to the call center SIP URI that is specified in the dialog node where the transfer action is configured.
1.  When one of the participants of the call hangs up, a SIP `BYE` HTTP request is sent to the other participant.

## Phone integration limits
{: #deploy-phone-limits}

Any speech service charges that are incurred by the phone integration are included as *Voice add-on* charges in your {{site.data.keyword.conversationshort}} service plan usage. The Voice add-on use is charged separately and in addition to your service plan charges. 

Plan usage is measured based on the number of monthly active users, where a user is identified by the caller's unique phone number. An MD5 hash is applied to the phone number and the 128-bit hash value is used for billing purposes.

The number of concurrent calls that your assistant can participate in at one time depends on your plan type.

| Plan             |  Concurrent calls |
|------------------|------------------:|
| Enterprise       |             1,000 |
| Plus             |               100 |
| Trial            |                 5 |
{: caption="Plan details" caption-side="top"}

## Troubleshooting the phone integration
{: #deploy-phone-troubleshooting}

Find solutions to problems that you might encounter while using the integration.

- If you get a *Forbidden* message, it means the phone number that you specified when you configured the integration cannot be verified. Make sure the number fully matches the SIP trunk phone number.

### Viewing logs
{: #deploy-phone-logs}

The log events that occur in the components that are used by the phone integration are written to IBM Log Analysis. To check the logs, create an instance and configure the platform logs to observe the region where your service instance is hosted.

For more information about setting up an instance, see [Provisioning an instance](/docs/log-analysis?topic=log-analysis-provision){: external}.

Log events that are generated by the integration are automatically forwarded to the IBM Log Analysis service instance that is available in the same location. However, if your service instance is hosted in the Washington DC location, create the IBM Log Analysis service instance in the Dallas region.

Currently, the Phone and SMS with Twilio integrations are the only components of your assistant that write logs to the IBM Log Analysis dashboard.
{: note}

After you create the instance, get log information by completing the following steps:

1.  Go to the [IBM Cloud Logging](https://cloud.ibm.com/observe/logging) page.
1.  Click *Configure platform logs*.
1.  Select the region and instance, and then click *Select*.
1.  To open the IBM Log Analysis console, click **Open Dashboard**.
1.  The source name of the log events is *Watson*.

    You can apply filters or search the logs by values such as a phone number or instance ID.
