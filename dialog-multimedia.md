---

copyright:
  years: 2015, 2018
lastupdated: "2018-03-19"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:swift: .ph data-hd-programlang='swift'}

# Multimedia responses

If you integrate the conversational skill with Slack or Facebook Messenger, you can specify dialog node responses that include multimedia or interactive elements such as clickable buttons. To specify an interactive response, you insert a block of JSON data into the output of a dialog node.

**Note:** If you want to use interactive messages with Slack, make sure you have enabled the interactive message support. For more information, see the Slack deployment [README ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://github.com/watson-developer-cloud/conversation-connector/blob/master/channels/slack/README.md#interactive-messages){: new_window}.

## Generic JSON format

You can specify interactive responses using a generic JSON format that supports either Slack or Facebook Messenger deployments. To specify an interactive response in the generic JSON format, insert the JSON into the `output.generic` field of the dialog node response. The following example shows how you might send a response containing multiple response types (text, an image, and clickable options):

```json
{
  "output": {
    "generic":[
      {
        "response_type": "text",
        "text": "Here are your nearest stores."
      },
      {
        "response_type": "image",
        "source": "http://...",
        "title": "Image title",
        "description": "Some description for the image"
      },
      {
        "response_type": "option",
        "title": "Click on one of the following",
        "options": [
          {
            "label": "Location 1",
            "value": "Location 1"
          },
          {
            "label": "Location 2",
            "value": "Location 2"
          },
          {
            "label": "Location 3",
            "value": "Location 3"
          }
        ]
      }
    ]
  }
}
```

For more information about the supported response types and how to specify them, see [Response types](#response-types).

At run time, the service converts this response into the format expected by the channel (Slack or Facebook Messenger). If the response contains multiple media types or attachments, the generic response is converted into a series of separate message payloads. The service then sends each message payload to the channel in a separate message.

**Note:** When a response is split into multiple messages, the service sends these messages to the channel in sequence. It is the responsibility of the channel to deliver these messages to the end user; this can be affected by network or server issues.

## Native JSON format

In addition to the generic JSON format, the service also supports channel-specific responses written using the native Slack and Facebook Messenger formats. You might want to use the native JSON formats if you need to specify a response type that is not currently supported by the generic JSON format.

You can specify native JSON for Slack or Facebook using the appropriate field in the dialog node response:

- `output.slack`: insert any JSON you want included in the `attachment` field of the Slack response. For more information about the Slack JSON format, see the Slack [documentation ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://api.slack.com/docs/message-attachments){: new_window}.

- `output.facebook`: insert any JSON you want included in the `message.attachment.payload` field of the Facebook response. For more information about the Facebook JSON format, see the Facebook [documentation ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://developers.facebook.com/docs/messenger-platform/send-messages/templates){: new_window}.

## Response types

The following response types are supported by the generic JSON format.

- [Image](#image)
- [Option](#image)
- [Pause](#image)
- [Text](#image)

### Image
{: #image}

Displays an image specified by a URL.

#### Fields

| Name          | Type   | Description                        | Required? |
|---------------|--------|------------------------------------|-----------|
| response_type | enum   | `image`                            | Y         |
| source        | string | The URL of the image. The specified image must be in .jpg, .gif, or .png format. | Y |
| title         | string | The title to show before the image.| N         |
| description   | string | The text of the description that accompanies the image. | N |

#### Example

This example displays an image with a title and descriptive text.

```json
{
  "response_type": "image",
  "source": "http://example.com/image.jpg",
  "title": "Example image",
  "description": "An example image returned as part of a multimedia response."
}
```

### Option
{: #option}

Displays a set of buttons users can click to choose an option. The specified value is then sent to the conversational skill as user input.

#### Fields

| Name          | Type   | Description                         | Required? |
|---------------|--------|-------------------------------------|-----------|
| response_type | enum   | `option`                            | Y         |
| title         | string | The title to show before the options. | Y       |
| description   | string | The text of the description that accompanies the options. | N |
| options       | list   | A list of key/value pairs specifying the options from which the user can choose. | Y |
| options[].label | string | The user-facing label for the option. | Y     |
| options[].value | string<br/>object | The value that will be sent to the {{site.data.keyword.conversationshort}} service if the user selects the option. This can be either a string (for simple responses) or an object containing multiple fields. See below for examples. | Y |

#### Example

This example displays two options:

- Option 1 (labeled 'Buy something') sends a simple string message (`option_1`), which is sent to the conversational skill using the `input.text` field of the message.
- Option 2 (labeled 'Exit') sends a complex message that includes both input text and an array of intents. The response can include any field that is a valid part of a {{site.data.keyword.conversationshort}} message. (For more information about the structure of message input, see the [API Reference ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/watson/developercloud/conversation/api/v1/?curl#send_message){: new_window}.)

```json
{
  "response_type": "option",
  "title": "Choose from the following options:",
  "options": [
    {
      "label": "Buy something",
      "value": "Place order"
    },
    {
      "label": "Exit",
      "value": {
        "input": {
          "text": "Exit"
        },
        "intent": [
          {
            "intent": "exit_app",
            "confidence": 1.0
          }
        ]
      }
    }
  ]
}
```

### Pause
{: #pause}

Pauses before sending the next message to the channel, and optionally sends a "user is typing" event (for channels that support it).

#### Fields

| Name          | Type   | Description        | Required? |
|---------------|--------|--------------------|-----------|
| response_type | enum   | `pause`            | Y         |
| time          | int    | How long to pause, in milliseconds. | Y |
| typing        | boolean | Whether to send the "user is typing" event during the pause. Ignored if the channel does not support this event. | N |

#### Example

This examples sends the "user is typing" event while pausing for 5 seconds.

```json
{
  "response_type": "pause",
  "time": 5000,
  "typing": true
}
```

### Text
{: #text}

Displays text.

#### Fields

| Name          | Type   | Description        | Required? |
|---------------|--------|--------------------|-----------|
| response_type | enum   | `text`             | Y         |
| text          | string | The text to display. This can include newline characters (`\n`) and Markdown tagging, if supported by the channel. (Any formatting not supported by the channel is ignored.) | Y |

#### Example

This examples displays a text message to the user.

```json
{
  "response_type": "text",
  "text": "This is a text response."
}
```