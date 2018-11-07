---

copyright:
  years: 2015, 2018
lastupdated: "2018-02-09"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:swift: .ph data-hd-programlang='swift'}

# マルチメディア応答

[{{site.data.keyword.conversationshort}} コネクター](conversation-connector.html)を使用して、ワークスペースを Slack または Facebook Messenger と統合した場合は、クリック可能なボタンなどのマルチメディア要素や対話式要素を含むダイアログ・ノード応答を指定できます。対話式応答を指定するには、JSON データ・ブロックをダイアログ・ノードの出力に挿入します。

**注:** Slack で対話式メッセージを使用する場合は、対話式メッセージのサポートを有効にしたことを確認してください。詳しくは、Slack デプロイメントの [README ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/watson-developer-cloud/conversation-connector/blob/master/channels/slack/README.md#interactive-messages){: new_window} を参照してください。

## 一般的な JSON 形式

対話式応答は、Slack または Facebook Messenger デプロイメントに対応する一般的な JSON 形式を使用して指定できます。一般的な JSON 形式で対話式応答を指定するには、ダイアログ・ノード応答の `output.generic` フィールドに JSON を挿入します。次の例は、複数の応答タイプ (テキスト、イメージ、クリック可能なオプションなど) を含む応答を送信する方法を示しています。

```json
{
  "output": {
    "generic":[
      {
        "response_type": "text",
        "text": "Here are your nearest stores."
      },
      {
        "response_type": "image",
        "source": "http://...",
        "title": "Image title",
        "description": "Some description for the image"
      },
      {
        "response_type": "option",
        "title": "Click on one of the following",
        "options": [
          {
            "label": "Location 1",
            "value:" "Location 1"
          },
          {
            "label": "Location 2",
            "value:" "Location 2"
          },
          {
            "label": "Location 3",
            "value:" "Location 3"
          }
        ]
      }
    ]
  }
}
```

サポートされる応答タイプとその指定方法については、[応答タイプ](#response-types)を参照してください。

実行時には、{{site.data.keyword.conversationshort}} コネクターが、この応答をチャネル (Slack または Facebook Messenger) で予期される形式に変換します。応答に複数のメディア・タイプまたは添付ファイルが含まれている場合、汎用応答は一連の別々のメッセージ・ペイロードに変換されます。そして、コネクターは、各メッセージ・ペイロードを別々のメッセージとしてチャネルに送信します。

**注:** 1 つの応答が複数のメッセージに分割された場合、{{site.data.keyword.conversationshort}} コネクターはそれらのメッセージをチャネルに順次送信します。それらのメッセージをエンド・ユーザーまで届けるのはチャネルの役割であり、これはネットワークやサーバーの問題による影響を受けることがあります。

## ネイティブの JSON 形式

{{site.data.keyword.conversationshort}} コネクターは、一般的な JSON 形式に加えて、Slack や Facebook Messenger のネイティブ形式を使用して作成されたチャネル固有の応答もサポートします。一般的な JSON 形式では現在サポートされていない応答タイプを指定する必要がある場合は、ネイティブの JSON 形式を使用する必要があります。

Slack または Facebook のネイティブの JSON を指定するには、ダイアログ・ノード応答で適切なフィールドを使用します。

- `output.slack`: Slack 応答の `attachment` フィールドに含める JSON を挿入します。Slack の JSON 形式について詳しくは、Slack の [資料 ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://api.slack.com/docs/message-attachments){: new_window} を参照してください。

- `output.facebook`: Facebook 応答の `message.attachment.payload` フィールドに含める JSON を挿入します。Facebook の JSON 形式について詳しくは、Facebook の[資料 ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://developers.facebook.com/docs/messenger-platform/send-messages/templates){: new_window} を参照してください。

## 応答タイプ

一般的な JSON 形式でサポートされている応答タイプは次のとおりです。

### イメージ

URL で指定されたイメージを表示します。

#### フィールド

| 名前          | 型   | 説明                        | 必須かどうか |
|---------------|--------|------------------------------------|-----------|
| response_type | 列挙型     | `image`                    | Y         |
| source        | ストリング | イメージの URL。指定するイメージは、.jpg、.gif、または .png 形式でなければなりません。| Y |
| title         | ストリング | イメージの前に表示するタイトル。| N         |
| description   | ストリング | イメージに付随する説明テキスト。| N |

#### 例

次の例は、イメージとタイトルおよび説明テキストを表示します。

```json
{
  "response_type": "image",
  "source": "http://example.com/image.jpg",
  "title": "Example image",
  "description": "An example image returned as part of a multimedia response."
}
```

### オプション

オプションを選択するためにユーザーがクリックできる一連のボタンを表示します。その後、指定された値がユーザー入力としてワークスペースに送信されます。

#### フィールド

| 名前          | 型   | 説明                         | 必須かどうか |
|---------------|--------|-------------------------------------|-----------|
| response_type | 列挙型     | `option`           | Y     |
| title         | ストリング | オプションの前に表示するタイトル。  | Y     |
| description   | ストリング | オプションに付随する説明テキスト。  | N     |
| options       | リスト     | ユーザーが選択できるオプションを示すキー/値ペアのリスト。| Y |
| options[].label | ストリング | ユーザーに対して表示されるオプションのラベル。| Y     |
| options[].value | ストリング<br/>オブジェクト | ユーザーがオプションを選択した場合に {{site.data.keyword.conversationshort}} サービスに送信される値。これは、ストリング (単純応答の場合) または複数のフィールドを含むオブジェクトのいずれかです。以下の例を参照してください。
| Y |

#### 例

この例は 2 つのオプションを表示します。

- (「Buy something」というラベルの) オプション 1 は、単純なストリング・メッセージ (`option_1`) を送信します。このメッセージはメッセージの `input.text` フィールドを使用してワークスペースに送信されます。
- (「Exit」というラベルの) オプション 2 は、入力テキストとインテント配列の両方が含まれた複合メッセージを送信します。応答には、{{site.data.keyword.conversationshort}} メッセージの有効な部分である任意のフィールドを含めることができます (メッセージ入力の構造について詳しくは、[API リファレンス ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.ibm.com/watson/developercloud/conversation/api/v1/?curl#send_message){: new_window} を参照してください)。

```json
{
  "response_type": "option",
  "title": "Choose from the following options:",
  "options": [
    {
      "label": "Buy something",
      "value": "Place order"
    },
    {
      "label": "Exit",
      "value": {
        "input": {
          "text": "Exit"
        },
        "intent": [
          {
            "intent": "exit_app",
            "confidence": 1.0
          }
        ]
      }
    }
  ]
}
```

### テキスト

テキストを表示します。

#### フィールド

| 名前          | 型   | 説明        | 必須かどうか |
|---------------|--------|--------------------|-----------|
| response_type | 列挙型     | `text`         | Y         |
| text          | ストリング | 表示するテキスト。これには、改行文字 (`\n`) とマークダウン・タグ (チャネルでサポートされている場合) を含めることができます (チャネルでサポートされない形式は無視されます)。| Y |

#### 例

次の例は、ユーザーにテキスト・メッセージを表示します。

```json
{
  "response_type": "text",
  "text": "This is a text response."
}
```
