---

copyright:
  years: 2015, 2019
lastupdated: "2019-05-28"

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

# 對話建置提示
{: #dialog-tips}

瞭解如何建置對話，並取得完成更複雜步驟的一些提示。
{: shortdesc}

請檢閱以下這些來自有經驗對話設計者的提示。

## 規劃整體對話
{: #dialog-tips-plan}

- 先規劃您要建置之對話的設計，再新增單一對話節點。必要的話，請在紙張上概略畫出設計。
- 儘可能根據實際證明及行為的資料來進行設計決策。請不要新增節點來處理某人*認為* 可能會發生的狀況。
- 避免依現狀複製商業處理程序。它們很少是交談式處理程序。
- 如果人們已經使用某個處理程序，請檢查他們的處理方法。一般而言，人們會從交談的角度將處理程序最佳化。
- 決定您助理的語氣、特質與定位。在您建立的對話中，一致地反映這些選擇。
- 絕對不要將助理誤傳為是一個人。如果使用者相信助理是一個人，然後又發現其實並不是，使用者可能會不相信該助理。
- 並不是一切都必須是交談。有時候 Web 表單的運作效果更佳。

## 新增節點
{: #dialog-tips-nodes}

- 新增說明節點用途的節點名稱。

  您現在知道節點的作用，但幾個月後您可能就不知道了。未來的您以及任何團隊成員都會感謝您新增了敘述性的節點名稱。而且，節點名稱會顯示在日誌中，這可協助您在稍後對交談進行除錯。
- 若要收集執行作業所需的資訊，請嘗試使用含空位的節點（而非一堆個別節點），從使用者獲取資訊。請參閱[使用空位收集資訊](/docs/services/assistant?topic=assistant-dialog-slots)。
- 若為複雜的處理程序流程，請在處理程序開始時告知使用者他們需要提供的所有資訊。
- 瞭解助理如何通過對話樹狀結構，以及資料夾、分支、跳至和離題對路徑的影響。請參閱[對話流程](/docs/services/assistant?topic=assistant-dialog-overview#dialog-overview-flow)。
- 請不要到處新增跳至動作。它們會提高對話流程的複雜性，讓稍後的對話除錯作業更加困難。
- 若要跳至與現行節點相同分支中的節點，請使用*跳過使用者輸入*，而不要使用*跳至*。

  使用了這個選項，當您移除或重新排序要跳至的子節點時，便不必編輯現行節點的設定。請參閱[定義下一步](/docs/services/assistant?topic=assistant-dialog-overview#dialog-overview-jump-to)。
- 在您啟用從節點離題之前，請測試最常見的使用者情境。並且確定已將可能的離題到節點配置成返回。請參閱[離題](/docs/services/assistant?topic=assistant-dialog-runtime#dialog-runtime-digressions)。

## 新增回應
{: #dialog-tips-responses}

- 使答案保持簡短且有用。
- 在回應中反映使用者的目的。

  這樣做可向使用者保證機器人理解他們的目的，或者如果機器人不理解，讓使用者有機會立即更正誤解。
- 只有在答案取決於經常變更的資料時，才在回應中包含外部網站的鏈結。
- 避免過度使用按鈕。鼓勵使用者從一組按鈕中挑選預先定義的選項，這比較不像實際的交談，並且會讓您較無法瞭解使用者真正想要做什麼。當您讓實際使用者以他們自己的話來詢問事情時，您可以使用輸入來訓練系統，並衍生出更好的目的。
- 避免在一個節點即可應付的情況下使用一堆節點。例如，將多個條件式回應新增至單一節點，以根據使用者提供的詳細資料傳回不同的回應。請參閱[條件式回應](/docs/services/assistant?topic=assistant-dialog-overview#dialog-overview-multiple)。
- 回應用字要小心。您可能只因為回應的用字遣詞，就改變了某人對您系統的反應方式。變更一行文字可以避免您必須撰寫多行程式碼來實作複雜的程式化解決方案。
- 經常備份您的技能。請參閱[下載技能](/docs/services/assistant?topic=assistant-skill-add#skill-add-download)。

## 從使用者輸入擷取資訊的提示
{: #dialog-tips-user-input}

可能很難瞭解要在對話節點中使用的語法，以便在使用者輸入中正確地擷取您要尋找的資訊。以下是您可以用來因應常見目標的方法。

- **傳回使用者的輸入**：您可以擷取使用者說出的確切文字，並在您的回應中傳回。請在回應中使用下列 SpEL 表示式，以在回應中重複使用者指定的文字：

  `You said: <? input.text ?>.`

  如果已開啟自動更正，但您要傳回使用者未經更正的原始輸入，則可以使用 `<? input.original_text ?>`。但是，請務必先使用回應條件來檢查 `original_text` 欄位是否存在。
  {: note}

- **判定使用者輸入中的字數**：您可以在 input.text 物件上，執行任何支援的「字串」方法。例如，您可以使用下列 SpEL 表示式，找出使用者話語中有多少個字：

  `input.text.split(' ').size()`

  請參閱[適用於字串的表示式語言方法](/docs/services/assistant?topic=assistant-dialog-methods#dialog-methods-strings)，以瞭解您可以使用的其他方法。

- **處理多個目的**：使用者的輸入表示希望完成兩個個別作業。`I want to open a savings account and apply for a credit card.` 對話如何同時辨識並處理這兩者呢？請參閱來自 Simon O'Doherty 部落格的 [Compound questions](https://sodoherty.ai/2017/02/06/compound-questions/){: new_window} 項目，以瞭解您可以嘗試的策略。（Simon 是 {{site.data.keyword.conversationshort}} 團隊的開發人員。）

- **處理不明確的目的**：使用者輸入表示的希望不夠明確，導致助理找到兩個以上的節點，它們具有可能可以處理此希望的目的。對話如何知道要遵循哪個對話分支？如果您啟用澄清，即可向使用者顯示他們有哪些選項，並要求使用者挑選正確的選項。如需詳細資料，請參閱[澄清](/docs/services/assistant?topic=assistant-dialog-runtime#dialog-runtime-disambiguation)。

- **處理輸入中的多個實體**：如果只要評估實體類型中第一個偵測到之實例的值，您可以使用 `@entity == 'specific-value'` 語法，而非 `@entity:(specific-value)` 格式。

  例如，在使用 `@appliance == 'air conditioner'` 時，只會評估第一個偵測到之 `@appliance` 實體的值。但是，使用 `@appliance:(air conditioner)` 則會展開為 `entity['appliance'].contains('air conditioner')`，如此只要在使用者輸入中偵測到至少有一個值為 'air conditioner' 的 `@appliance` 實體就會相符。

## 條件用法提示
{: #dialog-tips-condition-usage}

- **檢查具有特殊字元的值**：如果您要檢查實體或環境定義變數是否包含值，而且該值包含特殊字元（例如單引號 (')），則必須用括弧括住您要檢查的值。例如，若要確認實體或環境定義變數是否包含名稱 `O'Reilly`，您必須用括弧括住此名稱。

  `@person:(O'Reilly)` 及 `$person:(O'Reilly)`

  助理會將這些速記參照轉換成這些完整的 SpEL 表示式：

  `entities['person']?.contains('O''Reilly')` 及 `context['person'] == 'O''Reilly'`

  SpEL 會使用第二個單引號來跳出名稱中的單一單引號。
  {: note}

- **檢查多個值**：如果您要檢查多個值，則可以建立條件，以使用 OR 運算子 (`||`) 在條件中列出多個值。例如，若要定義一個當環境定義變數 `$state` 包含 Massachusetts、Maine 或 New Hampshire 的縮寫時為 true 的條件，您可以使用此表示式：

  `$state:MA || $state:ME || $state:NH`

- **檢查數值**：比較數值時，請先確定您所要檢查的實體或變數具有一個值。如果實體或變數沒有數值，則在數值比較中會將其視為具有空值 (0)。

  例如，您想要檢查使用者在使用者輸入中所指定的金額值是否小於 100。如果您使用條件 `@price < 100`，且 `@price` 實體是空值，則會將條件評估為 `true`，因為 0 小於 100，即使未曾設定價格也是如此。若要防止此類型的不正確結果，請使用條件，例如 `@price AND @price < 100`。如果 `@price` 沒有值，則此條件會正確地傳回 false。

- **檢查具有特定目的名稱型樣的目的**：您可以使用條件，以尋找與型樣相符的目的。例如，若要尋找目的名稱開頭為 'User_' 之任何偵測到的目的，您可以在條件中使用如下語法：

  `intents[0].intent.startsWith("User_")`

  不過，當您這樣做時，會考慮所有偵測到的目的，即使是信賴度小於 0.2 的目的也一樣。也請確認，並未傳回 Watson 根據其信賴分數而視為不相關的目的。若要這樣做，請如下所示變更條件：

  `!irrelevant && intents[0].intent.startsWith("User_")`

- **模糊比對對實體辨識的影響**：如果使用實體作為條件，並已啟用模糊比對，則只有在相符項的信賴度大於 30% 時，才會將 `@entity_name` 評估為 true。亦即，只有在 `@entity_name.confidence > .3` 時。

## 儲存並辨識輸入中的實體型樣群組
{: #dialog-tips-get-pattern-groups}

若要將型樣實體的值儲存在環境定義變數，請在實體名稱後面附加 .literal。使用此語法可確保使用者輸入中符合所指定型樣的確切文字段會儲存在變數。

|變數          |值                                  |
|------------|---------------------|
|email      | <? @email.literal ?> |

若要將單一群組中的文字儲存在已定義群組的型樣實體，請指定您要儲存的群組陣列數目。例如，假設 @phone_number 實體的實體型樣定義如下（請記住，括弧表示型樣群組）：

`\b((958)|(555))-(\d{3})-(\d{4})\b`

若只要儲存使用者輸入中所指定電話號碼的區域碼，您可以使用下列語法：

|變數          |值                                  |
|----------------|-------------------------------|
|area_code      | <? @phone_number.groups[1] ?> |

群組是由用來定義群組型樣的正規表示式所分隔。例如，如果符合實體 `@phone_number` 中所定義型樣的使用者輸入為：`958-234-3456`，則會建立下列群組：

|群組號碼     |正規表示式引擎值    |對話值         |說明        |
|--------------|---------------------|----------------|-------------|
|groups[0]    |`958-234-3456`      |`958-234-3456` |第一個群組一律是完整比對字串。|
|groups[1]    |`((958)`l`(555))`   |`958`          |符合第一個已定義群組之正規表示式的字串，即 `((958)`l`(555))`。|
|groups[2]    |`(958)`             |`958`          |符合內含為 OR 表示式 `((958)`l`(555))` 中第一個運算元的群組|
|groups[3]    |`(555)`             |`null`         |不符合內含為 OR 表示式 `((958)`l`(555))` 中第二個運算元的群組|
|groups[4]    |`(\d{3})`           |`234`          |符合針對群組所定義之正規表示式的字串。|
|groups[5]    |`(\d{4})`           |`3456`         |符合針對群組所定義之正規表示式的字串。|
{: caption="群組詳細資料" caption-side="top"}

為了協助您解密要使用哪個群組號碼來擷取感興趣的輸入區段，您可以一次擷取所有群組的相關資訊。請使用下列語法，來建立可傳回所有已分組型樣實體相符項陣列的環境定義變數：

|變數          |值                                  |
|--------------------------|----------------------------|
|array_of_matched_groups  | <? @phone_number.groups ?> |

請使用「試用」窗格來輸入部分測試電話號碼值。針對輸入 `958-123-2345`，此表示式會將 `$array_of_matched_groups` 設為 `["958-123-2345","958","958",null,"123","2345"]`。

然後，您可以從 0 開始計算陣列中的每一個值，以取得其群組號碼。

|陣列元素值          |陣列元素號碼         |
|---------------------|----------------------|
|"958-123-2345"      |0 |
|"958"               |1 |
|"958"               |2 |
|null                |3 |
|"123"               |4 |
|"2345"              |5 |
{: caption="陣列元素" caption-side="top"}

從結果中，您可以判定若要擷取電話號碼的最後四位數，您需要群組 #5（舉例說明）。

若要傳回建立用來代表已分組型樣實體的 JSONArray 結構，請使用下列語法：

|變數          |值                                  |
|----------------------|---------------------------------|
|json_matched_groups  | <? @phone_number.groups_json ?> |

此表示式會將 `$json_matched_groups` 設為下列 JSON 陣列：

```json
[
  {"group": "group_0","location": [0, 12]},
  {"group": "group_1","location": [0, 3]},
  {"group": "group_2","location": [0, 3]},
  {"group": "group_3"},
  {"group": "group_4","location": [4, 7]},
  {"group": "group_5","location": [8, 12]}
]
```
{: codeblock}

`location` 是實體的內容，其使用以零起始的字元偏移，來指出偵測到的實體值在輸入文字中的開始及結束位置。
{: note}

如果您預期要在輸入中提供兩個電話號碼，則可以檢查兩個電話號碼。例如，如果有兩個電話號碼的話，請使用下列語法來擷取第二個號碼的區域碼。

|變數          |值                                  |
|------------------|---------------------------------------------|
|second_areacode  | <? entities['phone_number'][1].groups[1] ?> |

如果輸入是 `I want to change my phone number from 958-234-3456 to 555-456-5678`，則 `$second_areacode` 等於 `555`。

## 檢視 API 呼叫詳細資料
{: #dialog-tips-inspect-api}

當您使用「試用」窗格來測試對話時，您可能想要知道從服務傳回的基礎 API 呼叫看起來像怎樣。您可以使用 Web 瀏覽器所提供的開發人員工具來檢查它們。

例如，從 Chrome 中開啟開發人員工具。按一下「網路」工具。「名稱」區段會列出多個 API 呼叫。按一下與測試話語相關聯的訊息呼叫，然後按一下「回應」直欄，以查看 API 回應內文。它會列出在使用者輸入中辨識到的目的和實體及其信賴分數，以及在呼叫時的環境定義變數值。若要以結構化格式檢視回應內文，請按一下「預覽」直欄。

![顯示如何使用 Chrome Web 瀏覽器開發人員工具來檢視 API 呼叫詳細資料。](images/api-browser-dev.png)
