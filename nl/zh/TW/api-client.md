---

copyright:
  years: 2015, 2019
lastupdated: "2019-08-08"

subcollection: assistant


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

# 建置用戶端應用程式
{: #api-client}

因此，您會有一個可運行的對話技能及一個使用該技能的助理。現在您想要開發一個與使用者互動並與 {{site.data.keyword.conversationfull}} 服務通訊的自訂用戶端應用程式。
{: shortdesc}

## 設定 {{site.data.keyword.conversationshort}} 服務
{: #api-client-setup}

我們在本節建立的範例應用程式，會實作認知個人助理的數個功能。應用程式碼將連接至執行認知處理（例如，偵測使用者目的）的 {{site.data.keyword.conversationshort}} 助理。

繼續使用此範例之前，您需要設定必要的助理：

1.  下載對話技能 <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/assistant/assistant-simple-example.json" download="assistant-simple-example.json">JSON 檔案</a>。
1.  [匯入技能](/docs/services/assistant?topic=assistant-skill-dialog-add#skill-dialog-add-task)至 {{site.data.keyword.conversationshort}} 服務的實例。
1.  [建立助理](/docs/services/assistant?topic=assistant-assistant-add)，並連接您所匯入的技能。

## 取得服務資訊
{: #api-client-get-info}

若要存取 {{site.data.keyword.conversationshort}} 服務 REST API，您的應用程式需要可以向 {{site.data.keyword.Bluemix}} 進行鑑別，並連接至正確的助理。您需要複製服務認證及助理 ID，並將它們貼入至應用程式碼。

若要從 {{site.data.keyword.conversationshort}} 工具存取服務認證及助理 ID，請移至**助理**標籤，然後針對您要連接的助理，按一下 ![功能表](images/kebab-react.png) 功能表。選取**檢視 API 詳細資料**，來查看助理的詳細資料，包括助理 ID 及 API 金鑰。

您也可以從 {{site.data.keyword.Bluemix_short}} 儀表板中存取服務認證。

## 與 {{site.data.keyword.conversationshort}} 服務通訊
{: #api-client-communicate}

與 {{site.data.keyword.conversationshort}} 服務互動十分簡單。讓我們看一下連接至服務、傳送單一訊息，然後將輸出列印至主控台的範例：

```javascript
// Example 1: sets up service wrapper, sends initial message, and
// receives response.

const AssistantV2 = require('ibm-watson/assistant/v2');

// Set up Assistant service wrapper.
const service = new AssistantV2({
  iam_apikey: '{apikey}', // replace with API key
  version: '2019-02-28',
});

const assistantId = '{assistant_id}'; // replace with assistant ID
let sessionId;

// Create session.
service
  .createSession({
    assistant_id: assistantId,
  })
  .then(res => {
    sessionId = res.session_id;
    sendMessage({
      message_type: 'text',
      text: ''  // start conversation with empty message
    });
  })
  .catch(err => {
    console.log(err); // something went wrong
  });

// Send message to assistant.
function sendMessage(messageInput) {
  service
    .message({
      assistant_id: assistantId,
      session_id: sessionId,
      input: messageInput
    })
    .then(res => {
      processResponse(res);
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


// We're done, so we close the session.
service
  .deleteSession({
    assistant_id: assistantId,
    session_id: sessionId,
  })
  .catch(err => {
    console.log(err); // something went wrong
  });
}
```
{: codeblock}
{: javascript}

```python
# Example 1: sets up service wrapper, sends initial message, and
# receives response.

import ibm_watson

# Set up Assistant service.
service = ibm_watson.AssistantV2(
    iam_apikey = '{apikey}', # replace with API key
    version = '2019-02-28'
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

# Print the output from dialog, if any. Supports only a single
# text response.
if response['output']['generic']:
    if response['output']['generic'][0]['response_type'] == 'text':
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

import com.ibm.watson.assistant.v2.Assistant;
import com.ibm.watson.assistant.v2.model.CreateSessionOptions;
import com.ibm.watson.assistant.v2.model.DeleteSessionOptions;
import com.ibm.watson.assistant.v2.model.MessageOptions;
import com.ibm.watson.assistant.v2.model.MessageResponse;
import com.ibm.watson.assistant.v2.model.SessionResponse;
import com.ibm.cloud.sdk.core.service.security.IamOptions;
import java.util.logging.LogManager;

public class AssistantSimpleExample {
  public static void main(String[] args) {

    // Suppress log messages in stdout.
    LogManager.getLogManager().reset();

    // Set up Assistant service.
    IamOptions iamOptions = new IamOptions.Builder().apiKey("{apikey}").build();
    Assistant service = new Assistant("2019-02-28", iamOptions);
    String assistantId = "{assistant_id}"; // replace with assistant ID

    // Create session.
CreateSessionOptions createSessionOptions = new CreateSessionOptions.Builder(assistantId).build();
    SessionResponse session = service.createSession(createSessionOptions).execute().getResult();
    String sessionId = session.getSessionId();

    // Start conversation with empty message.
    MessageOptions messageOptions = new MessageOptions.Builder(assistantId,
                                                        sessionId).build();
    MessageResponse response = service.message(messageOptions).execute().getResult();

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

第一步是建立 {{site.data.keyword.conversationshort}} 服務的封套。

封套是一個物件，您將用來將輸入傳送至服務並接收來自服務的輸出。在建立服務封套時，請指定來自服務金鑰的鑑別認證，以及所使用的 {{site.data.keyword.conversationshort}} API 版本。

在此 Node.js 範例中，封套是儲存在變數 `service` 的 `AssistantV2` 實例。其他語言的 Watson SDK 提供用於實例化服務封套的相等機制。
{: javascript}

在此 Python 範例中，封套是儲存在變數 `service` 的 `watson_developer_cloud.AssistantV2` 實例。其他語言的 Watson SDK 提供用於實例化服務封套的相等機制。
{: python}

在此 Java 範例中，封套是儲存在變數 `service` 的 `Assistant` 實例。其他語言的 Watson SDK 提供用於實例化服務封套的相等機制。
{: java}

建立服務封套之後，我們會用它來建立階段作業，並傳送訊息至助理。在此範例中，訊息是空的；我們只想在對話中觸發 conversation_start 節點，因此不需要任何輸入文字。然後，我們會將回應文字列印至主控台，最後刪除階段作業。

使用 `node <filename.js>` 指令，以執行範例應用程式。
{: javascript}

使用 `python3 <filename.py>` 指令，以執行範例應用程式。
{: python}

將程式碼範例貼到名為 `AssistantSimpleExample.java` 的檔案中。然後，您可以進行編譯並執行。
{: java}

**附註：**請確定您已使用 `npm install ibm-watson` 安裝 Watson SDK for Node.js。
{: javascript}

**附註：**請確定您已使用 `pip install --upgrade ibm-watson` 或 `easy_install --upgrade ibm-watson` 安裝 Watson SDK for Python。
{: python}

**附註：**請確定您已安裝 [Watson SDK for Java ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/watson-developer-cloud/java-sdk/blob/master/README.md){: new_window}。
{: java}

假設所有作業都如預期般運作，則助理會傳回對話的輸出，然後將其列印至主控台：

```
Welcome to the Watson Assistant example!
```
{: screen}

此輸出告訴我們，我們已順利與 {{site.data.keyword.conversationshort}} 服務通訊，並在對話中收到 conversation_start 節點所指定的歡迎訊息。我們現在可以新增使用者介面，將它用來處理使用者輸入。

## 處理使用者輸入以偵測目的
{: #api-client-process-input}

為了可以處理使用者輸入，我們需要將使用者介面新增至用戶端應用程式。在此範例中，我們將簡化作業，並使用標準輸入及輸出。
<span class="ph style-scope doc-content" data-hd-programlang="javascript">我們可以使用 Node.js prompt-sync 模組來執行這項作業。（您可以使用 `npm install prompt-sync` 來安裝 prompt-sync。）</span>
<span class="ph style-scope doc-content" data-hd-programlang="python">我們可以使用 Python 3 `input` 函數來執行這項作業。</span>
<span class="ph style-scope doc-content" data-hd-programlang="java">我們可以使用 Java `Console.readLine()` 函數來執行這項作業。</span>

```javascript
// Example 2: adds user input and detects intents.

const prompt = require('prompt-sync')();
const AssistantV2 = require('ibm-watson/assistant/v2');

// Set up Assistant service wrapper.
const service = new AssistantV2({
  iam_apikey: '{apikey}', // replace with API key
  version: '2019-02-28',
});

const assistantId = '{assistant_id}'; // replace with assistant ID
let sessionId;

// Create session.
service
  .createSession({
    assistant_id: assistantId,
  })
  .then(res => {
    sessionId = res.session_id;
    sendMessage({
      message_type: 'text',
      text: ''
    }); // start conversation with empty message
  })
  .catch(err => {
    console.log(err); // something went wrong
  });

// Send message to assistant.
function sendMessage(messageInput) {
  service
    .message({
      assistant_id: assistantId,
      session_id: sessionId,
      input: messageInput
    })
    .then(res => {
      processResponse(res);
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

  // Prompt for the next round of input.
    const newMessageFromUser = prompt('>> ');
  if (newMessageFromUser === 'quit') {
    service
      .deleteSession({
        assistant_id: assistantId,
        session_id: sessionId,
      })
      .catch(err => {
        console.log(err); // something went wrong
      });
    return;
  }
  newMessageInput = {
    message_type: 'text',
    text: newMessageFromUser
  }
  sendMessage(newMessageInput);
}
```
{: codeblock}
{: javascript}

```python
# Example 2: adds user input and detects intents.

import ibm_watson

# Set up Assistant service.
service = ibm_watson.AssistantV2(
    iam_apikey = '{apikey}', # replace with API key
    version = '2019-02-28'
)

assistant_id = '{assistant_id}' # replace with assistant ID

# Create session.
session_id = service.create_session(
    assistant_id = assistant_id
).get_result()['session_id']

# Initialize with empty value to start the conversation.
message_input = {
    'message_type:': 'text',
    'text': ''
    }

# Main input/output loop
while message_input['text'] != 'quit':

    # Send message to assistant.
    response = service.message(
        assistant_id,
        session_id,
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

import com.ibm.watson.assistant.v2.Assistant;
import com.ibm.watson.assistant.v2.model.CreateSessionOptions;
import com.ibm.watson.assistant.v2.model.DeleteSessionOptions;
import com.ibm.watson.assistant.v2.model.DialogRuntimeResponseGeneric;
import com.ibm.watson.assistant.v2.model.MessageInput;
import com.ibm.watson.assistant.v2.model.MessageOptions;
import com.ibm.watson.assistant.v2.model.MessageResponse;
import com.ibm.watson.assistant.v2.model.RuntimeIntent;
import com.ibm.watson.assistant.v2.model.SessionResponse;
import com.ibm.cloud.sdk.core.service.security.IamOptions;
import java.util.List;
import java.util.logging.LogManager;

public class AssistantSimpleExample {
  public static void main(String[] args) {

    // Suppress log messages in stdout.
    LogManager.getLogManager().reset();

    // Set up Assistant service.
    IamOptions iamOptions = new IamOptions.Builder().apiKey("{apikey}").build();
    Assistant service = new Assistant("2019-02-28", iamOptions);
    String assistantId = "{assistant_id}"; // replace with assistant ID

    // Create session.
CreateSessionOptions createSessionOptions = new CreateSessionOptions.Builder(assistantId).build();
    SessionResponse session = service.createSession(createSessionOptions).execute().getResult();
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
      MessageResponse response = service.message(messageOptions).execute().getResult();

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

此版本應用程式的開始方式像之前一樣：將空白訊息傳送至助理以開始交談。

`processResponse()` 函數現在會顯示對話技能偵測到的任何目的，以及輸出文字。之後，它會提示下一輪的使用者輸入。
{: javascript }

然後，它會顯示對話技能偵測到的任何目的，以及輸出文字。之後，它會提示下一輪的使用者輸入。
{: python }

然後，它會顯示對話所偵測到的任何目的以及輸出文字，並在之後提示輸入下一輪的使用者輸入。
{: java}

我們尚未實作自然語言方式來結束交談，因此，改用文字指令 `quit` 來指示程式應該刪除階段作業並結束。

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

我們現在已有所進展！{{site.data.keyword.conversationshort}} 服務會正確地辨識我們的目的，且對話會傳回每一個目的的正確輸出文字（如果提供的話）。

不過，不會發生任何其他情況。當我們詢問時間時，未收到任何回答；當我們說再見時，並未結束交談。這是因為那些目的需要應用程式採取其他動作。

## 實作應用程式動作
{: #api-client-implement-actions}

除了要向使用者顯示的輸出文字之外，我們的 {{site.data.keyword.conversationshort}} 對話還會使用回應 JSON 中的 `actions` 陣列，根據偵測到的目的來發出應用程式需要執行動作的信號。對話在判斷用戶端應用程式需要執行某項作業時，它會傳回 `type` 為 `client` 的動作物件。動作的 `name` 指出特定動作，即 `display_time` 或 `end_conversation`。（動作的其他內容可以指定與動作相關的參數、認證和其他資訊，但我們的範例不需要動作名稱之外的任何項目。）

我們知道，對話永不會一次要求多個動作，因此，用戶端只需要檢查 `actions` 陣列中是否存在單一 `client` 動作。如果找到一個，則其可以執行指定的動作。（此版本也移除偵測到之目的的顯示，因為我們確定將會正確地識別這些目的。）

```javascript
// Example 3: implements app actions.

const prompt = require('prompt-sync')();
const AssistantV2 = require('ibm-watson/assistant/v2');

// Set up Assistant service wrapper.
const service = new AssistantV2({
  iam_apikey: '{apikey}', // replace with API key
  version: '2019-02-28',
});

const assistantId = '{assistant_id}'; // replace with assistant ID
let sessionId;

// Create session.
service
  .createSession({
    assistant_id: assistantId,
  })
  .then(res => {
    sessionId = res.session_id;
    sendMessage({
      message_type: 'text',
      text: ''  // start conversation with empty message
    });
  })
  .catch(err => {
    console.log(err); // something went wrong
  });

// Send message to assistant.
function sendMessage(messageInput) {
  service
    .message({
      assistant_id: assistantId,
      session_id: sessionId,
      input: messageInput
    })
    .then(res => {
      processResponse(res);
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
    // Display the output from assistant, if any. Assumes a single text response.
    if (response.output.generic) {
      if (response.output.generic.length > 0) {
        if (response.output.generic[0].response_type === 'text') {
          console.log(response.output.generic[0].text);
      }
    }
    }
  }

  // If we're not done, prompt for the next round of input.
  if (!endConversation) {
    const newMessageFromUser = prompt('>> ');
    newMessageInput = {
      message_type: 'text',
      text: newMessageFromUser
    }
    sendMessage(newMessageInput);
  } else {
    service
      .deleteSession({
        assistant_id: assistantId,
        session_id: sessionId,
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

```python
# Example 3: Implements app actions.

import ibm_watson
import time

# Set up Assistant service.
service = ibm_watson.AssistantV2(
    iam_apikey = '{apikey}', # replace with API key
    version = '2019-02-28'
)

assistant_id = '{assistant_id}' # replace with assistant ID

# Create session.
session_id = service.create_session(
    assistant_id = assistant_id
).get_result()['session_id']

# Initialize with empty values to start the conversation.
message_input = {'text': ''}
current_action = ''

# Main input/output loop
while current_action != 'end_conversation':
    # Clear any action flag set by the previous response.
    current_action = ''

    # Send message to assistant.
    response = service.message(
        assistant_id,
        session_id,
        input = message_input
    ).get_result()

    # Print the output from dialog, if any. Supports only a single
    # text response.
    if response['output']['generic']:
        if response['output']['generic'][0]['response_type'] == 'text':
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
        message_input = {
            'text': user_input
        }

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

import com.ibm.watson.assistant.v2.Assistant;
import com.ibm.watson.assistant.v2.model.CreateSessionOptions;
import com.ibm.watson.assistant.v2.model.DeleteSessionOptions;
import com.ibm.watson.assistant.v2.model.DialogNodeAction;
import com.ibm.watson.assistant.v2.model.DialogRuntimeResponseGeneric;
import com.ibm.watson.assistant.v2.model.MessageInput;
import com.ibm.watson.assistant.v2.model.MessageOptions;
import com.ibm.watson.assistant.v2.model.MessageResponse;
import com.ibm.watson.assistant.v2.model.RuntimeIntent;
import com.ibm.watson.assistant.v2.model.SessionResponse;
import com.ibm.cloud.sdk.core.service.security.IamOptions;
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
    Assistant service = new Assistant("2019-02-28", iamOptions);
    String assistantId = "{assistant_id}"; // replace with assistant ID

    // Create session.
CreateSessionOptions createSessionOptions = new CreateSessionOptions.Builder(assistantId).build();
    SessionResponse session = service.createSession(createSessionOptions).execute().getResult();
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
      MessageResponse response = service.message(messageOptions).execute().getResult();

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

應用程式現在會檢查回應中的 `actions` 陣列，以查看是否有 `type`=`client` 的動作。如果有，它會檢查動作的 `name` 值，並執行適當的動作（顯示本端系統時間，或設定一個指出交談已結束的內部旗標）。

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

成功！應用程式現在會使用 {{site.data.keyword.conversationshort}} 服務來識別自然語言輸入中的目的、顯示適當的回應，然後實作所要求的 client 動作。

當然，現實應用程式會使用更複雜的使用者介面（例如網路聊天視窗）。它會實作更複雜的動作，並可能與客戶資料庫或其他商務系統整合。另外，它也需要將其他資料（例如使用者 ID）傳送給助理，以識別每個唯一的使用者。但是，應用程式如何與 {{site.data.keyword.conversationshort}} 服務互動的基本原則仍會相同。

如需一些更複雜的範例，請參閱[範例應用程式](/docs/services/assistant?topic=assistant-sample-apps)。

## 使用第 1 版 API
{: #api-client-v1-api}

建議使用第 2 版 API 來建置與 {{site.data.keyword.conversationshort}} 服務通訊的運行環境用戶端應用程式。不過，某些較舊的應用程式可能仍在使用第 1 版 API，其中包括類似的運行環境方法，用來將訊息傳送至對話技能內的工作區。

請注意，如果您的應用程式使用第 1 版 API，它會直接與工作區通訊，略過助理的編排及狀態管理功能。這表示您的應用程式負責使用環境定義來管理狀態資訊。您的應用程式必須藉由以下方法來維護環境定義：儲存隨著每個回應一起接收的環境定義，並將它連同每個新的訊息要求傳回給服務。（第 1 版 `/message` 方法一律會傳回環境定義與每個回應。）

如需第 1 版 `/message` 方法及環境定義的相關資訊，請參閱[第 1 版 API 參考資料 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://cloud.ibm.com/apidocs/assistant#get-response-to-user-input){: new_window}。
