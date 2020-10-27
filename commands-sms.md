---

copyright:
  years: 2015, 2020
lastupdated: "2020-10-19"

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

# SMS with Twilio integration reference ![Beta](images/beta.png)
{: #commands-sms}

Add action commands to the message `context` object to manage the flow of conversations with customers who interact with your assistant by submitting SMS messages over the telephone.
{: shortdesc}

The SMS with Twilio integration is available as a beta feature.
{: note}

Learn about the supported commands and reserved context variables that are used by the SMS with Twilio integration.

## Supported commands
{: #commands-sms-actions}

Each action consists of a `command` property, followed by an optional `parameter` property to define parameters for commands that require them. The commands that are described in the following table are supported by the SMS with Twilio integration.

| Action command | Description | Parameters |
| ----- | ----- | ----- |
| `smsActForceNoInputTurn` | Forces a new turn in the conversation without waiting for input from the user. The SMS with Twilio integration sends a message request with `smsNoInputTurn` in the text field so that you can map this request to an intent in your dialog. | None |
| `terminateSession` | Ends the current SMS session. Use this command to ensure that the subsequent text message starts a new assistant-level session which does not retain any context values from the current session. | None |
| `smsActSendMedia` | Enables MMS messaging.  | `mediaURL`: Specifies a JSON array of publicly accessible media URLs that are sent to the user. |
| `smsActSetDisambiguationConfig` | Configures how to handle the choices that are displayed in a disambiguation list. | <ul><li>`prefixText`: Text to include before each option. For example, `Press %s for` where `%s` represents the number corresponding to a list choice; this is replaced with the actual number at run time.</li></ul> |
| `smsActSetOptionsConfig` | Configures how to handle option response types. | <ul><li>`prefixText`: Text to include before each option. For example, `Press %s for` where `%s` represents the number corresponding to a list choice; this is replaced with the actual number at run time.</li></ul> |
{: caption="Table 1. Actions that you can initiate from the dialog" caption-side="top"}

## Reserved context variables
{: #commands-sms-context-variables}

The following table describes the context variables that have special meaning in the context of the SMS with Twilio integration. They should not be used for any purpose other than the documented use.

Table 2 describes the context variables that are set from your dialog. Table 3 describes the context variables that you can set by the phone integration.

### Table 2. Context variables that are set by your dialog
{: #commands-sms-context-variables-set-by-dialog}

| Context variable name | Expected value | Description |
| --------------------- | -------------- | ----------- |
| `smsConversationResponseTimeout` | Time in ms | The amount of time in milliseconds that the integration waits to receive a response from the dialog. If the time limit is exceeded, the integration attempts to contact the dialog again. If the service still can't be reached, the SMS response fails. |
{: caption="Table 2. Voice context variables set by the dialog" caption-side="top"}

### Table 3. Context variables that are set by the integration
{: #commands-sms-context-variables-set-by-integration}

| Context variable name | Description |
| --------------------- | ----------- |
| `smsError` | When the integration fails to send an SMS message, this variable contains details about the error that occurred.  |
| `smsSessionID` | The globally unique identifier (GUID) for the related SMS Gateway session. |
| `smsMedia` | The `arraylist` of `mediaURL` and corresponding `mediaContentType`. This context variable is cleared at the end of each conversation turn. |
{: caption="Table 3. Voice context variables set by the integration" caption-side="top"}
