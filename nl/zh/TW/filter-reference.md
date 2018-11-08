---

copyright:
  years: 2015, 2017
lastupdated: "2017-09-18"

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

# 過濾查詢參照

{{site.data.keyword.conversationshort}} 服務 REST API 透過過濾查詢提供強大的日誌搜尋功能。您可以使用 /logs API `filter` 參數來搜尋工作區日誌，以尋找符合所指定查詢的事件。

`filter` 參數是一個可快取的查詢，用來將結果限制為符合所指定過濾器的結果。您可以過濾任何屬於 JSON 回應模型的物件（例如，使用者輸入文字、偵測到的目的及實體，或信任評分）。

若要查看各種過濾查詢的範例，請參閱[範例](#examples)。

如需 /logs `GET` 方法及其回應模型的相關資訊，請參閱 [API 參考資料 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://www.ibm.com/watson/developercloud/conversation/api/v1/#get_logs){: new_window}。

## 過濾查詢參照

下列範例顯示過濾查詢的一般形式：

| 位置               | 查詢運算子     | 術語         |
|:------------------:|:--------------:|:------------:|
|`request.input.text`|`::`            |`"IBM Watson"`|

- _位置_ 識別您要過濾的欄位（在此範例中，是 `request.input.text`）。
- _查詢運算子_，指定您要使用的相符類型（模糊符合或完全符合）。
- _術語_ 指定您要用來評估欄位進行比對的表示式或值。術語可以包含字面文字及運算子（如[下節](#operators)所述）。

依目的或實體過濾，需要與過濾其他欄位稍微不同的語法。如需相關資訊，請參閱[依目的或實體過濾](#intent_entity_filter)。

**附註：**過濾查詢語法使用 HTTP 查詢中不容許的某些字元。請確定作為 HTTP 查詢一部分傳送時，會以 URL 編碼所有特殊字元（包括空格及引號）。例如，過濾器 `response_timestamp<2016-11-01` 將指定為 `response_timestamp%3C2016-11-01`。

## 運算子
<!-- {#operators} -->

您可以在過濾查詢中使用下列運算子。

| 運算子   | 說明        |
|:-------------------:|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `:` | 模糊符合查詢運算子。如果您要比對任何包含查詢術語或查詢術語之文法變式的值，請在查詢術語的前面加上 `:`。模糊符合適用於使用者輸入文字、回應輸出文字及實體值。|
| `::` | 完全符合查詢運算子。如果您只要比對完全等同於查詢術語的值，請在查詢術語的前面加上 `::`。|
| `:!` | 負模糊符合查詢運算子。如果您只要比對_不_ 包含查詢術語或查詢術語之文法變式的值，請在查詢術語的前面加上 `:!`。|
| `::!` | 負完全符合查詢運算子。如果您只要比對_不_ 完全符合查詢術語的值，請在查詢術語的前面加上 `::!`。|
| `<=`, `>=`, `>`, `<` | 比較運算子。請在查詢術語的前面加上這些運算子，以根據算術比較進行比對。|
| `\` | 跳出運算子。用於包含將剖析為運算子之控制字元的查詢中。例如，`\!hello` 將符合文字 `!hello`。|
| `""` | 文字詞組。用來含括包含空格或其他特殊字元的查詢術語。API 不會剖析引號內的任何特殊字元。|
| `~` | 近似符合。將後面接著 `1` 或 `2` 的這個運算子附加至查詢術語尾端，以指定查詢字串與回應物件中相符項之間容許的單字元差異數目。例如，`car~1` 將符合 `car`、`cat` 或 `cars`，但不符合 `cats`。若過濾 `log_id` 或任何日期或時間欄位，或者使用模糊符合，則此運算子無效。|
| `*` | 萬用字元運算子。比對任何為零個以上字元的序列。若過濾 `log_id` 或任何日期或時間欄位，則此運算子無效。|
| `()`, `[]` | 分組運算子。用來使用布林運算子含括多個表示式的邏輯分組。|
| \| | 布林 _or_ 運算子。|
| `,` | 布林 _and_ 運算子。|

### 依目的或實體過濾
<!-- {#intent_entity_filter} -->

因為在內部儲存目的及實體的方式的差異，所以過濾特定目的或實體的語法與用於所傳回 JSON 中其他欄位的語法不同。若要在 `intents` 或 `entities` 集合內指定 `intent` 或 `entity` 欄位，您必須使用 `:` 比對運算子，而非點。

例如，此查詢比對任何記載的事件，而回應包含偵測到且名為 `hello` 的目的：

`response.intents:intent::hello`

請注意 `:` 運算子取代點 (intents:intent)

使用相同的型樣來比對回應中偵測到之目的或實體的任何欄位。例如，此查詢比對任何記載的事件，而回應包含偵測到且值為 `soda` 的實體：

`response.entities:value::soda`

同樣地，您可以過濾作為要求一部分傳送的目的或實體，如下列範例所示：

`request.intents:intent::hello`

**附註**：過濾在所有偵測到的目的上運作的目的。若只要過濾偵測到且具有最高信任的目的，您可以使用 `response.top_intent` 速記語法，如下列範例所示：

`response.top_intent::goodbye`

### 依其他欄位過濾

若要過濾日誌資料中的任何其他欄位，請將位置指定為從 /logs API 識別 JSON 回應中巢狀物件層次的路徑。使用點 (`.`) 來指定 JSON 資料中連續的巢狀層次。例如，位置 `request.input.text` 識別使用者輸入文字欄位，如下列 JSON 片段所示：

```json
  "request": {
    "input": {
      "text": "Good morning"
    }
  }
```
<!-- {data-copy=false} -->

## 範例

下列範例說明使用此語法的各種類型查詢。

| 說明        | 查詢  |
|---------|-----------|
| 回應的日期是在 2017 年 7 月。| `response_timestamp>=2017-07-01,response_timestamp<2017-08-01` |
| 回應的時間戳記早於 `2016-11-01T04:00:00.000Z`。| `response_timestamp<2016-11-01T04:00:00.000Z` |
| 使用者輸入文字包含字組 "order" 或文法變式（例如，`orders` 或 `ordering`）。| `request.input.text:order` |
| 回應中的目的名稱完全符合 `place_order`。| `response.intents:intent::place_order` |
| 回應中的實體名稱完全符合 `beverage`。| `response.entities:entity::beverage` |
| 使用者輸入文字不包含字糽 "order" 或文法變式。| `request.input.text:!order` |
| 偵測到且具有最高信任的目的名稱不完全符合 `hello`。| `response.top_intent::!hello` |
| 使用者輸入文字包含字串 `!hello`。| `request.input.text:\!hello` |
| 使用者輸入文字包含字串 `IBM Watson`。| `request.input.text:"IBM Watson"` |
| 使用者輸入文字包含的字串與 `Watson` 不得有超過 2 個單字元的差異。| `request.input.text:Watson~2` |
| 使用者輸入文字包含由 `comm` 組成的字串，後面接著零個以上的其他字元，後面接著 `s`。| `request.input.text:comm*s` |
| 環境定義中的部署 ID 符合 `my_app`。| `request.context.metadata.deployment::my_app` |
| 回應中的實體有一個大於 0.8 的信任值。| `response.intents:confidence>0.8` |
| 回應中的目的名稱完全符合 `hello` 或 `goodbye`。| <code>response.intents:intent::(hello\|goodbye)</code> |
| 回應中的目的名稱為 `hello`，且信任值等於或大於 0.8。| `response.intents:(intent:hello,confidence>=0.8)` |
| 回應中的目的名稱完全符合 `order`，且回應中的實體名稱完全符合 `beverage`。| `[response.intents:intent::order,response.entities:entity::beverage]` |
<!-- -->
