---

copyright:
  years: 2015, 2019
lastupdated: "2019-07-19"

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

# 取得定義目的的協助 ![僅限加值或超值方案](images/plus.png)
{: #intent-recommendations}

如果您具有現有企業客戶支援中心會談記錄資料，請讓 Watson 分析該資料，以瞭解支援團隊花費大部分時間處理的客戶需求。
然後，Watson 可以建議可以用於訓練助理的目的和使用者範例，讓助理未來可辨識相同和類似的要求。
{: shortdesc}

此特性適用於加值或超值方案使用者。
{: note}

客戶需求會在 {{site.data.keyword.conversationshort}} 中表示為*目的*。如果您尚未定義任何目的，則可以向 Watson 要求協助，以快速開始使用。上傳其客戶話語來自客服中心文字記錄的檔案，供 {{site.data.keyword.conversationshort}} 服務進行分析。Watson 會根據其揭示的見解，建議您應該建置的一組基本目的，以涵蓋客戶最常出現的需求。

作為客戶想要討論變更的主題，您可以使用目的使用者範例建議功能，協助將您的目的保持最新狀態並隨時間變化。

下列視訊提供三分半鐘的概觀，說明目的及目的使用者範例建議。

<iframe class="embed-responsive-item" id="youtubeplayer" title="目的建議概觀" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/64h59KqDY98?rel=0" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

若要進一步瞭解目的建議如何協助您更快速地建置有用的機器人，請[閱讀本部落格文章 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://medium.com/ibm-watson/let-watson-train-your-virtual-assistant-57bd53b12bc3){: new_window}。

發掘您的現有資料來執行下列其中一項：

- [取得目的建議](#intent-recommendations-get-intent-recommendations)
- [取得目的使用者範例建議](#intent-recommendations-get-example-recommendations)

如需此特性的語言支援的相關資訊，請參閱[支援的語言](/docs/services/assistant?topic=assistant-language-support)。

## 建立使用者範例原始檔
{: #intent-recommendations-log-files-add}

您必須先以正確的格式提供資料，Watson 才能對資料進行分析。建立逗點區隔值 (CSV) 檔案，每行一句客戶話語。理想情況下，話語是擷取自客服中心記錄的簡短詞組，其中包含實際的客戶問題和要求。每個使用者範例原始檔的大小上限為 20 MB。

請遵循以下其他準則：

  - 從檔案所包含的話語中，移除所有機密資料。

機密資料包括與可識別之自然人相關的任何資訊（例如姓名、電子郵件位址及客戶 ID），以及受管制的資料（例如受保護的健康資訊）。
  - 不要包含長度超過 1024 個字元的話語。過長的話語會被截斷。
  - 檔案必須至少包含 100 句話語；含有 500 句以上話語的檔案將會提供更適當的結果。
  - 如果話語包含逗點，請用引號括住該話語。
  - CSV 只能包含一個直欄。
  - 移除所有不是客戶產生的話語內容，包括任何真人服務專員的回應或附註。

  例如：

  ```
  What happens to my coverage if I trade in my car?
  i'd like to buy a house.
  How do I add a dependent to my plan?
  "first, i want to know if i am already registered."
  ```

您上傳的所有檔案都會在現行服務實例的所有技能間共用。當您同時要求目的建議和目的使用者範例建議時，會發掘所有可用檔案中的話語。

## 取得 Watson 目的建議
{: #intent-recommendations-get-intent-recommendations}

讓 Watson 分析您的客服中心會談記錄，並建議一些讓您開始使用的起始目的。如果您已建立某些目的，請讓 Watson 分析您的日誌，並比較其發現的目的與您的現有目的，以識別訓練資料中的間隙，並建議可填入它們的新目的。

若要使用此特性，請上傳包含客戶話語的檔案。Watson 會評估這些話語，並識別客戶經常提及的一般問題區域。然後，{{site.data.keyword.conversationshort}} 會顯示一組離散目的，用於擷取趨勢客戶需求。您可以檢閱每個建議的目的及其對應的使用者範例，以選擇您要新增至訓練資料的範例。

## 取得目的建議
{: #intent-recommendations-get-intent-recommendations-task}

開始之前，請先用您的資料建立 CSV 檔案。請參閱[建立使用者範例原始檔](#intent-recommendations-log-files-add)。

若要取得目的建議，請完成下列步驟：

1.  開啟對話技能。技能會開啟到**目的**頁面。

1.  按一下**取得建議**。

1.  **僅限第一次**：按一下**新增檔案**，然後按一下**選擇檔案**，來瀏覽並選取您先前建立的 CSV 檔。

    給 Watson 一些時間來分析資料並將話語分組。

1.  檢閱 Watson 建議的目的。

    目的建議的排序方式如下：

    1.  具有最頻繁出現的話語的目的。與這些目的相關聯的話語的頻率表明這些目的識別到最常見的客戶痛點。
    1.  這些目的與已新增到技能的目的之間的差異程度。其唯一性暗示它們可以處理目前不符合的客戶需求。
    1.  目的中使用者範例的彼此相似程度，這指出目的的強度。

    您可以將游標移至目的上方，以查看其關聯話語的少數範例。如果您要查看所有目的分組的清單，請按一下**顯示所有建議**。

1.  按一下目的，以查看與其相關聯的一組完整使用者範例，並採取下列其中一個動作：

    取消選取您不想新增為使用者範例的任何話語。

    - 若要新增以選取的話語作為使用者範例的建議目的，請按一下**建立目的**。
    - 若要改為將從建議目的中選取的話語新增至其中一個現有目的作為使用者範例，請按一下**新增至現有目的**、選擇目的，然後按一下**新增**。

您以此方式新增的目的及目的使用者範例，會計入目的及目的使用者範例總計中，每個方案都對此總計都有所限制。如需詳細資料，請參閱[目的限制](/docs/services/assistant?topic=assistant-intents#intents-limits)。

作為客戶想要討論變更的主題，您可以使用目的使用者範例建議功能，協助將您的目的保持最新狀態並隨時間變化。

## 取得目的使用者範例建議
{: #intent-recommendations-get-example-recommendations}

向 Watson 提供用於尋找目的使用者範例的資料不需要將這些範例與目的相關聯。您只要提供原始客戶話語，然後讓 Watson 選擇適合現行目的的話語。Watson 會針對每個目的分析相同的資料，以找出適合該目的的使用者範例。

開始之前，請先用您的資料建立 CSV 檔案。請參閱[建立使用者範例原始檔](#intent-recommendations-log-files-add)。

1.  遵循[建立目的](/docs/services/assistant?topic=assistant-intents#intents-creating-intents-task)中的步驟來建立目的。

1.  至少新增 5 個使用者範例，說明您預期客戶為了觸發此目的而可能說出之一般話語的完整範圍。

    這些種子使用者範例會教導 Watson 要在您上傳的檔案中尋找的話語類型。

1.  按一下**顯示建議**。

    使用者範例原始檔會在服務實例的技能之間共用。如果您的同事擁有相同實例中的技能並上傳檔案，則他們的檔案會新增至您的共用*使用者範例原始檔* 集合。

1.  **僅限第一次**：按一下**上傳檔案**，然後按一下**選擇檔案**，以瀏覽找出您先前建立的 CSV 檔案並加以選取。

    如果您上傳的檔案包含所有目的類型的話語，也沒關係。Watson 會知道您正在使用的目的，並尋找適當的範例，以對這個特定目的提供建議。

    在檔案上傳並由 Watson 進行處理後，將顯示建議的話語。如果未提供任何建議，則該檔案不會包含適合此目的的範例。

1.  如果檔案無法為任何目的提供有用的建議，您可以上傳另一個檔案，嘗試一組不同的話語。如需詳細資料，請參閱[管理上傳的檔案](#intent-recommendations-manage-files)。

1.  Watson 顯示建議後，請選取要新增為此目的的使用者範例的話語，然後按一下**新增**。或者，按**下一組**來檢閱其他話語。
1.  若要自行在 CSV 檔案的內容中搜尋使用者範例，請按一下**搜尋日誌**標籤、輸入要作為搜尋基礎的關鍵字，然後按 **Enter** 鍵。

    請遵循下列搜尋查詢語法準則：

    - 支援布林運算子（例如，`AND` 及 `OR`）。
    - 新增加上引號的文字，以搜尋完全相符的文字 ("thisstringmustbepresent")。
    - 您可以使用正規表示式（例如 `*ly`），來尋找結尾為 `ly` 的所有詞彙。
    - 下列字元用作正規表示式運算子：

      `+ - = && || > < ! ( ) { } [ ] ^ " ~ * ? : \ /`

      如果您要在搜尋詞彙中包括一個，而不將它作為運算子進行處理，則必須以反斜線 (`\`) 作為其字首。

您以此方式新增的使用者範例，會計入目的使用者範例總計中，每個方案對此總計都有所限制。如需詳細資料，請參閱[目的限制](/docs/services/assistant?topic=assistant-intents#intents-limits)。

## 管理已上傳的檔案
{: #intent-recommendations-manage-files}

上傳至少一個檔案之後，您可以開啟*使用者範例原始檔* 集合來管理檔案。上傳的檔案會新增至跨現行 {{site.data.keyword.conversationshort}} 服務實例共用的檔案集合。

1.  若要存取集合，請按一下以開啟目的、按一下**顯示建議**，然後從資訊看板中按一下**檢視檔案**。

    如果您尚未新增任何目的，請從主要「目的」頁面中，展開*目的建議* 區段，按一下**取得建議**，按一下**顯示所有建議**，然後按一下**檢視檔案**。

1.  您可以採取下列動作：

    - 若要新增檔案，請按一下**新增檔案**，然後瀏覽找出檔案並加以選取。
    - 若要刪除檔案，您必須移除從與此服務實例相關聯之任何技能上傳的所有檔案；您不能只刪除一個檔案。首先，請確定沒有其他人正在使用這些檔案，然後按一下**全部刪除**來刪除所有已上傳的檔案。

1.  關閉*使用者範例原始檔* 頁面。
