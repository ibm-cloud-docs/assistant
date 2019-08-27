---

copyright:
  years: 2015, 2019
lastupdated: "2019-05-14"

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

# 實作回應
{: #api-dialog-responses}

對話節點可以回應使用者，該回應中包括文字、影像或互動式元素（例如，可按式選項）。如果您要建置自己的用戶端應用程式，則必須實作對話所傳回之所有回應類型的正確顯示。（如需對話回應的相關資訊，請參閱[回應](/docs/services/assistant?topic=assistant-dialog-overview#dialog-overview-responses)。）

## 回應輸出格式
{: #api-dialog-responses-output}

依預設，來自對話節點的回應指定於 `output.generic` 物件，該物件位於從 `/message` API 傳回的回應 JSON 中。`generic` 物件包含最多有 5 個回應元素的陣列，而這些元素適用於任何頻道。下列 JSON 範例顯示一個包含文字及影像的回應：

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

如此範例所示，`output.text` 陣列中也會傳回文字回應 (`OK, here's a picture of a dog.`)。對於不支援 `output.generic` 格式的舊版應用程式，舊版相容性會包含此項。
{: note}

用戶端應用程式有責任適當地處理所有回應類型。在此情況下，您的應用程式將需要向使用者顯示指定的文字及影像。

## 回應類型
{: #api-dialog-responses-types}

回應的每個元素都是其中一個受支援的回應類型（目前為 `image`、`option`、`pause`、`text` 和 `suggestion`）。每一種回應類型都使用一組不同的 JSON 內容來指定，因此，每一個回應所包含的內容會根據回應類型而有所不同。如需 `/message` API 回應模型的完整資訊，請參閱 [API 參考資料 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://{DomainName}/apidocs/assistant-v2#send-user-input-to-assistant){: new_window}。

本節說明可用的回應類型，以及它們在 `/message` API 回應 JSON 中的呈現方式。（如果您使用 Watson SDK，則可以使用針對您的語言所提供的介面來存取相同的物件。）

本節中的範例顯示在運行環境從 `/message` API 所傳回的 JSON 資料的格式。請牢記，這與用來在對話節點內定義回應的 JSON 格式不同。如需相關資訊，請參閱[使用 JSON 編輯器定義回應](/docs/services/assistant?topic=assistant-dialog-responses-json)。
{: note}

### 文字
{: #api-dialog-responses-text}

`text` 回應類型用於來自對話的一般文字回應：

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

請注意，為了相容於舊版，相同的文字也會包含在對話回應的 `output.text` 陣列中（如這個頁面開頭的範例所示）。

### 影像
{: #api-dialog-responses-image}

`image` 回應類型指示用戶端應用程式顯示影像，並選擇性地隨附標題和說明：

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

您的應用程式負責擷取 `source` 內容所指定的影像，並將它顯示給使用者。如果提供了選用的 `title` 和 `description`，則您的應用程式可以依照任何適當的方式來顯示它們（例如，將影像下的標題和說明呈現為浮動說明）。

### 暫停
{: #api-dialog-responses-pause}

`pause` 回應類型指示應用程式在顯示下一個回應之前等待指定的時間間隔：

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

此暫停可能是對話所要求的，以容許有時間完成要求，或者只是模擬真人服務專員可能在回應之間暫停的現象。暫停時間可長達 10 秒。

通常，`pause` 回應會結合其他回應一起傳送。在顯示陣列中的下一個回應之前，您的應用程式應暫停 `time` 內容所指定的間隔（毫秒）。選用的 `typing` 內容會要求用戶端應用程式顯示「使用者正在鍵入」指示器（如果支援的話），以模擬真人服務專員。

### 選項
{: #api-dialog-responses-option}

`option` 回應類型指示用戶端應用程式顯示使用者介面控制項，讓使用者能夠從選項清單中進行選取，然後根據選取的選項，將輸入送回給助理：

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

您的應用程式可以使用任何適當的使用者介面控制項（例如，一組按鈕或下拉清單）來顯示指定的選項。選用的 `preference` 內容指出應用程式應使用的偏好控制項類型（`button` 或 `dropdown`）（如果支援的話）。為了獲得最佳使用者體驗，較好的作法是在選項不超過三個時，將選項顯示為按鈕；選項超過三個時，將選項顯示為下拉清單。

對於每個選項，`label` 內容指定應該會針對使用者介面控制項中的選項而出現的標籤文字。`value` 內容指定當使用者選取對應的選項時，應該送回給助理的輸入（使用 `/message` API）。

如需在簡單用戶端應用程式中實作 `option` 回應的範例，請參閱[範例：實作 option 回應](#api-dialog-option-example)。

### 建議
{: #api-dialog-responses-suggestion}

此特性僅適用於「加值」或「超值」方案使用者。
{: tip}

不清楚使用者的意圖時，澄清特性會使用 `suggestion` 回應類型來建議可能的相符項。`suggestion` 回應包含 `suggestions` 陣列，其中每個建議都對應於一個可能的相符對話節點：

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

請注意，`suggestion` 回應的結構與 `option` 回應的結構非常相似。與選項一樣，每個建議都包含可向使用者顯示的 `label` 以及 `value`，而此值指定在使用者選擇對應建議時應該送回給助理的輸入。若要在應用程式中實作 `suggestion` 回應，您可以使用將用於 `option` 回應的相同方法。

如需澄清特性的相關資訊，請參閱[澄清](/docs/services/assistant?topic=assistant-dialog-runtime#dialog-runtime-disambiguation)。

## 範例：實作 option 回應
{: #api-dialog-option-example}

為了說明用戶端應用程式可以如何處理 option 回應（即提示使用者從選項清單中進行選取），可以延伸[建置用戶端應用程式](/docs/services/assistant?topic=assistant-api-client)中說明的用戶端範例。這是一個簡化的用戶端應用程式，使用標準輸入和輸出來處理三個目的（傳送問候語、顯示現行時間和結束應用程式）：

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

如果您要嘗試本主題中顯示的程式碼範例，應該先設定必要的工作區，並取得所需的 API 詳細資料。如需相關資訊，請參閱[建置用戶端應用程式](/docs/services/assistant?topic=assistant-api-client)。
{: note}

### 接收 option 回應

如果您要向使用者提供有限的選項清單，而不是解譯自然語言輸入，可以使用 `option` 回應。這可以用於您要使用者能夠快速從一組明確的選項中進行選取的任何狀況。

在簡化用戶端應用程式中，將使用此功能從助理支援的動作清單（問候語、顯示時間和結束）中進行選取。除了先前顯示的三個目的（`#hello`、`#time` 和 `#goodbye`）之外，範例工作區還支援第四個目的：`#menu`，當用戶要求查看可用動作清單時，即與此目的相符。

工作區辨識到 `#menu` 目的時，對話會使用 `option` 回應進行回應：

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

`option` 回應包含多個要向使用者顯示的選項。每個選項都包含兩個物件：`label` 和 `value`。`label` 是用於識別選項的面向使用者字串；`value` 指定在使用者選擇該選項時應該送回給助理的對應訊息輸入。

用戶端應用程式需要使用此回應中的資料來建置向使用者顯示的輸出以及將適當的訊息傳送給助理。

### 列出可用選項

處理 option 回應的第一步是使用每個選項的 `label` 內容指定的文字向使用者顯示選項。您可以使用應用程式支援的任何方法來顯示選項，通常是下拉清單或一組可按一下的按鈕（如果指定 option 回應的選用 `preference` 內容，則指出可能的話，應用程式應該顯示的顯示類型）。

簡化的範例使用標準輸入和輸出，因此我們無權存取實際使用者介面。我們會改為僅將選項顯示為編號清單。

```javascript
// Option example 1: lists options.

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
    sendMessage({text: ''}); // start conversation with empty message
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
      input: messageInput,
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
      message_type: 'text',
      text: newMessageFromUser
    }
    sendMessage(newMessageInput);
  } else {
    // We're done, so we delete the session.
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

下面將進一步討論用於輸出助理回應的程式碼。現在，應用程式支援 `text` 和 `option` 回應類型，而非採用 `text` 回應：

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

如果 `response_type`=`text`，則將如前所述顯示輸出。但是，如果 `response_type`=`option`，則必須執行更多工作。首先，顯示 `title` 內容的值，該內容充當引入文字以引進選項清單；然後，列出選項，並使用 `label` 內容的值來識別每個選項（實際應用程式會在下拉清單中顯示這些標籤，或者將其顯示為可按一下按鈕上的標籤）。

您可以觸發 `#menu` 目的來查看結果：

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

如您所見，應用程式現在藉由列出可用選項來正確處理 `option` 回應。不過，我們尚未將使用者的選擇轉換為有意義的輸入。

### 選取選項

除了 `label` 之外，回應中的每個選項還包含 `value` 物件，該物件包含在使用者選擇對應選項時應該傳回給助理的輸入資料。`value.input` 物件相當於 `/message` API 的 `input` 內容，這表示只能將此物件依現狀送回給助理。

若要在應用程式中執行此作業，在用戶端收到 `option` 回應時，將設定新的 `promptOption` 旗標。此旗標為 true 時，表示我們知道要從 `value.input` 中取得下一輪輸入，而不是接受來自使用者的自然語言文字輸入（同樣地，我們並沒有實際使用者介面，因此只會提示使用者依編號從清單中選取一個有效的選項）。

```javascript
// Option example 2: sends back selected option value.

const prompt = require('prompt-sync')();
const AssistantV2 = require('ibm-watson/assistant/v2');

// Set up Assistant service wrapper.
const service = new AssistantV2({
  iam_apikey: 'AZkSnK4b40UI5kLepHKxIKYpVcxeg0yPLbVVwGFW8kjM', // replace with API key
  version: '2019-02-28',
});

const assistantId = 'dcd5c5ad-f3a1-4345-89c5-708b0b5ff4f7'; // replace with assistant ID
let sessionId;

// Create session.
service
  .createSession({
    assistant_id: assistantId,
  })
  .then(res => {
    sessionId = res.session_id;
    sendMessage({text: ''}); // start conversation with empty message
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
      input: messageInput,
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
        text: newText
      }
    }
    sendMessage(messageInput); 
  } else {
    // We're done, so we delete the session.
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

我們只需將所選取回應中的 `value.input` 物件用作下一輪訊息輸入，而不是使用文字輸入來建置新的 `input` 物件。隨後，助理會像使用者直接鍵入輸入文字那樣地進行回應。

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

我們現在可以藉由進行自然語言要求或從選項功能表中進行選取來存取助理的所有功能。

請注意，對於 `suggestion` 回應，也可使用相同的方法。如果方案支援澄清特性，則在不清楚可能的選項中哪個是正確的情況下，您可以使用類似的邏輯提示使用者從清單中進行選取。如需澄清特性的相關資訊，請參閱[澄清](/docs/services/assistant?topic=assistant-dialog-runtime#dialog-runtime-disambiguation)。
