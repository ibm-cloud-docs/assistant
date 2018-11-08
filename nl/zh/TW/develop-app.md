---

copyright:
  years: 2015, 2018
lastupdated: "2018-01-29"

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

# 建置用戶端應用程式

您有一個運作中的對話。現在要開發與使用者互動並與 {{site.data.keyword.conversationfull}} 服務通訊的應用程式。
{: shortdesc}

您可以按一下右上方的語言選取器，來檢視 Node.js (Javascript) 或 Python 的這個指導教學。如需所有支援語言的詳細資料，請參閱 {{site.data.keyword.watson}} [SDK ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](/docs/services/watson/getting-started-sdks.html#sdks){: new_window}。
{: tip }

## 設定 {{site.data.keyword.conversationshort}} 服務

我們在本節建立的範例應用程式，會實作認知個人助理的數個功能。應用程式碼將連接至執行認知處理（例如偵測使用者目的）的 {{site.data.keyword.conversationshort}} 工作區。

繼續使用此範例之前，您需要設定必要的 {{site.data.keyword.conversationshort}} 工作區：

1.  下載 <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/conversation/conversation-simple-example.json" download="conversation-simple-example.json">JSON 檔案</a>工作區。
1.  [匯入工作區](/docs/services/conversation/configure-workspace.html#creating-workspaces)至 {{site.data.keyword.conversationshort}} 服務實例。

## 取得服務資訊

若要存取 {{site.data.keyword.conversationshort}} 服務 REST API，您的應用程式需要可以向 {{site.data.keyword.Bluemix}} 進行鑑別，並連接至正確的 {{site.data.keyword.conversationshort}} 工作區。您需要複製服務認證及工作區 ID，並將它們貼入至應用程式碼。

若要從工作區中存取服務認證及工作區 ID，請選取 ![功能表](images/Menu_16.png) 功能表，選擇**部署**，然後移至**認證**標籤。

您也可以從 {{site.data.keyword.Bluemix_short}} 儀表板中存取服務認證。

## 與 {{site.data.keyword.conversationshort}} 服務通訊

與 {{site.data.keyword.conversationshort}} 服務互動十分簡單。讓我們看一下連接至服務、傳送單一訊息，然後將輸出列印至主控台的範例：

```javascript
// Example 1: sets up service wrapper, sends initial message, and
// receives response.

var ConversationV1 = require('watson-developer-cloud/conversation/v1');

// Set up Conversation service wrapper.
var conversation = new ConversationV1({
  username: 'USERNAME', // replace with service username
  password: 'PASSWORD', // replace with service password
  version_date: '2017-05-26'
});

var workspace_id = 'WORKSPACE_ID'; // replace with workspace ID

// Start conversation with empty message.
conversation.message({
  workspace_id: workspace_id
  }, processResponse);

// Process the conversation response.
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

# Set up Conversation service.
conversation = watson_developer_cloud.ConversationV1(
  username = 'USERNAME', # replace with username from service key
  password = 'PASSWORD', # replace with password from service key
  version = '2017-05-26'
)
workspace_id = 'WORKSPACE_ID' # replace with workspace ID

# Start conversation with empty message.
response = conversation.message(
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

第一步是建立 {{site.data.keyword.conversationshort}} 服務的封套。

封套是一個物件，您將用來將輸入傳送至服務並接收來自服務的輸出。在建立服務封套時，請指定來自服務金鑰的鑑別認證，以及所使用的 {{site.data.keyword.conversationshort}} API 版本。

在此 Node.js 範例中，封套是儲存在變數 `conversation` 的 `ConversationV1` 實例。其他語言的 Watson SDK 提供用於實例化服務封套的相等機制。
{: javascript}

在此 Python 範例中，封套是儲存在變數 `conversation` 的 `watson_developer_cloud.ConversationV1` 實例。其他語言的 Watson SDK 提供用於實例化服務封套的相等機制。
{: python}

建立服務封套之後，我們會用它來傳送訊息至 {{site.data.keyword.conversationshort}} 服務。在此範例中，訊息是空的；我們只想在對話中觸發 conversation_start 節點，因此不需要任何輸入文字。

使用 `node <filename.js>` 指令，以執行範例應用程式。
{: javascript}

使用 `python <filename.py>` 指令，以執行範例應用程式。
{: python}

**附註：**請確定您已使用 `npm install watson-developer-cloud` 安裝 Watson SDK for Node.js。
{: javascript}

**附註：**請確定您已使用 `pip install --upgrade watson-developer-cloud` 或 `easy_install --upgrade watson-developer-cloud` 安裝 Watson SDK for Python。
{: python}

假設所有作業都如預期般運作，則 {{site.data.keyword.conversationshort}} 服務會傳回對話的輸出，然後將其列印至主控台：

```
Welcome to the {{site.data.keyword.conversationshort}} example!
```
{: screen}

此輸出告訴我們，我們已順利與 {{site.data.keyword.conversationshort}} 服務通訊，並在對話中接收到 conversation_start 節點所指定的歡迎訊息。我們現在可以新增使用者介面，將它用來處理使用者輸入。

## 處理使用者輸入以偵測目的

為了可以處理使用者輸入，我們需要將使用者介面新增至應用程式。在此範例中，我們將簡化作業，並使用標準輸入及輸出。
<span class="ph style-scope doc-content" data-hd-programlang="javascript">我們可以使用 Node.js prompt-sync 模組來執行這項作業（您可以使用 `npm install prompt-sync` 來安裝 prompt-sync。）</span>
<span class="ph style-scope doc-content" data-hd-programlang="python">我們可以使用 Python `input` 函數來執行這項作業。</span>

```javascript
// Example 2: adds user input and detects intents.

var prompt = require('prompt-sync')();
var ConversationV1 = require('watson-developer-cloud/conversation/v1');

// Set up Conversation service wrapper.
var conversation = new ConversationV1({
  username: 'USERNAME', // replace with service username
  password: 'PASWORD', // replace with service password
  version_date: '2017-05-26'
});

var workspace_id = 'WORKSPACE_ID'; // replace with workspace ID

// Start conversation with empty message.
conversation.message({
  workspace_id: workspace_id
  }, processResponse);

// Process the conversation response.
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
  conversation.message({
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

# Set up Conversation service.
conversation = watson_developer_cloud.ConversationV1(
  username = 'USERNAME', # replace with username from service key
  password = 'PASSWORD', # replace with password from service key
  version = '2017-05-26'
)
workspace_id = 'WORKSPACE_ID' # replace with workspace ID

# Initialize with empty value to start the conversation.
user_input = ''

# Main input/output loop
while True:

  # Send message to Conversation service.
  response = conversation.message(
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

此版本應用程式的開始方式像之前一樣：將空白訊息傳送至 {{site.data.keyword.conversationshort}} 服務以開始交談。

`processResponse()` 函數現在會顯示對話所偵測到的任何目的以及輸出文字，然後提示輸入下一輪的使用者輸入。
{: javascript }

然後，它會顯示對話所偵測到的任何目的以及輸出文字，然後提示輸入下一輪的使用者輸入。（目前是使用 `while True` 迴圈，因為我們尚未實作結束交談的方式。）
{: python }

但仍有問題：

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

{{site.data.keyword.conversationshort}} 服務會偵測到正確的目的，但每次交談都會傳回來自 conversation_start 節點的歡迎訊息 (`Welcome to the {{site.data.keyword.conversationshort}} example!`)。

發生此情況是因為 {{site.data.keyword.conversationshort}} 服務為無狀態；應用程式需負責維護狀態資訊。因為我們尚未執行任何動作來維護狀態，所以 {{site.data.keyword.conversationshort}} 服務會將每一輪的使用者輸入都視為新交談的第一次，而觸發 conversation_start 節點。

## 維護狀態

交談的狀態資訊是使用*環境定義* 進行維護。環境定義是在應用程式與 {{site.data.keyword.conversationshort}} 服務之間來回傳遞的 JSON 物件。應用程式需負責維護從某次交談到下一次交談的環境定義。

環境定義包含每一個使用者交談的唯一 ID，以及隨著每一次交談而遞增的計數器。舊版範例不會保留環境定義，這表示每一輪的輸入都會看似是新交談的開始。修正這種情況的方法是儲存環境定義，並且每一次都將它送回至 {{site.data.keyword.conversationshort}} 服務。

除了在交談中維護我們的位置之外，環境定義還可以用來儲存您要在應用程式與 {{site.data.keyword.conversationshort}} 服務之間來回傳遞的任何其他資料。這可以包含您要在整個交談中維護的持續性資料（例如客戶的名稱或帳號），或是您要追蹤的任何其他資料（例如選項設定的現行狀態）。

```javascript
// Example 3: maintains state.

var prompt = require('prompt-sync')();
var ConversationV1 = require('watson-developer-cloud/conversation/v1');

// Set up Conversation service wrapper.
var conversation = new ConversationV1({
  username: 'USERNAME', // replace with service username
  password: 'PASSWORD', // replace with service password
  version_date: '2017-05-26'
});

var workspace_id = 'WORKSPACE_ID'; // replace with workspace ID

// Start conversation with empty message.
conversation.message({
  workspace_id: workspace_id
  }, processResponse);

// Process the conversation response.
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
    conversation.message({
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

# Set up Conversation service.
conversation = watson_developer_cloud.ConversationV1(
  username = 'USERNAME', # replace with username from service key
  password = 'PASSWORD', # replace with password from service key
  version = '2017-05-26'
)
workspace_id = 'WORKSPACE_ID' # replace with workspace ID

# Initialize with empty value to start the conversation.
user_input = ''
context = {}

# Main input/output loop
while True:

  # Send message to Conversation service.
  response = conversation.message(
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

對於先前範例的唯一變更是，在每一輪的交談中，我們現在會送回在前一輪收到的 `response.context` 物件：
{: javascript }

```javascript
    conversation.message({
      input: { text: newMessageFromUser },
      context : response.context,
    }, processResponse)
```
{: codeblock}
{: javascript }

對於先前範例的唯一變更是，我們現在會將從對話收到的環境定義儲存至稱為 `context` 的變數，而且我們會透過下一輪的使用者輸入將它送回：
{: python }

```python
  response = conversation.message(
    workspace_id = workspace_id,
    input = {
      'text': user_input
    },
    context = context
  )
```
{: codeblock }
{: python }

這可確保將環境定義從某一次維護到下一次，因此，{{site.data.keyword.conversationshort}} 服務不再認為每一次都是第一次：

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

我們現在已有所進展！{{site.data.keyword.conversationshort}} 服務會正確地辨識我們的目的，且對話會傳回每一個目的的正確輸出文字（如果提供的話）。

不過，不會發生任何其他情況。當我們詢問時間時，未收到任何回答；當我們說再見時，並未結束交談。這是因為那些目的需要應用程式採取其他動作。

## 實作應用程式動作

除了要向使用者顯示的輸出文字之外，我們的對話還會使用回應 JSON 中的 `output` 物件，根據偵測到的目的來發出應用程式需要執行動作的信號。

這些動作旗標是使用 `action` 內容進行傳送，而我們的對話將此內容定義為回應 JSON 的一部分。對話在判斷應用程式需要執行某項作業時，會將 `action` 值設為適當的值，這可以是 `display_time` 或 `end_conversation`。

請謹記，`output` 只是 JSON 物件，您可以在其中新增想要的任何有效內容。針對較複雜的應用程式，您可以使用具有多個 action 旗標的陣列。

但在我們的範例中，我們使用的是支援單一 action 旗標的簡單鍵值組。我們的應用程式碼需要檢查回應中的 `action` 內容值，然後執行任何指定的動作。（此版本也移除偵測到之目的的顯示，因為我們確定將會正確地識別這些目的。）

```javascript
// Example 4: implements app actions.

var prompt = require('prompt-sync')();
var ConversationV1 = require('watson-developer-cloud/conversation/v1');

// Set up Conversation service wrapper.
var conversation = new ConversationV1({
  username: 'USERNAME', // replace with service username
  password: 'PASSWORD', // replace with service password
  version_date: '2017-05-26'
});

var workspace_id = 'WORKSPACE_ID'; // replace with workspace ID

// Start conversation with empty message.
conversation.message({
  workspace_id: workspace_id
  }, processResponse);

// Process the conversation response.
function processResponse(err, response) {
  if (err) {
    console.error(err); // something went wrong
    return;
  }

  var endConversation = false;

  // Check for action flags.
  if (response.output.action === 'display_time') {
    // User asked what time it is, so we output the local system time.
    console.log('The current time is ' + new Date().toLocaleTimeString());
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
    conversation.message({
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

# Set up Conversation service.
conversation = watson_developer_cloud.ConversationV1(
  username = 'USERNAME', # replace with username from service key
  password = 'PASSWORD', # replace with password from service key
  version = '2017-05-26'
)
workspace_id = 'WORKSPACE_ID' # replace with workspace ID

# Initialize with empty value to start the conversation.
user_input = ''
context = {}
current_action = ''

# Main input/output loop
while current_action != 'end_conversation':

  # Send message to Conversation service.
  response = conversation.message(
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
    print('The current time is ' + time.strftime('%I:%M:%S %p'))
  # If we're not done, prompt for next round of input.
  if current_action != 'end_conversation':
    user_input = input('>> ')
```
{: codeblock}
{: python}

processResponse() 函數現在會檢查從 {{site.data.keyword.conversationshort}} 服務收到之 `output` 物件的 `action` 內容值。如果值是 `display_time` 或 `end_conversation`，應用程式會執行適當的動作。
{: javascript}

應用程式現在會檢查從 {{site.data.keyword.conversationshort}} 服務收到之 `output` 物件的 `action` 內容值。如果值是 `display_time`，則應用程式會執行適當的動作。如果值是 `end_conversation`，則應用程式會知道不要再提示輸入其他的使用者輸入，而且 `while` 迴圈會結束。
{: python}

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

當然，現實應用程式會使用更複雜的使用者介面（例如網路聊天視窗）。它會實作更複雜的動作，並可能與客戶資料庫或其他商務系統整合。但是，應用程式如何與 {{site.data.keyword.conversationshort}} 服務互動的基本原則仍會相同。

如需一些更複雜的範例，請查看「導覽」窗格中的「範例」應用程式。
