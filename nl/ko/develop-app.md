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

# 클라이언트 애플리케이션 빌드

작업 대화 상자가 있습니다. 이제 사용자와 상호작용하고 {{site.data.keyword.conversationfull}} 서비스와 통신할 애플리케이션을 개발할 수 있습니다.
{: shortdesc}

오른족 위에 있는 언어 선택기를 클릭하여 Node.js (Javascript) 또는 Python에 대한 튜토리얼을 볼 수 있습니다. 지원되는 모든 언어에 관한 세부사항은 {{site.data.keyword.watson}} [SDKs ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](/docs/services/watson/getting-started-sdks.html#sdks){: new_window}를 참조하십시오.
{: tip }

## {{site.data.keyword.conversationshort}} 서비스 설정

이 섹션에서 작성할 애플리케이션 예제에서는 코그너티브 개인 비서의 여러 기능을 구현합니다. 애플리케이션 코드가 {{site.data.keyword.conversationshort}} 작업공간에 연결되며 이 작업공간에서 코그너티브 처리(예: 사용자 인텐트 발견)가 이루어집니다.

이 예제를 계속하기 전에 필수 {{site.data.keyword.conversationshort}} 작업공간을 설정해야 합니다.

1.  작업공간 <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/conversation/conversation-simple-example.json" download="conversation-simple-example.json">JSON 파일</a>을 다운로드하십시오.
1.  {{site.data.keyword.conversationshort}} 서비스의 인스턴스에 [작업공간을 가져오십시오](/docs/services/conversation/configure-workspace.html#creating-workspaces).

## 서비스 정보 가져오기

{{site.data.keyword.conversationshort}} 서비스 REST API에 액세스하려면 애플리케이션이 {{site.data.keyword.Bluemix}}에 인증하고 오른쪽 {{site.data.keyword.conversationshort}} 작업공간에 연결되어야 합니다. 서비스 신임 정보 및 작업공간 ID를 복사하고 애플리케이션 코드에 붙여넣어야 합니다.

작업공간에서 서비스 신임 정보 및 작업공간 ID에 액세스하려면 ![메뉴](images/Menu_16.png) 메뉴를 선택하고 **배치**를 선택한 다음 **신임 정보** 탭으로 이동하십시오.

또한, {{site.data.keyword.Bluemix_short}} 대시보드에서 서비스에 신임 정보에 액세스할 수 있습니다.

## {{site.data.keyword.conversationshort}} 서비스 와 통신

{{site.data.keyword.conversationshort}} 서비스와 통신하는 방법은 간단합니다. 서비스에 연결하고, 단일 메시지를 발송하고, 결과를 콘솔에 출력하는 예제를 확인하십시오.

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
# 예제 1: 서비스 랩퍼를 설정하고 초기 응답을 발송 및
# 응답을 수신합니다.

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

첫 번째 단계는 {{site.data.keyword.conversationshort}} 서비스의 랩퍼를 작성하는 것입니다.

랩퍼는 서비스로 입력을 보내고 서비스에서 출력을 받는 데 사용할 오브젝트입니다. 서비스 랩퍼를 작성할 때 서비스 키의 인증 신임 정보 및 사용 중인 {{site.data.keyword.conversationshort}} API의 버전을 지정하십시오.

이 Node.js 예제에서 랩퍼는 `conversation` 변수에 저장된 `ConversationV1`의 인스턴스입니다. 다른 언어로 된 Watson SDK는 서비스 랩퍼를 인스턴스화하는 동일한 메커니즘을 제공합니다.
{: javascript}

Python 예제에서, 랩퍼는 `conversation` 변수에 저장된 `watson_developer_cloud.ConversationV1`의 인스턴스입니다. 다른 언어로 된 Watson SDK는 서비스 랩퍼를 인스턴스화하는 동일한 메커니즘을 제공합니다.
{: python}

서비스 랩퍼를 작성한 후 이를 사용하여 {{site.data.keyword.conversationshort}} 서비스로 메시지를 보냅니다. 이 예제에서 메시지는 비어 있습니다. 대화 상자에서 conversation_start 노드를 트리거할 수 있으므로 입력 텍스트는 필요하지 않습니다.

`node <filename.js>` 명령을 사용하여 예제 애플리케이션을 실행하십시오.
{: javascript}

`python <filename.py>` 명령을 사용하여 예제 애플리케이션을 실행하십시오.
{: python}

**참고:** `npm install watson-developer-cloud`을 사용하여 Watson SDK for Node.js를 설치했는지 확인하십시오.
{: javascript}

**참고:** `pip install --upgrade watson-developer-cloud` 또는 `easy_install --upgrade watson-developer-cloud`를 사용하여 Watson SDK for Python을 설치했는지 확인하십시오.
{: python}

모든 것이 예상대로 작동되면, {{site.data.keyword.conversationshort}} 서비스가 대화 상자에서 출력을 리턴하며, 출력은 콘솔에 인쇄됩니다.

```
Welcome to the {{site.data.keyword.conversationshort}} example!
```
{: screen}

이 출력은 {{site.data.keyword.conversationshort}} 서비스와 통신했고 대화 상자에서 conversation_start 노드를 통해 지정된 환영 메시지를 수신했음을 나타냅니다. 이제 사용자 인터페이스를 추가할 수 있으며, 이를 통해 사용자 입력을 처리할 수 있습니다.

## 사용자 입력을 처리하여 인텐트 발견

사용자 입력을 처리하기 위해 애플리케이션에 사용자 인터페이스를 추가해야 합니다. 다음 예제에서는 내용을 단순화하고 표준 입출력을 사용합니다.
<span class="ph style-scope doc-content" data-hd-programlang="javascript">Node.js prompt-sync 모듈을 사용하여 이를 수행할 수 있습니다. (`npm install prompt-sync`를 사용하여 prompt-sync를 설치할 수 있습니다.)</span>
<span class="ph style-scope doc-content" data-hd-programlang="python">Python `input` 함수를 사용하여 이를 수행할 수 있습니다.</span>

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
# 예제 2: 사용자 입력을 추가하고 인텐트를 발견합니다.

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

이 버전의 애플리케이션은 전과 같은 방식으로 시작됩니다(빈 메시지를 {{site.data.keyword.conversationshort}} 서비스로 보내 대화 시작).

`processResponse()` 함수는 출력 텍스트와 함께 대화 상자에서 발견된 인텐트를 표시한 후 다음 사용자 입력을 요청하는 프롬프트를 표시합니다.
{: javascript }

그러면 대화 상자에서 발견한 모든 인텐트가 출력 텍스트와 함께 표시되고 다음 라운드의 사용자 입력을 묻는 프롬프트가 표시됩니다. (우리는 아직 대화의 마지막 방법을 구현하지 않았기 때문에, 현재로서는 `while True` 루프를 사용하고 있습니다.)
{: python }

하지만 여전히 올바르지 않은 내용이 있습니다.

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

{{site.data.keyword.conversationshort}} 서비스가 올바른 인텐트를 발견하고 있으며, 그럼에도 불구하고 대화 턴마다 conversation_start 노드에서 환영 메시지를 리턴합니다(`Welcome to the {{site.data.keyword.conversationshort}} example!`).

{{site.data.keyword.conversationshort}} 서비스가 Stateless이므로 이러한 일이 발생합니다. 이는 상태 정보를 유지하는 애플리케이션 때문입니다. 상태를 유지하기 위해 아직 어떤 것도 하지 않으므로 {{site.data.keyword.conversationshort}} 서비스는 사용자 입력마다 새 대화의 첫 턴으로 간주하며, conversation_start 노드를 트리거합니다.

## 상태 유지

대화에 대한 상태 정보는 *컨텍스트*를 사용하여 유지됩니다. 컨텍스트는 애플리케이션과 {{site.data.keyword.conversationshort}} 서비스 간에 전달되는 JSON 오브젝트입니다. 애플리케이션은 하나의 대화 턴에서 다음 대화 턴으로 컨텍스트를 유지합니다.

컨텍스트에는 사용자와의 각 대화 턴에 대한 고유 ID 및 대화할 때마다 증분되는 카운터가 포함됩니다. 이전 버전의 예제에서는 컨텍스트를 유지하지 않았으며, 이는 입력마다 새 대화의 시작으로 간주했음을 의미합니다. 이는 매번 컨텍스트를 저장하고 다시 {{site.data.keyword.conversationshort}} 서비스로 보냄으로써 수정할 수 있습니다.

대화에서 위치를 유지하는 것 외에, 컨텍스트를 사용하여 애플리케이션과 {{site.data.keyword.conversationshort}} 서비스 간에 전달할 다른 모든 데이터를 저장할 수도 있습니다. 이러한 데이터에는 대화 전체에서 유지할 지속적 데이터(예: 고객 이름 또는 계좌 번호) 또는 추적할 다른 모든 데이터(예: 옵션 설정의 현재 상태)가 포함될 수 있습니다.

```javascript
// 예제 3: 상태를 유지합니다.

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
# 예제 3: 상태를 유지합니다.

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

이전 예제에서 변경된 유일한 내용은 대화마다 이전 대화에서 수신한 `response.context` 오브젝트를 다시 보낸다는 점입니다.
{: javascript }

```javascript
    conversation.message({
      input: { text: newMessageFromUser },
      context : response.context,
    }, processResponse)
```
{: codeblock}
{: javascript }

이전 예제의 유일한 변경 사항은 대화 상자에서 수신된 컨텍스트를 `context`라는 변수에 저장하고 다음 라운드의 사용자 입력과 함께 다시 보냅니다.
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

이렇게 하면 컨텍스트가 하나의 대화 턴에서 다음 턴까지 유지되므로 {{site.data.keyword.conversationshort}} 서비스가 더 이상 각 대화를 처음으로 간주하지 않습니다.

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

이제 다음을 진행합니다. {{site.data.keyword.conversationshort}} 서비스는 올바르게 인텐트를 인식하며, 대화 상자는 인텐트마다 올바른 출력 텍스트(제공된 경우)를 리턴합니다.

하지만 어떤 상황도 발생하지 않습니다. 질문을 해도 응답이 없으며 작별 인사를 해도 대화가 종료되지 않습니다. 이는 해당 인텐트에서 앱이 추가 조치를 수행해야 하기 때문입니다.

## 앱 조치 구현

대화 상자는 사용자에게 출력 텍스트를 표시하는 것 외에 발견된 인텐트에 따라 애플리케이션이 조치를 실행해야 할 때 신호에 대한 응답 JSON에 `output` 오브젝트를 사용합니다.

이러한 조치 플래그는 `action` 특성을 사용하여 전송되며, 대화 상자는 이 플래그를 응답 JSON의 일부로 정의합니다. 대화 상자가 애플리케이션이 어떤 조치를 수행해야 한다고 판별하면 `action` 값을 적절한 값으로 설정합니다(`display_time` 또는 `end_conversation`).

`output`은 단지 JSON 오브젝트이며, 여기에 원하는 올바른 컨텐츠를 추가할 수 있습니다. 복잡한 애플리케이션의 경우 여러 조치 플래그와 함께 배열을 사용할 수 있습니다.

하지만 예제에서는 단일 조치 플래그를 지원하는 단순 키/값 쌍을 사용합니다. 애플리케이션 코드는 응답에서 `action` 특성 값을 검사한 다음 지정된 조치를 수행해야 합니다. (또한 발견된 인텐트가 올바르게 식별되므로 이 버전은 이 인텐트의 표시를 제거합니다.)

```javascript
// 예제 4: 앱 조치를 구현합니다.

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
# 예제 4: 앱 조치를 구현합니다.

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

processResponse() 함수는 이제 {{site.data.keyword.conversationshort}} 서비스에서 수신된 `output` 오브젝트의 `action` 특성 값을 검사합니다. 값이 `display_time` 또는 `end_conversation`인 경우 애플리케이션이 적절한 조치를 수행합니다.
{: javascript}

앱은 이제 {{site.data.keyword.conversationshort}} 서비스에서 수신된 `output` 오브젝트의 `action` 특성 값을 검사합니다. 값이 `display_time`인 경우, 애플리케이션이 적절한 조치를 수행합니다. 값이 `end_conversation`인 경우, 앱은 더 많은 사용자 입력을 요구하지 않으며, `while` 루프가 종료됩니다.
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

완료되었습니다. 애플리케이션은 이제 {{site.data.keyword.conversationshort}} 서비스를 사용하여 자연어 입력에서 인텐트를 식별하고, 적절한 응답을 표시하며, 요청된 클라이언트 조치를 구현합니다.

물론, 실제 애플리케이션은 더 정교한 사용자 인터페이스를 사용합니다(예: 웹 대화 창). 또한 복잡한 조치를 구현하며, 고객 데이터베이스 또는 기타 비즈니스 시스템과 통합할 수도 있습니다. 하지만 애플리케이션이 {{site.data.keyword.conversationshort}} 서비스와 상호작용하는 방법에 대한 기본 원칙은 동일합니다.

더 복잡한 예제의 경우 탐색 분할창의 샘플 앱을 살펴보십시오.
