---

copyright:
  years: 2019, 2023
lastupdated: "2023-03-24"

keywords: log webhook

subcollection: assistant

---

{{site.data.keyword.attribute-definition-list}}

Documentation for the **classic {{site.data.keyword.assistant_classic_short}}** experience has moved. For the most up-to-date version, see [Logging activity with a webhook](/docs/watson-assistant?topic=watson-assistant-webhook-log){: external}. To see all documentation for the new {{site.data.keyword.conversationshort}}, please go [here](https://cloud.ibm.com/docs/watson-assistant){: external}.
{: attention}

# Logging activity with a webhook
{: #webhook-log}

[Plus]{: tag-green}[Enterprise]{: tag-purple}

You can log activity by making a call to an external service or application every time a customer submits input to the assistant.
{: shortdesc}

A webhook is a mechanism that allows you to call out to an external program based on events in your program.

This feature is available only to Plus and Enterprise plan users.

Add a log webhook to your assistant if you want to use an external service to log {{site.data.keyword.assistant_classic_short}} activity. You can log two kinds of activity:

- **Messages and responses**: The log webhook is triggered each time the assistant responds to user input. You can use this option as an alternative to the built-in analytics feature to handle logging yourself. (For more information about the built-in analytics support, see [Metrics overview](/docs/assistant?topic=assistant-logs-overview).)
  
    The log webhook is not supported for API clients that use the v1 `/message` method.
    {: note}

- **Call detail records (CDRs)**: The log webhook is triggered after each telephone call a user makes to your assistant using the phone integration. A Call Detail Record (CDR) is a summary report that documents the details of a telephone call, including phone numbers, call length, latency, and other diagnostic information. CDR records are only available for assistants that use the phone integration.

The log webhook does not return anything to your assistant.

For environments where private endpoints are in use, keep in mind that a webhook sends traffic over the internet. For more information, see [Private network endpoints](/docs/assistant?topic=assistant-security#security-private-endpoints).
{: note}

## Defining the webhook
{: #webhook-log-create}

You can define one webhook URL to use for logging every incoming message or CDR event.

The programmatic call to the external service must meet these requirements:

- The call must be a POST HTTP request.

To add the webhook details, complete the following steps:

1. From your assistant, click the ![Overflow menu](images/kebab.png) icon, and then choose **Settings**.

1. Click **Webhooks > Log webhook**.

1. Set the *Log webhook* switch to **Enabled**.

    If you cannot enable the webhook, you might need to upgrade your service plan.

1. In the **URL** field, add the URL for the external application to which you want to send HTTP POST request callouts.

    For example, to write the message to the 

    ```bash
    https://www.mycompany.com/my_log_service
    ```
    {: codeblock}

    You must specify a URL that uses the SSL protocol, so specify a URL that begins with `https`.

1. In the **Secret** field, add a token to pass with the request that can be used to authenticate with the external service.

    The secret must be specified as a text string, such as `purple unicorn`.  Maximum length is 1,024 characters. You cannot specify a context variable.

    It is the responsibility of the external service to check for and verify the secret. If the external service does not require a secret, you can leave this field empty.

    It is the responsibility of the external service to check for and verify the secret. If the external service does not require a token, specify any string you want. You cannot leave this field empty.

    If you want to see the secret as you enter it, click on the **Show password** icon ![view icon](../../icons/view.svg) before you start typing. After you save the secret, the string is replaced by asterisks and can't be viewed again.
    {: note}

1. Click the appropriate checkboxes to select which kinds of activity you want to log:

    - To log messages and responses, select **Subscribe to conversation logs**.
    - To log CDR events for the phone integration, select **Subscribe to CDR (Call Detail Records)**.

1. In the Headers section, add any headers that you want to pass to the service one at a time by clicking **Add header**.

    The service automatically sends an `Authorization` header with a JWT; you do not need to add one. If you want to handle authorization yourself, add your own authorization header and it will be used instead.

    After you save the header value, the string is replaced by asterisks and can't be viewed again. 
    {: note}

Your webhook details are saved automatically.

## Removing the webhook
{: #webhook-log-delete}

If you decide you do not want to log messages programmatically, complete the following steps:

1. From the assistant overview page, click the ![Overflow menu](images/kebab.png) icon, and then choose **Settings**.

1. Click **Webhooks > Log webhook**.

1. Do one of the following things:

    - To change the webhook that you want to call, click **Delete webhook** to delete the currently specified URL and secret. You can then add a new URL and other details.
    - To stop calling a webhook to log every message and response, click the *Log webhook* switch to disable the webhook altogether.

## Webhook security
{: #webhook-log-security}

To authenticate the webhook request, verify the JSON Web Token (JWT) that is sent with the request. The webhook microservice automatically generates a JWT and sends it in the `Authorization` header with each webhook call. It is your responsibility to add code to the external service that verifies the JWT.

For example, if you specify `purple unicorn` in the **Secret** field, you might add code similar to this:

```javascript
const jwt = require('jsonwebtoken');
...
const token = request.headers.authentication; // grab the "Authentication" header
try {
  const decoded = jwt.verify(token, 'purple unicorn');
} catch(err) {
  // error thrown if token is invalid
}
```
{: codeblock}

## Webhook request body
{: #webhook-log-request-body}

The request body the webhook sends to the external service is a JSON object with the following structure:

```text
{
  "event": {
    "name": "{event_type}"
   },
  "payload": {
    ...
  }
}
```
{: codeblock}

where `{event_type}` is either `message_logged` (for messages and responses) or `cdr_logged` (for CDR events).

The `payload` object contains the event data to be logged. The structure of the `payload` object depends on the type of event.

### Message event payload
{: #webhook-log-request-body-message}

For `message_logged` events, the `payload` object contains data about a message request sent to the assistant and the message response returned to the integration or client application. For more information about the fields that are part of message requests and responses, see the [API reference](https://cloud.ibm.com/apidocs/assistant/assistant-v2#message).

The log webhook payload might include data that is not currently supported by the API (for example, data returned from actions skills). Any fields that are not defined in the API reference documentation are subject to change.
{: important}

### CDR event payload
{: #webhook-log-request-body-cdr}

For `cdr_logged` events, the `payload` object contains data about a Call Detail Record (CDR) event that was handled by the phone integration. The structure of the `payload` object for a CDR event is as shown by this example:

```json
{
  "primary_phone_number": "+18005550123",
  "global_session_id": "9caa8bad-aaa8-4a5a-a4b5-62bccc703d15",
  "failure_occurred": false,
  "transfer_occurred": false,
  "active_calls": 0,
  "warnings_and_errors": [
    {
      "code": "CWSMR0033W",
      "message": "CWSMR0033W: The inbound RTP audio stream jitter of 43 ms exceeds the maximum jitter threshold of 30 ms."
    },
    {
      "code": "CWSMR0070W",
      "message": "CWSMR0070W: A request to the Watson Speech To Text service failed for the following reason = Unexpected server response: 403, response headers = {\"strict-transport-security\":\"max-age=31536000; includeSubDomains;\",\"content-length\":\"157\",\"content-type\":\"application/json\",\"x-dp-watson-tran-id\":\"23860083-88b6-41d7-9130-30bbfebe647e\",\"x-request-id\":\"23860083-88b6-41d7-9130-30bbfebe647e\",\"x-global-transaction-id\":\"6c764df3-81db-41bb-a14f-62384facffca\",\"server\":\"watson-gateway\",\"x-edgeconnect-midmile-rtt\":\"1\",\"x-edgeconnect-origin-mex-latency\":\"28\",\"date\":\"Thu, 13 May 2021 20:31:12 GMT\",\"connection\":\"keep-alive\"}, response body = {\"code\":403,\"trace\":\"23860083-88b6-41d7-9130-30bbfebe647e\",\"error\":\"Forbidden\",\"more_info\":\"[https://cloud.ibm.com/docs/watson?topic=watson-forbidden-error](https://cloud.ibm.com/docs/watson?topic=watson-forbidden-error)\"}, x-global-transaction-id = 6c764df3-81db-41bb-a14f-62384facffca. The Media Relay will reattempt to send the request."
    }
  ],
  "realtime_transport_network_summary": {
    "inbound_stream": {
      "average_jitter": 4,
      "canonical_name": "b74f3689-1ae8-4a0a-bde3-adf5b488553e",
      "maximum_jitter": 18,
      "packets_lost": 0,
      "packets_transmitted": 952,
      "tool_name": ""
    },
    "outbound_stream": {
      "average_jitter": 0,
      "canonical_name": "voice.gateway",
      "maximum_jitter": 0,
      "packets_lost": 0,
      "packets_transmitted": 838,
      "tool_name": "IBM Voice Gateway/1.0.7.0"
    }
  },
  "call": {
    "start_timestamp": "2021-10-12T20:54:02.591Z",
    "stop_timestamp": "2021-10-12T20:54:20.375Z",
    "milliseconds_elapsed": 17784,
    "outbound": false,
    "end_reason": "assistant_hangup",
    "security": {
      "media_encrypted": false,
      "signaling_encrypted": false,
      "sip_authenticated": false
    }
  },
  "session_initiation_protocol": {
    "invite_arrival_time": "2021-10-12T20:54:00.565Z",
    "setup_milliseconds": 2026,
    "headers": {
      "call_id": "17465345_115257202@10.90.150.99",
      "from_uri": "sip:+18885550456@pstn.twilio.com",
      "to_uri": "sip:+18005550123@public.voip.us-south.assistant.test.watson.cloud.ibm.com"
    }
  },
  "max_response_milliseconds": {
    "assistant": 339,
    "text_to_speech": 535,
    "speech_to_text": 0
  },
  "assistant_interaction_summaries": [
    {
      "session_id": "7874ec3a-1330-4180-afe1-46bfb220af5b",
      "assistant_id": "97f16ba4-ad94-41af-aa6c-33cd56ad5e7e",
      "turns": [
        {
          "assistant": {
            "log_id": "58bebfd1-0118-419b-a555-b152a1efbbe8",
            "response_milliseconds": 339,
            "start_timestamp": "2021-10-12T20:54:00.722Z"
          },
          "request": {
            "type": "start"
          },
          "response": [
            {
              "barge_in_occurred": true,
              "streaming_statistics": {
                "response_milliseconds": 301,
                "start_timestamp": "2021-10-12T20:54:00.722Z",
                "stop_timestamp": "2021-10-12T20:54:01.023Z",
                "transaction_id": "3dce431c-fb2f-4b62-9fce-585f4e06fe00"
              },
              "type": "text_to_speech"
            }
          ]
        },
        {
          "assistant": {
            "log_id": "38f36bfb-c2aa-4600-9418-6ab422664e31",
            "response_milliseconds": 158,
            "start_timestamp": "2021-10-12T20:54:05.621Z"
          },
          "request": {
            "type": "dtmf"
          },
          "response": [
            {
              "type": "disable_speech_barge_in"
            },
            {
              "type": "text_to_speech",
              "barge_in_occurred": false,
              "streaming_statistics": {
                "transaction_id": "af4c47c3-5cc4-43c8-9b9c-81d6f997c52f",
                "start_timestamp": "2021-10-12T20:54:06.321Z",
                "stop_timestamp": "2021-10-12T20:54:14.338Z",
                "response_milliseconds": 535
              }
            },
            {
              "type": "enable_speech_barge_in"
            },
            {
              "type": "text_to_speech",
              "barge_in_occurred": true,
              "streaming_statistics": {
                "transaction_id": "eafdd846-2829-4e1a-8068-b1035510b1e1",
                "start_timestamp": "2021-10-12T20:54:14.795Z",
                "stop_timestamp": "2021-10-12T20:54:20.388Z",
                "response_milliseconds": 447
              }
            }
          ]
        },
        {
          "assistant": {
            "log_id": "07d74b35-0205-43e4-923c-1e43e1cb429c",
            "response_milliseconds": 0,
            "start_timestamp": "2021-10-12T20:54:20.377Z"
          },
          "request": {
            "type": "hangup"
          },
          "response": []
        }
      ]
    }
  ]
}

```
{: codeblock}

The  `payload`  JSON object for a call detail record event contains the following keys:

| Key | Type | Description |
| --- | --- | --- |
| `primary_phone_number` | string | The phone number that was called. |
| `global_session_id` | string | The unique session identifier. |
| `failure_occurred` | Boolean | Indicates whether a failure occurred during the call. | 
| `failure_details` |  string | Details about a failure. | 
| `transfer_occurred` | Boolean | Indicates whether an attempt was made to transfer a call |
| `active_calls` | Number | Number of active calls when the call started. |
| `x-global-sip-trunk-call-id` | string | The value of the SIP trunk call ID header extracted from the initial SIP `INVITE` request. The following SIP trunk call ID headers are supported:  \n *X-Twilio-CallSid*  \n *X-SID*  \n *X-Global-SIP-Trunk-Call-ID* |
| `call` | JSON object | Contains information about the call. |
| `session_initiation_protocol` | JSON object | SIP protocol related details. |
| `max_response_milliseconds` |JSON object | Maximum latency for various services used during the call. |
| `assistant_interaction_summaries` | JSON array | Details about the {{site.data.keyword.assistant_classic_short}} transactions that took place during the call. |
| `injected_custom_data` | JSON object | A JSON object that contains a set of key/value pairs. Extracted from the 	`cdr_custom_data` context variable. |
| `warnings_and_errors` | JSON array | An array of warnings and errors that were logged during the call. |
| `realtime_transport_network_summary` | JSON object | When RTCP is enabled, the `realtime_transport_network_summary` object provides statistics for the inbound stream in the `inbound_stream` object and statistics for the outbound stream in the `outbound_stream` object. |
{: caption="Keys for the payload object" caption-side="top"}

The `call` object contains the following keys:

| Key | Type | Description |
| --- | --- | --- |
| `start_timestamp` | string. Time in the ISO format `yyyy-MM-ddTHH:mm:ss.SSSZ`| Time when the call started. |
| `stop_timestamp` | string. Time in the ISO format `yyyy-MM-ddTHH:mm:ss.SSSZ` | Time when the call ended. |
| `milliseconds_elapsed` | number | Length of the call in milliseconds. |
| `end_reason` | string | Call end reason:  \n *assistant_transfer*  \n *assistant_hangup*  \n *caller_hangup*  \n *failed* |
| `security.media_encrypted` | Boolean | Indicates whether the media was encrypted. |
| `security.signaling_encrypted` | Boolean | Indicates whether the SIP signaling was encrypted. |
| `security.sip_authenticated` | Boolean | Indicates whether SIP authentication was used to challenge the authentication of the caller. |
{: caption="Keys for the call object" caption-side="top"}

The `session_initiation_protocol` object contains the following keys:

| Key | Type | Description |
| --- | --- | --- |
| `invite_arrival_timestamp` | string. Time in the ISO format `yyyy-MM-ddTHH:mm:ss.SSSZ` | Time when the `INVITE` request arrived. |
| `setup_milliseconds` | number | Time it took to set up the call in milliseconds. Specifically, the field shows the time between when the initial SIP `INVITE` request was received and when the final SIP `ACK` request was received. |
| `headers.call_id` | string | The SIP `Call-ID` header field pulled from the SIP `INVITE` related to the call. |
| `headers.from_uri` | string | SIP URI from the initial SIP INVITE `From` field |
| `headers.to_uri` | string | SIP URI from the initial SIP INVITE `To` field |
{: caption="Keys for the session_initiation_protocol object" caption-side="top"}

The `assistant_interaction_summaries` object contains the following keys:

| Key | Type | Description |
| --- | --- | --- |
| `assistant_id` | string | The unique identifier of the assistant. |
| `session_id` | string | The unique identifier of the session. |
| `turns` | JSON array | An array of the {{site.data.keyword.assistant_classic_short}} transactions that took place during the conversation. |
{: caption="Keys for the assistant_interaction_summaries object" caption-side="top"}

The `turn` object contains the following keys:

| Key | Type | Description |
| --- | --- | --- |
| `assistant.log_id` | string | A unique identifier for the logged transaction. Can be used to correlate between message logs and CDR events. |
| `assistant.start_timestamp` | string. Time in the ISO format `yyyy-MM-ddTHH:mm:ss.SSSZ` | Time when the request was sent to {{site.data.keyword.assistant_classic_short}}. |
| `assistant.response_milliseconds` | number | Time between when the request was sent and when the response was received from {{site.data.keyword.assistant_classic_short}}. |
| `request` | JSON object | A request sent to {{site.data.keyword.assistant_classic_short}}. |
| `response` | JSON array | An array of the `response` objects associated with the request. |
{: caption="Keys for the turn object" caption-side="top"}

The `request` object contains the following keys:

| Key | Type | Description |
| --- | --- | --- |
| `type` | string | The request type: \n *start* - an initial request to {{site.data.keyword.assistant_classic_short}} \n *speech_to_text* - a request is triggered on speech recognition  \n *dtmf* - a request is triggered when DTMF collection completes  \n *sms* - a request is triggered when a SMS message is received from the caller  \n *post_response_timeout* - a request is triggered when the post response timer expires  \n *redirect* - a request is triggered when a call is redirected  \n *transfer* - a request is triggered when a call is transferred  \n *transfer_failed* - a request is triggered when a call transfer fails  \n *final_utterance_timeout* - a request is triggered when the final utterance timer expires  \n *no_input_turn* - a request is triggered when `no inpout turn` is enabled  \n *sms_failure* - a request is triggered when a SMS message can't be sent to the caller  \n *speech_to_text_result_filtered* - a request is triggered when an utterance is filtered due to low confidence level  \n *mrcp_recognition_unsuccessful* - a request is triggered when the MRCP recognition completes without a final utterance  \n *network_warning* - a request is triggered when a network error is detected  \n *media_capability_change* - a request is triggered when media capabilities change in the middle of a call |
|`streaming_statistics`| JSON object|Contains information and statistics related to the {{site.data.keyword.speechtotextshort}} recognition. |
{: caption="Keys for the request object" caption-side="top"}

The `request.streaming_statistics` object contains the following keys:

| Key | Type | Description |
| --- | --- | --- |
| `transaction_id` | string | A unique identifier of the transaction. |
| `start_timestamp` | string. Time in the ISO format `yyyy-MM-ddTHH:mm:ss.SSSZ` | Time when the transaction started. |
| `stop_timestamp` | string. Time in the ISO format `yyyy-MM-ddTHH:mm:ss.SSSZ` |  Time when the transaction ended. |
| `response_milliseconds` | number | Latency in milliseconds between when silence is detected in the caller's speech and a final result from {{site.data.keyword.speechtotextshort}} is received. |
| `echo_detected` | Boolean | Indicates whether an echo was detected. The value can be `true` or `false`. |
| `confidence` | number | The confidence score of the final utterance. |
{: caption="Keys for the request.streaming_statistics object" caption-side="top"}

The `response` object contains the following keys:

| Key | Type | Description |
| --- | --- | --- |
| `type` | string | The response type:  \n *text_to_speech* - a command to play an utterance to the caller  \n *sms* - a command to send a SMS to the caller  \n *url* - a command to play an audio file to the caller  \n *transfer* - a command to transfer a call  \n *text_to_speech_config* - a command to change {{site.data.keyword.texttospeechshort}} settings  \n *speech_to_text_config* - a command to change the {{site.data.keyword.speechtotextshort}} settings  \n *pause_speech_to_text* - a command to stop speech recognition    \n *unpause_speech_to_text* - a command to start speech recognition  \n *pause_dtmf* - a command to stop DTMF recognition  \n *unpause_dtmf* - a command to start speech recognition  \n _enable_speech_barge_in_ - a command to enable speech barge-in so that callers can interrupt playback by speaking  \n *disable_speech_barge_in* - a command to disable speech barge-in so that playback isn't interrupted when callers speak over top of the played back audio  \n *enable_dtmf_barge_in* - a command to enables DTMF barge-in so that callers can interrupt playback from the phone integration by pressing a key.  \n *disable_dtmf_barge_in* - a command to disable DTMF barge-in so that playback from the phone integration isn't interrupted when callers press keys  \n *dtmf* - a command to send DTMFs to the caller  \n *hangup* - a command to disconnect a call |
| `barge_in_occurred` | Boolean | Indicates whether barge-in occurred during the turn. |
| `streaming_statistics` | JSON object | Contains information and statistics related to the {{site.data.keyword.texttospeechshort}} synthesis and playback. |
{: caption="Keys for the response object" caption-side="top"}

The `response.streaming_statistics` object contains the following keys:

| Key | Type | Description |
| --- | --- | --- |
| `transaction_id` | string | A unique identifier of the transaction. |
| `start_timestamp` | string. Time in the ISO format `yyyy-MM-ddTHH:mm:ss.SSSZ` | Time when the transaction started. |
| `stop_timestamp` | string. Time in the ISO format `yyyy-MM-ddTHH:mm:ss.SSSZ` | Time when the transaction ended. |
| `response_milliseconds` | number | Time in milliseconds between when a text utterance is sent to the {{site.data.keyword.texttospeechshort}} service and when the phone integration receives the first packet of synthesized audio. |
{: caption="Keys for the response.streaming_statistics object" caption-side="top"}

The `max_response_milliseconds` object contains the following keys:

| Key | Type | Description |
| --- | --- | --- |
| `assistant` | number | Maximum round-trip latency in milliseconds, calculated from all {{site.data.keyword.assistant_classic_short}} requests related to the call. |
| `text_to_speech` | number | Maximum time in milliseconds between when a text utterance is sent to the {{site.data.keyword.texttospeechshort}} service and when the phone integration receives the first packet of synthesized audio. Calculated from all the {{site.data.keyword.texttospeechshort}} requests related to this call. |
| `speech_to_text` | number|Maximum latency in milliseconds between when silence is detected in the user's speech and a final result from {{site.data.keyword.speechtotextshort}} is received. This value is calculated from all the {{site.data.keyword.speechtotextshort}} recognition results related to this call. |
{: caption="Keys for the max_response_milliseconds object" caption-side="top"}


#### Mapping between CDR and Watson Assistant response types
{: #webhook-log-cdr-response-type-mapping}

| CDR response type | Watson Assistant response type |
| --- | --- |
| `text_to_speech` | `text` |
| `url` | `audio` |
| `dtmf` | `dtmf`, `command_info.type` : `send` |
| `sms` | `user_defined`,  `vgwAction.command` : `vgwActSendSMS`|
| `transfer` | `connect_to_agent` |
| `text_to_speech_config` | `text_to_speech`, `command_info.type` : `configure` |
| `speech_to_text_config` | `speech_to_text`, `command_info.type` : `configure` |
| `unpause_speech_to_text` | `start_activities`, `type`:`speech_to_text_recognition` |
| `pause_speech_to_text` | `stop_activities`, `type`:`speech_to_text_recognition` |
| `unpause_dtmf` | `start_activities`, `type`:`dtmf_collection`|
| `pause_dtmf` | `stop_activities`, `type`:`dtmf_collection` |
| `enable_speech_barge_in` | `text_to_speech`, `command_info.type` : `enable_barge_in` |
| `disable_speech_barge_in` | `text_to_speech`, `command_info.type` : `disable_barge_in` |
| `enable_dtmf_barge_in` | `dtmf`, `command_info.type` : `enable_barge_in` |
| `disable_dtmf_barge_in` | `dtmf`, `command_info.type` : `disable_barge_in` |
| `hangup` | `end_session` |
{: caption="Mapping between CDR and Watson Assistant response types" caption-side="top"}


#### Warning details
{: #webhook-log-cdr-warning-details}

The  `warnings_and_errors`  object contains warnings and errors that were logged during the call, listed in order of occurrence. Warnings for the following conditions are included:

-   Messages when utterances are filtered out by the confidence score threshold.
-  {{site.data.keyword.texttospeechshort}} underflows, which is when {{site.data.keyword.texttospeechshort}} synthesis can't keep up with the phone integration streaming rate and audio might skip.
-   RTP network warnings, such as high packet loss or high average jitter, if RTCP is enabled.

```plaintext

  "warnings_and_errors": [
    {
      "message": "CWSMR0032W: A Watson Speech to Text final utterance has a confidence score of 0.1, which does not meet the confidence score threshold of 0.2. The utterance will be ignored.",
      "id": "CWSMR0032W"
    },
    {
      "message": "CWSMR0031W: The synthesis stream from the Watson Text To Speech service can't keep up with the playback rate to the caller, so audio might skip. transaction ID=a1b2c3d4e5",
      "id": "CWSMR0031W"
    }
  ]
```

The object for each warning contains the following keys:

| Key | Type | Description |
| --- | --- | --- |
| `message` | string | The text of the warning message that was logged. |
| `id` | string | The unique message identifier. |
{: caption="Keys for the warnings_and_errors object" caption-side="top"}

#### RTP network summary details
{: #webhook-log-cdr-rtp-network-summary-details}

When RTCP is enabled, each  `realtime_transport_network_summary`  object provides statistics for the inbound stream in the  `inbound_stream`  object and statistics for the outbound stream in the  `outbound_stream`  object.

```json
"realtime_transport_network_summary": {
  "inbound_stream": {
      "maximum_jitter": 5,
      "average_jitter": 1,
      "packets_lost": 0,
      "packets_transmitted": 1000,
      "canonical_name": "user@example.com",
      "tool_name": "User SIP Phone"
   },
  "outbound_stream": {
      "maximum_jitter": 5,
      "average_jitter": 1,
      "packets_lost": 0,
      "packets_transmitted": 2000,
      "canonical_name": "voice.gateway@127.0.0.1",
      "tool_name": "IBM Voice Gateway/1.0.0.5"
   }
}

```     
{: codeblock}


The objects for each stream contain the following keys:


| Key | Type | Description |
| --- | --- | --- |
| `maximum_jitter` | Number | Maximum jitter during the call |
| `average_jitter` | Number | Average jitter, calculated over the call duration |
| `packets_lost` | Number | An estimate of the number of packets that were lost during the call |
| `packets_transmitted` | Number | An estimate of the total number of packets that were transmitted during the call |
| `canonical_name` | string | A unique identifier for the sender of the stream, typically in  *@* format |
| `tool_name` | string | The name of the application or tool where the stream originated. For Voice Gateway, the default is `IBM Voice Gateway/`. |
{: caption="Keys for RTP network summary details"  caption-side="top"}


#### Injecting custom values into call detail record events
{: #webhook-log-cdr-custom-data}

By including the  `cdr_custom_data`  context variable in your Watson Assistant dialog or actions, you can add custom data to your Call Detail Records. Each time this object is received it will merge with any previously received `cdr_custom_data` . You can record data related to various activities that are happening during a call. This includes ways to identify completion of specific tasks.

```plaintext
  "context": {
    "integrations": {
      "voice_telephony": {
        "cdr_custom_data": {
          "key1": "value1",
          "key2": "value2"
        }
      }
    }
  }

```
{: codeblock}


When generating a CDR report, the custom data is included in the  `injected_custom_data`  field. For example, in the following CDR report, the  `key1`  and  `key2`  are added to the  `injected_custom_data`  field.

```plaintext
{
  "payload": {
  ...
    "injected_custom_data": {
      "key1": "value1",
      "key2": "value2"
    }
  ...
  }
}

```
{: codeblock}


##### Merging CDR data
{: #webhook-log-cdr-merging-custom-data}

During a call, if the  `cdr_custom_data`  context variable is received multiple times, the data is merged in the  `injected_custom_data`  field.

In the first call, values for  `key1`  and  `key2`  are defined.


```plaintext
  "context": {
    "integrations": {
      "voice_telephony": {
        "cdr_custom_data": {
          "key1": "value1",
          "key2": "value2"
        }
      }
    }
  }

```
{: codeblock}


In the second call, values for  `key1`  and  `key3`  are defined.


```plaintext
  "context": {
    "integrations": {
      "voice_telephony": {
        "cdr_custom_data": {
          "key1": "value11",
          "key3": "valu3"
        }
      }
    }
  }

```
{: codeblock}



After these calls, the following code example shows how the custom CDR data is merged in the  `injected_custom_data`  field.

```plaintext
{
  "payload": {
  ...
    "injected_custom_data": {
      "key1": "value11",
      "key2": "value2",
      "key3": "value3"
    }
  ...
  }
}

```
{: codeblock}

##### Removing custom CDR data
{: #webhook-log-cdr-removing-custom-data}

To remove a key from the CDR custom data, set the  `cdr_custom_data`  context variable with that key set to  `""`.

The following example shows that to remove the  `key1`  field from the custom CDR data, you would overwrite the  `key1`  value with a blank,  `""`

```plaintext
  "context": {
    "integrations": {
      "voice_telephony": {
        "cdr_custom_data": {
          "key1": ""
        }
      }
    }
  }

```
{: codeblock}


