---

copyright:
  years: 2015, 2019
lastupdated: "2019-06-04"

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

# 用於存取物件的表示式
{: #expression-language}

您可以使用「Spring 表示式 (SpEL)」語言，來撰寫存取物件及物件內容的表示式。如需相關資訊，請參閱 [Spring 表示式語言 (SpEL) ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](http://docs.spring.io/spring/docs/current/spring-framework-reference/html/expressions.html){: new_window}。
{: shortdesc}

## 評估語法
{: #expression-language-long-syntax}

若要展開其他變數內的變數值，或對內容及廣域物件呼叫方法，請使用 `<? expression ?>` 表示式語法。例如：

- **展開內容**
    - `"output":{"text":"Your name is <? context.userName ?>"}`

- **對廣域物件內容呼叫方法**
    - `"context":{"email": "<? @email.literal ?>"}`

## 速記語法
{: #expression-language-shorthand-syntax}

學習如何使用 SpEL 速記語法來快速參照下列物件：

- [環境定義變數](#expression-language-shorthand-context)
- [實體](#expression-language-shorthand-entities)
- [目的](#expression-language-shorthand-intents)

### 環境定義變數的速記語法
{: #expression-language-shorthand-context}

下表顯示可用來在條件表示式中撰寫環境定義變數的速記語法範例。

|速記語法                |SpEL 中的完整語法   |
|----------------------------|-----------------------------------------|
|`$card_type`               |`context['card_type']`                  |
|`$(card-type)`             |`context['card-type']`                  |
|`$card_type:VISA`          |`context['card_type'] == 'VISA'`        |
|`$card_type:(MASTER CARD)` |`context['card_type'] == 'MASTER CARD'` |

您可以在環境定義變數名稱中包含特殊字元（例如連字號或句點）。不過，在評估 SpEL 表示式時，這麼做可能會導致問題。例如，連字號可能會解譯為減號。若要避免這類問題，請使用完整表示式語法或速記語法 `$(variable-name)` 來參照變數，而且不要在名稱中使用下列特殊字元：

- 括弧 ()
- 多個單引號 ''
- 引號 "

### 實體的速記語法
{: #expression-language-shorthand-entities}

下表顯示在參照實體時可使用的速記語法範例。

|速記語法                |SpEL 中的完整語法   |
|---------------------|------------------------------------------|
|`@year`             |`entities['year']?.value`                |
|`@year == 2016`     |`entities['year']?.value == 2016`        |
|`@year != 2016`     |`entities['year']?.value != 2016`        |
|`@city == 'Boston'` |`entities['city']?.value == 'Boston'`    |
|`@city:Boston`      |`entities['city']?.contains('Boston')`   |
|`@city:(New York)`  |`entities['city']?.contains('New York')` |

在 SpEL 中，問號 `(?)` 會防止在實體物件為空值時觸發空值指標異常狀況。

如果您要檢查的實體值包含 `)` 字元，則無法使用 `:` 運算子進行比較。例如，如果您要檢查城市實體是否為 `Dublin (Ohio)`，則必須使用 `@city == 'Dublin (Ohio)'`，而非 `@city:(Dublin (Ohio))`。

### 目的的速記語法
{: #expression-language-shorthand-intents}

下表顯示在參照目的時可使用的速記語法範例。

<table>
  <caption>目的速記語法</caption>
  <tr>
    <th>速記語法</th>
    <th>SpEL 中的完整語法</th>
  </tr>
  <tr>
    <td>`#help`</td>
    <td>`intent == 'help'`</td>
  </tr>
  <tr>
    <td>`! #help`</td>
    <td>`intent != 'help'`</td>
  </tr>
  <tr>
    <td>`NOT #help`</td>
    <td>`intent != 'help'`</td>
  </tr>
  <tr>
    <td>`#help` 或 `#i_am_lost`</td>
    <td>`(intent == 'help' || intent == 'I_am_lost')`</td>
  </tr>
</table>

## 內建廣域變數
{: #expression-language-builtin-vars}

您可以使用表示式語言來擷取下列廣域變數的內容資訊：

|廣域變數             |定義       |
|----------------------|------------|
|*context*            |已處理交談訊息的 JSON 物件部分。|
|*entities[ ]*        |支援第一個元素之預設存取的實體清單。|
|*input*              |已處理交談訊息的 JSON 物件部分。|
|*intents[ ]*         |支援第一個元素之預設存取的目的清單。|
|*output*             |已處理交談訊息的 JSON 物件部分。|

## 存取實體
{: #expression-language-access-entity}

entities 陣列包含使用者輸入中所辨識的一個以上實體。

測試對話時，您可以藉由在對話節點回應中指定此表示式，來查看使用者輸入中所辨識實體的詳細資料：

```json
<? entities ?>
```
{: codeblock}

針對使用者輸入 *Hello now*，助理會辨識 @sys-date 及 @sys-time 系統實體，因此回應會包含下列實體物件：

```json
[
{"entity":"sys-date","location":[6,9],"value":"2017-08-07",
  "confidence":1,"metadata":{"calendar_type":"GREGORIAN",
  "timezone":"America/New_York"}},
{"entity":"sys-time","location":[6,9],"value":"15:01:00",
  "confidence":1,"metadata":{"calendar_type":"GREGORIAN",
  "timezone":"America/New_York"}}
]
```
{: codeblock}

### 實體在輸入中的放置位置很重要時
{: #expression-language-placement-matters}

當您使用速記表示式 `@city.contains('Boston')` 時，**只有在** `Boston` 是使用者輸入中偵測到的第一個實體時，對話節點才會傳回 true。請您只有在實體在輸入中的放置位置對您而言很重要，而且您只要檢查第一個提及項目時，才使用此語法。

如果您希望在使用者輸入中每次提及詞彙時（不論實體的提及順序為何），條件都傳回 true，請使用完整的 SpEL 表示式。不論如何放置，在所有 @city 實體中至少發現一個 'Boston' 城市實體時，條件 `entities['city']?.contains('Boston')` 會傳回 true。

例如，使用者提交 `"I want to go from Toronto to Boston"`。即會偵測到 `@city:Toronto` 及 `@city:Boston` 實體，並在所傳回的陣列中如下表示：

- `entities.city[0].value = 'Toronto'`
- `entities.city[1].value = 'Boston'`

所傳回陣列中的實體順序符合其在使用者輸入中的提及順序。
{: note}

### 實體內容
{: #expression-language-entity-props}

每一個實體都有一組相關聯的內容。您可以透過實體內容來存取實體的相關資訊。

|內容                  |定義       |用法提示   |
|-----------------------|------------|------------|
|*confidence*          |十進位百分比，代表助理對於已辨識實體的信賴度。除非您已啟動實體的模糊比對，否則實體的信賴度是 0 或 1。已啟用模糊比對時，預設信賴水準臨界值為 0.3。不論是否已啟用模糊比對，系統實體一律具有信賴水準 1.0。|如果信賴水準未高於指定的百分比，您可以在條件中使用此內容，使其傳回 false。|
|*location*            |以零為起始的字元偏移，指出偵測到的實體值在輸入文字中的開始及結束位置。|使用 `.literal`，以擷取 location 內容中所儲存之開始與結束索引值之間的文字段。|
|*value*               |輸入中識別的實體值。|此內容會傳回訓練資料中定義的實體值，即使比對是針對其中一個相關聯的同義字進行也一樣。您可以使用 `.values` 來擷取可能在使用者輸入中多次出現的實體。|

### 實體內容用法範例
{: #expression-language-entity-props-example}

在下列範例中，技能包含內含 JFK 值及同義字 'Kennedy Airport" 的機場實體。使用者輸入是 *I want to go to Kennedy Aiport*。

- 若要在使用者輸入中辨識到 'JFK' 實體時傳回特定回應，您可以將此表示式新增至回應條件：
  `entities.airport[0].value == 'JFK'`
  或
  `@airport = "JFK"`
- 若要傳回使用者在對話回應中所指定的實體名稱，請使用 .literal 內容：
  `So you want to go to <?entities.airport[0].literal?>...`
  或
  `So you want to go to @airport.literal ...`

  在回應中，這兩種格式都會評估為 'So you want to go to Kennedy Airport...'。

- 表示式（如 `@airport:(JFK)` 或 `@airport.contains('JFK')`）一律會參照實體的 **value**（在此範例中為 `JFK`）。
- 若要更嚴格地限制在已啟用模糊比對時將哪些詞彙識別為機場，您可以在節點條件中指定此表示式，例如：`@airport && @airport.confidence > 0.7`。只有在助理對輸入文字包含機場參照的信賴度有 70% 時，才會執行此節點。

在此範例中，使用者輸入是 *Are there places to exchange currency at JFK, Logan, and O'Hare?*

- 若要擷取使用者輸入中多次出現的實體類型，請使用下列這類語法：

    ```json
    "context":{
      "airports":"@airport.values"
    }
    ```

  稍後若要在對話回應中參照擷取的清單，請使用此語法：
  `You asked about these airports: <? $airports.join(', ') ?>.`
  這會顯示如下：
  `You asked about these airports: JFK, Logan, O'Hare.`

## 存取目的
{: #expression-language-intent}

intents 陣列包含使用者輸入中所辨識的一個以上目的，並依信賴度以遞減順序排序。

每一個目的都只有一個內容：`confidence` 內容。confidence 內容是一個十進位百分比，代表助理對於已辨識目的的信賴度。

測試對話時，您可以藉由在對話節點回應中指定此表示式，來查看使用者輸入中所辨識目的的詳細資料：

```json
<? intents ?>
```
{: codeblock}

針對使用者輸入 *Hello now*，助理會尋找含有 #greeting 目的的完全相符項。因此，它會先列出 #greeting 目的物件詳細資料。回應也會包含技能中所定義的前 10 個其他目的，而不論其信賴分數為何。（在此範例中，它對其他目的的信賴度設為 0，因為第一個目的是完全相符項。）會傳回前 10 個目的，因為「試用」窗格會傳送 `alternate_intents:true` 參數及其要求。如果您要直接使用 API，而且想要查看前 10 個結果，則請務必在呼叫中指定此參數。如果 `alternate_intents` 是 false（這是預設值），則只會在陣列中傳回信賴度高於 0.2 的目的。

```json
[{"intent":"greeting","confidence":1},
{"intent":"yes","confidence":0},
{"intent":"pizza-order","confidence":0}]
```
{: codeblock}

下列範例顯示如何檢查目的值：

- `intents[0] == 'Help'`
- `intent == 'Help'`

`intent == 'help'` 與 `intents[0] == 'help'` 不同，因為 `intent == 'help'` 在偵測不到目的時不會擲出異常狀況。只有在目的信賴度超出臨界值時，它才會評估為 true。如果想要的話，您可以指定條件的自訂信賴水準，例如 `intents.size() > 0 && intents[0] == 'help' && intents[0].confidence > 0.1`

## 存取輸入
{: #expression-language-intent-props}

輸入 JSON 物件只包含一個內容：text 內容。text 內容代表使用者輸入的文字。

### 輸入內容用法範例
{: #expression-language-intent-props-example}

下列範例顯示如何存取輸入：

- 若要在使用者輸入為 "Yes" 時執行節點，請將此表示式新增至節點條件：`input.text == 'Yes'`

您可以使用任何[字串方法](/docs/services/assistant/dialog-methods#dialog-methods-strings)來評估或操作使用者輸入中的文字。例如：

- 若要檢查使用者輸入是否包含 "Yes"，請使用：`input.text.contains( 'Yes' )`。
- 如果使用者輸入是數字，則會傳回 true：`input.text.matches( '[0-9]+' )`。
- 若要檢查輸入字串是否包含 10 個字元，請使用：`input.text.length() == 10`。
