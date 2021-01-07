---

copyright:
  years: 2015, 2021
lastupdated: "2021-01-07"

subcollection: assistant


---

{:curl: .ph data-hd-programlang='curl'}
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:swift: data-hd-programlang='swift'}
{:new_window: target="_blank"}
{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:codeblock: .codeblock}
{:download: .download}
{:deprecated: .deprecated}
{:important: .important}
{:note: .note}
{:tip: .tip}

# Building a custom client
{: #api-client}

If none of the built-in integrations meet your requirements, you can deploy your assistant by developing a custom client application that interacts with your users, communicates with the {{site.data.keyword.conversationfull}} service, and integrates with other services as needed.
{: shortdesc}

## Setting up the {{site.data.keyword.conversationshort}} service
{: #api-client-setup}

The example application we will create in this section implements several simple functions to illustrate how a client application interacts with the service. The application code will connect to an assistant, where the cognitive processing (such as the detection of user intents) takes place.

If you want to try this example yourself, first you need to set up the required assistant:

1.  Download the dialog skill <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/assistant/assistant-simple-example-v2.json" download="assistant-simple-example-v2.json">JSON file</a>.
1.  [Import the skill](/docs/assistant?topic=assistant-skill-dialog-add#skill-dialog-add-task) into an instance of the {{site.data.keyword.conversationshort}} service.
1.  [Create an assistant](/docs/assistant?topic=assistant-assistant-add) and add the skill you imported.

## Getting service information
{: #api-client-get-info}

To access the {{site.data.keyword.conversationshort}} service REST APIs, your application needs to be able to authenticate with {{site.data.keyword.Bluemix}} and connect to the right assistant. You'll need to copy the service credentials and assistant ID and paste them into your application code. You'll also need the URL for the location of your service instance (for example, `https://api.us-south.assistant.watson.cloud.ibm.com`).

To access the service credentials and the assistant ID from the {{site.data.keyword.conversationshort}} tool, go to the **Assistants** tab and click the ![Menu](images/kebab-react.png) menu for the assistant you want to connect to. Select **Settings** and then navigate to the **API details** page to see the details for the assistant, including the URL, assistant ID, and API key.

## Communicating with the {{site.data.keyword.conversationshort}} service
{: #api-client-communicate}

Interacting with the {{site.data.keyword.conversationshort}} service from your client application is simple. We'll start with an example that connects to the service, sends a single message, and prints the output to the console:

```javascript
// Example 1: Creates service object, sends initial message, and
// receives response.

const AssistantV2 = require('ibm-watson/assistant/v2');
const { IamAuthenticator } = require('ibm-watson/auth');

// Create Assistant service object.
const assistant = new AssistantV2({
  version: '2020-09-24',
  authenticator: new IamAuthenticator({
    apikey: '{apikey}', // replace with API key
  }),
  url: '{url}', // replace with URL
});

const assistantId = '{assistant_id}'; // replace with assistant ID

// Start conversation with empty message
messageInput = {
  messageType: 'text',
  text: '',
};
sendMessage(messageInput);

// Send message to assistant.
function sendMessage(messageInput) {
  assistant
    .messageStateless({
      assistantId,
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
  // Display the output from assistant, if any. Supports only a single
  // text response.
  if (response.output.generic) {
    if (response.output.generic.length > 0) {
      if (response.output.generic[0].response_type === 'text') {
        console.log(response.output.generic[0].text);
        }
      }
    }
  }
```
{: codeblock}
{: javascript}

```python
# Example 1: Creates service object, sends initial message, and
# receives response.

from ibm_watson import AssistantV2
from ibm_cloud_sdk_core.authenticators import IAMAuthenticator

# Create Assistant service object.
authenticator = IAMAuthenticator('{apikey}') # replace with API key
assistant = AssistantV2(
    version = '2020-09-24',
    authenticator = authenticator
)
assistant.set_service_url('{url}')
assistant_id = '{assistant_id}' # replace with assistant ID

# Start conversation with empty message.
response = assistant.message_stateless(
    assistant_id,
).get_result()

# Print the output from dialog, if any. Supports only a single
# text response.
if response['output']['generic']:
    if response['output']['generic'][0]['response_type'] == 'text':
        print(response['output']['generic'][0]['text'])
```
{: codeblock}
{: python}

```java
/*
 * Example 1: Creates service object, sends initial message, and
 * receives response.
 */

import com.ibm.cloud.sdk.core.security.Authenticator;
import com.ibm.cloud.sdk.core.security.IamAuthenticator;
import com.ibm.watson.assistant.v2.Assistant;
import com.ibm.watson.assistant.v2.model.MessageResponseStateless;
import com.ibm.watson.assistant.v2.model.MessageStatelessOptions;
import com.ibm.watson.assistant.v2.model.RuntimeResponseGeneric;
import java.util.List;
import java.util.logging.LogManager;

public class AssistantSimpleExample {
  public static void main(String[] args) {

    // Suppress log messages in stdout.
    LogManager.getLogManager().reset();

    // Create Assistant service object.
    Authenticator authenticator = new IamAuthenticator("{apikey}"); // replace with API key
    Assistant assistant = new Assistant("2020-09-24", authenticator);
    assistant.setServiceUrl("{url}");
    String assistantId = "{assistant_id}"; // replace with assistant ID

    // Start conversation with empty message.
    MessageStatelessOptions messageOptions = new MessageStatelessOptions.Builder(assistantId)
      .build();
    MessageResponseStateless response = assistant.messageStateless(messageOptions)
      .execute()
      .getResult();

    // Print the output from dialog, if any. Assumes a single text response.
      List<RuntimeResponseGeneric> responseGeneric = response.getOutput().getGeneric();
      if(responseGeneric.size() > 0) {
        if(responseGeneric.get(0).responseType().equals("text")) {
          System.out.println(responseGeneric.get(0).text());
        }
      }
  }
}
```
{: codeblock}
{: java}

The first step is to create a the service object, a sort of wrapper for the {{site.data.keyword.conversationshort}} service.

You use the service object for sending input to, and receiving output from, the service. When you create the service object, you specify the authentication credentials from the service key, as well as the version of the {{site.data.keyword.conversationshort}} API you are using.

In this Node.js example, the seervice object is an instance of `AssistantV2`, stored in the variable `assistant`. The Watson SDKs for other languages provide equivalent mechanisms for instantiating a service object.
{: javascript}

In this Python example, the service object is an instance of `watson_developer_cloud.AssistantV2`, stored in the variable `assistant`. The Watson SDKs for other languages provide equivalent mechanisms for instantiating a service object.
{: python}

In this Java example, the service object is an instance of `Assistant`, stored in the variable `assistant`. The Watson SDKs for other languages provide equivalent mechanisms for instantiating a service object.
{: java}

After creating the service object, we use it to send a message to the assistant, using the stateless `message` method. In this example, the message is empty; we just want to trigger the `conversation_start` node in the dialog, so we don't need any input text. We then print the response text to the console.

Use the `node <filename.js>` command to run the example application.
{: javascript}

Use the `python3 <filename.py>` command to run the example application.
{: python}

Paste the example code into a file named `AssistantSimpleExample.java`. You can then compile and run it.
{: java}

**Note:** Make sure you have installed the Watson SDK for Node.js using `npm install ibm-watson`.
{: javascript}

**Note:** Make sure you have installed the Watson SDK for Python using `pip install --upgrade ibm-watson` or `easy_install --upgrade ibm-watson`.
{: python}

**Note:** Make sure you have installed the [Watson SDK for Java](https://github.com/watson-developer-cloud/java-sdk/blob/master/README.md){: external}.
{: java}

Assuming everything works as expected, the assistant returns the output from the dialog, which the app then prints to the console:

```
Welcome to the Watson Assistant example!
```
{: screen}

This output tells us that we have successfully communicated with the {{site.data.keyword.conversationshort}} service and received the welcome message specified by the `conversation_start` node in the dialog. Now we can add a user interface, making it possible to process user input.

## Processing user input to detect intents
{: #api-client-process-input}

To be able to process user input, we need to add a user interface to our client application. For this example, we'll keep things simple and use standard input and output.
<span class="ph style-scope doc-content" data-hd-programlang="javascript">We can use the Node.js prompt-sync module to do this. (You can install prompt-sync using `npm install prompt-sync`.)</span>
<span class="ph style-scope doc-content" data-hd-programlang="python">We can use the Python 3 `input` function to do this.</span>
<span class="ph style-scope doc-content" data-hd-programlang="java">We can use the Java `Console.readLine()` function to do this.</span>

```javascript
// Example 2: Adds user input and detects intents.

const prompt = require('prompt-sync')();
const AssistantV2 = require('ibm-watson/assistant/v2');
const { IamAuthenticator } = require('ibm-watson/auth');

// Create Assistant service object.
const assistant = new AssistantV2({
  version: '2020-09-24',
  authenticator: new IamAuthenticator({
    apikey: '{apikey}', // replace with API key
  }),
  url: '{urll}', // replace with URL
});

const assistantId = '{assistant_id}'; // replace with assistant ID

// Start conversation with empty message
messageInput = {
  messageType: 'text',
  text: '',
};
sendMessage(messageInput);

// Send message to assistant.
function sendMessage(messageInput) {
  assistant
    .messageStateless({
      assistantId,
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

  // If an intent was detected, log it out to the console.
  if (response.output.intents.length > 0) {
    console.log('Detected intent: #' + response.output.intents[0].intent);
  }

  // Display the output from assistant, if any. Supports only a single
  // text response.
  if (response.output.generic) {
    if (response.output.generic.length > 0) {
      if (response.output.generic[0].response_type === 'text') {
        console.log(response.output.generic[0].text);
      }
    }
  }

  // If we're not done, prompt for the next round of input.
  const newMessageFromUser = prompt('>> ');
  if (newMessageFromUser !== 'quit') {
    newMessageInput = {
      messageType: 'text',
      text: newMessageFromUser,
    }
    sendMessage(newMessageInput);
  }
}
```
{: codeblock}
{: javascript}

```python
# Example 2: Adds user input and detects intents.

from ibm_watson import AssistantV2
from ibm_cloud_sdk_core.authenticators import IAMAuthenticator

# Create Assistant service object.
authenticator = IAMAuthenticator('{apikey}') # replace with API key
assistant = AssistantV2(
    version = '2020-09-24',
    authenticator = authenticator
)
assistant.set_service_url('{url}')
assistant_id = '{assistant_id}' # replace with assistant ID

# Initialize with empty message to start the conversation.
message_input = {
    'message_type:': 'text',
    'text': ''
    }

# Main input/output loop
while message_input['text'] != 'quit':

    # Send message to assistant.
    response = assistant.message_stateless(
        assistant_id,
        input = message_input
    ).get_result()

    # If an intent was detected, print it to the console.
    if response['output']['intents']:
        print('Detected intent: #' + response['output']['intents'][0]['intent'])

    # Print the output from dialog, if any. Supports only a single
    # text response.
    if response['output']['generic']:
        if response['output']['generic'][0]['response_type'] == 'text':
            print(response['output']['generic'][0]['text'])

    # Prompt for next round of input.
    user_input = input('>> ')
    message_input = {
        'text': user_input
    }
```
{: codeblock }
{: python }

```java
/*
 * Example 2: Adds user input and detects intents.
 */

import com.ibm.cloud.sdk.core.security.Authenticator;
import com.ibm.cloud.sdk.core.security.IamAuthenticator;
import com.ibm.watson.assistant.v2.Assistant;
import com.ibm.watson.assistant.v2.model.MessageInputStateless;
import com.ibm.watson.assistant.v2.model.MessageResponseStateless;
import com.ibm.watson.assistant.v2.model.MessageStatelessOptions;
import com.ibm.watson.assistant.v2.model.RuntimeIntent;
import com.ibm.watson.assistant.v2.model.RuntimeResponseGeneric;
import java.util.List;
import java.util.logging.LogManager;

public class AssistantSimpleExample {
  public static void main(String[] args) {

    // Suppress log messages in stdout.
    LogManager.getLogManager().reset();

    // Create Assistant service object.
    Authenticator authenticator = new IamAuthenticator("{apikey}"); // replace with API key
    Assistant assistant = new Assistant("2020-09-24", authenticator);
    assistant.setServiceUrl("{url}");
    String assistantId = "{assistant_id}"; // replace with assistant ID

    // Initialize with empty message to start the conversation.
    MessageInputStateless input = new MessageInputStateless.Builder()
      .messageType("text")
      .text("")
      .build();

    // Main input/output loop
    do {
      // Send message to assistant.
      MessageStatelessOptions messageOptions = new MessageStatelessOptions.Builder(assistantId)
        .input(input)
        .build();
      MessageResponseStateless response = assistant.messageStateless(messageOptions)
        .execute()
        .getResult();

      // If an intent was detected, print it to the console.
      List<RuntimeIntent> responseIntents = response.getOutput().getIntents();
      if(responseIntents.size() > 0) {
        System.out.println("Detected intent: #" + responseIntents.get(0).intent());
      }

      // Print the output from dialog, if any. Assumes a single text response.
      List<RuntimeResponseGeneric> responseGeneric = response.getOutput().getGeneric();
      if(responseGeneric.size() > 0) {
        if(responseGeneric.get(0).responseType().equals("text")) {
          System.out.println(responseGeneric.get(0).text());
        }
      }

      // Prompt for next round of input.
      System.out.print(">> ");
      String inputText = System.console().readLine();
      input = new MessageInputStateless.Builder()
        .messageType("text")
        .text(inputText)
        .build();
    } while(!input.text().equals("quit"));
  }
}
```
{: codeblock }
{: java }

This version of the application begins the same way as before: sending an empty message to the assistant to start the conversation.

The `processResponse()` function now displays any intent detected by the dialog skill, along with the output text. It then prompts for the next round of user input.
{: javascript }

It then displays any intent detected by the dialog skill, along with the output text. It then prompts for the next round of user input.
{: python }

It then displays any intent detected by the dialog along with the output text, and then it prompts for the next round of user input.
{: java}

We haven't yet implemented a natural-language way to end the conversation, so instead, the client app is just watching for the literal command `quit` to indicate that the program should exit.

But something still isn't right:

```
Welcome to the Watson Assistant example!
>> hello
Detected intent: #hello
Welcome to the Watson Assistant example!
>> goodbye
Detected intent: #goodbye
Welcome to the Watson Assistant example!
>> quit
```
{: screen}

The {{site.data.keyword.conversationshort}} service is detecting the correct `#hello` and `#goodbye` intents, and yet every turn of the conversation returns the welcome message from the conversation_start node (`Welcome to the {{site.data.keyword.conversationshort}} example!`).

This is happening because we are using the stateless `message` method, which means that it is the responsibility of our client application to maintain state information for the conversation. Because we are not yet doing anything to maintain state, the {{site.data.keyword.conversationshort}} service sees every round of user input as the first turn of a new conversation, triggering the conversation_start node.

## Maintaining state

State information for your conversation is maintained using the *context*. The context is an object that can be passed back and forth between your application and the {{site.data.keyword.conversationshort}} service, storing information that can be preserved or updated as the conversation goes on. Because we are using the stateless `message` method, the assistant does not store the context, so it is the responsibility of our client application to maintain it from one turn of the conversation to the next.

The context includes a session ID for each conversation with a user, as well as a counter that is incremented with each turn of the conversation. The assistant updates the context and returns it with each response. But our previous version of the example did not preserve the context, so these updates were lost, and each round of input appeared to be the start of a new conversation. We can easily fix that by saving the context and sending it back to the {{site.data.keyword.conversationshort}} service each time.

In addition to maintaining our place in the conversation, the context can also be used to store any other data you want to pass back and forth between your application and the {{site.data.keyword.conversationshort}} service. This can include persistent data you want to maintain throughout the conversation (such as a customer's name or account number), or any other data you want to track (such as the current status of option settings).

```javascript
// Example 3: Preserves context to maintain state.

const prompt = require('prompt-sync')();
const AssistantV2 = require('ibm-watson/assistant/v2');
const { IamAuthenticator } = require('ibm-watson/auth');

// Create Assistant service object.
const assistant = new AssistantV2({
  version: '2020-09-24',
  authenticator: new IamAuthenticator({
    apikey: '{apikey}', // replace with API key
  }),
  url: '{url}', // replace with URL
});

const assistantId = '{assistant_id}'; // replace with assistant ID

// Start conversation with empty message
messageInput = {
  messageType: 'text',
  text: '',
};
context = {};
sendMessage(messageInput, context);

// Send message to assistant.
function sendMessage(messageInput, context) {
  assistant
    .messageStateless({
      assistantId,
      input: messageInput,
      context: context,
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

  let context = response.context;

  // If an intent was detected, log it out to the console.
  if (response.output.intents.length > 0) {
    console.log('Detected intent: #' + response.output.intents[0].intent);
  }

  // Display the output from assistant, if any. Supports only a single
  // text response.
  if (response.output.generic) {
    if (response.output.generic.length > 0) {
      if (response.output.generic[0].response_type === 'text') {
        console.log(response.output.generic[0].text);
      }
    }
  }

  // If we're not done, prompt for the next round of input.
  const newMessageFromUser = prompt('>> ');
  if (newMessageFromUser !== 'quit') {
    newMessageInput = {
      messageType: 'text',
      text: newMessageFromUser,
    }
    sendMessage(newMessageInput, context);
  }
}
```
{: codeblock}
{: javascript }

```python
# Example 3: Preserves context to maintain state.

from ibm_watson import AssistantV2
from ibm_cloud_sdk_core.authenticators import IAMAuthenticator

# Create Assistant service object.
authenticator = IAMAuthenticator('{apikey}') # replace with API key
assistant = AssistantV2(
    version = '2020-09-24',
    authenticator = authenticator
)
assistant.set_service_url('{url}')
assistant_id = '{assistant_id}' # replace with assistant ID

# Initialize with empty message to start the conversation.
message_input = {
    'message_type:': 'text',
    'text': ''
    }
context = {}

# Main input/output loop
while message_input['text'] != 'quit':

    # Send message to assistant.
    response = assistant.message_stateless(
        assistant_id,
        input = message_input,
        context = context
    ).get_result()

    context = response['context']

    # If an intent was detected, print it to the console.
    if response['output']['intents']:
        print('Detected intent: #' + response['output']['intents'][0]['intent'])

    # Print the output from dialog, if any. Supports only a single
    # text response.
    if response['output']['generic']:
        if response['output']['generic'][0]['response_type'] == 'text':
            print(response['output']['generic'][0]['text'])

    # Prompt for next round of input.
    user_input = input('>> ')
    message_input = {
        'text': user_input
    }
```
{: codeblock }
{: python }

```java
/*
 * Example 3: Preserves context to maintain state.
 */

import com.ibm.cloud.sdk.core.security.Authenticator;
import com.ibm.cloud.sdk.core.security.IamAuthenticator;
import com.ibm.watson.assistant.v2.Assistant;
import com.ibm.watson.assistant.v2.model.MessageContextStateless;
import com.ibm.watson.assistant.v2.model.MessageInputStateless;
import com.ibm.watson.assistant.v2.model.MessageResponseStateless;
import com.ibm.watson.assistant.v2.model.MessageStatelessOptions;
import com.ibm.watson.assistant.v2.model.RuntimeIntent;
import com.ibm.watson.assistant.v2.model.RuntimeResponseGeneric;
import java.util.List;
import java.util.logging.LogManager;

public class AssistantSimpleExample {
  public static void main(String[] args) {

    // Suppress log messages in stdout.
    LogManager.getLogManager().reset();

    // Create Assistant service object.
    Authenticator authenticator = new IamAuthenticator("{apikey}"); // replace with API key
    Assistant assistant = new Assistant("2020-09-24", authenticator);
    assistant.setServiceUrl("{url}");
    String assistantId = "{assistant_id}"; // replace with assistant ID

    // Initialize with empty message to start the conversation.
    MessageInputStateless input = new MessageInputStateless.Builder()
      .messageType("text")
      .text("")
      .build();
    MessageContextStateless context = new MessageContextStateless.Builder()
      .build();

    // Main input/output loop
    do {
      // Send message to assistant.
      MessageStatelessOptions messageOptions = new MessageStatelessOptions.Builder(assistantId)
        .input(input)
        .context(context)
        .build();
      MessageResponseStateless response = assistant.messageStateless(messageOptions)
        .execute()
        .getResult();

      context = response.getContext();

      // If an intent was detected, print it to the console.
      List<RuntimeIntent> responseIntents = response.getOutput().getIntents();
      if(responseIntents.size() > 0) {
        System.out.println("Detected intent: #" + responseIntents.get(0).intent());
      }

      // Print the output from dialog, if any. Assumes a single text response.
      List<RuntimeResponseGeneric> responseGeneric = response.getOutput().getGeneric();
      if(responseGeneric.size() > 0) {
        if(responseGeneric.get(0).responseType().equals("text")) {
          System.out.println(responseGeneric.get(0).text());
        }
      }

      // Prompt for next round of input.
      System.out.print(">> ");
      String inputText = System.console().readLine();
      input = new MessageInputStateless.Builder()
        .messageType("text")
        .text(inputText)
        .build();
    } while(!input.text().equals("quit"));
  }
}
```
{: codeblock }
{: java }

The only change from the previous example is that we are now storing the context received from the dialog in a variable called `context`, and we're sending it back with the next round of user input:
{: javascript }

```javascript
  assistant
    .messageStateless({
      assistantId,
      input: messageInput,
      context: context,
    })
```
{: codeblock}
{: javascript }

The only change from the previous example is that we are now storing the context received from the dialog in a variable called `context`, and we're sending it back with the next round of user input:
{: python }

```python
response = assistant.message_stateless(
    assistant_id,
    input = message_input,
    context = context
).get_result()
```
{: codeblock }
{: python }

The only change from the previous example is that we are now storing the context received from the dialog in a variable called `context`, and we're including it as part of the message options along with the next round of user input:
{: java}

```java
MessageStatelessOptions messageOptions = new MessageStatelessOptions.Builder(assistantId)
  .input(input)
  .context(context)
  .build();
```
{: codeblock }
{: java }

This ensures that the context is maintained from one turn to the next, so the {{site.data.keyword.conversationshort}} service no longer thinks every turn is the first:

```
Welcome to the Watson Assistant example!
>> hello
Detected intent: #hello
Good day to you.
>> goodbye
Detected intent: #goodbye
OK! See you later.
>> quit
```
{: screen}

Now we're making progress! The {{site.data.keyword.conversationshort}} service is correctly recognizing our intents, and the dialog is returning the correct output text (where provided) for each intent.

However, although the service is recognizing the #goodbye intent and even responding appropriately, the conversation doesn't actually end. That's because we have not yet implemented the client actions that need to be carried out when the assistant requests them.

## Implementing client actions
{: #api-client-implement-actions}

In addition to the output text to be displayed to the user, our {{site.data.keyword.conversationshort}} dialog skill uses the `actions` array in the response JSON to signal when the application needs to carry out an action, based on the detected intents. When the dialog determines that the client application needs to do something, it returns an action object with a `type` of `client`. The `name` of the action indicates the specific action the client should carry out, and additional properties of the action can specify parameters, credentials, or other information needed for completing the action.

A client action might be a function the app needs to perform locally (such as accessing a database), or it might be a function the app calls from another service. By using client actions, you can build a client app that not only handles user interaction, but also orchestrates the integration between your assistant and external services.

For more information about how client actions are requested from a dialog node, see [Requesting client actions](/docs/assistant?topic=assistant-dialog-actions-client).
{: note}

The skill we are using for our example recognizes can request two different actions that must be completed by the client app:

- If the user indicates that the conversation is over (the #goodbye intent), the dialog requests a client action called `end_conversation`, which signals that the app should exit.

- If the user asks for a weather forecast (the #weather intent), the dialog requests the `get_weather` action. This signals that the app should request the client retrieve the weather forecast and then send the retrieved information back to the assistant so it can be included in a response to the user.

This version of our example client app adds support for these two actions. Our dialog will never request more than one action at a time, so our client only needs to check for the existence of a single `client` action in the `actions` array. (The `actions` array supports up to 5 actions of all types.) If a `client` action is present, the app carries out the specified action. (This version also removes the display of detected intents, now that we're sure those are being correctly identified.)

To keep things simple, we're simulating the call to an external weather service with a simple
<span class="ph style-scope doc-content" data-hd-programlang="python">`get_weather_forecast`</span>
<span class="ph style-scope doc-content" data-hd-programlang="python">`getWeatherForecast`</span>
stub function. This function only knows how to predict the weather in Boston, and it assumes that it's always sunny.

```javascript
// Example 4: Implements app actions.

const prompt = require('prompt-sync')();
const AssistantV2 = require('ibm-watson/assistant/v2');
const { IamAuthenticator } = require('ibm-watson/auth');

// Create Assistant service object.
const assistant = new AssistantV2({
  version: '2020-09-24',
  authenticator: new IamAuthenticator({
    apikey: '{apikey}', // replace with API key
  }),
  url: '{url}', // replace with URL
});

const assistantId = '{assistant_id}'; // replace with assistant ID

// Start conversation with empty input text
messageInput = {
  messageType: 'text',
  text: '',
};
context = {
  'skills': {
    'main skill': {
      'user_defined': {
        'user_location': 'Boston'
      }
    }
  }
};
sendMessage(messageInput, context);

// Send message to assistant.
function sendMessage(messageInput, context) {
  assistant
    .messageStateless({
      assistantId,
      input: messageInput,
      context: context,
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
  let skipUserInput = false;
  let newMessageFromUser = '';
  let context = response.context;

  // Check for client actions requested by the assistant.
  if (response.output.actions) {
    if (response.output.actions[0].type === 'client'){
      if (response.output.actions[0].name === 'get_weather') {
        // User asked for the weather forecast.
        let forecastLocation = response.output.actions[0].parameters['forecast_location'];
        let resultVar = response.output.actions[0].result_variable;
        let weatherForecast = getWeatherForecast(forecastLocation);
        context['skills']['main skill']['user_defined'][resultVar] = weatherForecast;
        skipUserInput = true;
      } else if (response.output.actions[0].name === 'end_conversation') {
        // User said goodbye, so we're done.
        endConversation = true;
      }
    }
  } 
  //else {
    // Display the output from assistant, if any. Assumes a single text response.
    if (response.output.generic) {
      if (response.output.generic.length > 0) {
        if (response.output.generic[0].response_type === 'text') {
          console.log(response.output.generic[0].text);
        }
      }
    }
  //}

  // If we're not done, send the next round of input.
  if (!endConversation) {
    if (!skipUserInput) {
      newMessageFromUser = prompt('>> ');
    }
    newMessageInput = {
      messageType: 'text',
      text: newMessageFromUser,
    },
    sendMessage(newMessageInput, response.context);
  } else {
    return;
  }
}

// Function to simulate a weather service that only knows about Boston
function getWeatherForecast(forecastLocation) {
  let weatherForecast = '';
  if (forecastLocation === 'Boston') {
    weatherForecast = 'sunny'
  }
  return weatherForecast;
}
```
{: codeblock}
{: javascript}

```python
# Example 4: Implements app actions.

from ibm_watson import AssistantV2
from ibm_cloud_sdk_core.authenticators import IAMAuthenticator
import json

# Create Assistant service object.
authenticator = IAMAuthenticator('{apikey}') # replace with API key
assistant = AssistantV2(
    version = '2020-09-24',
    authenticator = authenticator
)
assistant.set_service_url('{url}')
assistant_id = '{assistant_id}' # replace with assistant ID

# Initialize with empty input text to start the conversation.
message_input = {
                    'message_type': 'text',
                    'text': ''
                }
context = {
    "skills": {
        "main skill": {
            "user_defined": {
                "user_location": "Boston"
            }
        }
    }
}
current_action = ''

# Function to simulate a weather service that only knows about Boston
def get_weather_forecast(forecast_location):
    weather_forecast = ''
    if forecast_location == 'Boston':
        weather_forecast = 'sunny'
    return weather_forecast

# Main input/output loop
while current_action != 'end_conversation':
    # Clear any action flag set by the previous response.
    current_action = ''
    skip_user_input = False

    # Send message to assistant.

    response = assistant.message_stateless(
        assistant_id,
        input = message_input,
        context = context
    ).get_result()

    context = response['context']

    # Print the output from dialog, if any. Supports only a single
    # text response.
    if response['output']['generic']:
        if response['output']['generic'][0]['response_type'] == 'text':
            print(response['output']['generic'][0]['text'])

    # Check for client actions requested by the assistant.
    if 'actions' in response['output']:
        if response['output']['actions'][0]['type'] == 'client':
            current_action = response['output']['actions'][0]['name']

    # User asked for the weather forecast.
    if current_action == 'get_weather':
        forecast_location = response['output']['actions'][0]['parameters']['forecast_location']
        result_var = response['output']['actions'][0]['result_variable']
        weather_forecast = get_weather_forecast(forecast_location)
        context['skills']['main skill']['user_defined'][result_var] = weather_forecast
        skip_user_input = True
    # If we're not done, send the next round of input.
    if current_action != 'end_conversation' and not skip_user_input:
        user_input = input('>> ')
        message_input['text'] = user_input
```
{: codeblock}
{: python}

```java
/*
 * Example 4: implements app actions.
 */

import com.ibm.cloud.sdk.core.security.Authenticator;
import com.ibm.cloud.sdk.core.security.IamAuthenticator;
import com.ibm.watson.assistant.v2.Assistant;
import com.ibm.watson.assistant.v2.model.DialogNodeAction;
import com.ibm.watson.assistant.v2.model.MessageContextSkill;
import com.ibm.watson.assistant.v2.model.MessageContextSkills;
import com.ibm.watson.assistant.v2.model.MessageContextStateless;
import com.ibm.watson.assistant.v2.model.MessageInputStateless;
import com.ibm.watson.assistant.v2.model.MessageResponseStateless;
import com.ibm.watson.assistant.v2.model.MessageStatelessOptions;
import com.ibm.watson.assistant.v2.model.RuntimeResponseGeneric;
import java.util.HashMap;
import java.util.List;
import java.util.logging.LogManager;
import java.util.Map;

public class AssistantSimpleExample {
  public static void main(String[] args) {

    // Suppress log messages in stdout.
    LogManager.getLogManager().reset();

    // Create Assistant service object.
    Authenticator authenticator = new IamAuthenticator("{apikey}"); // replace with API key
    Assistant assistant = new Assistant("2020-09-24", authenticator);
    assistant.setServiceUrl("{url}");
    String assistantId = "{assistant_id}"; // replace with assistant ID

    // Initialize with empty input text to start the conversation.
    MessageInputStateless input = new MessageInputStateless.Builder()
      .messageType("text")
      .text("")
      .build();
    String currentAction;
    Boolean skipUserInput;
    Map<String, Object> userDefinedContext = new HashMap<>();
    userDefinedContext.put("user_location", "Boston");
    Map<String, Object> systemContext = new HashMap<>();
    MessageContextSkill mainSkillContext = new MessageContextSkill.Builder()
      .userDefined(userDefinedContext)
      .system(systemContext)
      .build();
    MessageContextSkills skillsContext = new MessageContextSkills();
    skillsContext.put("main skill", mainSkillContext);
    MessageContextStateless context = new MessageContextStateless.Builder()
      .skills(skillsContext)
      .build();

    // Main input/output loop
    do {
      // Clear any action flag set by the previous response.
      currentAction = "";
      skipUserInput = false;

      // Send message to assistant.
      MessageStatelessOptions messageOptions = new MessageStatelessOptions.Builder(assistantId)
        .input(input)
        .context(context)
        .build();
      MessageResponseStateless response = assistant.messageStateless(messageOptions)
        .execute()
        .getResult();

      context = response.getContext();

      // Print the output from dialog, if any. Assumes a single text response.
      List<RuntimeResponseGeneric> responseGeneric = response.getOutput().getGeneric();
      if(responseGeneric.size() > 0) {
        if(responseGeneric.get(0).responseType().equals("text")) {
          System.out.println(responseGeneric.get(0).text());
        }
      }

      // Check for any actions requested by the assistant.
      List<DialogNodeAction> responseActions = response.getOutput().getActions();
      if(responseActions != null) {
        if(responseActions.get(0).getType().equals("client")) {
          currentAction = responseActions.get(0).getName();
        }
      }

      // User asked for the weather forecast.
      if(currentAction.equals("get_weather")) {
        String forecastLocation = responseActions.get(0).getParameters().get("forecast_location").toString();
        String resultVariable = responseActions.get(0).getResultVariable();
        String weatherForecast = getWeatherForecast(forecastLocation);
        context.skills()
          .get("main skill")
          .userDefined()
          .put(resultVariable, weatherForecast);
        skipUserInput = true;
      }

      // If we're not done, prompt for next round of input.
      if(!currentAction.equals("end_conversation") && !skipUserInput) {
        System.out.print(">> ");
        String inputText = System.console().readLine();
        input = new MessageInputStateless.Builder()
          .messageType("text")
          .text(inputText)
          .build();
      }

    } while(!currentAction.equals("end_conversation"));
  }

  // Function to simulate a weather service that only knows about Boston
  public static String getWeatherForecast(String forecastLocation) {
    String weatherForecast = "";
    if(forecastLocation.equals("Boston")) {
      weatherForecast = "sunny";
    }
    return weatherForecast;
  }
}
```
{: codeblock}
{: java}

When it receives a response, the app now checks the `actions` array to see if an action with `type`=`client` is present. If so, it checks the `name` value of the action and carries out the appropriate action.

If the requested action is `end_conversation`, the app uses this as an indicator that the conversation is over. The main input/output loop can now check for this (instead of the hardcoded `quit` command we used before) and exit, instead of prompting for another round of input.

The `get_weather` action is more interesting. In this case, we need some more information from the client action request:

  - The location parameter to pass to the weather service, which is included in the `parameters` array. (For our example, the dialog is taking the location we specified in the `location` context variable, but of course a real application would use some more intelligent mechanism to determine the location to use.)

  - The name of the context variable where the result should be stored, specified by the `result_variable` property.
  
After we retrieve these values from the client action request, we can call the weather service and store the result in the context variable so it will be sent to the service with the next message.

```javascript
let forecastLocation = response.output.actions[0].parameters['forecast_location'];
let resultVar = response.output.actions[0].result_variable;
let weatherForecast = getWeatherForecast(forecastLocation);
context['skills']['main skill']['user_defined'][resultVar] = weatherForecast;
skipUserInput = true;
```
{: codeblock}
{: javascript}

```python
forecast_location = response['output']['actions'][0]['parameters']['forecast_location']
result_var = response['output']['actions'][0]['result_variable']
weather_forecast = get_weather_forecast(forecast_location)
context['skills']['main skill']['user_defined'][result_var] = weather_forecast
skip_user_input = True
```
{: codeblock}
{: python}

```java
String forecastLocation = responseActions.get(0).getParameters().get("forecast_location").toString();
String resultVariable = responseActions.get(0).getResultVariable();
String weatherForecast = getWeatherForecast(forecastLocation);
context.skills()
  .get("main skill")
  .userDefined()
  .put(resultVariable, weatherForecast);
skipUserInput = true;
```
{: codeblock}
{: java}

We also set a
<span class="ph style-scope doc-content" data-hd-programlang="python">`skip_user_input`</span>
<span class="ph style-scope doc-content" data-hd-programlang="java">`skipUserInput`</span>
<span class="ph style-scope doc-content" data-hd-programlang="javascript">`skipUserInput`</span>
flag, which tells the app not to prompt the user for input before sending the next message. We do this because, from the dialog's point of view, the result of the client action effectively _is_ the "user input" for this turn of the conversation; the child node that receives the next message only needs the data we stored in the context.

```
Welcome to the Watson Assistant example!
>> hello
Good day to you.
>> what's the weather today?
Checking the weather...
It will be sunny in Boston today.
>> goodbye
OK! See you later.
```
{: screen}

Success! The application now uses the {{site.data.keyword.conversationshort}} service to identify the intents in natural-language input, displays the appropriate responses, and implements the requested client actions.

The dialog in our assistant also includes another child node that handles the case where the weather service fails, and no forecast information is stored in the context. Since our simulated weather service only knows about Boston, we can easily test this case by changing our hardcoded `user_location` context variable to a different city:

```javascript
context = {
  'skills': {
    'main skill': {
      'user_defined': {
        'user_location': 'Tokyo'
      }
    }
  }
};
```
{: codeblock}
{: javascript}

```python
context = {
    "skills": {
        "main skill": {
            "user_defined": {
                "user_location": "Tokyo"
            }
        }
    }
}
```
{: codeblock}
{: python}

```java
userDefinedContext.put("user_location", "Tokyo");
```
{: codeblock}
{: java}

Now the weather service fails to return any forecast, so a different dialog node handles the response:

```
Welcome to the Watson Assistant example!
>> what's the weather today?
Checking the weather...
Oops! I couldn't get the weather forecast. Maybe try again later?
>> goodbye
OK! See you later.
```
{: screen}

This simple example illustrates how a client app serves both as the channel for users communicating with the assistant, as well as a point of integration with other services. Of course, a real-world application would use a more sophisticated user interface, such as a web interface; and it would implement more complex actions, possibly integrating with a customer database or other business systems. It would also need to send additional data to the assistant, such as a user ID to identify each unique user. But the basic principles of how the application interacts with the {{site.data.keyword.conversationshort}} service would remain the same.

For some more complex examples, see [Sample apps](/docs/assistant?topic=assistant-sample-apps).

## Using the v1 runtime API
{: #api-client-v1-api}

Using the v2 API is the recommended way to build a runtime client application that communicates with the {{site.data.keyword.conversationshort}} service. However, some older applications might still be using the v1 runtime API, which includes a similar method for sending messages to the workspace within a dialog skill. Note that if your app uses the v1 runtime API, it communicates directly with the workspace, bypassing the skill orchestration and state-management capabilities of the assistant.

For more information about the v1 `/message` method and context, see the [v1 API Reference](https://{DomainName}/apidocs/assistant/assistant-v1#message){: external}.
