---

copyright:
  years: 2020, 2021
lastupdated: "2021-11-03"

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

{{site.data.content.newlink}}

# Handling phone interactions
{: #dialog-voice-actions}

If your assistant uses the phone integration, you can use various response types to customize the behavior of the integration or manage the flow of conversations that your assistant has with customers over the telephone.
{: shortdesc}

You can use response types to perform the following phone-specific actions:

- [Apply advanced settings to the {{site.data.keyword.speechtotextshort}} service](#dialog-voice-actions-speech-advanced)
- [Apply advanced settings to the {{site.data.keyword.texttospeechshort}} service](#dialog-voice-actions-text-advanced)
- [Transfer a call to a human agent](#dialog-voice-actions-transfer)
- [Play hold music or a voice recording](#dialog-voice-actions-hold-music)
- [Enable keypad entry](#dialog-voice-actions-dtmf)
- [Transfer the caller to the web chat integration](#dialog-voice-actions-transfer-channel)
- [End the call](#dialog-voice-actions-hangup)
- [Send a text message during a phone conversation](#dialog-voice-actions-sms)

In some cases, you might want to combine response types to perform multiple actions. For example, you might want to implement two-factor authentication by requesting phone keypad entry and sending a text message from the same dialog node or step. For more information, see [Defining a sequence of phone actions](#dialog-voice-actions-sequence).

For reference information about phone-specific repsonse types and related context variables, see [Phone context variables](/docs/assistant?topic=assistant-phone-context).

## Adding phone-specific responses to your dialog or actions 
{: #dialog-voice-actions-add}

To initiate a voice-specific interaction from a dialog node or a step in an action, add a response within the `output.generic` array using the appropriate response type.

Although many response types can be specified using the {{site.data.keyword.conversationshort}} user interface, phone-specific response types must currently be added using the JSON editor.
{: note}

For more information about using the JSON editor to add responses, see [Defining responses using the JSON editor](/docs/assistant?topic=assistant-dialog-responses-json).

## Applying advanced settings to the {{site.data.keyword.speechtotextshort}} service
{: #dialog-voice-actions-speech-advanced}

Use the `speech_to_text` response type to send configuration commands to the {{site.data.keyword.speechtotextshort}} service instance used by the phone integration. By sending a `speech_to_text` response from a dialog node or action step, you can dynamically change the {{site.data.keyword.speechtotextshort}} configuration during a conversation. 

By default, any {{site.data.keyword.speechtotextshort}} configuration changes you make persist for the remainder of the conversation, or until you update them again. You can change this behavior by specifying the `update_strategy` property of the `parameters` object.

The format of the `speech_to_text` response type is as follows:

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

Each command type along with its related parameters are described in the following sections.

### `command_info.type` : `configure`

Dynamically reconfigures the {{site.data.keyword.speechtotextshort}} service by applying a set of configuration parameters, which can be based on the dialog or action flow. For example, you might want to choose a particular customization ID or grammar at a specific point in the conversation.

| parameter | description | required | default |
| ----------|-------------|----------|---------|
| `narrowband_recognize` | The {{site.data.keyword.speechtotextshort}} service configuration to use for narrowband codecs (such as PCMU and PCMA, which are sampled at 8 kHz). The parameters defined by this object are used when connecting to the {{site.data.keyword.speechtotextshort}} service for speech recognition requests. For more information about these parameters, see the [{{site.data.keyword.speechtotextshort}} API documentation](/apidocs/speech-to-text#WSRecognizeMethod){: external}. | no | Current {{site.data.keyword.speechtotextshort}} configuration | 
| `broadband_recognize` | The {{site.data.keyword.speechtotextshort}} service configuration to use for broadband codecs (such as G722, which is sampled at 8 kHz). The parameters defined by this object are used when connecting to the {{site.data.keyword.speechtotextshort}} service for speech recognition requests. For more information about these parameters, see the [{{site.data.keyword.speechtotextshort}} API documentation](/apidocs/speech-to-text#WSRecognizeMethod){: external}. | no | Current {{site.data.keyword.speechtotextshort}} configuration |
| `band_preference` | Specifies which audio band (`narrowband` or `broadband`) is preferred when negotiating audio codecs for the session. Set to `broadband` to use broadband audio when possible. | no | `narrowband` |
| `update_strategy` | Specifies the update strategy to use when setting the speech configuration. Possible values include: \n - `replace`: Replaces the configuration for the rest of the session. Any root-level fields in the new configuration completely overwrite the previous configuration. \n - `replace_once`: Replaces the configuration only for the next turn of the conversation. Subsequently, the previous configuration is used. \n - `merge`: Merges the new configuration with the existing configuration for the rest of the session. Only changed parameters are overwritten; any other configuration parameters are unchanged. \n - `merge_once`: Merges the new configuration with the existing configuration only for the next turn of the conversation. Subsequently, the previous configuration is used. | no | `replace` |
  
The parameters that you can set for `narrowband_recognize` and `broadband_recognize` reflect the parameters that are made available by the {{site.data.keyword.speechtotextshort}} WebSocket interface. The WebSocket API sends two types of parameters: query parameters, which are sent when the phone integration connects to the service, and message parameters, which are sent as part of the JSON data in the request body. For example, `model` is a query parameter, and `smart_formatting` is a WebSocket message parameter. For a full list of parameters, see the [{{site.data.keyword.speechtotextshort}} API documentation](/apidocs/speech-to-text){: external}.
  
You can define the following query parameters for the phone integration's connection to the {{site.data.keyword.speechtotextshort}} service. Any other parameter that you define for `narrowband` or `broadband` is passed through as part of the WebSocket message request.
  
- `model`
- `acoustic_customization_id`
- `version`
- `x-watson-learning-opt-out`
- `base_model_version`
- `language_customization_id`

The following parameters from the {{site.data.keyword.speechtotextshort}} service can't be modified because they have fixed values that are used by the phone integration.
  
- `action`
- `content-type`
- `interim_results`
- `continuous`
- `inactivity_timeout`
  
When configuring dynamically from {{site.data.keyword.conversationshort}} using the `configure` command, note that only the root level fields, such as `narrowband` or `broadband`, are updated. If these fields are omitted from the command, the original configuration settings persist. You can use the `update_strategy` values `merge` and `merge_once` to merge configuration parameters with the existing configuration.
{: note}

### Using a custom language model 
{: #dialog-voice-actions-custom-language}

When you set up the phone integration, you can configure the integration to use a custom language model all the time. However, you might want to use a standard language model most of the time, and specify a custom language model to use only for specific topics that your assistant is designed to help customers with. For example, you might want to use a custom model that specializes in medical terms for a dialog branch that helps with medical bills only. You can apply a custom language model for a specific dialog node or action step. 

For more information, about custom language models, see [Creating a custom language model](/docs/speech-to-text?topic=speech-to-text-languageCreate){: external}.

To apply a custom language model to a specific dialog node or action step, use the `speech_to_text` command.

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
              "x-watson-learning-opt-out": true,
              "model": "en-US_NarrowbandModel",
              "profanity_filter": true,
              "smart_formatting": true,
              "language_customization_id": "81d3630-ba58-11e7-aa4b-41bcd3f6f24d",
              "acoustic_customization_id": "e4766090-ba51-11e7-be33-99bd3ac8fa93"
            }
          }
        }
      }
    ]
  }
}
```
{: codeblock}

You can also apply an acoustic model that you might have trained to deal with background noise, accents, or other things that are associated with the quality or noise of the signal.

### Using a custom grammar
{: #dialog-voice-actions-custom-grammar}

The {{site.data.keyword.speechtotextshort}} service supports the use of grammars. A grammar allows you to configure the audio to match specific characteristics only.

You can think of it this way: 

- A custom language model expands the service's base vocabulary.
- A grammar restricts the words that the service can recognize from that vocabulary. 

When you use a grammar with a custom language model for speech recognition, the service can recognize only words, phrases, and strings that are recognized by the grammar. For example, maybe you want to accept only a yes or no response. You can define a grammar that allows only those options.

For more information, see [Using grammars with custom language models](/docs/speech-to-text?topic=speech-to-text-grammars){: external}.

This example shows how to specify a custom grammar during the conversation:

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

###  Examples

The following examples illustrate how to use the `speech_to_text` response type to send configuration commands to the {{site.data.keyword.speechtotextshort}} service.

#### Example: Setting the language model

In this example, the language model is switched to Spanish (`es-ES_NarrowbandModel`), and smart formatting is enabled.

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

#### Example: Updating the `recognizeBody` property for one conversation turn

The following example shows how to specify the use of a custom language model for a single turn of the conversation turn. This is done by setting `update_strategy` to `merge_once` and specifying the the ID of the custom language model in the configuration parameters.

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
              "language_customization_id": "ao45vohgFuxyOQRgztu-02I10ut7aJcM-AdInT-VWgj3V" 
            }
          }
        }
      }
    ]
  }
}

```
{: codeblock}

## Applying advanced settings to the {{site.data.keyword.texttospeechshort}} service
{: #dialog-voice-actions-text-advanced}

Use the `text_to_speech` response type to send configuration commands to the {{site.data.keyword.texttospeechshort}} service instance used by the phone integration. By sending a `text_to_speech` response from a dialog node or action step, you can dynamically change the {{site.data.keyword.texttospeechshort}} configuration during a conversation.

By default, any {{site.data.keyword.texttospeechshort}} configuration changes you make persist for the remainder of the conversation, or until you update them again. You can change this behavior by specifying the `update_strategy` property of the `parameters` object.

The format of the `speech_to_text` response type is as follows:

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

Each command type along with its related parameters are described in the following sections.

### **command_info.type** : `configure`

Dynamically reconfigures the {{site.data.keyword.texttospeechshort}} service by applying a set of configuration parameters, which can be based on the dialog or action flow. For example, you might want to choose a particular voice at a specific point in the conversation.

| parameter | description | required | default |
| --------- |-------------|----------|---------|
| `synthesize` | The {{site.data.keyword.texttospeechshort}} service configuration to use when synthesizing audio. The parameters defined by this object are used when connecting to the {{site.data.keyword.texttospeechshort}} service for speech synthesis requests. For more information about these parameters, see the [{{site.data.keyword.texttospeechshort}} API documentation](/apidocs/text-to-speech#synthesize-audio-websockets-){: external}. | yes | Current {{site.data.keyword.texttospeechshort}} configuration |
| `update_strategy` | Specifies the update strategy to use when setting the speech configuration. Possible values include: \n - `replace`: Replaces the configuration for the rest of the session. Any root-level fields in the new configuration completely overwrite the previous configuration. \n - `replace_once`: Replaces the configuration only for the next turn of the conversation. Subsequently, the previous configuration is used. \n - `merge`: Merges the new configuration with the existing configuration for the rest of the session. Only changed parameters are overwritten; any other configuration parameters are unchanged. \n - `merge_once`: Merges the new configuration with the existing configuration only for the next turn of the conversation. Subsequently, the previous configuration is used. | no | `replace` |

The parameters that you can set for `synthesize` reflect the parameters that are made available by the {{site.data.keyword.texttospeechshort}} WebSocket interface. The WebSocket API sends two types of parameters: query parameters, which are sent when phone integration connects to the service, and message parameters, which are sent as part of the JSON data in the request body. For a full list of parameters, see the [{{site.data.keyword.texttospeechshort}} API documentation](/apidocs/text-to-speech){: external}.

### `command_info.type` : `disable_barge_in`

Disables speech barge-in so that playback isn't interrupted when the caller speaks while audio is being played back.

No parameters.  

### `command_info.type` : `enable_barge_in`

Enables speech barge-in so that callers can interrupt playback by speaking.

No parameters.

### Change the assistant's voice
{: #dialog-voice-actions-change-voice}

You can change the voice of your assistant when it covers certain topics in the conversation that warrant it. For example, you might want to use a voice with a British accent for a branch of the conversation that applies only to customers in the UK.

This example shows how to specify a voice during the conversation:

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
              "voice": "en-GB_KateV3Voice"
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

## Transferring a call to a human agent
{: #dialog-voice-actions-transfer}

When you configure the phone integration, you can optionally set up backup call center support, which makes it possible for the assistant to transfer a call to a human agent. You can use the *Connect to human agent* response type in a dialog node or action step to initiate a transfer to a human agent at a specific point in the conversation. When a *Connect to human agent* response is sent to the phone integration, a SIP transfer is initiated using the SIP `REFER` message, as defined by [RFC 5589](https://datatracker.ietf.org/doc/html/rfc5589#section-6.1){: external}.

For more information about initiating a transfer to a human agent during the conversation, see the following documentation:
- [Adding *Connect to human agent* response type](/docs/assistant?topic=assistant-dialog-overview#dialog-overview-add-connect-to-human-agent) (dialog node)
- [Deciding what to do next](/docs/assistant?topic=assistant-actions#actions-what-next) (action step)

The phone integration supports additional parameters for the *Connect to human agent* response type. You can add these phone-specific parameters to the `connect_to_agent` response type using the JSON editor.

The `connect_to_agent` response type supports the ability to specify the target transfer information under the `transfer_info` parameter.

The following example shows a transfer that uses all of the configurable parameters:

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
                "transfer_headers": [
                  {
                    "name": "Customer-Header1",
                    "value": "Some-Custom-Info"
                  },
                  {
                    "name": "User-to-User",
                    "value": "XXXXXX"
                  }
                ],
                "transfer_headers_send_method": "refer_to_header"
              }
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
    ]
  }
}
```
{: codeblock}

The `connect_to_agent` response type supports the following phone-specific properties.

| Parameter | Default | Description |
|-----------|---------|-------------|
| `service_desk.sip.uri` | N/A | The SIP or telephone URI to transfer the call to, such as `sip:12345556789\\@myhost.com` or `tel:+18883334444` |
| `service_desk.sip.transfer_headers` | N/A | A list of custom header field name/value pairs to be added to a transfer request |
| `service_desk.sip.transfer_headers_send_method` | `custom_header` | The method by which the SIP transfer headers are sent: \n - `custom_header`: Sends the transfer headers as part of the SIP message. This is the default value. \n - `contact_header`: Sends the transfer headers in the `Contact` header. \n - `refer_to_header`: Sends the transfer headers in the `Refer-To` header. |

If you define a SIP URI as the transfer target, escape the at sign (`@`) in the URI by adding two backslashes (`\\`) in front of it. This is to prevent the string from being recognized as part of the entity shorthand syntax.

```json
    "uri": "sip:12345556789\\@myhost.com"
```
{: codeblock}

###  Passing Watson Assistant Metadata in SIP Signaling

To support loading the conversational history between the caller and {{site.data.keyword.conversationshort}}, the phone integration specifies a value for the `User-to-User` header as a key that can be used with the web chat integration. If `User-to-User` is specified in the `transfer_headers` list, the session history key is sent in the `X-Watson-Assistant-Session-History-Key` header.

The value of the SIP header is limited to 1024 bytes.

How this data is presented in the SIP `REFER` message also depends on the value of `transfer_headers_send_method`(as defined in [Generic Service Desk SIP Parameters](#generic-service-desk-sip-parameters)). 

The following example shows the data included as headers:

```text
REFER sip:b@atlanta.example.com SIP/2.0
Via: SIP/2.0/UDP agenta.atlanta.example.com;branch=z9hG4bK2293940223
To: <sip:b@atlanta.example.com>
From: <sip:a@atlanta.example.com>;tag=193402342
Call-ID: 898234234@agenta.atlanta.example.com
CSeq: 23 REFER
Max-Forwards: 7
Refer-To: sip:user@domain.com
X-Watson-Assistant-Token: 8f817472-8c57-4117-850d-fdf4fd23ba7
User-to-User: dev::latest::212033::0a64c30d-c558-4055-85ad-ef75ad6cc29d::978f1fd7-4e24-47d8-adb0-24a8a6eff69e::b5ffd6c2-902f-4658-b586-e3fc170a6cf3::7ad616a350cc48078f17e3ee3df551de;encoding=ascii
Contact: sip:a@atlanta.example.com
Content-Length: 0
```
{: codeblock}


If a custom `User-to-User` header is specified, then the session history key is set in the `X-Watson-Assistant-Session-History-Key` header:

```text
REFER sip:b@atlanta.example.com SIP/2.0
Via: SIP/2.0/UDP agenta.atlanta.example.com;branch=z9hG4bK2293940223
To: <sip:b@atlanta.example.com>
From: <sip:a@atlanta.example.com>;tag=193402342
Call-ID: 898234234@agenta.atlanta.example.com
CSeq: 93809823 REFER
Max-Forwards: 70
Refer-To: sip:user@domain.com
User-to-User: 637573746f6d2d757365722d746f2d75736572;encoding=hex;
X-Watson-Assistant-Session-History-Key: dev::latest::212033::0a64c30d-c558-4055-85ad-ef75ad6cc29d::978f1fd7-4e24-47d8-adb0-24a8a6eff69e::b5ffd6c2-902f-4658-b586-e3fc170a6cf3::7ad616a350cc48078f17e3ee3df551de
Contact: sip:a@atlanta.example.com
Content-Length: 0
```
{: codeblock}

This example shows the metadata passed into the `Refer-To` header as query parameters (as defined by [SIP RFC 3261](https://tools.ietf.org/html/rfc3261){: external}).

```text
REFER sip:b@atlanta.example.com SIP/2.0
Via: SIP/2.0/UDP agenta.atlanta.example.com;branch=z9hG4bK2293940223
To: <sip:b@atlanta.example.com>
From: <sip:a@atlanta.example.com>;tag=193402342
Call-ID: 898234234@agenta.atlanta.example.com
CSeq: 23 REFER
Max-Forwards: 70
Refer-To: sip:user@domain.com?User-to-User=dev::latest::893499::dff9c274-adc4-4f63-93de-781166760bf8::978f1fd7-4e24-47d8-adb0-24a8a6eff69e::b5ffd6c2-902f-4658-b586-e3fc170a6cf3::7ad616a350cc48078f17e3ee3df551de%3Bencoding%3Dascii
Contact: sip:a@atlanta.example.com
Content-Length: 0
```
{: codeblock}

If a custom `User-to-User` header is specified, then the session history key is set in the `X-Watson-Assistant-Session-History-Key` header.

```text
REFER sip:b@atlanta.example.com SIP/2.0
Via: SIP/2.0/UDP agenta.atlanta.example.com;branch=z9hG4bK2293940223
To: <sip:b@atlanta.example.com>
From: <sip:a@atlanta.example.com>;tag=193402342
Call-ID: 898234234@agenta.atlanta.example.com
CSeq: 93809823 REFER
Max-Forwards: 70
Refer-To: sip:user@domain.com?User-to-User=637573746f6d2d757365722d746f2d75736572%3Bencoding%3Dhe&X-Watson-Assistant-Session-History-Key=dev::latest::893499::dff9c274-adc4-4f63-93de-781166760bf8::978f1fd7-4e24-47d8-adb0-24a8a6eff69e::b5ffd6c2-902f-4658-b586-e3fc170a6cf3::7ad616a350cc48078f17e3ee3df551de
Contact: sip:a@atlanta.example.com
Content-Length: 0

```
{: codeblock}

## Playing hold music or a voice recording
{: #dialog-voice-actions-hold-music}

To play hold music or to play a recorded message, use the `audio` response type. This response type can be specified in a dialog node or action step using the JSON editor.

You cannot play hold music during a call transfer. However, you might want to play hold music if your assistant needs time to perform processing of some kind, such as calling a client-side action or making a call to a webhook. 


The phone integration supports the following properties for the `audio` response type:

| Property | Description |
|----------|-------------|
| `source` | The URL of a publicly-accessible `.wav` audio file. The audio file must be single channel (mono) and PCM-encoded, and must have an 8,000 Hz sampling rate with 16 bits per sample. |
| `channel_options.voice_telephony.loop` | Whether to repeatedly restart the audio playback after it finishes. The default value is `false`. |

If you set `channel_options.voice_telephony.loop` to `true`, add a user-defined response with the `vgwActForceNoInputTurn` command. This command instructs the phone integration to initiate a turn with a `vgwNoInputTurn` text without waiting for an input from the caller. In the `vgwNoInputTurn` turn you can initiate a transaction while the caller is on hold. When the `vgwNoInputTurn` turn completes, the looped audio stops.

The following example shows an `audio` response with `loop`=`true`, and a `user_defined` response specifying the `vgwActForceNoInputTurn` command.

```json
{
  "output": {
    "generic": [
      {
        "user_defined": {
          "vgwAction": {
            "command": "vgwActForceNoInputTurn"
          }
        },
        "response_type": "user_defined"
      },
      {
        "source": "https://upload.wikimedia.org/wikipedia/commons/d/d8/Random_composition3.wav",
        "response_type": "audio",
        "channel_options": {
          "voice_telephony": {
            "loop": true
          }
        }
      }
    ]
  }
}
```
{: codeblock}

## Enabling keypad entry
{: #dialog-voice-actions-dtmf}

If you want customers to be able to send information by typing it on their phone keypad instead of speaking, you can add support for phone keypad entry. The best way to implement this type of support is to enable dual-tone multifrequency (DTMF) signaling. DTMF is a protocol used to transmit tones that are generated when a user presses keys on a push-button phone. The tones have a specific frequency and duration that can be interpreted by the phone network.

To start listening for tones as the user presses phone keys, use the `dtmf` response type in a dialog node or action step. This response type can be added using the JSON editor.

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

The `command_info` property specifies a DTMF command for the phone integration. The supported commands and their related parameters are as follows.

### `command_info.type` : `collect`

Instructs the phone integration to collect dual-tone multi-frequency signaling (DTMF) input from a user. This command supports the following parameters:

| parameter name | description | required | default |
|----------------|-------------|----------|---------|
| `termination_key` | The DTMF termination key, which signals the end of DTMF input (for example, `#`). | no | n/a |
| `count` | The number of DTMF digits to collect. This must be a positive integer no larger than 100. | Required if `termintation_key`, or `minimum_count` and `maximum_count`, are not defined | n/a |
| `minimum_count` | The minimum number of DTMF digits to collect. This property is used along with `maximum_count` to define a range for the number of digits to collect. This value must be a positive integer with a minimum value of 1 and a maximum value less than `maximum_count`. | Required if `terminatation_key` and `count` are not defined. | n/a |
| `maximum_count` | The maximum number of DTMF digits to collect. This property is used along with `minimum_count` to define a range for the number of digits to collect. When this number of digits is collected, a conversation turn is initiated. This value must be a positive integer no greater than 100. | Required if `termintation_key` and `count` are not defined. | n/a |
| `inter_digit_timeout_count` | The amount of time (in milliseconds) to wait for a new DTMF digit after a DTMF digit is received. During an active DTMF collection, this timeout activates when the first DTMF collection is received. When the inter-digit timeout is active, it deactivates the post-response timeout timer. If the `inter_digit_timeout_count` parameter is not specified, the post-response timer resets after receiving each DTMF digit, and it stays active until either the post-response timeout count is met or the collection completes. This value is a positive integer no higher than 100,000 (or 100 seconds). | no | n/a |
| `ignore_speech` | Whether to disable speech recognition during collection of DTMF digits, until either the collection completes or a timeout occurs. | no | false |
| `stop_after_collection` | Whether to stop DTMF input when the DTMF collection completes. After this command, all DTMF input is ignored until it is reenabled using the `start` response type. | no | false |

###  `command_info.type` : `disable_barge_in`

Disables DTMF barge-in so that playback from the phone integration is not interrupted when callers press keys. If `disable_barge_in` is enbaled, keys pressed during playback are ignored.

This command has no parameters.

### `command_info.type` : `enable_barge_in`

Enables DTMF barge-in so that callers can interrupt playback from the phone integration by pressing a key.

This command has no parameters.

### `command_info.type` : `send`

Sends DTMF signals using the phone integration.

This command supports the following parameters:

| parameter | description | required | default |
|-----------|-------------|----------|---------|
| `digits`  | An array of JSON objects where each element represents a DTMF tone to be sent to a caller. | yes | n/a |
| `digits[].code` | The event code to send. In addition to the digits 0 through 9, you can specify the following codes: \n - 10: `*` \n - 11: `#` \n - 12: `A` \n - 13: `B` \n - 14: `C` \n - 15: `D` | yes | n/a |
| `digits[].duration` | The duration (in milliseconds) of the event. | no | 200 |
| `digits[].volume` | The power level of the tone, in dBm0. The supported range is 0 to -63 dBm0. | no | 0 |
| `send_interval` | An interval (in milliseconds) to wait before sending the next DTMF tone in the list. | no | 200 |

###  Examples

This example shows the `dtmf` response type with the `collect` command, used to collect DTMF input.

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
            "count": 16,
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

This example shows the `dtmf` response type with the `send` command, used to send DTMF signals.

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

## Transfering the caller to the web chat integration
{: #dialog-voice-actions-transfer-channel}

You can transfer the caller from the current phone call to a web chat session by using the `channel_transfer` response type. The assistant sends an SMS message to the caller that includes a URL that the caller can tap to load the web chat widget in the phone's browser. The web chat session displays the history of the phone call and can start the process of collecting information needed to complete the transaction. This can be useful in situations where the customer can provide information more easily in writing than by speaking (for example, changing an address).

After the transfer has successfully completed, the caller can hang up the phone and continue the copnversation using the web chat.
The `channel_transfer` response type can be used with the phone integration only if the *SMS with Twilio* integration is also configured for the assistant.


```json
{
  "output": {
    "generic": [
      {
        "response_type": "text",
        "values": [
          {
            "text": "I will send you a text message now with a link to our website."
          }
        ],
        "selection_policy": "sequential"
      },
      {
        "response_type": "channel_transfer",
        "message_to_user": "Click the link to connect with an agent using our website.",
        "transfer_info": {
          "target": {
            "chat": {
              "url": "https://example.com/webchat"
            }
          }
        }
      }
    ]
  }
}
```
{: codeblock}



## End the call
{: #dialog-voice-actions-hangup}

You can instruct your assistant end a phone call by using the `end_session` response type, as shown in this example.

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

You can optionally include custom headers to include with the SIP `BYE` request that is generated when the phone integration receives this response type. 

This example shows the `end_session` response type with custom SIP headers:

```json
{
  "output": {
    "generic": [
      {
        "response_type": "end_session",
        "channel_options": {
          "voice_telephony": {
            "sip": {
              "headers": [
                {
                  "name": "Customer-Header1",
                  "value": "Some-Custom-Info"
                },
                {
                  "name": "User-to-User",
                  "value": "XXXXXX"
                }
              ]
            }
          }
        }
      }
    ]
  }
}
```
{: codeblock}

## Sending a text message during a phone conversation
{: #dialog-voice-actions-sms}

There are some situations when it is useful to be able to send a text message during an ongoing voice. For example, you might want the customer to specify a street address, which is easier to communicate accurately in writing than by transcribing voice input.

Before you can send SMS messages during a phone call, you must set up the *SMS with Twilio* integration. For more information, see [Integrating with *SMS with Twilio*](/docs/assistant?topic=assistant-deploy-sms).
{: note}

When you exchange a text with a customer during a conversation, the dialog initiates the SMS message exchange. It sends a text message to the user and asks for the user to respond to it.

To send a specific message from a dialog node or action step, use the `user_defined` response type with the `vgwActSendSMS` command:

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

You can specify any of the following parameters in the `parameters` object:

| Parameter         | Type   | Description |
|-------------------|--------|-------------|
| message           | string | The text of the SMS message to send. Required. |
| mediaURL          | list   | A list of URLs for media files to be sent with the message as MMS attachments. Optional. |
| tenantPhoneNumber | string | The phone number that is associated with the tenant. The format of the number must match the format that is required by the SMS provider. If no `tenantPhoneNumber` value is provided, the tenant ID from the phone integration configuration for the active call is used. Optional. |
| userPhoneNumber   | string | The phone number to send the SMS message to. The format of the number must match the format that is required by the SMS provider. If no `userPhoneNumber` value is provided, the voice caller's phone number from `From` header of the incoming SIP `INVITE` request is used. Optional. |

If your *SMS with Twilio* integration supports more than one SMS phone number, or you are using a non-Twilio SIP trunk, be sure to specify the phone number that you want to use to send the text message. Otherwise, the text is sent using the same phone number that was called.

After the assistant receives an SMS message, a new conversation turn is initiated with the text input `vgwSMSMessage`. This input indicates that a message was received from the caller. The text of the customer's message is included as the value of the `vgwSMSMessage`context variable. 

If the assistant is unable to send an SMS message to the caller, a new turn is initiated with the text input `vgwSMSFailed`. This input indicates that an SMS message could not be sent the caller. You can design your dialog or actions to handle such a failure by creating intents or actions that are triggered by the input text `vgwSMSFailed`.

``` json
{
  "input": {
    "message_type": "text",
    "text": "vgwSMSMessage"
  },
  "context": {
    "skills": {
      "main skill": {
        "user_defined": {
          "vgwSMSMessage": "1545 Lexington Ave."
        }
      }
    }
}
```
{: codeblock}

## Defining a sequence of phone commands
{: #dialog-voice-actions-sequence}

If you want to run more than one command in succession, include multiple responses in the `output.generic` array. These commands are processed in the order in which they are specified in the array.

This example shows two responses: first a text response, followed by an `end_session` response to end the call.

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
