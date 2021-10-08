---

copyright:
  years: 2019, 2021
lastupdated: "2021-06-17"

keywords: log webhook

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
{:video: .video}

{{site.data.content.newlink}}

# Logging activity with a webhook ![Enterprise plan only](images/enterprise.png)
{: #webhook-log}

You can log activity by making a call to an external service or application every time a customer submits input to the assistant.
{: shortdesc}

A webhook is a mechanism that allows you to call out to an external program based on events in your program.

This feature is available only to Enterprise plan users.

Add a log webhook to your assistant if you want to use an external service to log {{site.data.keyword.conversationshort}} activity. You can log two kinds of activity:

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

1. Click the appropriate checkboxes to select which kinds of activity you want to log:

    - To log messages and responses, select **Subscribe to conversation logs**.
    - To log CDR events for the phone integration, select **Subscribe to CDR (Call Detail Records)**.

1. In the Headers section, add any headers that you want to pass to the service one at a time by clicking **Add header**.

    The service automatically sends an `Authorization` header with a JWT; you do not need to add one. If you want to handle authorization yourself, add your own authorization header and it will be used instead.

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

```
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
  "payload": {
    "active_calls": 1,
    "assistant_interaction_summaries": [
      {
        "assistant_id": "21f4fd4f-a85c-40eb-a29c-d69a31e3d800",
        "session_id": "2a3b6954-ea7a-40ab-9729-e85fcf2c786f",
        "start_timestamp": "2021-04-21T16:31:06.696Z",
        "stop_timestamp": "2021-04-21T16:31:28.345Z",
        "turns": [
          {
            "assistant": {
              "log_id": "928409de-0ca1-4e77-b855-764233f8142b",
              "response_milliseconds": 156
            },
            "request": {
              "type": "start"
            },
            "response": [
              {
                "barge_in_occurred": true,
                "streaming_statistics": {
                  "response_milliseconds": 301,
                  "start_timestamp": "2021-04-21T16:31:08.352Z",
                  "stop_timestamp": "2021-04-21T16:31:12.149Z",
                  "transaction_id": "3dce431c-fb2f-4b62-9fce-585f4e06fe00"
                },
                "type": "text_to_speech"
              }
            ]
          },
          {
            "assistant": {
              "log_id": "d8bc82b8-9bad-4b3b-a771-d8f0e7f359ae",
              "response_milliseconds": 181
            },
            "request": {
              "streaming_statistics": {
                "confidence": 0.99,
                "echo_detected": false,
                "response_milliseconds": 624,
                "start_timestamp": "2021-04-21T16:31:12.147Z",
                "stop_timestamp": "1970-01-01T00:00:00Z",
                "transaction_id": "a4e97d73-c676-4b56-9f9c-785bb8781059"
              },
              "type": "speech_to_text"
            },
            "response": [
              {
                "barge_in_occurred": true,
                "streaming_statistics": {
                  "response_milliseconds": 325,
                  "start_timestamp": "2021-04-21T16:31:12.682Z",
                  "stop_timestamp": "2021-04-21T16:31:15.049Z",
                  "transaction_id": "9d819ffe-f9a2-4e95-956b-0036d2d6d6a5"
                },
                "type": "text_to_speech"
              }
            ]
          },
          {
            "assistant": {
              "log_id": "e4118c32-6f79-4837-a5fc-e18f3aadbdb2",
              "response_milliseconds": 157
            },
            "request": {
              "streaming_statistics": {
                "confidence": 0.98,
                "echo_detected": false,
                "response_milliseconds": 1005,
                "start_timestamp": "2021-04-21T16:31:15.048Z",
                "stop_timestamp": "1970-01-01T00:00:00Z",
                "transaction_id": "a4e97d73-c676-4b56-9f9c-785bb8781059"
              },
              "type": "speech_to_text"
            },
            "response": [
              {
                "barge_in_occurred": false,
                "streaming_statistics": {
                  "response_milliseconds": 0,
                  "start_timestamp": "2021-04-21T16:31:15.230Z",
                  "stop_timestamp": "2021-04-21T16:31:17.389Z",
                  "transaction_id": "8f925d45-75e3-48bc-be28-92848d39567d"
                },
                "type": "text_to_speech"
              }
            ]
          },
          {
            "assistant": {
              "log_id": "3784a688-5c67-453e-ba57-a6472d0b9607",
              "response_milliseconds": 102
            },
            "request": {
              "streaming_statistics": {
                "confidence": 0.98,
                "echo_detected": false,
                "response_milliseconds": 1207,
                "start_timestamp": "2021-04-21T16:31:18.929Z",
                "stop_timestamp": "1970-01-01T00:00:00Z",
                "transaction_id": "a4e97d73-c676-4b56-9f9c-785bb8781059"
              },
              "type": "speech_to_text"
            },
            "response": [
              {
                "barge_in_occurred": true,
                "streaming_statistics": {
                  "response_milliseconds": 298,
                  "start_timestamp": "2021-04-21T16:31:19.348Z",
                  "stop_timestamp": "2021-04-21T16:31:24.204Z",
                  "transaction_id": "6f65ad6a-cae0-46cc-829b-f5c6d1fdaed3"
                },
                "type": "text_to_speech"
              }
            ]
          },
          {
            "assistant": {
              "log_id": "899e062c-3fcb-4bf5-b142-cc8ab0d57b9d",
              "response_milliseconds": 97
            },
            "request": {
              "streaming_statistics": {
                "confidence": 0.98,
                "echo_detected": false,
                "response_milliseconds": 1401,
                "start_timestamp": "2021-04-21T16:31:24.204Z",
                "stop_timestamp": "1970-01-01T00:00:00Z",
                "transaction_id": "a4e97d73-c676-4b56-9f9c-785bb8781059"
              },
              "type": "speech_to_text"
            },
            "response": [
              {
                "barge_in_occurred": true,
                "streaming_statistics": {
                  "response_milliseconds": 320,
                  "start_timestamp": "2021-04-21T16:31:24.648Z",
                  "stop_timestamp": "2021-04-21T16:31:28.178Z",
                  "transaction_id": "5a3393bd-8c64-42e7-9874-ff2a721c7771"
                },
                "type": "text_to_speech"
              }
            ]
          },
          {
            "assistant": {
              "log_id": "6b2cd0ec-55e7-4ab5-8714-9ebc7f387254",
              "response_milliseconds": 163
            },
            "request": {
              "streaming_statistics": {
                "confidence": 0.87,
                "echo_detected": false,
                "response_milliseconds": 1354,
                "start_timestamp": "2021-04-21T16:31:28.177Z",
                "stop_timestamp": "1970-01-01T00:00:00Z",
                "transaction_id": "a4e97d73-c676-4b56-9f9c-785bb8781059"
              },
              "type": "speech_to_text"
            },
            "response": []
          },
          {
            "assistant": {
              "log_id": "80585f84-a1fe-4bcf-8737-737b01ed00e3",
              "response_milliseconds": 0
            },
            "request": {
              "type": "hangup"
            },
            "response": []
          }
        ]
      }
    ],
    "call": {
      "end_reason": "callerHangup",
      "milliseconds_elapsed": 21906,
      "outbound": false,
      "security": {
        "media_encrypted": false,
        "signaling_encrypted": true,
        "sip_authenticated": false
      },
      "start_timestamp": "2021-04-21T16:31:07.609Z",
      "stop_timestamp": "2021-04-21T16:31:29.515Z"
    },
    "failure_occurred": false,
    "global_session_id": "17465345_115257202@10.90.150.99",
    "max_response_milliseconds": {
      "assistant": 181,
      "speech_to_text": 1401,
      "text_to_speech": 325
    },
    "primary_phone_number": "+18005550123",    "realtime_transport_network_summary": {
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
    "session_initiation_protocol": {
      "headers": {
        "call_id": "17465345_115257202@10.90.150.99",
        "from_uri": "sip:+18885550456@pstn.twilio.com",
        "to_uri": "sip:+18005550123@public.voip.us-south.assistant.test.watson.cloud.ibm.com"
      },
      "invite_arrival_time": "2021-04-21T16:31:06.078Z",
      "setup_milliseconds": 1531
    },
    "transfer_occurred": false,
    "warnings_and_errors": []
  },
  "event": {
    "name": "cdr_logged"
  }
}
```
{: codeblock}
