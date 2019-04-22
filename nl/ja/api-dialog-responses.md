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

# 応答の実装
{: #dialog-api-responses}

ダイアログ・ノードは、テキスト、画像、またはクリック可能なオプションなどのインタラクティブな要素を含む応答でユーザーに応答できます。独自のクライアント・アプリケーションを作成している場合は、ダイアログによって返されるすべての応答タイプを正しく表示するように実装する必要があります。(ダイアログ応答について詳しくは、[応答](/docs/services/assistant?topic=assistant-dialog-overview#responses)を参照してください)。

## 応答出力フォーマット
{: #dialog-api-responses-output}

デフォルトでは、ダイアログ・ノードからの応答は、/message API から返される応答 JSON の `output.generic` オブジェクトで指定されます。`generic` オブジェクトには、任意のチャネルを対象とした最大 5 つの応答要素の配列が含まれています。次の JSON の例は、テキストと画像を含む応答を示しています。

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

**注:** この例が示すように、テキスト応答 (`OK, here's a picture of a dog.`) も `output.text` 配列に入れて返されます。これは、`output.generic` フォーマットをサポートしていないアプリケーションとの下位互換性のために含められています。

クライアント・アプリケーションはすべての応答タイプを適切に処理する必要があります。この例では、アプリケーションは、指定されたテキストおよびイメージをユーザーに表示する必要があります。

## 応答タイプ
{: #dialog-api-responses-types}

応答の各要素は、サポートされている応答タイプ (現時点では、`image`、`option`、`pause`、および `text`) のいずれかです。各応答タイプは、異なる JSON プロパティーのセットを使用して指定されるため、各応答に含まれるプロパティーは応答タイプによって異なります。/message API の応答モデルについて詳しくは、[API リファレンス ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://cloud.ibm.com/apidocs/assistant?curl=#get-response-to-user-input){: new_window} を参照してください)。

このセクションでは、使用可能な応答タイプと、/message API 応答 JSON でのそれらの表記について説明します。(Watson SDK を使用している場合は、ご使用の言語用に提供されているインターフェースを使用して同じオブジェクトにアクセスできます。)

**注:** このセクションの例は、実行時に /message API から返される JSON データの形式を示しています。これはダイアログ・ノード内で応答を定義するために使用される JSON 形式とは異なることに注意してください。詳しくは、[JSON エディターを使用して応答を定義する](/docs/services/assistant?topic=assistant-dialog-responses-json)を参照してください。

### イメージ
{: #dialog-api-responses-image}

`image` 応答タイプは、クライアント・アプリケーションにイメージ (オプションで、タイトルおよび説明を含む) を表示するように指示します。

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

`source` プロパティーで指定されたイメージを取得してユーザーに表示することは、アプリケーション側で行う必要があります。オプションの `title` と `description` が提供されている場合は、アプリケーションで適切な方法でそれらを表示できます (例えば、タイトルをイメージの下に表示し、説明をホバー・テキストとして表示するなど)。

### オプション
{: #dialog-api-responses-option}

`option` 応答タイプは、クライアント・アプリケーションに対して、ユーザーにオプションのリストから選択をさせるためのユーザー・インターフェース・コントロールを表示し、選択されたオプションに基づいて {{site.data.keyword.conversationshort}} サービスに入力を返すように指示します。

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

アプリは、適切なユーザー・インターフェース・コントロール (ボタンのセット、ドロップダウン・リストなど) を使用して、指定されたオプションを表示できます。オプションの `preference` プロパティーは、アプリが使用する優先されるコントロールのタイプ (`button` または `dropdown`) を示します (サポートされている場合)。

オプションごとに、`label` プロパティーが、UI コントロールでそのオプションに対して表示されるラベル・テキストを指定します。`value` プロパティーは、ユーザーが対応するオプションを選択したときに (/message API を使用して) {{site.data.keyword.conversationshort}} サービスに送り返す必要がある入力を指定します。`value` プロパティーには、テキスト入力だけでなく、サービスに送信する必要があるインテントやエンティティーなどの他の入力オブジェクトも含めることができます。

### 一時停止
{: #dialog-api-responses-pause}

`pause` 応答タイプは、次の応答を表示するまで、指定された期間待機するようにアプリケーションに指示します。

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

この一時停止は、要求が完了するまでの時間を見込んで、または単に応答の間に休止が入ることがある人間のエージェントの体裁を装うために、ダイアログによって要求されることがあります。設定可能な一時停止の時間は最大 10 秒です。

`pause` 応答は通常、他の応答と組み合わせて送信されます。アプリケーションは、配列内の次の応答を表示する前に、`time` プロパティーで指定された期間 (ミリ秒単位) 一時停止します。オプションの `typing` プロパティーは、サポートされている場合、人間のエージェントを装うために「ユーザーが入力中」のインジケーターをクライアント・アプリケーションが表示することを要求します。 

### テキスト
{: #dialog-api-responses-text}

`text` 応答タイプは、次のように、ダイアログからの通常のテキスト応答に使用されます。

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

下位互換性のために、ダイアログ応答の `output.text` 配列にも同じテキストが含まれていることに注意してください (このページの冒頭の例を参照)。
