---

copyright:
  years: 2015, 2019
lastupdated: "2019-10-11"

subcollection: assistant


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
{: #api-dialog-responses}

A dialog node can respond to users with a response that includes text, images, or interactive elements such as clickable options. If you are building your own client application, you must implement the correct display of all response types returned by your dialog. (For more information about dialog responses, see [Responses](/docs/services/assistant?topic=assistant-dialog-overview#dialog-overview-responses)).

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
  }
}
```

As this example shows, the text response (`OK, here's a picture of a dog.`) is also returned in the `output.text` array. This is included for backward compatibility for older applications that do not support the `output.generic` format.
{: note}

It is the reponsibility of your client application to handle all response types appropriately. In this case, your application would need to display the specified text and image to the user.

## Response types
{: #api-dialog-responses-types}

Each element of a response is of one of the supported response types (currently `image`, `option`, `pause`, `text`, and `suggestion`). Each response type is specified using a different set of JSON properties, so the properties included for each response will vary depending upon response type. For complete information about the response model of the `/message` API, see the [API Reference ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://{DomainName}/apidocs/assistant-v2#send-user-input-to-assistant){: new_window}.)

This section describes the available response types and how they are represented in the `/message` API response JSON. (If you are using the Watson SDK, you can use the interfaces provided for your language to access the same objects.)

The examples in this section show the format of the JSON data returned from the `/message API` at run time. Keep in mind that this is different from the JSON format used to define responses within a dialog node. For more information, see [Defining responses using the JSON editor](/docs/services/assistant?topic=assistant-dialog-responses-json)).
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
  }
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
  }
}
```

Your application is responsible for retrieving the image specified by the `source` property and displaying it to the user. If the optional `title` and `description` are provided, your application can display them in whatever way is appropriate (for example, rendering the title below the image and the description as hover text).

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
  }
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
  }
}
```

Your app can display the specified options using any suitable user-interface control (for example, a set of buttons or a drop-down list). The optional `preference` property indicates the preferred type of control your app should use (`button` or `dropdown`), if supported. For the best user experience, a good practice is to present three or fewer options as buttons, and more than three options as a drop-down list.

For each option, the `label` property specifies the label text that should appear for the option in the UI control. The `value` property specifies the input that should be sent back to the assistant (using the `/message` API) when the user selects the corresponding option.

For an example of implementing `option` responses in a simple client application, see [Example: Implementing option responses](#api-dialog-option-example).

### Suggestion
{: #api-dialog-responses-suggestion}

This feature is available only to Plus or Premium plan users.
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
  }
}
```

Note that the structure of a `suggestion` response is very similar to the structure of an `option` response. As with options, each suggestion includes a `label` that can be displayed to the user and a `value` specifying the input that should be sent back to the assistant if the user chooses the corresponding suggestion. To implement `suggestion` responses in your application, you can use the same approach that you would use for `option` responses.

For more information about the disambiguation feature, see [Disambiguation](/docs/services/assistant?topic=assistant-dialog-runtime#dialog-runtime-disambiguation).

## Example: Implementing option responses
{: #api-dialog-option-example}

To show how a client application might handle option responses, which prompt the user to select from a list of choices, we can extend the client example described in [Building a client application](/docs/services/assistant?topic=assistant-api-client). This is a simplified client app that uses standard input and output to handle three intents (sending a greeting, showing the current time, and exiting from the app):

```
Welcome to the {{site.data.keyword.conversationshort}} example!
>> hello
Good day to you.
>> what time is it?
The current time is 12:40:42 PM.
>> goodbye
OK! See you later.
```
{: screen}

If you want to try the example code shown in this topic, you should first set up the required workspace and obtain the API details you will need. For more information, see [Building a client application](/docs/services/assistant?topic=assistant-api-client).
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
  }
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
{: javascript}

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
{: javascript}

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
{: javascript}

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

Note that the same approach is used for `suggestion` responses as well. If your plan supports the disambiguation feature, you can use similar logic to prompt users to select from a list when it isn't clear which of several possible options is correct. For more information about the disambiguation feature, see [Disambiguation](/docs/services/assistant?topic=assistant-dialog-runtime#dialog-runtime-disambiguation).
