---

copyright:
  years: 2015, 2019
lastupdated: "2019-08-06"

keywords: intent, intent conflicts, annotate

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

# 定義目的
{: #intents}

***目的*** 是在客戶輸入中所表達的用途或目標，例如回答問題或處理帳單付款。藉由辨識在客戶輸入中所表達的目的，{{site.data.keyword.conversationshort}} 服務可以選擇正確的對話流程來進行回應。
{: shortdesc}

<iframe class="embed-responsive-item" id="youtubeplayer" title="使用目的" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/OPdOCUPGMIQ" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

## 目的建立概觀
{: #intents-described}

- 規劃應用程式的目的。

  請考慮客戶可能想要執行的作業，以及您希望應用程式能夠代替他們處理的事情。例如，您可能希望應用程式協助客戶進行購買。如果是這樣，您可以新增 `#buy_something` 目的。（當成字首新增至目的名稱的 `#`，有助於將它明確地識別為目的。）

- 教導 Watson 您的目的。

  決定您要應用程式為您的客戶處理哪些商業要求之後，您必須教導 Watson 這些要求的相關資訊。對於每個商業目標（例如 `#buy_something`），您必須至少提供 5 個您的客戶一般會用來指出其目標的話語範例。例如，`I want to make a purchase.`。
  
  理想情況下，請尋找可從現有商業處理程序中擷取的實際使用者話語範例。使用者範例應針對您的特定業務進行自訂。例如，如果您是保險公司，則使用者範例可能類似於：`I want to buy a new XYZ insurance plan.`。
  
  助理會使用您提供的範例來建置機器學習模型，此模型可辨識相同及相似類型的話語，然後將它們對映至適當的目的。

請先從幾個目的開始，然後在您反覆擴展應用程式範圍時測試這些目的。

![僅限加值或超值方案](images/plus.png) 如果您已有客服中心的會談記錄或已有從線上應用程式收集的客戶查詢，請利用這些資料。與 Watson 共用實際客戶話語，並讓 Watson 針對您的需求建議最佳目的和目的使用者範例。如需詳細資料，請參閱[定義目的時取得協助](/docs/services/assistant?topic=assistant-intent-recommendations)。
    

## 建立目的
{: #intents-create-task}

1.  開啟對話技能。技能會開啟到**目的**頁面。

1.  選取**建立目的**。

1.  在**目的名稱**欄位中，鍵入目的的名稱。
    - 目的名稱可以包含字母（Unicode 編碼）、數字、底線、連字號及句點。
    - 名稱不可包含 `..` 或任何只有句點的其他字串。
    - 目的名稱不可包含空格，且不得超過 128 個字元。以下是目的名稱的範例：
        - `#weather_conditions`
        - `#pay_bill`
        - `#escalate_to_agent`

    將 `#` 記號加在目的名稱前面，以協助將該詞彙識別為目的。您不需要新增它。
    {: tip}

    （選用）在**說明**欄位中新增目的的說明。

1.  選取**建立目的**，以儲存目的名稱。

    ![顯示新目的定義的畫面擷取](images/create_intent.png)

1.  接下來，在**新增使用者範例**欄位中，鍵入目的的使用者範例文字。範例可以是任何字串，長度最多為 1024 個字元。下列話語是可能的 `#pay_bill` 目的範例：
    - `I need to pay my bill.`
    - `Pay my account balance`
    - `make a payment`

    若要瞭解在使用者範例中包含實體參照的影響，請參閱[如何處理實體參照](#intents-entity-references)。
    {: tip}

    當應用程式與 {{site.data.keyword.conversationshort}} 互動時，可能會在 URL 中公開目的名稱及範例文字。請不要在這些構件中包含機密或個人資訊。
    {: important}

1.  按一下**新增範例**以儲存使用者範例。

1.  重複相同的處理程序來新增其他範例。

    針對每一個目的，至少提供五個範例。
    {: important}

    ![僅限加值或超值方案](images/plus.png) 若要取得建立使用者範例的協助，請參閱[取得目的使用者範例建議](/docs/services/assistant?topic=assistant-intent-recommendations#intent-recommendations-get-example-recommendations)。

1.  當您完成新增範例時，請按一下 ![關閉箭頭](images/close_arrow.png)，以完成建立目的。

系統會開始根據您新增的目的及使用者範例自行訓練。

## 如何處理實體參照
{: #intents-entity-references}

當您在使用者範例中包含實體提及項目時，機器學習模型會在以下情境中以不同方式使用資訊：

- [在目的範例中參照實體值及同義字](#intents-related-entities)
- [已註釋的提及項目](#intents-annotated-mentions)
- [在目的範例中直接參照實體名稱](#intents-entity-as-example)

### 在目的範例中參照實體值及同義字
{: #intents-related-entities}

如果您已定義或計劃要定義與此目的相關的實體，請在部分範例中提及實體值或同義字。這麼做有助於建立目的與實體之間的關係。這層關係很薄弱，但的確會通知模型。

![顯示目的定義的畫面擷取](images/define_intent.png)

*重要事項*：

  - 目的範例資料應該是使用者提供的資料中具有代表性和典型性的資料。範例可能收集自實際使用者資料或特定領域的專家。重要的是資料本質具代表性且正確。
  - 訓練及測試資料（僅適用於評估用途）都應該會反映目的在實際使用時的分佈情況。一般而言，較常使用的目的相對會有較多的範例，而且回應涵蓋面也較佳。
  - 您可以在範例文字中包含標點符號，只要標點符號是自然地出現即可。如果您認為部分使用者會以包含標點符號的範例來表示其目的，但部分使用者不會，則請包含這兩種版本。一般而言，涵蓋的型樣類型愈多，回應就愈佳。

### 已註釋的提及項目
{: #intents-annotated-mentions}

定義實體時，您可以直接從現有目的使用者範例中註釋實體的提及項目。目的分類模型*不會* 使用目的與實體之間以這種方式識別的關係。不過，當您將提及項目新增至實體時，也會將它新增至該實體作為新值。此外，當您將提及項目新增至現有實體值時，還會將它新增至該實體值作為新同義字。目的分類會使用目的使用者範例中的這些字典參照類型，來建立目的與實體之間的弱參照。


如需環境定義實體的相關資訊，請參閱[新增環境定義實體](/docs/services/assistant?topic=assistant-entities#entities-create-annotation-based)。

### 在目的範例中直接參照實體名稱
{: #intents-entity-as-example}

這是進階方法。如果使用此方法，則必須以一致的方式使用。
{: note}

您可以選擇在目的範例中直接參照實體。例如，假設您有一個名為 `@PhoneModelName` 的實體，其中包含值 *Galaxy S8*、*Moto Z2*、*LG G6* 和 *Google Pixel 2*。在您建立目的（例如，`#order_phone`）時，可以提供訓練資料，如下所示：

- 我可以取得 `@PhoneModelName` 嗎？
- 協助我訂購 `@PhoneModelName`。
- `@PhoneModelName` 有現貨嗎？
- 將 `@PhoneModelName` 新增至我的訂單。

![顯示目的定義的畫面擷取](images/define_intent_entity.png)

目前，您只能直接參照您所定義的同義字實體（會忽略型樣值）。您不可以使用[系統實體](/docs/services/assistant?topic=assistant-system-entities)。

如果您選擇在訓練資料的*任何位置* 將實體參照為目的範例（例如，`@PhoneModelName`），則會抵消在目的範例的其他位置使用直接參照的值（例如，*Galaxy S8*）。所有目的便會使用「實體作為目的範例」的方法。您不能僅針對特定目的套用此方法。
{: important}

實際上，這表示如果您先前根據直接參照 (*Galaxy S8*) 訓練過大部分目的，而現在只將實體參照 (`@PhoneModelName`) 用於一個目的，則此變更會影響先前的訓練。如果您選擇使用 `@Entity` 參照，則必須將所有先前的直接參照取代為 `@Entity` 參照。

使用已定義 10 個值的 `@Entity` 來定義一個範例目的**並不**等同於指定該範例目的 10 次。{{site.data.keyword.conversationshort}} 服務不會給與那一個範例目的語法如此高的權重。

## 測試目的
{: #intents-test}

完成建立新的目的之後，您就可以測試系統，看看它是否如預期般辨識目的。

1.  按一下 ![詢問 Watson](images/ask_watson.png) 圖示。

1.  在「試用」窗格中，輸入問題或其他字串，然後按 Enter 鍵，以查看辨識的目的。如果辨識到錯誤的目的，您可以將此文字當作範例新增至正確的目的，來改善模型。

    如果您最近變更過技能，則可能會看到一則訊息，指出系統仍在重新訓練中。如果您看到此訊息，請等到訓練完成之後再進行測試：
    {: tip}

    ![顯示重新訓練訊息的畫面擷取](images/training.png)

    回應指出從您的輸入中辨識到的目的。

    ![測試目的的畫面擷取](images/test_intents.png)

1.  如果系統無法辨識正確的目的，您可以予以更正。若要更正辨識到的目的，請選取顯示的目的，然後從清單選取正確的目的。提交更正之後，系統會自動自行重新訓練以納入新資料。

    ![更正辨識到之目的的畫面擷取](images/correct_intent.png)

1.  {: #intents-mark-irrelevant}如果輸入與應用程式中的任何目的都無關，您可以選取顯示的目的，然後按一下**標示為不相關**，藉以教導助理。

    ![「標示為不相關」畫面擷取](images/irrelevant.png)

    如需此動作的相關資訊，請參閱[教導助理學習要忽略的主題](/docs/services/assistant?topic=assistant-logs#logs-mark-irrelevant)。

如果未正確地辨識您的目的，請考慮進行下列類型的變更：

- 將無法辨識的文字當作範例新增至正確的目的。
- 將現有範例從某個目的移至另一個目的。
- 請考慮您的目的是否太過類似，並視情況重新定義。

## 絕對評分
{: #intents-absolute-scoring}

{{site.data.keyword.conversationshort}} 服務會針對每個目的的信賴度進行獨立評分，與其他目的沒有關係。此方法可增加彈性；可以偵測單一使用者輸入中的多個目的。這也表示系統可能根本不會傳回目的。如果最高目的的信賴分數很低（小於 0.2），則最高目的會包含在 API 所傳回的目的陣列中，但是不會觸發以該目的為條件的任何節點。如果您要偵測「偵測不到任何信賴分數良好的目的」的情況，請在您的對話節點中使用 `irrelevant` 特殊條件。如需相關資訊，請參閱[特殊條件](/docs/services/assistant?topic=assistant-dialog-overview#dialog-overview-special-conditions)。

隨著目的信賴分數變更，您的對話可能需要重組。例如，如果對話節點在其條件中使用一個目的，而該目的的信賴分數開始持續地掉到 0.2 以下，則會停止處理此對話節點。如果信賴分數變更，對話的行為也會變更。

## 目的限制
{: #intents-limits}

可以建立的目的數和範例數取決於 {{site.data.keyword.conversationshort}} 方案類型：

| 方案 | 每個技能的目的數 | 每個技能的範例數 |
|------------------|------------------:|-------------------:|
|超值              |             2,000 |             25,000 |
|加值              |             2,000 |             25,000 |
|標準              |             2,000 |             25,000 |
|精簡、加值試用|               100 |             25,000 |
{: caption="方案詳細資料" caption-side="top"}

## 編輯目的
{: #intents-edit}

您可以按一下清單中的任何目的，以開啟它來進行編輯。您可以進行下列變更：

- 重新命名目的。
- 刪除目的。
- 新增、編輯或刪除範例。
- 將範例移至不同的目的。

您可以按 Tab 鍵從目的名稱移至每個範例，並視需要編輯範例。

若要移動或刪除範例，請按一下相關聯的勾選框，然後按一下**移動**或**刪除**。

  ![顯示如何移動或刪除範例的畫面擷取](images/move_example.png)

## 搜尋目的
{: #intents-search}

請使用「搜尋」特性，來尋找使用者範例、目的名稱及說明。

1.  從**目的**頁面中，按一下「搜尋」圖示。

    ![「目的」頁面標頭中的搜尋圖示](images/header-search-icon.png)

1.  輸入搜尋詞彙或詞組。

    ![目的搜尋詞彙](images/searchint_1.png)

即會顯示包含搜尋詞彙的目的及對應的範例。

  ![傳回的目的搜尋](images/searchint_2.png)

## 匯出目的
{: #intents-export}

您可以將若干目的匯出至 CSV 檔案，然後針對另一個 {{site.data.keyword.conversationshort}} 應用程式匯入及重複使用這些目的。

1.  在**目的**頁面上，從清單中選取所需目的，然後按一下**匯出**。

    ![匯出選項](images/ExportIntent.png)

## 匯入目的及範例
{: #intents-import}

如果您有大量目的及範例，則可能會發現從逗點區隔值 (CSV) 檔案匯入它們會比逐一定義它們更為容易。請務必從檔案所包含的使用者範例中，移除所有個人資料。

或者，您可以上傳具有原始使用者話語的檔案（例如，從客服中心日誌），並讓 Watson 從資料中尋找使用者範例的候選項。如需相關資訊，請參閱[從日誌檔新增範例](/docs/services/assistant?topic=assistant-intent-recommendations#intent-recommendations-get-example-recommendations)。此特性僅適用於「加值」或「超值」方案使用者。

1.  將目的及範例收集到 CSV 檔案，或將它們從試算表匯出至 CSV 檔案。檔案中每一行的必要格式如下：

    ```
    <example>,<intent>
    ```
    {: screen}

    其中 `<example>` 是使用者範例的文字，而 `<intent>` 是您要範例比對的目的名稱。例如：

    ```
    Tell me the current weather conditions.,weather_conditions
    Is it raining?,weather_conditions
    What's the temperature?,weather_conditions
    Where is your nearest location?,find_location
    Do you have a store in Raleigh?,find_location
    ```
    {: screen}

    **重要事項：**儲存含 UTF-8 編碼且沒有位元組順序標記 (BOM) 的 CSV 檔案。

1.  從**目的**頁面中，按一下*匯入* 圖示 ![「匯入」圖示](images/importGA.png)，然後拖曳檔案，或瀏覽以從電腦中選取檔案。

    ![匯入選項](images/ImportIntent.png)

    **重要事項：**CSV 檔案大小上限為 10 MB。如果您的 CSV 檔案較大，請考慮將它分割為多個檔案，並分別進行匯入。

    檔案會進行驗證並匯入，而系統會開始根據新資料自行訓練。

您可以在**目的**標籤上檢視匯入的目的及對應的範例。您可能需要重新整理頁面才能看到新目的和範例。

## 解決目的衝突 ![僅限「加值」或「超值」](images/plus.png)
{: #intents-resolve-conflicts}

此特性僅適用於「加值」或「超值」使用者。
{: note}

當*個別* 目的中有兩個以上目的範例非常類似，以致於讓 {{site.data.keyword.conversationshort}} 感到混淆，不知要使用哪個目的時，{{site.data.keyword.conversationshort}} 應用程式即會偵測到衝突。

若要解決衝突，請執行下列動作：

1.  從**目的**頁面中，檢閱有衝突的任何目的。

    ![「目的衝突」清單](images/ConflictIntent1.png)

    切換**僅顯示衝突**開關，以查看僅包含有衝突目的的清單。
    {: tip}

    ![「僅限衝突」視圖](images/ConflictIntent2.png)

1.  開啟目的衝突。針對造成衝突的目的範例，按一下**解決衝突**。

    ![衝突的目的範例](images/ConflictIntent3.png)

1.  現在，您可以選擇將衝突範例移至另一個目的，或者選擇刪除整個衝突範例。

    在此情況下，`Cancel my order` 及 `I want to cancel my order` 這兩個範例會同時出現在 `#cancel` 目的及 `#eCommerce_Cancel_Product_Order` 目的中。

    ![衝突的目的範例](images/ConflictIntent4.png)

    其他使用者範例是訓練範例，不一定會發生衝突，但類似於發生衝突的範例。即會顯示它們來提供環境定義，以協助解決衝突。

1.  選取 `Cancel my order` 及 `I want to cancel my order` 這兩個範例，然後將其從 `#cancel` 目的移至 `#eCommerce_Cancel_Product_Order` 目的：

    ![衝突的目的範例](images/ConflictIntent5.png)

1.  決定放置範例的位置時，請尋找具有同義或幾乎同義之範例的目的。

    盡可能保持每個目的的相異性，並聚焦於一個目標。如果您有兩個目的具有多個重疊的使用者範例，則可能不需要兩個個別目的。您可以將不直接重疊的使用者範例移至其中一個目的或刪除這些使用者範例，然後刪除另一個目的。
    {: tip}

    選取 `#cancel` 目的中的其他範例，然後予以刪除：

    ![衝突的目的範例](images/ConflictIntent6.png)

1.  按一下**提交**按鈕，以解決這些衝突：

    ![衝突的目的範例](images/ConflictIntent7.png)

    *重設* 選項可讓您開始在目的之間移動衝突範例。*取消* 會讓您返回目的頁面。

您已解決衝突，可以繼續檢閱其他有衝突的目的。

請觀看此視訊以進一步瞭解。

<iframe class="embed-responsive-item" id="youtubeplayer0" title="「目的衝突解決」概觀" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/9gQtjCBxjdc?rel=0" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

## 刪除目的
{: #intents-delete}

您可以選取一些要刪除的目的。

**重要事項**：藉由刪除您也會一併刪除所有相關聯範例的目的，之後就無法擷取這些項目。所有參照這些目的的對話節點，都必須手動更新成不再參照已刪除的內容。

1.  在**目的**頁面上，從清單中選取所需目的，然後按一下**刪除**。

    ![刪除選項](images/DeleteIntent.png)
