---

copyright:
  years: 2015, 2019
lastupdated: "2019-02-21"

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

# JSON エディターを使用した応答の定義
{: #dialog-responses-json}

場合によっては、JSON エディターを使用して応答を定義する必要が生じることがあります。(ダイアログの応答について詳しくは、[応答](/docs/services/assistant?topic=assistant-dialog-overview#dialog-overview-responses)を参照してください)。応答 JSON を編集すると、通信チャネルまたはカスタム・アプリケーションに返されるデータに直接アクセスできます。

## 一般的な JSON 形式
{: #dialog-responses-json-generic}

応答のための汎用 JSON 形式を使用して、あらゆるチャネルに使用できる応答を指定します。この形式は、Slack と Facebook の統合でサポートされるさまざまな応答タイプに使用できますし、カスタム・クライアント・アプリケーションで実装することもできます。(この形式は、{{site.data.keyword.conversationshort}} ツールを使用して定義されたダイアログ応答でデフォルトで使用されます。)

このツールからダイアログ・ノード応答のために JSON エディターを開く方法については、[JSON エディターでのコンテキスト変数](/docs/services/assistant?topic=assistant-dialog-runtime#dialog-runtime-context-var-json)を参照してください。

一般的な JSON 形式で対話式応答を指定するには、ダイアログ・ノード応答の `output.generic` フィールドに適切な JSON オブジェクトを挿入します。次の例は、複数の応答タイプ (テキスト、イメージ、クリック可能なオプションなど) を含む応答を送信する方法を示しています。

```json
{
  "output": {
    "generic":[
      {
        "response_type": "text",
        "values": [
          {
            "text": "Here are your nearest stores."
          }
        ]
      },
      {
        "response_type": "image",
        "source": "http://example.com/image.jpg",
        "title": "Example image",
        "description": "Some description for the image."
      },
      {
        "response_type": "option",
        "title": "Click on one of the following",
        "options": [
          {
            "label": "Location 1",
            "value": {
              "input": {
                "text": "Location 1"
              }
            }
          },
          {
            "label": "Location 2",
            "value": {
              "input": {
                "text": "Location 2"
              }
            }
          },
          {
            "label": "Location 3",
            "value": {
              "input": {
                "text": "Location 3"
              }
            }
          }
        ]
      }
    ]
  }
}
```

JSON オブジェクトを使用してサポートされる各応答タイプを指定する方法について詳しくは、[応答タイプ](#dialog-responses-json-response-types)を参照してください。

{{site.data.keyword.conversationshort}} コネクターを使用している場合、応答は実行時にチャネル (Slack または Facebook Messenger) で予期される形式に変換します。応答に複数のメディア・タイプまたは添付ファイルが含まれている場合、汎用応答は必要に応じて一連の別々のメッセージ・ペイロードに変換されます。そして、コネクターは、各メッセージ・ペイロードを別々のメッセージとしてチャネルに送信します。

**注:** 1 つの応答が複数のメッセージに分割された場合、{{site.data.keyword.conversationshort}} コネクターはそれらのメッセージをチャネルに順次送信します。 それらのメッセージをエンド・ユーザーまで届けるのはチャネルの役割であり、これはネットワークやサーバーの問題による影響を受けることがあります。

独自のクライアント・アプリケーションを構築する場合、アプリは、該当する各応答タイプを実装する必要があります。詳しくは、[応答の実装](/docs/services/assistant?topic=assistant-api-dialog-responses)を参照してください。

## ネイティブの JSON 形式
{: #dialog-responses-json-native}

ダイアログ・ノード JSON は、一般的な JSON 形式に加えて、Slack や Facebook Messenger のネイティブ形式を使用して作成されたチャネル固有の応答もサポートします。これらの形式は、{{site.data.keyword.conversationshort}} コネクターでもサポートされます。ワークスペースが 1 つのチャネル・タイプでのみ統合されることが分かっていて、一般的な JSON 形式では現在サポートされていない応答タイプを指定する必要がある場合は、ネイティブの JSON 形式を使用する必要があります。

Slack または Facebook のネイティブの JSON を指定するには、ダイアログ・ノード応答で適切なフィールドを使用します。

- `output.slack`: Slack 応答の `attachment` フィールドに含める JSON を挿入します。Slack の JSON 形式について詳しくは、Slack の [資料 ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://api.slack.com/docs/message-attachments){: new_window} を参照してください。

- `output.facebook`: Facebook 応答の `message.attachment.payload` フィールドに含める JSON を挿入します。 Facebook の JSON 形式について詳しくは、Facebook の[資料 ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://developers.facebook.com/docs/messenger-platform/send-messages/templates){: new_window} を参照してください。

## 応答タイプ
{: #dialog-responses-json-response-types}

一般的な JSON 形式でサポートされている応答タイプは次のとおりです。

### イメージ
{: #dialog-responses-json-image}

URL で指定されたイメージを表示します。

#### フィールド
{: #{: #dialog-responses-json-image-fields}

| 名前          | タイプ   | 説明                        | 必須かどうか |
|---------------|--------|------------------------------------|-----------|
| response_type | 列挙型   | `image`                            | Y         |
| source        | ストリング | イメージの URL。 指定するイメージは、.jpg、.gif、または .png 形式でなければなりません。 | Y |
| title         | ストリング | イメージの前に表示するタイトル。| N         |
| description   | ストリング | イメージに付随する説明テキスト。 | N |

#### 例
{: #dialog-responses-json-image-example}

次の例は、イメージとタイトルおよび説明テキストを表示します。

```json
{
  "output": {
    "generic":[
      {
        "response_type": "image",
        "source": "http://example.com/image.jpg",
        "title": "Example image",
        "description": "An example image returned as part of a multimedia response."
      }
    ]
  }
}
```

### オプション
{: #dialog-responses-json-option}

オプションを選択するためにユーザーが使用できる一連のボタンまたはドロップダウン・リストを表示します。その後、指定された値がユーザー入力としてワークスペースに送信されます。

#### フィールド
{: #dialog-responses-json-option-fields}

| 名前          | タイプ   | 説明                         | 必須かどうか |
|---------------|--------|-------------------------------------|-----------|
| response_type | 列挙型   | `option`                            | Y         |
| title         | ストリング | オプションの前に表示するタイトル。 | Y       |
| description   | ストリング | オプションに付随する説明テキスト。 | N |
| preference    | 列挙型   | 優先的に表示されるコントロールのタイプ。ただしチャネルによってサポートされる場合 (`dropdown` または `button`)。現在、{{site.data.keyword.conversationshort}} コネクターは `button` しかサポートしません。| N |
| options       | リスト   | ユーザーが選択できるオプションを示すキー/値ペアのリスト。 | Y |
| options[].label | ストリング | ユーザーに対して表示されるオプションのラベル。 | Y     |
| options[].value | オブジェクト | ユーザーがオプションを選択した場合に {{site.data.keyword.conversationshort}} サービスに送信される応答を定義するオブジェクト。| Y |
| options[].value.input | オブジェクト | オプションに対応する入力テキストを入れる入力オブジェクト。| N |
| options[].value.input.text | ストリング | オプションのために本サービスに送信されるテキスト。| N |

#### 例
{: #dialog-responses-json-option-example}

この例は 2 つのオプションを表示します。

- オプション 1 (`Buy something` というラベル) は、単純なストリング・メッセージ (`Place order`) を送信します。このメッセージは入力テキストとしてワークスペースに送信されます。
- (`Exit` というラベルの) オプション 2 は、入力テキストとインテント配列の両方が含まれた複合メッセージを送信します。 応答には、{{site.data.keyword.conversationshort}} メッセージの有効な部分である任意のフィールドを含めることができます (メッセージ入力の構造について詳しくは、[API リファレンス ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://cloud.ibm.com/apidocs/assistant?curl=#get-response-to-user-input){: new_window} を参照してください)。

```json
{
  "output": {
    "generic":[
      {
        "response_type": "option",
        "title": "Choose from the following options:",
        "preference": "button",
        "options": [
          {
            "label": "Buy something",
            "value": {
              "input": {
                "text": "Place order"
              }
            }
          },
          {
            "label": "Exit",
      "value": {
              "input": {
                "text": "Exit"
              },
              "intents": [
                {
                  "intent": "exit_app",
            "confidence": 1.0
          }
              ]
            }
          }
        ]
      }
    ]
  }
}
```

### 一時停止
{: #dialog-responses-json-pause}

次のメッセージをチャネルに送信する前に一時停止し、オプションで「user is typing」イベントを送信します (それをサポートするチャネルの場合)。

#### フィールド
{: #dialog-responses-json-pause-fields}

| 名前          | タイプ   | 説明        | 必須かどうか |
|---------------|--------|--------------------|-----------|
| response_type | 列挙型   | `pause`            | Y         |
| time          | 整数    | 一時停止する時間 (ミリ秒単位)。| Y |
| typing        | ブール  | 一時停止の際に「user is typing」イベントを送信するかどうか。チャネルがこのイベントをサポートしない場合には無視されます。| N |

#### 例
{: #dialog-responses-json-pause-example}

この例では、5 秒間の一時停止の際に「user is typing」イベントを送信します。

```json
{
  "output": {
    "generic":[
      {
        "response_type": "pause",
        "time": 5000,
        "typing": true
      }
    ]
  }
}
```

### テキスト
{: #dialog-responses-json-text}

テキストを表示します。 種類を増やすために、複数の代替テキスト応答を指定できます。複数の応答を指定する場合、リスト全体の中で順番に繰り返す方法、ランダムに応答を選択する方法、または指定された応答をすべて出力する方法のいずれかを選択できます。

#### フィールド
{: #dialog-responses-json-text-fields}

| 名前          | タイプ   | 説明        | 必須かどうか |
|---------------|--------|--------------------|-----------|
| response_type | 列挙型   | `text`             | Y         |
| values        | リスト   | テキスト応答を定義する 1 つ以上のオブジェクトのリスト。| Y |
| values.[_n_].text   | ストリング | 応答のテキスト。これには、改行文字 (`\n`) とマークダウン・タグ (チャネルでサポートされている場合) を含めることができます (チャネルでサポートされない形式は無視されます)。 | N |
| selection_policy | ストリング | 複数の応答が指定されている場合に、リストから応答を選択する方法。可能な値は、`sequential`、`random`、および `multiline` です。| N |
| delimiter     | ストリング | 複数の応答間の区切り記号として出力する区切り文字。`selection_policy`=`multiline` の場合にのみ使用します。デフォルトの区切り文字は、改行 (`\n`) です。| N |

#### 例
{: #dialog-responses-json-text-example}

次の例は、ユーザーに挨拶のメッセージを表示します。

```json
{
  "output": {
    "generic":[
      {
        "response_type": "text",
        "values": [
          { "text": "Hello." },
          { "text": "Hi there." }
        ],
        "selection_policy": "random"
      }
    ]
  }
}  
```
