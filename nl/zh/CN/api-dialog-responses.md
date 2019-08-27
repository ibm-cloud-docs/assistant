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

# 实现响应
{: #api-dialog-responses}

对话节点可以使用包含文本、图像或交互式元素（例如，可单击选项）的响应来回应用户。如果要构建您自己的客户机应用程序，那么必须实现对话返回的所有响应类型的正确显示。（有关对话响应的更多信息，请参阅[响应](/docs/services/assistant?topic=assistant-dialog-overview#dialog-overview-responses)。）

## 响应输出格式
{: #api-dialog-responses-output}

缺省情况下，在从 `/message` API 返回的响应 JSON 中的 `output.generic` 对象中指定对话节点的响应。`generic` 对象包含由最多 5 个响应元素组成的数组，这些响应元素可用于任何通道。以下 JSON 示例显示了包含文本和图像的响应：

```json
{
  "output": {
    "generic":[
      {
        "response_type": "text",
        "text": "好，这是一张狗的照片。"
      },
      {
        "response_type": "image",
        "source": "http://example.com/dog.jpg"
      }
    ],
    "text" : ["好，这是一张狗的照片。"]
  }
}
```

如本示例所示，文本响应（`好，这是一张狗的照片。`）还会在 `output.text` 数组中返回。包含此响应是为了向后兼容不支持 `output.generic` 格式的旧应用程序。
{: note}

客户机应用程序负责适当地处理所有响应类型。在本例中，应用程序需要向用户显示指定的文本和图像。

## 响应类型
{: #api-dialog-responses-types}

响应的每个元素都是其中一个受支持的响应类型（目前为 `image`、`option`、`pause`、`text` 和 `suggestion`）。每种响应类型都是使用一组不同的 JSON 属性进行指定，因此为每个响应包含的属性将根据响应类型而变化。有关 `/message` API 的响应模型的完整信息，请参阅 [API 参考 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://{DomainName}/apidocs/assistant-v2#send-user-input-to-assistant){: new_window}。

此部分描述了可用的响应类型以及这些类型在 `/message` API 响应 JSON 中的表示方式。（如果使用的是 Watson SDK，那么可以使用为您的语言提供的界面来访问相同的对象。）

此部分中的示例显示了运行时从 `/message` API 返回的 JSON 数据的格式。请记住，这与用于定义对话节点内响应的 JSON 格式不同。有关更多信息，请参阅[使用 JSON 编辑器定义响应](/docs/services/assistant?topic=assistant-dialog-responses-json)。
{: note}

### 文本
{: #api-dialog-responses-text}

`text` 响应类型用于对话中的普通文本响应：

```json
{
  "output": {
    "generic":[
      {
        "response_type": "text",
        "text": "好，您要在下周一飞往波士顿。"
      }
    ]
  }
}
```

请注意，为了实现向后兼容性，相同的文本还会包含在对话响应的 `output.text` 数组中（如本页面开头的示例中所示）。

### 图像
{: #api-dialog-responses-image}

`image` 响应类型指示客户机应用程序显示图像，并可选择附带标题和描述：

```json
{
  "output": {
    "generic":[
      {
        "response_type": "image",
        "source": "http://example.com/image.jpg",
        "title": "图像示例",
        "description": "这是示例图像"
      }
    ]
  }
}
```

应用程序负责检索 `source` 属性指定的图像，并将其显示给用户。如果提供了可选的 `title` 和 `description`，那么应用程序可以用任何适当的方式显示图像（例如，在图像下方呈现标题，并将描述呈现为悬停文本）。

### 暂停
{: #api-dialog-responses-pause}

`pause` 响应类型指示应用程序先等待指定的时间间隔，然后再显示下一个响应：

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

对话可能会请求此暂停，以便有时间完成请求，或者仅仅是模拟人工座席可能在响应之间暂停的情况。暂停可以是不超过 10 秒的任何持续时间。

`pause` 响应通常与其他响应组合在一起发送。应用程序应该先暂停等待 `time` 属性指定的时间间隔（以毫秒为单位），然后再显示数组中的下一个响应。可选的 `typing` 属性请求客户机应用程序显示“用户正在输入”指示符（如果支持），以模拟人工座席。

### 选项
{: #api-dialog-responses-option}

`option` 响应类型指示客户机应用程序显示用户界面控件，以支持用户从选项列表中进行选择，然后根据所选的选项将输入发送回助手：

```json
{
  "output": {
    "generic":[
      {
        "response_type": "option",
        "title": "可用选项",
        "description": "请选择下列其中一个选项：",
        "preference": "button",
        "options": [
          {
            "label": "选项 1",
            "value": {
              "input": {
      "text": "选项 1"
              }
            }
          },
          {
            "label": "选项 2",
            "value": {
              "input": {
      "text": "选项 2"
              }
            }
          }
        ]
      }
    ]
  }
}
```

应用程序可以使用任何适用的用户界面控件（例如，一组按钮或一个下拉列表）来显示指定的选项。如果支持，可选的 `preference` 属性可指示应用程序应使用的首选控件类型（`button` 或 `dropdown`）。为了获得最佳用户体验，较好的做法是在选项不超过三个时，将选项显示为按钮；选项超过三个时，将选项显示为下拉列表。

对于每个选项，`label` 属性指定在 UI 控件中对于该选项应显示的标签文本。`value` 属性指定在用户选择相应的选项时应发送回助手的输入（使用 `/message` API）。

有关在简单客户机应用程序中实现 `option` 响应的示例，请参阅[示例：实现 option 响应](#api-dialog-option-example)。

### 建议
{: #api-dialog-responses-suggestion}

此功能仅可供增强版或高端套餐用户使用。
{: tip}

不清楚用户的意图时，消歧功能会使用 `suggestion` 响应类型来建议可能的匹配项。`suggestion` 响应包含 `suggestions` 数组，其中每个建议对应于一个可能的匹配对话节点：

```json
{
  "output": {
    "generic":[
      {
        "response_type": "suggestion",
        "title": "您的意思是：",
        "suggestions": [
          {
            "label": "我要一杯饮料。",
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
                "text": "我要点单"
              }
            },
  "output": {
    "text": [
                "我将给您拿一杯饮料。"
              ],
              "generic": [
                {
                  "response_type": "text",
                  "text": "我将给您拿一杯饮料。"
                }
              ],
              "nodes_visited_details": [
                {
                  "dialog_node": "node_1_1547675028546",
                  "title": "点饮料",
                  "user_label": "我要一杯饮料。",
                  "conditions": "#order_drink"
                }
              ]
            },
            "source_dialog_node": "root"
          },
          {
            "label": "我需要续杯。",
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
                "text": "我需要续杯"
              }
            },
  "output": {
    "text": [
                "我将给您续杯。"
              ],
              "generic": [
                {
                  "response_type": "text",
                  "text": "我将给您续杯。"
                }
              ],
              "nodes_visited_details": [
                {
                  "dialog_node": "node_2_1547675097178",
                  "title": "续杯",
                  "user_label": "我需要续杯。",
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

请注意，`suggestion` 响应的结构与 `option` 响应的结构非常相似。与选项一样，每个建议都包含 `label`（可向用户显示）和 `value`（用于指定在用户选择相应的建议时应发送回助手的输入）。要在应用程序中实现 `suggestion` 响应，可以使用您会用于 `option` 响应的相同方法。

有关消歧功能的更多信息，请参阅[消歧](/docs/services/assistant?topic=assistant-dialog-runtime#dialog-runtime-disambiguation)。

## 示例：实现 option 响应
{: #api-dialog-option-example}

为了说明客户机应用程序可以如何处理 option 响应（即提示用户从选项列表中进行选择），可以扩展[构建客户机应用程序](/docs/services/assistant?topic=assistant-api-client)中描述的客户机示例。这是一个简化的客户机应用程序，使用标准输入和输出来处理三个意向（发送问候、显示当前时间和退出应用程序）：

```
欢迎使用 {{site.data.keyword.conversationshort}} 示例！
>> hello
您好。
>> what time is it?
当前时间是 12:40:42。
>> goodbye
好的，再会。
```
{: screen}

如果要试用本主题中显示的示例代码，应该首先设置必需的工作空间，并获取所需的 API 详细信息。有关更多信息，请参阅[构建客户机应用程序](/docs/services/assistant?topic=assistant-api-client)。
{: note}

### 接收 option 响应

如果您希望向用户提供有限的选项列表，而不是解释自然语言输入，可以使用 `option` 响应。在您希望用户能够快速从一组明确的选项中进行选择的任何情况下，都可以使用 option 响应。

在简化的客户机应用程序中，将使用此功能从助手支持的操作列表（问候、显示时间和退出）中进行选择。除了先前显示的三个意向（`#hello`、`#time` 和 `#goodbye`）之外，示例工作空间还支持第四个意向：`#menu`，当用户要求查看可用操作列表时，即与此意向相匹配。

工作空间识别到 `#menu` 意向时，对话会使用 `option` 响应进行响应：

```json
{
  "output": {
    "generic":[
      {
        "title": "您想做什么？",
        "options": [
          {
            "label": "发送问候",
            "value": {
              "input": {
      "text": "您好"
              }
            }
          },
          {
            "label": "显示本地时间",
            "value": {
              "input": {
      "text": "时间"
              }
            }
          },
          {
            "label": "退出",
      "value": {
        "input": {
      "text": "再见"
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

`option` 响应包含多个要向用户显示的选项。每个选项都包含两个对象：`label` 和 `value`。`label` 是用于标识选项的面向用户的字符串；`value` 指定在用户选择该选项时应发送回助手的相应消息输入。

客户机应用程序需要使用此响应中的数据来构建向用户显示的输出以及向助手发送相应的消息。

### 列出可用选项

处理 option 响应的第一步是使用每个选项的 `label` 属性指定的文本向用户显示选项。可以使用应用程序支持的任何方法来显示选项，通常是下拉列表或一组可单击的按钮。（如果指定了 option 响应的可选 `preference` 属性，那么表示如果可能，应用程序应该显示的显示类型。）

简化的示例使用标准输入和输出，因此我们无权访问真正的 UI。我们会改为仅将选项显示为编号列表。

```javascript
// 选项示例 1：列出选项。

const prompt = require('prompt-sync')();
const AssistantV2 = require('ibm-watson/assistant/v2');

// 设置 Assistant 服务包装器。
const service = new AssistantV2({
  iam_apikey: '{apikey}', // 替换为 API 密钥
  version: '2019-02-28',
});

const assistantId = '{assistant_id}'; // 替换为助手标识
let sessionId;

// 创建会话。
service
  .createSession({
    assistant_id: assistantId,
  })
  .then(res => {
    sessionId = res.session_id;
    sendMessage({text: ''}); // 以空消息开始会话
  })
  .catch(err => {
    console.log(err); // 出错了
  });

// 向助手发送消息。
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
      console.log(err); // 出错了
    });
}

// 处理响应。
function processResponse(response) {

  let endConversation = false;

  // 检查助手请求的客户机操作。
  if (response.output.actions) {
    if (response.output.actions[0].type === 'client'){
      if (response.output.actions[0].name === 'display_time') {
        // 用户询问现在几点，所以我们输出本地系统时间。
    console.log('The current time is ' + new Date().toLocaleTimeString() + '.');
      } else if (response.output.actions[0].name === 'end_conversation') {
        // 用户说再见，所以我们结束。
    console.log(response.output.generic[0].text);
        endConversation = true;
      }
    }
  } else {
    // 显示助手的输出（如有）。仅支持单个
    // 响应。
    if (response.output.generic) {
      if (response.output.generic.length > 0) {
        switch (response.output.generic[0].response_type) {
          case 'text':
            // 这是文本响应，因此仅显示该文本。
            console.log(response.output.generic[0].text);
            break;
          case 'option':
            // 这是选项响应，因此需要向用户显示
            // 选项列表。
            console.log(response.output.generic[0].title);
            const options = response.output.generic[0].options;
            // 通过标签列出选项。
            for (let i = 0; i < options.length; i++) {
              console.log((i+1).toString() + '. ' + options[i].label);
            }
            break;
        }
      }
    }
  }

  // 如果未结束，那么提示进行下一轮输入。
  if (!endConversation) {
    const newMessageFromUser = prompt('>> ');
    newMessageInput = {
      message_type: 'text',
      text: newMessageFromUser
    }
    sendMessage(newMessageInput);
  } else {
    // 操作已完成，将删除会话。
    service
      .deleteSession({
        assistant_id: assistantId,
        session_id: sessionId,
      })
      .then(res => {
        return;
      })
      .catch(err => {
        console.log(err); // 出错了
      });
  }
}
```
{: codeblock}
{: javascript}

下面将进一步讨论用于输出来自助手的响应的代码。现在，应用程序不采用 `text` 响应，而改为支持 `text` 和 `option` 响应类型：

```javascript
// 显示助手的输出（如有）。仅支持单个
    // 响应。
    if (response.output.generic) {
      if (response.output.generic.length > 0) {
        switch (response.output.generic[0].response_type) {
          case 'text':
            // 这是文本响应，因此仅显示该文本。
            console.log(response.output.generic[0].text);
            break;
          case 'option':
            // 这是选项响应，因此需要向用户显示
            // 选项列表。
            console.log(response.output.generic[0].title);
            const options = response.output.generic[0].options;
            // 通过标签列出选项。
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

如果 `response_type`=`text`，那么将如前所述显示输出。但是，如果 `response_type`=`option`，那么必须执行更多工作。首先，显示 `title` 属性的值，该属性充当引入文本以引入选项列表；然后，列出选项，并使用 `label` 属性的值来标识每个选项。（实际应用程序会在下拉列表中显示这些标签，或者将其显示为可单击按钮上的标签。）

可以通过触发 `#menu` 意向来查看结果：

```
欢迎使用 Watson Assistant 示例！
>> 有哪些可用的操作？
您想做什么
1. 发送问候
2. 显示本地时间
3. 退出
>> 2
很抱歉，我不明白您在说什么。
>>
```
{: screen}

如您所见，应用程序现在通过列出可用选项来正确处理 `option` 响应。不过，我们尚未将用户的选择转化为有意义的输入。

### 选择选项

除了 `label` 之外，响应中的每个选项还包含 `value` 对象，该对象含有在用户选择相应选项时应该发送回助手的输入数据。`value.input` 对象等效于 `/message` API 的 `input` 属性，这意味着只能将此对象按原样返回给助手。

要在应用程序中执行此操作，在客户机收到 `option` 响应时，将设置新的 `promptOption` 标志。此标志为 true 时，表示我们知道要从 `value.input` 中获取下一轮输入，而不是接受来自用户的自然语言文本输入。（同样，我们并没有真正的用户界面，因此仅提示用户按编号从列表中选择一个有效的选项。）

```javascript
// 选项示例 2：发送回所选选项值。

const prompt = require('prompt-sync')();
const AssistantV2 = require('ibm-watson/assistant/v2');

// 设置 Assistant 服务包装器。
const service = new AssistantV2({
  iam_apikey: 'AZkSnK4b40UI5kLepHKxIKYpVcxeg0yPLbVVwGFW8kjM', // 替换为 API 密钥
  version: '2019-02-28',
});

const assistantId = 'dcd5c5ad-f3a1-4345-89c5-708b0b5ff4f7'; // 替换为助手标识
let sessionId;

// 创建会话。
service
  .createSession({
    assistant_id: assistantId,
  })
  .then(res => {
    sessionId = res.session_id;
    sendMessage({text: ''}); // 以空消息开始会话
  })
  .catch(err => {
    console.log(err); // 出错了
  });

// 向助手发送消息。
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
      console.log(err); // 出错了
    });
}

// 处理响应。
function processResponse(response) {

  let endConversation = false;
  let promptOption = false;

  // 检查助手请求的客户机操作。
  if (response.output.actions) {
    if (response.output.actions[0].type === 'client'){
      if (response.output.actions[0].name === 'display_time') {
        // 用户询问现在几点，所以我们输出本地系统时间。
    console.log('The current time is ' + new Date().toLocaleTimeString() + '.');
      } else if (response.output.actions[0].name === 'end_conversation') {
        // 用户说再见，所以我们结束。
    console.log(response.output.generic[0].text);
        endConversation = true;
      }
    }
  } else {
    // 显示助手的输出（如有）。仅支持单个
    // 响应。
    if (response.output.generic) {
      if (response.output.generic.length > 0) {
        switch (response.output.generic[0].response_type) {
          case 'text':
            // 这是文本响应，因此仅显示该文本。
            console.log(response.output.generic[0].text);
            break;
          case 'option':
            // 这是选项响应，因此需要向用户显示
            // 选项列表。
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

  // 如果未结束，那么提示进行下一轮输入。
  if (!endConversation) {
    let messageInput;
    if (promptOption == true) {
      // 提示从选项列表中选择有效的选项。
      let choice;
      do {
        choice = prompt('? ');
          if (isNaN(choice)) {
            choice = 0;
          }
      } while (choice < 1 || choice > response.output.generic[0].options.length);
      const value = response.output.generic[0].options[choice-1].value;
      // 使用所选选项中的消息输入。
      messageInput = value.input;
    } else {
      // 我们没有显示选项，因此仅提示提供
      // 下一轮输入。
      const newText = prompt('>> ');
      messageInput = {
        text: newText
      }
    }
    sendMessage(messageInput); 
  } else {
    // 操作已完成，将删除会话。
    service
      .deleteSession({
        assistant_id: assistantId,
        session_id: sessionId,
      })
      .then(res => {
        return;
      })
      .catch(err => {
        console.log(err); // 出错了
      });
  }
}
```
{: codeblock}
{: javascript}

我们只需将所选响应中的 `value.input` 对象用作下一轮消息输入，而不是使用文本输入来构建新的 `input` 对象。随后，助手会像用户直接输入了输入文本那样进行响应。

```
欢迎使用 Watson Assistant 示例！
>> 嗨
您好。
>> 有哪些选项？
您想做什么
1. 发送问候
2. 显示本地时间
3. 退出
? 2
当前时间是下午 1:29:14。
>> 再见
好，再会。
```
{: screen}

现在可以通过发出自然语言请求或从选项菜单中进行选择来访问助手的所有功能。

请注意，对于 `suggestion` 响应，也可使用相同的方法。如果套餐支持消歧功能，那么在不清楚可能的选项中哪个是正确的情况下，可以使用类似的逻辑提示用户从列表中进行选择。有关消歧功能的更多信息，请参阅[消歧](/docs/services/assistant?topic=assistant-dialog-runtime#dialog-runtime-disambiguation)。
