---

copyright:
  years: 2015, 2021
lastupdated: "2021-10-21"

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

# Defining responses using the JSON editor
{: #dialog-responses-json}

In some situations, you might need to define responses using the JSON editor. (For more information about dialog responses, see [Responses](/docs/assistant?topic=assistant-dialog-overview#dialog-overview-responses)). Editing the response JSON gives you direct access to the data that will be returned to the integration or custom client.

## Generic JSON format
{: #dialog-responses-json-generic}

The generic JSON format for responses is used to specify responses that are intended for any integration or custom client. This format can accommodate various response types that are supported by multiple integrations, and can also be implemented by a custom client application. (This is the format that is used by default for dialog responses defined using the {{site.data.keyword.conversationshort}} tool.)

For information about how to open the JSON editor for a dialog node response from the tool, see [Context variables in the JSON editor](/docs/assistant?topic=assistant-dialog-runtime-context#dialog-runtime-context-var-json).

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
        "source": "https://example.com/image.jpg",
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

For more information about how to specify each supported response type using JSON objects, see [Response types](#dialog-responses-json-response-types).

If you are using an integration, the response is converted at run time into the format expected by the channel. If the response contains multiple media types or attachments, the generic response is converted into a series of separate message payloads as needed. These are sent to the channel in separate messages.

When a response is split into multiple messages, the integration sends these messages to the channel in sequence. It is the responsibility of the channel to deliver these messages to the end user; this can be affected by network or server issues.
{: note}

If you are building your own client application, your app must implement each response type as appropriate. For more information, see [Implementing responses](/docs/assistant?topic=assistant-api-dialog-responses).

## Native JSON format
{: #dialog-responses-json-native}

In addition to the generic JSON format, the dialog node JSON also supports channel-specific responses written using the native JSON format for a specific channel (such as Slack or Facebook Messenger). You might want to use the native JSON format to specify a response type that is not currently supported by the generic JSON format. By checking the integration-specific context (`context.integrations`), a dialog node can determine the originating integration and then send appropriate native JSON responses.

You can specify native JSON for Slack or Facebook using the `output.integrations` object in the dialog node response. Each child of `output.integrations` represents output intended for a specific integration:

- `output.integrations.slack`: any JSON response you want to be included in the `attachment` field of a response intended for Slack. For more information about the Slack JSON format, see the Slack [documentation](https://api.slack.com/messaging/composing){: external}.

- `output.integrations.facebook`: any JSON you want included in the `message` field of a response intended for Facebook Messenger. For more information about the Facebook JSON format, see the Facebook [documentation](https://developers.facebook.com/docs/messenger-platform/send-messages/templates){: external}.

## Response types
{: #dialog-responses-json-response-types}

The following response types are supported by the generic JSON format.

### `image`
{: #dialog-responses-json-image}

Displays an image specified by a URL.

#### Fields
{: #dialog-responses-json-image-fields}

| Name          | Type   | Description                        | Required? |
|---------------|--------|------------------------------------|-----------|
| response_type | string | `image`                            | Y         |
| source        | string | The `https:` URL of the image. The specified image must be in .jpg, .gif, or .png format. | Y |
| title         | string | The title to show before the image.| N         |
| description   | string | The text of the description that accompanies the image. | N |
| alt_text      | string | Descriptive text that can be used for screen readers or other situations where the image cannot be seen. | N |

#### Example
{: #dialog-responses-json-image-example}

This example displays an image with a title and descriptive text.

```json
{
  "output": {
    "generic":[
      {
        "response_type": "image",
        "source": "https://example.com/image.jpg",
        "title": "Example image",
        "description": "An example image returned as part of a multimedia response."
      }
    ]
  }
}
```

### `video`
{: #dialog-responses-json-video}

Displays a video specified by a URL.

#### Fields
{: #dialog-responses-json-video-fields}

| Name          | Type   | Description                        | Required? |
|---------------|--------|------------------------------------|-----------|
| response_type | string | `video`                            | Y         |
| source        | string | The `https:` URL of the video. The URL can specify either a video file or a streaming video on a supported hosting service. For more information, see [Adding a *Video* response type](/docs/assistant?topic=assistant-dialog-overview#dialog-overview-add-video). | Y |
| title         | string | The title to show before the video.| N         |
| description   | string | The text of the description that accompanies the video. | N |
| alt_text      | string | Descriptive text that can be used for screen readers or other situations where the video cannot be seen. | N |
| channel_options.chat.dimensions.base_height | string | The base height (in pixels) to use to scale the video to a specific display size. | N |

#### Example
{: #dialog-responses-json-video-example}

This example displays an video with a title and descriptive text.

```json
{
  "output": {
    "generic":[
      {
        "response_type": "video",
        "source": "https://example.com/videos/example-video.mp4",
        "title": "Example video",
        "description": "An example video returned as part of a multimedia response."
      }
    ]
  }
}
```

### `audio`
{: #dialog-responses-json-audio}

Plays an audio clip specified by a URL.

#### Fields
{: #dialog-responses-json-audio-fields}

| Name          | Type   | Description                        | Required? |
|---------------|--------|------------------------------------|-----------|
| response_type | string | `audio`                            | Y         |
| source        | string | The `https:` URL of the audio clip. The URL can specify either an audio file or an audio clip on a supported hosting service. For more information, see [Adding an *Audio* response type](/docs/assistant?topic=assistant-dialog-overview#dialog-overview-add-audio). | Y |
| title         | string | The title to show before the audio player.| N  |
| description   | string | The text of the description that accompanies the audio player. | N |
| alt_text      | string | Descriptive text that can be used for screen readers or other situations where the audio player cannot be seen. | N |
| channel_options.voice_telephony.loop | string | Whether the audio clip should repeat indefinitely (phone integration only). | N |

#### Example
{: #dialog-responses-json-audio-example}

This example plays an audio clip with a title and descriptive text.

```json
{
  "output": {
    "generic":[
      {
        "response_type": "audio",
        "source": "https://example.com/audio/example-file.mp3",
        "title": "Example audio file",
        "description": "An example audio clip returned as part of a multimedia response."
      }
    ]
  }
}
```

### `iframe`
{: #dialog-responses-json-iframe}

Embeds content from an external website as an HTML `iframe` element.

#### Fields
{: #dialog-responses-json-iframe-fields}

| Name          | Type   | Description                        | Required? |
|---------------|--------|------------------------------------|-----------|
| response_type | string | `iframe`                           | Y         |
| source        | string | The URL of the external content. The URL must specify content that is embeddable in an HTML `iframe` element. For more information, see [Adding an *iframe* response type](/docs/assistant?topic=assistant-dialog-overview#dialog-overview-add-iframe). | Y |
| title         | string | The title to show before the embedded content.| N |
| description   | string | The text of the description that accompanies the embedded content. | N |
| image_url     | string | The URL of an image that shows a preview of the embedded content. | N |

#### Example
{: #dialog-responses-json-iframe-example}

This example embeds an iframe with a title and description.

```json
{
  "output": {
    "generic":[
      {
        "response_type": "iframe",
        "source": "https://example.com/embeddable/example",
        "title": "Example iframe",
        "description": "An example of embeddable content returned as an iframe response."
      }
    ]
  }
}
```

### `option`
{: #dialog-responses-json-option}

Displays a set of buttons or a drop-down list users can use to choose an option. The specified value is then sent to the workspace as user input.

#### Fields
{: #dialog-responses-json-option-fields}

| Name          | Type   | Description                         | Required? |
|---------------|--------|-------------------------------------|-----------|
| response_type | string | `option`                            | Y         |
| title         | string | The title to show before the options. | Y       |
| description   | string | The text of the description that accompanies the options. | N |
| preference    | string | The preferred type of control to display, if supported by the channel (`dropdown` or `button`). The {{site.data.keyword.conversationshort}} connector currently supports only `button`.| N |
| options       | list   | A list of key/value pairs specifying the options from which the user can choose. | Y |
| options[].label | string | The user-facing label for the option. | Y     |
| options[].value | object | An object defining the response that will be sent to the {{site.data.keyword.conversationshort}} service if the user selects the option. | Y |
| options[].value.input | object | An object that includes the message input corresponding to the option, including input text and any other field that is a valid part of a {{site.data.keyword.conversationshort}} message. For more information about the structure of message input, see the [API Reference](https://cloud.ibm.com/apidocs/assistant/assistant-v2?curl=#message){: external}. | N |

#### Example
{: #dialog-responses-json-option-example}

This example displays two options:

- Option 1 (labeled `Buy something`) sends a simple string message (`Place order`), which is sent to the workspace as the input text.
- Option 2 (labeled `Exit`) sends a complex message that includes both input text and an array of intents.

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

### `pause`
{: #dialog-responses-json-pause}

Pauses before sending the next message to the channel, and optionally sends a "user is typing" event (for channels that support it).

#### Fields
{: #dialog-responses-json-pause-fields}

| Name          | Type   | Description        | Required? |
|---------------|--------|--------------------|-----------|
| response_type | string | `pause`            | Y         |
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

### `text`
{: #dialog-responses-json-text}

Displays text. To add variety, you can specify multiple alternative text responses. If you specify multiple responses, you can choose to rotate sequentially through the list, choose a response randomly, or output all specified responses.

#### Fields
{: #dialog-responses-json-text-fields}

| Name          | Type   | Description        | Required? |
|---------------|--------|--------------------|-----------|
| response_type | string | `text`             | Y         |
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

### `search`
{: #dialog-responses-json-search-skill}

Calls the search skill linked to the assistant to retrieve results that are relevant to the user's query.

#### Fields
{: #dialog-responses-json-search-skill-fields}

| Name          | Type   | Description        | Required? |
|---------------|--------|--------------------|-----------|
| response_type | string | `search`           | Y         |
| query         | string | The text to use for the search query. This string can be empty, in which case the user input is used as the query. | Y |
| filter        | string | An optional filter that narrows the set of documents to be searched. | N |
| query_type    | string | The type of search query to use (`natural_language` or `discovery_query_language`). | Y |
| discovery_version | string | The version of the Discovery service API to use. The default is `2018-12-03`. | N |

#### Example
{: #dialog-responses-json-search-skill-example}

This examples uses the user input text to send a natural-language query to the search skill.

```json
{
  "output": {
    "generic": [
      {
        "response_type": "search_skill",
        "query": "",
        "query_type": "natural_language"
      }
    ]
  }
}
```

### `connect_to_agent`
{: #dialog-responses-json-connect-to-agent}

Requests that the conversation be transferred to a human service desk agent for help.

#### Fields
{: #dialog-responses-json-channel-transfer-fields}

| Name                   | Type   | Description        | Required? |
|------------------------|--------|--------------------|-----------|
| response_type          | string | `connect_to_agent` | Y         |
| message_to_human_agent | string | A message to display to the human agent to whom the conversation is being transferred. | Y |
| agent_available        | string | A message to display to the user when agents are available.                            | Y |
| agent_unavailable      | string | A message to display to the user when no agents are available.                         | Y |
| transfer_info          | object | Information used by the web chat service desk integrations for routing the transfer.   | N |
| transfer_info.target.zendesk.department | string | A valid department from your Zendesk account.                         | N |
| transfer_info.target.salesforce.button_id | string | A valid button ID from your Salesforce deployment.                  | N |

#### Example
{: #dialog-responses-json-connect-to-agent-example}

This example requests a transfer to a human agent and specifies messages to be displayed both to the user and to the agent at the time of transfer.

```json
{
  "output": {
    "generic": [
      {
        "response_type": "connect_to_agent",
        "message_to_human_agent": "User asked to speak to an agent.",
        "agent_available": {
          "message": "Please wait while I connect you to an agent."
        },
        "agent_unavailable": {
          "message": "I'm sorry, but no agents are online at the moment. Please try again later."
        }
      }
    ]
  }
}
```

### `channel_transfer`
{: #dialog-responses-json-channel-transfer}

Requests that the conversation be transferred to a different integration.

#### Fields
{: #dialog-responses-json-channel-transfer-fields}

| Name          | Type   | Description        | Required? |
|---------------|--------|--------------------|-----------|
| response_type | string | `channel_transfer` | Y         |
| message_to_user | string | A message to display to the user before the link for initiating the transfer. | Y |
| transfer_info | object | Information used by an integration to transfer the conversation to a different channel. | Y |
| transfer_info.target.chat | string | The URL for the website hosting the web chat to which the conversation is to be transferred. | Y |

#### Example
{: #dialog-responses-json-channel-transfer-example}

This example requests a transfer from Slack to web chat. In addition to the `channel_transfer` response, the output also includes a `text` response to be displayed by the web chat integration after the transfer. The use of the `channels` array ensures that the `channel_transfer` response is handled only by the Slack integration (before the transfer), and the `connect_to_agent` response only by the web chat integration (after the transfer). For more information about using `channels` to target specific integrations, see [Targeting specific integrations](#dialog-responses-json-target-integrations).

```json
{
  "output": {
    "generic": [
      {
        "response_type": "channel_transfer",
        "channels": [
          {
            "channel": "whatsapp"
          }
        ],
        "message_to_user": "Click the link to connect with an agent using our website.",
        "transfer_info": {
          "target": {
            "chat": {
              "url": "https://example.com/webchat"
            }
          }
        }
      },
      {
        "response_type": "connect_to_agent",
        "channels": [
          {
            "channel": "chat"
          }
        ],
        "message_to_human_agent": "User asked to speak to an agent.",
        "agent_available": {
          "message": "Please wait while I connect you to an agent."
        },
        "agent_unavailable": {
          "message": "I'm sorry, but no agents are online at the moment. Please try again later."
        },
        "transfer_info": {
          "target": {
            "zendesk": {
              "department": "Payments department"
            }
          }
        }
      }
    ]
  }
}
```

### `dtmf`
{: #dialog-responses-json-dtmf}

Sends commands to the phone integration to control input or output using dual-tone multi-frequency (DTMF) signals. (DTMF is a protocol used to transmit the tones that are generated when a user presses keys on a push-button phone.)

#### Fields
{: #dialog-responses-json-dtmf-fields}

| Name          | Type   | Description        | Required? |
|---------------|--------|--------------------|-----------|
| response_type | string | `dtmf`             | Y         |
| command_info  | object | Information specifying the DTMF command to send to the phone integration. | Y |
| command_info.type | string | The DTMF command to send. The following commands are supported: - `collect` \n - `disable_barge_in` \n - `enable_barge_in` \n - `send` | Y |
| command_info.parameters | object | Parameters related to the DTMF command. | N |
| command_info.parameters.termination_key | string | The DTMF termination key that signals the end of DTMF input (for example, `#`). Used with the `collect` command. | N |
| command_info.parameters.count | number | The number of DTMF digits to collect. Must be an integer between 1 and 100. Used with the `collect` command. | Required only if `termination_key`, or `minimum_count` and `maximum_count`, are not specified. |
| command_info.parameters.minimum_count | The minimum number of DTMF digits to collect. Used along with `maximum_count` to define a range for the number of digits to collect. This number must be a positive integer between 1 and `maximum_count`. | Required only if `termination_key` and `count` are not specified. |

#### Example

### `stop_activities`
{: #dialog-responses-json-stop-activities}

#### Fields

#### Example

### `start_activities`
{: #dialog-responses-json-start-activities}

#### Fields

#### Example

### `text_to_speech`
{: #dialog-responses-json-text-to-speech}

#### Fields

#### Example

### `speech_to_text`

#### Fields

#### Example

### `user_defined`
{: #dialog-responses-json-user-defined}

A custom response type containing any JSON data the client or integration knows how to handle. For example, you might customize the web chat to display a special kind of card, or build a custom application to format responses using a table or chart.

The user-defined response type is not displayed unless you have implemented code specifically to handle it. For more information about customizing the web chat, see [Applying advanced customizations](/docs/assistant?topic=assistant-web-chat-config). For more information about handling responses in a custom client app, see [Implementing responses](/docs/assistant?topic=assistant-api-dialog-responses).
{: note}

#### Fields
{: #dialog-responses-json-user-defined-fields}

| Name          | Type   | Description        | Required? |
|---------------|--------|--------------------|-----------|
| response_type | string | `user_defined`     | Y         |
| user_defined  | object | An object containing any data the client or integration knows how to handle. This object can contain any valid JSON data, but it cannot exceed a total size of 5000 bytes. | Y |

#### Example
{: #dialog-responses-json-user-defined-example}

This examples shows a generic example of a user-defined response. The `user_defined` object can contain any valid JSON data.

```json
{
  "output": {
    "generic":[
      {
        "response_type": "user_defined",
        "user_defined": {
          "field_1": "String value",
          "array_1": [
            1,
            2
          ],
          "object_1": {
            "property_1": "Another string value"
          }
        }
      }
    ]
  }
}
```

## Targeting specific integrations
{: #dialog-responses-json-target-integrations}

If you plan to use integrations to deploy your assistant to multiple channels, you might want to send different responses to different integrations.

This mechanism is useful if your dialog flow does not change based on the integration in use, and if you cannot know in advance what integration the response will be sent to at run time. By using `channels`, you can define a single dialog node that supports all integrations, while still customizing the output for each channel. For example, you might want to customize the text formatting, or even send different response types, based on what the channel supports.

Using `channels` is particularly useful in conjunction with the `channel_transfer` response type. Because the message output is processed both by the channel initiating the transfer and by the target channel, you can use `channels` to define responses that will only be displayed by one or the other. (For more information, and an example, see [Channel transfer](#dialog-responses-json-channel-transfer).)

To specify the integrations for which a response is intended, include the optional `channels` array as part of the response object. All response types support the `channels` array. This array contains one or more objects using the following syntax:

```json
{
  "channel": "<channel_name>"
}
```

The value of `<channel_name>` can be any of the following strings:

- **`chat`**: Web chat
- **`slack`**: Slack
- **`facebook`**: Facebook Messenger
- **`intercom`**: Intercom
- **`whatsapp`**: WhatsApp

Currently, the `channels` array is not supported by the phone integration or the SMS with Twilio integration. If your assistant is deployed to either of these integrations, do not use `channels` to target responses.
{: note}

The following example shows dialog node output that contains two responses: one intended for the web chat integration and one intended for the Slack and Facebook integrations.

```json
{
  "output": {
    "generic": [
      {
        "response_type": "text",
        "channels": [
          {
            "channel": "chat"
          }
        ],
        "text" : "This output is intended for the <strong>web chat</strong>."
      },
      {
        "response_type": "text",
        "channels": [
          {
            "channel": "slack"
          },
          {
            "channel": "facebook"
          }
        ],
        "text" : "This output is intended for either Slack or Facebook."
      }
    ]
  }
}
```

If the `channels` array is present, it must contain at least one channel object. Any integration that is not listed ignores the response. If the `channels` array is absent, all integrations handle the response.

**Note:** If you need to change the logic of your dialog flow based on which integration is in use, or based on context data that is specific to a particular integration, see [Adding custom dialog flows for integrations](/docs/assistant?topic=assistant-dialog-integrations).
