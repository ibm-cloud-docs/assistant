---

copyright:
  years: 2020, 2021
lastupdated: "2021-04-14"

subcollection: assistant

---

{{site.data.keyword.attribute-definition-list}}

Documentation for the **classic {{site.data.keyword.assistant_classic_short}}** experience has moved. For the most up-to-date version, see [SMS integration reference](/docs/watson-assistant?topic=watson-assistant-sms-reference){: external}.
{: attention}

# *SMS with Twilio* integration reference
{: #commands-sms}

Add action commands to the message `context` object to manage the flow of conversations with customers who interact with your assistant by submitting SMS messages over the telephone.
{: shortdesc}

Learn about the supported commands and reserved context variables that are used by the *SMS with Twilio* integration.

## Supported commands
{: #commands-sms-actions}

Each action consists of a `command` property, followed by an optional `parameter` property to define parameters for commands that require them. The commands that are described in the following table are supported by the *SMS with Twilio* integration.

| Action command | Description | Parameters |
| ----- | ----- | ----- |
| `smsActForceNoInputTurn` | Forces a new turn in the conversation without waiting for input from the user. The *SMS with Twilio* integration sends a message request with `smsNoInputTurn` in the text field so that you can map this request to an intent in your dialog. | None |
| `terminateSession` | Ends the current SMS session. Use this command to ensure that the subsequent text message starts a new assistant-level session which does not retain any context values from the current session. | None |
| `smsActSendMedia` | Enables MMS messaging.  | `mediaURL`: Specifies a JSON array of publicly accessible media URLs that are sent to the user. |
{: caption="Table 1. Actions that you can initiate from the dialog" caption-side="top"}

## Reserved context variables
{: #commands-sms-context-variables}

The following table describes the context variables that have special meaning in the context of the *SMS with Twilio* integration. They should not be used for any purpose other than the documented use.

Table 2 describes the context variables that you can set by the *SMS with Twilio* integration.

### Context variables that are set by the integration
{: #commands-sms-context-variables-set-by-integration}

| Context variable name | Description |
| --------------------- | ----------- |
| `assistant_phone_number` | The phone number associated with the assistant that received the text message. |
| `private.user_phone_number` | The phone number that the text message was received from. |
{: caption="SMS context variables set by the integration" caption-side="top"}

Example:

```json
{
  "context" : {
   "global" : {...},
   "skills" : {...},
   "integrations" : {
      "text_messaging": {
          "private":{
            "user_phone_number":"+12223456789",
          }
          "assistant_phone_number":"+18883456789"
      }
    }
  }
}
```
{: codeblock}
