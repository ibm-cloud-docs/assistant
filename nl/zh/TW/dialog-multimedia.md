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

# 多媒體回應

如果使用 [{{site.data.keyword.conversationshort}} 連接器](conversation-connector.html)來整合您的工作區與 Slack 或 Facebook Messenger，則可以指定包含多媒體或互動式元素（例如可按式按鈕）的對話節點回應。若要指定互動式回應，請將 JSON 資料區塊插入至對話節點的輸出。

**附註：**如果您要搭配使用互動式訊息與 Slack，請確定已啟用互動式訊息支援。如需相關資訊，請參閱 Slack 部署 [README ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/watson-developer-cloud/conversation-connector/blob/master/channels/slack/README.md#interactive-messages){: new_window}。

## 通用 JSON 格式

您可以使用支援 Slack 或 Facebook Messenger 部署的通用 JSON 格式，來指定互動式回應。若要指定通用 JSON 格式的互動式回應，請將 JSON 插入至對話節點回應的 `output.generic` 欄位。下列範例顯示如何傳送包含多種回應類型（文字、影像及可按式選項）的回應：

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

如需所支援回應類型及其指定方式的相關資訊，請參閱[回應類型](#response-types)。

在執行時，{{site.data.keyword.conversationshort}} 連接器會將此回應轉換為頻道（Slack 或 Facebook Messenger）所預期的格式。如果回應包含多種媒體類型或附件，則會將通用回應轉換為一系列的個別訊息有效負載。連接器接著會將每一個訊息有效負載以個別訊息形式傳送至頻道。

**附註：**將回應分割為多個訊息時，{{site.data.keyword.conversationshort}} 連接器會依序將這些訊息傳送至頻道。頻道必須負責將這些訊息遞送給一般使用者；這可能會受到網路或伺服器問題的影響。

## 原生 JSON 格式

除了通用 JSON 格式之外，{{site.data.keyword.conversationshort}} 連接器也支援使用原生 Slack 及 Facebook Messenger 格式所撰寫的頻道特定回應。如果您需要指定通用 JSON 格式目前不支援的回應類型，則可能會想要使用原生 JSON 格式。

您可以使用對話節點回應中的適當欄位，來指定適用於 Slack 或 Facebook 的原生 JSON：

- `output.slack`：在 Slack 回應的 `attachment` 欄位中，插入任何您想要併入的 JSON。如需 Slack JSON 格式的相關資訊，請參閱 Slack [文件 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://api.slack.com/docs/message-attachments){: new_window}。

- `output.facebook`：在 Facebook 回應的 `message.attachment.payload` 欄位中，插入任何您想要併入的 JSON。如需 Facebook JSON 格式的相關資訊，請參閱 Facebook [文件 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://developers.facebook.com/docs/messenger-platform/send-messages/templates){: new_window}。

## 回應類型

通用 JSON 格式支援下列回應類型。

### 影像

顯示 URL 所指定的影像。

#### 欄位

| 名稱          | 類型   | 說明        | 必要？    |
|---------------|--------|------------------------------------|-----------|
| response_type | enum   | `image`                            | Y         |
| source        | 字串   | 影像的 URL。指定的影像格式必須為 .jpg、.gif 或 .png。| Y |
| title         | 字串   | 要在影像前面顯示的標題。| N         |
| description   | 字串   | 影像隨附的說明文字。| N |

#### 範例

此範例顯示具有標題及說明文字的影像。

```json
{
  "response_type": "image",
  "source": "http://example.com/image.jpg",
  "title": "Example image",
  "description": "An example image returned as part of a multimedia response."
}
```

### 選項

顯示使用者可按一下以選擇選項的一組按鈕。指定的值接著會當成使用者輸入傳送至工作區。

#### 欄位

| 名稱          | 類型   | 說明        | 必要？    |
|---------------|--------|-------------------------------------|-----------|
| response_type | 列舉   | `option`                            | Y         |
| title         | 字串   | 要在選項之前顯示的標題。| Y       |
| description   | 字串   | 選項隨附的說明文字。| N |
| options       | 清單   | 鍵值組清單，用以指定使用者可從中選擇的選項。| Y |
| options[].label | 字串   | 選項的面向使用者標籤。| Y     |
| options[].value | 字串   <br/>物件 | 要在使用者選取選項時傳送至 {{site.data.keyword.conversationshort}} 服務的值。這可以是字串（適用於簡單回應）或包含多個欄位的物件。如需範例資訊，請參閱下面。| Y |

#### 範例

此範例顯示兩個選項：

- 選項 1（標示 'Buy something'）傳送簡單字串訊息 (`option_1`)，這會使用訊息的 `input.text` 欄位傳送至工作區。
- 選項 2（標示 'Exit'）傳送同時包含輸入文字及目的陣列的複雜訊息。回應可以包含 {{site.data.keyword.conversationshort}} 訊息有效部分的任何欄位（如需訊息輸入結構的相關資訊，請參閱 [API 參考資料 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://www.ibm.com/watson/developercloud/conversation/api/v1/?curl#send_message){: new_window}。）

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

### 文字

顯示文字。

#### 欄位

| 名稱          | 類型   | 說明        | 必要？    |
|---------------|--------|--------------------|-----------|
| response_type | 列舉   | `text`             | Y         |
| text          | 字串   | 要顯示的文字。這可以包含換行字元 (`\n`) 及 Markdown 標記（如果頻道支援的話）。（會忽略頻道不支援的任何格式化。）| Y |

#### 範例

此範例顯示要給使用者的文字訊息。

```json
{
  "response_type": "text",
  "text": "This is a text response."
}
```
