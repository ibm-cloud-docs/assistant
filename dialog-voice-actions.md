




---

copyright:
  years: 2020, 2021
lastupdated: "2021-09-25"

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

# Handling phone interactions
{: #dialog-voice-actions}

Learn about common actions you can use to manage the flow of conversations that your assistant has with customers over the telephone.
{: shortdesc}

Before you add customizations to your dialog that support phone interactions, you must set up the phone integration. For more information, see [Integrating with phone](/docs/assistant?topic=assistant-deploy-phone).

You can perform the following types of actions:

- [Send a text message during a phone conversation](#dialog-voice-actions-sms)
- [Transfer a call to a human agent](#dialog-voice-actions-transfer)
- [Play hold music or a voice recording](#dialog-voice-actions-hold-music)
- [Apply advanced speech services to specific topics ](#dialog-voice-actions-speech-advanced)
- [Apply advanced {{site.data.keyword.texttospeechshort}} services to specific topics ](#dialog-voice-actions-text-advanced)
- [Enable keypad entry](#dialog-voice-actions-dtmf)
- [End the call](#dialog-voice-actions-hangup)



In some cases, you might want to combine actions. For example, to enable two-factor authentication, you can both enable support for phone keypad entry and send a text message from the same dialog node or step in the Action. For more information about combining actions, see [Defining a sequence of actions](#dialog-voice-actions-sequence).

For command reference documentation, see [Phone integration commands reference](/docs/assistant?topic=assistant-commands-voice).

## Adding phone-based custom response types to your dialog or action 
{: #dialog-voice-actions-add}

When calling voice-specific actions from a dialog node or a step in the Action, you need to define the action within the `generic` array.

To enable voice-specific response type, you must add a JSON code block to the dialog node or step in the Action where you want the action to trigger. 

To add a JSON code block to a dialog node, complete the following steps:

1.  From your dialog skill, go to the *Dialog* page.
2.  Open the dialog node where you want to call the action.
3.  From the *Assistant responds* section, click the menu ![Overflow menu](images/more-icon.png), and then choose **Open JSON editor**.

    ![Shows the Assistant responds section of a dialog node with the user selecting the Open JSON editor from the options icon.](images/open-json-editor.png)
4.  Add the action command JSON code block to the `generic` array. 


To add a JSON code block to a step in the Action, complete the following steps:

1.  From your action skill, open the step where you want to call the action.
2.  Click the `</>`  button to switch to the **JSON editor**.

### Applying a setting to the entire conversation
{: #dialog-voice-actions-welcome}

To apply a customization, such as changing the assistant's voice, from the very start of the conversation, specify the action command in a node that you add before the Welcome node. In the new node's *If assistant recognizes* field, add the `conversation_start` special condition. For more information, see [Starting the conversation](/docs/assistant?topic=assistant-dialog-start#dialog-start-welcome).
In the Action, specify the action command in `Customer starts with`.



## Sending a text message during a phone conversation
{: #dialog-voice-actions-sms}

Before you add in-context SMS messages, you must set up the *Twilio messaging* integration. For more information, see [Integrating with *Twilio messaging*](/docs/assistant?topic=assistant-deploy-sms).

There are lots of times when it is useful to be able to send a text message in the context of an ongoing conversation. For example, maybe you want the customer to share a street address. Rather than trying to transcribe the audio as a person rattles off an address and risk possible mistakes, you can ask the customer to send it in a text message instead.

Whenever you exchange a text with a customer in the context of a conversation, the dialog initiates the SMS message exchange. It sends a text message to the user and asks for the user to respond to it.

To send a specific message from a dialog node or a step in the Action, add the `vgwActSendSMS` command.

```json
{
  "output": {
    "generic": [
      {
        "response_type": "text",
        "values": [
          {
            "text": "I will send you a text message now."
          }
        ],
        "selection_policy": "sequential"
      },
      {
        "response_type": "user_defined",
        "user_defined": {
          "vgwAction": {
            "command": "vgwActSendSMS",
            "parameters": {
              "message": "Hey, this is Watson Assistant. To send me your street address, respond to this text message with your address."
            }
          }
        }
      }
    ]
  }
}
```
{: codeblock}

If your *SMS with Twilio* integration supports more than one SMS phone number, be sure to specify the phone number that you want to use to send the text message. Otherwise, the text is sent using the same phone number that was called.

The customer's reply text is sent in the `input.text`field. 


``` json
{
  "input": {
    "message_type": "text",
    "text": "1545 Lexington Ave."
  },
  "context": {
    "integrations": {
      "text_messaging": {
        "private": {
          "user_phone_number": "+18594213456"
        },
        "assistant_phone_number": "+18882346789"
      }
    }
  }
}
```
{: codeblock}


  
**Inbound context related to the text_messaging channel**

Note that in this case the `input.text` is assumed to hold the text input received directly from the SMS channel.

|context name | details | value | legacy mapping |
| :------ |:-----:|:-----:|:-----:|
| private.user_phone_number | Phone number associated with a user and the SMS related message. Note that this is marked as private to prevent this information from being saved with Session History.| string| n/a|
| assistant_phone_number | Phone number associated with the Watson Assistant side that received the SMS message. | string | n/a|


## Transferring a call to a human agent
{: #dialog-voice-actions-transfer}

When you configure the phone integration, you can optionally define a default message and routing information to transfer calls to human agents in a call center when a connection fails. If you want to define specific behavior for a given dialog branch, you can add the `Connect to human agent` response type to do so.

The message defined in `Response when agents are online` will be played to the caller before a transfer is initiated. Subsequently, if the call transfer fails, the message defined under `Response when no agents are online` will be synthesized and played to the caller.  The Phone Integration will not use `Message to human agent` field, but it will be previewed to the agent if the WebChat AgentApp is used.

You can add the phone integration specific parameters to the `Connect to human agent` response type using the JSON editor. When you click **Open JSON editor**, you should see something similar to the following example:
 
```json
{
  "output": {
    "generic": [
      {
        "response_type": "connect_to_agent",
        "transfer_info": {
          "target": {}
        },
        "agent_available": {
          "message": "I'll transfer you to an agent"
        },
        "agent_unavailable": {
          "message": "Sorry, I could not find an agent."
        }
      }
    ]
  }
}
```
{: codeblock}

The `Connect to human agent` response type supports the ability to specify the target transfer information under the `transfer_info` parameter.

A more complete example of a transfer **utilizing** all of the configurable parameters is:

  

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
                "uri": "sip:user\\@domain.com",
                "accept_transfer_reject_codes": [
                  "410",
                  "202"
                ],
                "transfer_headers": [
                  {
                    "name": "Customer-Header1",
                    "value": "Some-Custom-Info"
                  },
                  {
                    "name": "User-To-User",
                    "value": "XXXXXX"
                  }
                ],
                "transfer_headers_send_method": "refer_to_header"
              }
            }
          },
          "agent_available": {
            "message": "I'll transfer you to an agent"
          },
          "agent_unavailable": {
            "message": "Sorry, I could not find an agent."
          },
          "message_to_human_agent": "The caller needs help resetting their password"
        }
      }
    ]
  }
}
```
{: codeblock}

Full list of the phone integration parameters.


| Parameter | Default | Description |
| ---- | ---- | ---- |
| `service_desk.sip.uri` | N/A | The SIP or telephone URI to transfer the call to, such as `sip:12345556789\\@myhost.com` or `tel:+18883334444` |
| `service_desk.sip.notify_codes_to_accept` | N/A | A list of the error codes that are treated as a successful response when Voice Gateway processes NOTIFY requests during a call transfer. |
| `service_desk.sip.transfer_headers` | N/A | A list of custom header field name and value pairs to be added to a transfer request. |
| `service_desk.sip.transfer_headers_send_method` | `custom_header` | The method by which the SIP Transfer Headers are sent. <ul><li>`custom_header`: Sends the transfer headers as part of the SIP message. Default. </li><li>`contact_header`: Sends the transfer headers in the Contact Header field.</li><li>`refer_to_header`: Sends the transfer headers in the `Refer-To` header field.</li>|


If you define a SIP URI as the transfer target, escape the at sign (`@`) in the URI by adding two backslashes (`\\`) in front of it.

```json
    "uri": "sip:12345556789\\@myhost.com"
```
{: codeblock}



###  Passing Watson Assistant Metadata in SIP Signaling

Generally there's two kinds of metadata values to be aware of:

- `User-To-User`: This will be defined by Deb, it's custom user data that will be appended in the SIP REFER message used to transfer

- `X-Watson-Assistant-Session-History-Key`: Contains a key that can be used by the Web Chat agent app to obtain information about the call such as the Session History.

Also, the value of the SIP header will be restricted to 1024 bytes.

Additionally, how the data is presented in the SIP REFER message depends on the value of `transfer_headers_send_method`. For example:


####  `custom_header`

The metadata is appended to the SIP REFER message as headers:

```
REFER sip:b@atlanta.example.com SIP/2.0
Via: SIP/2.0/UDP agenta.atlanta.example.com;branch=z9hG4bK2293940223
To: <sip:b@atlanta.example.com>
From: <sip:a@atlanta.example.com>;tag=193402342
Call-ID: 898234234@agenta.atlanta.example.com
CSeq: 23 REFER
Max-Forwards: 7
Refer-To: sip:user@domain.com
X-Watson-Assistant-Token: 8f817472-8c57-4117-850d-fdf4fd23ba7
User-To-User: 637573746f6d2d757365722d746f2d75736572;encoding=hex
Contact: sip:a@atlanta.example.co
Content-Length: 0
```
{: codeblock}

####  `refer_to_header`

  The metadata is passed into the `Refer-To` header as query parameters per the [SIP RFC 3261](https://tools.ietf.org/html/rfc3261)

```
REFER sip:b@atlanta.example.com SIP/2.0
Via: SIP/2.0/UDP agenta.atlanta.example.com;branch=z9hG4bK2293940223
To: <sip:b@atlanta.example.com>
From: <sip:a@atlanta.example.com>;tag=193402342
Call-ID: 898234234@agenta.atlanta.example.com
CSeq: 23 REFER
Max-Forwards: 70
Refer-To: sip:user@domain.com?User-To-User=637573746f6d2d757365722d746f2d75736572%3Bencoding%3Dhex&X-Watson-Assistant-Token=8f817472-8c57-4117-850d-fdf4fd23ba79
Contact: sip:a@atlanta.example.com
Content-Length: 0

```
{: codeblock}

####  Domain Concepts


- `SIP URIs`: Typically SIP URIs are used for identifying SIP devices and generally follow the pattern: `sip:[user]@[hostname]`.

- `User-to-User Information`: Generally data that is used to identify a call accross multiple applications. Generally this is shared in a SIP header `User-To-User`, but there are other methods of sharing the data per [RFC7433](https://tools.ietf.org/html/rfc7433)

- `Notify Codes`: During a call transfer that through a SIP REFER, the calling device typically sends `SIP NOTIFY` events to let the service know how the transfer is progressing. There are cases where some call devices send non-OK codes (2xx) such as `410 Gone` that need to be accepted as 'successful' transfers.


## Playing hold music or a voice recording
{: #dialog-voice-actions-hold-music}

To play hold music or to play a recorded message, use the `audio` response type.

You cannot play hold music during a call transfer. But, you might want to play hold music if your dialog needs time to perform processing of some kind, such as calling a client-side action or making a call to a webhook. You can look for places where the dialog uses the *Pause* response type to find nodes where this response type might be useful.


You can specify the following parameter values for the `audio` response type:

- `Audio source`: URL to a publicly-accessible audio file. The audio file must be single channel (mono), PCM-encoded, and have a 8,000 Hz sampling rate with 16 bits per sample. The file format must be `.wav`.
- `Loop`: Select `On` or `Off` to indicate whether to repeatedly restart the audio play back after it finishes. The default value is `Off`.



## Applying advanced speech services to specific topics
{: #dialog-voice-actions-speech-advanced}

You can apply the following speech customizations to specific dialog nodes:

- [Use a custom language model](#dialog-voice-actions-custom-language)
- [Use a custom grammar](#dialog-voice-actions-custom-grammar)

To make any of these types of changes, edit the speech service configuration by adding the `speech_to_text` response type to your dialog or step in the Actions. The configuration decisions you make in the dialog node override the configuration that is specified in the integration setup page. The changes you apply persist for the remainder of the conversation, unless you override them again.


```json
{
  "output": {
    "generic": [
      {
        "response_type": "speech_to_text",
        "command_info": {
          "type": "<command type>",
          "parameters": {
            "parameter name1": "parameter value",
            "parameter name2": "parameter value"
          }
        }
      }
    ]
  }
}
```
{: codeblock}


Each command type along with its related parameters are described below.



### **command_info.type** : *configure*

Applies a set of parameters to pass to the {{site.data.keyword.speechtotextshort}} service. Allows a user to dynamically define the parameters based on the dialog flow. For instance, a user can choose a specific customization ID or grammar at a specific point in the dialog flow or step in the Action.

  
|parameter name|parameter value| required (yes/no) | default |
| :------ |:-----:| :-----: | :-----: |
|narrowband_recognize|{{site.data.keyword.speechtotextshort}} service when using narrowband audio. This config object is opaque to {{site.data.keyword.conversationshort}} and passed straight through to the {{site.data.keyword.speechtotextshort}} engine. For a full list of parameters see the [WebSockets API reference for {{site.data.keyword.speechtotextshort}} Service](https://cloud.ibm.com/apidocs/speech-to-text#websocket_methods). Note that {{site.data.keyword.conversationshort}} is only validating the key and not the value here. | no | Config defined via UI or environment variables. 
|broadband_recognize|{{site.data.keyword.speechtotextshort}} service when using broadband audio. This config object is opaque to {{site.data.keyword.conversationshort}} and passed straight through to the {{site.data.keyword.speechtotextshort}} engine. For a full list of parameters see the [WebSockets API reference for {{site.data.keyword.speechtotextshort}} Service](https://cloud.ibm.com/apidocs/speech-to-text#websocket_methods). Note that {{site.data.keyword.conversationshort}} is only validating the key and not the value here.| no | Config defined via UI or environment variables. |
|band_preference|Defines which audio band to prefer when negotiating audio codecs in the session (*narrowband* or *broadband*). Set to *broadband* to use broadband audio when possible.|no|*narrowband*|
|update_strategy|Specifies the update strategy to choose when setting the speech configuration. Possible values include: *replace*, *replace_once*, *merge* or *merge_once* | no | *replace* |


  
**Config Related Parameters**

The parameters that you can set under *narrowband_recognize* and *broadband_recognize* reflect the parameters that are made available by the {{site.data.keyword.speechtotextshort}} WebSocket interface. The WebSocket API sends two types of parameters: query parameters, which are sent when Voice Gateway connects to the service, and message parameters, which are sent as JSON after the connection is established. For example, *model* and *customization_id* are query parameters, and *smart_formatting* is a WebSocket message parameter. For a full list of parameters, see the WebSockets API reference for {{site.data.keyword.speechtotextshort}} Service.

  
You can define the following query parameters for the Media Relay's connection to the {{site.data.keyword.speechtotextshort}} service. Any other parameter that you define under *narrowband* or *broadband* is passed through on the WebSocket message request.
  

- model
- acoustic_customization_id
- version
- x-watson-learning-opt-out
- base_model_version
- language_customization_id

Note: The following parameters from the {{site.data.keyword.speechtotextshort}} service can't be modified because they have fixed values that are used by the Media Relay.

  
- action
- content-type
- interim_results
- continuous
- inactivity_timeout
  

**Using update_strategy**

  
You can use the update_strategy property in dynamic configuration to define how changes to the configuration are made, by either replacing the configuration or merging new configuration properties, and specifying whether these changes occur for the duration of the call or one conversation turn. This table describes this in more detail:

|value|description |
| :------ |:-----:|
|*replace*|Replaces the configuration for the duration of the call.|
|*replace_once*|Replaces the configuration once, so the configuration is used for only the following conversation turn. Then, it reverts to the previous configuration.|
|*merge*|Merges the configuration with the existing configuration for the duration of the call.|
|*merge_once*|Merges the configuration for one turn of the conversation, and then reverts to the previous configuration.|

  


**Updating fields that are not root level**

  
When configuring dynamically from {{site.data.keyword.conversationshort}} using this speech command, it's important to note that only the root level fields, such as *narrowband* or *broadband*, are updated. If they are omitted from the action, the original configuration settings persist. You can use the different **update_strategy** properties for *merge* and *merge_once* to merge config fields with the existing configuration.


  

###  Examples

**Example: Setting the {{site.data.keyword.speechtotextshort}} language model to Spanish (es-ES_NarrowbandModel)**


In this example, the language model is switched to Spanish and smart formatting is enabled.


```json

{
  "output": {
    "generic": [
      {
        "response_type": "speech_to_text",
        "command_info": {
          "type": "configure",
          "parameters": {
            "narrowband_recognize": {
              "model": "es-ES_NarrowbandModel",
              "smart_formatting": true
            }
          }
        }
      }
    ]
  }
}

```
 {: codeblock}

**Example: Using the merge_once method to update the {{site.data.keyword.speechtotextshort}} recognizeBody property**


The following example shows how to specify the use of a customization language model for a single conversational turn. This is done through the use of update_strategy set to merge_once and specifying in the configuration parameters the id of the custom language model.

  

```json
{
  "output": {
    "generic": [
      {
        "response_type": "speech_to_text",
        "command_info": {
          "type": "configure",
          "parameters": {
            "update_strategy": "merge_once",
            "narrowband_recognize": {
              "customization_id": "ao45vohgFuxyOQRgztu-02I10ut7aJcM-AdInT-VWgj3V"
            }
          }
        }
      }
    ]
  }
}

```
{: codeblock}


**Example: Using a custom grammar**

The {{site.data.keyword.speechtotextshort}} service supports the use of grammars. A grammar allows you to configure the audio to match specific characteristics only.

You can think of it this way: 

- A custom language model expands the service's base vocabulary.
- A grammar restricts the words that the service can recognize from that vocabulary. 

When you use a grammar with a custom language model for speech recognition, the service can recognize only words, phrases, and strings that are recognized by the grammar. For example, maybe you want to accept only a yes or no response. You can define a grammar that allows only those options.

For more information, see [Using grammars with custom language models](/docs/speech-to-text?topic=speech-to-text-grammars){: external}.

To specify a custom grammar for a dialog node, add the following command: 

```json
{
  "output": {
    "generic": [
      {
        "response_type": "speech_to_text",
        "command_info": {
          "type": "configure",
          "parameters": {
            "update_strategy": "merge_once",
            "narrowband_recognize": {
              "x-watson-learning-opt-out": true,
              "grammar_name": "names-abnf",
              "language_customization_id": "81d3630-ba58-11e7-aa4b-41bcd3f6f24d"
            }
          }
        }
      }
    ]
  }
}
```
{: codeblock}


## Applying advanced {{site.data.keyword.texttospeechshort}} services to specific topics ](#dialog-voice-actions-text-advanced)


You can apply a set of parameters to pass to the {{site.data.keyword.texttospeechshort}} service. {{site.data.keyword.conversationshort}} allows a user to dynamically define the parameters based on the dialog flow. For instance, you can choose a specific voice at a specific point in the dialog flow or step in the Action. 


```json

{
  "output": {
    "generic": [
      {
        "response_type": "text_to_speech",
        "command_info": {
          "type": "<command type>",
          "parameters": {
            "parameter name": "parameter value",
            "parameter name": "parameter value"
          }
        }
      }
    ]
  }
}
```  
{: codeblock}

Each command type along with its related parameters are described below.

  

### **command_info.type** : *configure*
  

Applies a set of parameters to pass to the {{site.data.keyword.texttospeechshort}} service. Watson Assistant allows a user to dynamically define the parameters based on the dialog flow. For instance, you can choose a specific voice at a specific point in the dialog flow or step in the Action.

  

|parameter name|parameter value| required (yes/no) | default |
| :------ |:-----:| :-----: | :-----: |
|synthesize|Parameters for the {{site.data.keyword.texttospeechshort}} service. See [WebSockets API reference for Watson Text to Speech Service](https://cloud.ibm.com/apidocs/text-to-speech#websocket_methods).Note that WA is only validating the key and not the value here.|yes|n/a|
|update_method|Specifies the update strategy to choose when setting the speech configuration. Possible values include: *replace*, *replace_once*, *merge* or *merge_once* | no | *replace* |

  

**Using update_strategy**

You can use the update_method property in dynamic configuration to define how changes to the configuration are made, by either replacing the configuration or merging new configuration properties, and specifying whether these changes occur for the duration of the call or one conversation turn. This table describes this in more detail:

  
|value|description |
| :------ |:-----:|
|*replace*|Replaces the configuration for the duration of the call.|
|*replace_once*|Replaces the configuration once, so the configuration is used for only the following conversation turn. Then, it reverts to the previous configuration.|
|*merge*|Merges the configuration with the existing configuration for the duration of the call.|
|*merge_once*|Merges the configuration for one turn of the conversation, and then reverts to the previous configuration.|

  

**Config Related Parameters**


The parameters that you can set under *synthesize* reflect the parameters that are made available by the Text To Speech WebSocket interface. The WebSocket API sends two types of parameters: query parameters, which are sent when Voice Gateway connects to the service, and message parameters, which are sent as JSON after the connection is established. For a full list of parameters, see the WebSockets API reference for {{site.data.keyword.texttospeechshort}} service.

  

### **command_info.type** : *disable_barge_in*

Disables speech barge-in so that playback isn't interrupted when the caller speaks over top of the played back audio.

No parameters.

  

### **command_info.type** : *enable_barge_in*

Enables speech barge-in so that callers can interrupt playback by speaking.

No parameters.
  

  

###  Examples

  

**Example: Setting the {{site.data.keyword.texttospeechshort}} voice (en-US_LisaVoice)**

  

In this example, the voice is set to en-US_LisaVoice.

```json

{
  "output": {
    "generic": [
      {
        "response_type": "text_to_speech",
        "command_info": {
          "type": "configure",
          "parameters": {
            "synthesize": {
              "voice": "en-US_LisaVoice"
            }
          }
        }
      }
    ]
  }
}

```
{: codeblock}

In the `voice` parameter, specify the voice model that you want to use. For more information about voice model options, see [Supported languages and voices](/docs/text-to-speech?topic=text-to-speech-voices#languageVoices){: external}.

The model you specify must be one that is supported by the {{site.data.keyword.texttospeechshort}} service instance that is configured for use with the integration.
{: note}

The custom voice that you specify for this dialog branch is used by each subsequent dialog node that is processed during the session unless you add another `text_to_speech` response type to reset the voice model back to the default model.

## Enabling keypad entry
{: #dialog-voice-actions-dtmf}

If you want customers to be able to send information by typing it on their phone keypad instead of speaking, you can add support for phone keypad entry. The best way to implement this type of support is to enable dual-tone multifrequency (DTMF) signaling. DTMF is a system that interprets the tones that are generated when a user presses keys on a push-button phone. The tones have a specific frequency and duration that can be interpreted by the phone network.

To start listening for tones as the user presses phone keys, add the `dtmf` response type to the dialog node:

```json
{
  "output": {
    "generic": [
      {
        "response_type": "dtmf",
        "command_info": {
          "type": "<command type>",
          "parameters": {
            "parameter name": "parameter value",
            "parameter name": "parameter value"
          }
        },
        "channels": [
          {
            "channel": "voice_telephony"
          }
        ]
      }
    ]
  }
}
```
{: codeblock}

Each command type along with its related parameters are described below.

### **command_info.type** : *collect*

Instructs to collect dual-tone multi-frequency signaling (DTMF) input from a user.


|parameter name|details| required (yes/no) | default |
| :------ |:-----:| :-----: | :-----: |
| termination_key | The DTMF termination key, which signals the end of DTMF input. For example, "#". | no | n/a |
| count | The number of DTMF digits to collect. | Only required if termintation_key or minimum_count and maximum_count aren't defined. Must be a positive integer and be a number no larger than 100. | n/a|
| minimum_count | The minimum number of DTMF digits to collect when the DTMF collection is setup to accept a range of entries. Must be a positive integrer with a minimum value of 1 and a maximum value less than *maximum_count*. | Only required if terminatation_key or count is not defined. | n/a|
| maximum_count | The maximum number of DTMF digits to collect when the DTMF collection is setup to accept a range of entries. When this number of digits is collected, a conversation turn is initiated. Must be a positive integer and be a number no larger than 100.| Only required if termintation_key or count is not defined. | n/a|
| inter_digit_timeout_count | The amount of time in milliseconds to wait for a new DTMF digit after a DTMF digit is received. During an active DTMF collection, this timeout activates when the first DTMF collection is received. When the inter-digit timeout is active, it deactivates the post response timeout timer. If the inter_digit_timeout_count parameter isn't specified, the post response timer resets after receiving each DTMF digit, and stays active until either the post response timeout count is met or the collection completes. This value is a positive integer and should be set no higher than 100000 (or 100 secs). | no | n/a |
|ignore_speech | When set to true, speech is automatically disabled on the reception of a DTMF digit until either the collection completes or a timeout occurs. | no | false |
| stop_after_collection | Indicates whether to *stop* DTMF input when the DTMF collection completes. All DTMF input is ignored until it's reenabled by the *start* response_type. | no | false |

  

###  **command_info.type** : *disable_barge_in*

Disables DTMF barge-in so that playback from the Phone integration isn't interrupted when callers press keys. Keys pressed during playback will be ignored if this is enabled.


No parameters.
  

### **command_info.type** : *enable_barge_in*

Enables DTMF barge-in so that callers can interrupt playback from the Phone integration by pressing a key.
  

No parameters.
  

###  **command_info.type** : *send*

This command provides a way to send DTMF outbound.

  
Parameter names:

|parameter name|details| required (yes/no) | default |
| :------ |:-----:| :-----: | :-----: |
| digits | An array of JSON objects where each element represents a DTMF tone to be sent to a caller. See next table for the supported DTMF object attributes. | yes | n/a |
| send_interval | An interval in milliseconds (ms) to wait before sending the next DTMF tone in the list. | no | 200 |

  

`digit` object attributes:

 
|parameter name|details| required (yes/no) | default 
| :------ |:-----:| :-----: | :-----: 
| code | The event code to send. The supported range is 0 - 15. See table below for the supported DTMF codes. | yes | n/a 
| duration | The duration of the event in milliseconds (ms). | no | 200 
| volume | The power level of the tone in dBm0. The supported range is 0 to -63 dBm0. | no | 0 |  


Supported event codes:

 
|event|code 
| :------ | :-----: |
| 0 - 9 | 0 - 9 
| * | 10 |
| # | 11 |
| A - D | 12 - 15 |

  

  

###  Examples

  

**Example of a dtmf collect response:**

  

```json

{
  "output": {
    "generic": [
      {
        "response_type": "dtmf",
        "command_info": {
          "type": "collect",
          "parameters": {
            "termination_key": "#",
            "count": "16",
            "ignore_speech": true
          }
        },
        "channels": [
          {
            "channel": "voice_telephony"
          }
        ]
      }
    ]
  }
}

```
{: codeblock}

**Example of a dtmf send response:**

  

```json

{
  "output": {
    "generic": [
      {
        "response_type": "dtmf",
        "command_info": {
          "type": "send",
          "parameters": {
            "digits": [
              {
                "code": "9",
                "volume": -8
              },
              {
                "code": "11"
              }
            ],
            "send_interval": 100
          }
        },
        "channels": [
          {
            "channel": "voice_telephony"
          }
        ]
      }
    ]
  }
}

```
{: codeblock}



## End the call
{: #dialog-voice-actions-hangup}

To have the assistant end the call, use the `end_session` command.

```json
{
  "output": {
    "generic": [
      {
        "response_type": "end_session"
      }
    ]
  }
}
```
{: codeblock}


## Adding actions to your dialog or action 
{: #dialog-voice-legacy-actions-add}

When calling voice-specific actions from a dialog node or a step in the Action, you need to define the action within the `generic` array using a `JSON editor`. 

    For example:

```json
{
  "generic": [
    {
      "values": [
    {
      "response_type": "user_defined",
      "user_defined": {
        "vgwAction": {
          "command": "<action name>"
        }
      }
    }
  ]
}   
 ```
 {: codeblock}
 
 
 ```json
{
  "output": {
    "generic": [
      {
        "response_type": "user_defined",
        "user_defined": {
          "vgwAction": {
            "command": "vgwActSendSMS",
            "parameters": {
              "message": "Hey, this is Watson Assistant. To send me your street address, respond to this text message with your address."
            }
          }
        }
      }
    ]
  }
}
```
 {: codeblock}



## Defining a sequence of actions
{: #dialog-voice-actions-sequence}

If you want to run more than one command in succession, you can have multiple response_types in the **generic** array.

```json
{
  "output": {
    "generic": [
      {
        "values": [
          {
            "text": "Goodbye."
          }
        ],
        "response_type": "text",
        "selection_policy": "sequential"
      },
      {
        "response_type": "end_session"
      }
    ]
  }
}
```
{: codeblock}
