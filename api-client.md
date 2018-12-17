---

copyright:
  years: 2015, 2018
lastupdated: "2018-12-17"

---

{:curl: #curl .ph data-hd-programlang='curl'}
{:javascript: #javascript .ph data-hd-programlang='javascript'}
{:java: #java .ph data-hd-programlang='java'}
{:python: #python .ph data-hd-programlang='python'}
{:swift: data-hd-programlang='swift'}
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:codeblock: .codeblock}
{:download: .download}
{:deprecated: .deprecated}
{:important: .important}
{:note: .note}
{:tip: .tip}

# Building a client application

So you have a working dialog skill and an assistant that uses it. Now you want to develop a custom client application that will interact with your users and communicate with the {{site.data.keyword.conversationfull}} service.
{: shortdesc}

You can view this tutorial for Node.js (Javascript), Python 3, or Java by clicking the language selector in the upper right. For details about all supported languages, refer to the {{site.data.keyword.watson}} [SDKs ![External link icon](../../icons/launch-glyph.svg "External link icon")](/docs/services/watson/getting-started-sdks.html#sdks){: new_window}.
{: tip }

## Setting up the {{site.data.keyword.conversationshort}} service

The example application we will create in this section implements several functions of a cognitive personal assistant. The application code will connect to a {{site.data.keyword.conversationshort}} assistant, where the cognitive processing (such as the detection of user intents) takes place.

Before continuing with this example, you need to set up the required assistant:

1.  Download the dialog skill <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/assistant/assistant-simple-example.json" download="assistant-simple-example.json">JSON file</a>.
1.  [Import the skill](create-skill.html#creating-skills) into an instance of the {{site.data.keyword.conversationshort}} service.
1.  [Create an assistant](create-assistant.html#creating-assistants) and connect the skill you imported.

## Getting service information

To access the {{site.data.keyword.conversationshort}} service REST APIs, your application needs to be able to authenticate with {{site.data.keyword.Bluemix}} and connect to the right assistant. You'll need to copy the service credentials and assistant ID and paste them into your application code.

To access the service credentials and the assistant ID from the {{site.data.keyword.conversationshort}} tool, go to the **Assistants** tab and click the ![Menu](images/kabob-grey.png) menu for the assistant you want to connect to. Select **View API Details** to see the details for the assistant, including the assistant ID and API key.

You can also access the service credentials from your {{site.data.keyword.Bluemix_short}} dashboard.

## Communicating with the {{site.data.keyword.conversationshort}} service

Interacting with the {{site.data.keyword.conversationshort}} service is simple. Let's take a look at an example that connects to the service, sends a single message, and prints the output to the console:

```javascript
// Example 1: sets up service wrapper, sends initial message, and 
// receives response.

var AssistantV2 = require('watson-developer-cloud/assistant/v2');

// Set up Assistant service wrapper.
var service = new AssistantV2({
  iam_apikey: '{apikey}', // replace with API key
  version: '2018-09-20'
});

var assistantId = '{assistant_id}'; // replace with assistant ID
var sessionId;

// Create session.
service.createSession({
  assistant_id: assistantId
}, function(err, result) {
  if (err) {
    console.error(err); // something went wrong
    return;
  }
  sessionId = result.session_id;
  sendMessage(); // start conversation with empty message
});

// Send message to assistant.
function sendMessage() {
  service.message({
    assistant_id: assistantId,
    session_id: sessionId
  }, processResponse);
}

// Process the response.
function processResponse(err, response) {
  if (err) {
    console.error(err); // something went wrong
    return;
  }

  // Display the output from assistant, if any. Assumes a single text response.
  if (response.output.generic.length != 0) {
      console.log(response.output.generic[0].text);
  }

  // We're done, so we close the session
  service.deleteSession({
    assistant_id: assistantId,
    session_id: sessionId
  }, function(err, result) {
    if (err) {
      console.error(err); // something went wrong
    }
  });
}
```
{: codeblock}
{: javascript}

```python
# Example 1: sets up service wrapper, sends initial message, and
# receives response.

import watson_developer_cloud

# Set up Assistant service.
service = watson_developer_cloud.AssistantV2(
    iam_apikey = '{apikey}', # replace with API key
    version = '2018-09-20'
)

assistant_id = '{assistant_id}' # replace with assistant ID

# Create session.
session_id = service.create_session(
    assistant_id = assistant_id
).get_result()['session_id']

# Start conversation with empty message.
response = service.message(
    assistant_id,
    session_id
).get_result()

# Print the output from dialog, if any. Assumes a single text response.
if response['output']['generic']:
    print(response['output']['generic'][0]['text'])

# We're done, so we delete the session.
service.delete_session(
    assistant_id = assistant_id,
    session_id = session_id
)
```
{: codeblock}
{: python}

```java
/*
 * Example 1: sets up service wrapper, sends initial message, and
 * receives response.
 */

import com.ibm.watson.developer_cloud.assistant.v2.Assistant;
import com.ibm.watson.developer_cloud.assistant.v2.model.CreateSessionOptions;
import com.ibm.watson.developer_cloud.assistant.v2.model.DeleteSessionOptions;
import com.ibm.watson.developer_cloud.assistant.v2.model.MessageOptions;
import com.ibm.watson.developer_cloud.assistant.v2.model.MessageResponse;
import com.ibm.watson.developer_cloud.assistant.v2.model.SessionResponse;
import com.ibm.watson.developer_cloud.service.security.IamOptions;
import java.util.logging.LogManager;

public class AssistantSimpleExample {
  public static void main(String[] args) {

    // Suppress log messages in stdout.
    LogManager.getLogManager().reset();

    // Set up Assistant service.
    IamOptions iamOptions = new IamOptions.Builder().apiKey("{apikey}").build();
    Assistant service = new Assistant("2018-09-20", iamOptions);
    String assistantId = "{assistant_id}"; // replace with assistant ID

    // Create session.
    CreateSessionOptions createSessionOptions = new CreateSessionOptions.Builder(assistantId).build();
    SessionResponse session = service.createSession(createSessionOptions).execute();
    String sessionId = session.getSessionId();

    // Start conversation with empty message.
    MessageOptions messageOptions = new MessageOptions.Builder(assistantId, sessionId).build();
    MessageResponse response = service.message(messageOptions).execute();

    // Print the output from dialog, if any. Assumes a single text response.
    System.out.println(response.getOutput().getGeneric().get(0).getText());

    // We're done, so we delete the session.
    DeleteSessionOptions deleteSessionOptions = new DeleteSessionOptions.Builder(assistantId, sessionId).build();
    service.deleteSession(deleteSessionOptions).execute();
  }
}
```
{: codeblock}
{: java}

The first step is to create a wrapper for the {{site.data.keyword.conversationshort}} service.

The wrapper is an object you will use to send input to, and receive output from, the service. When you create the service wrapper, specify the authentication credentials from the service key, as well as the version of the {{site.data.keyword.conversationshort}} API you are using.

In this Node.js example, the wrapper is an instance of `AssistantV2`, stored in the variable `service`. The Watson SDKs for other languages provide equivalent mechanisms for instantiating a service wrapper.
{: javascript}

In this Python example, the wrapper is an instance of `watson_developer_cloud.AssistantV2`, stored in the variable `service`. The Watson SDKs for other languages provide equivalent mechanisms for instantiating a service wrapper.
{: python}

In this Java example, the wrapper is an instance of `Assistant`, stored in the variable `service`. The Watson SDKs for other languages provide equivalent mechanisms for instantiating a service wrapper.
{: java}

After creating the service wrapper, we use it to create a session and send a message to the assistant. In this example, the message is empty; we just want to trigger the conversation_start node in the dialog, so we don't need any input text. We then print the response text to the console, and finally we delete the session.

Use the `node <filename.js>` command to run the example application.
{: javascript}

Use the `python3 <filename.py>` command to run the example application.
{: python}

Paste the example code into a file named `AssistantSimpleExample.java`. You can then compile and run it.
{: java}

**Note:** Make sure you have installed the Watson SDK for Node.js using `npm install watson-developer-cloud`.
{: javascript}

**Note:** Make sure you have installed the Watson SDK for Python using `pip install --upgrade watson-developer-cloud` or `easy_install --upgrade watson-developer-cloud`.
{: python}

**Note:** Make sure you have installed the [Watson SDK for Java ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://github.com/watson-developer-cloud/java-sdk/blob/develop/README.md){: new_window}.
{: java}

Assuming everything works as expected, the assistant returns the output from the dialog, which is then printed to the console:

```
Welcome to the Watson Assistant example!
```
{: screen}

This output tells us that we have successfully communicated with the {{site.data.keyword.conversationshort}} service and received the welcome message specified by the conversation_start node in the dialog. Now we can add a user interface, making it possible to process user input.

## Processing user input to detect intents

To be able to process user input, we need to add a user interface to our client application. For this example, we'll keep things simple and use standard input and output.
<span class="ph style-scope doc-content" data-hd-programlang="javascript">We can use the Node.js prompt-sync module to do this. (You can install prompt-sync using `npm install prompt-sync`.)</span>
<span class="ph style-scope doc-content" data-hd-programlang="python">We can use the Python 3 `input` function to do this.</span>
<span class="ph style-scope doc-content" data-hd-programlang="java">We can use the Java `Console.readLine()` function to do this.</span>

```javascript
// Example 2: adds user input and detects intents.

var prompt = require('prompt-sync')();
var AssistantV2 = require('watson-developer-cloud/assistant/v2');

// Set up Assistant service wrapper.
var service = new AssistantV2({
  iam_apikey: '{apikey}', // replace with API key
  version: '2018-09-20'
});

var assistantId = '{assistant_id}'; // replace with assistant ID
var sessionId;

// Create session.
service.createSession({
  assistant_id: assistantId
}, function(err, result) {
  if (err) {
    console.error(err); // something went wrong
    return;
  }
  sessionId = result.session_id;
  sendMessage(); // start conversation with empty message
});

// Send message to assistant.
function sendMessage(messageText) {
  service.message({
    assistant_id: assistantId,
    session_id: sessionId,
    input: {
      message_type: 'text',
      text: messageText
    }
  }, processResponse);
}

// Process the response.
function processResponse(err, response) {
  if (err) {
    console.error(err); // something went wrong
    return;
  }

  // If an intent was detected, log it out to the console.
  if (response.output.intents.length > 0) {
    console.log('Detected intent: #' + response.output.intents[0].intent);
  }

  // Display the output from assistant, if any. Assumes a single text response.
  if (response.output.generic.length != 0) {
    console.log(response.output.generic[0].text);
  }

  // Prompt for the next round of input.
  var newMessageFromUser = prompt('>> ');
    if (newMessageFromUser === 'quit') {
      service.deleteSession({
        assistant_id: assistantId,
        session_id: sessionId
      }, function(err, result) {
        if (err) {
          console.error(err); // something went wrong
        }
      });
      return;
    }
  sendMessage(newMessageFromUser);
}
```
{: codeblock}
{: javascript}

```python
# Example 2: adds user input and detects intents.

import watson_developer_cloud

# Set up Assistant service.
service = watson_developer_cloud.AssistantV2(
    iam_apikey = '{apikey}', # replace with API key
    version = '2018-09-20'
)

assistant_id = '{assistant_id}' # replace with assistant ID

# Create session.
session_id = service.create_session(
    assistant_id = assistant_id
).get_result()['session_id']

# Initialize with empty value to start the conversation.
user_input = ''

# Main input/output loop
while user_input != 'quit':

    # Send message to assistant.
    response = service.message(
        assistant_id,
        session_id,
        input = {
            'text': user_input
        }
    ).get_result()

    # If an intent was detected, print it to the console.
    if response['output']['intents']:
        print('Detected intent: #' + response['output']['intents'][0]['intent'])

    # Print the output from dialog, if any. Assumes a single text response.
    if response['output']['generic']:
        print(response['output']['generic'][0]['text'])

    # Prompt for next round of input.
    user_input = input('>> ')

# We're done, so we delete the session.
service.delete_session(
    assistant_id = assistant_id,
    session_id = session_id
)
```
{: codeblock }
{: python }

```java
/*
 * Example 2: adds user input and detects intents.
 */

import com.ibm.watson.developer_cloud.assistant.v2.Assistant;
import com.ibm.watson.developer_cloud.assistant.v2.model.CreateSessionOptions;
import com.ibm.watson.developer_cloud.assistant.v2.model.DeleteSessionOptions;
import com.ibm.watson.developer_cloud.assistant.v2.model.DialogRuntimeResponseGeneric;
import com.ibm.watson.developer_cloud.assistant.v2.model.MessageInput;
import com.ibm.watson.developer_cloud.assistant.v2.model.MessageOptions;
import com.ibm.watson.developer_cloud.assistant.v2.model.MessageResponse;
import com.ibm.watson.developer_cloud.assistant.v2.model.RuntimeIntent;
import com.ibm.watson.developer_cloud.assistant.v2.model.SessionResponse;
import com.ibm.watson.developer_cloud.service.security.IamOptions;
import java.util.List;
import java.util.logging.LogManager;

public class AssistantSimpleExample {
  public static void main(String[] args) {

    // Suppress log messages in stdout.
    LogManager.getLogManager().reset();

    // Set up Assistant service.
    IamOptions iamOptions = new IamOptions.Builder().apiKey("{apikey}").build();
    Assistant service = new Assistant("2018-09-20", iamOptions);
    String assistantId = "{assistant_id}"; // replace with assistant ID

    // Create session.
    CreateSessionOptions createSessionOptions = new CreateSessionOptions.Builder(assistantId).build();
    SessionResponse session = service.createSession(createSessionOptions).execute();
    String sessionId = session.getSessionId();

    // Initialize with empty value to start the conversation.
    String inputText = "";

    // Main input/output loop
    do {
      // Send message to assistant.
      MessageInput input = new MessageInput.Builder().text(inputText).build();
      MessageOptions messageOptions = new MessageOptions.Builder(assistantId, sessionId)
                                                  .input(input)
                                                  .build();
      MessageResponse response = service.message(messageOptions).execute();

      // If an intent was detected, print it to the console.
      List<RuntimeIntent> responseIntents = response.getOutput().getIntents();
      if(responseIntents.size() > 0) {
        System.out.println("Detected intent: #" + responseIntents.get(0).getIntent());
      }

      // Print the output from dialog, if any. Assumes a single text response.
      List<DialogRuntimeResponseGeneric> responseGeneric = response.getOutput().getGeneric();
      if(responseGeneric.size() > 0) {
        System.out.println(response.getOutput().getGeneric().get(0).getText());
      }

      // Prompt for next round of input.
      System.out.print(">> ");
      inputText = System.console().readLine();
    } while(!inputText.equals("quit"));

    // We're done, so we delete the session.
    DeleteSessionOptions deleteSessionOptions = new DeleteSessionOptions.Builder(assistantId, sessionId).build();
    service.deleteSession(deleteSessionOptions).execute();
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

We haven't yet implemented a natural-language way to end the conversation, so instead we're using the literal command `quit` to indicate that the program should delete the session and exit.

```
Welcome to the Watson Assistant example!
>> hello
Detected intent: #hello
Good day to you.
>> what time is it?
Detected intent: #time
>> goodbye
Detected intent: #goodbye
OK! See you later.
>> quit
```
{: screen}

Now we're making progress! The {{site.data.keyword.conversationshort}} service is correctly recognizing our intents, and the dialog is returning the correct output text (where provided) for each intent.

However, nothing else is happening. When we ask for the time, we get no answer; and when we say goodbye, the conversation does not end. That's because those intents require additional actions to be taken by the app.

## Implementing app actions

In addition to the output text to be displayed to the user, our {{site.data.keyword.conversationshort}} dialog uses the `actions` array in the response JSON to signal when the application needs to carry out an action, based on the detected intents. When the dialog determines that the client application needs to do something, it returns an action object with a `type` of `client`. The `name` of the action indicates the specific action, either `display_time` or `end_conversation`. (Additional properties of the action can specify parameters, credentials, and other information related to the action, but for our example we don't need anything but the action name.)

We know that our dialog will never request more than one action at a time, so our client only needs to check for the existence of a single `client` action in the `actions` array. If it finds one, it can then carry out the specified action. (This version also removes the display of detected intents, now that we're sure those are being correctly identified.)

```javascript
// Example 3: implements app actions.

var prompt = require('prompt-sync')();
var AssistantV2 = require('watson-developer-cloud/assistant/v2');

// Set up Assistant service.
var service = new AssistantV2({
  iam_apikey: '{apikey}', // replace with API key
  version: '2018-09-20'
});

var assistantId = '{assistant_id}'; // replace with assistant ID
var sessionId;

// Create session.
service.createSession({
  assistant_id: assistantId
}, function(err, result) {
  if (err) {
    console.error(err); // something went wrong
    return;
  }
  sessionId = result.session_id;
  sendMessage(''); // start conversation with empty message
});

// Send message to assistant.
function sendMessage(messageText) {
  service.message({
    assistant_id: assistantId,
    session_id: sessionId,
    input: {
      message_type: 'text',
      text: messageText
    }
  }, processResponse);
}

// Process the response.
function processResponse(err, response) {
  if (err) {
    console.error(err); // something went wrong
    return;
  }

  var endConversation = false;

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
    // Display the output from assistant, if any. Assumes a single text response.
    if (response.output.generic.length != 0) {
      console.log(response.output.generic[0].text);
    }
  }

  // If we're not done, prompt for the next round of input.
  if (!endConversation) {
    var newMessageFromUser = prompt('>> ');
    sendMessage(newMessageFromUser);
  } else {
    service.deleteSession({
      assistant_id: assistantId,
      session_id: sessionId
    }, function(err, result) {
      if (err) {
        console.error(err); // something went wrong
      }
    });
    return;
  }
}
```
{: codeblock}
{: javascript}

```python
# Example 3: Implements app actions.

import watson_developer_cloud
import time

# Set up Assistant service.
service = watson_developer_cloud.AssistantV2(
    iam_apikey = '{apikey}', # replace with API key
    version = '2018-09-20'
)

assistant_id = '{assistant_id}' # replace with assistant ID

# Create session.
session_id = service.create_session(
    assistant_id = assistant_id
).get_result()['session_id']

# Initialize with empty values to start the conversation.
user_input = ''
current_action = ''

# Main input/output loop
while current_action != 'end_conversation':
    # Clear any action flag set by the previous response.
    current_action = ''

    # Send message to assistant.
    response = service.message(
        assistant_id,
        session_id,
        input = {
            'text': user_input
        }
    ).get_result()

    # Print the output from dialog, if any. Assumes a single text response.
    if response['output']['generic']:
        print(response['output']['generic'][0]['text'])

    # Check for client actions requested by the assistant.
    if 'actions' in response['output']:
        if response['output']['actions'][0]['type'] == 'client':
            current_action = response['output']['actions'][0]['name']

    # User asked what time it is, so we output the local system time.
    if current_action == 'display_time':
        print('The current time is ' + time.strftime('%I:%M:%S %p') + '.')
    # If we're not done, prompt for next round of input.
    if current_action != 'end_conversation':
        user_input = input('>> ')

# We're done, so we delete the session.
service.delete_session(
    assistant_id = assistant_id,
    session_id = session_id
)
```
{: codeblock}
{: python}

```java
/*
 * Example 3: implements app actions.
 */

import com.ibm.watson.developer_cloud.assistant.v2.Assistant;
import com.ibm.watson.developer_cloud.assistant.v2.model.CreateSessionOptions;
import com.ibm.watson.developer_cloud.assistant.v2.model.DeleteSessionOptions;
import com.ibm.watson.developer_cloud.assistant.v2.model.DialogNodeAction;
import com.ibm.watson.developer_cloud.assistant.v2.model.DialogRuntimeResponseGeneric;
import com.ibm.watson.developer_cloud.assistant.v2.model.MessageInput;
import com.ibm.watson.developer_cloud.assistant.v2.model.MessageOptions;
import com.ibm.watson.developer_cloud.assistant.v2.model.MessageResponse;
import com.ibm.watson.developer_cloud.assistant.v2.model.RuntimeIntent;
import com.ibm.watson.developer_cloud.assistant.v2.model.SessionResponse;
import com.ibm.watson.developer_cloud.service.security.IamOptions;
import java.time.LocalTime;
import java.time.format.DateTimeFormatter;
import java.util.List;
import java.util.logging.LogManager;

public class AssistantSimpleExample {
  public static void main(String[] args) {

    // Suppress log messages in stdout.
    LogManager.getLogManager().reset();

    // Set up Assistant service.
    IamOptions iamOptions = new IamOptions.Builder().apiKey("{apikey}").build();
    Assistant service = new Assistant("2018-09-20", iamOptions);
    String assistantId = "{assistant_id}"; // replace with assistant ID

    // Create session.
    CreateSessionOptions createSessionOptions = new CreateSessionOptions.Builder(assistantId).build();
    SessionResponse session = service.createSession(createSessionOptions).execute();
    String sessionId = session.getSessionId();

    // Initialize with empty values to start the conversation.
    String inputText = "";
    String currentAction;

    // Main input/output loop
    do {
      // Clear any action flag set by the previous response.
      currentAction = "";

      // Send message to assistant.
      MessageInput input = new MessageInput.Builder().text(inputText).build();
      MessageOptions messageOptions = new MessageOptions.Builder(assistantId, sessionId)
                                                  .input(input)
                                                  .build();
      MessageResponse response = service.message(messageOptions).execute();

      // Print the output from dialog, if any. Assumes a single text response.
      List<DialogRuntimeResponseGeneric> responseGeneric = response.getOutput().getGeneric();
      if(responseGeneric.size() > 0) {
        System.out.println(response.getOutput().getGeneric().get(0).getText());
      }

      // Check for any actions requested by the assistant.
      List<DialogNodeAction> responseActions = response.getOutput().getActions();
      if(responseActions != null) {
        if(responseActions.get(0).getActionType().equals("client")) {
          currentAction = responseActions.get(0).getName();
        }
      }

      // User asked what time it is, so we output the local system time.
      if(currentAction.equals("display_time")) {
        DateTimeFormatter fmt = DateTimeFormatter.ofPattern("h:mm:ss a");
        LocalTime time = LocalTime.now();
        System.out.println("The current time is " + time.format(fmt) + ".");
      }

      // If we're not done, prompt for next round of input.
      if(!currentAction.equals("end_conversation")) {
        System.out.print(">> ");
        inputText = System.console().readLine();
      }

    } while(!currentAction.equals("end_conversation"));

    // We're done, so we delete the session.
    DeleteSessionOptions deleteSessionOptions = new DeleteSessionOptions.Builder(assistantId, sessionId).build();
    service.deleteSession(deleteSessionOptions).execute();
  }
}
```
{: codeblock}
{: java}

The app now checks the `actions` array in the response to see if an action with `type`=`client` is present. If so, it checks the `name` value of the action and carries out the appropriate action (either displaying the local system time, or setting an internal flag that indicates that the conversation is over).

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

Success! The application now uses the {{site.data.keyword.conversationshort}} service to identify the intents in natural-language input, displays the appropriate responses, and implements the requested client actions.

Of course, a real-world application would use a more sophisticated user interface, such as a web chat window. And it would implement more complex actions, possibly integrating with a customer database or other business systems. It would also need to send additional data to the assistant, such as a user ID to identify each unique user. But the basic principles of how the application interacts with the {{site.data.keyword.conversationshort}} service would remain the same.

For some more complex examples, see [Sample apps](/docs/services/assistant/sample-applications.html).

## Accessing context

The *context* is an object containing variables that persist throughout a conversation and can be shared by the dialog and the client application. If your application is using the v2 API, the context is automatically maintained by the assistant on a per-session basis. Both the dialog and the client application can read and write context variables. By default, the context is not returned to a client application, but you can optionally request that it be included in the response to each `/message` request.

**Important:** One use of the context is to specify a unique user ID for each end user who interacts with the assistant. For user-based plans, this ID is used for billing purposes. (For more information, see [User-based plans](/docs/services/assistant/services-information.html#user-based-plans).)

There are two types of context:

- **Global context**: context variables that are shared by all skills used by an assistant, including internal system variables used to manage conversation flow.

- **Skill-specific context**: context variables specific to a particular skill, including any user-defined variables needed by your application. Currently, only one skill (named `main skill`) is supported.

The following example shows a `/message` request that includes both global and skill-specific context variables; it also uses the `options.return_context` property to request that the context be returned with the response.

```javascript
service.message({
  assistant_id: '{assistant_id}',
  session_id: '{session_id}',
  input: {
    'message_type': 'text',
    'text': 'Hello',
    'options': {
      'return_context': true
    }
  },
  context: {
    'global': {
      'system': {
        'user_id': 'my_user_id'
      }
    },
    'skills': {
      'main skill': {
        'user_defined': {
          'account_number': '123456'
        }
      }
    }
  }
}, function(err, response) {
  if (err)
    console.log('error:', err);
  else
    console.log(JSON.stringify(response, null, 2));
});
```
{: codeblock}
{: javascript}

```python
response=service.message(
    assistant_id='{assistant_id}',
    session_id='{session_id}',
    input={
        'message_type': 'text',
        'text': 'Hello',
        'options': {
            'return_context': True
        }
    },
    context={
        'global': {
            'system': {
                'user_id': 'my_user_id'
            }
        },
        'skills': {
            'main skill': {
                'user_defined': {
                    'account_number': '123456'
                }
            }
        }
    }
).get_result()

print(json.dumps(response, indent=2))
```
{: codeblock}
{: python}

```java
    MessageInputOptions inputOptions = new MessageInputOptions();
    inputOptions.setReturnContext(true);

    MessageInput input = new MessageInput.Builder()
      .messageType("text")
      .text("Hello")
      .options(inputOptions)
      .build();

    // create global context with user ID
    MessageContextGlobalSystem system = new MessageContextGlobalSystem();
    system.setUserId("my_user_id");
    MessageContextGlobal globalContext = new MessageContextGlobal();
    globalContext.setSystem(system);

    // build user-defined context variables, put in skill-specific context for main skill
    Map<String, String> userDefinedContext = new HashMap<>();
    userDefinedContext.put("account_num","123456");
    Map<String, Map> mainSkillContext = new HashMap<>();
    mainSkillContext.put("user_defined", userDefinedContext);
    MessageContextSkills skillsContext = new MessageContextSkills();
    skillsContext.put("main skill", mainSkillContext);

    MessageContext context = new MessageContext();
    context.setGlobal(globalContext);
    context.setSkills(skillsContext);

    MessageOptions options = new MessageOptions.Builder("{assistant_id}", "{session_id}")
      .input(input)
      .context(context)
      .build();

    MessageResponse response = service.message(options).execute();

    System.out.println(response);
```
{: codeblock}
{: java}

In this example request, the application specifies a value for `user_id` as part of the global context. In addition, it sets one user-defined context variable (`account_number`) as part of the skill-specific context. This context variable can be accessed by dialog nodes as `$account_number`. (For more information about using the context in your dialog, see [How the dialog is processed](https://{DomainName}/docs/services/assistant/dialog-runtime.html#dialog-runtime).)

You can specify any variable name you want to use for a user-defined context variable. If the specified variable already exists, it is overwritten with the new value; if not, a new variable is added to the context.

The output from this request includes not only the usual output, but also the context, showing that the specified values have been added.

```json
{
  "output": {
    "generic": [
      {
        "response_type": "text",
        "text": "Welcome to the Watson Assistant example!"
      }
    ],
    "intents": [
      {
        "intent": "hello",
        "confidence": 1
      }
    ],
    "entities": []
  },
  "context": {
    "global": {
      "system": {
        "turn_count": 1,
        "user_id": "my_user_id"
      }
    },
    "skills": {
      "main skill": {
        "user_defined": {
          "account_number": "123456"
        }
      }
    }
  }
}
```

For detailed information about how to access context variables using the API, see the [v2 API Reference ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://{DomainName}/apidocs/assistant-v2#send-user-input-to-assistant){: new_window}.)

## Using the v1 API
{: #v1-api}

Using the v2 API is the recommended way to build a runtime client application that communicates with the {{site.data.keyword.conversationshort}} service. However, some older applications might still be using the v1 API, which includes a similar runtime method for sending messages to the workspace within a dialog skill.

Note that if your app uses the v1 API, it communicates directly with the workspace, bypassing the orchestration and state-management capabilities of the assistant. This means that your application is responsible for managing state information using the context. Your application must maintain the context by saving the context received with each response and sending it back to the service with each new message request. (The v1 `/message` method always returns the context with each response.)

For more information about the v1 `/message` method and context, see the [v1 API Reference ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://cloud.ibm.com/apidocs/assistant#get-response-to-user-input){: new_window}.