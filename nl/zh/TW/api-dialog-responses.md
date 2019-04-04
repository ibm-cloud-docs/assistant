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

# 實作回應
{: #dialog-api-responses}

對話節點可以回應使用者，該回應中包括文字、影像或互動式元素（例如，可按式選項）。如果您要建置自己的用戶端應用程式，則必須實作對話所傳回之所有回應類型的正確顯示。（如需對話回應的相關資訊，請參閱[回應](/docs/services/assistant?topic=assistant-dialog-overview#responses)。）

## 回應輸出格式
{: #dialog-api-responses-output}

依預設，來自對話節點的回應指定於 `output.generic` 物件，該物件位於從 /message API 傳回的回應 JSON 中。`generic` 物件包含最多有 5 個回應元素的陣列，而這些元素適用於任何頻道。下列 JSON 範例顯示一個包含文字及影像的回應：

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

**附註：**如這個範例所示，`output.text` 陣列中也會傳回文字回應 (`OK, here's a picture of a dog.`)。對於不支援 `output.generic` 格式的應用程式，舊版相容性會包含此項。

用戶端應用程式有責任適當地處理所有回應類型。在此情況下，您的應用程式將需要向使用者顯示指定的文字及影像。

## 回應類型
{: #dialog-api-responses-types}

回應的每一個元素都是其中一種支援的回應類型（目前為 `image`、`option`、`pause` 及 `text`）。每一種回應類型都使用一組不同的 JSON 內容來指定，因此，每一個回應所包含的內容會根據回應類型而有所不同。如需有關 /message API 回應模型的完整資訊，請參閱 [API 參考資料 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://cloud.ibm.com/apidocs/assistant?curl=#get-response-to-user-input){: new_window}。

本節說明可用的回應類型，以及它們在 /message API 回應 JSON 中的呈現方式。（如果您使用 Watson SDK，則可以使用針對您的語言所提供的介面來存取相同的物件。）

**附註：**本節中的範例顯示在執行時期從 /message API 所傳回的 JSON 資料的格式。請牢記，這與用來在對話節點內定義回應的 JSON 格式不同。如需相關資訊，請參閱[使用 JSON 編輯器定義回應](/docs/services/assistant?topic=assistant-dialog-responses-json)。

### 影像
{: #dialog-api-responses-image}

`image` 回應類型指示用戶端應用程式顯示影像，並選擇性地隨附標題和說明：

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

您的應用程式負責擷取 `source` 內容所指定的影像，並將它顯示給使用者。如果提供了選用的 `title` 和 `description`，則您的應用程式可以依照任何適當的方式來顯示它們（例如，將影像下的標題和說明呈現為浮動說明）。

### 選項
{: #dialog-api-responses-option}

`option` 回應類型指示用戶端應用程式顯示使用者介面控制項，讓使用者能夠從選項清單中進行選取，然後根據選取的選項，將輸入傳回給 {{site.data.keyword.conversationshort}} 服務：

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

您的應用程式可以使用任何適當的使用者介面控制項（例如，一組按鈕或下拉清單）來顯示指定的選項。選用的 `preference` 內容指出應用程式應使用的偏好控制項類型（`button` 或 `dropdown`）（如果支援的話）。

對於每個選項，`label` 內容指定應該會針對使用者介面控制項中的選項而出現的標籤文字。`value` 內容指定當使用者選取對應的選項時，應該傳回給 {{site.data.keyword.conversationshort}} 服務的輸入（使用 /message API）。請注意，`value` 內容不僅可以包括文字輸入，還可以包括其他輸入物件（例如，目的及實體），全部都應傳送至服務。

### 暫停
{: #dialog-api-responses-pause}

`pause` 回應類型指示應用程式在顯示下一個回應之前等待指定的時間間隔：

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

此暫停可能是對話所要求的，以容許有時間完成要求，或者只是模擬真人服務專員可能在回應之間暫停的現象。暫停時間可長達 10 秒。

通常，`pause` 回應會結合其他回應一起傳送。在顯示陣列中的下一個回應之前，您的應用程式應暫停 `time` 內容所指定的間隔（毫秒）。選用的 `typing` 內容會要求用戶端應用程式顯示「使用者正在鍵入」指示器（如果支援的話），以模擬真人服務專員。

### 文字
{: #dialog-api-responses-text}

`text` 回應類型用於來自對話的一般文字回應：

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

請注意，為了相容於舊版，相同的文字也會併入對話回應的 `output.text` 陣列中（如這個頁面開頭的範例所示）。
