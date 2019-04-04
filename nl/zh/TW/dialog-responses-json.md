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

# 使用 JSON 編輯器定義回應
{: #dialog-responses-json}

在某些情況下，您可能需要使用 JSON 編輯器來定義回應。（如需對話回應的相關資訊，請參閱[回應](/docs/services/assistant?topic=assistant-dialog-overview#dialog-overview-responses)。）編輯回應 JSON 可讓您直接存取要傳回給通訊頻道或自訂應用程式的資料。

## 通用 JSON 格式
{: #dialog-responses-json-generic}

回應的通用 JSON 格式用來指定適用於任何頻道的回應。此格式適用於 Slack 及 Facebook 整合所支援的各種回應類型，而且自訂用戶端應用程式也可實作此格式。（依預設，此格式用於使用 {{site.data.keyword.conversationshort}} 工具所定義的對話回應。）

如需如何從工具開啟對話節點回應的 JSON 編輯器的相關資訊，請參閱 [JSON 編輯器中的環境定義變數](/docs/services/assistant?topic=assistant-dialog-runtime#dialog-runtime-context-var-json)。

若要指定通用 JSON 格式的互動式回應，請將適當的 JSON 物件插入至對話節點回應的 `output.generic` 欄位。下列範例顯示如何傳送包含多種回應類型（文字、影像及可按式選項）的回應：

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

如需如何使用 JSON 物件來指定每個支援回應類型的相關資訊，請參閱[回應類型](#dialog-responses-json-response-types)。

如果您使用 {{site.data.keyword.conversationshort}} 連接器，則在執行時期會將回應轉換為頻道（Slack 或 Facebook Messenger）所預期的格式。如果回應包含多種媒體類型或附件，則會視需要將通用回應轉換為一系列的個別訊息有效負載。連接器接著會將每一個訊息有效負載以個別訊息形式傳送至頻道。

**附註：**將回應分割為多個訊息時，{{site.data.keyword.conversationshort}} 連接器會依序將這些訊息傳送至頻道。頻道必須負責將這些訊息遞送給一般使用者；這可能會受到網路或伺服器問題的影響。

如果您要建置自己的用戶端應用程式，您的應用程式必須視情況實作每個回應類型。如需相關資訊，請參閱[實作回應](/docs/services/assistant?topic=assistant-api-dialog-responses)。

## 原生 JSON 格式
{: #dialog-responses-json-native}

除了通用 JSON 格式之外，對話節點 JSON 還支援使用原生 Slack 及 Facebook Messenger 格式所撰寫的頻道特定回應。{{site.data.keyword.conversationshort}} 連接器也支援這些格式。如果您知道工作區只會與一個頻道類型整合，且需要指定通用 JSON 格式目前不支援的回應類型，則可以使用原生 JSON 格式。

您可以使用對話節點回應中的適當欄位，來指定適用於 Slack 或 Facebook 的原生 JSON：

- `output.slack`：插入您要併入 Slack 回應之 `attachment` 欄位的任何 JSON。如需 Slack JSON 格式的相關資訊，請參閱 Slack [文件 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://api.slack.com/docs/message-attachments){: new_window}。

- `output.facebook`：在 Facebook 回應的 `message.attachment.payload` 欄位中，插入任何您想要包含的 JSON。如需 Facebook JSON 格式的相關資訊，請參閱 Facebook [文件 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://developers.facebook.com/docs/messenger-platform/send-messages/templates){: new_window}。

## 回應類型
{: #dialog-responses-json-response-types}

通用 JSON 格式支援下列回應類型。

### 影像
{: #dialog-responses-json-image}

顯示 URL 所指定的影像。

#### 欄位
{: #{: #dialog-responses-json-image-fields}

|名稱          |類型   |說明        |必要？    |
|---------------|--------|------------------------------------|-----------|
|response_type |列舉   |`image`                            |Y     |
|source        |字串   |影像的 URL。指定的影像格式必須為 .jpg、.gif 或 .png。|Y     |
|title         |字串   |要在影像前面顯示的標題。|N |
|description   |字串   |伴隨影像的說明文字。|N |

#### 範例
{: #dialog-responses-json-image-example}

此範例會顯示具有標題及說明文字的影像。

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

### 選項
{: #dialog-responses-json-option}

顯示一組按鈕或下拉清單，使用者可用來選擇選項。指定的值接著會當成使用者輸入傳送至工作區。

#### 欄位
{: #dialog-responses-json-option-fields}

|名稱          |類型   |說明        |必要？    |
|---------------|--------|-------------------------------------|-----------|
|response_type |列舉   |`option`                            |Y     |
|title         |字串   |要在選項之前顯示的標題。|Y     |
|description   |字串   |伴隨選項的說明文字。|N |
| preference    |列舉   | 要顯示的偏好控制類型（如果頻道支援的話），`dropdown` 或 `button`。{{site.data.keyword.conversationshort}} 連接器目前僅支援 `button`。|N |
|options       |清單   |鍵值組清單，用以指定使用者可從中選擇的選項。|Y     |
|options[].label |字串   |使用者看到的選項標籤。|Y     |
|options[].value |物件 | 定義回應的物件，當使用者選取該選項時，會被傳送至 {{site.data.keyword.conversationshort}} 服務。|Y     |
| options[].value.input |物件 | 輸入物件，包括對應至選項的輸入文字。|N |
| options[].value.input.text |字串   | 將針對選項傳送至服務的文字。|N |

#### 範例
{: #dialog-responses-json-option-example}

此範例顯示兩個選項：

- 選項 1（標示為 `Buy something`）會傳送簡單字串訊息 (`Place order`)，其會傳送至工作區作為輸入文字。
- 選項 2（標示為 `Exit`）會傳送複雜訊息，同時包括輸入文字及目的陣列。回應可以包含屬於 {{site.data.keyword.conversationshort}} 訊息有效部分的任何欄位。（如需訊息輸入結構的相關資訊，請參閱 [API 參考資料 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://cloud.ibm.com/apidocs/assistant?curl=#get-response-to-user-input){: new_window}。）

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

### 暫停
{: #dialog-responses-json-pause}

在將下一則訊息傳送至頻道之前暫停，並選擇性地傳送「使用者正在鍵入」事件（適用於支援該事件的頻道）。

#### 欄位
{: #dialog-responses-json-pause-fields}

|名稱          |類型   |說明        |必要？    |
|---------------|--------|--------------------|-----------|
|response_type |列舉   | `pause`            |Y     |
| time          | int    |暫停時間（毫秒）。|Y     |
| typing        | 布林 | 在暫停期間，是否傳送「使用者正在鍵入」事件。如果頻道不支援此事件，則予以忽略。|N |

#### 範例
{: #dialog-responses-json-pause-example}

此範例會傳送「使用者正在鍵入」事件，並同時暫停 5 秒。

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

### 文字
{: #dialog-responses-json-text}

顯示文字。若要新增變化，您可以指定多個替代文字回應。如果您指定多個回應，則可以選擇在清單中循序替換、隨機選擇回應，或輸出所有指定的回應。

#### 欄位
{: #dialog-responses-json-text-fields}

|名稱          |類型   |說明        |必要？    |
|---------------|--------|--------------------|-----------|
|response_type |列舉   |`text`             |Y     |
| values        |清單   | 定義文字回應的一個以上物件的清單。|Y     |
| values.[_n_].text   |字串   | 回應的文字。這可以包含換行字元 (`\n`) 及 Markdown 標記（如果頻道支援的話）。（會忽略頻道不支援的任何格式化。）|N |
| selection_policy |字串   | 如果指定多個回應時，如何從清單中選取回應。可能的值為 `sequential`、`random` 及 `multiline`。|N |
| delimiter     |字串   | 定界字元，輸出為回應之間的分隔字元。只有在 `selection_policy`=`multiline` 時使用。預設的定界字元為換行 (`\n`)。|N |

#### 範例
{: #dialog-responses-json-text-example}

此範例會顯示問候語訊息給使用者。

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
