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

# 응답 구현
{: #api-dialog-responses}

대화 노드는 텍스트, 이미지 또는 대화식 요소(예: 클릭 가능한 옵션)를 포함하는 응답으로 사용자에게 응답할 수 있습니다. 고유 클라이언트 애플리케이션을 빌드하는 경우 대화에서 리턴된 모든 응답 유형의 올바른 표시를 구현해야 합니다. (대화 응답에 대한 자세한 정보는 [응답](/docs/services/assistant?topic=assistant-dialog-overview#dialog-overview-responses)을 참조하십시오.)

## 응답 출력 형식
{: #api-dialog-responses-output}

기본적으로 대화 노드의 응답은 `/message` API에서 리턴된 응답 JSON의 `output.generic` 오브젝트에 지정됩니다. `generic` 오브젝트에는 모든 채널에 사용할 수 있는 최대 5개의 응답 요소로 이루어진 배열이 포함되어 있습니다. 다음 JSON 예제는 텍스트 및 이미지를 포함하는 응답을 보여줍니다.

```json
{
  "output": {
    "generic":[
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

이 예제에 표시된 대로 텍스트 응답(`OK, here's a picture of a dog.`)이 `output.text` 배열에도 리턴됩니다. 이는 `output.generic` 형식을 지원하지 않는 이전 애플리케이션의 이전 버전과의 호환성을 위해 포함됩니다.
{: note}

모든 응답 유형을 적절히 처리하는 것은 클라이언트 애플리케이션의 책임입니다. 이 경우 애플리케이션이 사용자에게 지정된 텍스트 및 이미지를 표시해야 할 수 있습니다.

## 응답 유형
{: #api-dialog-responses-types}

응답의 각 요소는 지원되는 응답 유형(현재 `image`, `option`, `pause`, `text` 및 `suggestion`) 중 하나입니다. 각 응답 유형은 다른 JSON 특성 세트를 사용하여 지정되므로 각 응답에 대해 포함된 특성은 응답 유형에 따라 다릅니다. `/message` API의 응답 모델에 대한 전체 정보는 [API 참조 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://{DomainName}/apidocs/assistant-v2#send-user-input-to-assistant){: new_window}를 참조하십시오.

이 섹션에서는 사용 가능한 응답 유형과 이러한 응답 유형이 `/message` API 응답 JSON에 표시되는 방법에 대해 설명합니다. (Watson SDK를 사용 중인 경우 사용자 언어에 제공된 인터페이스를 사용하여 동일한 오브젝트에 액세스할 수 있습니다.)

이 섹션의 예제는 런타임 시 `/message API`에서 리턴된 JSON 데이터의 형식을 보여줍니다. 이 형식은 대화 노드 내에서 응답을 정의하는 데 사용되는 JSON 형식과 다르다는 점을 기억하십시오. 자세한 정보는 [JSON 편집기를 사용하여 응답 정의](/docs/services/assistant?topic=assistant-dialog-responses-json)를 참조하십시오.
{: note}

### 텍스트
{: #api-dialog-responses-text}

`text` 응답 유형은 대화의 일반 텍스트 응답에 사용됩니다.

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

이전 버전과의 호환성을 위해 동일한 텍스트가 대화 응답의 `output.text` 배열(이 페이지의 시작 부분에 있는 예제에 표시됨)에도 포함됩니다.

### 이미지
{: #api-dialog-responses-image}

`image` 응답 유형은 선택적으로 제목과 설명이 수반된 이미지를 표시하도록 클라이언트 애플리케이션에 지시합니다.

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

애플리케이션은 `source` 특성에 지정된 이미지를 검색하여 사용자에게 표시하는 작업을 담당합니다. 선택적 `title` 및 `description`이 제공되는 경우 애플리케이션이 이러한 특성을 적절한 방식으로 표시할 수 있습니다(예: 이미지 아래에 제목을 렌더링하고 설명을 풍선 텍스트로 렌더링).

### 일시정지
{: #api-dialog-responses-pause}

`pause` 응답 유형은 다음 응답을 표시하기 전에 지정된 간격 동안 대기하도록 애플리케이션에 지시합니다.

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

이 일시중지는 요청이 완료될 때까지 기다리거나 단순히 응답 사이에 일시중지할 수 있는 휴먼 에이전트의 모양을 가장하기 위해 대화에서 요청될 수 있습니다. 일시중지 기간은 최대 10초까지 가능합니다.

`pause` 응답은 일반적으로 다른 응답과 결합하여 전송됩니다. 애플리케이션이 배열에 있는 다음 응답을 표시하기 전에 `time` 특성(밀리초 단위)에 지정된 간격 동안 일시정지해야 합니다. 선택적 `typing` 특성은 클라이언트 애플리케이션이 휴먼 에이전트를 시뮬레이션하기 위해 "사용자가 입력 중" 표시기를 표시하도록 요청합니다(지원되는 경우).

### 옵션
{: #api-dialog-responses-option}

`option` 응답 유형은 사용자가 옵션 목록에서 선택할 수 있도록 하는 사용자 인터페이스 제어를 표시하도록 클라이언트 애플리케이션에 지시한 후 선택한 옵션에 따라 어시스턴트에 입력을 반송합니다.

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

앱은 적합한 사용자 인터페이스 제어(예: 단추 세트 또는 드롭 다운 목록)를 사용하여 지정된 옵션을 표시할 수 있습니다. 선택적 `preference` 특성은 지원되는 경우 앱에서 사용해야 하는 기본 제어 유형(`button` 또는 `dropdown`)을 표시합니다. 최적의 사용자 경험을 위해서는 두 세 개의 옵션을 단추로 제공하고 4개 이상의 옵션을 드롭 다운 목록으로 제공하는 것이 좋습니다. 

각 옵션에서 `label` 특성은 UI 제어의 옵션에 대해 표시되어야 하는 레이블 텍스트를 지정합니다. `value` 특성은 사용자가 해당 옵션을 선택할 때 `/message` API를 사용하여 어시스턴트에 반송되어야 하는 입력을 지정합니다.

단순 클라이언트 애플리케이션에서 `option` 응답 구현의 예제는 [예제: 옵션 응답 구현](#api-dialog-option-example)을 참조하십시오. 

### 제안사항
{: #api-dialog-responses-suggestion}

이 기능은 Plus 또는 Premium 플랜 사용자만 사용할 수 있습니다.
{: tip}

`suggestion` 응답 유형은 사용자가 원하는 작업이 무엇인지 분명하지 않을 때 가능한 일치를 제안하기 위해 모호성 제거 기능이 사용합니다. `suggestion` 응답은 `suggestions` 배열을 포함하며 이러한 배열 각각은 가능한 일치 대화 노드에 해당합니다. 

```json
{
  "output": {
    "generic":[
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

`suggestion` 응답의 구조는 `option` 응답의 구조와 매우 유사합니다. 옵션과 마찬가지로, 각각의 제안은 사용자에게 표시될 수 있는 `label` 및 사용자가 대응하는 제안을 선택하는 경우 어시스턴트에 다시 전송되어야 하는 입력을 지정하는 `value`를 포함합니다. 애플리케이션에서 `suggestion` 응답을 구현하기 위해 `option` 응답에 사용하는 것과 동일한 접근 방법을 사용할 수 있습니다. 

모호성 제거 기능에 대한 자세한 정보는 [모호성 제거](/docs/services/assistant?topic=assistant-dialog-runtime#dialog-runtime-disambiguation)를 참조하십시오.

## 예: 옵션 응답 구현
{: #api-dialog-option-example}

사용자가 선택 목록에서 선택할 수 있도록 프롬프트로 표시되는 옵션 응답을 클라이언트 애플리케이션이 처리하는 방법을 보여주기 위해 [클라이언트 애플리케이션 빌드](/docs/services/assistant?topic=assistant-api-client)에서 설명한 클라이언트 예제를 확장할 수 있습니다. 이는 표준 입력 및 출력을 사용하여 3개의 인텐트(인사 보내기, 현재 시간 표시 및 앱 종료)를 처리하는 단순한 클라이언트 앱입니다. 

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

이 주제에 표시된 예제 코드를 시도하려면 먼저 필수 작업공간을 설정하고 필요한 API 세부사항을 가져와야 합니다. 자세한 정보는 [클라이언트 애플리케이션 빌드](/docs/services/assistant?topic=assistant-api-client)를 참조하십시오.
{: note}

### 옵션 응답 수신

`option` 응답은 자연어 입력을 해석하는 것이 아니라 한정된 선택 목록을 사용자에게 제공하려고 할 때 사용할 수 있습니다. 사용자가 모호하지 않은 옵션 세트에서 신속하게 선택할 수 있도록 하려는 모든 상황에서 사용할 수 있습니다.

이 단순한 클라이언트 앱에서는 이러한 기능을 사용하여 어시스턴트가 지원하는 조치(인사말, 시간 표시 및 종료)의 목록에서 선택합니다. 이전에 제시된 3개의 인텐트(`#hello`, `#time`및 `#goodbye)`에 추가로, 예제 작업공간은 사용자가 사용 가능한 조치의 목록을 보고자할 때 일치되는 4번째 인텐트인 `#menu`를 지원합니다. 

작업공간이 `#menu` 인텐트를 인식하면, 대화는 `option` 응답으로 답변합니다.

```json
{
  "output": {
    "generic":[
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

`option` 응답에는 사용자에게 표시되는 여러 옵션이 포함되어 있습니다. 각 옵션은 두 오브젝트 `label` 및 `value`를 포함합니다. `label`은 옵션을 식별하는 사용자 대면 문자열이며, `value`는 사용자가 옵션을 선택하는 경우 어시스턴트로 다시 보내야 하는 해당 메시지를 지정합니다. 

이 클라이언트 앱에서는 이 응답의 데이터를 사용하여 사용자에게 표시하는 출력을 빌드하고 적합한 메시지를 어시스턴트에 전송합니다. 

### 사용 가능한 옵션 나열

옵션 응답을 처리하는 첫 번째 단계는 각 옵션의 `label` 특성에 지정된 텍스트를 사용하여 사용자에게 옵션을 표시하는 것입니다. 애플리케이션이 지원하는 기술, 일반적으로 드롭 다운 목록 또는 클릭 가능한 단추 세트를 사용하여 옵션을 표시할 수 있습니다. (지정된 경우 옵션 응답의 선택적 `preference` 특성은 가능한 경우 애플리케이션이 표시해야 하는 표시 유형을 나타냅니다.)

이 단순 예제에서는 표준 입력 및 출력을 사용하므로 실제 UI에 액세스하지 않습니다. 대신에 옵션을 번호가 매겨진 목록으로 제공합니다. 

```javascript
// Option example 1: lists options.

const prompt = require('prompt-sync')();
const AssistantV2 = require('ibm-watson/assistant/v2');

// 어시스턴트 서비스 랩퍼를 설정합니다.
const service = new AssistantV2({
  iam_apikey: '{apikey}', // replace with API key
  version: '2019-02-28',
});

const assistantId = '{assistant_id}'; // replace with assistant ID
let sessionId;

// 세션을 작성합니다.
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

// 어시스턴트로 메시지를 발송합니다.
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

// 응답을 처리합니다.
function processResponse(response) {

  let endConversation = false;

  // 어시스턴트에 의해 요청된 클라이언트 조치를 확인합니다.
  if (response.output.actions) {
    if (response.output.actions[0].type === 'client'){
      if (response.output.actions[0].name === 'display_time') {
        // 사용자가 몇 시인지 물어서 로컬 시스템 시간을 출력합니다.
        console.log('The current time is ' + new Date().toLocaleTimeString() + '.');
      } else if (response.output.actions[0].name === 'end_conversation') {
        // 사용자가 goodbye라고 말했으므로 이제 끝났습니다.
        console.log(response.output.generic[0].text);
        endConversation = true;
      }
    }
  } else {
    // 어시스턴트로부터의 출력을 표시합니다(있는 경우). 단일 응답만
    // 지원합니다.
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

  // 아직 끝나지 않은 경우 다음 라운드의 입력에 대한 프롬프트를 표시합니다.
  if (!endConversation) {
    const newMessageFromUser = prompt('>> ');
    newMessageInput = {
      message_type: 'text',
      text: newMessageFromUser
    }
    sendMessage(newMessageInput);
  } else {
    // 이제 끝났으므로 세션을 삭제합니다.
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

어시스턴트에서 응답을 출력하는 코드를 자세히 살펴보겠습니다. 이제, 애플리케이션은 `text` 응답을 가정하는 대신, `text`와 `option`을 모두 지원합니다. 

```javascript
    // 어시스턴트로부터의 출력을 표시합니다(있는 경우). 단일 응답만
    // 지원합니다.
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

`response_type`=`text`인 경우 출력을 이전과 같이 표시합니다. 그러나 `response_type`=`option`인 경우에는 좀 더 작업해야 합니다. 먼저 `title` 특성 값을 표시하는데 이 특성은 옵션 목록을 소개하는 도입 텍스트 역할을 합니다. 그런 다음 각 옵션을 식별하기 위해 `label` 특성 값을 사용하여 옵션을 나열합니다. (실제 애플리케이션은 드롭 다운 목록 또는 클릭 가능한 단추로 이러한 레이블을 표시합니다.)

`#menu` 인텐트를 트리거하여 결과를 확인할 수 있습니다. 

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

사용자가 볼 수 있는 것처럼, 이제 애플리케이션이 사용 가능한 선택사항을 나열하여 `option`을 올바르게 처리하고 있습니다. 그러나 사용자의 선택을 의미 있는 입력으로 아직 변환하지는 않고 있습니다. 

### 옵션 선택

`label`외에도, 응답의 각 옵션은 사용자가 해당 옵션을 선택하는 경우 어시스턴트로 다시 전송해야 하는 입력 데이터를 포함하는 `value` 오브젝트도 포함합니다. `value.input` 오브젝트는 `/message` API의 `input` 특성과 동일하며, 이 오브젝트를 어시스턴트에게 있는 그대로 전송할 수 있음을 의미합니다. 

이 애플리케이션에서는 이를 위해 클라이언트가 `opiton` 응답을 수신할 때 새로운 `promptOption` 플래그를 설정합니다. 이 플래그가 true이면, 사용자로부터 자연어 텍스트 입력을 받기보다는 `value.input`에서 다음 입력을 가져오려고 하는 것입니다. (다시, 실제 사용자 인터페이스가 없으므로 사용자가 목록에서 올바른 옵션을 숫자로 선택하도록 프롬프트만 표시합니다.)

```javascript
// Option example 2: sends back selected option value.

const prompt = require('prompt-sync')();
const AssistantV2 = require('ibm-watson/assistant/v2');

// 어시스턴트 서비스 랩퍼를 설정합니다.
const service = new AssistantV2({
  iam_apikey: 'AZkSnK4b40UI5kLepHKxIKYpVcxeg0yPLbVVwGFW8kjM', // replace with API key
  version: '2019-02-28',
});

const assistantId = 'dcd5c5ad-f3a1-4345-89c5-708b0b5ff4f7'; // replace with assistant ID
let sessionId;

// 세션을 작성합니다.
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

// 어시스턴트로 메시지를 발송합니다.
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

// 응답을 처리합니다.
function processResponse(response) {

  let endConversation = false;
  let promptOption = false;

  // 어시스턴트에 의해 요청된 클라이언트 조치를 확인합니다.
  if (response.output.actions) {
    if (response.output.actions[0].type === 'client'){
      if (response.output.actions[0].name === 'display_time') {
        // 사용자가 몇 시인지 물어서 로컬 시스템 시간을 출력합니다.
        console.log('The current time is ' + new Date().toLocaleTimeString() + '.');
      } else if (response.output.actions[0].name === 'end_conversation') {
        // 사용자가 goodbye라고 말했으므로 이제 끝났습니다.
        console.log(response.output.generic[0].text);
        endConversation = true;
      }
    }
  } else {
    // 어시스턴트로부터의 출력을 표시합니다(있는 경우). 단일 응답만
    // 지원합니다.
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

  // 아직 끝나지 않은 경우 다음 라운드의 입력에 대한 프롬프트를 표시합니다.
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
    // 이제 끝났으므로 세션을 삭제합니다.
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

텍스트 입력을 사용하여 새로운 `input` 오브젝트를 빌드하는 것이 아니라 선택된 응답에서 다음 메시지 입력으로 `value.input` 오브젝트를 사용하기만 하면 됩니다. 그러면 어시스턴트가 사용자가 입력 텍스트를 직접 입력한 것과 동일하게 응답합니다. 

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

이제 자연어 요청을 작성하거나 옵션 메뉴에서 선택하여 어시스턴트의 모든 기능에 액세스할 수 있습니다.

동일한 접근 방법이 또한 `suggestion` 응답에도 사용된다는 점에 유의하십시오. 플랜에서 모호성 제거 기능을 지원하는 경우 유사한 로직을 사용하여 여러 가능한 옵션 중 어느 것이 올바른지 분명하지 않은 경우 목록에서 선택할 수 있도록 프롬프트를 표시할 수 있습니다. 모호성 제거 기능에 대한 자세한 정보는 [모호성 제거](/docs/services/assistant?topic=assistant-dialog-runtime#dialog-runtime-disambiguation)를 참조하십시오.
