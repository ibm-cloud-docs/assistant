---

copyright:
  years: 2015, 2018
lastupdated: "2018-02-05"

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

# 表示式語言方法

您可以處理從使用者詞語中擷取的值，而這些使用者詞語是您要在回應的環境定義變數、條件或其他位置中參照的詞語。
{: shortdesc}

## 評估語法

若要在其他變數內展開變數值，或將方法套用至輸出文字或環境定義變數，請使用 `<? expression ?>` 表示式語法。例如：

- **將數值內容增量**
    - `"output":{"number":"<? output.number + 1 ?>"}`
- **對物件呼叫方法**
    - `"context":{"toppings": "<? context.toppings.append( 'onions' ) ?>"}`

下列各節說明您可用來處理值的方法。它們是依資料類型進行分組：

- [陣列](dialog-methods.html#arrays)
- [日期和時間](dialog-methods.html#date-time)
- [數字](dialog-methods.html#numbers)
- [物件](dialog-methods.html#objects)
- [字串](dialog-methods.html#strings)

## 陣列
{: #arrays}

您無法在設定陣列值的相同節點內，在節點條件或回應條件中，使用這些方法來檢查陣列中的值。

### JSONArray.append(object)

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

### JSONArray.contains(object value)

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

### JSONArray.get(integer)

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
    "text": {
      "values": [
        "The first item in the array is <?$nested.array.get(0)?>"
      ],
      "selection_policy": "sequential"
    }
  }
```
{: codeblock}

### JSONArray.getRandomItem()

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
    "text": "<? $toppings_array.getRandomItem() ?> is a great choice!"
  }
}
```
{: codeblock}

結果：`"ham is a great choice!"`、`"onion is a great choice!"` 或 `"olives is a great choice!"`

**附註：**會隨機選擇產生的輸出文字。

### JSONArray.join(string delimiter)

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
    "text": "This is the array: <? $toppings_array.join(';') ?>"
  }
}
```
{: codeblock}

結果：

```json
This is the array: onion;olives;ham;
```
{: codeblock}

### JSONArray.remove(integer)

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

### JSONArray.set(integer index, object value)

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
{: #com.google.gson.JsonArray}

除了內建方法之外，您還可以使用 `com.google.gson.JsonArray` 類別的標準方法。

#### 新陣列

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
{: #date-time}

您可以利用數種方法來處理日期和時間。

如需如何從使用者輸入中辨識及擷取日期和時間資訊的相關資訊，請參閱 [@sys-date 及 @sys-time 實體](system-entities.html#sys-datetime)。

### .after(String date/time)

- 判定日期/時間值是否在日期/時間引數之後。
- 類似 `.before()`。

### .before(String date or time)

- 例如：
- @sys-time.before('12:00:00')
- @sys-date.before('2016-11-21')
- 判定日期/時間值是否在日期/時間引數之前。
- 如果比較不同的項目（例如 `time vs. date`、`date vs. time` 及 `time vs. date and time`），則此方法會傳回 false，並在回應 JSON 日誌 `output.log_messages` 中列印異常狀況。
    - 例如，`@sys-date.before(@sys-time)`。
- 如果比較 `date and time vs. time`，則此方法會忽略日期，而僅比較時間。

### now()

- 靜態函數。
- 傳回含有現行日期和時間的字串，格式為 `yyyy-MM-dd HH:mm:ss`。
- 您可以對此函數所傳回的日期-時間值呼叫其他日期/時間方法，因此可將其傳入作為其引數。
- 如果已設定環境定義變數 `$timezone`，則此函數會傳回用戶端時區的日期和時間。

輸出欄位中使用 `now()` 的對話節點範例：

```json
{
  "conditions": "#what_time_is_it",
  "output": {
    "text": "<? now() ?>"
   }
}
```
{: codeblock}

節點條件中的 `now()` 範例（判定是否仍是早上）：

```json
{
  "conditions": "now().before('12:00:00')",
  "output": {
    "text": "Good morning!"
   }
}
```
{: codeblock}

### .reformatDateTime(String format)

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

- 判定日期/時間值是否與日期/時間引數相同。

### .sameOrAfter(String date/time)

- 判定日期/時間值是在日期/時間引數之後還是與其相同。
- 類似 `.after()`。

### .sameOrBefore(String date/time)

- 判定日期/時間值是在日期/時間引數之前還是與其相同。

### java.util.Date 支援
{: #java.util.Date}

除了內建方法之外，您還可以使用 `java.util.Date` 類別的標準方法。

#### 日期計算

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
{: #numbers}

這些方法可協助您取得及重新格式化數值。

如需可從使用者輸入辨識及擷取數字之系統實體的相關資訊，請參閱 [@sys-number 實體](system-entities.html#sys-number)。

如果您要服務辨識使用者輸入中的特定數字格式（例如訂單號碼參照），請考慮建立型樣實體來進行擷取。如需詳細資料，請參閱[建立實體](entities.html#creating-entities)。

### toDouble()

  將物件或欄位轉換為 Double 數字類型。您可以在任何物件或欄位上呼叫此方法。如果轉換失敗，則會傳回*空值*。

### toInt()

  將物件或欄位轉換為 Integer 數字類型。您可以在任何物件或欄位上呼叫此方法。如果轉換失敗，則會傳回*空值*。

### toLong()

  將物件或欄位轉換為 Long 數字類型。您可以在任何物件或欄位上呼叫此方法。如果轉換失敗，則會傳回*空值*。

  如果您在 SpEL 表示式中指定 Long 數字類型，則必須在數字後面加上 `L`，以進行這類識別。例如，`5000000000L`。任何不適用於 32 位元 Integer 類型的數字都需要使用此語法。例如，大於 2^31 (2,147,483,648) 或小於 -2^31 (-2,147,483,648) 的數字都會視為 Long 數字類型。Long 數字類型的最小值為 -2^63，而最大值為 2^63-1。

### Java 數字支援
{: #java.lang.Number}

### java.lang.Math()

執行基本數值運算。

您可以使用 Class 方法，包括下列項目：

- max()

```json
{
  "context": {
    "bigger_number": "<? T(Math).max($number1,$number2) ?>"
  },
  "output": {
    "text": {
      "values": [
        "The bigger number is $bigger_number."
      ],
      "selection_policy": "sequential"
    }
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
    "text": {
      "values": [
        "The smaller number is $smaller_number."
      ],
      "selection_policy": "sequential"
    }
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
    "text": {
      "values": [
        "Your number $base to the second power is $power_of_two."
      ],
      "selection_policy": "sequential"
    }
  }
}
```
{: codeblock}

如需其他方法的相關資訊，請參閱 [java.lang.Math 參考文件](https://docs.oracle.com/javase/7/docs/api/java/lang/Math.html)。

### java.util.Random()

傳回亂數。您可以使用下列其中一個語法選項：

- 若要傳回隨機布林值（true 或 false），請使用 `<?new Random().nextBoolean()?>`。
- 若要傳回 0（包含）與 1（排除）之間的隨機倍精準數，請使用 `<?new Random().nextDouble()?>`
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
    "text": {
      "values": [
        "Here's a random number between 0 and @sys-number.literal: $answer."
      ],
      "selection_policy": "sequential"
    }
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
{: #objects}

### JSONObject.has(string)

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

### JSONObject.remove(string)

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
{: #com.google.gson.JsonObject}

除了內建方法之外，您還可以使用 `com.google.gson.JsonObject` 類別的標準方法。

## 字串
{: #strings}

這些方法可協助您處理文字。

如需如何從使用者輸入中辨識及擷取特定類型之字串的相關資訊（例如人名及位置），請參閱[系統實體](system-entities.html)。

**附註：**針對涉及正規表示式的方法，請參閱 [RE2 語法參考資料 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/google/re2/wiki/Syntax){: new_window}，以取得要在指定正規表示式時使用之語法的詳細資料。

### String.append(object)

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

### String.contains(string)

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

### String.endsWith(string)

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

此方法會傳回由輸入正規表示式的指定群組索引所擷取的字串。

針對此輸入：

```
"Hello 123456".
```
{: codeblock}

此語法：

```json
{
  "context": {
    "number_extract": "<? input.text.extract('[\\d]+',0) ?>"
  }
}
```
{: codeblock}

  **重要事項：**若要處理 `\\d` 作為正規表示式，您必須新增另一個 `\\` 來跳出這兩條反斜線：`\\\\d`

結果：

```json
{
  "context": {
    "number_extract": "123456"
  }
}
```
{: codeblock}

### String.find(string regexp)

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

### String.isEmpty()

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

### String.matches(string regexp)

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

### String.startsWith(string)

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

### String.substring(int beginIndex, int endIndex)

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
    "input_upper_case": "this is a dog!"
  }
}
```
{: codeblock}

### String.toUpperCase()

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
{: #java.lang.String}

除了內建方法之外，您還可以使用 `java.lang.String` 類別的標準方法。

#### java.lang.String.format()

您可以將標準 Java 字串的 `format()` 方法套用至文字。如需用來指定格式詳細資料之語法的相關資訊，請參閱 [java.util.formatter 參考資料 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://docs.oracle.com/javase/7/docs/api/java/util/Formatter.html#syntax){: new_window}。

例如，下列表示式會接受三個十進位整數（1、1 及 2），並將它們新增至句子中。

```json
{
  "formatted String": "<? T(java.lang.String).format('%d + %d equals %d', 1, 1, 2) ?>"
}
```
{: codeblock}

結果：`1 + 1 equals 2`。

## 間接資料類型轉換

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
