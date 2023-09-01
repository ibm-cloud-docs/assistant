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

# Phone integration context variables
{: #phone-context}

You can use context variables to manage the flow of conversations with customers who interact with your assistant over the telephone.
{: shortdesc}

The following tables describe the context variables that have special meaning in the context of the phone integration. They should not be used for any purpose other than the documented use.

## Context variables that are set by your dialog or actions
{: #phone-context-variables-set-by-dialog}

| Name | Type | Description | Default |
|------|------|-------------|---------|
| `final_utterance_timeout_count` | Number | The time (in milliseconds) that the phone integration waits to receive a final utterance from the {{site.data.keyword.speechtotextshort}} service. The timeout occurs if the phone integration does not receive a final utterance within the specified time limit, even if hypotheses continue to be generated. When the timeout occurs, the phone integration sends {{site.data.keyword.assistant_classic_short}} a text update that includes the word `vgwFinalUtteranceTimeout` to indicate that no final utterance was received. | N/A |
| `post_response_timeout_count` | Number | The time (in milliseconds) to wait for a new utterance after the response is played back to the caller. When this timeout occurs, the phone integration channel sends a text message to the assistant that includes the word `vgwPostResponseTimeout` and sets the context variable `input.integrations.voice_telephony.post_response_timeout_occurred` to `true`. | 7000 |
| `turn_settings.timeout_count` | Number | The time (in milliseconds) to wait for a response from {{site.data.keyword.assistant_classic_short}}. If this time is exceeded, the phone integration tries again to contact {{site.data.keassistant_classic_shortnshort}}. If the service still can't be reached, the call fails. | N/A |
| `cdr_custom_data` | object | Any JSON key/value pairs to collect and store with the CDR record at the end of the phone call. Each time this object is received, it is merged with any previously received `cdr_custom_data` context. | N/A |
{: caption="Voice context variables set by the dialog or actions" caption-side="top"}

### Example

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
        "turn_settings": {
          "timeout_count": 5000
        },
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

## Context variables that are set by the phone channel
{: #phone-context-variables-set-by-phone-channel}

| Name | Type | Description |
|------|------|-------------|
| `sip_call_id` | string | The SIP call ID associated with the {{site.data.keyword.assistant_classic_short}} session. |
| `sip_custom_invite_headers` | object | A JSON object containing key/value pairs defining SIP headers that are pulled from the initial SIP `INVITE` request and passed to the {{site.data.keyword.assistant_classic_short}} service (for example, `{"Custom-Header1": "123"}`). |
| `private.sip_from_uri` | string | The SIP `From` URI associated with the {{site.data.keyword.assistant_classic_short}} service. |
| `private.sip_request_uri` | string | The SIP request URI that started the conversation session. |
| `private.sip_to_uri` | string | The SIP `To` URI associated with the conversation session. |
| `private.user_phone_number` | string | The phone number that the call was received from. |
| `assistant_phone_number` | string | The phone number associated with the Watson Assistant side that received the phone call. | 
{: caption="Context variables set by the phone channel" caption-side="top"}

## Input parameters that are set by the phone channel
{: #phone-context-input-parameters-set-by-phone-channel}

The following input parameters are only valid for the current conversation turn.

| Name | Type | Description |
|------|------|-------------|
| `post_response_timeout_occurred` | boolean | Whether the post response timeout expired. |
| `barge_in_occurred` | boolean | Whether barge-in occurred. |
| `final_utterance_timeout_occurred` | `true` or `false` | Whether the final utterance timeout expired. |
| `dtmf_collection_succeeded` | boolean | Whether the DTMF collection succeeded or failed. When `true`, a DTMF collection succeeded, and returns the expected number of digits. When `false`, a DTMF collection failed to collect the specified number of digits. Even when `dtmf_collection_succeeded` is `false`, all collected digits are passed to the dialog in the input string of the turn request. |
| `is_dtmf` | boolean | Whether the input to {{site.data.keyword.assistant_classic_short}} is dual-tone multi-frequency signaling (DTMF). |
| `speech_to_text_result` | object | The final response from the {{site.data.keyword.speechtotextshort}} service in JSON format, including the transcript and confidence score for the top hypothesis and any alternatives. The format matches exactly the format that is received from the {{site.data.keyword.speechtotextshort}} service. (For more information, see the [{{site.data.keyword.speechtotextshort}} API documentation](https://cloud.ibm.com/apidocs/speech-to-text#recognize){: external}.) |
{: caption="Input parameters set by the phone channel" caption-side="top"}

### Example

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

