---

copyright:
  years: 2015, 2022
lastupdated: "2022-06-13"

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

# Implementing responses
{: #api-dialog-responses}

A dialog node can respond to users with a response that includes text, images, or interactive elements such as clickable options. If you are building your own client application, you must implement the correct display of all response types returned by your dialog. (For more information about dialog responses, see [Responses](/docs/assistant?topic=assistant-dialog-overview#dialog-overview-responses)).

## Response output format
{: #api-dialog-responses-output}

By default, responses from a dialog node are specified in the `output.generic` object in the response JSON returned from the `/message` API. The `generic` object contains an array of up to 5 response elements that are intended for any channel. The following JSON example shows a response that includes text and an image:

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
  },
  "user_id": "faf4a112-f09f-4a95-a0be-43c496e6ac9a"
}
```

As this example shows, the text response (`OK, here's a picture of a dog.`) is also returned in the `output.text` array. This is included for backward compatibility for older applications that do not support the `output.generic` format.
{: note}

It is the reponsibility of your client application to handle all response types appropriately. In this case, your application would need to display the specified text and image to the user.

## Response types
{: #api-dialog-responses-types}

Each element of a response is of one of the supported response types (currently `image`, `option`, `pause`, `text`, `suggestion`, and `user_defined`). Each response type is specified using a different set of JSON properties, so the properties included for each response will vary depending upon response type. For complete information about the response model of the `/message` API, see the [API Reference](https://{DomainName}/apidocs/assistant/assistant-v2#message){: external}.)

This section describes the available response types and how they are represented in the `/message` API response JSON. (If you are using the Watson SDK, you can use the interfaces provided for your language to access the same objects.)

The examples in this section show the format of the JSON data returned from the `/message API` at run time, and is different from the JSON format used to define responses within a dialog node. You can use the format in the examples to update `output.generic` with [webhooks](/docs/assistant?topic=assistant-dialog-webhooks#dialog-webhooks-outputgeneric). To update `output.generic` using the JSON editor, see [Defining responses using the JSON editor](/docs/assistant?topic=assistant-dialog-responses-json)).
{: note}

### Text
{: #api-dialog-responses-text}

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
  },
  "user_id": "faf4a112-f09f-4a95-a0be-43c496e6ac9a"
}
```

Note that for backward compatibility, the same text is also included in the `output.text` array in the dialog response (as shown in the example at the beginning of this page).

### Image
{: #api-dialog-responses-image}

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
  },
  "user_id": "faf4a112-f09f-4a95-a0be-43c496e6ac9a"
}
```

Your application is responsible for retrieving the image specified by the `source` property and displaying it to the user. If the optional `title` and `description` are provided, your application can display them in whatever way is appropriate (for example, rendering the title after the image and the description as hover text).

### Video
{: #api-dialog-responses-video}

The `video` response type instructs the client application to display a video, optionally accompanied by a `title`, `description` and `alt_text` for accessibility:

```json
{
  "output": {
    "generic":[
      {
        "response_type": "video",
        "source": "http://example.com/video.mp4",
        "title": "Video example",
        "description": "This is an example video",
        "alt_text": "A video showing a great example",
        "channel_options": {
          "chat": {
            "dimensions": {
              "base_height": 180
            }
          }
        }
      }
    ]
  },
  "user_id": "faf4a112-f09f-4a95-a0be-43c496e6ac9a"
}
```

Your application is responsible for retrieving the video specified by the `source` property and displaying it to the user. If the optional `title` and `description` are provided, your application can display them in whatever way is appropriate.

The optional `channel_options.chat.dimensions.base_height` property takes a number signifying the amount of pixels the video should be rendered at a width of 360 pixels. Your app should use this value to maintain the proper aspect ratio of the video if it is rendered in a nonstandard size. 

### Audio
{: #api-dialog-responses-audio}

The `audio` response type instructs the client application to play an audio file:

```json
{
  "output": {
    "generic":[
      {
        "response_type": "audio",
        "source": "http://example.com/audio.mp3",
        "channel_options": {
          "voice_telephony": {
            "loop": true
          }
        }
      }
    ]
  },
  "user_id": "faf4a112-f09f-4a95-a0be-43c496e6ac9a"
}
```

Your application is responsible for playing the audio file.

The optional `channel_options.voice_telephony.loop` property takes a boolean signifying if the audio file should be played as a continuous loop. (This option is typically used for hold music that might need to continue for an undefined time.)

### iframe
{: #api-dialog-responses-iframe}

The `iframe` response type instructs the client application to display content in an embedded `iframe` element, optionally accompanied by a title:

```json
{
  "output": {
    "generic":[
      {
        "response_type": "iframe",
        "source": "http://example.com/iframe.html",
        "title": "My IFrame"
      }
    ]
  },
  "user_id": "faf4a112-f09f-4a95-a0be-43c496e6ac9a"
}
```

Your application is responsible for displaying the `iframe` content. Content in an embedded `iframe` is useful for displaying third-party content, or for content from your own site that you do not want to reauthor using the [`user_defined`](#api-dialog-responses-user-defined) response type.

### Pause
{: #api-dialog-responses-pause}

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
  },
  "user_id": "faf4a112-f09f-4a95-a0be-43c496e6ac9a"
}
```

This pause might be requested by the dialog to allow time for a request to complete, or simply to mimic the appearance of a human agent who might pause between responses. The pause can be of any duration up to 10 seconds.

A `pause` response is typically sent in combination with other responses. Your application should pause for the interval specified by the `time` property (in milliseconds) before displaying the next response in the array. The optional `typing` property requests that the client application show a "user is typing" indicator, if supported, in order to simulate a human agent.

### Option
{: #api-dialog-responses-option}

The `option` response type instructs the client application to display a user interface control that enables the user to select from a list of options, and then send input back to the assistant based on the selected option:

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
  },
  "user_id": "faf4a112-f09f-4a95-a0be-43c496e6ac9a"
}
```

Your app can display the specified options using any suitable user-interface control (for example, a set of buttons or a drop-down list). The optional `preference` property indicates the preferred type of control your app should use (`button` or `dropdown`), if supported. For the best user experience, a good practice is to present three or fewer options as buttons, and more than three options as a drop-down list.

For each option, the `label` property specifies the label text that should appear for the option in the UI control. The `value` property specifies the input that should be sent back to the assistant (using the `/message` API) when the user selects the corresponding option.

For an example of implementing `option` responses in a simple client application, see [Example: Implementing option responses](#api-dialog-option-example).

### Suggestion
{: #api-dialog-responses-suggestion}

This feature is available only to users with a paid plan.
{: tip}

The `suggestion` response type is used by the disambiguation feature to suggest possible matches when it isn't clear what the user wants to do. A `suggestion` response includes an array of `suggestions`, each one corresponding to a possible matching dialog node:

```json
{
  "output": {
    "generic": [
      {
        "response_type": "suggestion",
        "title": "Did you mean:",
        "suggestions": [
          {
            "label": "I'd like to order a drink.",
            "value": {
              "intents": [
                {
                  "intent": "order_drink",
                  "confidence": 0.7330395221710206
                }
              ],
              "entities": [],
              "input": {
                "suggestion_id": "576aba3c-85b9-411a-8032-28af2ba95b13",
                "text": "I want to place an order"
              }
            },
            "output": {
              "text": [
                "I'll get you a drink."
              ],
              "generic": [
                {
                  "response_type": "text",
                  "text": "I'll get you a drink."
                }
              ],
              "nodes_visited_details": [
                {
                  "dialog_node": "node_1_1547675028546",
                  "title": "order drink",
                  "user_label": "I'd like to order a drink.",
                  "conditions": "#order_drink"
                }
              ]
            },
            "source_dialog_node": "root"
          },
          {
            "label": "I need a drink refill.",
            "value": {
              "intents": [
                {
                  "intent": "refill_drink",
                  "confidence": 0.2529746770858765
                }
              ],
              "entities": [],
              "input": {
                "suggestion_id": "6583b547-53ff-4e7b-97c6-4d062270abcd",
                "text": "I need a drink refill"
              }
            },
            "output": {
              "text": [
                "I'll get you a refill."
              ],
              "generic": [
                {
                  "response_type": "text",
                  "text": "I'll get you a refill."
                }
              ],
              "nodes_visited_details": [
                {
                  "dialog_node": "node_2_1547675097178",
                  "title": "refill drink",
                  "user_label": "I need a drink refill.",
                  "conditions": "#refill_drink"
                }
              ]
            },
            "source_dialog_node": "root"
          }
        ]
      }
    ],
  },
  "user_id": "faf4a112-f09f-4a95-a0be-43c496e6ac9a"
}
```

Note that the structure of a `suggestion` response is very similar to the structure of an `option` response. As with options, each suggestion includes a `label` that can be displayed to the user and a `value` specifying the input that should be sent back to the assistant if the user chooses the corresponding suggestion. To implement `suggestion` responses in your application, you can use the same approach that you would use for `option` responses.

For more information about the disambiguation feature, see [Disambiguation](/docs/assistant?topic=assistant-dialog-runtime#dialog-runtime-disambiguation).

### Search
{: #api-dialog-responses-search}

This feature is available only to users with a paid plan.
{: tip}

The `search` response type is used by a search skill to return the results from a Watson Discovery search. A `search` response includes an array of `results`, each of which provides information about a match returned from the Discovery search query:

```json
{
  "output": {
    "generic": [
      {
        "response_type": "search",
        "header": "I found the following information that might be helpful.",
        "results": [
          {
            "title": "About",
            "body": "IBM Watson Assistant is a cognitive bot that you can customize for your business needs, and deploy across multiple channels to bring help to your customers where and when they need it.",
            "url": "https://cloud.ibm.com/docs/assistant?topic=assistant-index",
            "id": "6682eca3c5b3778ccb730b799a8063f3",
            "result_metadata": {
              "confidence": 0.08401551980328191,
              "score": 0.73975396
            },
            "highlight": {
              "Shortdesc": [
                "IBM <em>Watson</em> <em>Assistant</em> is a cognitive bot that you can customize for your business needs, and deploy across multiple channels to bring help to your customers where and when they need it."
              ],
              "url": [
                "https://cloud.ibm.com/docs/<em>assistant</em>?topic=<em>assistant</em>-index"
              ],
              "body": [
                "IBM <em>Watson</em> <em>Assistant</em> is a cognitive bot that you can customize for your business needs, and deploy across multiple channels to bring help to your customers where and when they need it."
              ]
            }
          }
        ]
      }
    ]
  },
  "user_id": "58e1b04e-f4bb-469a-9e4c-dffe1d4ebf23"
}
```

For each search result, the `title`, `body`, and `url` properties include content returned from the Discovery query. The search skill configuration determines which fields in the Discovery collection are mapped to these fields in the response. Your application can use these fields to display the results to the user (for example, you might use the `body` text to show an abstract or description of the matching document, and the `url` value to create a link the user can click to open the document).

In addition, the `header` property provides a message to display to the user about the results of the search. In the case of a successful search, `header` provides introductory text to be displayed before the search results (for example, `I found the following information that might be helpful.`). Different message text indicates that the search did not return any results, or that the connection to the Discovery service failed. You can customize these messages in the search skill configuration.

For more information about search skills, see [Creating a search skill](/docs/assistant?topic=assistant-skill-search-add).

### User-defined
{: #api-dialog-responses-user-defined}

A user-defined response type can contain up to 5000 KB of data to support a type of a response you have implemented in your client. For example, you might define a user-defined response type to display a special color-coded card, or to format data in a table or graphic.

The `user_defined` property of the response is an object that can contain any valid JSON data:

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
  },
  "user_id": "faf4a112-f09f-4a95-a0be-43c496e6ac9a"
}
```

Your application can parse and display the data in any way you choose.

## Example: Implementing option responses
{: #api-dialog-option-example}

To show how a client application might handle option responses, which prompt the user to select from a list of choices, we can extend the client example described in [Building a client application](/docs/assistant?topic=assistant-api-client). This is a simplified client app that uses standard input and output to handle three intents (sending a greeting, showing the current time, and exiting from the app):

```
Welcome to the Watson Assistant example!
>> hello
Good day to you.
>> what time is it?
The current time is 12:40:42 PM.
>> goodbye
OK! See you later.
```
{: screen}

If you want to try the example code shown in this topic, you should first set up the required workspace and obtain the API details you will need. For more information, see [Building a client application](/docs/assistant?topic=assistant-api-client).
{: note}

### Receiving an option response

The `option` response can be used when you want to present the user with a finite list of choices, rather than interpreting natural language input. This can be used in any situation where you want to enable the user to quickly select from a set of unambiguous options.

In our simplified client app, we will use this capability to select from a list of the actions the assistant supports (greetings, displaying the time, and exiting). In addition to the three intents previously shown (`#hello`, `#time`, and `#goodbye`), the example workspace supports a fourth intent: `#menu`, which is matched when the use asks to see a list of available actions.

When the workspace recognizes the `#menu` intent, the dialog responds with an `option` response:

```json
{
  "output": {
    "generic": [
      {
        "title": "What do you want to do?",
        "options": [
          {
            "label": "Send greeting",
            "value": {
              "input": {
                "text": "hello"
              }
            }
          },
          {
            "label": "Display the local time",
            "value": {
              "input": {
                "text": "time"
              }
            }
          },
          {
            "label": "Exit",
            "value": {
              "input": {
                "text": "goodbye"
              }
            }
          }
        ],
        "response_type": "option"
      }
    ],
    "intents": [
      {
        "intent": "menu",
        "confidence": 0.6178638458251953
      }
    ],
    "entities": []
  },
  "user_id": "faf4a112-f09f-4a95-a0be-43c496e6ac9a"
}
```

The `option` response contains multiple options to be presented to the user. Each option includes two objects, `label` and `value`. The `label` is a user-facing string identifying the option; the `value` specifies the corresponding message input that should be sent back to the assistant if the user chooses the option.

Our client app will need to use the data in this response to build the output we show to the user, and to send the appropriate message to the assistant.

### Listing available options

The first step in handling an option response is to display the options to the user, using the text specified by the `label` property of each option. You can display the options using any technique that your application supports, typically a drop-down list or a set of clickable buttons. (The optional `preference` property of an option response, if specified, indicates which type of display the application should display if possible.)

Our simplified example uses standard input and output, so we don't have access to a real UI. Instead we will just present the options as a numbered list.

```javascript
// Option example 1: lists options.

const prompt = require('prompt-sync')();
const AssistantV2 = require('ibm-watson/assistant/v2');
const { IamAuthenticator } = require('ibm-watson/auth');

// Set up Assistant service wrapper.
const service = new AssistantV2({
  version: '2019-02-28',
  authenticator: new IamAuthenticator({
    apikey: '{apikey}', // replace with API key
  })
});

const assistantId = '{assistant_id}'; // replace with assistant ID
let sessionId;

// Create session.
service
  .createSession({
    assistantId,
  })
  .then(res => {
    sessionId = res.result.session_id;
    sendMessage({
      messageType: 'text',
      text: '',  // start conversation with empty message
    });
  })
  .catch(err => {
    console.log(err); // something went wrong
  });

// Send message to assistant.
function sendMessage(messageInput) {
  service
    .message({
      assistantId,
      sessionId,
      input: messageInput,
    })
    .then(res => {
      processResponse(res.result);
    })
    .catch(err => {
      console.log(err); // something went wrong
    });
}

// Process the response.
function processResponse(response) {

  let endConversation = false;

  // Check for client actions requested by the assistant.
  if (response.output.actions) {
    if (response.output.actions[0].type === 'client'){
      if (response.output.actions[0].name === 'display_time') {
        // User asked what time it is, so we output the local system time.
        console.log('The current time is ' + new Date().toLocaleTimeString() + '.');
      } else if (response.output.actions[0].name === 'end_conversation') {
        // User said goodbye, so we're done.
        console.log(response.output.generic[0].text);
        endConversation = true;
      }
    }
  } else {
    // Display the output from assistant, if any. Supports only a single
    // response.
    if (response.output.generic) {
      if (response.output.generic.length > 0) {
        switch (response.output.generic[0].response_type) {
          case 'text':
            // It's a text response, so we just display it.
            console.log(response.output.generic[0].text);
            break;
          case 'option':
            // It's an option response, so we'll need to show the user
            // a list of choices.
            console.log(response.output.generic[0].title);
            const options = response.output.generic[0].options;
            // List the options by label.
            for (let i = 0; i < options.length; i++) {
              console.log((i+1).toString() + '. ' + options[i].label);
            }
            break;
        }
      }
    }
  }

  // If we're not done, prompt for the next round of input.
  if (!endConversation) {
    const newMessageFromUser = prompt('>> ');
    newMessageInput = {
      messageType: 'text',
      text: newMessageFromUser,
    }
    sendMessage(newMessageInput);
  } else {
    // We're done, so we delete the session.
    service
      .deleteSession({
        assistantId,
        sessionId,
      })
      .then(res => {
        return;
      })
      .catch(err => {
        console.log(err); // something went wrong
      });
  }
}
```
{: codeblock}

Let's take a closer look at the code that outputs the response from the assistant. Now, instead of assuming a `text` response, the application supports both the `text` and `option` response types:

```javascript
    // Display the output from assistant, if any. Supports only a single
    // response.
    if (response.output.generic) {
      if (response.output.generic.length > 0) {
        switch (response.output.generic[0].response_type) {
          case 'text':
            // It's a text response, so we just display it.
            console.log(response.output.generic[0].text);
            break;
          case 'option':
            // It's an option response, so we'll need to show the user
            // a list of choices.
            console.log(response.output.generic[0].title);
            const options = response.output.generic[0].options;
            // List the options by label.
            for (let i = 0; i < options.length; i++) {
              console.log((i+1).toString() + '. ' + options[i].label);
            }
            break;
        }
      }
    }
```
{: codeblock}

If `response_type`=`text`, we just display the output, as before. But if `response_type`=`option`, we have to do a bit more work. First we display the value of the `title` property, which serves as lead-in text to introduce the list of options; then we list the options, using the value of the `label` property to identify each one. (A real-world application would show these labels in a drop-down list or as the labels on clickable buttons.)

You can see the result by triggering the `#menu` intent:

```
Welcome to the Watson Assistant example!
>> what are the available actions?
What do you want to do?
1. Send greeting
2. Display the local time
3. Exit
>> 2
Sorry, I have no idea what you're talking about.
>>
```
{: screen}

As you can see, the application is now correctly handling the `option` response by listing the available choices. However, we aren't yet translating the user's choice into meaningful input.

### Selecting an option

In addition to the `label`, each option in the response also includes a `value` object, which contains the input data that should be sent back to the assistant if the user chooses the corresponding option. The `value.input` object is equivalent to the `input` property of the `/message` API, which means that we can just send this object back to the assistant as-is.

To do this in our application, we will set a new `promptOption` flag when the client receives an `option` response. When this flag is true, we know that we want to take the next round of input from `value.input` rather than accepting natural language text input from the user. (Again, we don't have a real user interface, so we will just prompt the user to select a valid option from the list by number.)

```javascript
// Option example 2: sends back selected option value.

const prompt = require('prompt-sync')();
const AssistantV2 = require('ibm-watson/assistant/v2');
const { IamAuthenticator } = require('ibm-watson/auth');

// Set up Assistant service wrapper.
const service = new AssistantV2({
  version: '2019-02-28',
  authenticator: new IamAuthenticator({
    apikey: '{apikey}', // replace with API key
  })
});

const assistantId = '{assistant_id}'; // replace with assistant ID
let sessionId;

// Create session.
service
  .createSession({
    assistantId,
  })
  .then(res => {
    sessionId = res.result.session_id;
    sendMessage({
      messageType: 'text',
      text: '',  // start conversation with empty message
    });
  })
  .catch(err => {
    console.log(err); // something went wrong
  });

// Send message to assistant.
function sendMessage(messageInput) {
  service
    .message({
      assistantId,
      sessionId,
      input: messageInput,
    })
    .then(res => {
      processResponse(res.result);
    })
    .catch(err => {
      console.log(err); // something went wrong
    });
}

// Process the response.
function processResponse(response) {

  let endConversation = false;
  let promptOption = false;

  // Check for client actions requested by the assistant.
  if (response.output.actions) {
    if (response.output.actions[0].type === 'client'){
      if (response.output.actions[0].name === 'display_time') {
        // User asked what time it is, so we output the local system time.
        console.log('The current time is ' + new Date().toLocaleTimeString() + '.');
      } else if (response.output.actions[0].name === 'end_conversation') {
        // User said goodbye, so we're done.
        console.log(response.output.generic[0].text);
        endConversation = true;
      }
    }
  } else {
    // Display the output from assistant, if any. Supports only a single
    // response.
    if (response.output.generic) {
      if (response.output.generic.length > 0) {
        switch (response.output.generic[0].response_type) {
          case 'text':
            // It's a text response, so we just display it.
            console.log(response.output.generic[0].text);
            break;
          case 'option':
              // It's an option response, so we'll need to show the user
              // a list of choices.
              console.log(response.output.generic[0].title);
              const options = response.output.generic[0].options;
              // List the options by label.
              for (let i = 0; i < options.length; i++) {
                console.log((i+1).toString() + '. ' + options[i].label);
              }
            promptOption = true;
            break;
        }
      }
    }
  }

  // If we're not done, prompt for the next round of input.
  if (!endConversation) {
    let messageInput;
    if (promptOption == true) {
      // Prompt for a valid selection from the list of options.
      let choice;
      do {
        choice = prompt('? ');
          if (isNaN(choice)) {
            choice = 0;
          }
      } while (choice < 1 || choice > response.output.generic[0].options.length);
      const value = response.output.generic[0].options[choice-1].value;
      // Use message input from the selected option.
      messageInput = value.input;
    } else {
      // We're not showing options, so we just prompt for the next
      // round of input.
      const newText = prompt('>> ');
      messageInput = {
        text: newText,
      }
    }
    sendMessage(messageInput); 
  } else {
    // We're done, so we delete the session.
    service
      .deleteSession({
        assistantId,
        sessionId,
      })
      .then(res => {
        return;
      })
      .catch(err => {
        console.log(err); // something went wrong
      });
  }
}
```
{: codeblock}

All we have to do is use the `value.input` object from the selected response as the next round of message input, rather than building a new `input` object using text input. The assistant then responds exactly as it would if the user had typed the input text directly.

```
Welcome to the Watson Assistant example!
>> hi
Good day to you.
>> what are the choices?
What do you want to do?
1. Send greeting
2. Display the local time
3. Exit
? 2
The current time is 1:29:14 PM.
>> bye
OK! See you later.
```
{: screen}

We can now access all of the functions of the assistant either by making natural-language requests or by selecting from a menu of options.

Note that the same approach is used for `suggestion` responses as well. If your plan supports the disambiguation feature, you can use similar logic to prompt users to select from a list when it isn't clear which of several possible options is correct. For more information about the disambiguation feature, see [Disambiguation](/docs/assistant?topic=assistant-dialog-runtime#dialog-runtime-disambiguation).
