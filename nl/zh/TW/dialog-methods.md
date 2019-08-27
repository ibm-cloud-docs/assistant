---

copyright:
  years: 2015, 2019
lastupdated: "2019-08-12"

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

# 表示式語言方法
{: #dialog-methods}

您可以處理從使用者詞語中擷取的值，而這些使用者詞語是您要在回應的環境定義變數、條件或其他位置中參照的詞語。
{: shortdesc}

## 使用表示式語法的位置
{: #dialog-methods-evaluation-syntax}

若要在其他變數內展開變數值，或將方法套用至輸出文字或環境定義變數，請使用 `<? expression ?>` 表示式語法。例如：

- **在對話節點文字回應中參照使用者輸入**

  ```bash
  You said <? input.text ?>.
  ```
  {: codeblock}

- **從 JSON 編輯器將數值內容增量**

    ```json
    "output":{"number":"<? output.number + 1 ?>"}
    ```
    {: codeblock}

- **從環境定義編輯器將元素新增至環境定義變數陣列**

|環境定義變數名稱|環境定義變數值|
|-----------------------|------------------------|
|`toppings`| `<? context.toppings.append( 'onions' ) ?>` |

您可以在對話節點條件和對話節點回應條件中使用 SpEL 表示式。在條件中使用表示式時，不需要周圍 `<? ?>` 語法。

- **檢查對話節點條件中是否有特定實體值**

  ```bash
  @city.toLowerCase() == 'paris'
  ```
  {: codeblock}

- **檢查對話節點回應條件中是否有特定日期範圍**

  ```bash
  @sys-date.after(today())
  ```
  {: codeblock}

下列各節說明您可用來處理值的方法。它們是依資料類型進行分組：

- [陣列](#dialog-methods-arrays)
- [日期和時間](#dialog-methods-date-time)
- [數字](#dialog-methods-numbers)
- [物件](#dialog-methods-objects)
- [字串](#dialog-methods-strings)

## 陣列
{: #dialog-methods-arrays}

您無法在設定陣列值的相同節點內，在節點條件或回應條件中，使用這些方法來檢查陣列中的值。

### JSONArray.append(object)
{: #dialog-methods-arrays-append}

此方法會將新值附加至 JSONArray，並傳回已修改的 JSONArray。

針對此「對話」運行環境定義：

```json
{
  "context": {
    "toppings_array": ["onion", "olives"]
  }
}
```
{: codeblock}

進行下列更新：

```json
{
  "context": {
    "toppings_array": "<? $toppings_array.append('ketchup', 'tomatoes') ?>"
  }
}
```
{: codeblock}

結果：

```json
{
  "context": {
    "toppings_array": ["onion", "olives", "ketchup", "tomatoes"]
  }
}
```
{: codeblock}

### JSONArray.clear()
{: #dialog-methods-arrays-clear}

此方法可清除陣列中的所有值，並傳回空值。

在輸出中使用下列表示式來定義一個欄位，以清除您儲存至其值的環境定義變數 ($toppings_array) 的陣列。

```json
{
  "output": {
    "array_eraser": "<? $toppings_array.clear() ?>"
  }
}
```
{: codeblock}

如果您後續參照 $toppings_array 環境定義變數，則它只會傳回 '[]'。

### JSONArray.contains(Object value)
{: #dialog-methods-arrays-contains}

如果輸入 JSONArray 包含輸入值，則此方法會傳回 true。

針對前一個節點或外部應用程式所設定的這個「對話」運行環境定義：

```json
{
  "context": {
    "toppings_array": ["onion", "olives", "ham"]
  }
}
```
{: codeblock}

對話節點或回應條件：

```json
$toppings_array.contains('ham')
```
{: codeblock}

結果：`True`，因為陣列包含元素 ham。

### JSONArray.containsIntent(String intent_name, Double min_score, [Integer top_n])
{: #dialog-methods-arrays-containsIntent}

如果 `intents` JSONArray 特別包含指定的目的，且該目的具有的信賴分數等於或高於指定的最低分數，則此方法會傳回 `true`。您可以選擇性地指定一個數目，以指出陣列中該數目的最上層元素內必須包含目的。

如果指定的目的不在陣列中、沒有任何信賴分數等於或高於最低信賴分數，或目的在陣列中低於指定的索引位置，則會傳回 `false`。

服務會自動產生 `intents` 陣列，列出每當提交使用者輸入時服務在輸入中偵測到的目的。此陣列會列出服務偵測到的所有目的，其排列順序為最高信賴度目的放在第一位。

您可以在節點條件中使用此方法，不只檢查是否有目的，還可設定信賴分數臨界值，節點必須符合此臨界值，才能進行處理並傳回其回應。

例如，若您想要只在符合下列條件時才觸發對話節點，請在節點條件中使用下列表示式：

- 存在 `#General_Ending` 目的。
- `#General_Ending` 目的的信賴分數超過 80%。
- `#General_Ending` 目的是目的陣列中前 2 個目的的其中一個。

```bash
intents.containsIntent("General_Ending", 0.8, 2)
```
{: codeblock}

### JSONArray.filter(temp, "temp.property operator comparison_value")
{: #dialog-methods-arrays-filter}

藉由比較每個陣列元素值與您指定的值，來過濾陣列。此方法類似於[集合預測](#dialog-methods-collection-projection)。集合預測會根據陣列元素名稱/值配對中的名稱來傳回已過濾的陣列。過濾方法會根據陣列元素名稱/值配對中的值來傳回已過濾的陣列。

過濾表示式由下列值組成：

- `temp`：評估每個陣列元素時暫時使用的變數名稱。例如，`city`。
- `property`：要與 `comparison_value` 相比較的元素內容。請將內容指定為您在第一個參數中命名的臨時變數的內容。使用語法：`temp.property`。例如，如果 `latitude` 是陣列中名稱/值配對的有效元素名稱，請將內容指定為 `city.latitude`。
- `operator`：用來比較內容值與 `comparison_value` 的運算子。

    支援的運算子為：

    <table>
    <caption>支援的過濾運算子</caption>
    <tr>
      <th>運算子   </th>
      <th>說明</th>
    </tr>
    <tr>
      <td>`==`</td>
      <td>等於</td>
    </tr>
    <tr>
      <td>`>`</td>
      <td>大於</td>
    </tr>
    <tr>
      <td>`<`</td>
      <td>小於</td>
    </tr>
    <tr>
      <td>`>=`</td>
      <td>大於或等於</td>
    </tr>
    <tr>
      <td>`<=`</td>
      <td>小於或等於</td>
    </tr>
    <tr>
      <td>`!=`</td>
      <td>不等於</td>
    </tr>
    </table>

- `comparison_value`：您想要與每個陣列元素內容值比較的值。若要指定可以根據使用者輸入而變更的值，請使用環境定義變數或實體作為值。如果您指定可以改變的值，請新增邏輯以保證在評估時 `comparison_value` 值是有效的，否則會發生錯誤。

#### 過濾範例 1

例如，您可以使用過濾方法來評估一個包含一組城市名稱及其人口數的陣列，以傳回較小的陣列，其中只包含人口超過 5 百萬的城市。

下列 `$cities` 環境定義變數包含物件的陣列。每個物件都包含 `name` 和 `population` 內容。

```json
[
   {
      "name":"Tokyo",
      "population":9273000
   },
   {
      "name":"Rome",
      "population":2868104
   },
   {
      "name":"Beijing",
      "population":20693000
   },
   {
      "name":"Paris",
      "population":2241346
   }
]
```
{: codeblock}

在下列範例中，任意臨時變數名稱為 `city`。SpEL 表示式會過濾 `$cities` 陣列，使其只包含人口超過 5 百萬的城市：

```bash
$cities.filter("city", "city.population > 5000000")
```
{: codeblock}

此表示式會傳回下列過濾的陣列：

```json
[
   {
      "name":"Tokyo",
      "population":9273000
   },
   {
      "name":"Beijing",
      "population":20693000
   }
]
```
{: codeblock}

您可以使用集合預測來建立新的陣列，其中只包含過濾方法所傳回陣列中的城市名稱。然後，您可以使用 `join` 方法，將陣列中的兩個名稱元素值顯示為字串，並以逗點和空格來區隔值。

```bash
The cities with more than 5 million people include <?  T(String).join(", ",($cities.filter("city", "city.population > 5000000")).![name]) ?>.
```
{: codeblock}

產生的回應為：`The cities with more than 5 million people include Tokyo, Beijing.`

#### 過濾範例 2

過濾方法的強大之處在於您不需要將 `comparison_value` 值寫在程式中。在此範例中，寫在程式中的值 5000000 會改以環境定義變數取代。

在此範例中，`$population_min` 環境定義變數包含數字 `5000000`。任意臨時變數名稱為 `city`。SpEL 表示式會過濾 `$cities` 陣列，使其只包含人口超過 5 百萬的城市：

```bash
$cities.filter("city", "city.population > $population_min")
```
{: codeblock}

此表示式會傳回下列過濾的陣列：

```json
[
   {
      "name":"Tokyo",
      "population":9273000
   },
   {
      "name":"Beijing",
      "population":20693000
   }
]
```
{: codeblock}

在比較數值時，請務必設定涉及有效值比較的環境定義變數，然後再觸發過濾方法。請注意，如果與 `null` 比較的陣列元素可能包含它，則它可以是有效值。例如，如果東京的人口名稱/值配對是 `"population":null`，且比較表示式為 `"city.population == $population_min"`，則 `null` 會是 `$population_min` 環境定義變數的有效值。
{: tip}

您可以使用如下的對話節點回應表示式：

```bash
The cities with more than $population_min people include <?  T(String).join(", ",($cities.filter("city", "city.population > $population_min")).![name]) ?>.
```
{: codeblock}

產生的回應為：`The cities with more than 5000000 people include Tokyo, Beijing.`

#### 過濾範例 3

在此範例中，會使用實體名稱作為 `comparison_value`。使用者輸入為 `What is the population of Tokyo?` 任意臨時變數名稱為 `y`。您已建立一個名為 `@city` 的實體，用來辨識城市名稱，包括 `Tokyo`。

```bash
$cities.filter("y", "y.name == @city")
```

此表示式會傳回下列陣列：

```json
[
   {
      "name":"Tokyo",
      "population":9273000
   }
]
```
{: codeblock}

您可以使用集合專案來取得僅具有原始陣列中人口元素的陣列，然後使用 `get` 方法來傳回入口元素的值。

```bash
The population of @city is: <? ($cities.filter("y", "y.name == @city").![population]).get(0) ?>.
```
{: codeblock}

此表示式傳回：`The population of Tokyo is 9273000.`

### JSONArray.get(Integer)
{: #dialog-methods-arrays-get}

此方法會傳回來自 JSONArray 的輸入索引。

針對前一個節點或外部應用程式所設定的這個「對話」運行環境定義：

```json
{
  "context": {
    "name": "John",
    "nested": {
      "array": [ "one", "two" ]
    }
  }
}
```
{: codeblock}

對話節點或回應條件：

```json
$nested.array.get(0).getAsString().contains('one')
```
{: codeblock}

結果：`True`，因為巢狀陣列包含 `one` 作為值。

回應：

```json
"output": {
    "generic":[
      {
        "values": [
        {
        "text" : "The first item in the array is <?$nested.array.get(0)?>"
        }
      ],
      "response_type": "text",
      "selection_policy": "sequential"
    }
  ]
  }
```
{: codeblock}

### JSONArray.getRandomItem()
{: #dialog-methods-arrays-getRandom}

此方法會傳回來自輸入 JSONArray 的隨機項目。

針對此「對話」運行環境定義：

```json
{
  "context": {
    "toppings_array": ["onion", "olives", "ham"]
  }
}
```
{: codeblock}

對話節點輸出：

```json
{
  "output": {
    "generic":[
      {
        "values": [
        {
    "text": "<? $toppings_array.getRandomItem() ?> is a great choice!"
        }
      ],
      "response_type": "text",
      "selection_policy": "sequential"
    }
  ]
  }
}
```
{: codeblock}

結果：`"ham is a great choice!"`、`"onion is a great choice!"` 或 `"olives is a great choice!"`

**附註：**會隨機選擇產生的輸出文字。

### JSONArray.indexOf(value)
{: #dialog-methods-arrays-indexOf}

如果在陣列中找不到值，則此方法會傳回陣列中元素的索引號碼，其符合您指定為參數或 `-1` 的值。值可以是「字串」(`"School"`)、「整數」(`8`) 或「倍精準數」(`9.1`)。值必須完全相符，且區分大小寫。

例如，下列環境定義變數包含陣列：

```json
{
  "context": {
    "array1": ["Mary","Lamb","School"],
    "array2": [8,9,10],
    "array3": [8.1,9.1,10.1]
  }
}
```

下列表示式可用來判定指定值的陣列索引：

```bash
<? $array1.indexOf("Mary") ?> returns `0`
<? $array2.indexOf(9) ?> returns `1`
<? $array3.indexOf(10.1) ?> returns `2`
```

例如，此方法有助於取得目的陣列中的元素索引。您可以將 `indexOf` 方法套用至每次評估使用者輸入時所產生的目的陣列，以判定特定目的的陣列索引號碼。

```bash
intents.indexOf("General_Greetings")
```
{: codeblock}

如果您想要知道特定目的的信賴分數，可以使用語法 `intents[`*`index`*`].confidence`，將上述表示式當作 *`index`* 值傳遞至表示式。例如：

```bash
intents[intents.indexOf("General_Greetings")].confidence
```
{: codeblock}

### JSONArray.join(String delimiter)
{: #dialog-methods-arrays-join}

此方法會將這個陣列中的所有值都結合至字串。值會轉換為字串，並以輸入定界字元分隔。

針對此「對話」運行環境定義：

```json
{
  "context": {
    "toppings_array": ["onion", "olives", "ham"]
  }
}
```
{: codeblock}

對話節點輸出：

```json
{
  "output": {
    "generic":[
      {
        "values": [
        {
    "text": "This is the array: <? $toppings_array.join(';') ?>"
        }
      ],
      "response_type": "text",
      "selection_policy": "sequential"
    }
  ]
  }
}
```
{: codeblock}

結果：

```json
This is the array: onion;olives;ham;
```
{: codeblock}

如果使用者輸入提及多種餡料，並且您已定義名稱為 `@toppings` 的實體用於辨識餡料提及項目，則可以在回應中使用下列表示式來列出前面提到的餡料：

```json
So, you'd like <? @toppings.values.join(',') ?>.
```
{: codeblock}

如果您定義一個變數，將多個值儲存在 JSON 陣列中，則可以從陣列傳回值的子集，然後使用 join() 方法適當地進行格式化。

#### 集合預測
{: #dialog-methods-collection-projection}

`集合預測` SpEL 表示式會從包含物件的陣列中擷取子集合。集合預測的語法為 `array_that_contains_value_sets.![value_of_interest]`。

例如，下列環境定義變數定義用來儲存航班資訊的 JSON 陣列。每個航班有兩個資料點：時間和航班碼。

```json
"flights_found": [
  {
    "time": "10:00",
    "flight_code": "OK123"
  },
  {
    "time": "12:30",
    "flight_code": "LH421"
  },
  {
    "time": "16:15",
    "flight_code": "TS4156"
  }
]
```
{: codeblock}

若只要傳回航班碼，您可以使用下列語法來建立集合預測表示式：

```
<? $flights_found.![flight_code] ?>
```

此表示式會將 `flight_code` 值的陣列傳回為 `["OK123","LH421","TS4156"]`。如需詳細資料，請參閱 [SpEL 集合預測文件](https://docs.spring.io/spring/docs/3.0.x/reference/expressions.html)。

如果您將 `join()` 方法套用至所傳回陣列中的值，則會以逗點區隔清單顯示航班碼。例如，您可以在回應中使用下列語法：

```
The flights that fit your criteria are:
  <? T(String).join(",", $flights_found.![flight_code]) ?>.
```
{: codeblock}

結果：`The flights that match your criteria are: OK123,LH421,TS4156.`

### JSONArray.joinToArray(template)
{: #dialog-methods-arrays-joinToArray}

此方法會將您在範本中定義的格式套用至陣列，並傳回根據規格格式化的陣列。例如，此方法有助於將格式化套用至您要在對話回應中傳回的陣列值。

範本可以指定為「字串」、「JSON 物件」或「JSON 陣列」。若要參照您在範本中編輯的陣列值，請遵循下列語法慣例：

- `%`：代表要從所編輯的陣列中傳回的元素或元素內容的開頭或結尾。
- `e`：暫時代表您要套用格式化的陣列元素。無法從 `e` 變更此臨時變數名稱。

例如，您有一個環境定義變數，其中包含的陣列具有三個航班的航班詳細資料清單。

```json
"flights": [
      {
        "flight": "DL1040",
        "origin": "JFK",
        "carrier": "Alitalia",
        "duration": 485,
        "destination": "FCO",
        "arrival_date": "2019-02-03",
        "arrival_time": "07:00",
        "departure_date": "2019-02-02",
        "departure_time": "16:45"
      },
      {
        "flight": "DL1710",
        "origin": "JFK",
        "carrier": "Delta",
        "duration": 379,
        "destination": "LAX",
        "arrival_date": "2019-02-02",
        "arrival_time": "10:19",
        "departure_date": "2019-02-02",
        "departure_time": "07:00"
      },
      {
        "flight": "DL4379",
        "origin": "BOS",
        "carrier": "Virgin Atlantic",
        "duration": 385,
        "destination": "LHR",
        "arrival_date": "2019-02-03",
        "arrival_time": "09:05",
        "departure_date": "2019-02-02",
        "departure_time": "21:40"
      }
    ]
```
{: codeblock}

您想要只傳回航班碼的清單。若只要從每個陣列中擷取 `flight` 元素的值，並以清單傳回它，您可以使用下列表示式：

```
The available flights are <? $flights.joinToArray("%e.flight%"). ?>
```
{: codeblock}

對話節點回應為 `The available flights are ["DL1040","DL1710","DL4379"].`

若要將陣列顯示為文字，請在表示式中使用 `join` 方法，如下所示：

```
The available flights are <? $flights.joinToArray("%e.flight%").join(", "). ?>
```
{: codeblock}

回應為 `The available flights are DL1040, DL1710, DL4379.`

#### 複雜範本
{: #dialog-methods-complex-template}

若要建立更複雜的範本，而不是直接在方法參數中指定範本詳細資料，您可以建立環境定義變數。

此範本環境定義變數包含陣列元素的子集，並在它們前面新增標籤，因此資訊將顯示在回應的清晰易讀的清單中：

```json
"template": "<br/>Flight number: %e.flight% <br/> Airline: %e.carrier% <br/> Departure date: %e.departure_date% <br/> Departure time: %e.departure_time% <br/> Arrival time: %e.arrival_time% <br/>"
```
{: codeblock}

部分整合頻道（包括 Facebook 及 Slack）*不會* 呈現適用於換行的 `<br/>` HTML 標籤。
{: note}

在對話節點回應中使用此表示式，將 `$template` 中定義的範本套用至 `$flights` 中的陣列。

```
The flight info is <? $flights.joinToArray($template).join(" ") ?>
```
{: codeblock}

回應看起來像這樣：

```
The flight info is
Flight number: DL1040
Airline: Alitalia
Departure date: 2019-02-02
Departure time: 16:45
Arrival time: 07:00

Flight number: DL1710
Airline: Delta
Departure date: 2019-02-02
Departure time: 07:00
Arrival time: 10:19

Flight number: DL4379
Airline: Virgin Atlantic
Departure date: 2019-02-02
Departure time: 21:40
Arrival time: 09:05
```
{: screen}

使用此方法的優點在於，陣列中值的變更頻率，或陣列中的元素數目是否增加，都無關緊要。只要每個陣列元素至少包含範本所參照的內容子集，表示式就可以運作。

#### JSON 物件範本範例
{: #dialog-methods-object-template}

在此範例中，範本環境定義變數會定義為 JSON 物件，從 `$flights` 環境定義變數的陣列中所指定的每個航班元素擷取航班編號，以及抵達和出發的日期和時間。例如，您可以使用此方法，將標準格式化套用至由兩家不同航空公司所管理之航班的航班詳細資料，以及在其 Web 服務中以不同方式將航班資訊格式化的人員。

```json
"template": {
      "departure": "Flight %e.flight% departs on %e.departure_date% at %e.departure_time%.",
      "arrival": "Flight %e.flight% arrives on %e.arrival_date% at %e.arrival_time%."
    }
```
{: codeblock}

您可能想要設計自訂用戶端應用程式，以從傳回的陣列中讀取物件，並適當地將助理的回應值格式化。您的對話節點回應可以使用此表示式，將航班抵達詳細資料物件當作陣列傳回：

```
<? $flights.joinToArray($template) ?>
```
{: screen}

這是對話節點回應：

```json
[
  {
    "arrival":"Flight DL1040 arrives on 2019-02-03 at 07:00.",
    "departure":"Flight DL1040 departs on 2019-02-02 at 16:45."
    },
  {
    "arrival":"Flight DL1710 arrives on 2019-02-02 at 10:19.",
    "departure":"Flight DL1710 departs on 2019-02-02 at 07:00."
    },
  {
    "arrival":"Flight DL4379 arrives on 2019-02-03 at 09:05.",
    "departure":"Flight DL4379 departs on 2019-02-02 at 21:40."
    }
  ]
  ```

請注意，在回應中會交換 `arrival` 和 `departure` 元素的順序。此服務通常會重新排序「JSON 物件」中的元素。如果想要元素依特定順序傳回，請改用「JSON 陣列」或「字串」值來定義範本。

### JSONArray.remove(Integer)
{: #dialog-methods-arrays-remove}

此方法會從 JSONArray 中移除索引位置中的元素，並傳回已更新的 JSONArray。

針對此「對話」運行環境定義：

```json
{
  "context": {
    "toppings_array": ["onion", "olives"]
  }
}
```
{: codeblock}

進行下列更新：

```json
{
  "context": {
    "toppings_array": "<? $toppings_array.remove(0) ?>"
  }
}
```
{: codeblock}

結果：

```json
{
  "context": {
    "toppings_array": ["olives"]
  }
}
```
{: codeblock}

### JSONArray.removeValue(object)
{: #dialog-methods-arrays-removeValue}

此方法會從 JSONArray 中移除第一次出現的值，並傳回已更新的 JSONArray。

針對此「對話」運行環境定義：

```json
{
  "context": {
    "toppings_array": ["onion", "olives"]
  }
}
```
{: codeblock}

進行下列更新：

```json
{
  "context": {
    "toppings_array": "<? $toppings_array.removeValue('onion') ?>"
  }
}
```
{: codeblock}

結果：

```json
{
  "context": {
    "toppings_array": ["olives"]
  }
}
```
{: codeblock}

### JSONArray.set(Integer index, Object value)
{: #dialog-methods-arrays-set}

此方法會將 JSONArray 的輸入索引設為輸入值，並傳回已修改的 JSONArray。

針對此「對話」運行環境定義：

```json
{
  "context": {
    "toppings_array": ["onion", "olives", "ham"]
  }
}
```
{: codeblock}

對話節點輸出：

```json
{
  "context": {
    "toppings_array": "<? $toppings_array.set(1,'ketchup')?>"
  }
}
```
{: codeblock}

結果：

```json
{
  "context": {
    "toppings_array": ["onion", "ketchup", "ham"]
  }
}
```
{: codeblock}

### JSONArray.size()
{: #dialog-methods-arrays-size}

此方法會將 JSONArray 的大小傳回為整數。

針對此「對話」運行環境定義：

```json
{
  "context": {
    "toppings_array": ["onion", "olives"]
  }
}
```
{: codeblock}

進行下列更新：

```json
{
  "context": {
    "toppings_array_size": "<? $toppings_array.size() ?>"
  }
}
```
{: codeblock}

結果：

```json
{
  "context": {
    "toppings_array_size": 2
  }
}
```
{: codeblock}

### JSONArray split(String regexp)
{: #dialog-methods-arrays-split}

此方法使用輸入正規表示式來分割輸入字串。結果是字串的 JSONArray。

針對此輸入：

```
"bananas;apples;pears"
```
{: codeblock}

此語法：

```json
{
  "context": {
    "array": "<?input.text.split(";")?>
  }
}
```
{: codeblock}

此輸出中的結果：

```json
{
  "context": {
    "array": [ "bananas", "apples", "pears" ]
  }
}
```
{: codeblock}

### com.google.gson.JsonArray 支援
{: #dialog-methods-arrays-com-google-gson-JsonArray}

除了內建方法之外，您還可以使用 `com.google.gson.JsonArray` 類別的標準方法。

#### 新陣列
{: #dialog-methods-arrays-new}

new JsonArray().append('value')

若要定義將填入使用者所提供值的新陣列，您可以將陣列實例化。當您將陣列實例化時，也必須將位置保留元值新增至陣列中。您可以使用下列語法來執行這項作業：

```json
{
  "context":{
    "answer": "<? output.answer?:new JsonArray().append('temp_value') ?>"
  }
```
{: codeblock}

## 日期和時間
{: #dialog-methods-date-time}

您可以利用數種方法來處理日期和時間。

如需如何從使用者輸入中辨識及擷取日期和時間資訊的相關資訊，請參閱 [@sys-date 及 @sys-time 實體](/docs/services/assistant?topic=assistant-system-entities#system-entities-sys-date-time)。

### .after(String date or time)
{: #dialog-methods-dates-after}

判定日期/時間值是否在日期/時間引數之後。

### .before(String date or time)
{: #dialog-methods-dates-before}

判定日期/時間值是否在日期/時間引數之前。

例如：
- @sys-time.before('12:00:00')
- @sys-date.before('2016-11-21')

- 如果比較不同的項目（例如 `time vs. date`、`date vs. time` 及 `time vs. date and time`），則此方法會傳回 false，並在回應 JSON 日誌 `output.log_messages` 中列印異常狀況。

  例如，`@sys-date.before(@sys-time)`。
- 如果比較 `date and time vs. time`，則此方法會忽略日期，而僅比較時間。

### now()
{: #dialog-methods-dates-now}

傳回含有現行日期和時間的字串，格式為 `yyyy-MM-dd HH:mm:ss`。

- 靜態函數。
- 您可以對此函數所傳回的日期-時間值呼叫其他日期/時間方法，因此可將其傳入作為其引數。
- 如果已設定環境定義變數 `$timezone`，則此函數會傳回用戶端時區的日期和時間。

輸出欄位中使用 `now()` 的對話節點範例：

```json
{
  "conditions": "#what_time_is_it",
  "output": {
    "generic": [
      {
        "values": [
          {
          "text": "<? now() ?>"
          }
        ],
        "response_type": "text",
        "selection_policy": "sequential"
      }
    ]
  }
}
```
{: codeblock}

節點條件中的 `now()` 範例（判定是否仍是早上）：

```json
{
  "conditions": "now().before('12:00:00')",
  "output": {
    "generic":[
      {
        "values": [
          {
          "text": "Good morning!"
          }
        ],
        "response_type": "text",
        "selection_policy": "sequential"
        }
      ]
  }
}
```
{: codeblock}

### .reformatDateTime(String format)
{: #dialog-methods-dates-reformatDateTime}

將日期和時間字串格式化為使用者輸出所要的格式。

根據指定的格式，傳回格式化的字串：

- `MM/dd/yyyy` 表示 12/31/2016
- `h a` 表示 10pm

若要傳回星期幾，請指定：

- `E` 表示星期二
- `u` 表示日期索引（1 = 星期一、...、7 = 星期日）

例如，此環境定義變數定義會建立 $time 變數，用來將值 17:30:00 儲存為 *5:30 PM*。

```json
{
  "context": {
    "time": "<? @sys-time.reformatDateTime('h:mm a') ?>"
  }
}
```
{: codeblock}

格式遵循 Java [SimpleDateFormat ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](http://docs.oracle.com/javase/6/docs/api/java/text/SimpleDateFormat.html){: new_window} 規則。

**附註**：嘗試僅格式化時間時，會將日期視為 `1970-01-01`。

### .sameMoment(String date/time)
{: #dialog-methods-dates-sameMoment}

- 判定日期/時間值是否與日期/時間引數相同。

### .sameOrAfter(String date/time)
{: #dialog-methods-dates-sameOrAfter}

- 判定日期/時間值是在日期/時間引數之後還是與其相同。
- 類似 `.after()`。

### .sameOrBefore(String date/time)
{: #dialog-methods-dates-sameOrBefore}

- 判定日期/時間值是在日期/時間引數之前還是與其相同。

### today()
{: #dialog-methods-dates-today}

傳回含有現行日期的字串，格式為 `yyyy-MM-dd`。

- 靜態函數。
- 您可以對此函數所傳回的日期值呼叫其他日期方法，並可將其傳入作為引數。
- 如果已設定環境定義變數 `$timezone`，則此函數會傳回用戶端時區的日期。否則，會使用 `GMT` 時區。

在輸出欄位中使用 `today()` 的對話節點範例：

```json
{
  "conditions": "#what_day_is_it",
  "output": {
    "generic": [
      {
        "values": [
          {
          "text": "Today's date is <? today() ?>."
          }
        ],
        "response_type": "text",
        "selection_policy": "sequential"
      }
    ]
  }
}
```
{: codeblock}

結果：`Today's date is 2018-03-09.`

## 日期和時間計算
{: #dialog-methods-date-time-calculations}

使用下列方法來計算日期。

| 方法                  |說明        |
|-------------------------|-------------|
|`<date>.minusDays(n)`| 傳回指定日期之前 n 天的日期。|
|`<date>.minusMonths(n)`| 傳回指定日期之前 n 個月的日期。|
|`<date>.minusYears(n)`| 傳回指定日期之前 n 年的日期。|
|`<date>.plusDays(n)`| 傳回指定日期之後 n 天的日期。|
|`<date>.plusMonths(n)`| 傳回指定日期之後 n 個月的日期。|
|`<date>.plusYears(n)`| 傳回指定日期之後 n 年的日期。|

其中 `<date>` 是以格式 `yyyy-MM-dd` 或 `yyyy-MM-dd HH:mm:ss` 指定。

若要取得明天的日期，請指定下列表示式：

```json
{
  "output": {
    "generic": [
      {
        "values": [
          {
          "text": "Tomorrow's date is <? today().plusDays(1) ?>."
          }
        ],
        "response_type": "text",
        "selection_policy": "sequential"
      }
    ]
  }
}
```
{: codeblock}

如果今天為 2018 年 3 月 9 日，則結果為：`Tomorrow's date is 2018-03-10.`

若要取得從今天算起一週後的日期，請指定下列表示式：

```json
{
  "output": {
    "generic": [
      {
        "values": [
          {
          "text": "Next week's date is <? @sys-date.plusDays(7) ?>."
          }
        ],
        "response_type": "text",
        "selection_policy": "sequential"
      }
    ]
  }
}
```
{: codeblock}

如果 @sys-date 實體所擷取的日期為今天的日期 2018 年 3 月 9 日，則結果為：`Next week's date is 2018-03-16.`

若要取得上個月的日期，請指定下列表示式：

```json
{
  "output": {
    "generic": [
      {
        "values": [
          {
          "text": "Last month the date was <? today().minusMonths(1) ?>."
          }
        ],
        "response_type": "text",
        "selection_policy": "sequential"
      }
    ]
  }
}
```
{: codeblock}

如果今天為 2018 年 3 月 9 日，則結果為：`Last month the date was 2018-02-9.`

使用下列方法來計算時間。

| 方法                  |說明        |
|-------------------------|-------------|
| `<time>.minusHours(n)`   | 傳回指定時間之前 n 小時的時間。|
| `<time>.minusMinutes(n)` | 傳回指定時間之前 n 分鐘的時間。|
| `<time>.minusSeconds(n)`  | 傳回指定時間之前 n 秒的時間。|
| `<time>.plusHours(n)`   | 傳回指定時間之後 n 小時的時間。|
| `<time>.plusMinutes(n)` | 傳回指定時間之後 n 分鐘的時間。|
| `<time>.plusSeconds(n)`  | 傳回指定時間之後 n 秒的時間。|

其中 `<time>` 是以格式 `HH:mm:ss` 指定。

若要取得從現在算起一小時後的時間，請指定下列表示式：

```json
{
  "output": {
    "generic": [
      {
        "values": [
          {
          "text": "One hour from now is <? now().plusHours(1) ?>."
          }
        ],
        "response_type": "text",
        "selection_policy": "sequential"
      }
    ]
  }
}
```
{: codeblock}

如果現在時間是上午 8 點，則結果為：`One hour from now is 09:00:00.`

若要取得 30 分鐘前的時間，請指定下列表示式：

```json
{
  "output": {
    "generic": [
      {
        "values": [
          {
          "text": "A half hour before @sys-time is <? @sys-time.minusMinutes(30) ?>."
          }
        ],
        "response_type": "text",
        "selection_policy": "sequential"
      }
    ]
  }
}
```
{: codeblock}

如果 @sys-time 實體所擷取的時間為上午 8 點，則結果為：`A half hour before 08:00:00 is 07:30:00.`

若要重新格式化傳回的時間，您可以使用下列表示式：

```json
{
  "output": {
    "generic": [
      {
        "values": [
          {
          "text": "6 hours ago was <? now().minusHours(6).reformatDateTime('h:mm a') ?>."
          }
        ],
        "response_type": "text",
        "selection_policy": "sequential"
      }
    ]
  }
}
```
{: codeblock}

如果現在時間是下午 2:19，則結果為：`6 hours ago was 8:19 AM.`

### 使用時間跨距
{: #dialog-methods-time-spans}

若要根據今天的日期是否落在特定時間範圍內來顯示回應，您可以使用時間相關方法的組合。例如，如果您每年都會在聖誕節假期提供特殊優惠，則可以檢查今天的日期是否落在今年的 11 月 25 日與 12 月 24 日之間。首先，定義感興趣的日期作為環境定義變數。

在下列開始和結束日期環境定義變數表示式中，藉由連結動態衍生的現行年份值與寫在程式中的月份和日期值來建構日期。

```json
"context": {
    "end_date": "<? now().reformatDateTime('Y') + '-12-24' ?>",
   "start_date": "<? now().reformatDateTime('Y') + '-11-25' ?>"
 }
```

在回應條件中，您可以指出只有在現行日期落在定義為環境定義變數的開始與結束日期之間時，才要顯示回應。

`now().after($start_date) && now().before($end_date)`

### java.util.Date 支援
{: #dialog-methods-dates-java-util-date}

除了內建方法之外，您還可以使用 `java.util.Date` 類別的標準方法。

若要取得從今天算起一週後某天的日期，您可以使用下列語法。

```json
{
  "context": {
    "week_from_today": "<? new Date(new Date().getTime() +
      (7 * (24*60*60*1000L))) ?>"
  }
}
```
{: codeblock}

此表示式會先取得現行日期（以毫秒為單位，自 1970 年 1 月 1 日 00:00:00 GMT 開始）。它也會計算 7 天的毫秒數（`(24*60*60*1000L)` 代表以毫秒為單位的一天。）它接著會將現行日期加上 7 天。結果是從今天算起一週後某天的完整日期。例如，`Fri Jan 26 16:30:37 UTC 2018`。請注意，時間位於 UTC 時區。您可以隨時將 7 變更為可傳入的變數（例如，`$number_of_days`）。只需要確定其值是在評估此表示式之前所設定。

如果您要接著可以比較日期與服務所產生的另一個日期，則必須將日期重新格式化。系統實體 (`@sys-date`) 及其他內建方法 (`now()`) 會將日期轉換為 `yyyy-MM-dd` 格式。

```json
{
  "context": {
    "week_from_today": "<? new Date(new Date().getTime() +
      (7 * (24*60*60*1000L))).format('yyyy-MM-dd') ?>"
  }
}
```
{: codeblock}

將日期重新格式化之後，結果是 `2018-01-26`。現在，您可以在回應條件中使用 `@sys-date.after($week_from_today)` 這類表示式，來比較使用者輸入中所指定的日期與環境定義變數中所儲存的日期。

下列表示式會計算從現在算起 3 小時的時間。

```json
{
  "context": {
    "future_time": "<? new Date(new Date().getTime() + (3 * (60*60*1000L)) -
      (5 * (60*60*1000L))).format('h:mm a') ?>"
  }
}
```
{: codeblock}

`(60*60*1000L)` 值代表一小時（以毫秒為單位）。此表示式會將現行時間加上 3 小時。然後，會將它減去 5 小時，將 UTC 時區的時間重新計算為 EST 時區的時間。它也會將日期值重新格式化，以包含小時及分鐘 AM 或 PM。

## 數字
{: #dialog-methods-numbers}

這些方法可協助您取得及重新格式化數值。

如需可從使用者輸入辨識及擷取數字之系統實體的相關資訊，請參閱 [@sys-number 實體](/docs/services/assistant?topic=assistant-system-entities#system-entities-sys-number)。

如果您要服務辨識使用者輸入中的特定數字格式（例如訂單號碼參照），請考慮建立型樣實體來進行擷取。如需詳細資料，請參閱[建立實體](/docs/services/assistant?topic=assistant-entities)。

如果，如果您要變更數字的小數位數，將數字重新格式化為貨幣值，請參閱 [String format() 方法](#java.lang.String)。

### toDouble()
{: #dialog-methods-numbers-toDouble}

  將物件或欄位轉換為 Double 數字類型。您可以在任何物件或欄位上呼叫此方法。如果轉換失敗，則會傳回*空值*。

### toInt()
{: #dialog-methods-numbers-toInt}

  將物件或欄位轉換為 Integer 數字類型。您可以在任何物件或欄位上呼叫此方法。如果轉換失敗，則會傳回*空值*。

### toLong()
{: #dialog-methods-numbers-toLong}

  將物件或欄位轉換為 Long 數字類型。您可以在任何物件或欄位上呼叫此方法。如果轉換失敗，則會傳回*空值*。

  如果您在 SpEL 表示式中指定 Long 數字類型，則必須在數字後面加上 `L`，以進行這類識別。例如，`5000000000L`。任何不適用於 32 位元 Integer 類型的數字都需要使用此語法。例如，大於 2^31 (2,147,483,648) 或小於 -2^31 (-2,147,483,648) 的數字都會視為 Long 數字類型。Long 數字類型的最小值為 -2^63，而最大值為 2^63-1。

### Java 數字支援
{: #dialog-methods-numbers-java}

### java.lang.Math()
{: #dialog-methods-numbers-java-lang-math}

執行基本數值運算。

您可以使用 Class 方法，包括下列項目：

- max()

```json
{
  "context": {
    "bigger_number": "<? T(Math).max($number1,$number2) ?>"
  },
  "output": {
    "generic": [
      {
        "values": [
          {
          "text": "The bigger number is $bigger_number."
          }
        ],
        "response_type": "text",
        "selection_policy": "sequential"
      }
    ]
  }
}
```
{: codeblock}

- min()

```json
{
  "context": {
    "smaller_number": "<? T(Math).min($number1,$number2) ?>"
  },
  "output": {
    "generic": [
      {
        "values": [
          {
          "text": "The smaller number is $smaller_number."
          }
        ],
        "response_type": "text",
        "selection_policy": "sequential"
      }
    ]
  }
}
```
{: codeblock}

- pow()

```json
{
  "context": {
    "power_of_two": "<? T(Math).pow($base.toDouble(),2.toDouble()) ?>"
  },
  "output": {
    "generic": [
      {
        "values": [
          {
          "text": "Your number $base to the second power is $power_of_two."
          }
        ],
        "response_type": "text",
        "selection_policy": "sequential"
      }
    ]
  }
}
```
{: codeblock}

如需其他方法的相關資訊，請參閱 [java.lang.Math 參考文件 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://docs.oracle.com/javase/7/docs/api/java/lang/Math.html)。

### java.util.Random()
{: #dialog-methods-numbers-java-util-random}

傳回亂數。您可以使用下列其中一個語法選項：

- 若要傳回隨機布林值（true 或 false），請使用 `<?new Random().nextBoolean()?>`。
- 若要傳回 0（包括）與 1（排除）之間的隨機倍精準數，請使用 `<?new Random().nextDouble()?>`
- 若要傳回 0（包含）與所指定數字之間的隨機整數，請使用 `<?new Random().nextInt(n)?>`，其中 n 是您要 + 1 的數字範圍頂端。
  例如，如果您要傳回介於 0 與 10 之間的亂數，請指定 `<?new Random().nextInt(11)?>`。
- 若要傳回完整整數值範圍（-2147483648 到 2147483648）的隨機整數，請使用 `<?new Random().nextInt()?>`。

例如，您可以建立 #random_number 目的所觸發的對話節點。第一個回應條件看起來可能像這樣：

```json
Condition = @sys-number
{
  "context": {
    "answer": "<? new Random().nextInt(@sys-number.numeric_value + 1) ?>"
  },
  "output": {
    "generic": [
      {
        "values": [
          {
          "text": "Here's a random number between 0 and @sys-number.literal: $answer."
          }
        ],
        "response_type": "text",
        "selection_policy": "sequential"
      }
    ]
  }
}
```
{: codeblock}

如需其他方法的相關資訊，請參閱 [java.util.Random 參考文件](https://docs.oracle.com/javase/7/docs/api/java/util/Random.html)。

您也可以使用下列類別的標準方法：

- `java.lang.Byte`
- `java.lang.Integer`
- `java.lang.Long`
- `java.lang.Double`
- `java.lang.Short`
- `java.lang.Float`

## 物件
{: #dialog-methods-objects}

### JSONObject.clear()
{: #dialog-methods-objects-jsonobject-clear}

此方法可清除 JSON 物件中的所有值，並傳回空值。

例如，您想要清除 $user 環境定義變數中的現行值。

```json
{
  "context": {
    "user": {
      "first_name": "John",
      "last_name": "Snow"
    }
  }
}
```
{: codeblock}

在輸出中使用下列表示式來定義一個欄位，以清除其值的物件。

```json
{
  "output": {
    "object_eraser": "<? $user.clear() ?>"
  }
}
```
{: codeblock}

如果您後續參照 $user 環境定義變數，則它只會傳回 `{}`。

您可以在 API `/message` 呼叫內文中，對 `context` 或 `output` JSON 物件使用 `clear()` 方法。

#### 清除環境定義
{: #dialog-methods-clearing-context}

當您使用 `clear()` 方法來清除 `context` 物件時，它會清除**所有**變數，但下列變數除外：

 - `context.conversation_id`
 - `context.timezone`
 - `context.system`

**警告**：所有環境定義變數值表示：

  - 在現行階段作業期間已觸發的節點中，針對變數所設定的所有預設值。
  - 在現行階段作業期間，利用使用者或外部服務所提供的資訊，對預設值所做的任何更新。

若要使用此方法，您可以在表示式中利用您在輸出物件中定義的變數指定它。例如：

```json
{
  "output": {
    "generic": [
      {
        "values": [
          {
          "text": "Response for this node."
          }
        ],
        "response_type": "text",
        "selection_policy": "sequential"
      }
    ],
    "context_eraser": "<? context.clear() ?>"
  }
}

```

#### 清除輸出
{: #dialog-methods-clearing-output}

當您使用 `clear()` 方法來清除 `output` 物件時，它會清除所有變數，但您用來清除輸出物件的變數，以及您在現行節點中定義的任何文字回應除外。它也不會清除下列變數：

- `output.nodes_visited`
- `output.nodes_visited_details`

若要使用此方法，您可以在表示式中利用您在輸出物件中定義的變數指定它。例如：

```json
{
  "output": {
    "generic": [
      {
        "values": [
          {
          "text": "Have a great day!"
          }
        ],
        "response_type": "text",
        "selection_policy": "sequential"
      }
    ],
    "output_eraser": "<? output.clear() ?>"
  }
}
```

如果樹狀結構中先前的節點定義文字回應 `I'm happy to help.`，然後跳至具有上述定義之 JSON 輸出物件的節點，則只有 `Have a great day.` 會顯示為回應。不會顯示 `I'm happy to help.` 輸出，因為已清除它，並已取代為呼叫 `clear()` 方法之節點中的文字回應。

### JSONObject.has(String)
{: #dialog-methods-objects-jsonobject-has}

如果複雜 JSONObject 具有輸入名稱的內容，則此方法會傳回 true。

針對此「對話」運行環境定義：

```json
{
  "context": {
    "user": {
      "first_name": "John",
      "last_name": "Snow"
    }
  }
}
```
{: codeblock}

對話節點輸出：

```json
{
  "conditions": "$user.has('first_name')"
}
```
{: codeblock}

結果：條件是 true，因為使用者物件包含 `first_name` 內容。

### JSONObject.remove(String)
{: #dialog-methods-objects-jsonobject-remove}

此方法會從輸入 `JSONObject` 中移除名稱的內容。此方法所傳回的 `JSONElement` 是要被移除的 `JSONElement`。

針對此「對話」運行環境定義：

```json
{
  "context": {
    "user": {
      "first_name": "John",
      "last_name": "Snow"
    }
  }
}
```
{: codeblock}

對話節點輸出：

```json
{
  "context": {
    "attribute_removed": "<? $user.remove('first_name') ?>"
  }
}
```
{: codeblock}

結果：

```json
{
  "context": {
    "user": {
      "last_name": "Snow"
    },
    "attribute_removed": {
      "first_name": "John"
    }
  }
}
```
{: codeblock}

### com.google.gson.JsonObject 支援
{: #dialog-methods-objects-com-google-gson-JsonObject}

除了內建方法之外，您還可以使用 `com.google.gson.JsonObject` 類別的標準方法。

## 字串
{: #dialog-methods-strings}

這些方法可協助您處理文字。

如需如何從使用者輸入中辨識及擷取特定類型之字串的相關資訊（例如人名及位置），請參閱[系統實體](/docs/services/assistant?topic=assistant-system-entities)。

**附註：**針對涉及正規表示式的方法，請參閱 [RE2 語法參考資料 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/google/re2/wiki/Syntax){: new_window}，以取得要在指定正規表示式時使用之語法的詳細資料。

### String.append(Object)
{: #dialog-methods-strings-append}

此方法會將輸入物件作為字串附加至字串，並傳回已修改的字串。

針對此「對話」運行環境定義：

```json
{
  "context": {
    "my_text": "This is a text."
  }
}
```
{: codeblock}

此語法：

```json
{
  "context": {
    "my_text": "<? $my_text.append(' More text.') ?>"
  }
}
```
{: codeblock}

此輸出中的結果：

```json
{
  "context": {
    "my_text": "This is a text. More text."
  }
}
```
{: codeblock}

### String.contains(String)
{: #dialog-methods-strings-contains}

如果字串包含輸入子字串，則此方法會傳回 true。

輸入："Yes, I'd like to go."

此語法：

```json
{
  "conditions": "input.text.contains('Yes')"
}
```
{: codeblock}

結果：條件是 `true`。

### String.endsWith(String)
{: #dialog-methods-strings-endsWith}

如果字串的結尾是輸入子字串，則此方法會傳回 true。

針對此輸入：

```
"What is your name?".
```
{: codeblock}

此語法：

```json
{
  "conditions": "input.text.endsWith('?')"
}
```
{: codeblock}

結果：條件是 `true`。

### String.extract(String regexp, Integer groupIndex)
{: #dialog-methods-strings-extract}

此方法會傳回輸入中與指定的正規表示式群組型樣相符的字串。如果找不到相符項，則會傳回空字串。

此方法旨在擷取不同正規表示式型樣群組的相符項，而不是擷取單一正規表示式型樣的不同相符項。若要尋找不同的相符項，請參閱 [getMatch](#dialog-methods-strings-getMatch) 方法。
{: note}

在此範例中，環境定義變數將儲存與指定的正規表示式型樣群組相符的字串。在表示式中，定義兩個正規表示式型樣群組，並以括弧括住每個群組。這會自然產生第三個群組，此群組同時包含這兩個群組。第一個正規表示式群組 (groupIndex 0) 與同時包含完整數字群組和文字群組的字串相符。第二個正規表示式群組 (groupIndex 1) 與第一次出現的數字群組相符。第三個群組 (groupIndex 2) 與數字群組後第一次出現的文字群組相符。

```json
{
  "context": {
    "number_extract": "<? input.text.extract('([\\d]+)(\\b [A-Za-z]+)',n) ?>"
  }
}
```
{: codeblock}

指定 JSON 格式的正規表示式時，必須提供兩個反斜線 (\\)。如果在節點回應中指定此表示式，則只需要一個反斜線。例如： 

`<? input.text.extract('([\d]+)(\b [A-Za-z]+)',n) ?>`

輸入：

```
"Hello 123 this is 456".
```
{: codeblock}

結果：

- n=`0` 時，值為 `123 this`。
- n=`1` 時，值為 `123`。
- n=`2` 時，值為 `this`。

### String.find(String regexp)
{: #dialog-methods-strings-find}

如果字串的任何區段符合輸入正規表示式，則此方法會傳回 true。您可以針對 JSONArray 或 JSONObject 元素呼叫此方法，它會在進行比較之前，將陣列或物件轉換為字串。

針對此輸入：

```
"Hello 123456".
```
{: codeblock}

此語法：

```json
{
  "conditions": "input.text.find('^[^\d]*[\d]{6}[^\d]*$')"
}
```
{: codeblock}

結果：條件是 true，因為輸入文字的數值部分符合正規表示式 `^[^\d]*[\d]{6}[^\d]*$`。

### String.getMatch(String regexp, Integer matchIndex)
{: #dialog-methods-strings-getMatch}

此方法會傳回輸入中與出現的指定正規表示式型樣相符的字串。如果找不到相符項，則此方法會傳回空字串。

找到相符項時，會將其新增到您可以視為*相符項陣列* 的內容。如果您要傳回第三個相符項，則因為陣列元素計數從 0 開始，所以請指定 2 作為 `matchIndex` 值。例如，如果輸入的字串包含與指定型樣相符的三個字組，則只能藉由指定其索引值來傳回第一個、第二個或第三個相符項。

在下列表示式中，您將在輸入中尋找一組數字。此表示式會將第二個型樣相符字串儲存到 `$second_number` 環境定義變數中，因為已指定索引值 1。

```json
{
  "context": {
    "second_number": "<? input.text.getMatch('([\\d]+)',1) ?>"
  }
}
```
{: codeblock}

如果您以 JSON 語法指定表示式，則必須提供兩個反斜線 (\\)。如果在節點回應中指定該表示式，則只需要一個反斜線。 

例如： 

`<? input.text.getMatch('([\d]+)',1) ?>`

- 使用者輸入：

  ```
  "hello 123 i said 456 and 8910".
  ```
  {: codeblock}

- 結果：`456`

在此範例中，表示式會在輸入中尋找第三個文字區塊。

`<? input.text.getMatch('(\b [A-Za-z]+)',2) ?>`

對於相同的使用者輸入，此表示式會傳回 `and`。

### String.isEmpty()
{: #dialog-methods-strings-isEmpty}

如果字串是空字串，而非空值，則此方法會傳回 true。

針對此「對話」運行環境定義：

```json
{
  "context": {
    "my_text_variable": ""
  }
}
```
{: codeblock}

此語法：

```json
{
  "conditions": "$my_text_variable.isEmpty()"
}
```
{: codeblock}

結果：條件是 `true`。

### String.length()
{: #dialog-methods-strings-length}

此方法會傳回字串的字元長度。

針對此輸入：

```
"Hello"
```
{: codeblock}

此語法：

```json
{
  "context": {
    "input_length": "<? input.text.length() ?>"
  }
}
```
{: codeblock}

此輸出中的結果：

```json
{
  "context": {
    "input_length": 5
  }
}
```
{: codeblock}

### String.matches(String regexp)
{: #dialog-methods-strings-matches}

如果字串符合輸入正規表示式，則此方法會傳回 true。

針對此輸入：

```
"Hello".
```
{: codeblock}

此語法：

```json
{
  "conditions": "input.text.matches('^Hello$')"
}
```
{: codeblock}

結果：條件是 true，因為輸入文字符合正規表示式 `\^Hello\$`。

### String.startsWith(String)
{: #dialog-methods-strings-startsWith}

如果字串的開頭是輸入子字串，則此方法會傳回 true。

針對此輸入：

```
"What is your name?".
```
{: codeblock}

此語法：

```json
{
  "conditions": "input.text.startsWith('What')"
}
```
{: codeblock}

結果：條件是 `true`。

### String.substring(Integer beginIndex, Integer endIndex)
{: #dialog-methods-strings-substring}

此方法會從位於 `beginIndex` 的字元取得子字串，且最後一個字元設為 `endIndex` 之前的索引。不包含 endIndex 字元。

針對此「對話」運行環境定義：

```json
{
  "context": {
    "my_text": "This is a text."
  }
}
```
{: codeblock}

此語法：

```json
{
  "context": {
    "my_text": "<? $my_text.substring(5, $my_text.length()) ?>"
  }
}
```
{: codeblock}

此輸出中的結果：

```json
{
  "context": {
    "my_text": "is a text."
  }
}
```
{: codeblock}

### String.toLowerCase()
{: #dialog-methods-strings-toLowerCase}

此方法會傳回轉換為小寫字母的原始字串。

針對此輸入：

```
"This is A DOG!"
```
{: codeblock}

此語法：

```json
{
  "context": {
    "input_lower_case": "<? input.text.toLowerCase() ?>"
  }
}
```
{: codeblock}

此輸出中的結果：

```json
{
  "context": {
    "input_lower_case": "this is a dog!"
  }
}
```
{: codeblock}

### String.toUpperCase()
{: #dialog-methods-strings-toUpperCase}

此方法會傳回轉換為大寫字母的原始字串。

針對此輸入：

```
"hi there".
```
{: codeblock}

此語法：

```json
{
  "context": {
    "input_upper_case": "<? input.text.toUpperCase() ?>"
  }
}
```
{: codeblock}

此輸出中的結果：

```json
{
  "context": {
    "input_upper_case": "HI THERE"
  }
}
```
{: codeblock}

### String.trim()
{: #dialog-methods-strings-trim}

此方法會修除字串開頭及結尾的任何空格，並傳回已修改的字串。

針對此「對話」運行環境定義：

```json
{
  "context": {
    "my_text": "   something is here    "
  }
}
```
{: codeblock}

此語法：

```json
{
  "context": {
    "my_text": "<? $my_text.trim() ?>"
  }
}
```
{: codeblock}

此輸出中的結果：

```json
{
  "context": {
    "my_text": "something is here"
  }
}
```
{: codeblock}

### java.lang.String 支援
{: #dialog-methods-strings-java-lang-String-format}

除了內建方法之外，您還可以使用 `java.lang.String` 類別的標準方法。

#### java.lang.String.format()
{: #dialog-methods-strings-java-lang-String-format}

您可以將標準 Java 字串的 `format()` 方法套用至文字。如需用來指定格式詳細資料之語法的相關資訊，請參閱 [java.util.formatter 參考資料 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://docs.oracle.com/javase/7/docs/api/java/util/Formatter.html#syntax){: new_window}。

例如，下列表示式會接受三個十進位整數（1、1 及 2），並將它們新增至句子中。

```json
{
  "formatted String": "<? T(java.lang.String).format('%d + %d equals %d', 1, 1, 2) ?>"
}
```
{: codeblock}

結果：`1 + 1 equals 2`。

若要變更數字的小數位數，請使用下列語法：

```
{
  <? T(String).format('%.2f',<number to format>) ?>
}
```
{: codeblock}

例如，如果需要以美元格式化的 $number 變數是 `4.5`，則 `Your total is $<? T(String).format('%.2f',$number) ?>` 這類回應會傳回 `Your total is $4.50.`

## 間接資料類型轉換
{: #dialog-methods-indirect-type-conversion}

例如，當您在文字內包含表示式以作為節點回應的一部分時，值會呈現為字串。如果您要以原始資料類型來呈現表示式，則周圍請不要有文字。

例如，您可以將此表示式新增至對話節點回應，以字串格式傳回使用者輸入中所辨識的實體：

```json
The entities are <? entities ?>.
```
{: codeblock}

如果使用者將 *Hello now* 指定為輸入，則 `now` 參照會觸發 @sys-date 及 @sys-time 實體。entities 物件是陣列，但因為以文字包含表示式，所以會以字串格式傳回實體，如下所示：

```json
  The entities are 2018-02-02, 14:34:56.
```
{: codeblock}

如果您未在回應中包含文字，則會改為傳回陣列。例如，如果只將回應指定為表示式，且周圍沒有文字。

```
  <? entities ?>
```
{: codeblock}

會以原生資料類型將實體資訊傳回為陣列。

```json
[
  {
    "entity":"sys-date","location":[6,9],"value":"2018-02-02","confidence":1,"metadata":{"calendar_type":"GREGORIAN","timezone":"America/New_York"}
  },
  {
    "entity":"sys-time","location":[6,9],"value":"14:33:22","confidence":1,"metadata":{"calendar_type":"GREGORIAN","timezone":"America/New_York"}
  }
  ]
```
{: codeblock}

另一個範例是，下列 $array 環境定義變數是陣列，但 $string_array 環境定義變數是字串。

```json
{
  "context": {
    "array": [
      "one",
      "two"
    ],
    "array_in_string": "this is my array: $array"
  }
}
```
{: codeblock}

如果您在「試用」窗格中檢查這些環境定義變數的值，則會看到它們的值指定如下：

**$array**：`["one","two"]`

**$array_in_string**：`"this is my array: [\"one\",\"two\"]"`

您可以接著對 $array 變數執行陣列方法（例如 `<? $array.removeValue('two') ?>`），而不是對 $array_in_string 變數。
