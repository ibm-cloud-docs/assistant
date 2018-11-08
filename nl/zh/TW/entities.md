---

copyright:
  years: 2015, 2018
lastupdated: "2018-01-30"

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

# 定義實體

***實體*** 代表與使用者用途相關的物件類別或資料類型。藉由辨識使用者輸入中提及的實體，{{site.data.keyword.conversationshort}} 服務可以選擇達成目的所要採取的特定動作。

<iframe class="embed-responsive-item" id="youtubeplayer" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/kAZ9m-oCKxM" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

## 實體限制
{: #entity-limits}

您可以建立的實體、實體值及同義字數目，取決於您的 {{site.data.keyword.conversationshort}} 服務方案：

| 服務方案          | 每個工作區的實體數       | 每個工作區的實體值          | 每個工作區的實體同義字數      |
|-------------------|-----------------------:|----------------------------:|--------------------------------:|
| 標準/進階         |                          1000 |                            100,000 |                              100,000 |
| 精簡              |                            25 |                            100,000 |                              100,000 |

您為使用而啟用的系統實體會計入方案使用總計。

## 建立實體
{: #creating-entities}

使用 {{site.data.keyword.conversationshort}} 工具來建立實體。

1.  在 {{site.data.keyword.conversationshort}} 工具中，開啟工作區，然後按一下**實體**標籤。如果看不到**實體**，請使用 ![功能表](images/Menu_16.png) 功能表來開啟頁面。

1.  按一下**新增實體**。

    您也可以按一下**使用系統實體**，從 {{site.data.keyword.IBM_notm}} 所提供可套用至任何使用案例的共用實體清單中進行選取。如需詳細資料，請參閱[啟用系統實體](#enable_system_entities)。

1.  在**實體名稱**欄位中，鍵入實體的敘述性名稱。

    實體名稱可以包含字母（Unicode 形式）、數字、底線及連字號。例如：
    - `@location`
    - `@menu_item`
    - `@product`

    在 {{site.data.keyword.conversationshort}} 工具中建立實體時，請不要在實體名稱中包含 `@`。實體名稱不能包含空格或長度超過 64 個字元。實體名稱的開頭不能是 `sys-` 字串，此字串保留供系統實體使用。
    {: tip}

1.  選取**建立實體**。

    ![建立實體的畫面擷取](images/create_entity.png)

1.  在**值名稱**欄位中，鍵入實體可能值的文字，然後按 `Enter` 鍵。實體值可以是任何字串，長度最多為 64 個字元。

    > **重要事項：**請不要在實體名稱或值中包含機密或個人資訊。名稱和值可以顯示在應用程式的 URL 中。

1.  針對**模糊符合**，按一下按鈕以選取開啟或關閉；依預設，會關閉模糊符合。此特性適用於[支援的語言](lang-support.html)主題中指出的語言。
 {: #fuzzy-matching}

    您可以開啟模糊符合，以改善服務辨識使用者輸入術語的能力，而其使用的語法與實體類似，但不需要完全符合。模糊符合有三個元件 - 詞幹分析、拼字錯誤及部分符合：
    - *詞幹分析* - 此特性辨識具有數個文法形式之實體值的詞幹形式。例如，'bananas' 的詞幹應該是 'banana'，而 'running' 的詞幹應該是 'run'。
    - *拼字錯誤* - 儘管存在拼字錯誤或些微的語法差異，但是此特性還是可以將使用者輸入對映至適當的對應實體。例如，如果您將 *giraffe* 定義為 animal 實體的同義字，而且使用者輸入包含 *giraffes* 或 *girafe* 一詞，則模糊符合可以將詞彙正確地對映至 animal 實體。
    - *部分符合* - 使用部分符合，此特性會自動建議使用者定義實體中的子字串型同義字，並指派較低的信任評分（與確切的實體符合相較之下）。

    **附註** - 針對英文，模糊符合可防止將一些常用的有效英文單字擷取為給定實體的模糊符合項。此特性使用標準英文字典單字。您也可以定義英文實體值/同義字，而模糊符合只會比對您定義的實體值/同義字。例如，模糊符合可能會比對 `unsure` 一詞與 `insurance`；但是，如果您已將 `unsure` 定義為 `@option` 這類實體的值/同義字，則 `unsure` 一律會符合 `@option`，而不是 `insurance`。

1.  輸入值名稱之後，就可以從*類型* 下拉功能表中選取`同義字`或`型樣`，以新增該實體值的任何同義字，或定義該實體值的特定型樣。

    ![值的類型選取器](images/value_type.png)

    > **附註：**您可以新增單一實體值的同義字*或* 型樣，但不能同時新增兩者。

    - 在**同義字**欄位中，鍵入實體值的任何同義字。同義字可以是任何字串，長度最多為 64 個字元。

      ![定義實體的畫面擷取](images/define_entity.png)
    - **型樣**欄位可讓您定義實體值的特定型樣。在欄位中，**必須**將型樣輸入為正規表示式。
  

      ![定義型樣實體的畫面擷取](images/patternents1.png)
      {: #pattern-entities}

      在此範例中，針對實體 *ContactInfo*，phone、email 及 website 值的型樣可以定義如下：
      - phone
        - `localPhone`：`(\d{3})-(\d{4})`，例如：426-4968
        - `fullUSphone`：`(\d{3})-(\d{3})-(\d{4})`，例如：800-426-4968
        - `internationalPhone`：`^(\(?\+?[0-9]*\)?)?[0-9_\- \(\)]*$`，例如，+44 1962 815000
      - `email`：`\b[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Za-z]{2,}\b`，例如 name@ibm.com
      - `website`：`(https?:\/\/)?([\da-z\.-]+)\.([a-z\.]{2,6})([\/\w \.-]*)*\/?$`，例如：https://www.ibm.com

      通常，使用型樣實體時，需要從對話樹狀結構中將符合型樣的文字儲存在環境定義變數（或動作變數）。

      假設您向使用者詢問其電子郵件位址。對話節點條件將會包含與 `@contactInfo:email` 類似的條件。若要將使用者輸入的電子郵件指派為環境定義變數，可以使用下列語法來擷取對話節點之回應區段內的型樣相符項：

      ```json
      {
          "context" : {
            "email": "<? @contactInfo.literal ?>"
          }
      }
      ```
      {: screen}
      {: #capture-group}

      *擷取群組* - 對於正規表示式，會將在成對一般括弧內的任何型樣部分擷取為群組。例如，實體值 `fullUSphone` 包含三個已擷取的群組：

        - `(\d{3})` - 美國區域碼
        - `(\d{3})` - 字首
        - `(\d{4})` - 行號

      群組可能十分有用，例如，如果您想要 {{site.data.keyword.conversationshort}} 服務要求使用者輸入其電話號碼，然後只使用回應中其所提供號碼的區域碼。

      若要將使用者輸入的區域碼指派為環境定義變數，可以使用下列語法來擷取對話節點之回應區段內的群組相符項：

        ```json
        {
            "context" : {
            "area_code": "<? @fullUSphone.groups[1] ?>"
            }
        }
        ```
       {: screen}

      如需在對話運行環境中使用擷取群組的相關資訊，請參閱[在環境定義變數中儲存型樣實體值](dialog-overview-context-groups.html)。

      {{site.data.keyword.conversationshort}} 服務所採用的型樣相符引擎具有一些語法限制，這些限制是必要的，以避免使用其他正規表示式引擎時可能發生的效能問題。
        - 實體型樣不得包含：
          - 正重複次數（例如 `x*+`）
          - 逆參照（例如 `\g1`）
          - 條件式分支（例如 `(?(cond)true)`）
        - 若型樣實體的開頭或結尾為 Unicode 字元，並且包含單字界限（例如 `\bš\b`），則型樣相符無法正確地比對單字界限。在此範例中，針對輸入 `š zkouška`，相符項會傳回 `Group 0: 6-7 š` (`š zkou`_**`š`**_`ka`)，而不是正確的 `Group 0: 0-1 š` (_**`š`**_ `zkouška`)。

      根據 Java 正規表示式引擎，正規表示式引擎是鬆散的。如果您嘗試透過 API 或從 {{site.data.keyword.conversationshort}} 服務工具使用者介面內上傳不受支援的型樣，則 {{site.data.keyword.conversationshort}} 服務不會產生錯誤。

1.  按一下**新增值**，然後重複新增其他實體值的處理程序。

1.  在完成新增實體值時，請選取 ![關閉箭頭](images/close_arrow.png)，以完成建立實體。

### 結果

您建立的實體即會新增至**實體**標籤，而且系統會開始對新資料訓練它自己。

## 編輯實體

您可以按一下清單中的任何實體，以開啟它來進行編輯。您可以重新命名或刪除實體，也可以新增、編輯或刪除值、同義字或型樣。

> **附註**：如果您將實體類型從 `synonym` 變更為 `pattern`（反之亦然），則會轉換現有值，但可能不實用。

## 搜尋實體

使用「搜尋」特性，尋找實體名稱、值及同義字。

1.  選取導覽列中的**實體**標籤，然後選取*我的實體*。

    ![「實體」標籤概觀](images/entity_oview.png)

    **附註**：系統實體是無法搜尋的。

1.  選取「搜尋」圖示：![「搜尋」圖示](images/search_icon.png)

1.  輸入搜尋詞彙或詞組。

    ![實體搜尋詞彙](images/searchent_1.png)

    **附註**：第一次搜尋時，會建立索引；您可能會看到在編製內容索引時等待的訊息。

### 結果

顯示包含搜尋詞彙及對應範例的實體。選取任何結果，以開啟進行編輯。

  ![傳回的實體搜尋](images/searchent_2.png)

## 匯入實體

如果您有大量實體，則可能會發現從逗點區隔值 (CSV) 檔案匯入它們更為容易，而不需要在 {{site.data.keyword.conversationshort}} 工具中逐一定義它們。

1.  將實體收集到 CSV 檔案，或將它們從試算表匯出至 CSV 檔案。檔案中每一行的必要格式如下：

    ```
    <entity>,<value>,<synonyms>
    ```
    {: screen}

    其中 &lt;entity&gt; 是實體的名稱、&lt;value&gt; 是實體的值，而 &lt;synonyms&gt; 是該值的同義字清單（以逗點區隔）。

    ```
    weekday,Monday,Mon
    weekday,Tuesday,Tue,Tues
    weekday,Wednesday,Wed
    weekday,Thursday,Thur,Thu,Thurs
    weekday,Friday,Fri
    weekday,Saturday,Sat
    weekday,Sunday,Sun
    month,January,Jan
    month,February,Feb
    month,March,Mar
    month,April,Apr
    month,May
    ```
    {: screen}

    匯入 CSV 檔案也支援型樣。任何使用 `/` 包裝的字串都會被視為型樣（相對於同義字）。

    ```
    ContactInfo,localPhone,/(\d{3})-(\d{4})/
    ContactInfo,fullUSphone,/(\d{3})-(\d{3})-(\d{4})/
    ContactInfo,internationalPhone,/^(\(?\+?[0-9]*\)?)?[0-9_\- \(\)]*$/
    ContactInfo,email,/\b[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Za-z]{2,}\b/
    ContactInfo,website,/(https?:\/\/)?([\da-z\.-]+)\.([a-z\.]{2,6})([\/\w \.-]*)*\/?$/
    ```
    {: screen}

        儲存含 UTF-8 編碼且沒有位元組順序標記 (BOM) 的 CSV 檔案。CSV 檔案大小上限為 10MB。如果您的 CSV 檔案較大，請考慮將它分割為多個檔案，並分別進行匯入。在 {{site.data.keyword.conversationshort}} 工具中，開啟工作區，然後按一下**實體**標籤。
    {: tip}

1.  按一下 ![匯入](images/importGA.png)，然後拖曳檔案，或瀏覽以從電腦中選取檔案。檔案會進行驗證並匯入，而且系統會開始對新資料訓練它自己。

### 結果

您可以在「實體」標籤上檢視匯入的實體。您可能需要重新整理頁面才能看到新的實體。

## 匯出實體
{: #export_entities}

您可以將若干實體匯出至 CSV 檔案，然後針對另一個 {{site.data.keyword.conversationshort}} 應用程式匯入及重複使用它們。

匯出 CSV 檔案支援型樣。任何使用 `/` 包裝的字串都會被視為型樣（相對於同義字）。
{: tip}

1.  選取您要的實體，然後選取**匯出**。

    ![匯出實體按鈕](images/ExportEntity.png)

## 刪除實體
{: #delete_entities}

您可以選取一些要刪除的實體。

**重要事項**：藉由刪除實體，您也會刪除所有關聯的值、同義字或型樣，稍後無法擷取這些項目。所有參照這些實體或值的對話節點都必須手動更新成不再參照已刪除的內容。

1.  選取您要的實體，然後選取**刪除**。

    ![刪除實體按鈕](images/DeleteEntity.png)

## 啟用系統實體
{: #enable_system_entities}

{{site.data.keyword.conversationshort}} 服務提供若干*系統實體*，這些系統實體是可用於任何應用程式的一般實體。啟用系統實體可將許多使用案例通用的訓練資料快速移入您的工作區。

系統實體可用來辨識其代表之物件類型的更大範圍的值。例如，`@sys-number` 系統實體符合任何數值，包括整數、小數位數，甚至是以單字形式寫出的數字。

系統實體進行集中維護，因此可自動提供所有更新。您無法修改系統實體。

1.  在「實體」標籤上，按一下**系統實體**。

    ![「系統實體」標籤的畫面擷取](images/system_entities_1.png)

1.  瀏覽系統實體清單，以選擇對應用程式有用的系統實體。
    - 若要查看系統實體的相關資訊（包括相符輸入的範例），請按一下清單中的實體。
    - 如需可用系統實體的詳細資料，請參閱[系統實體](system-entities.html)。

1.  按一下系統實體旁的切換開關，以啟用或停用該系統實體。

### 結果

啟用系統實體之後，{{site.data.keyword.conversationshort}} 服務就會開始重新訓練。訓練完成之後，您就可以使用實體。
