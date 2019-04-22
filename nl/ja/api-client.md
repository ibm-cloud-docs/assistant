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

# クライアント・アプリケーションの構築
{: #api-client}

作業ダイアログ・スキルとそれを使用するアシスタントが用意できました。ここからは、ユーザーと対話し、{{site.data.keyword.conversationfull}} サービスと通信するカスタム・クライアント・アプリケーションを開発します。
{: shortdesc}

## {{site.data.keyword.conversationshort}} サービスのセットアップ
{: #api-client-setup}

このセクションで作成するアプリケーション例は、コグニティブ・パーソナル・アシスタントの機能をいくつか実装します。 アプリケーション・コードは {{site.data.keyword.conversationshort}} アシスタントに接続し、そこでコグニティブ処理 (ユーザー・インテントの検出など) が行われます。

この例に進む前に、必要なアシスタントを以下のようにしてセットアップする必要があります。

1.  ダイアログ・スキル <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/assistant/assistant-simple-example.json" download="assistant-simple-example.json">JSON ファイル</a>をダウンロードします。
1.  {{site.data.keyword.conversationshort}} サービスのインスタンスに[スキルをインポート](/docs/services/assistant?topic=assistant-skill-add#creating-skills)します。
1.  [アシスタントを作成](/docs/services/assistant?topic=assistant-assistant-add#creating-assistants)し、インポートしたスキルを接続します。

## サービス情報の取得
{: #api-client-get-info}

{{site.data.keyword.conversationshort}} サービスの REST API にアクセスするには、アプリケーションが {{site.data.keyword.Bluemix}} で認証され、適切なアシスタントに接続できる必要があります。 また、サービス資格情報とアシスタント ID をコピーして、アプリケーション・コードに貼り付ける必要があります。

{{site.data.keyword.conversationshort}} ツールからサービス資格情報およびアシスタント ID をアクセスするには、**「アシスタント (Assistants)」**タブに進み、接続するアシスタントの ![メニュー](images/kabob-grey.png) メニューをクリックします。**「API 詳細の表示 (View API Details)」**を選択して、アシスタント ID および API キーを含む、アシスタントの詳細を表示します。

{{site.data.keyword.Bluemix_short}} ダッシュボードからサービス資格情報にアクセスすることもできます。

## {{site.data.keyword.conversationshort}} サービスとの通信
{: #api-client-communicate}

{{site.data.keyword.conversationshort}} サービスとの対話はシンプルです。 次の例を見てみましょう。この例は、サービスに接続し、単一のメッセージを送信して、出力をコンソールに印刷します。

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

最初のステップは、{{site.data.keyword.conversationshort}} サービス用のラッパーを作成することが目的です。

このラッパーは、入力の送信先と出力の受信元として使用するオブジェクトであり、サービスです。 サービス・ラッパーを作成するときは、サービス・キーからの認証資格情報だけでなく、使用する {{site.data.keyword.conversationshort}} API のバージョンも指定します。

この Node.js 例では、ラッパーは変数 `service` に保管されている `AssistantV2` のインスタンスです。 他の言語用の Watson SDK も、サービス・ラッパーをインスタンス化するための同等のメカニズムを備えています。
{: javascript}

この Python  例では、ラッパーは変数 `service` に保管されている `watson_developer_cloud.AssistantV2` のインスタンスです。 他の言語用の Watson SDK も、サービス・ラッパーをインスタンス化するための同等のメカニズムを備えています。
{: python}

この Java 例では、ラッパーは変数 `service` に保管されている `Assistant` のインスタンスです。 他の言語用の Watson SDK も、サービス・ラッパーをインスタンス化するための同等のメカニズムを備えています。
{: java}

サービス・ラッパーを作成した後、それを使用してセッションを作成し、アシスタントにメッセージを送信します。この例では、メッセージは空です。目的はダイアログ内の conversation_start ノードをトリガーすることだけなので、入力テキストは必要ありません。 次に応答テキストをコンソールに出力し、最後にセッションを削除します。

`node <filename.js>` コマンドを使用して、このサンプル・アプリケーションを実行します。
{: javascript}

`python3 <filename.py>` コマンドを使用して、このサンプル・アプリケーションを実行します。
{: python}

ファイル `AssistantSimpleExample.java` にサンプル・コードを貼り付けます。その後、それをコンパイルし、実行することができます。
{: java}

**注:** `npm install watson-developer-cloud` を使用して Node.js 版の Watson SDK がインストールされていることを確認してください。
{: javascript}

**注:** `pip install --upgrade watson-developer-cloud` または `easy_install --upgrade watson-developer-cloud` を使用して Python 版の Watson SDK がインストールされていることを確認してください。
{: python}

**注:** [Watson SDK for Java ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/watson-developer-cloud/java-sdk/blob/develop/README.md){: new_window} がインストールされていることを確認してください。
{: java}

すべてが正常に機能していれば、アシスタントはダイアログからの出力を返し、その出力は次のようにコンソールに印刷されます。

```
Welcome to the Watson Assistant example!
```
{: screen}

この出力は、{{site.data.keyword.conversationshort}} サービスと正常に通信できて、ダイアログ内の conversation_start ノードで指定されたウェルカム・メッセージを受け取ったことを示しています。 これで、ユーザー・インターフェースを追加して、ユーザー入力を処理できるようにすることができるようになりました。

## インテントを検出するためのユーザー入力の処理
{: #api-client-process-input}

ユーザー入力を処理するためには、クライアント・アプリケーションにユーザー・インターフェースを追加する必要があります。 この例では、いろいろなことをシンプルにしておき、標準入出力を使用します。
<span class="ph style-scope doc-content" data-hd-programlang="javascript">そのためには、Node.js prompt-sync モジュールを使用できます。 (prompt-sync は、`npm install prompt-sync` を使用してインストールできます。)</span>
<span class="ph style-scope doc-content" data-hd-programlang="python">そのためには、Python 3 `input` 関数を使用できます。</span>
<span class="ph style-scope doc-content" data-hd-programlang="java">そのためには、Java `Console.readLine()` 関数を使用できます。</span>

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

このバージョンのアプリケーションも、従来と同じように始まっています。つまり、アシスタントに空のメッセージを送信して会話を開始します。

`processResponse()` 関数が、ダイアログ・スキルで検出されたインテントを出力テキストと共に表示します。
その後、ユーザー入力の次のラウンドのプロンプトを出します。
{: javascript }

そうすると、ダイアログ・スキルで検出されたインテントが出力テキストと共に表示され、ユーザー入力の次のラウンドのプロンプトが出されます。
{: python }

この関数はダイアログで検出されたインテントを出力テキストとともに表示するようになり、ユーザー入力の次のラウンドのプロンプトを出します。
{: java}

自然言語を使用して会話を終了する方法はまだ実装されていないため、代わりにリテラル・コマンド `quit` を使用して、プログラムでセッションを削除して終了することを指定します。

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

大分改善されました。 {{site.data.keyword.conversationshort}} サービスはインテントを正しく認識していて、ダイアログはインテントごとに正しい出力テキストを返しています (提供される場合)。

しかし、それ以外のことは起こっていません。 時間を尋ねても答が返ってきません。「goodbye」と言っても会話が終わりません。 これらのインテントについては、アプリが追加のアクションを取る必要があるからです。

## アプリ・アクションの実装
{: #api-client-implement-actions}

出力テキストをユーザーに表示することに加え、{{site.data.keyword.conversationshort}} ダイアログは、検出されたインテントに基づいてアプリケーションがアクションを実行する必要があるときに、応答 JSON 内の `actions` 配列を使用してそのことを通知します。ダイアログは、クライアント・アプリケーションが何かをする必要があると判断すると、`type` が `client` であるアクション・オブジェクトを返します。アクションの `name` は特定のアクション (`display_time` または `end_conversation`) を示します。(アクションの追加プロパティーでは、アクションに関連するパラメーターや資格情報などの情報を指定できますが、この例ではアクション名以外には何も必要ありません。)

ダイアログでは一度に複数のアクションが要求されることはないため、クライアントは `actions` 配列内に単一の `client` アクションが存在するかどうかを確認するだけでよいことになります。見つかった場合は、指定されたアクションを実行できます。(検出されたインテントが正しく識別されていることを確認できたので、このバージョンではさらに、検出されたインテントの表示を削除します。)

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

アプリは応答内の `actions` 配列をチェックして、`type`=`client` であるアクションが存在するかどうかを確認します。存在する場合、アクションの `name` 値をチェックし、適切なアクション (ローカル・システム時刻の表示、または会話が終了したことを示す内部フラグの設定) を実行します。

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

成功です! アプリケーションは {{site.data.keyword.conversationshort}} サービスを使用して、自然言語の入力の中にあるインテントを識別し、該当する応答を表示して、要求されたクライアント・アクションを実装するようになりました。

もちろん、実用アプリケーションの場合は、Web チャット・ウィンドウなど、より洗練されたユーザー・インターフェースを使用することになります。 また、より複雑なアクションを実装し、おそらく、顧客データベースなどのビジネス・システムと統合されることになります。 また、固有の各ユーザーを識別するためのユーザー ID など、追加のデータをアシスタントに送信する必要もあります。しかし、その場合でも、アプリケーションが {{site.data.keyword.conversationshort}} サービスと対話する方法の基本的原則は変わりません。

より複雑な例については、[Sample apps](/docs/services/assistant?topic=assistant-sample-apps を参照してください。

## コンテキストへのアクセス
{: #api-client-get-context}

*コンテキスト*は、会話を通して持続し、ダイアログとクライアント・アプリケーションで共有できる変数を含むオブジェクトです。アプリケーションが v2 API を使用している場合、コンテキストはセッションごとにアシスタントによって自動的に維持されます。ダイアログとクライアント・アプリケーションはどちらもコンテキスト変数を読み書きできます。デフォルトでは、コンテキストはクライアント・アプリケーションに返されませんが、オプションで各 `/message` 要求への応答に含めるよう要求できます。

**重要:** コンテキストの用途の 1 つは、アシスタントと対話するエンド・ユーザーごとに固有のユーザー ID を指定することです。ユーザー・ベース・プランの場合は、この ID が請求処理のために使用されます。(詳細については、[ユーザー・ベース・プラン](/docs/services/assistant?topic=assistant-services-information#user-based-plans)を参照してください)。

コンテキストには、次の 2 つのタイプがあります。

- **グローバル・コンテキスト**: 会話フローを管理するために使用される内部システム変数など、アシスタントによって使用されるすべてのスキルで共有されるコンテキスト変数。

- **スキル固有のコンテキスト**: アプリケーションに必要なユーザー定義変数など、特定のスキルに固有のコンテキスト変数。現在、サポートされているスキルは 1 つ (`main skill`) だけです。

次の例は、グローバル・コンテキスト変数とスキル固有のコンテキスト変数の両方を含む `/message` 要求を示しています。この例では、`options.return_context` プロパティーを使用して、応答と共にコンテキストを返すことも要求しています。

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

この要求例のアプリケーションでは、グローバル・コンテキストの一部として `user_id` の値を指定しています。さらに、スキル固有のコンテキストの一部として、1 つのユーザー定義のコンテキスト変数 (`account_number`) を設定しています。このコンテキスト変数は、`$account_number` としてダイアログ・ノードからアクセスできます。(ダイアログのコンテキストの使用方法については、[ダイアログを処理する方法](/docs/services/assistant?topic=assistant-dialog-runtime)を参照してください。)

ユーザー定義のコンテキスト変数に使用する任意の変数名を指定できます。指定した変数が既に存在する場合は、新しい値で上書きされます。存在しない場合は、新しい変数がコンテキストに追加されます。

この要求からの出力には、通常の出力だけでなく、指定された値が追加されたことを示すコンテキストも含まれています。

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

API を使用したコンテキスト変数にアクセスする方法について詳しくは、[v2 API リファレンス![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://{DomainName}/apidocs/assistant-v2#send-user-input-to-assistant){: new_window}を参照してください。

## v1 API の使用
{: #api-client-v1-api}

{{site.data.keyword.conversationshort}} サービスと通信するランタイム・クライアント・アプリケーションを構築する場合には、v2 API を使用することをお勧めします。ただし、古いアプリケーションの中には依然として v1 API (ダイアログ・スキル内でワークスペースにメッセージを送信するための同様のランタイム・メソッドが含まれる) を使用しているものもあります。

v1 API を使用している場合、アプリはアシスタントのオーケストレーション機能と状態管理機能をバイパスして、ワークスペースと直接通信することに注意してください。つまり、状態情報の管理については、アプリケーション側でコンテキストを使用して行う必要があるということです。アプリケーションは、各応答と共に受け取ったコンテキストを保存し、新しいメッセージ要求ごとにそれをサービスに送り返すことによってコンテキストを維持しなければなりません。(v1 `/message` メソッドは常に各応答と共にコンテキストを返します。)

v1 `/message` メソッドおよびコンテキストについて詳しくは、[v1 API リファレンス![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://cloud.ibm.com/apidocs/assistant#get-response-to-user-input){: new_window}を参照してください。
