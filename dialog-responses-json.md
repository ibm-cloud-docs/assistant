---

copyright:
  years: 2015, 2019
lastupdated: "2018-12-17"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
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

# Defining responses using the JSON editor
{: #dialog-responses-json}

In some situations, you might need to define responses using the JSON editor. (For more information about dialog responses, see [Responses](dialog-overview.html#responses)). Editing the response JSON gives you direct access to the data that will be returned to the communication channel or custom application.

## Generic JSON format
{: #dialog-responses-json-generic}

The generic JSON format for responses is used to specify responses that are intended for any channel. This format can accommodate various response types that are supported by Slack and Facebook integrations, and can also be implemented by a custom client application. (This is the format that is used by default for dialog responses defined using the {{site.data.keyword.conversationshort}} tool.)

For information about how to open the JSON editor for a dialog node response from the tool, see [Context variables in the JSON editor](dialog-runtime.html#context-var-json).

To specify an interactive response in the generic JSON format, insert the appropriate JSON objects into the `output.generic` field of the dialog node response. The following example shows how you might send a response containing multiple response types (text, an image, and clickable options):

```json
{
  "output": {
    "generic":[
      {
        "response_type": "text",
        "values": [
          {
            "text": "Here are your nearest stores."
          }
        ]
      },
      {
        "response_type": "image",
        "source": "http://example.com/image.jpg",
        "title": "Example image",
        "description": "Some description for the image."
      },
      {
        "response_type": "option",
        "title": "Click on one of the following",
        "options": [
          {
            "label": "Location 1",
            "value": {
              "input": {
                "text": "Location 1"
              }
            }
          },
          {
            "label": "Location 2",
            "value": {
              "input": {
                "text": "Location 2"
              }
            }
          },
          {
            "label": "Location 3",
            "value": {
              "input": {
                "text": "Location 3"
              }
            }
          }
        ]
      }
    ]
  }
}
```

For more information about how to specify each supported response type using JSON objects, see [Response types](#response-types).

If you are using the {{site.data.keyword.conversationshort}} connector, the response is converted at run time into the format expected by the channel (Slack or Facebook Messenger). If the response contains multiple media types or attachments, the generic response is converted into a series of separate message payloads as needed. The connector then sends each message payload to the channel in a separate message.

**Note:** When a response is split into multiple messages, the {{site.data.keyword.conversationshort}} connector sends these messages to the channel in sequence. It is the responsibility of the channel to deliver these messages to the end user; this can be affected by network or server issues.

If you are building your own client application, your app must implement each response type as appropriate. For more information, see [Implementing responses](api-dialog-responses.html).

## Native JSON format
{: #dialog-responses-json-native}

In addition to the generic JSON format, the dialog node JSON also supports channel-specific responses written using the native Slack and Facebook Messenger formats. These formats are also supported by the {{site.data.keyword.conversationshort}} connector. You might want to use the native JSON formats if you know your workspace will only be integrated with one channel type, and you need to specify a response type that is not currently supported by the generic JSON format.

You can specify native JSON for Slack or Facebook using the appropriate field in the dialog node response:

- `output.slack`: insert any JSON you want to be included in the `attachment` field of the Slack response. For more information about the Slack JSON format, see the Slack [documentation ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://api.slack.com/docs/message-attachments){: new_window}.

- `output.facebook`: insert any JSON you want included in the `message.attachment.payload` field of the Facebook response. For more information about the Facebook JSON format, see the Facebook [documentation ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://developers.facebook.com/docs/messenger-platform/send-messages/templates){: new_window}.

## Response types
{: #dialog-responses-json-response-types}

The following response types are supported by the generic JSON format.

### Image
{: #dialog-responses-json-image}

Displays an image specified by a URL.

#### Fields
{: #{: #dialog-responses-json-image-fields}

| Name          | Type   | Description                        | Required? |
|---------------|--------|------------------------------------|-----------|
| response_type | enum   | `image`                            | Y         |
| source        | string | The URL of the image. The specified image must be in .jpg, .gif, or .png format. | Y |
| title         | string | The title to show before the image.| N         |
| description   | string | The text of the description that accompanies the image. | N |

#### Example
{: #dialog-responses-json-image-example}

This example displays an image with a title and descriptive text.

```json
{
  "output": {
    "generic":[
      {
        "response_type": "image",
        "source": "http://example.com/image.jpg",
        "title": "Example image",
        "description": "An example image returned as part of a multimedia response."
      }
    ]
  }
}
```

### Option
{: #dialog-responses-json-option}

Displays a set of buttons or a drop-down list users can use to choose an option. The specified value is then sent to the workspace as user input.

#### Fields
{: #dialog-responses-json-option-fields}

| Name          | Type   | Description                         | Required? |
|---------------|--------|-------------------------------------|-----------|
| response_type | enum   | `option`                            | Y         |
| title         | string | The title to show before the options. | Y       |
| description   | string | The text of the description that accompanies the options. | N |
| preference    | enum   | The preferred type of control to display, if supported by the channel (`dropdown` or `button`). The {{site.data.keyword.conversationshort}} connector currently supports only `button`.| N |
| options       | list   | A list of key/value pairs specifying the options from which the user can choose. | Y |
| options[].label | string | The user-facing label for the option. | Y     |
| options[].value | object | An object defining the response that will be sent to the {{site.data.keyword.conversationshort}} service if the user selects the option. | Y |
| options[].value.input | object | An input object that includes the input text corresponding to the option. | N |
| options[].value.input.text | string | The text that will be sent to the service for the option. | N |

#### Example
{: #dialog-responses-json-option-example}

This example displays two options:

- Option 1 (labeled `Buy something`) sends a simple string message (`Place order`), which is sent to the workspace as the input text.
- Option 2 (labeled `Exit`) sends a complex message that includes both input text and an array of intents. The response can include any field that is a valid part of a {{site.data.keyword.conversationshort}} message. (For more information about the structure of message input, see the [API Reference ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://cloud.ibm.com/apidocs/assistant?curl=#get-response-to-user-input){: new_window}.)

```json
{
  "output": {
    "generic":[
      {
        "response_type": "option",
        "title": "Choose from the following options:",
        "preference": "button",
        "options": [
          {
            "label": "Buy something",
            "value": {
              "input": {
                "text": "Place order"
              }
            }
          },
          {
            "label": "Exit",
            "value": {
              "input": {
                "text": "Exit"
              },
              "intents": [
                {
                  "intent": "exit_app",
                  "confidence": 1.0
                }
              ]
            }
          }
        ]
      }
    ]
  }
}
```

### Pause
{: #dialog-responses-json-pause}

Pauses before sending the next message to the channel, and optionally sends a "user is typing" event (for channels that support it).

#### Fields
{: #dialog-responses-json-pause-fields}

| Name          | Type   | Description        | Required? |
|---------------|--------|--------------------|-----------|
| response_type | enum   | `pause`            | Y         |
| time          | int    | How long to pause, in milliseconds. | Y |
| typing        | boolean | Whether to send the "user is typing" event during the pause. Ignored if the channel does not support this event. | N |

#### Example
{: #dialog-responses-json-pause-example}

This examples sends the "user is typing" event while pausing for 5 seconds.

```json
{
  "output": {
    "generic":[
      {
        "response_type": "pause",
        "time": 5000,
        "typing": true
      }
    ]
  }
}
```

### Text
{: #dialog-responses-json-text}

Displays text. To add variety, you can specify multiple alternative text responses. If you specify multiple responses, you can choose to rotate sequentially through the list, choose a response randomly, or output all specified responses.

#### Fields
{: #dialog-responses-json-text-fields}

| Name          | Type   | Description        | Required? |
|---------------|--------|--------------------|-----------|
| response_type | enum   | `text`             | Y         |
| values        | list   | A list of one or more objects defining text response. | Y |
| values.[_n_].text   | string | The text of a response. This can include newline characters (`\n`) and Markdown tagging, if supported by the channel. (Any formatting not supported by the channel is ignored.) | N |
| selection_policy | string | How a response is selected from the list, if more than one response is specified. The possible values are `sequential`, `random`, and `multiline`. | N |
| delimiter     | string | The delimiter to output as a separator between responses. Used only when `selection_policy`=`multiline`. The default delimiter is newline (`\n`). | N |

#### Example
{: #dialog-responses-json-text-example}

This examples displays a greeting message to the user.

```json
{
  "output": {
    "generic":[
      {
        "response_type": "text",
        "values": [
          { "text": "Hello." },
          { "text": "Hi there." }
        ],
        "selection_policy": "random"
      }
    ]
  }
}  
```