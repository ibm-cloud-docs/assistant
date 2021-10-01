
---

copyright:
  years: 2020, 2021
lastupdated: "2021-06-23"

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

# Phone integration reference
{: #commands-voice}

Use phone-integration specific response types to manage the flow of conversations with customers who interact with your assistant over the telephone.
{: shortdesc}

Learn about the supported response types and reserved context variables that are used by the phone integration.

## Supported response types
{: #commands-voice-actions}

See how to use various response types to perform phone integration specific actions.

| Action | Response type| Description | Response object |
| -----| ----- | ----- | ----- |
|`Play text`| `text`|Plays an utterance string from the `text` field. | </p><p><code>{<br>&nbsp;&nbsp;"response_type": "text",<br>&nbsp;&nbsp;"text": "Hello! What can I do for you?"<br>}</code></p> |
|`Play audio` | `audio`|Plays an audio file. The file must be single channel (mono), PCM-encoded, and have a 8,000 Hz sampling rate with 16 bits per sample. | </p><p><code>{<br>&nbsp;&nbsp;"response_type": "audio",<br>&nbsp;&nbsp;"source": "https://raw.githubusercontent.com/WASdev/sample.voice.gateway/master/audio/musicOnHoldSample.wav",<br>&nbsp;&nbsp;"channel_options": {<br>&nbsp;&nbsp;&nbsp;&nbsp;"voice_telephony": {<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"loop": "false"<br>&nbsp;&nbsp;&nbsp;&nbsp;}<br>&nbsp;&nbsp;}<br>}</code></p> |
|`Collect DTMF` | `dtmf` | Instructs the phone integration to collect dual-tone multi-frequency signaling (DTMF) input. |  </p><p><code>{<br>&nbsp;&nbsp;"response_type": "dtmf",<br>&nbsp;&nbsp;"command_info": {<br>&nbsp;&nbsp;&nbsp;&nbsp;"type": "collect",<br>&nbsp;&nbsp;&nbsp;&nbsp;"parameters": {<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"termination_key": "#",<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"count": "16",<br> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"ignore_speech": true<br> &nbsp;&nbsp;&nbsp;&nbsp;}<br>&nbsp;&nbsp;}<br>}</code></p> |
| `Disable DTMF barge-in` |`dtmf`| Disables DTMF barge-in so that playback from the phone integration isn't interrupted when callers press keys. | </p><p><code>{<br>&nbsp;&nbsp;"response_type": "dtmf",<br>&nbsp;&nbsp;"command_info": { <br>&nbsp;&nbsp;&nbsp;&nbsp;"type": "disable_barge_in"<br>&nbsp;&nbsp;}<br>}</code></p> |
|`Enable DTMF barge-in`|`dtmf`| Enables DTMF barge-in so that callers can interrupt playback from the phone integration by pressing a key. | </p><p><code>{<br>&nbsp;&nbsp;"response_type": "dtmf",<br>&nbsp;&nbsp;"command_info": { <br>&nbsp;&nbsp;&nbsp;&nbsp;"type": "enable_barge_in"<br>&nbsp;&nbsp;}<br>}</code></p> |
| `Stop DTMF processing` | `stop_activities`|Disables DTMF input. All DTMF input is ignored until it's reenabled by the `Start DTMF processing` action.  | </p><p><code>{<br>&nbsp;&nbsp;"response_type": "stop_activities",<br>&nbsp;&nbsp;"activities": [<br>&nbsp;&nbsp;&nbsp;&nbsp;{<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"type": "dtmf_collection"<br>&nbsp;&nbsp;&nbsp;&nbsp;}<br>&nbsp;&nbsp;]<br>}</code></p> |
|`Start DTMF processing` | `start_activities`|Reenables DTMF input that was disabled by the `stop_activities` action. | </p><p><code>{<br>&nbsp;&nbsp;"response_type": "start_activities",<br>&nbsp;&nbsp;"activities": [<br>&nbsp;&nbsp;&nbsp;&nbsp;{<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"type": "dtmf_collection"<br>&nbsp;&nbsp;&nbsp;&nbsp;}<br>&nbsp;&nbsp;]<br>}</code></p> |
|`Disable speech barge-in` |`text_to_speech`| Disables speech barge-in so that playback from the phone integration isn't interrupted when callers speak. | </p><p><code>{<br>&nbsp;&nbsp;"response_type":"text_to_speech",<br>&nbsp;&nbsp;"command_info": { <br>&nbsp;&nbsp;&nbsp;&nbsp;"type": "disable_barge_in"<br>&nbsp;&nbsp;}<br>}</code></p> |
`Enable speech barge-in` |`text_to_speech`| Enables speech barge-in so that callers can interrupt playback from the phone integration by speaking. | </p><p><code>{<br>&nbsp;&nbsp;"response_type":"text_to_speech",<br>&nbsp;&nbsp;"command_info": { <br>&nbsp;&nbsp;&nbsp;&nbsp;"type": "enable_barge_in"<br>&nbsp;&nbsp;}<br>}</code></p>  |
|`Hangup` | `end_session`|Hangs up the call. | </p><p><code>{<br>&nbsp;&nbsp;"response_type": "end_session",<br>&nbsp;&nbsp;"channel_options": {<br> &nbsp;&nbsp;&nbsp;&nbsp;"voice_telephony": {<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"sip": {<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"headers": [<br> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"name": "Customer-Header1",<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"value": "Some-Custom-Info"<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;},<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"name": "User-To-User",<br> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"value": "XXXXXX"<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;]<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}<br>&nbsp;&nbsp;&nbsp;&nbsp;}<br> &nbsp;&nbsp;}<br>}<br></code></p> |
|`Stop speech processing` | `stop_activities`|Pauses speech-to-text processing until it's reenabled by the `Start speech processing` action. If recording is enabled and speech-to-text processing is paused, the audio from the caller isn't captured. | </p><p><code>{<br>&nbsp;&nbsp;"response_type": "stop_activities",<br>&nbsp;&nbsp;"activities": [<br>&nbsp;&nbsp;&nbsp;&nbsp;{<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"type": "speech_to_text_recognition"<br>&nbsp;&nbsp;&nbsp;&nbsp;}<br>&nbsp;&nbsp;]<br>}</code></p>|
|`Start speech processing` | `start_activities`|Resumes speech-to-text processing that was previously paused by the `stop_activities` action. | </p><p><code>{<br>&nbsp;&nbsp;"response_type": "start_activities",<br>&nbsp;&nbsp;"activities": [<br>&nbsp;&nbsp;&nbsp;&nbsp;{<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"type": "speech_to_text_recognition"<br>&nbsp;&nbsp;&nbsp;&nbsp;}<br>&nbsp;&nbsp;]<br>}</code></p>|
|`Send a SMS message` | `user_defined`|Enables the assistant to send an SMS message to the user.  <br> Parameters: <ul><li>`message`: An SMS message that is sent to the user. Required.</li><li>`mediaURL`: An array of publicly accessible URLs of the media to be sent to the user. Optional. </li><li>`tenantPhoneNumber` : The phone number that is associated with the tenant. The format of the number must match the format of the number that is required by the SMS provider. If no value is provided, the tenant ID from the voice configuration for the active call is sent. Optional.</li><li>`userPhoneNumber`: The phone number to send an SMS message to. The format of the number must match the format of the number that is required by the SMS provider. If no value is provided, the voice caller's phone number from an incoming SIP INVITE request (From header) is used. Optional.</li></ul> | </p><p><code>{<br>&nbsp;&nbsp;"response_type": "user_defined",<br>&nbsp;&nbsp;"user_defined": {<br>&nbsp;&nbsp;&nbsp;&nbsp;"vgwAction": {<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"command": "vgwActSendSMS",<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"parameters": {<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"message": "Hey, this is Watson Assistant. To send me your street address, respond to this text message with your address."<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}<br>&nbsp;&nbsp;&nbsp;&nbsp;}<br>&nbsp;&nbsp;}<br>}</code></p>|
|`Apply advanced speech services` | `speech_to_text`| Applies a set of parameters for the phone integration to pass to the {{site.data.keyword.speechtotextshort}} service. {{site.data.keyword.conversationshort}} dynamically defines the parameters based on the call. | </p><p><code>{<br>&nbsp;&nbsp;"response_type": "speech_to_text",<br>&nbsp;&nbsp;"command_info": { <br>&nbsp;&nbsp;&nbsp;&nbsp;"type": "configure"<br>&nbsp;&nbsp;&nbsp;&nbsp;"parameters": {<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"narrowband_recognize": {<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"model": "es-ES_NarrowbandModel",<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"smart_formatting": true<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}<br>&nbsp;&nbsp;&nbsp;&nbsp;} <br>&nbsp;&nbsp;}<br>} </code></p>|
|`Applying advanced {{site.data.keyword.texttospeechshort}} settings` |`text_to_speech`| Applies a set of parameters for the phone integration to pass to the {{site.data.keyword.texttospeechshort}} service. {{site.data.keyword.conversationshort}} dynamically defines the parameters based on the call. |  </p><p><code>{<br>&nbsp;&nbsp;"response_type": "connect_to_agent",  <br>&nbsp;&nbsp;"transfer_info": {    <br>&nbsp;&nbsp;&nbsp;&nbsp;"target": {      <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"service_desk": {        <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"sip": {          <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"uri": "sip:user\\@domain.com",          <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"accept_transfer_reject_codes": [            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"410",            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"202"          <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;],          <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"transfer_headers": [            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{              <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"name": "Customer-Header1",              <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"value": "Some-Custom-Info"            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;},            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{              <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"name": "User-To-User",              <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"value": "XXXXXX"            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}          <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;],          <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"transfer_headers_send_method": "refer_to_header"        <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}      <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}    <br>&nbsp;&nbsp;&nbsp;&nbsp;},    <br>&nbsp;&nbsp;&nbsp;&nbsp;"agent_available": {      "message": "I'll transfer you to an agent"    <br>&nbsp;&nbsp;&nbsp;&nbsp;},    <br>&nbsp;&nbsp;&nbsp;&nbsp;"agent_unavailable": {      "message": "Sorry, I could not find an agent."    <br>&nbsp;&nbsp;&nbsp;&nbsp;},    <br>&nbsp;&nbsp;&nbsp;&nbsp;"message_to_human_agent": "The caller needs help resetting their password"  <br>&nbsp;&nbsp;}<br>}</code></p> |
{: caption="Table 1. Actions that you can initiate from dialog" caption-side="top"}

## Reserved context variables
{: #commands-voice-context-variables}

The following table describes the context variables that have special meaning in the context of the phone integration. They should not be used for any purpose other than the documented use.

Table 2 describes the context variables that are set from your dialog. Table 3 describes the context variables that you can set by the phone integration.

### Table 2. Context variables that are set by your dialog
{: #commands-voice-context-variables-set-by-dialog}

| Context variable name | Expected value | Description |
| -------------- | ------ | ----------- |
| `final_utterance_timeout_count` | Time in ms | The amount of time in milliseconds that the phone integration waits to receive a final utterance from the {{site.data.keyword.speechtotextshort}} service. The timeout occurs if the phone integration does not receive a final utterance within the specified time limit, even if hypotheses continue to be generated. When the timeout occurs, the phone integration sends {{site.data.keyword.conversationshort}} a text update with the word "vgwFinalUtteranceTimeout" to indicate that no final utterance was received. |
| `post_response_timeout_count` | Time in ms | The amount of time in milliseconds to wait for a new utterance after the response is played back to the caller. If this value is exceeded, the {{site.data.keyword.conversationshort}} receives a text update with the word "vgwPostResponseTimeout" to indicate that a timeout occurred.|
{: caption="Table 2. Voice context variables set by the dialog" caption-side="top"}

### Table 3. Context variables that are set by the integration
{: #commands-voice-context-variables-set-by-integration}

| Context variable name | Expected value | Description |
| -------------- | ------ | ----------- |
| `post_response_timeout_occurred` | `true` or `false` | Indicates whether the post response timeout expired. |
| `barge_in_occurred` | `true` or `false` | Indicates whether barge-in occurred. |
| `final_utterance_timeout_occurred` | `true` or `false` | Indicates whether the final utterance timeout expired. |
| `dtmf_collection_succeeded` | `true` or `false` | Variable sent to the phone integration from {{site.data.keyword.conversationshort}} to indicate whether the DTMF collection succeeded or failed. When `true`, a DTMF collection succeeded, and returns the expected number of digits. When `false`, a DTMF collection failed to collect the specified number of digits. Even when `false`, all collected digits are passed to the dialog in the input string of the turn request. |
| `is_dtmf` | `true` or `false` | Indicates whether the input to {{site.data.keyword.conversationshort}} is dual-tone multi-frequency signaling (DTMF). |
| `user_phone_number` | String | The phone number that the call was received from. |
| `sip_call_id` | SIP Call-ID | The SIP call ID associated with the {{site.data.keyword.conversationshort}} session. |
| `sip_custom_invite_headers` | User-defined value | A user-defined list of SIP headers that are pulled from the initial SIP `INVITE` request and passed to the {{site.data.keyword.conversationshort}} service. For example: "`sip_custom_invite_headers`": {<br>&nbsp;&nbsp;"Custom-Header1": "123",<br>&nbsp;&nbsp;"Custom-Header2": "456"<br>}</code> |
| `sip_from_uri` | SIP From URI | The SIP From URI associated with the {{site.data.keyword.conversationshort}} service.|
| `sip_request_uri` | SIP Request URI | The SIP request URI that started the conversation session. |
| `sip_to_uri` | SIP To URI | The SIP To URI associated with the conversation session.|
| `private.user_phone_number` | String | The phone number that the SMS message was received from. |
| `assistant_phone_number` | String | The phone number associated with the Watson Assistant side that received the SMS message.| 
| `speech_to_text_result` | JSON object | The final response from the {{site.data.keyword.speechtotextshort}} service in JSON format, including the transcript and confidence score for the top hypothesis and any alternatives.<p>The format matches exactly the format that is received from the {{site.data.keyword.speechtotextshort}} service:</p><p><code>{<br/>&nbsp;&nbsp;"result_index": 0,<br/>&nbsp;&nbsp;"warnings": [<br/>&nbsp;&nbsp;&nbsp;&nbsp;"Unknown arguments: continuous."<br/>&nbsp;&nbsp;],<br/>&nbsp;&nbsp;"results": [<br/>&nbsp;&nbsp;&nbsp;&nbsp;{<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"final": true,<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"alternatives": [<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"transcript": "Hello world",<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"confidence": 0.758<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;},<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"transcript": "Hello wooled",<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"confidence": 0.358<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;]<br/>&nbsp;&nbsp;&nbsp;&nbsp;}<br/>&nbsp;&nbsp;]<br/>}.</code></p> |
{: caption="Table 3. Voice context variables set by the integration" caption-side="top"}
