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

# 构建客户机应用程序
{: #api-client}

您已有了有效的对话技能以及使用该技能的助手。现在，您希望开发定制客户机应用程序，用来与用户进行交互并与 {{site.data.keyword.conversationfull}} 服务进行通信。
{: shortdesc}

## 设置 {{site.data.keyword.conversationshort}} 服务
{: #api-client-setup}

我们将在本部分中创建示例应用程序，用于实现认知个人助手的若干功能。应用程序代码将连接到 {{site.data.keyword.conversationshort}} 助手，在其中将执行认知处理（例如，用户意向检测）。

继续使用此示例之前，您需要先设置必需的助手：

1.  下载对话技能 <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/assistant/assistant-simple-example.json" download="assistant-simple-example.json">JSON 文件</a>。
1.  [导入技能](/docs/services/assistant?topic=assistant-skill-add#creating-skills)至 {{site.data.keyword.conversationshort}} 服务的实例。
1.  [创建助手](/docs/services/assistant?topic=assistant-assistant-add#creating-assistants)并连接导入的技能。

## 获取服务信息
{: #api-client-get-info}

要访问 {{site.data.keyword.conversationshort}} 服务 REST API，应用程序需要能够向 {{site.data.keyword.Bluemix}} 进行认证，并连接到正确的助手。您需要复制服务凭证和助手标识，然后将其粘贴到应用程序代码中。

要从 {{site.data.keyword.conversationshort}} 工具访问服务凭证和助手标识，请转至**助手**选项卡，然后单击要连接到的助手的 ![菜单](images/kabob-grey.png) 菜单。选择**查看 API 详细信息**以查看助手的详细信息，包括助手标识和 API 密钥。

还可以通过 {{site.data.keyword.Bluemix_short}}“仪表板”来访问服务凭证。

## 与 {{site.data.keyword.conversationshort}} 服务通信
{: #api-client-communicate}

与 {{site.data.keyword.conversationshort}} 服务交互非常简单。请查看下面的示例，此示例连接到服务，发送一条消息，然后将输出显示到控制台：

```javascript
// 示例 1：设置服务包装器，发送初始消息，
// 并接收响应。

var AssistantV2 = require('watson-developer-cloud/assistant/v2');

// 设置 Assistant 服务包装器。
var service = new AssistantV2({
  iam_apikey: '{apikey}', // 替换为 API 密钥
  version: '2018-09-20'
});

var assistantId = '{assistant_id}'; // 替换为助手标识
var sessionId;

// 创建会话。
service.createSession({
  assistant_id: assistantId
}, function(err, result) {
  if (err) {
    console.error(err); // 发生了问题
    return;
  }
  sessionId = result.session_id;
  sendMessage(); // 使用空消息启动会话
});

// 向助手发送消息。
function sendMessage() {
  service.message({
    assistant_id: assistantId,
    session_id: sessionId
  }, processResponse);
}

// 处理响应。
function processResponse(err, response) {
  if (err) {
    console.error(err); // something went wrong
    return;
  }

  // 显示助手的输出（如有）。假定是单个文本响应。
  if (response.output.generic.length != 0) {
      console.log(response.output.generic[0].text);
  }

  // 操作已完成，将关闭会话
  service.deleteSession({
    assistant_id: assistantId,
    session_id: sessionId
  }, function(err, result) {
    if (err) {
    console.error(err); // 发生了问题
    }
  });
}
```
{: codeblock}
{: javascript}

```python
# 示例 1：设置服务包装器，发送初始消息，
# 然后接收响应。

import watson_developer_cloud

# 设置 Assistant 服务。
service = watson_developer_cloud.AssistantV2(
    iam_apikey = '{apikey}', # 替换为 API 密钥
    version = '2018-09-20'
)

assistant_id = '{assistant_id}' # 替换为助手标识

# 创建会话。
session_id = service.create_session(
    assistant_id = assistant_id
).get_result()['session_id']

# 以空消息开始会话。
response = service.message(
    assistant_id,
    session_id
).get_result()

# 显示对话的输出（如有）。假定是单个文本响应。
if response['output']['generic']:
    print(response['output']['generic'][0]['text'])

# 操作已完成，将删除会话。
service.delete_session(
    assistant_id = assistant_id,
    session_id = session_id
)
```
{: codeblock}
{: python}

```java
/*
 * 示例 1：设置服务包装器，发送初始消息，
 * 然后接收响应。
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

    // 禁止日志消息显示在 stdout 中。
    LogManager.getLogManager().reset();

    // 设置 Assistant 服务。
    IamOptions iamOptions = new IamOptions.Builder().apiKey("{apikey}").build();
    Assistant service = new Assistant("2018-09-20", iamOptions);
    String assistantId = "{assistant_id}"; // 替换为助手标识

    // 创建会话。
    CreateSessionOptions createSessionOptions = new CreateSessionOptions.Builder(assistantId).build();
    SessionResponse session = service.createSession(createSessionOptions).execute();
    String sessionId = session.getSessionId();

    // 以空消息开始会话。
MessageOptions messageOptions = new MessageOptions.Builder(assistantId, sessionId).build();
    MessageResponse response = service.message(messageOptions).execute();

    // 显示对话的输出（如有）。假定是单个文本响应。
    System.out.println(response.getOutput().getGeneric().get(0).getText());

    // 操作已完成，将删除会话。
    DeleteSessionOptions deleteSessionOptions = new DeleteSessionOptions.Builder(assistantId, sessionId).build();
    service.deleteSession(deleteSessionOptions).execute();
  }
}
```
{: codeblock}
{: java}

第一步是为 {{site.data.keyword.conversationshort}} 服务创建包装器。

包装器是一个对象，用于向服务发送输入并从服务接收输出。创建服务包装器时，请指定服务密钥中的认证凭证以及使用的 {{site.data.keyword.conversationshort}} API 的版本。

在此 Node.js 示例中，包装器是存储在 `service` 变量中的 `AssistantV2` 实例。针对其他语言的 Watson SDK 提供了用于实例化服务包装器的等效机制。
{: javascript}

在此 Python 示例中，包装器是存储在 `service` 变量中的 `watson_developer_cloud.AssistantV2` 实例。针对其他语言的 Watson SDK 提供了用于实例化服务包装器的等效机制。
{: python}

在此 Java 示例中，包装器是存储在 `service` 变量中的 `Assistant` 实例。针对其他语言的 Watson SDK 提供了用于实例化服务包装器的等效机制。
{: java}

创建服务包装器之后，将使用该包装器来创建会话，并将消息发送给助手。在此示例中，消息为空；我们只想触发对话中的 conversation_start 节点，因此不需要任何输入文本。然后，将响应文本显示到控制台，最后删除会话。

使用 `node <filename.js>` 命令运行示例应用程序。
{: javascript}

使用 `python3 <filename.py>` 命令运行示例应用程序。
{: python}

将示例代码粘贴到名为 `AssistantSimpleExample.java` 的文件中。然后，可以编译并运行该文件。
{: java}

**注：**确保已使用 `npm install watson-developer-cloud` 安装用于 Node.js 的 Watson SDK。
{: javascript}

**注：**确保已使用 `pip install --upgrade watson-developer-cloud` 或 `easy_install --upgrade watson-developer-cloud` 安装用于 Python 的 Watson SDK。
{: python}

**注：**确保已安装 [Watson SDK for Java ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://github.com/watson-developer-cloud/java-sdk/blob/develop/README.md){: new_window}。
{: java}

假定一切正常，那么助手将返回对话输出，然后该输出将显示到控制台：

```
欢迎使用 Watson Assistant 示例！
```
{: screen}

此输出表明，我们已成功与 {{site.data.keyword.conversationshort}} 服务通信，并收到了对话中 conversation_start 节点指定的欢迎消息。现在，我们可以添加用户界面，以便能处理用户输入。

## 处理用户输入以检测意向
{: #api-client-process-input}

为了能够处理用户输入，需要向客户机应用程序添加用户界面。对于此示例，我们将简化工作，使用标准输入和输出。
<span class="ph style-scope doc-content" data-hd-programlang="javascript">可以使用 Node.js prompt-sync 模块来执行此操作。（可以使用 `npm install prompt-sync` 来安装 prompt-sync。）</span>
<span class="ph style-scope doc-content" data-hd-programlang="python">可以使用 Python 3 `input` 函数来执行此操作。</span>
<span class="ph style-scope doc-content" data-hd-programlang="java">可以使用 Java `Console.readLine()` 函数来执行此操作。</span>

```javascript
// 示例 2：添加用户输入并检测意向。

var prompt = require('prompt-sync')();
var AssistantV2 = require('watson-developer-cloud/assistant/v2');

// 设置 Assistant 服务包装器。
var service = new AssistantV2({
  iam_apikey: '{apikey}', // 替换为 API 密钥
  version: '2018-09-20'
});

var assistantId = '{assistant_id}'; // 替换为助手标识
var sessionId;

// 创建会话。
service.createSession({
  assistant_id: assistantId
}, function(err, result) {
  if (err) {
    console.error(err); // 发生了问题
    return;
  }
  sessionId = result.session_id;
  sendMessage(); // 使用空消息启动会话
});

// 向助手发送消息。
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

// 处理响应。
function processResponse(err, response) {
  if (err) {
    console.error(err); // something went wrong
    return;
  }

  // 如果检测到意向，就将其记录到控制台。
  if (response.output.intents.length > 0) {
    console.log('Detected intent: #' + response.output.intents[0].intent);
  }

  // 显示助手的输出（如有）。假定是单个文本响应。
  if (response.output.generic.length != 0) {
    console.log(response.output.generic[0].text);
  }

  // 提示下一轮输入。
  var newMessageFromUser = prompt('>> ');
    if (newMessageFromUser === 'quit') {
      service.deleteSession({
        assistant_id: assistantId,
        session_id: sessionId
      }, function(err, result) {
        if (err) {
    console.error(err); // 发生了问题
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
# 示例 2：添加用户输入并检测意向。

import watson_developer_cloud

# 设置 Assistant 服务。
service = watson_developer_cloud.AssistantV2(
    iam_apikey = '{apikey}', # 替换为 API 密钥
    version = '2018-09-20'
)

assistant_id = '{assistant_id}' # 替换为助手标识

# 创建会话。
session_id = service.create_session(
    assistant_id = assistant_id
).get_result()['session_id']

# 使用空值进行初始化以启动会话。
user_input = ''

# 主输入/输出循环
while user_input != 'quit':

    # 向助手发送消息。
    response = service.message(
        assistant_id,
        session_id,
        input = {
            'text': user_input
    }
  ).get_result()

    # 如果检测到意向，将其显示到控制台。
  if response['output']['intents']:
        print('Detected intent: #' + response['output']['intents'][0]['intent'])

    # 显示对话的输出（如有）。假定是单个文本响应。
    if response['output']['generic']:
        print(response['output']['generic'][0]['text'])

    # 提示进行下一轮输入。
  user_input = input('>> ')
# 操作已完成，将删除会话。
service.delete_session(
    assistant_id = assistant_id,
    session_id = session_id
)
```
{: codeblock }
{: python }

```java
/*
 * 示例 2：添加用户输入并检测意向。
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

    // 禁止日志消息显示在 stdout 中。
    LogManager.getLogManager().reset();

    // 设置 Assistant 服务。
    IamOptions iamOptions = new IamOptions.Builder().apiKey("{apikey}").build();
    Assistant service = new Assistant("2018-09-20", iamOptions);
    String assistantId = "{assistant_id}"; // 替换为助手标识

    // 创建会话。
    CreateSessionOptions createSessionOptions = new CreateSessionOptions.Builder(assistantId).build();
    SessionResponse session = service.createSession(createSessionOptions).execute();
    String sessionId = session.getSessionId();

    // 使用空值进行初始化以启动会话。
    String inputText = "";

    // 主输入/输出循环
    do {
      // 向助手发送消息。
      MessageInput input = new MessageInput.Builder().text(inputText).build();
      MessageOptions messageOptions = new MessageOptions.Builder(assistantId, sessionId)
                                                  .input(input)
                                                  .build();
      MessageResponse response = service.message(messageOptions).execute();

      // 如果检测到意向，就将其显示到控制台。
      List<RuntimeIntent> responseIntents = response.getOutput().getIntents();
      if(responseIntents.size() > 0) {
        System.out.println("Detected intent: #" + responseIntents.get(0).getIntent());
      }

      // 显示对话的输出（如有）。假定是单个文本响应。
    List<DialogRuntimeResponseGeneric> responseGeneric = response.getOutput().getGeneric();
      if(responseGeneric.size() > 0) {
        System.out.println(response.getOutput().getGeneric().get(0).getText());
      }

      // 提示进行下一轮输入。
      System.out.print(">> ");
      inputText = System.console().readLine();
    } while(!inputText.equals("quit"));

    // 操作已完成，将删除会话。
    DeleteSessionOptions deleteSessionOptions = new DeleteSessionOptions.Builder(assistantId, sessionId).build();
    service.deleteSession(deleteSessionOptions).execute();
  }
}
```
{: codeblock }
{: java }

此版本应用程序的开始方式跟之前一样：向助手发送空消息来启动会话。

现在，`processResponse()` 函数会显示对话技能检测到的任何意向以及输出文本。然后，提示进行下一轮的用户输入。
{: javascript }

接下来，显示对话技能检测到的任何意向以及输出文本。然后，提示进行下一轮的用户输入。
{: python }

然后，此函数会显示对话检测到的任何意向以及输出文本，然后提示您进行下一轮的用户输入。
{: java}

我们尚未实现结束会话的自然语言方式，因此将改为使用字面值命令 `quit` 来指示程序应该删除会话并退出。

```
欢迎使用 Watson Assistant 示例！
>> hello
检测到的意向：#hello
您好。
>> what time is it?
检测到的意向：#time
>> goodbye
检测到的意向：#goodbye
好的，再会。
>> quit
```
{: screen}

现在我们已经取得了进步！{{site.data.keyword.conversationshort}} 服务将正确识别我们的意向，并且对话将返回每个意向的正确输出文本（如果提供）。

但是，别的什么也没发生。我们请求返回时间，但没有回答；我们说再见，会话也没有结束。这是因为这些意向需要应用程序执行其他操作。

## 实现应用程序操作
{: #api-client-implement-actions}

除了要向用户显示的输出文本之外，{{site.data.keyword.conversationshort}} 对话还会使用响应 JSON 中的 `actions` 数组，在应用程序需要执行操作时，根据检测到的意向发出信号。对话确定客户机应用程序需要执行某些操作时，会返回 action 对象，其中 `type` 为 `client`。操作的 `name` 指示特定操作 `display_time` 或 `end_conversation`。（操作的其他属性可以指定与操作相关的参数、凭证和其他信息，但对于我们的示例，除了操作名称，不需要其他任何信息。）

我们确信自己的对话绝不会一次请求多个操作，因此，客户机只需检查 `actions` 数组中是否存在单个 `client` 操作即可。如果发现此操作，那么客户机可以执行指定的操作。（此版本也不会显示检测到的意向，因为我们确信可以正确识别这些意向。）

```javascript
// 示例 3：实现应用程序操作。

var prompt = require('prompt-sync')();
var AssistantV2 = require('watson-developer-cloud/assistant/v2');

// 设置 Assistant 服务。
var service = new AssistantV2({
  iam_apikey: '{apikey}', // 替换为 API 密钥
  version: '2018-09-20'
});

var assistantId = '{assistant_id}'; // 替换为助手标识
var sessionId;

// 创建会话。
service.createSession({
  assistant_id: assistantId
}, function(err, result) {
  if (err) {
    console.error(err); // 发生了问题
    return;
  }
  sessionId = result.session_id;
  sendMessage(''); // 使用空消息启动会话
});

// 向助手发送消息。
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

// 处理响应。
function processResponse(err, response) {
  if (err) {
    console.error(err); // something went wrong
    return;
  }

  var endConversation = false;

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
    // 显示助手的输出（如有）。假定是单个文本响应。
    if (response.output.generic.length != 0) {
      console.log(response.output.generic[0].text);
    }
  }

  // 如果未结束，那么提示进行下一轮输入。
  if (!endConversation) {
    var newMessageFromUser = prompt('>> ');
    sendMessage(newMessageFromUser);
  } else {
    service.deleteSession({
      assistant_id: assistantId,
      session_id: sessionId
    }, function(err, result) {
      if (err) {
    console.error(err); // 发生了问题
      }
    });
    return;
  }
}
```
{: codeblock}
{: javascript}

```python
# 示例 3：实现应用程序操作。

import watson_developer_cloud
import time

# 设置 Assistant 服务。
service = watson_developer_cloud.AssistantV2(
    iam_apikey = '{apikey}', # 替换为 API 密钥
    version = '2018-09-20'
)

assistant_id = '{assistant_id}' # 替换为助手标识

# 创建会话。
session_id = service.create_session(
    assistant_id = assistant_id
).get_result()['session_id']

# 使用空值进行初始化以启动会话。
user_input = ''
current_action = ''

# 主输入/输出循环
while current_action != 'end_conversation':
    # 清除先前响应设置的任何操作标志。
    current_action = ''

    # 向助手发送消息。
    response = service.message(
        assistant_id,
        session_id,
        input = {
            'text': user_input
    }
  ).get_result()

    # 显示对话的输出（如有）。假定是单个文本响应。
    if response['output']['generic']:
        print(response['output']['generic'][0]['text'])

    # 检查助手请求的客户机操作。
    if 'actions' in response['output']:
        if response['output']['actions'][0]['type'] == 'client':
            current_action = response['output']['actions'][0]['name']

    # 用户询问现在几点，所以我们输出本地系统时间。
    if current_action == 'display_time':
        print('The current time is ' + time.strftime('%I:%M:%S %p') + '.')
    # 如果未结束，那么提示进行下一轮输入。
    if current_action != 'end_conversation':
    user_input = input('>> ')
# 操作已完成，将删除会话。
service.delete_session(
    assistant_id = assistant_id,
    session_id = session_id
)
```
{: codeblock}
{: python}

```java
/*
 * 示例 3：实现应用程序操作。
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

    // 禁止日志消息显示在 stdout 中。
    LogManager.getLogManager().reset();

    // 设置 Assistant 服务。
    IamOptions iamOptions = new IamOptions.Builder().apiKey("{apikey}").build();
    Assistant service = new Assistant("2018-09-20", iamOptions);
    String assistantId = "{assistant_id}"; // 替换为助手标识

    // 创建会话。
    CreateSessionOptions createSessionOptions = new CreateSessionOptions.Builder(assistantId).build();
    SessionResponse session = service.createSession(createSessionOptions).execute();
    String sessionId = session.getSessionId();

    // 使用空值进行初始化以启动会话。
    String inputText = "";
    String currentAction;

    // 主输入/输出循环
    do {
      // 清除先前响应设置的任何操作标志。
      currentAction = "";

      // 向助手发送消息。
      MessageInput input = new MessageInput.Builder().text(inputText).build();
      MessageOptions messageOptions = new MessageOptions.Builder(assistantId, sessionId)
                                                  .input(input)
                                                  .build();
      MessageResponse response = service.message(messageOptions).execute();

      // 显示对话的输出（如有）。假定是单个文本响应。
      List<DialogRuntimeResponseGeneric> responseGeneric = response.getOutput().getGeneric();
      if(responseGeneric.size() > 0) {
        System.out.println(response.getOutput().getGeneric().get(0).getText());
      }

      // 检查助手请求的任何操作。
      List<DialogNodeAction> responseActions = response.getOutput().getActions();
      if(responseActions != null) {
        if(responseActions.get(0).getActionType().equals("client")) {
          currentAction = responseActions.get(0).getName();
        }
      }

      // 用户询问现在几点，所以我们输出本地系统时间。
    if(currentAction.equals("display_time")) {
        DateTimeFormatter fmt = DateTimeFormatter.ofPattern("h:mm:ss a");
        LocalTime time = LocalTime.now();
        System.out.println("The current time is " + time.format(fmt) + ".");
      }

      // 如果操作未完成，那么提示进行下一轮输入。
      if(!currentAction.equals("end_conversation")) {
        System.out.print(">> ");
        inputText = System.console().readLine();
      }

    } while(!currentAction.equals("end_conversation"));

    // 操作已完成，将删除会话。
    DeleteSessionOptions deleteSessionOptions = new DeleteSessionOptions.Builder(assistantId, sessionId).build();
    service.deleteSession(deleteSessionOptions).execute();
  }
}
```
{: codeblock}
{: java}

现在，应用程序会检查响应中的 `actions` 数组，以了解是否存在 `type` 为 `client` 的操作。如果存在，应用程序会检查该操作的 `name` 值并执行相应的操作（显示本地系统时间或设置指示会话结束的内部标志）。

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

成功！现在，应用程序可使用 {{site.data.keyword.conversationshort}} 服务来识别自然语言输入中的意向，显示相应的响应，并实现所请求的客户机操作。

当然，现实世界应用程序将使用更复杂的用户界面，例如 Web 交谈窗口。它将实现更复杂的操作，可能是与客户数据库或其他业务系统相集成。它还需要向助手发送其他数据，例如用户标识，用于标识每个唯一用户。但是，应用程序如何与 {{site.data.keyword.conversationshort}} 服务进行交互的基本原则将保持不变。

有关一些更复杂的示例，请参阅[样本应用程序](/docs/services/assistant?topic=assistant-sample-apps)。

## 访问上下文
{: #api-client-get-context}

*context* 是一个包含变量的对象，这些变量在整个会话中持久存储，并且可以由对话和客户机应用程序共享。如果应用程序使用的是 V2 API，那么上下文由助手逐个会话自动进行维护。对话和客户机应用程序都可以读取和写入上下文变量。缺省情况下，上下文不会返回给客户机应用程序，但您可以选择请求将其包含在对每个 `/message` 请求的响应中。

**重要信息：**上下文的一个用途是指定与助手进行交互的每个最终用户的唯一用户标识。对于基于用户的套餐，此标识用于计费。（有关更多信息，请参阅[基于用户的套餐](/docs/services/assistant?topic=assistant-services-information#user-based-plans)。）

有两种类型的上下文：

- **全局上下文**：由助手使用的所有技能共享的上下文变量，包括用于管理会话流的内部系统变量。

- **特定于技能的上下文**：特定于某种技能的上下文变量，包括应用程序所需的任何用户定义的变量。目前，仅支持一种技能（名为 `main skill`）。

以下示例显示了包含全局上下文变量和特定于技能的上下文变量的 `/message` 请求；此示例还使用 `options.return_context` 属性来请求随响应返回上下文。

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

    // 使用用户标识创建全局上下文
    MessageContextGlobalSystem system = new MessageContextGlobalSystem();
    system.setUserId("my_user_id");
    MessageContextGlobal globalContext = new MessageContextGlobal();
    globalContext.setSystem(system);

    // 构建用户定义的上下文变量，并为 main skill 放入特定于技能的上下文
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

在此示例请求中，应用程序将 `user_id` 的值指定为全局上下文的一部分。此外，应用程序还将一个用户定义的上下文变量 (`account_number`) 设置为特定于技能的上下文的一部分。此上下文变量可由对话节点作为 `$account_number` 进行访问。（有关在对话中使用上下文的更多信息，请参阅[如何处理对话](/docs/services/assistant?topic=assistant-dialog-runtime)。）

可以指定要用于用户定义的上下文变量的任何变量名称。如果指定的变量已存在，那么将使用新值覆盖该变量；如果不存在，会将新变量添加到上下文中。

此请求的输出不仅包括通常的输出，还包括上下文，其中显示添加了指定值。

```json
{
  "output": {
    "generic":[
      {
        "response_type": "text",
        "text": "欢迎使用 Watson Assistant 示例！"
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

有关如何使用 API 访问上下文变量的详细信息，请参阅 [V2 API 参考 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://{DomainName}/apidocs/assistant-v2#send-user-input-to-assistant){: new_window}。

## 使用 V1 API
{: #api-client-v1-api}

建议使用 V2 API 来构建与 {{site.data.keyword.conversationshort}} 服务进行通信的运行时客户机应用程序。但是，一些较旧的应用程序可能仍在使用 V1 API，V1 API 包含用于在对话技能中向工作空间发送消息的类似运行时方法。

请注意，如果应用程序使用的是 V1 API，那么应用程序会直接与工作空间进行通信，而绕过助手的编排和状态管理功能。这意味着应用程序负责使用上下文来管理状态信息。应用程序必须通过保存随每个响应收到的上下文，并将其随每个新消息请求发送回服务，从而维护上下文。（V1 `/message` 方法始终会随每个响应返回上下文。）

有关 V1 `/message` 方法和上下文的更多信息，请参阅 [V1 API 参考 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://cloud.ibm.com/apidocs/assistant#get-response-to-user-input){: new_window}。
