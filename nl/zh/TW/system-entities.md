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

# 系統實體詳細資料
{: #system-entities}

此參照小節提供有關可用系統實體的完整資訊。如需系統實體及其使用方式的相關資訊，請參閱[定義實體](/docs/services/assistant?topic=assistant-entities#entities-enable-system-entities)，並搜尋「啟用系統實體」。
{: shortdesc}

系統實體適用於[支援的語言](/docs/services/assistant?topic=assistant-language-support)主題中指出的語言。

## @sys-currency 實體
{: #system-entities-sys-currency}

@sys-currency 系統實體偵測到以具有貨幣符號或貨幣特定術語的詞語所表示的貨幣值。會傳回數值。

### 可辨識的格式
{: #system-entities-sys-currency-formats}

- 20 cents
- Five dollars
- $10

### meta 資料
{: #system-entities-sys-currency-metadata}

- `.numeric_value`：整數或倍精準數形式的標準數值（以基本單位為單位）
- `.unit`：基本單位貨幣代碼（例如，'USD' 或 'EUR'）

### 傳回
{: #system-entities-sys-currency-returns}

針對輸入 `twenty dollars` 或 `$1,234.56`，@sys-currency 會傳回下列值：

|屬性                          |類型   |針對 `twenty dollars` 所傳回  |針對 `$1,234.56` 所傳回 |
|-----------------------------|--------|-------------------------------|-------------------------:|
|@sys-currency               |字串   |20                          |1234.56 |
|@sys-currency.literal       |字串   |twenty dollars                |$1,234.56 |
|@sys-currency.numeric_value |數字   |20                          |1234.56 |
|@sys-currency.location      |陣列   |[0,14]                        |[0,9] |
|@sys-currency.unit          |字串   |USD*                          |USD |

*@sys-currency.unit 一律會傳回 3 個字母的 ISO 貨幣代碼。

在西班牙文中，針對輸入 `veinte euro` 或 <code>&euro;1.234,56</code>，@sys-currency 會傳回下列值：

|屬性                          |類型   |針對 `veinte euro` 所傳回   |針對 <code>&euro;1.234,56</code> 所傳回 |
|-----------------------------|--------|-----------------------------|-------------------------:|
|@sys-currency               |字串   |20                          |1234.56 |
|@sys-currency.literal       |字串   |veinte euro                 |&euro;1.234,56 |
|@sys-currency.numeric_value |數字   |20                          |1234.56 |
|@sys-currency.location      |陣列   |[0,11]                       |[0,9]|
|@sys-currency.unit          |字串   |EUR*                        |EUR  |
*@sys-currency.unit 一律會傳回 3 個字母的 ISO 貨幣代碼。

您可以取得其他支援的語言及國家貨幣的對等結果。

### @system-currency 用法提示
{: #system-entities-currencty-usage-tips}

- 貨幣值也會辨識為 @sys-number 實體實例。如果您要使用不同的條件同時檢查貨幣值及數字，請在檢查數字的條件上面放置用於檢查貨幣的條件。

- 如果您使用 @sys-currency 實體作為節點條件，而且使用者將 `$0` 指定為值，則會將值適當地辨識為貨幣，但會將條件評估為數字零，而非貨幣零。因此，它不會傳回預期的回應。若要以能夠正確處理零的方式來檢查貨幣值，請改為在節點條件中使用完整 SpEL 表示式語法 `entities['sys-currency']?.value`。

## @sys-date 及 @sys-time 實體
{: #system-entities-sys-date-time}

`@sys-date` 系統實體會擷取 `Friday`、`today` 或 `November 1` 這類提及。此實體的值會將對應的推斷日期儲存為 "yyyy-MM-dd" 格式的字串，例如 "2016-11-21"。系統會使用現行日期值來擴增日期的遺漏元素（例如 "November 21" 的年份）。

**附註：**僅針對英文語言環境，日期輸入的預設系統行為是 MM/DD/YYYY。只有在前兩個數字大於 12 時，這才會變更為 DD/MM/YYYY。儲存的值仍然會是 "yyyy-MM-dd" 格式。

`@sys-time` 系統實體會擷取 `2pm`、`at 4` 或 `15:30` 這類提及。此實體的值會將時間儲存為 "HH:mm:ss" 格式的字串。例如，"13:00:00"。

### 日期時間提及
{: #system-entities-date-time-mentions}

會將日期和時間的提及（例如 `now` 或 `two hours from now`）擷取為兩個不同的實體提及：一個是 `@sys-date`，另一個是 `@sys-time`。這兩個提及不會彼此鏈結，但是它們會共用跨整個日期時間提及的相同文字字串。

也會將多單字日期時間提及（例如 `on Monday at 4pm`）擷取為兩個 @sys-date 及 @sys-time 提及。連續提及時，它們也會共用跨整個日期時間提及的單一文字字串。

### 日期和時間範圍
{: #system-entities-date-time-ranges}

會將日期範圍的提及（例如 `the weekend`、`next week` 或 `from Monday to Friday`）擷取為用於顯示範圍開始及結束的 `@sys-date` 實體提及配對。同樣地，會將時間範圍的提及（例如 `from 2 to 3`）擷取為兩個 `@sys-time` 實體，以顯示開始及結束時間。配對中的兩個實體會共用一個用於對應至完整日期或時間範圍提及的文字字串。

### `Last` 及 `Next` 日期和時間
{: #system-entities-last-next}

在某些語言環境中，只會使用 "last Monday" 這類詞組來指定前一週的星期一。相對地，其他語言環境會使用 "last Monday" 來指定曾是星期一的最後一天，但可能是在同一週或前一週。

例如，針對 Friday June 16，在某些語言環境中，"last Monday" 可能指的是 June 12 或 June 5，而在其他語言環境中，它就只是 June 5（前一週）。這個相同邏輯針對 "next Monday" 這類詞組保留 true。

{{site.data.keyword.conversationshort}} 服務會將 "last" 及 "next" 日期視為參照最接近的前一天或隔天（可能在同一週或前一週）。

"for the last 3 days" 或 "in the next 4 hours" 這類時間詞組的邏輯相同。例如，如果是 "in the next 4 hours"，這會導致兩個 `@sys-time` 實體：一個是現行時間，一個是比現行時間晚四個小時的時間。

### 時區
{: #system-entities-time-zones}

與現行時間相關的日期或時間提及是使用所選擇的時區進行解析。依預設，這是 UTC (GMT)。這表示，依預設，位在與 UTC 不同的時區中的 REST API 用戶端，將會觀察到根據現行 UTC 時間所擷取的 `now` 值。

REST API 用戶端可以選擇性地將本端時區新增為環境定義變數 `$timezone`。此環境定義變數應該與每個用戶端要求一起傳送。例如，`$timezone` 值應該是 `America/Los_Angeles`、`EST` 或 `UTC`。如需所支援時區的完整清單，請參閱[支援的時區](/docs/services/assistant?topic=assistant-time-zones)。

提供 `$timezone` 變數時，會根據用戶端時區（而非 UTC）來計算相對 @sys-date 及 @sys-time 提及的值。

#### 與時區相關的提及範例
{: #system-entities-time-zone-examples}

- now
- in two hours
- today
- tomorrow
- 2 days from now

### 可辨識的格式
{: #system-entities-sys-date-time-formats}

- November 21
- 10:30
- at 6 pm
- this weekend

### 傳回
{: #system-entities-sys-date-time-returns}

針對輸入 `November 21`，@sys-date 會傳回下列值：

|屬性                          |類型   |針對 `November 21` 所傳回 |
|-------------------------|--------|---------------------------:|
|@sys-date.literal       |字串   |November 21 |
|@sys-date               |字串   |20xx-11-21 *|
|@sys-date.location      |陣列   |[0,11]  |
|@sys-date.calendar_type |字串   |GREGORIAN |

- @sys-date 一律會以下列格式傳回日期：yyyy-MM-dd。
- \* 傳回下一個相符日期。如果該日期已過了本年度，則這會傳回下一年的日期。

針對輸入 `at 6 pm`，@sys-time 會傳回下列值：

|屬性                          |類型   |針對 `at 6 pm` 所傳回 |
|-------------------------|--------|-----------------------:|
|@sys-time.literal       |字串   |at 6 pm |
|@sys-time               |字串   |18:00:00 |
|@sys-time.location      |陣列   |[0,7]|
|@sys-time.calendar_type |字串   |GREGORIAN |

- @sys-time 一律會以下列格式傳回時間：HH:mm:ss。

如需處理日期及時間值的相關資訊，請參閱[日期和時間](/docs/services/assistant?topic=assistant-dialog-methods#dialog-methods-date-time)方法參照。
{: tip}

## @sys-location 實體
{: #system-entities-sys-location}

**測試版，針對[支援的語言](/docs/services/assistant?topic=assistant-language-support)主題中指出的語言**：@sys-location 系統實體會從使用者輸入中擷取位置名稱（國家/地區、州/省、城市、鄉鎮等等）。實體的值不是位置的系統標準值。

### 可辨識的格式
{: #system-entities-sys-location-formats}

- Boston
- U.S.A.
- New South Wales

如需處理字串值的相關資訊，請參閱[字串](/docs/services/assistant?topic=assistant-dialog-methods#dialog-methods-strings)方法參照。
{: tip}

## @sys-number 實體
{: #system-entities-sys-number}

@sys-number 系統實體會偵測使用數字或單字所撰寫的數字。在任一情況下，都會傳回數值。

### 可辨識的格式
{: #system-entities-sys-number-formats}

- 21
- twenty one
- 3.13

### meta 資料
{: #system-entities-sys-number-metadata}

- `.numeric_value` - 整數或倍精準數形式的標準數值

### 傳回
{: #system-entities-sys-number-returns}

針對輸入 `twenty` 或 `1,234.56`，@sys-number 會傳回下列值：

|屬性                          |類型   |針對 `twenty` 所傳回  |針對 `1,234.56` 所傳回 |
|-----------------------------|--------|-------------------|------------------------:|
|@sys-number               |字串   |20                          |1234.56 |
|@sys-number.literal       |字串   |twenty            |1,234.56 |
|@sys-number.location      |陣列   |[0,6]                 |[0,8]|
|@sys-number.numeric_value |數字   |20                          |1234.56 |

在西班牙文中，針對輸入 `veinte` 或 `1.234,56`，@sys-number 會傳回下列值：

|屬性                          |類型   |針對 `veinte` 所傳回  |針對 `1.234,56` 所傳回 |
|-----------------------------|--------|-----------------------|------------------------:|
|@sys-number               |字串   |20                          |1234.56 |
|@sys-number.literal       |字串   |veinte                |1.234,56 |
|@sys-number.location      |陣列   |[0,6]                 |[0,8] |
|@sys-number.numeric_value |數字   |20                          |1234.56 |

您可以取得其他支援的語言的對等結果。

### @system-number 用法提示
{: #system-entities-sys-number-usage-tips}

- 如果您使用 @sys-number 實體作為節點條件，而且使用者將零指定為值，則會將 0 值適當地辨識為數字，但會將條件評估為 false，而且無法適當地傳回關聯的回應。若要以能夠正確處理零的方式來檢查數字，請改為在節點條件中使用完整 SpEL 表示式語法 `entities['sys-number']?.value`。

- 如果您使用 @sys-number 來比較條件中的數值，請務必另外檢查數字本身是否存在。如果找不到數字，則會將 @sys-number 評估為空值，這樣可能會將您的比較評估為 true，即使數字不存在。

  例如，不要單獨使用 `@sys-number<4`，因為如果找不到數字，則會將 `@sys-number` 評估為空值。因為空值小於 4，所以會將條件評估為 true，即使數字不存在。

  請改用 `@sys-number AND @sys-number<4`。如果找不到數字，則會將第一個條件評估為 false，這樣會適當地將整個條件評估為 false。

如需處理數值的相關資訊，請參閱[數字](/docs/services/assistant?topic=assistant-dialog-methods#dialog-methods-numbers)方法參照。
{: tip}

## @sys-percentage 實體
{: #system-entities-sys-percentage}

@sys-percentage 系統實體會偵測到以具有百分比符號的詞語所表示或使用 `percent` 這個單字所撰寫的百分比。在任一情況下，都會傳回數值。

### 可辨識的格式
{: #system-entities-sys-percentage-formats}

- 15%
- 10 percent

### meta 資料
{: #system-entities-sys-percentage-metadata}

`.numeric_value`：整數或倍精準數形式的標準數值

### 傳回
{: #system-entities-sys-percentage-returns}

針對輸入 `1,234.56%`，@sys-percentage 會傳回下列值：

|屬性                          |類型   |針對 `1,234.56%` 所傳回 |
|-------------------------------|--------|-------------------------:|
|@sys-percentage               |字串   |1234.56 |
|@sys-percentage.literal       |字串   |1,234.56% |
|@sys-percentage.location      |陣列   |[0,9] |
|@sys-percentage.numeric_value |數字   |1234.56 |

在西班牙文中，針對輸入 `1.234,56%`，@sys-currency 會傳回下列值：

|屬性                          |類型   |針對 `1.234,56%` 所傳回 |
|-------------------------------|--------|-------------------------:|
|@sys-percentage               |字串   |1234.56 |
|@sys-percentage.literal       |字串   |1.234,56% |
|@sys-percentage.location      |陣列   |[0,9] |
|@sys-percentage.numeric_value |數字   |1234.56 |

您可以取得其他支援的語言的對等結果。

### @system-percentage 用法提示
{: #system-entities-sys-percentage-usage-tips}

- 百分比值也會辨識為 @sys-number 實體實例。如果您要使用不同的條件同時檢查百分比值及數字，請在檢查數字的條件上面放置用於檢查百分比的條件。

- 如果您使用 @sys-percentage 實體作為節點條件，而且使用者將 `0%` 指定為值，則會將值適當地辨識為百分比，但會將條件評估為數字零，而非百分比 0%。因此，它不會傳回預期的回應。若要以能夠正確處理零百分比的方式來檢查百分比，請改為在節點條件中使用完整 SpEL 表示式語法 `entities['sys-percentage']?.value`。

- 如果您輸入 `1-2%` 這類值，則會將值 `1%` 及 `2%` 傳回為系統實體。索引會是 1% 與 2% 之間的完整範圍，而且兩個實體都會有相同的索引。

## @sys-person 實體
{: #system-entities-sys-person}

**測試版，針對[支援的語言](/docs/services/assistant?topic=assistant-language-support)主題中指出的語言**：@sys-person 系統實體會從使用者輸入中擷取名稱。名稱會個別進行辨識，因此不會將 "Joe" 視為 "Joseph"，反之亦然。實體的值不是名稱的系統標準值。

### 可辨識的格式
{: #system-entities-sys-person-formats}

- Ronald
- Jane Doe
- Vijay

如需處理字串值的相關資訊，請參閱[字串](/docs/services/assistant?topic=assistant-dialog-methods#dialog-methods-strings)方法參照。
{: tip}
