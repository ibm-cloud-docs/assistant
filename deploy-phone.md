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

# Integrating with phone ![Beta](images/beta.png)
{: #deploy-phone}

Add a phone integration so your assistant can answer when your customers call.
{: shortdesc}

When your customer makes a phone call through a Session Initiation Protocol (SIP) trunk that you configure, the phone integration answers. The integration converts output from your dialog from text to voice by using the {{site.data.keyword.texttospeechfull}} service. The audio is sent to the telephone network through the SIP trunk. When the customer replies, voice is converted to text by using the {{site.data.keyword.speechtotextfull}} service.

This integration type is available as a beta feature in the Dallas, Frankfurt, and Washington, DC locations.
{: note}

## Set up the integration
{: #deploy-phone-setup}

You must have Manager service level access to the instance. For more information about access levels, see [Managing access to resources](/docs/assistant?topic=assistant-access-control).

1. From the Assistants page, click to open the assistant tile that you want to deploy.

1. From the Integrations section, click **Add integration**.

1. Click **Phone**.

1. Click **Create**.

1. Scroll to the *SIP Trunking configuration* section, and then copy the value from the **SIP uniform resource identifier (URI)** field. 

    You will provide this value to your SIP trunk provider when you configure the trunk in the next step. 

1. If you don't have a SIP trunk configured, [work with a SIP trunk provider](#deploy-phone-sip-providers) to set up a SIP trunk now.

    A SIP trunk is equivalent to an analog telephone line, except it uses Voice over Internet Protocol (VoIP) to transmit voice data and can support multiple concurrent calls. The trunk can connect to the public switched telephone network (PSTN) or your company's on-premises private branch exchange (PBX).

    - You will need to provide the SIP URI for your assistant during the SIP trunk setup process.
    - Create a phone number through your SIP provider, and then assign it to your SIP trunk.

1. On the phone integration setup page, add the phone number that you created through your SIP trunk provider in the previous step to the **Phone number** field.

     Specify the number by using the international phone number format: `+1 958 555 0123`. Do *not* surround the area code with parentheses, such as (958).
    
     The phone number must be unique per phone integration. If you use Twilio as the SIP trunk provider, you can use the same phone number for the phone and text messaging integrations.

     If you get a *Forbidden* message, it means the phone number cannot be verified. Make sure the number fully matches the SIP trunk phone number.

1. Review the speech services that will be used by the phone integration.

    If you don't have existing instances of the speech services, a Plus plan instance of each of the following services is created for you:

    - {{site.data.keyword.speechtotextshort}}
    - {{site.data.keyword.texttospeechshort}}

    If you have instances of the {{site.data.keyword.speechtotextshort}} and {{site.data.keyword.texttospeechshort}} services, they are configured for use with your phone integration automatically. 
    
    The models that are chosen automatically use the same language as the assistant to which you are adding the integration. You can choose to use different models if you want.

    If you want to use instances that are dedicated to handling speech services for your assistant only, or if you want to use a plan other than Plus, create the instances first. You must create the instances before you set up the integration. Then, you can choose the existing instances from the list.
    {: tip}
    
    If you want to use a model that was created in a different service instance, click **More options** to show all service instances that you can access as options. For example, if you created specialized custom models that you want your assistant to use, you can find and select them.

    - **{{site.data.keyword.speechtotextshort}}**: Optionally choose a different {{site.data.keyword.speechtotextshort}} service language model to use to define the language for your assistant to use when it speaks to customers.

      For example, indicate whether to use British or American English. The list shows options from {{site.data.keyword.speechtotextshort}} service instances that you can access. 
      
      For more information about language models, see [Languages and models](https://cloud.ibm.com/docs/speech-to-text?topic=speech-to-text-models){: external} in the {{site.data.keyword.speechtotextshort}} documentation.

    - **{{site.data.keyword.texttospeechshort}}**: Optionally choose a different {{site.data.keyword.texttospeechshort}} service voice model to use a different voice for your assistant. 

      The list shows options from {{site.data.keyword.texttospeechshort}} service instances that you can access. 
      
      For more information about voice options, and to listen to audio samples, see [Languages and voices](https://cloud.ibm.com/docs/text-to-speech?topic=text-to-speech-voices){: external} in the {{site.data.keyword.texttospeechshort}} documentation.

    Regardless of the instances you choose to use, any speech service charges that are incurred by the phone integration are included in usage costs of the {{site.data.keyword.conversationshort}} service plan. After the instances are created, you can access them directly from the IBM Cloud dashboard. Any API calls that are made to the instances outside of your assistant are charged separately as speech service usage costs.
    {: important}

1. Click **Save and exit**.

If you want your assistant to be able to switch between voice and text during a customer interaction, enable both the phone and Twilio messaging integrations. The integrations do not need to use the same third-party service provider. For more information, see [Integrating with *Twilio messaging*](/docs/assistant?topic=assistant-deploy-sms).

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

- **Enable secure trunking**: Select this option to use Secure Real-Time Transfer Protocol (SRTP) to secure the audio that is transmitted over the phone. For more information about RTP, see [Call routing details](#deploy-phone-route).
- **Enable SIP authentication**: Select this option if you want the session initiation to be done over SIP Secure (SIPS). 

  SIPS applies TLS (Transport Layer Security) to the connection. When SIPS is enabled, all inbound traffic, meaning requests from the SIP trunk provider to your assistant, must be authenticated. After selecting this option, allow the service credentials (API key) for your {{site.data.keyword.conversationshort}} service instance to be used for authentication.

  If you use Twilio as your SIP trunk provider, do not enable SIP authentication. Twilio does not support SIP authentication of originating calls.
  {: important}

### Apply advanced SIP trunk configuration settings
{: #deploy-phone-sip-trunk-config}

- **SIP INVITE headers to extract**: List headers that you want to use in your dialog. 

  The SIP request often sends INVITE headers with information about the request that is used by the SIP network. For example, many companies use Interactive Voice Response (IVR) systems that pass information about an incoming call by using headers. If you want to make use of any of these headers, list the header names here. When provided, the header value is stored as a context variable (`$vgwSIPCustomInviteHeader`) that can be referenced by your dialog. For example, you can check the header value in a dialog node condition to determine whether to process a branch or not. Or you might pass custom headers, such as a `session_id` that you can use subsequently to find all the messages from a single call in the assistant logs. Header titles do not include spaces.

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

- Do not include HTML elements in your dialog text responses.
- The phone integration does not support chat transfers that are initiated with the *Connect to human agent* response type.
- You can include search skill response types in dialog nodes that the phone integration will read. The introductory message (*I searched my knowledge base* and so on), and then the body of only the first search result is read.

  Your search results must be short. Be sure to test the search results that are returned and curate the data collection that you use as necessary.

For more information about how to implement common actions from your dialog, see [Handling phone integrations](/docs/assistant?topic=assistant-dialog-voice-actions).

## Supported SIP trunk providers
{: #deploy-phone-sip-providers}

The following SIP trunk providers are supported:

- [Twilio](#deploy-phone-twilio-setup)
- [Other third-party providers](#deploy-phone-request-setup)

You need a credit card to create an account with a third-party provider. Your account is charged for the phone integration's use of the SIP trunk that you configure.
{: note}

### Creating a Twilio SIP trunk
{: #deploy-phone-twilio-setup}

To set up a Twilio SIP trunk, complete the following steps: 

1.  Create a Twilio account on the [Twilio website](https://www.twilio.com/sip-trunking){: external}.

1.  Create a SIP trunk with your Twilio account.

1.  From the Twilio website, go to the *Elastic SIP Trunking* dashboard.

1.  Select *Trunks* from the navigation bar and create a SIP trunk. If you already have a SIP trunk, click the plus sign (+). Enter a name for your SIP trunk and click *Create*.

1.  From the Elastic SIP Trunks page, select your SIP trunk.

1.  Select *Origination* from the navigation bar for your SIP trunk and configure the origination SIP URI.

    You can get the SIP URI for your phone integration from the phone integration configuration page.

1.  If you plan to support call transfers, enable the public switched telephone network (PSTN) option also.

1.  Select *Numbers* from the navigation bar for your SIP trunk, and then do one of the following things:
  
    - Click *Buy a Number*.
    - If you already have a number, you can click the plus sign (+) to provision a new phone number in your region.
  
1.  Assign the number to the SIP trunk you created by going back to the SIP trunk and clicking the number sign (#) icon.

If you use a Lite or Trial Twilio account for testing purposes, then be sure to verify the transfer target. For more information, see the [Twilio documentation](https://support.twilio.com/hc/en-us/articles/223136107-How-does-Twilio-s-Free-Trial-work-){: external}.

You cannot enable SIP authentication if you choose Twilio as your SIP trunk provider. Twilio doesn't support SIPS for originating calls.

### Using other third-party providers
{: #deploy-phone-request-setup}

You can ask for help setting up an account with another SIP trunk provider by opening a support request.

The SIP trunk provider sets up a SIP trunk for your voice traffic, and manages access from allowed IP addresses. Most of the major SIP trunk providers have existing relationships with IBM. Therefore, the network configuration that is required to support the SIP trunk connection typically can be handled for you with minimal effort.

1. Create a [{{site.data.keyword.Bluemix_notm}} case](https://cloud.ibm.com/unifiedsupport/cases/form){: external}.

1. Click **Customer success** as the case type.

1. For **Subject**, enter `SIP trunk provider setup for Watson Assistant`.

1. Include the following information in the description:

   - Company Name
   - Your {{site.data.keyword.Bluemix_notm}} account ID
   - Your {{site.data.keyword.conversationshort}} service name
   - Network diagram with IP address or SIP trunk provider information

### Call routing details
{: #deploy-phone-route}

Incoming calls to your assistant follow this path: 

1.  A customer calls the customer support phone number that is managed by your Session Initiation Protocol (SIP) trunk provider.
1.  The SIP trunk service sends a SIP `INVITE` HTTP request to your assistant's phone integration to establish a connection. 
1.  The phone integration connects to the speech services that are required to support the interaction.
1.  After the services are ready, the connection is established, and audio is sent over the Real-time Transport Protocol (RTP). 

    RTP is a network protocol for delivering audio and video over IP networks.
1.  The audio is converted to text by the {{site.data.keyword.speechtotextshort}} service and is sent to your assistant's dialog skill for evaluation.
1.  The dialog processes the input and calculates the best response.
1.  The dialog text response is sent to the {{site.data.keyword.texttospeechshort}} service to be converted to audio.
1.  The audio is sent back to the caller over the existing connection.
1.  If the caller asks to speak to a person, the assistant can transfer the person to a call center. A SIP `REFER` request is sent to the SIP trunk provider so it can transfer the call to the call center SIP URI that is specified in the dialog node where the transfer action is configured.
1.  When one of the participants of the call hangs up, a SIP `BYE` HTTP request is sent to the other participant.

## Phone integration limits
{: #deploy-phone-limits}

Any speech service charges that are incurred by the phone integration are included in the usage costs of the {{site.data.keyword.conversationshort}} service plan. Plan usage is measured based on the number of monthly active users.

The number of concurrent calls that your assistant can participate in at one time depends on your plan type.

| Plan             |  Concurrent calls |
|------------------|------------------:|
| Plus             |               100 |
| Plus Trial       |               500 |
{: caption="Plan details" caption-side="top"}

<!--| Enterprise       |               500 |-->

## Troubleshooting the phone integration
{: #deploy-phone-troubleshooting}

The log events that occur in the components that are used by the phone integration are written to IBM Log Analysis with LogDNA. To check the logs, create an instance and configure the platform logs to observe the region where your service instance is hosted.

Log events that are generated by the integration are automatically forwarded to the IBM Log Analysis with LogDNA service instance that is available in the same location. However, if your service instance is hosted in the Washington DC location, create the IBM Log Analysis with LogDNA service instance in the Dallas region.

For more information about setting up an instance, see [Provisioning an instance](/docs/Log-Analysis-with-LogDNA?topic=Log-Analysis-with-LogDNA-provision){: external}.

Currently, the phone and Twilio messaging integrations are the only components of your assistant that write logs to the IBM Log Analysis with LogDNA dashboard.
{: note}

After you create the instance, you can click **View LogDNA** to see the phone integration logs.

The source name of the log events is *Watson*. You can apply filters or search the logs by values such as a phone number or instance ID.