---

copyright:
  years: 2015, 2019
lastupdated: "2019-07-17"

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
{:gif: data-image-type='gif'}

# 新系統實體 ![測試版](images/beta.png)
{: #beta-system-entities}

啟用新的系統實體，以充分運用對 IBM 提供的基於數字的系統實體所做的改善。

目前，只能針對以英文或德文編寫的對話技能來啟用此設定。
{: note}

新系統實體可以辨識使用者輸入中更細緻的提及項目。例如，如果依名稱提及國定假日（如 `Thanksgiving`），則 `@sys-date` 可以計算其日期。`@sys-date` 還可以辨識在使用者輸入中所提及日期中指定的年份。透過這些改進，助理能更輕鬆地識別許多數字型系統實體。例如，辨識為 `@sys-date` 的日期提及項目（如 `May 10`）不會同時識別為 `@sys-number` 提及項目。

`@sys-location` 和 `@sys-person` 系統實體未變更。
{: note}

## 啟用新的系統實體
{: #beta-system-entities-enable}

若要啟用新的系統實體，請完成下列步驟：

1.  從「技能」頁面中，開啟您的技能。
1.  按一下**選項**標籤。
1.  按一下**系統實體**，然後選擇**試用測試版**。
1.  按一下**實體**標籤，然後按一下**系統實體**。
1.  開啟要在對話中使用的系統實體。

藉由將一個以上的新系統實體新增至對話節點條件，或新增至對話節點條件式回應的條件中，以測試新的系統實體。然後，在「試用」窗格中，提交使用者話語以觸發所新增的節點。

如果您決定偏好使用舊版系統實體的行為，則可以隨時停止使用測試版版本。回到**選項**標籤上的**系統實體**頁面，然後選擇**保存現有**。給 Watson 一些時間來進行重新訓練。

如果您決定繼續使用測試版版本，請檢閱以系統實體值為條件或傳回系統實體值的所有現有對話節點，以判定是否需要進行變更。例如，某些提及項目先前被分類為多個系統實體類型。但現在僅分類為一個系統實體類型，例如貨幣提及項目只會分類為 `@sys-currency`。您可能可以簡化先前用於處理該行為的邏輯。或者，您可能需要修訂已新增且依賴舊行為的邏輯。

### 系統系統實體內容
{: #beta-system-entities-props}

為了改善系統實體，已將新內容新增至基於數字的系統實體的實體物件。

下表概述新增的新內容。若要查看與現行系統實體相關聯的內容，請參閱[系統實體詳細資料](/docs/services/assistant?topic=assistant-system-entities)。

表 1. 新系統實體內容

|系統實體|內容                  |說明        |
|---------------|----------|-------------|
| `@sys-date` | `alternatives` |擷取的日期是在使用者未明確指出日期時，除了儲存為 `@sys-date` 值的日期以外，使用者可能指的是其他日期。例如，如果今天的日期為 2019 年 3 月 10 日，但使用者輸入的是 `on March 1`，則會將明年 3 月 1 日儲存為 `@sys-date` (`2020-03-01`)。不過，使用者可能指的是 2019 年 3 月 1 日。因此，系統還會將今年的日期 (`2019-03-01`) 儲存為替代日期值。替代值會儲存為物件，其中包含每個替代日期的值和信賴度，並以 JSON 陣列格式儲存。若要取得替代值，請使用語法：`@sys-date.alternative`。在大部分情況下，未來的日期會儲存為 `@sys-date` 值，而過去的日期會儲存為替代日期。這同樣適用於指定星期幾但未指定介系詞（如 `on`）或形容詞（如 `this` 或 `next`）的情況。例如，`Are you open Monday?`。不過，如果使用者在詞組中指定星期幾（例如 `on Monday` 或 `next Monday`），則會假設日期為未來的日期，並且不會為其建立替代日期。例如，如果今天是星期二，而使用者輸入 `this Tuesday` 或 `next Tuesday`，則 `@sys-date` 會設定為下週的星期二，並且不會建立替代日期。|
| `@sys-date` | `datetime_link` |如果存在，指出使用者的輸入一起提及了日期和時間，這暗示日期和時間彼此相關。包含日期和時間之字串的開始及結束索引值會儲存為鏈結名稱的一部分。例如，如果輸入是 `Are you open today at 5?`，則 `today` 會辨識為在位置 `[13,18]` 的日期參照，而 `at 5` 會辨識為在位置 `[19,23]` 的時間參照。位置值 `[13,23]`（它同時跨越日期和時間提及項目）會附加到產生的 datetime_link。它會命名為 `datetime_link_13_23`，且包含在參與此關係之 `@sys-date` 及 `@sys-time` 系統實體的輸出中。|
| `@sys-date` | `festival` |辨識語言環境特有的假日名稱，並且可以傳回即將到來的假日的日期，例如 `Thanksgiving`、`Christmas` 和 `Halloween`。如果您需要在條件中指定假日名稱，請全部使用小寫字母，例如 (`@sys-date.festival == 'thanksgiving'`)。|
| `@sys-date` | `granularity` |辨識時間範圍的提及項目，例如 `next year` (=`year`)。選項包括 `day`、`weekend`、`week`、`fortnight`、`month`、`quarter` 和 `year`。|
| `@sys-date` | `interpretation` |助理傳回的物件，其中包含新的欄位，可提高系統實體分類器的精準度。您可以在用來參照欄位的表達中省略 `interpretation`。例如，您可以使用簡短語法 `<? @sys-date.festival ?>` 來存取 interpretation 物件中 `festival` 欄位的值。|
| `@sys-date` | `range_link` |如果存在，指出使用者的輸入所包含的語法暗示已指定日期範圍。例如，輸入可能為 `I will travel from March 15 to March 22`。對於偵測到的兩個 `@sys-date` 系統實體，其輸出中都會有 `range_link` 內容。這提供額外的資訊，包括每個 `@sys-date` 在範圍關係中扮演的角色。例如，開始日期的角色類型為 `date_from`，結束日期的角色類型為 `date_to`。若要檢查角色值，您可以使用語法 `@sys-date.role?.type == 'date_from'`。如果使用者輸入隱含範圍，但僅指定一個日期，則不會傳回 `range_link` 內容，但只會傳回一個角色類型。例如，如果使用者詢問 `Have you been open since yesterday`，則會將 yesterday 的日期辨識為 `@sys-date` 提及項目，並且會為其傳回類型為 `date_from` 的角色。同時也會傳回 `range_modifier`，它會識別輸入中觸發識別某一範圍的字組。在此範例中，修飾元為 `since`。 |
| `@sys-date` |`relative_year`、`relative_month`、`relative_week`、`relative_weekend`、`relative_day`|辨識並擷取使用者輸入中相對日期值的提及項目，例如 `two weeks ago` (`relative_week = -2`)、`this weekend (relative_weekend = 0)` 或 `today` (`relative_day = 0`)。|
| `@sys-date` |`specific_year`、`specific_quarter`、`specific_month`、`specific_day`、`specific_day_of_week`|辨識並擷取使用者輸入中特定日期值的提及項目，例如 `2020` (`specific_year = 2020`)、`Q2` (`specific_quarter = 2`)、`March` (`specific_month = 3`)、`Monday` (`specific_day_of_week = monday`) 或 `3rd` (`specific_day = 3`)。|
| `@sys-number` | `interpretation` |助理傳回的物件，其中包含新的欄位，可提高系統實體分類器的精準度。您可以在用來參照欄位的表達中省略 `interpretation`。例如，您可以使用簡短語法 `<? @sys-date.range_link ?>` 來存取 interpretation 物件中 `range_link` 欄位的值。|
| `@sys-number` | `range_link` |如果存在，指出使用者的輸入所包含的語法暗示已指定數字範圍。例如，輸入可能為 `I'm interested in 5 to 7.`。對於偵測到的兩個 `@sys-number` 系統實體，其輸出中都會有 `range_link` 內容。這提供額外的資訊，包括每個 `@sys-number` 在範圍關係中扮演的角色。例如，開始數字的角色類型為 `number_from`，結束數字的角色類型為 `number_to`。若要檢查角色值，您可以使用語法 `@sys-number.role?.type == 'number_from'`。|
| `@sys-time` | `alternatives` |擷取的時間是在使用者未明確指出時間時，除了儲存為 `@sys-time` 值的時間以外，使用者可能指的是其他時間。例如，使用者說出 `The meeting is at 5` 時，系統會將時間計算為 5:00:00 或 5 AM，並將其儲存為 `@sys-time`。不過，使用者可能指的是 17:00:00 或 5 PM。系統也會將稍後的時間儲存為替代時間值。如果使用者輸入 `The meeting is at 5pm`，則 `@sys-time` 會設定為 17:00:00，並且不會建立替代值，因為不存在 AM 和 PM 混淆的問題。替代值會儲存為物件，其中包含每個替代時間的值和信賴度，並以 JSON 陣列格式儲存。若要取得替代值，請使用語法：`@sys-time.alternative`。|
| `@sys-time` | `datetime_link` |如果存在，指出使用者的輸入一起提及了日期和時間，這暗示日期和時間彼此相關。包含日期和時間之字串的開始及結束索引值會儲存為鏈結名稱的一部分。例如，如果輸入是 `Are you open today at 5?`，則 `today` 會辨識為在位置 `[13,18]` 的日期參照，而 `at 5` 會辨識為在位置 `[19,23]` 的時間參照。位置值 `[13,23]`（它同時跨越日期和時間提及項目）會附加到產生的 datetime_link。它會命名為 `datetime_link_13_23`，且包含在參與此關係之 `@sys-date` 及 `@sys-time` 系統實體的輸出中。|
| `@sys-time` | `granularity` |辨識時間範圍的提及項目，例如 `now` (=`instant`)、`3 o'clock`、`noon` (=`hour`) 或 `17:00:00` (=`second`)。選項包括 `hour`、`minute`、`second` 和 `instant`。|
| `@sys-time` | `interpretation` |助理傳回的物件，其中包含新的欄位，可提高系統實體分類器的精準度。您可以在用來參照欄位的表達中省略 `interpretation`。例如，您可以使用簡短語法 `<? @sys-time.granularity ?>` 來存取 interpretation 物件中 `granularity` 欄位的值。|
| `@sys-time` | `part_of_day` |辨識代表一天中時間的詞彙，例如 `morning`、`afternoon`、`evening`、`night` 或 `now`。此外，可設定任意時間，例如 `9:00:00` 表示 morning、`15:00:00` 表示 afternoon、`18:00:00` 表示 evening，而 `22:00:00` 表示 night。|
| `@sys-time` | `range_link` |如果存在，指出使用者的輸入所包含的語法暗示已指定時間範圍。例如，輸入可能為 `Are you open from 9AM to 11AM`。對於偵測到的兩個 `@sys-time` 系統實體，其輸出中都會有 `range_link` 內容。這提供額外的資訊，包括每個 `@sys-time` 在範圍關係中扮演的角色。例如，開始時間的角色類型為 `time_from`，結束時間的角色類型為 `time_to`。若要檢查角色值，您可以使用語法 `@sys-time.role?.type == 'time_from'`。如果使用者輸入隱含範圍，但僅指定一個時間，則不會傳回 `range_link` 內容，但只會傳回一個角色類型。例如，如果使用者詢問 `Are you open until 9PM`，則會將 9PM 辨識為 `@sys-time` 提及項目，並且會為其傳回類型為 `time_to` 的角色。同時也會傳回 `range_modifier`，它會識別輸入中觸發識別某一範圍的字組。在此範例中，修飾元為 `until`。|
| `@sys-time` |`relative_hour`、`relative_minute`、`relative_second`|辨識時間的相對提及項目，例如 `5 hours ago` (`relative_hour = -5`)、`in two minutes`(`relative_minute = 2`) 或 `in a second` (`relative_second = 1`)。|
| `@sys-time` |`specific_hour`、`specific_minute`、`specific_second`|辨識時間的特定提及項目，例如 `at 5 o'clock` (`specific_hour = 5`)、`at 2:30`(`specific_minute = 30`) 或 `23:30:22` (`specific_second = 22`)。|
{: caption="新的系統實體內容" caption-side="top"}

下表中的值不是實體物件的內容。這些屬性是可以在產品中使用的 Helper 值，用於從新的系統實體中快速擷取日期或時間詳細資料。

表 2. 系統實體 Helper 函數

|系統實體| Helper 值|說明        |
|---------------|----------|-------------|
| `@sys-date` | `day` |將日期中指定的日作為數值傳回。例如，如果日期為 `5 December 2019`，則會傳回 `5`。|
| `@sys-date` | `day_of_week` |評估日期，以判定這是星期幾。以小寫格式儲存星期幾。例如，`Sunday` 會儲存為 `sunday`。使用小寫首字母來檢查條件中的星期幾名稱。例如：`@sys-date.day_of_week == 'sunday'`|
| `@sys-date` | `month` |將日期中指定的月作為數值傳回。例如，如果日期為 `5 December 2019`，則會傳回 `12`。|
| `@sys-date` | `year` |將日期中指定的年作為數值傳回。例如，如果日期為 `5 December 2019`，則會傳回 `2019`。|
| `@sys-time` | `hour` |將時間中指定的小時作為數值傳回。例如，如果時間為 `5:30:10 PM`，則會傳回 `17` 以代表 5 PM。|
| `@sys-time` | `minute` |傳回時間中指定的分鐘值。`30`。如果未指定分鐘，則此 Helper 會傳回 `0`。|
| `@sys-time` | `second` |傳回時間中指定的秒值。`10`。如果未指定秒值，則此 Helper 會傳回 `0`。|
{: caption="系統實體 Helper 函數" caption-side="top"}

### 用法範例
{: #beta-system-entities-examples}

您可以使用下列類型的表示式來充分運用改進的系統實體中的新內容。此表格顯示使用者在提及日期 `4 July 2019` 和時間 `3:30 PM` 時傳回的結果。您可以在對話文字回應中使用這些表示式，而不需要在 `<? ?>` 語法中括起它們。

表 3. 使用新系統實體來取得日期和時間詳細資料

|SpEL 表示式語法|結果|
|------------------------|---------|
| `@sys-date.year` | `2019` |
| `@sys-date.month` | `7` |
| `@sys-date.day` | `4` |
| `@sys-date.day_of_week` | `thursday` |
| `@sys-time.hour` | `15` |
| `@sys-time.minute` | `30` |
| `@sys-time.second` | `0` |
{: caption="使用新系統實體來取得日期和時間詳細資料" caption-side="top"}

在以 `#Customer_Care_Store_Hours` 目的為條件的節點中，可以新增使用新系統實體內容的條件式回應，以根據使用者的詢問來提供商店營業時間略有不同的回答。 

您可能並不想要在實際對話中使用所有這些條件式回應；這裡說明這些條件式回應僅是為了說明可以使用的條件式回應。
{: tip}

表 4. 在條件式回應中使用新的系統實體

|條件式回應條件語法|說明        |範例回應文字|
|--------------------------------|-------------|----------|
|`@sys-date.festival == 'thanksgiving'`|檢查客戶是否特別詢問感恩節的營業時間。|我們在感恩節為有需要的人提供免費餐。|
|`@sys-date.festival`|檢查客戶是否詢問其他特定假日的營業時間。|我們在聖誕節、7 月 4 日和總統日不營業。在其他假日，營業時間為上午 10 點到下午 5 點。|
|`@sys-time.part_of_day == 'night'`|檢查使用者是否在輸入中包含將 night 作為一天中的時間提及的任何詞彙。|我們不會營業到很晚；大多數時候結束營業的時間為晚上 9 點。|
|`@sys-date.datetime_link && input.text.contains('today') && now().sameOrAfter(@sys-time)`|檢查輸入是否包含 `today at 8` 這類的詞組。此外，還會檢查偵測到的 `@sys-time` 是否早於現行時間。您可以將環境定義變數新增至條件式回應，以建立值為 `<? @sys-time.plusHours(12) ?>` 的 `$new_time` 變數。您可以使用此方法來建立日期變數，以擷取使用者在中午時間詢問 `Are you open today at 8` 時更有可能所指的時間。系統假設使用者指的是早上 8 點，而不是晚上 8 點。|您是指今天 $new_time 點，對嗎？|
|`@sys-time.range_link && entities[0].role?.type == 'time_from' && entities[1].role?.type == 'time_to'`|檢查輸入是否包含時間範圍。此條件也會先確定在 entities 陣列中列出的第一個 `@sys-time` 系統實體是開始時間，以及第二個是結束時間，再將這些系統實體包含在回應中。|您要知道我們的營業時間是否為 `<? entities[0] ?>` 到 `<? entities[1] ?>`，對嗎？|
|`@sys-time && entities.size() < 2 && entities[0].role?.type == 'time_to'` |檢查輸入是否包含一個具有 `time_to` 角色類型的 `@sys-time` 提及項目。如果使用者輸入與此條件相符，則暗示使用者已指定開放式時間範圍的結束時間。例如，回應將由輸入 `Are you open until 9pm?` 來觸發。此輸入與此條件相符，因為它僅在時間範圍內識別到一個時間，並且該提及項目具有 `time_to` 角色。|您是要從現在 (`<? now().reformatDateTime('h:mm a') ?>`) 直到 `<? @sys-time.reformatDateTime('h:mm a') ?>` 嗎？|
|`@sys-date.day_of_week == 'sunday'`|檢查使用者詢問的特定日期是否為某個星期日。|我們星期日不營業。|
|`@sys-date.specific_day_of_week == 'monday' && @sys-date.alternative`|檢查使用者是否在其查詢中提及一星期中的 `Monday`（星期一）。例如，`Are you open Monday`。此條件也會檢查是否已儲存任何替代日期。助理不完全確定使用者指的是哪個星期一時，會建立替代日期，因此還會儲存替代星期一的日期。使用者指定星期幾時，助理會假設使用者指的是未來的日期（下一個星期一）。您可以使用偵測到的替代值來新增用於複查目標日期的回應。在此情況下，替代日期是上一個星期一的日期。|您指的是 `@sys-date` 還是 `@sys-date.alternative`？|
|`true`               |回應其他任何詢問店鋪營業時間資訊的要求。|我們的營業時間是星期一到星期六，早上 9 點到晚上 9 點|
{: caption="在條件式回應中使用新的系統實體" caption-side="top"}
