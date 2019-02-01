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

# Implementing responses
{: #dialog-api-responses}

A dialog node can respond to users with a response that includes text, images, or interactive elements such as clickable options. If you are building your own client application, you must implement the correct display of all response types returned by your dialog. (For more information about dialog responses, see [Responses](dialog-overview.html#responses)).

## Response output format
{: #dialog-api-responses-output}

By default, responses from a dialog node are specified in the `output.generic` object in the response JSON returned from the /message API. The `generic` object contains an array of up to 5 response elements that are intended for any channel. The following JSON example shows a response that includes text and an image:

```json
{
  "output": {
    "generic": [
      {
        "response_type": "text",
        "text": "OK, here's a picture of a dog."
      },
      {
        "response_type": "image",
        "source": "http://example.com/dog.jpg"
      }
    ],
    "text" : ["OK, here's a picture of a dog."]
  }
}
```

**Note:** As this example shows, the text response (`OK, here's a picture of a dog.`) is also returned in the `output.text` array. This is included for backward compatibility for applications that do not support the `output.generic` format.

It is the reponsibility of your client application to handle all response types appropriately. In this case, your application would need to display the specified text and image to the user.

## Response types
{: #dialog-api-responses-types}

Each element of a response is of one of the supported response types (currently `image`, `option`, `pause`, and `text`). Each response type is specified using a different set of JSON properties, so the properties included for each response will vary depending upon response type. For complete information about the response model of the /message API, see the [API Reference ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://cloud.ibm.com/apidocs/assistant?curl=#get-response-to-user-input){: new_window}.)

This section describes the available response types and how they are represented in the /message API response JSON. (If you are using the Watson SDK, you can use the interfaces provided for your language to access the same objects.)

**Note:** The examples in this section show the format of the JSON data returned from the /message API at run time. Keep in mind that this is different from the JSON format used to define responses within a dialog node. For more information, see [Defining responses using the JSON editor](dialog-responses-json.html)).

### Image
{: #dialog-api-responses-image}

The `image` response type instructs the client application to display an image, optionally accompanied by a title and description:

```json
{
  "output": {
    "generic":[
      {
        "response_type": "image",
        "source": "http://example.com/image.jpg",
        "title": "Image example",
        "description": "This is an example image"
      }
    ]
  }
}
```

Your application is responsible for retrieving the image specified by the `source` property and displaying it to the user. If the optional `title` and `description` are provided, your application can display them in whatever way is appropriate (for example, rendering the title below the image and the description as hover text).

### Option
{: #dialog-api-responses-option}

The `option` response type instructs the client application to display a user interface control that enables the user to select from a list of options, and then send input back to the {{site.data.keyword.conversationshort}} service based on the selected option:

```json
{
  "output": {
    "generic":[
      {
        "response_type": "option",
        "title": "Available options",
        "description": "Please select one of the following options:",
        "preference": "button",
        "options": [
          {
            "label": "Option 1",
            "value": {
              "input": {
                "text": "option 1"
              }
            }
          },
          {
            "label": "Option 2",
            "value": {
              "input": {
                "text": "option 2"
              }
            }
          }
        ]
      }
    ]
  }
}
```

Your app can display the specified options using any suitable user-interface control (for example, a set of buttons or a drop-down list). The optional `preference` property indicates the preferred type of control your app should use (`button` or `dropdown`), if supported.

For each option, the `label` property specifies the label text that should appear for the option in the UI control. The `value` property specifies the input that should be sent back to the {{site.data.keyword.conversationshort}} service (using the /message API) when the user selects the corresponding option. Note that the `value` property can include not only text input but also other input objects such as intents and entities, all of which should be sent to the service.

### Pause
{: #dialog-api-responses-pause}

The `pause` response type instructs the application to wait for a specified interval before displaying the next response:

```json
{
  "output": {
    "generic":[
      {
        "response_type": "pause",
        "time": 500,
        "typing": false
      }
    ]
  }
}
```

This pause might be requested by the dialog to allow time for a request to complete, or simply to mimic the appearance of a human agent who might pause between responses. The pause can be of any duration up to 10 seconds.

A `pause` response is typically sent in combination with other responses. Your application should pause for the interval specified by the `time` property (in milliseconds) before displaying the next response in the array. The optional `typing` property requests that the client application show a "user is typing" indicator, if supported, in order to simulate a human agent.

### Text
{: #dialog-api-responses-text}

The `text` response type is used for ordinary text responses from the dialog:

```json
{
  "output": {
    "generic":[
      {
        "response_type": "text",
        "text": "OK, you want to fly to Boston next Monday."
      }
    ]
  }
}
```

Note that for backward compatibility, the same text is also included in the `output.text` array in the dialog response (as shown in the example at the beginning of this page).