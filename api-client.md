---

copyright:
  years: 2015, 2018
lastupdated: "2018-04-24"

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
{:tip: .tip}

# Building a client application

So you have a working dialog skill and an assistant that uses it. Now you want to develop a custom client application that will interact with your users and communicate with the {{site.data.keyword.conversationfull}} service.
{: shortdesc}

You can view this tutorial for Node.js (Javascript), Python 3, or Java by clicking the language selector in the upper right. For details about all supported languages, refer to the {{site.data.keyword.watson}} [SDKs ![External link icon](../../icons/launch-glyph.svg "External link icon")](/docs/services/watson/getting-started-sdks.html#sdks){: new_window}.
{: tip }

## Setting up the {{site.data.keyword.conversationshort}} service

The example application we will create in this section implements several functions of a cognitive personal assistant. The application code will connect to a {{site.data.keyword.conversationshort}} assistant, where the cognitive processing (such as the detection of user intents) takes place.

Before continuing with this example, you need to set up the required assistant:

1.  Download the dialog skill <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/conversation/conversation-simple-example.json" download="conversation-simple-example.json">JSON file</a>.
1.  [Import the skill](/docs/services/conversation/configure-workspace.html#creating-workspaces) into an instance of the {{site.data.keyword.conversationshort}} service.
1.  [Create an assistant](create-assistant.html#creating-assistants) and connect the skill you imported.

## Getting service information

**QUESTION: What is the equivalent for v2?**

To access the {{site.data.keyword.conversationshort}} service REST APIs, your application needs to be able to authenticate with {{site.data.keyword.Bluemix}} and connect to the right {{site.data.keyword.conversationshort}} workspace. You'll need to copy the service credentials and workspace ID and paste them into your application code.

To access the service credentials and the workspace ID from your workspace, select the ![Menu](images/Menu_16.png) menu, choose **Deploy**, and then go to the **Credentials** tab.

You can also access the service credentials from your {{site.data.keyword.Bluemix_short}} dashboard.

## Communicating with the {{site.data.keyword.conversationshort}} service

Interacting with the {{site.data.keyword.conversationshort}} service is simple. Let's take a look at an example that connects to the service, sends a single message, and prints the output to the console:

```javascript
// Example 1: sets up service wrapper, sends initial message, and 
// receives response.

var AssistantV1 = require('watson-developer-cloud/assistant/v1');

// Set up Assistant service wrapper.
var service = new AssistantV1({
  username: 'USERNAME', // replace with service username
  password: 'PASSWORD', // replace with service password
  version: '2018-02-16'
});

var workspace_id = 'WORKSPACE_ID'; // replace with workspace ID

// Start conversation with empty message.
service.message({
  workspace_id: workspace_id
  }, processResponse);

// Process the service response.
function processResponse(err, response) {
  if (err) {
    console.error(err); // something went wrong
    return;
  }
  
  // Display the output from dialog, if any.
  if (response.output.text.length != 0) {
      console.log(response.output.text[0]);
  }
}
```
{: codeblock}
{: javascript}

```python
# Example 1: sets up service wrapper, sends initial message, and
# receives response.

import watson_developer_cloud

# Set up Assistant service.
service = watson_developer_cloud.AssistantV1(
  username = 'USERNAME', # replace with service username
  password = 'PASSWORD', # replace with service password
  version = '2018-02-16'
)
workspace_id = 'WORKSPACE_ID' # replace with workspace ID

# Start conversation with empty message.
response = service.message(
  workspace_id = workspace_id,
  input = {
    'text': ''
  }
)

# Print the output from dialog, if any.
if response['output']['text']:
  print(response['output']['text'][0])
```
{: codeblock}
{: python}

```java
/*
 * Example 1: sets up service wrapper, sends initial message, and
 * receives response.
 */

import com.ibm.watson.developer_cloud.assistant.v1.Assistant;
import com.ibm.watson.developer_cloud.assistant.v1.model.MessageOptions;
import com.ibm.watson.developer_cloud.assistant.v1.model.MessageRequest;
import com.ibm.watson.developer_cloud.assistant.v1.model.MessageResponse;
import java.util.logging.LogManager;

public class AssistantSimpleExample {
  public static void main(String[] args) {

    // Suppress log messages in stdout.
    LogManager.getLogManager().reset();

    // Set up Assistant service.
    Assistant service = new Assistant("2018-02-16");
    service.setUsernameAndPassword("USERNAME", // replace with service username
                                   "PASSWORD"); // replace with service password
    String workspaceId = "WORKSPACE_ID"; // replace with workspace ID

    // Start assistant with empty message.
    MessageOptions options = new MessageOptions.Builder(workspaceId).build();
    MessageResponse response = service.message(options).execute();

    System.out.println(response.getOutput().getText().get(0));
  }
}
```
{: codeblock}
{: java}

The first step is to create a wrapper for the {{site.data.keyword.conversationshort}} service.

The wrapper is an object you will use to send input to, and receive output from, the service. When you create the service wrapper, specify the authentication credentials from the service key, as well as the version of the {{site.data.keyword.conversationshort}} API you are using.

In this Node.js example, the wrapper is an instance of `AssistantV1`, stored in the variable `service`. The Watson SDKs for other languages provide equivalent mechanisms for instantiating a service wrapper.
{: javascript}

In this Python example, the wrapper is an instance of `watson_developer_cloud.AssistantV1`, stored in the variable `service`. The Watson SDKs for other languages provide equivalent mechanisms for instantiating a service wrapper.
{: python}

In this Java example, the wrapper is an instance of `Assistant`, stored in the variable `service`. The Watson SDKs for other languages provide equivalent mechanisms for instantiating a service wrapper.
{: java}

After creating the service wrapper, we use it to send a message to the {{site.data.keyword.conversationshort}} service. In this example, the message is empty; we just want to trigger the conversation_start node in the dialog, so we don't need any input text.

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

Assuming everything works as expected, the {{site.data.keyword.conversationshort}} service returns the output from the dialog, which is then printed to the console:

```
Welcome to the {{site.data.keyword.conversationshort}} example!
```
{: screen}

This output tells us that we have successfully communicated with the {{site.data.keyword.conversationshort}} service and received the welcome message specified by the conversation_start node in the dialog. Now we can add a user interface, making it possible to process user input.

## Processing user input to detect intents

To be able to process user input, we need to add a user interface to our application. For this example, we'll keep things simple and use standard input and output.
<span class="ph style-scope doc-content" data-hd-programlang="javascript">We can use the Node.js prompt-sync module to do this. (You can install prompt-sync using `npm install prompt-sync`.)</span>
<span class="ph style-scope doc-content" data-hd-programlang="python">We can use the Python 3 `input` function to do this.</span>
<span class="ph style-scope doc-content" data-hd-programlang="java">We can use the Java `Console.readLine()` function to do this.</span>

```javascript
// Example 2: adds user input and detects intents.

var prompt = require('prompt-sync')();
var AssistantV1 = require('watson-developer-cloud/assistant/v1');

// Set up Assistant service wrapper.
var service = new AssistantV1({
  username: 'USERNAME', // replace with service username
  password: 'PASSWORD', // replace with service password
  version: '2018-02-16'
});

var workspace_id = 'WORKSPACE_ID'; // replace with workspace ID

// Start conversation with empty message.
service.message({
  workspace_id: workspace_id
  }, processResponse);

// Process the service response.
function processResponse(err, response) {
  if (err) {
    console.error(err); // something went wrong
    return;
  }

  // If an intent was detected, log it out to the console.
  if (response.intents.length > 0) {
    console.log('Detected intent: #' + response.intents[0].intent);
  }

  // Display the output from dialog, if any.
  if (response.output.text.length != 0) {
      console.log(response.output.text[0]);
  }

  // Prompt for the next round of input.
  var newMessageFromUser = prompt('>> ');
  service.message({
    workspace_id: workspace_id,
    input: { text: newMessageFromUser }
    }, processResponse)
}
```
{: codeblock}
{: javascript}

```python
# Example 2: adds user input and detects intents.

import watson_developer_cloud

# Set up Assistant service.
service = watson_developer_cloud.AssistantV1(
  username = 'USERNAME', # replace with service username
  password = 'PASSWORD', # replace with service password
  version = '2018-02-16'
)
workspace_id = 'WORKSPACE_ID' # replace with workspace ID

# Initialize with empty value to start the conversation.
user_input = ''

# Main input/output loop
while True:

  # Send message to Assistant service.
  response = service.message(
    workspace_id = workspace_id,
    input = {
      'text': user_input
    }
  )

  # If an intent was detected, print it to the console.
  if response['intents']:
    print('Detected intent: #' + response['intents'][0]['intent'])

  # Print the output from dialog, if any.
  if response['output']['text']:
    print(response['output']['text'][0])

  # Prompt for next round of input.
  user_input = input('>> ')
```
{: codeblock }
{: python }

```java
/*
 * Example 2: adds user input and detects intents.
 */

import com.ibm.watson.developer_cloud.assistant.v1.Assistant;
import com.ibm.watson.developer_cloud.assistant.v1.model.InputData;
import com.ibm.watson.developer_cloud.assistant.v1.model.MessageOptions;
import com.ibm.watson.developer_cloud.assistant.v1.model.MessageRequest;
import com.ibm.watson.developer_cloud.assistant.v1.model.MessageResponse;
import com.ibm.watson.developer_cloud.assistant.v1.model.RuntimeIntent;
import java.util.List;
import java.util.logging.LogManager;

public class AssistantSimpleExample {
  public static void main(String[] args) {

    // Suppress log messages in stdout.
    LogManager.getLogManager().reset();

    // Set up Assistant service.
    Assistant service = new Assistant("2018-02-16");
    service.setUsernameAndPassword("USERNAME", // replace with service username
                                   "PASSWORD"); // replace with service password
    String workspaceId = "WORKSPACE_ID"; // replace with workspace ID

    // Initialize with empty value to start the conversation.
    MessageOptions options = new MessageOptions.Builder(workspaceId).build();

    // Main input/output loop
    do {
      // Send message to Assistant service.
      MessageResponse response = service.message(options).execute();      
      String responseText = response.getOutput().getText().get(0);
      List<RuntimeIntent> responseIntents = response.getIntents();

      // If an intent was detected, print it to the console.
      if(responseIntents.size() > 0) {
        System.out.println("Detected intent: #" + responseIntents.get(0).getIntent());
      }

      // Print the output from dialog, if any.
      System.out.println(responseText);

      // Prompt for next round of input.
      System.out.print(">> ");
      String inputText = System.console().readLine();
      InputData input = new InputData.Builder(inputText).build();
      options = new MessageOptions.Builder(workspaceId).input(input).build();
    } while(true);
  }
}
```
{: codeblock }
{: java }

This version of the application begins the same way as before: sending an empty message to the {{site.data.keyword.conversationshort}} service to start the conversation.

The `processResponse()` function now displays any intent detected by the dialog along with the output text, and then it then prompts for the next round of user input.
{: javascript }

It then displays any intent detected by the dialog along with the output text, and then it prompts for the next round of user input. (We're using a `while True` loop for now, since we haven't yet implemented a way of ending the conversation.)
{: python }

It then displays any intent detected by the dialog along with the output text, and then it prompts for the next round of user input. (We're using a `do-while` loop with `while(true)` for now, since we haven't yet implemented a way of ending the conversation.)
{: java}

But something still isn't right:

```
Welcome to the {{site.data.keyword.conversationshort}} example!
>> hello
Detected intent: #hello
Welcome to the {{site.data.keyword.conversationshort}} example!
>> what time is it?
Detected intent: #time
Welcome to the {{site.data.keyword.conversationshort}} example!
>> goodbye
Detected intent: #goodbye
Welcome to the {{site.data.keyword.conversationshort}} example!
>>
```
{: screen}

The {{site.data.keyword.conversationshort}} service is detecting the correct intents, and yet every turn of the conversation returns the welcome message from the conversation_start node (`Welcome to the {{site.data.keyword.conversationshort}} example!`).

This is happening because the {{site.data.keyword.conversationshort}} service is stateless; it is the responsibility of the application to maintain state information. Because we are not yet doing anything to maintain state, the {{site.data.keyword.conversationshort}} service sees every round of user input as the first turn of a new conversation, triggering the conversation_start node.

## Maintaining state

State information for your conversation is maintained using the *context*. The context is an object that is passed back and forth between your application and the {{site.data.keyword.conversationshort}} service. It is the responsibility of your application to maintain the context from one turn of the conversation to the next.

The context includes a unique identifier for each conversation with a user, as well as a counter that is incremented with each turn of the conversation. Our previous version of the example did not preserve the context, which means that each round of input appeared to be the start of a new conversation. We can fix that by saving the context and sending it back to the {{site.data.keyword.conversationshort}} service each time.

In addition to maintaining our place in the conversation, the context can also be used to store any other data you want to pass back and forth between your application and the {{site.data.keyword.conversationshort}} service. This can include persistent data you want to maintain throughout the conversation (such as a customer's name or account number), or any other data you want to track (such as the current status of option settings).

```javascript
// Example 3: maintains state.

var prompt = require('prompt-sync')();
var AssistantV1 = require('watson-developer-cloud/assistant/v1');

// Set up Assistant service wrapper.
var service = new AssistantV1({
  username: 'USERNAME', // replace with service username
  password: 'PASSWORD', // replace with service password
  version: '2018-02-16'
});

var workspace_id = 'WORKSPACE_ID'; // replace with workspace ID

// Start conversation with empty message.
service.message({
  workspace_id: workspace_id
  }, processResponse);

// Process the service response.
function processResponse(err, response) {
  if (err) {
    console.error(err); // something went wrong
    return;
  }

  // If an intent was detected, log it out to the console.
  if (response.intents.length > 0) {
    console.log('Detected intent: #' + response.intents[0].intent);
  }

  // Display the output from dialog, if any.
  if (response.output.text.length != 0) {
      console.log(response.output.text[0]);
  }

  // Prompt for the next round of input.
    var newMessageFromUser = prompt('>> ');
    // Send back the context to maintain state.
    service.message({
      workspace_id: workspace_id,
      input: { text: newMessageFromUser },
      context : response.context,
    }, processResponse)
}
```
{: codeblock}
{: javascript }

```python
# Example 3: maintains state.

import watson_developer_cloud

# Set up Assistant service.
service = watson_developer_cloud.AssistantV1(
  username = 'USERNAME', # replace with service username
  password = 'PASSWORD', # replace with service password
  version = '2018-02-16'
)
workspace_id = 'WORKSPACE_ID' # replace with workspace ID

# Initialize with empty value to start the conversation.
user_input = ''
context = {}

# Main input/output loop
while True:

  # Send message to Assistant service.
  response = service.message(
    workspace_id = workspace_id,
    input = {
      'text': user_input
    },
    context = context
  )

  # If an intent was detected, print it to the console.
  if response['intents']:
    print('Detected intent: #' + response['intents'][0]['intent'])

  # Print the output from dialog, if any.
  if response['output']['text']:
    print(response['output']['text'][0])

  # Update the stored context with the latest received from the dialog.
  context = response['context']

  # Prompt for next round of input.
  user_input = input('>> ')
```
{: codeblock }
{: python }

```java
/*
 * Example 3: maintains state.
 */

import com.ibm.watson.developer_cloud.assistant.v1.Assistant;
import com.ibm.watson.developer_cloud.assistant.v1.model.Context;
import com.ibm.watson.developer_cloud.assistant.v1.model.InputData;
import com.ibm.watson.developer_cloud.assistant.v1.model.MessageOptions;
import com.ibm.watson.developer_cloud.assistant.v1.model.MessageRequest;
import com.ibm.watson.developer_cloud.assistant.v1.model.MessageResponse;
import com.ibm.watson.developer_cloud.assistant.v1.model.RuntimeIntent;
import java.util.List;
import java.util.logging.LogManager;

public class AssistantSimpleExample {
  public static void main(String[] args) {

    // Suppress log messages in stdout.
    LogManager.getLogManager().reset();

    // Set up Assistant service.
    Assistant service = new Assistant("2018-02-16");
    service.setUsernameAndPassword("USERNAME", // replace with service username
                                   "PASSWORD"); // replace with service password
    String workspaceId = "WORKSPACE_ID"; // replace with workspace ID

    // Initialize with empty value to start the conversation.
    MessageOptions options = new MessageOptions.Builder(workspaceId).build();
    Context context = new Context();

    // Main input/output loop
    do {
      // Send message to Assistant service.
      MessageResponse response = service.message(options).execute();

      // If an intent was detected, print it to the console.
      List<RuntimeIntent> responseIntents = response.getIntents();
      if(responseIntents.size() > 0) {
        System.out.println("Detected intent: #" + responseIntents.get(0).getIntent());
      }

      // Print the output from dialog, if any.
      List<String> responseText = response.getOutput().getText();
      if(responseText.size() > 0) {
        System.out.println(responseText.get(0));
      }

      // Update the stored context with the latest received from the dialog.
      context = response.getContext();

      // Prompt for next round of input.
      System.out.print(">> ");
      String inputText = System.console().readLine();
      InputData input = new InputData.Builder(inputText).build();
      options = new MessageOptions.Builder(workspaceId).input(input).context(context).build();
    } while(true);
  }
}
```
{: codeblock }
{: java }

The only change from the previous example is that with each round of the conversation, we now send back the `response.context` object we received in the previous round:
{: javascript }

```javascript
    service.message({
      input: { text: newMessageFromUser },
      context : response.context,
    }, processResponse)
```
{: codeblock}
{: javascript }

The only change from the previous example is that we are now storing the context received from the dialog in a variable called `context`, and we're sending it back with the next round of user input:
{: python }

```python
  response = service.message(
    workspace_id = workspace_id,
    input = {
      'text': user_input
    },
    context = context
  )
```
{: codeblock }
{: python }

The only change from the previous example is that we are now storing the context received from the dialog in a variable called `context`, and we're including it as part of the options sent back with the next round of user input:
{: java}

```java
options = new MessageOptions.Builder(workspaceId).input(input).context(context).build();
```
{: codeblock }
{: java }

This ensures that the context is maintained from one turn to the next, so the {{site.data.keyword.conversationshort}} service no longer thinks every turn is the first:

```
>> hello
Detected intent: #hello
Good day to you.
>> what time is it?
Detected intent: #time
>> goodbye
Detected intent: #goodbye
OK! See you later.
>>
```
{: screen}

Now we're making progress! The {{site.data.keyword.conversationshort}} service is correctly recognizing our intents, and the dialog is returning the correct output text (where provided) for each intent.

However, nothing else is happening. When we ask for the time, we get no answer; and when we say goodbye, the conversation does not end. That's because those intents require additional actions to be taken by the app.

## Implementing app actions

In addition to the output text to be displayed to the user, our {{site.data.keyword.conversationshort}} dialog uses the `output` object in the response JSON to signal when the application needs to carry out an action, based on the detected intents.

These action flags are sent using the `action` property, which our dialog defines as part of the response JSON. When the dialog determines that the application needs to do something, it sets the value of `action` to the appropriate value, either `display_time` or `end_conversation`.

Keep in mind that `output` is just a JSON object, and you can add any valid content to it that you want. For a more complex application, you might use an array with multiple action flags.

But in our example, we're using a simple key/value pair that supports a single action flag. Our application code needs to check the value of the `action` property in the response and then carry out any specified action. (This version also removes the display of detected intents, now that we're sure those are being correctly identified.)

```javascript
// Example 4: implements app actions.

var prompt = require('prompt-sync')();
var AssistantV1 = require('watson-developer-cloud/assistant/v1');

// Set up Assistant service wrapper.
var service = new AssistantV1({
  username: 'USERNAME', // replace with service username
  password: 'PASSWORD', // replace with service password
  version: '2018-02-16'
});

var workspace_id = 'WORKSPACE_ID'; // replace with workspace ID

// Start conversation with empty message.
service.message({
  workspace_id: workspace_id
  }, processResponse);

// Process the service response.
function processResponse(err, response) {
  if (err) {
    console.error(err); // something went wrong
    return;
  }

  var endConversation = false;

  // Check for action flags.
  if (response.output.action === 'display_time') {
    // User asked what time it is, so we output the local system time.
    console.log('The current time is ' + new Date().toLocaleTimeString() + '.');
  } else if (response.output.action === 'end_conversation') {
    // User said goodbye, so we're done.
    console.log(response.output.text[0]);
    endConversation = true;
  } else {
    // Display the output from dialog, if any.
    if (response.output.text.length != 0) {
        console.log(response.output.text[0]);
    }
  }

  // If we're not done, prompt for the next round of input.
  if (!endConversation) {
    var newMessageFromUser = prompt('>> ');
    service.message({
      workspace_id: workspace_id,
      input: { text: newMessageFromUser },
      // Send back the context to maintain state.
      context : response.context,
    }, processResponse)
  }
}
```
{: codeblock}
{: javascript}

```python
# Example 4: implements app actions.

import watson_developer_cloud
import time

# Set up Assistant service.
service = watson_developer_cloud.AssistantV1(
  username = 'USERNAME', # replace with service username
  password = 'PASSWORD', # replace with service password
  version = '2018-02-16'
)
workspace_id = 'WORKSPACE_ID' # replace with workspace ID

# Initialize with empty value to start the conversation.
user_input = ''
context = {}
current_action = ''

# Main input/output loop
while current_action != 'end_conversation':

  # Send message to Assistant service.
  response = service.message(
    workspace_id = workspace_id,
    input = {
      'text': user_input
    },
    context = context
  )

  # Print the output from dialog, if any.
  if response['output']['text']:
    print(response['output']['text'][0])

  # Update the stored context with the latest received from the dialog.
  context = response['context']
  # Check for action flags sent by the dialog.
  if 'action' in response['output']:
    current_action = response['output']['action']
  # User asked what time it is, so we output the local system time.
  if current_action == 'display_time':
    print('The current time is ' + time.strftime('%I:%M:%S %p') + '.')
  # If we're not done, prompt for next round of input.
  if current_action != 'end_conversation':
    user_input = input('>> ')
```
{: codeblock}
{: python}

```java
/*
 * Example 4: implements app actions.
 */

import com.ibm.watson.developer_cloud.assistant.v1.Assistant;
import com.ibm.watson.developer_cloud.assistant.v1.model.Context;
import com.ibm.watson.developer_cloud.assistant.v1.model.InputData;
import com.ibm.watson.developer_cloud.assistant.v1.model.MessageOptions;
import com.ibm.watson.developer_cloud.assistant.v1.model.MessageRequest;
import com.ibm.watson.developer_cloud.assistant.v1.model.MessageResponse;
import com.ibm.watson.developer_cloud.assistant.v1.model.RuntimeIntent;
import java.time.LocalTime;
import java.time.format.DateTimeFormatter;
import java.util.List;
import java.util.logging.LogManager;
import org.json.JSONObject;

public class AssistantSimpleExample {
  public static void main(String[] args) {

    // Suppress log messages in stdout.
    LogManager.getLogManager().reset();

    // Set up Assistant service.
    Assistant service = new Assistant("2018-02-16");
    service.setUsernameAndPassword("USERNAME", // replace with service username
                                   "PASSWORD"); // replace with service password
    String workspaceId = "WORKSPACE_ID"; // replace with workspace ID

    // Initialize with empty value to start the conversation.
    MessageOptions options = new MessageOptions.Builder(workspaceId).build();
    Context context = new Context();
    String currentAction = "";

    // Main input/output loop
    do {
      // Send message to Assistant service.
      MessageResponse response = service.message(options).execute();

      // Print the output from dialog, if any.
      List<String> responseText = response.getOutput().getText();
      if(responseText.size() > 0) {
        System.out.println(responseText.get(0));
      }

      // Update the stored context with the latest received from the dialog.
      context = response.getContext();

      // Check for action flags sent by the dialog.
      JSONObject outputObj = new JSONObject(response.getOutput().toString());
      if(outputObj.has("action")) {
        currentAction = outputObj.getString("action");
      }
      else {
        currentAction = "";
      }

      // User asked wht time it is, so we output the local system time.
      if(currentAction.equals("display_time")) {
        DateTimeFormatter fmt = DateTimeFormatter.ofPattern("h:mm:ss a");
        LocalTime time = LocalTime.now();
        System.out.println("The current time is " + time.format(fmt) + ".");
      }

      // If we're not done, prompt for next round of input.
      if(!currentAction.equals("end_conversation")) {
        System.out.print(">> ");
        String inputText = System.console().readLine();
        InputData input = new InputData.Builder(inputText).build();
        options = new MessageOptions.Builder(workspaceId).input(input).context(context).build();
      }

    } while(!currentAction.equals("end_conversation"));
  }
}
```
{: codeblock}
{: java}

The processResponse() function now checks the value of the `action` property of the `output` object received from the {{site.data.keyword.conversationshort}} service. If the value is either `display_time` or `end_conversation`, the application carries out the appropriate action.
{: javascript}

The app now checks the value of the `action` property of the `output` object received from the {{site.data.keyword.conversationshort}} service. If the value is `display_time`, the application carries out the appropriate action. If the value is `end_conversation`, the app knows not to prompt for more user input, and the `while` loop ends.
{: python}

The app now checks the value of the `action` property of the `output` object received from the {{site.data.keyword.conversationshort}} service. If the value is `display_time`, the application carries out the appropriate action. If the value is `end_conversation`, the app knows not to prompt for more user input, and the `do-while` loop ends.
{: java}

**Note:** Because `action` is a custom property we defined in our response JSON, we cannot access it directly using the OutputData class. Instead, we convert the output back to JSON and then use the [JSON-java (org.json) ![External link icon](../../icons/launch-glyph.svg "External link icon")]](https://github.com/stleary/JSON-java){: new_window} package to parse it. (Make sure you have installed this package before you compile this version of the example.)
{: java}

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

Of course, a real-world application would use a more sophisticated user interface, such as a web chat window. And it would implement more complex actions, possibly integrating with a customer database or other business systems. But the basic principles of how the application interacts with the {{site.data.keyword.conversationshort}} service would remain the same.

For some more complex examples, see [Sample apps](sample-applications.html).