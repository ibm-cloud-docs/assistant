






---

copyright:
  years: 2020, 2021
lastupdated: "2021-10-03"

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

# Phone integration reference
{: #commands-voice}

Use phone-integration specific response types to manage the flow of conversations with customers who interact with your assistant over the telephone.
{: shortdesc}

Learn about the supported response types and reserved context variables that are used by the phone integration.

## Supported response types
{: #commands-voice-actions}

See how to use various response types to perform phone integration specific commands.

| Command | Response type| Description | Parameters|
| -----| ----- | ----- | ----- |
|`Play text`| `text`|Plays an utterance string from the `text` field. | `text`: an utterance string|
|`Play audio` | `audio`|Plays an audio file. The file must be single channel (mono), PCM-encoded, and have a 8,000 Hz sampling rate with 16 bits per sample. | <ul><li>`source`: The URL to play. Required.</li><li> `channel_options.voice_telephony.loop`: Optionally set to `true` or `false` to indicate whether to play the URL in a loop. The default value is `false`.</li></ul>|
|`Collect DTMF` | `dtmf` | Instructs the phone integration to collect dual-tone multi-frequency signaling (DTMF) input.  | <br/><ul><li> `command_info.type` : `collect`</li><li>  `command_info.parameters` : For the list of the parameters used for the `collect` command type, see [Enabling keypad entry](/docs/assistant?topic=dialog-voice-actions#dialog-voice-actions-dtmf)</li></ul></li></ul> |
| `Disable DTMF barge-in` |`dtmf`| Disables DTMF barge-in so that playback from the phone integration isn't interrupted when callers press keys. |`command_info.type` : `disable_barge_in`|
|`Enable DTMF barge-in`|`dtmf`| Enables DTMF barge-in so that callers can interrupt playback from the phone integration by pressing a key. | `command_info.type` : `enable_barge_in`|
|`Send DTMF` | `dtmf` | Provides a way to send DTMF outbound.  | <br/><ul><li> `command_info.type` : `send`</li><li>  `command_info.parameters` : For the list of the parameters used for the `send` command type, see [Enabling keypad entry](/docs/assistant?topic=dialog-voice-actions#dialog-voice-actions-dtmf). </li></ul></li></ul> |
| `Stop DTMF processing` | `stop_activities`|Disables DTMF input. All DTMF input is ignored until it's reenabled by the `Start DTMF processing` command.  |`activities[].type` : `dtmf_collection`|
|`Start DTMF processing` | `start_activities`|Reenables DTMF input that was disabled by the `stop_activities` command. | `activities[].type` : `dtmf_collection`|
|`Disable speech barge-in` |`text_to_speech`| Disables speech barge-in so that playback from the phone integration isn't interrupted when callers speak. | `command_info.type` : `disable_barge_in`|
`Enable speech barge-in` |`text_to_speech`| Enables speech barge-in so that callers can interrupt playback from the phone integration by speaking. |`command_info.type` : `enable_barge_in`|
|`Hangup` | `end_session`|Hangs up the call. | No supported parameters.|
|`Stop speech processing` | `stop_activities`|Pauses speech-to-text processing until it's reenabled by the `Start speech processing` command. If recording is enabled and speech-to-text processing is paused, the audio from the caller isn't captured. | `activities[].type` : `speech_to_text_recognition`|
|`Start speech processing` | `start_activities`|Resumes speech-to-text processing that was previously paused by the `stop_activities` command. |`activities[].type` : `speech_to_text_recognition`|
|`Send a SMS message` | `user_defined`|Enables the assistant to send an SMS message to the user.   | <ul><li>`message`: An SMS message that is sent to the user. Required.</li><li>`mediaURL`: An array of publicly accessible URLs of the media to be sent to the user. Optional. </li><li>`tenantPhoneNumber` : The phone number that is associated with the tenant. The format of the number must match the format of the number that is required by the SMS provider. If no value is provided, the tenant ID from the phone integration configuration for the active call is sent. Optional.</li><li>`userPhoneNumber`: The phone number to send an SMS message to. The format of the number must match the format of the number that is required by the SMS provider. If no value is provided, the voice caller's phone number from an incoming SIP INVITE request (From header) is used. Optional.</li></ul><br>For more details, see [Sending a text message during a phone conversation](/docs/assistant?topic=dialog-voice-actions#dialog-voice-actions-sms)|
|`Applying advanced settings to the {{site.data.keyword.speechtotextshort}} service` | `speech_to_text`| Applies a set of parameters for the phone integration to pass to the {{site.data.keyword.speechtotextshort}} service. {{site.data.keyword.conversationshort}} dynamically defines the parameters based on the call. |For the list of the supported parameters see [Applying advanced settings to the {{site.data.keyword.speechtotextshort}} service](/docs/assistant?topic=dialog-voice-actions#dialog-voice-actions-speech-advanced)|
|`Applying advanced settings to the {{site.data.keyword.texttospeechshort}} service` |`text_to_speech`| Applies a set of parameters for the phone integration to pass to the {{site.data.keyword.texttospeechshort}} service. {{site.data.keyword.conversationshort}} dynamically defines the parameters based on the call. | For the list of the supported parameters see [Applying advanced settings to the {{site.data.keyword.texttospeechshort}} service](/docs/assistant?topic=dialog-voice-actions#dialog-voice-actions-text-advanced)|
|`Connect to agent` |`connect_to_agent`| Transfers the call to the defined transfer target.|For the list of the supported parameters see [Transferring a call to a human agent](/docs/assistant?topic=dialog-voice-actions#dialog-voice-actions-transfer)|
{: caption="Table 1. Commands that you can initiate from a dialog or actions" caption-side="top"}

## Reserved context variables
{: #commands-voice-context-variables}

The following table describes the context variables that have special meaning in the context of the phone integration. They should not be used for any purpose other than the documented use.


### Table 2. Context variables that are set by your dialog or actions
{: #commands-voice-context-variables-set-by-dialog}

| Context variable name | Expected value | Default value |Description |
| -------------- | ------ | ----------- |----------- |
| `final_utterance_timeout_count` | Time in ms | N/A | The amount of time in milliseconds that the phone integration waits to receive a final utterance from the {{site.data.keyword.speechtotextshort}} service. The timeout occurs if the phone integration does not receive a final utterance within the specified time limit, even if hypotheses continue to be generated. When the timeout occurs, the phone integration sends {{site.data.keyword.conversationshort}} a text update with the word "vgwFinalUtteranceTimeout" to indicate that no final utterance was received. |
| `post_response_timeout_count` | Time in ms | 7000|The amount of time in milliseconds to wait for a new utterance after the response is played back to the caller. When this timeout occurs, the phone integration channel sends a message request to {{site.data.keyword.conversationshort}} that will include the following input context: `input.integrations.voice_telephony.post_response_timeout_occurred` set to `true`.|
|`cdr_custom_data` |A JSON object that contain a set of key/value pairs.| N/A| This context variable provides a way to collect and save data throughout a conversation that will be stored with the CDR record at the end of the phone call. Each time this object is received it will merge with any previously received `cdr_custom_data` context received in a previous response. |
{: caption="Table 2. Voice context variables set by the dialog or actions" caption-side="top"}


**Example**
```json
{
  "output": {
    "generic": [
      {
        "response_type": "text",
        "text": "Hello"
      }
    ]
  },
  "context": {
    "integrations": {
      "voice_telephony": {
        "post_response_timeout_count": 10000,
        "cdr_custom_data": {
          "key1": "value1",
          "key2": "value2"
        }
      }
    }
  }
}
```
{: codeblock}

### Table 3. Context variables that are set by the phone channel
{: #commands-voice-context-variables-set-by-phone-channel}

| Context variable name | Expected value | Description |
| -------------- | ------ | ----------- |
| `sip_call_id` | SIP Call-ID | The SIP call ID associated with the {{site.data.keyword.conversationshort}} session. |
| `sip_custom_invite_headers` | User-defined value | A user-defined list of SIP headers that are pulled from the initial SIP `INVITE` request and passed to the {{site.data.keyword.conversationshort}} service. For example: "`sip_custom_invite_headers`": {<br>&nbsp;&nbsp;"Custom-Header1": "123",<br>&nbsp;&nbsp;"Custom-Header2": "456"<br>}</code> |
| `private.sip_from_uri` | SIP From URI | The SIP From URI associated with the {{site.data.keyword.conversationshort}} service.|
| `private.sip_request_uri` | SIP Request URI | The SIP request URI that started the conversation session. |
| `private.sip_to_uri` | SIP To URI | The SIP To URI associated with the conversation session.|
| `private.user_phone_number` | String | The phone number that the call was received from. |
| `assistant_phone_number` | String | The phone number associated with the Watson Assistant side that received the phone call.| 
{: caption="Table 3. Context variables set by the phone channel" caption-side="top"}



### Table 4. Input parameters that are set by the phone channel
{: #commands-voice-input-parameters-set-by-phone-channel}


The following input parameters are only valid for the current conversation turn.

| Context variable name | Expected value | Description |
| -------------- | ------ | ----------- |
| `post_response_timeout_occurred` | `true` or `false` | Indicates whether the post response timeout expired. |
| `barge_in_occurred` | `true` or `false` | Indicates whether barge-in occurred. |
| `final_utterance_timeout_occurred` | `true` or `false` | Indicates whether the final utterance timeout expired. |
| `dtmf_collection_succeeded` | `true` or `false` | Variable sent to the phone integration from {{site.data.keyword.conversationshort}} to indicate whether the DTMF collection succeeded or failed. When `true`, a DTMF collection succeeded, and returns the expected number of digits. When `false`, a DTMF collection failed to collect the specified number of digits. Even when `false`, all collected digits are passed to the dialog in the input string of the turn request. |
| `is_dtmf` | `true` or `false` | Indicates whether the input to {{site.data.keyword.conversationshort}} is dual-tone multi-frequency signaling (DTMF). |
| `speech_to_text_result` | JSON object | The final response from the {{site.data.keyword.speechtotextshort}} service in JSON format, including the transcript and confidence score for the top hypothesis and any alternatives.<p>The format matches exactly the format that is received from the {{site.data.keyword.speechtotextshort}} service:</p><p><code>{<br/>&nbsp;&nbsp;"result_index": 0,<br/>&nbsp;&nbsp;"warnings": [<br/>&nbsp;&nbsp;&nbsp;&nbsp;"Unknown arguments: continuous."<br/>&nbsp;&nbsp;],<br/>&nbsp;&nbsp;"results": [<br/>&nbsp;&nbsp;&nbsp;&nbsp;{<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"final": true,<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"alternatives": [<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"transcript": "Hello world",<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"confidence": 0.758<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;},<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"transcript": "Hello wooled",<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"confidence": 0.358<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;]<br/>&nbsp;&nbsp;&nbsp;&nbsp;}<br/>&nbsp;&nbsp;]<br/>}.</code></p> |
{: caption="Table 4. Input parameters set by the phone channel" caption-side="top"}


**Example**

```json
{
  "input": {
    "text": "agent ",
    "integrations": {
      "voice_telephony": {
        "speech_to_text_result": {
          "result_index": 0,
          "stopTimestamp": "2021-09-29T17:43:31.036Z",
          "transaction_ids": {
            "x-global-transaction-id": "43dd6ce0-139a-4d76-95aa-86e03fcfc434",
            "x-dp-watson-tran-id": "6e60695e-fed7-4efe-a376-0888b027d30f"
          },
          "results": [
            {
              "final": true,
              "alternatives": [
                {
                  "transcript": "agent ",
                  "confidence": 0.78
                }
              ]
            }
          ],
          "transactionID": "43dd6ce0-139a-4d76-95aa-86e03fcfc434",
          "startTimestamp": "2021-09-29T17:43:29.436Z"
        },
        "is_dtmf": false,
        "barge_in_occurred": false
      }
    }
  },
  "context": {
    "skills": {
      "main skill": {
        "user_defined": {},
        "system": {}
      }
    },
    "integrations": {
      "voice_telephony": {
        "private": {
          "sip_to_uri": "sip:watson-conversation@10.10.10.10",
          "sip_from_uri": "sip:10.10.10.11",
          "sip_request_uri": "sip:test@10.10.10.10:5064;transport=tcp"
        },
        "sip_call_id": "QjryZsuAS4",
        "assistant_phone_number": "18882346789"
      }
    }
  }
}
```
{: codeblock}



