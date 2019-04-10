---

copyright:
  years: 2015, 2019
lastupdated: "2019-02-21"

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

# 클라이언트 애플리케이션 빌드
{: #api-client}

실제로 사용할 수 있는 대화 스킬과 이를 사용하는 어시스턴트가 있습니다. 이제 사용자와 상호작용하고 {{site.data.keyword.conversationfull}} 서비스와 통신할 사용자 정의 클라이언트 애플리케이션을 개발하려고 합니다.
{: shortdesc}

## {{site.data.keyword.conversationshort}} 서비스 설정
{: #api-client-setup}

이 섹션에서 작성할 애플리케이션 예제에서는 코그너티브 개인 비서의 여러 기능을 구현합니다. 애플리케이션 코드가 {{site.data.keyword.conversationshort}} 어시스턴트에 연결되며 여기서 코그너티브 처리(예: 사용자 인텐트 발견)가 이루어집니다.

이 예제를 계속하기 전에 필수 어시스턴트를 설정해야 합니다.

1.  대화 스킬 <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/assistant/assistant-simple-example.json" download="assistant-simple-example.json">JSON 파일</a>을 다운로드합니다.
1.  {{site.data.keyword.conversationshort}} 서비스의 인스턴스로 [스킬을 가져옵니다](/docs/services/assistant?topic=assistant-skill-add#creating-skills).
1.  [어시스턴트를 작성](/docs/services/assistant?topic=assistant-assistant-add#creating-assistants)하고 가져온 스킬을 연결합니다.

## 서비스 정보 가져오기
{: #api-client-get-info}

{{site.data.keyword.conversationshort}} 서비스 REST API에 액세스하려면 애플리케이션이 {{site.data.keyword.Bluemix}}에 인증하고 올바른 어시스턴트에 연결되어야 합니다. 서비스 인증 정보 및 어시스턴트 ID를 복사하고 애플리케이션 코드에 붙여넣어야 합니다.

{{site.data.keyword.conversationshort}} 도구에서 서비스 인증 정보와 어시스턴트 ID에 액세스하려면 **어시스턴트** 탭으로 이동하여 연결하려는 어시스턴트의 ![메뉴](images/kabob-grey.png) 메뉴를 클릭하십시오. 어시스턴트 ID 및 API 키를 포함하여, 어시스턴트의 세부사항을 보려면 **API 세부사항 보기**를 선택하십시오.

또한, {{site.data.keyword.Bluemix_short}} 대시보드에서 서비스에 인증 정보에 액세스할 수 있습니다.

## {{site.data.keyword.conversationshort}} 서비스와 통신하기
{: #api-client-communicate}

{{site.data.keyword.conversationshort}} 서비스와 통신하는 방법은 간단합니다. 서비스에 연결하고, 단일 메시지를 발송하고, 결과를 콘솔에 출력하는 예제를 살펴보겠습니다. 

```javascript
// 예제 1: 서비스 랩퍼를 설정하고, 초기 메시지를 발송하며,
// 응답을 수신합니다.

var AssistantV2 = require('watson-developer-cloud/assistant/v2');

// Set up Assistant service wrapper.
var service = new AssistantV2({
  iam_apikey: '{apikey}', // API 키로 대체
  version: '2018-09-20'
});

var assistantId = '{assistant_id}'; // 어시스턴트 ID로 대체
var sessionId;

// 세션을 작성합니다.
service.createSession({
  assistant_id: assistantId
}, function(err, result) {
  if (err) {
    console.error(err); // 문제가 발생한 경우
    return;
  }
  sessionId = result.session_id;
  sendMessage(); // 빈 메시지로 대화 시작
});

// 어시스턴트로 메시지를 발송합니다.
function sendMessage() {
  service.message({
    assistant_id: assistantId,
    session_id: sessionId
  }, processResponse);
}

// 응답을 처리합니다.
function processResponse(err, response) {
  if (err) {
    console.error(err); // 문제가 발생한 경우
    return;
  }

  // 어시스턴트로부터의 메시지를 표시합니다(있는 경우). 단일 텍스트 응답을 가정합니다.
  if (response.output.generic.length != 0) {
      console.log(response.output.generic[0].text);
  }

  // 이제 끝났으므로 세션을 닫습니다.
  service.deleteSession({
    assistant_id: assistantId,
    session_id: sessionId
  }, function(err, result) {
    if (err) {
      console.error(err); // 문제가 발생한 경우
    }
  });
}
```
{: codeblock}
{: javascript}

```python
# 예제 1: 서비스 랩퍼를 설정하고, 초기 메시지를 발송하며,
# 응답을 수신합니다.

import watson_developer_cloud

# 어시스턴트 서비스를 설정합니다.
service = watson_developer_cloud.AssistantV2(
    iam_apikey = '{apikey}', # API 키로 대체
    version = '2018-09-20'
)

assistant_id = '{assistant_id}' # 어시스턴트 ID로 대체

# 세션을 작성합니다.
session_id = service.create_session(
    assistant_id = assistant_id
).get_result()['session_id']

# 빈 메시지로 대화를 시작합니다.
response = service.message(
    assistant_id,
    session_id
).get_result()

# 대화로부터의 출력을 인쇄합니다(있는 경우). 단일 텍스트 메시지을 가정합니다.
if response['output']['generic']:
    print(response['output']['generic'][0]['text'])

# 이제 끝났으므로 세션을 삭제합니다.
service.delete_session(
    assistant_id = assistant_id,
    session_id = session_id
)
```
{: codeblock}
{: python}

```java
/*
 * 예제 1: 서비스 랩퍼를 설정하고, 초기 메시지를 발송하며,
 * 응답을 수신합니다.
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

    // 표준 출력에 로그 메시지를 표시하지 않습니다.
    LogManager.getLogManager().reset();

    // 어시스턴트 서비스를 설정합니다.
    IamOptions iamOptions = new IamOptions.Builder().apiKey("{apikey}").build();
    Assistant service = new Assistant("2018-09-20", iamOptions);
    String assistantId = "{assistant_id}"; // 어시스턴트 ID로 대체

    // 세션을 작성합니다.
    CreateSessionOptions createSessionOptions = new CreateSessionOptions.Builder(assistantId).build();
    SessionResponse session = service.createSession(createSessionOptions).execute();
    String sessionId = session.getSessionId();

    // 빈 메시지로 대화를 시작합니다.
    MessageOptions messageOptions = new MessageOptions.Builder(assistantId, sessionId).build();
    MessageResponse response = service.message(messageOptions).execute();

    // 대화로부터의 출력을 인쇄합니다(있는 경우). 단일 텍스트 응답을 가정합니다.
    System.out.println(response.getOutput().getGeneric().get(0).getText());

    // 이제 끝났으므로 세션을 삭제합니다.
    DeleteSessionOptions deleteSessionOptions = new DeleteSessionOptions.Builder(assistantId, sessionId).build();
    service.deleteSession(deleteSessionOptions).execute();
  }
}
```
{: codeblock}
{: java}

첫 번째 단계는 {{site.data.keyword.conversationshort}} 서비스의 랩퍼를 작성하는 것입니다.

랩퍼는 서비스로 입력을 보내고 서비스에서 출력을 받는 데 사용할 오브젝트입니다. 서비스 랩퍼를 작성할 때 서비스 키의 인증용 인증 정보 및 사용 중인 {{site.data.keyword.conversationshort}} API의 버전을 지정하십시오.

이 Node.js 예제에서, 랩퍼는 변수 `service`에 저장된 `AssistantV2`의 인스턴스입니다. 다른 언어용 Watson SDK는 서비스 랩퍼를 인스턴스화하는 동일한 메커니즘을 제공합니다.
{: javascript}

이 Python 예제에서, 랩퍼는 변수 `service`에 저장된 `watson_developer_cloud.AssistantV2`의 인스턴스입니다. 다른 언어용 Watson SDK는 서비스 랩퍼를 인스턴스화하는 동일한 메커니즘을 제공합니다.
{: python}

이 Java 예제에서, 랩퍼는 변수 `service`에 저장된 `Assistant`의 인스턴스입니다. 다른 언어용 Watson SDK는 서비스 랩퍼를 인스턴스화하는 동일한 메커니즘을 제공합니다.
{: java}

서비스 랩퍼를 작성한 후 이를 사용하여 세션을 작성하고 어시스턴트로 메시지를 보냅니다. 이 예제에서 메시지는 비어 있습니다. 대화에서 conversation_start 노드를 트리거할 수 있으므로 입력 텍스트는 필요하지 않습니다. 그런 다음 콘솔에 응답 텍스트를 인쇄한 후 마지막으로 세션을 삭제합니다.

`node <filename.js>` 명령을 사용하여 예제 애플리케이션을 실행하십시오.
{: javascript}

`python3 <filename.py>` 명령을 사용하여 예제 애플리케이션을 실행하십시오.
{: python}

예제 코드를 `AssistantSimpleExample.java`라는 파일에 붙여넣습니다. 그런 다음 컴파일하여 실행할 수 있습니다.
{: java}

**참고:** `npm install watson-developer-cloud`를 사용하여 Watson SDK for Node.js를 설치했는지 확인하십시오.
{: javascript}

**참고:** `pip install --upgrade watson-developer-cloud` 또는 `easy_install --upgrade watson-developer-cloud`를 사용하여 Watson SDK for Python을 설치했는지 확인하십시오.
{: python}

**참고:** [Watson SDK for Java![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/watson-developer-cloud/java-sdk/blob/develop/README.md){: new_window}를 설치했는지 확인하십시오.
{: java}

모든 것이 예상대로 작동되면, 어시스턴트가 대화에서 출력을 리턴하며, 출력은 콘솔에 인쇄됩니다.

```
Welcome to the Watson Assistant example!
```
{: screen}

이 출력은 {{site.data.keyword.conversationshort}} 서비스와 통신했고 대화에서 conversation_start 노드를 통해 지정된 환영 메시지를 수신했음을 나타냅니다. 이제 사용자 인터페이스를 추가할 수 있으며, 이를 통해 사용자 입력을 처리할 수 있습니다.

## 사용자 입력을 처리하여 인텐트 발견
{: #api-client-process-input}

사용자 입력을 처리하기 위해 클라이언트 애플리케이션에 사용자 인터페이스를 추가해야 합니다. 다음 예제에서는 내용을 단순화하며 표준 입출력을 사용합니다.
<span class="ph style-scope doc-content" data-hd-programlang="javascript">Node.js prompt-sync 모듈을 사용하여 이를 수행할 수 있습니다. (`npm install prompt-sync`를 사용하여 prompt-sync를 설치할 수 있습니다.)</span>
<span class="ph style-scope doc-content" data-hd-programlang="python">Python 3 `input` 함수를 사용하여 이를 수행할 수 있습니다.</span>
<span class="ph style-scope doc-content" data-hd-programlang="java">Java `Console.readLine()` 함수를 사용하여 이를 수행할 수 있습니다.</span>

```javascript
// 예제 2: 사용자 입력을 추가하고 인텐트를 발견합니다.

var prompt = require('prompt-sync')();
var AssistantV2 = require('watson-developer-cloud/assistant/v2');

// 어시스턴트 서비스 랩퍼를 설정합니다.
var service = new AssistantV2({
  iam_apikey: '{apikey}', // API 키로 대체
  version: '2018-09-20'
});

var assistantId = '{assistant_id}'; // 어시스턴트 ID로 대체
var sessionId;

// 세션을 작성합니다.
service.createSession({
  assistant_id: assistantId
}, function(err, result) {
  if (err) {
    console.error(err); // 문제가 발생한 경우
    return;
  }
  sessionId = result.session_id;
  sendMessage(); // 빈 메시지로 대화 시작
});

// 어시스턴트로 메시지를 발송합니다.
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

// 응답을 처리합니다.
function processResponse(err, response) {
  if (err) {
    console.error(err); // 문제가 발생한 경우
    return;
  }

  // If an intent was detected, log it out to the console.
  if (response.output.intents.length > 0) {
    console.log('Detected intent: #' + response.output.intents[0].intent);
  }

  // 어시스턴트로부터의 출력을 표시합니다(있는 경우). 단일 텍스트 응답을 가정합니다.
  if (response.output.generic.length != 0) {
    console.log(response.output.generic[0].text);
  }

  // 다음 라운드의 입력에 대한 프롬프트를 표시합니다.
  var newMessageFromUser = prompt('>> ');
    if (newMessageFromUser === 'quit') {
      service.deleteSession({
        assistant_id: assistantId,
        session_id: sessionId
      }, function(err, result) {
        if (err) {
          console.error(err); // 문제가 발생한 경우
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
# 예제 2: 사용자 입력을 추가하고 인텐트를 발견합니다.

import watson_developer_cloud

# 어시스턴트 서비스를 설정합니다.
service = watson_developer_cloud.AssistantV2(
    iam_apikey = '{apikey}', # API 키로 대체
    version = '2018-09-20'
)

assistant_id = '{assistant_id}' # 어시스턴트 ID로 대체

# 세션을 작성합니다.
session_id = service.create_session(
    assistant_id = assistant_id
).get_result()['session_id']

# 대화를 시작하기 위해 빈 값으로 초기화합니다.
user_input = ''

# 기본 입/출력 루프
while user_input != 'quit':

    # 어시스턴트로 메시지를 발송합니다.
    response = service.message(
        assistant_id,
        session_id,
        input = {
            'text': user_input
    }
    ).get_result()

    # 인텐트가 발견된 경우 이를 콘솔에 인쇄합니다.
    if response['output']['intents']:
        print('Detected intent: #' + response['output']['intents'][0]['intent'])

    # 대화로부터의 출력을 인쇄합니다(있는 경우). 단일 텍스트 응답을 가정합니다.
    if response['output']['generic']:
        print(response['output']['generic'][0]['text'])

    # 다음 라운드의 입력에 대한 프롬프트를 표시합니다.
    user_input = input('>> ')

# 이제 끝났으므로 세션을 삭제합니다.
service.delete_session(
    assistant_id = assistant_id,
    session_id = session_id
)
```
{: codeblock }
{: python }

```java
/*
 * 예제 2: 사용자 입력을 추가하고 인텐트를 발견합니다.
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

    // 표준 출력에 로그 메시지를 표시하지 않습니다.
    LogManager.getLogManager().reset();

    // 어시스턴트 서비스를 설정합니다.
    IamOptions iamOptions = new IamOptions.Builder().apiKey("{apikey}").build();
    Assistant service = new Assistant("2018-09-20", iamOptions);
    String assistantId = "{assistant_id}"; // 어시스턴트 ID로 대체

    // 세션을 작성합니다.
    CreateSessionOptions createSessionOptions = new CreateSessionOptions.Builder(assistantId).build();
    SessionResponse session = service.createSession(createSessionOptions).execute();
    String sessionId = session.getSessionId();

    // 대화를 시작하기 위해 빈 값으로 초기화합니다.
    String inputText = "";

    // 기본 입/출력 루프
    do {
      // 어시스턴트로 메시지를 발송합니다.
      MessageInput input = new MessageInput.Builder().text(inputText).build();
      MessageOptions messageOptions = new MessageOptions.Builder(assistantId, sessionId)
                                                  .input(input)
                                                  .build();
      MessageResponse response = service.message(messageOptions).execute();

      // 인텐트가 발견된 경우 이를 콘솔에 인쇄합니다.
      List<RuntimeIntent> responseIntents = response.getOutput().getIntents();
      if(responseIntents.size() > 0) {
        System.out.println("Detected intent: #" + responseIntents.get(0).getIntent());
      }

      // 대화로부터의 출력을 인쇄합니다(있는 경우). 단일 텍스트 응답을 가정합니다.
      List<DialogRuntimeResponseGeneric> responseGeneric = response.getOutput().getGeneric();
      if(responseGeneric.size() > 0) {
        System.out.println(response.getOutput().getGeneric().get(0).getText());
      }

      // 다음 라운드의 입력에 대한 프롬프트를 표시합니다.
      System.out.print(">> ");
      inputText = System.console().readLine();
    } while(!inputText.equals("quit"));

    // 이제 끝났으므로 세션을 삭제합니다.
    DeleteSessionOptions deleteSessionOptions = new DeleteSessionOptions.Builder(assistantId, sessionId).build();
    service.deleteSession(deleteSessionOptions).execute();
  }
}
```
{: codeblock }
{: java }

이 버전의 애플리케이션은 전과 같은 방식으로 시작됩니다(빈 메시지를 어시스턴트로 보내 대화 시작).

`processResponse()` 함수는 이제 출력 텍스트와 함께 대화 스킬에서 발견한 모든 인텐트를 표시합니다.
그런 다음, 다음 라운드의 사용자 입력에 대한 프롬프트가 표시됩니다.
{: javascript }

그런 다음 출력 텍스트와 함께 대화 스킬에서 발견된 모든 인텐트를 표시합니다. 그런 다음, 다음 라운드의 사용자 입력에 대한 프롬프트가 표시됩니다.
{: python }

그런 다음 대화에서 발견된 모든 인텐트가 출력 텍스트와 함께 표시되고 다음 라운드의 사용자 입력에 대한 프롬프트가 표시됩니다.
{: java}

대화를 끝내기 위해 자연어를 사용하는 방법을 아직 구현하지 않았으므로, 그 대신 프로그램이 세션을 삭제하고 종료해야 한다는 것을 표시하기 위해 리터럴 명령 `quit`을 사용합니다.

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

이제 다음을 진행합니다. {{site.data.keyword.conversationshort}} 서비스는 올바르게 인텐트를 인식하며, 대화는 인텐트마다 올바른 출력 텍스트(제공된 경우)를 리턴합니다.

하지만 어떤 상황도 발생하지 않습니다. 질문을 해도 응답이 없으며 작별 인사를 해도 대화가 종료되지 않습니다. 이는 해당 인텐트는 앱에서 추가 조치를 수행해야 하기 때문입니다.

## 앱 조치 구현
{: #api-client-implement-actions}

{{site.data.keyword.conversationshort}} 대화는 사용자에게 출력 텍스트를 표시하는 것 외에, 발견된 인텐트에 따라 애플리케이션이 조치를 실행해야 할 때 신호에 대한 응답 JSON에 `actions` 오브젝트를 사용합니다. 클라이언트 애플리케이션이 무엇을 수행해야 한다고 대화가 결정하는 경우, `client`의 `type`을 사용하는 조치 오브젝트를 리턴합니다. 이 조치의 `name`은 특정 조치 `display_time` 또는 `end_conversation`를 나타냅니다.(이 조치의 추가 특성은 매개변수, 인증 정보 및 조치와 관련된 기타 정보를 지정할 수 있지만, 예제에서는 조치 이름만 필요합니다.)

대화가 한 번에 두 개 이상의 조치를 요청하지 않는다는 것을 알고 있으므로, 클라이언트는 `actions` 배열에서 단일 `client`의 존재 여부를 확인하기만 하면 됩니다. 하나를 찾으면 지정된 조치를 수행할 수 있습니다. (또한 발견된 인텐트가 올바르게 식별되므로 이 버전은 이 인텐트의 표시를 제거합니다.)

```javascript
// 예제 3: 앱 조치를 구현합니다.

var prompt = require('prompt-sync')();
var AssistantV2 = require('watson-developer-cloud/assistant/v2');

// 어시스턴트 서비스를 설정합니다.
var service = new AssistantV2({
  iam_apikey: '{apikey}', // API 키로 대체
  version: '2018-09-20'
});

var assistantId = '{assistant_id}'; // 어시스턴트 ID로 대체
var sessionId;

// 세션을 작성합니다.
service.createSession({
  assistant_id: assistantId
}, function(err, result) {
  if (err) {
    console.error(err); // 문제가 발생한 경우
    return;
  }
  sessionId = result.session_id;
  sendMessage(''); // 빈 메시지로 대화 시작
});

// 어시스턴트로 메시지를 발송합니다.
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

// 응답을 처리합니다.
function processResponse(err, response) {
  if (err) {
    console.error(err); // 문제가 발생한 경우
    return;
  }

  var endConversation = false;

  // 어시스턴트에 의해 요청된 클라이언트 조치를 확인합니다.
  if (response.output.actions) {
    if (response.output.actions[0].type === 'client'){
      if (response.output.actions[0].name === 'display_time') {
        // 사용자가 몇 시냐고 물어서 로컬 시스템 시간을 출력합니다.
        console.log('The current time is ' + new Date().toLocaleTimeString() + '.');
      } else if (response.output.actions[0].name === 'end_conversation') {
        // 사용자가 goodbye라고 말했으므로 이제 끝났습니다.
        console.log(response.output.generic[0].text);
        endConversation = true;
      }
    }
  } else {
    // 어시스턴트로부터의 출력을 인쇄합니다(있는 경우). 단일 텍스트 응답을 가정합니다.
    if (response.output.generic.length != 0) {
      console.log(response.output.generic[0].text);
    }
  }

  // 아직 끝나지 않은 경우 다음 라운드의 입력에 대한 프롬프트를 표시합니다.
  if (!endConversation) {
    var newMessageFromUser = prompt('>> ');
    sendMessage(newMessageFromUser);
  } else {
    service.deleteSession({
      assistant_id: assistantId,
      session_id: sessionId
    }, function(err, result) {
      if (err) {
        console.error(err); // 문제가 발생한 경우
      }
    });
    return;
  }
}
```
{: codeblock}
{: javascript}

```python
# 예제 3: 앱 조치를 구현합니다.

import watson_developer_cloud
import time

# 어시스턴트 서비스를 설정합니다.
service = watson_developer_cloud.AssistantV2(
    iam_apikey = '{apikey}', # API 키로 대체
    version = '2018-09-20'
)

assistant_id = '{assistant_id}' # 어시스턴트 ID로 대체

# 세션을 작성합니다.
session_id = service.create_session(
    assistant_id = assistant_id
).get_result()['session_id']

# 대화를 시작하기 위해 빈 값으로 초기화합니다.
user_input = ''
current_action = ''

# 기본 입/출력 루프
while current_action != 'end_conversation':
    # 이전 응답에 의해 설정된 모든 조치 플래그를 지웁니다.
    current_action = ''

    # 어시스턴트로 메시지를 발송합니다.
    response = service.message(
        assistant_id,
        session_id,
        input = {
            'text': user_input
    }
    ).get_result()

    # 대화로부터의 출력을 인쇄합니다(있는 경우). 단일 텍스트 응답을 가정합니다.
    if response['output']['generic']:
        print(response['output']['generic'][0]['text'])

    # 어시스턴트에서 요청한 클라이언트 조치를 확인합니다.
    if 'actions' in response['output']:
        if response['output']['actions'][0]['type'] == 'client':
            current_action = response['output']['actions'][0]['name']

    # 사용자가 몇 시인지 물어서 로컬 시스템 시간을 출력합니다.
    if current_action == 'display_time':
        print('The current time is ' + time.strftime('%I:%M:%S %p') + '.')
    # 아직 끝나지 않은 경우 다음 라운드의 입력에 대한 프롬프트를 표시합니다.
    if current_action != 'end_conversation':
        user_input = input('>> ')

# 이제 끝났으므로 세션을 삭제합니다.
service.delete_session(
    assistant_id = assistant_id,
    session_id = session_id
)
```
{: codeblock}
{: python}

```java
/*
 * 예제 3: 앱 조치를 구현합니다.
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

    // 표준 출력에 로그 메시지를 표시하지 않습니다.
    LogManager.getLogManager().reset();

    // 세션 서비스를 설정합니다.
    IamOptions iamOptions = new IamOptions.Builder().apiKey("{apikey}").build();
    Assistant service = new Assistant("2018-09-20", iamOptions);
    String assistantId = "{assistant_id}"; // 어시스턴트 ID로 대체

    // 세션을 작성합니다.
    CreateSessionOptions createSessionOptions = new CreateSessionOptions.Builder(assistantId).build();
    SessionResponse session = service.createSession(createSessionOptions).execute();
    String sessionId = session.getSessionId();

    // 대화를 시작하기 위해 빈 값으로 초기화합니다.
    String inputText = "";
    String currentAction;

    // 기본 입/출력 루프
    do {
      // 이전 응답에 의해 설정된 모든 조치 플래그를 지웁니다.
      currentAction = "";

      // 어시스턴트로 메시지를 발송합니다.
      MessageInput input = new MessageInput.Builder().text(inputText).build();
      MessageOptions messageOptions = new MessageOptions.Builder(assistantId, sessionId)
                                                  .input(input)
                                                  .build();
      MessageResponse response = service.message(messageOptions).execute();

      // 대화로부터의 출력을 인쇄합니다(있는 경우). 단일 텍스트 응답을 가정합니다.
      List<DialogRuntimeResponseGeneric> responseGeneric = response.getOutput().getGeneric();
      if(responseGeneric.size() > 0) {
        System.out.println(response.getOutput().getGeneric().get(0).getText());
      }

      // 어시스턴트에 의해 요청된 모든 조치를 확인합니다.
      List<DialogNodeAction> responseActions = response.getOutput().getActions();
      if(responseActions != null) {
        if(responseActions.get(0).getActionType().equals("client")) {
          currentAction = responseActions.get(0).getName();
        }
      }

      // 사용자가 몇 시인지 물어서 로컬 시스템 시간을 출력합니다.
      if(currentAction.equals("display_time")) {
        DateTimeFormatter fmt = DateTimeFormatter.ofPattern("h:mm:ss a");
        LocalTime time = LocalTime.now();
        System.out.println("The current time is " + time.format(fmt) + ".");
      }

      // 아직 끝나지 않은 경우 다음 라운드 입력에 대한 프롬프트를 표시합니다.
      if(!currentAction.equals("end_conversation")) {
        System.out.print(">> ");
        inputText = System.console().readLine();
      }

    } while(!currentAction.equals("end_conversation"));

    // 이제 끝났으므로 세션을 삭제합니다.
    DeleteSessionOptions deleteSessionOptions = new DeleteSessionOptions.Builder(assistantId, sessionId).build();
    service.deleteSession(deleteSessionOptions).execute();
  }
}
```
{: codeblock}
{: java}

이제 애플리케이션 응답에서 `actions` 배열을 확인하여 `type`=`client`를 포함하는 조치가 있는지 확인합니다. 그러한 경우, 조치의 `name` 값을 확인하고 적절한 조치(로컬 시스템 시간 표시 또는 대화가 끝났음을 표시하는 내부 플래그 설정)를 수행합니다.

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

물론, 실제 애플리케이션은 더 정교한 사용자 인터페이스를 사용합니다(예: 웹 대화 창). 또한 복잡한 조치를 구현하며, 고객 데이터베이스 또는 기타 비즈니스 시스템과 통합할 수도 있습니다. 또한, 각각의 고유한 사용자를 식별하기 위해 사용자 ID와 같은 추가적인 데이터를 어시스턴트에 전송해야 합니다. 하지만 애플리케이션이 {{site.data.keyword.conversationshort}} 서비스와 상호작용하는 방법에 대한 기본 원칙은 동일합니다.

몇 가지 더 복잡한 예제는 [샘플 앱](/docs/services/assistant?topic=assistant-sample-apps)를 참조하십시오.

## 컨텍스트 액세스
{: #api-client-get-context}

*컨텍스트*는 대화를 통해 지속되는 변수를 포함하는 오브젝트이며 대화와 클라이언트 애플리케이션에서 공유할 수 있습니다. 애플리케이션이 v2 API를 사용하는 경우에는 세션별로 어시스턴트에서 컨텍스트를 자동으로 관리합니다. 대화와 클라이언트 애플리케이션은 둘 다 컨텍스트 변수를 읽고 쓸 수 있습니다. 기본적으로 컨텍스트는 클라이언트 애플리케이션에 리턴되지 않지만, 선택적으로 각각의 `/message` 요청에 대한 응답에 포함되도록 요청할 수 있습니다.

**중요:** 어시스턴트 사용은 어시스턴트와 상호작용하는 각각의 최종 사용자에 대해 고유한 사용자 ID를 지정하는 것입니다. 사용자 기반 플랜의 경우 이 ID는 청구용으로 사용됩니다. (자세한 정보는 [사용자 기반 플랜](/docs/services/assistant?topic=assistant-services-information#user-based-plans)을 참조하십시오.)

다음과 같은 두 가지 유형의 컨텍스트가 있습니다.

- **글로벌 컨텍스트**: 대화 플로우를 관리하는 데 사용되는 내부 시스템 변수를 포함하여, 어시스턴트에서 사용하는 모든 스킬이 공유하는 컨텍스트 변수입니다.

- **스킬 특정 컨텍스트**: 애플리케이션에서 필요로 하는 사용자 정의 변수를 포함하여 특정 스킬에 고유한 컨텍스트 변수입니다. 현재, 하나의 스킬(이름: `main skill`)만 지원됩니다.

다음 예제는 글로벌 및 스킬 특정 컨텍스트 변수를 포함하는 `/message` 요청을 표시합니다. 또한 이 요청은 컨텍스트가 응답과 함께 리턴되도록 요청하기 위해 `options.return_context` 속성을 사용합니다 `

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

이 예제 요청에서 애플리케이션은 글로벌 컨텍스트의 일부로 `user_id`에 대한 값을 지정합니다.
또한, 스킬 특정 컨텍스트의 일부로 하나의 사용자 정의 컨텍스트 변수(`account_number`)를 설정합니다. 이 컨텍스트 변수는
대화 노드에서 `$account_number`로 액세스할 수 있습니다. (대화에서 컨텍스트를 사용하는 방법에 대한 자세한 정보는
[대화 처리 방법](/docs/services/assistant?topic=assistant-dialog-runtime)을 참조하십시오.)

사용자 정의 컨텍스트 변수에 사용할 변수 이름을 지정할 수 있습니다. 지정된 변수가 이미 존재하는 경우 새 값이 겹쳐씁니다. 그렇지 않은 경우, 새 변수가 컨텍스트에 추가됩니다.

이 요청의 출력에는 일반적인 출력뿐만 아니라 지정된 값이 추가되었음을 표시하는 컨텍스트도 포함됩니다.

```json
{
  "output": {
    "generic":[
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

API를 사용하여 컨텍스트 변수에 액세스하는 방법에 대한 자세한 정보는 [v2 API 참조 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://{DomainName}/apidocs/assistant-v2#send-user-input-to-assistant){: new_window}를 참조하십시오.

## v1 API 사용
{: #api-client-v1-api}

v2 API 사용은 {{site.data.keyword.conversationshort}} 서비스와 통신하는 런타임 클라이언트 애플리케이션을 빌드하는 데 권장되는 방법입니다. 그러나 일부 이전 애플리케이션에서는 여전히 v1 API를 사용하고 있을 수 있지만, 여기에는 대화 스킬에서 작업 공간으로 메시지를 보내기 위한 유사한 런타임 메소드가 포함되어 있습니다.

애플리케이션이 v1 API를 사용하는 경우, 이 애플리케이션은 어시스턴트의 오케스트레이션 및 상태 관리 기능을 사용하지 않고 작업공간과 직접 통신합니다.
이는 애플리케이션이 컨텍스트를 사용하여 상태 정보를 관리해야 함을 의미합니다.
애플리케이션은 각 응답과 함께 수신된 컨텍스트를 저장하고 각각의 새 메시지 요청과 함께 서비스에 다시 전송하여 컨텍스트를 유지보수해야 합니다.
(v1 `/message` 메소드는 항상 각 응답과 함께 컨텍스트를 리턴합니다.)

v1 `/message` 메소드 및 컨텍스트에 대한 자세한 정보는 [v1 API 참조![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://cloud.ibm.com/apidocs/assistant#get-response-to-user-input){: new_window}를 참조하십시오.
