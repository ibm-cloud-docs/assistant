---

copyright:
  years: 2015, 2019
lastupdated: "2019-08-06"

keywords: context, context variable, digression, disambiguation, autocorrection, spelling correction, spell check, confidence 

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

# 對話處理方式
{: #dialog-runtime}

瞭解人員在執行時期與已部署的 {{site.data.keyword.conversationshort}} 服務實例互動時，如何處理對話。
{: shortdesc}

## 對話呼叫的結構
{: #dialog-runtime-message-anatomy}

每一個使用者詞語都會以 /message API 呼叫形式傳遞至對話。這包含使用者所製作的詞語，用於回覆來自要求他們提供相關資訊之對話的提示。部分訂閱方案包含一定數量的 API 呼叫次數，以協助瞭解呼叫的構成成分。單一 /message API 呼叫相當於單一對話開啟，其包含使用者的輸入以及對話的對應回應。

/message API 呼叫要求及回應的內文包含下列物件：

- `context`：包含要持續保存的變數。若要將資訊從某個呼叫傳遞至下一個呼叫，應用程式開發人員必須在每一個後續 API 呼叫中傳遞前一個 API 呼叫的回應環境定義。例如，對話可以收集使用者名稱，然後在後續節點中依名稱來參照使用者。下列範例顯示 context 物件在對話 JSON 編輯器中的代表方式：

  ```json
  {
    "context" : {
      "user_name" : "<? @sys-person.literal ?>"
    }
  ```
  {: codeblock}

  如需相關資訊，請參閱[跨對話開啟保留資訊](#dialog-runtime-context)。

- `input`：使用者所提交的字串。字串最多可以包含 2,048 個字元。下列範例顯示 input 物件在對話 JSON 編輯器中的代表方式：

  ```json
  {
    "input" : {
      "text" : "Where's your nearest store?"
    }
  ```
  {: codeblock}

- `output`：要傳回給使用者的對話回應。下列範例顯示 output 物件在對話 JSON 編輯器中的代表方式：

  ```json
  {
  "output": {
    "generic": [
      {
        "values": [
          {
            "text": "This is my response text."
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

在產生的 API /訊息回應中，文字回應的格式如下：

```json
{
   "text": "This is my response text.",
   "response_type": "text"
}
```

支援下列 `output` 物件 JSON 格式，以實現舊版相容性。指定使用此格式之文字回應的任何工作區都會繼續正常運作。在引進複合式回應類型時，會使用 `output.generic` 結構來擴增 `output.text` 結構，以協助支援除了文字以外的其他回應類型。建立新的節點時，請使用新的格式給自己更大的彈性，因為如果需要，您可以隨後變更回應類型。
{: note}

  ```json
  {
  "output": {
    "text": {
      "values": [
        "This is my response text."
      ]
    }
  }
  ```
  {: codeblock}

除了可以定義文字回應之外，您還可以定義數種回應類型。如需詳細資料，請參閱[回應](/docs/services/assistant?topic=assistant-dialog-overview#dialog-overview-responses)。

您可以從 [API 參考資料 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://{DomainName}/apidocs/assistant-v2){: new_window} 進一步瞭解 /message API 呼叫。

### 跨對話開啟保留資訊
{: #dialog-runtime-context}

對話技能中的對話是無狀態的，表示它不會保留與使用者不同互動之間的資訊。當您將對話技能新增至助理並予以部署時，助理會儲存一個訊息呼叫中的環境定義，然後在整個現行階段作業期間，重新提交該環境定義給下一個要求。只要使用者與助理互動，就會持續進行現行階段作業，若為「加值」或「超值」方案，則可以再閒置最多 60 分鐘（若為「精簡」或「標準」方案，則為 5 分鐘）。如果您沒有將對話技能新增至助理，則作為自訂應用程式開發人員，您必須負責維護應用程式所需的任何持續性資訊。應用程式必須尋找環境定義物件，並將其儲存至訊息 API 回應中，以及使用交談流程中所提出的下一個 /message API 要求來將它傳入環境定義物件。

自行保留資訊的其中一種方式是，在用戶端應用程式（例如，Web 瀏覽器）中將整個環境定義物件儲存至記憶體中。因為應用程式變得更為複雜，或者，如果需要傳遞及儲存個人識別資訊，則您可以儲存及擷取資料庫中的資訊。當然，最簡單的方法是讓您完全不用儲存環境定義。若要實作此方法，請將對話技能新增至助理，並讓助理為您持續追蹤環境定義。

應用程式可以將資訊傳遞給對話，而對話可以更新此資訊，並將它傳回給應用程式或後續節點。對話會使用*環境定義變數* 來執行此動作。

## 環境定義變數
{: #dialog-runtime-context-variables}

環境定義變數是您在節點中定義的變數。您可以為其指定預設值。其他節點、應用程式邏輯或使用者輸入可以隨後設定或變更環境定義變數的值。

環境定義變數值的條件設定方式是從對話節點條件中參照環境定義變數以判斷是否執行節點。您也可以從對話節點回應條件中參照環境定義變數，以根據外部服務或使用者所提供的值來顯示不同的回應。

進一步瞭解：

- [從應用程式傳遞環境定義](#dialog-runtime-context-from-app)
- [將環境定義從節點傳遞至節點](#dialog-runtime-context-node-to-node)
- [定義環境定義變數](#dialog-runtime-context-var-define)
- [一般環境定義變數作業](#dialog-runtime-context-common-tasks)
- [刪除環境定義變數](#dialog-runtime-context-delete)
- [更新環境定義變數](#dialog-runtime-context-update)
- [如何處理環境定義變數](#dialog-runtime-context-processing)
- [作業順序](#dialog-runtime-context-order-of-ops)
- [將環境定義變數新增至含空位的節點](#dialog-runtime-context-var-slots)

### 從應用程式傳遞環境定義
{: #dialog-runtime-context-from-app}

設定環境定義變數並將環境定義變數傳遞給對話，即可將資訊從應用程式傳遞給對話。

例如，您的應用程式可以設定 $time_of_day 環境定義變數，並將它傳遞給對話，而此對話可以使用該資訊來修改向使用者顯示的問候語。

![顯示 Welcome 節點，其使用回應條件來檢查從應用程式傳遞給對話的 $time_of_day 環境定義變數值。](images/set-context.png)

在此範例中，對話知道應用程式將變數設為下列其中一個值：*morning*、*afternoon* 或 *evening*。它可以檢查每一個值，並視呈現的值傳回適當的問候語。如果未傳遞變數，或變數的值不符合其中一個預期值，則會向使用者顯示更一般的問候語。

### 將環境定義從節點傳遞至節點
{: #dialog-runtime-context-node-to-node}

對話也可以新增環境定義變數以在不同的節點之間傳遞資訊，或是更新環境定義變數的值。對話詢問並取得使用者的相關資訊時，可以追蹤資訊，而且稍後可以在交談中進行參照。

例如，在某個節點中，您可能會詢問使用者的名稱，並在後面的節點中，依名稱來處理使用者。

![顯示簡介節點，其詢問使用者的名稱，並將名稱儲存為環境定義變數。下一個節點會使用 $username 環境定義變數，依名稱來參照使用者。](images/set-context-username.png)

在此範例中，如果使用者提供名稱，則會使用系統實體 @sys-person 從輸入中擷取使用者名稱。在 JSON 編輯器中，會定義 username 環境定義變數，並將其設為 @sys-person 值。在後續節點中，回應中會包含 $username 環境定義變數，以依名稱來處理使用者。

### 定義環境定義變數
{: #dialog-runtime-context-var-define}

若要定義環境定義變數，請在節點的編輯視圖中，將變數名稱新增至**變數**欄位，然後將其預設值新增至**值**欄位。

1.  按一下以開啟您要新增環境定義變數的對話節點。

1.  按一下與節點回應相關聯的**選項** ![進階回應](images/kabob.png) 圖示，然後按一下**開啟環境定義編輯器**。

      ![顯示如何存取與標準節點回應相關聯的 JSON 編輯器。](images/contextvar-json-response.png)

      如果節點的**多個回應**設定是**開啟**，則您必須先針對要與環境定義變數相關聯的回應，按一下**編輯回應** ![編輯回應](images/edit-slot.png) 圖示。

      ![顯示如何存取與已啟用多個條件式回應的標準節點相關聯的 JSON 編輯器。](images/contextvar-json-multi-response.png)

1.  將變數名稱/值配對對新增至**變數**和**值**欄位。

    - `name` 可以包含任何大寫和小寫英文字母、數值字元 (0-9) 及底線。

      您可以在名稱中包括其他字元，例如句點及連字號。不過，如果您這樣做，則之後每次參照變數時，都必須指定速記語法 `$(variable-name)`。如需詳細資料，請參閱[存取物件的表示式](/docs/services/assistant?topic=assistant-expression-language#expression-language-shorthand-context)。
      {:tip}

    - `value` 可以是任何支援的 JSON 類型，例如簡字組串變數、數字、JSON 陣列或 JSON 物件。

下表顯示一些範例，說明如何為不同類型的值定義名稱/值配對：

|變數          |值                                  | 值類型 |
|:---------------|-------------------------------|------------|
|dessert        | "cake"                        | 字串     |
|age            |18                 | 數字     |
|toppings_array | ["onions","olives"]            | JSON 陣列 |
|full_name     | {"first":"John","last":"Doe"} | JSON 物件 |

隨後，若要參照這些環境定義變數，請使用語法 `$name`，其中 *name* 是您所定義之環境定義變數的名稱。

例如，您可以指定下列表示式作為對話回應：

`The customer, $age-year-old <? $full_name.first ?>, wants a pizza with <? $toppings_array.join(' and ') ?>, and then $dessert.`

產生的輸出顯示如下：

`The customer, 18-year-old John, wants a pizza with onions and olives, and then cake.`

您也可以使用 JSON 編輯器來定義環境定義變數。如果您要新增複式表示式作為變數值，則最好是使用 JSON 編輯器。如需詳細資料，請參閱 [JSON 編輯器中的環境定義變數](#dialog-runtime-context-var-json)。

### 一般環境定義變數作業
{: #dialog-runtime-context-common-tasks}

若要儲存使用者所提供的整個字串作為輸入，請使用 `input.text`：

|變數          |值                                  |
|----------|------------------|
|repeat   | `<?input.text?>` |

例如，使用者輸入為 `I want to order a device.`。如果節點回應是 `You said: $repeat`，則回應會顯示為 `You said: I want to order a device.`。

若要將實體的值儲存在環境定義變數，請使用下列語法：

|變數          |值                                  |
|----------|------------------|
|place    | `@place`         |

例如，使用者輸入為 `I want to go to Paris.`。如果您的 `@place` 實體辨識 `Paris`，則助理會將 `Paris` 儲存在 `$place` 環境定義變數。

若要儲存從使用者輸入中擷取的字串值，您可以包括 SpEL 表示式，其使用 `extract` 方法將正規表示式套用至使用者輸入。下列表示式會擷取使用者輸入中的數字，並將它儲存至 `$number` 環境定義變數。

|變數          |值                                  |
|----------|-------------------------------------|
|數字   | `<?input.text.extract('[\d]+',0)?>` |

若要儲存型樣實體的值，請在實體名稱後面加上 .literal。使用此語法可確保使用者輸入中符合所指定型樣的確切文字段會儲存在變數。

|變數          |值                                  |
|----------|------------------------|
|email    | `<? @email.literal ?>` |

例如，使用者輸入為 `Contact me at joe@example.com.`。名為 `@email` 的實體辨識 `name@domain.com` 電子郵件格式。藉由配置環境定義變數來儲存 `@email.literal`，您可以指出要儲存輸入中符合型樣的那一部分。如果您省略值表示式中的 `.literal` 內容，則會傳回您指定給型樣的實體值名稱，而不是符合型樣的使用者輸入區段。

這些值範例中，有許多都是使用方法來擷取使用者輸入的不同部分。如需可供您使用之方法的相關資訊，請參閱[表示式語言方法](/docs/services/assistant?topic=assistant-dialog-methods)。

### 刪除環境定義變數
{: #dialog-runtime-context-delete}

若要刪除環境定義變數，請將變數設為空值。

|變數          |值                                  |
|------------|------------------|
| order_form |`null`         |

或者，您可以在應用程式邏輯中刪除環境定義變數。如需如何移除整個變數的相關資訊，請參閱[刪除格式為 JSON 的環境定義變數](#dialog-runtime-context-delete-json)。

### 更新環境定義變數值
{: #dialog-runtime-context-update}

若要更新環境定義變數的值，請使用與前一個環境定義變數相同的名稱來定義環境定義變數，但這次，請為它指定不同的值。

當多個節點設定相同環境定義變數的值時，環境定義變數的值會隨著使用者的交談過程而變更。在任何給定時間套用哪個值，取決於使用者在交談過程中所觸發的節點。在處理的最後一個節點中，為環境定義變數指定的值，會改寫先前已處理之節點針對該變數所設定的任何值。

如需在值為 JSON 物件或 JSON 陣列資料類型時，如何更新環境定義變數值的相關資訊，請參閱[更新格式為 JSON 的環境定義變數值](#dialog-runtime-context-update-json)。

### 如何處理環境定義變數
{: #dialog-runtime-context-processing}

您定義環境定義變數的位置至關重要。在助理處理您在其中定義環境定義變數的對話節點部分之前，不會建立環境定義變數，也不會設為您為其指定的值。在大部分情況下，您將環境定義變數定義為節點回應的一部分。當您這麼做時，在助理傳回節點回應時，會建立環境定義變數並提供指定的值。

對於具有條件式回應的節點，會在符合特定回應的條件並處理該回應時，建立及設定環境定義變數。例如，如果您定義條件式回應 #1 的環境定義變數，且助理僅處理條件式回應 #2，則不會建立並設定您針對條件式回應 #1 所定義的環境定義變數。

如需當使用者與含空位的節點互動時，您要助理建立並設定的環境定義變數要新增至何處的相關資訊，請參閱[將環境定義變數新增至含空位的節點](#dialog-runtime-context-var-slots)。

### 作業順序
{: #dialog-runtime-context-order-of-ops}

當您定義多個要一起處理的變數時，您定義變數的順序並不會決定助理評估這些變數的順序。助理會依隨機順序來評估這些變數。請不要在清單的第一個環境定義變數中設定值，並預期能夠在清單的第二個變數中使用該值，因為並不保證會先執行第一個環境定義變數，再執行第二個變數。例如，請不要使用兩個環境定義變數來實作邏輯，其用來檢查使用者輸入是否包含 `Yes` 這個字。

|變數          |值                                  |
|-----------------|------------------|
| user_input      | <? input.text ?> |
| contains_yes    | <? $user_input.contains('Yes') ?> |

請改用略為複雜的表示式，避免一定要先評估清單中第一個變數 (user_input) 的值，然後才能評估第二個變數 (contains_yes)。

|變數          |值                                  |
|---------------|------------------|
| contains_yes  | <? input.text.contains('Yes') ?> |

### 將環境定義變數新增至含空位的節點
{: #dialog-runtime-context-var-slots}

如需空位的相關資訊，請參閱[使用空位收集資訊](/docs/services/assistant?topic=assistant-dialog-slots)。

1.  在編輯視圖中開啟含空位的節點。

    - 若要新增在符合空位的回應條件之後再進行處理的環境定義變數，請執行下列步驟：

      1.  按一下**編輯空位** ![編輯回應](images/edit-slot.png) 圖示。
      1.  按一下**選項** ![進階回應](images/kabob.png) 圖示，然後選取**啟用條件式回應**。
      1.  按一下要用來建立與環境定義變數之關聯的回應旁的**編輯回應** ![編輯回應](images/edit-slot.png) 圖示。
      1.  按一下回應區段中的**選項** ![進階回應](images/kabob.png) 圖示，然後按一下**開啟環境定義編輯器**。
      1.  將變數名稱/值配對對新增至**變數**和**值**欄位。

      ![顯示如何存取與空位條件式回應相關聯的 JSON 編輯器。](images/contextvar-json-slot-multi-response.png)

    - 若要新增在符合空位條件之後再進行設定或更新的環境定義變數，請執行下列步驟：

      1.  按一下**編輯空位** ![編輯回應](images/edit-slot.png) 圖示。
      1.  從*配置空位* 視圖標頭的**選項** ![進階回應](images/kabob.png) 功能表中，按一下**開啟 JSON 編輯器**。
      1.  新增 JSON 格式的變數名稱/值配對。

          ```json
          {
            "time_of_day": "morning"
          }
          ```
          {: codeblock}

      目前沒有任何方法可以使用環境定義編輯器來定義在這階段的對話節點評估期間所設定的環境定義變數。您必須改用 JSON 編輯器。如需使用 JSON 編輯器的相關資訊，請參閱 [JSON 編輯器中的環境定義變數](#dialog-runtime-context-var-json)。
      {: note}

      ![顯示如何存取與空位條件相關聯的 JSON 編輯器。](images/contextvar-json-slot-condition.png)

## JSON 編輯器中的環境定義變數
{: #dialog-runtime-context-var-json}

您也可以在 JSON 編輯器中定義環境定義變數。如果您正在定義複式環境定義變數，而且希望能夠在新增或變更變數時看到完整的 SpEL 表示式，則可能會想要使用 JSON 編輯器。

名稱/值配對必須符合以下需求：

- `name` 可以包含任何大寫和小寫英文字母、數值字元 (0-9) 及底線。

  您可以在名稱中包括其他字元，例如句點及連字號。不過，如果您這樣做，則之後每次參照變數時，都必須指定速記語法 `$(variable-name)`。如需詳細資料，請參閱[存取物件的表示式](/docs/services/assistant?topic=assistant-expression-language#expression-lanaguage-shorthand-context)。
  {:tip}

- `value` 可以是任何支援的 JSON 類型，例如簡字組串變數、數字、JSON 陣列或 JSON 物件。

下列 JSON 範例定義 $dessert 字串、$toppings_array 陣列、$age 數字及 $full_name 物件環境定義變數的值：

```json
{
  "context": {
    "dessert": "cake",
    "toppings_array": [
      "onions",
      "olives"
    ],
    "age": 18,
    "full_name": {
      "first": "Jane",
      "last": "Doe"
    }
  },
         "output": {}
       }
       ```
{: codeblock}

若要定義 JSON 格式的環境定義變數，請完成下列步驟：

1.  按一下以開啟您要新增環境定義變數的對話節點。

    針對此節點所定義的全部現有環境定義變數值都會顯示在一組對應的**變數**及**值**欄位中。如果您不希望它們顯示在節點的編輯視圖中，則必須關閉環境定義編輯器。您可以從用來開啟 JSON 編輯器的相同功能表中關閉此編輯器；下列步驟說明如何存取功能表。
    {: note}

1.  按一下與回應相關聯的**選項** ![進階回應](images/kabob.png) 圖示，然後按一下**開啟 JSON 編輯器**。

    ![顯示如何存取與標準節點回應相關聯的 JSON 編輯器。](images/contextvar-json-response.png)

    如果節點的**多個回應**設定是**開啟**，則您必須先針對要與環境定義變數相關聯的回應，按一下**編輯回應** ![編輯回應](images/edit-slot.png) 圖示。

    ![顯示如何存取與已啟用多個條件式回應的標準節點相關聯的 JSON 編輯器。](images/contextvar-json-multi-response.png)

1.  新增 `"context":{}` 區塊（如果不存在的話）。

    ```json
    {
      "context":{},
      "output":{}
    }
    ```
    {: codeblock}

1.  在環境定義區塊中，針對您要定義的每個環境定義變數，新增 `"name"` 及 `"value"` 配對。

    ```json
    {
          "context":{
      "name": "value"
    },
         "output": {}
       }
       ```
    {: codeblock}

    在此範例中，會將名為 `new_variable` 的變數新增至已包含變數的環境定義區塊。

    ```json
    {
          "context":{
      "existing_variable": "value",
        "new_variable":"value"
      }
    }
    ```
    {: codeblock}

    隨後，若要參照環境定義變數，請使用語法 `$name`，其中 *name* 是已定義之環境定義變數的名稱。例如，`$new_variable`。

進一步瞭解：

- [刪除格式為 JSON 的環境定義變數](#dialog-runtime-context-delete-json)
- [更新格式為 JSON 的環境定義變數值](#dialog-runtime-context-update-json)
- [將某個環境定義變數設為等於另一個](#dialog-runtime-var-equals-var)

### 刪除格式為 JSON 的環境定義變數
{: #dialog-runtime-context-delete-json}

若要刪除環境定義變數，請將變數設為空值。

```json
{
  "context": {
    "order_form": null
  }
}
```
{: codeblock}

如果您要移除環境定義變數的全部追蹤，則可以使用 JSONObject.remove(string) 方法將它從環境定義物件中刪除。不過，您必須使用變數來執行移除。在訊息輸出中定義新的變數，因此，不會儲存在現行呼叫外部。

```json
{
  "output": {
    "text" : {},
    "deleted_variable" : "<? context.remove('order_form') ?>"
  }
}
```
{: codeblock}

或者，您可以在應用程式邏輯中刪除環境定義變數。

### 更新格式為 JSON 的環境定義變數值
{: #dialog-runtime-context-update-json}

一般而言，如果節點設定已設定的環境定義變數值，則新值會改寫先前的值。

#### 更新複雜 JSON 物件

會改寫所有 JSON 類型的先前值，但 JSON 物件除外。如果 context 變數是複雜類型（例如 JSON 物件），則會使用 JSON 合併程序來更新變數。合併作業會新增任何新定義的內容，並改寫物件的任何現有內容。

在此範例中，會將名稱環境定義變數定義為複雜物件。

```json
{
  "context": {
    "complex_object": {
      "user_firstname" : "Paul",
      "user_lastname" : "Pan",
      "has_card" : false
    }
  }
}
```
{: codeblock}

對話節點會將環境定義變數 JSON 物件更新為下列值：

```json
{
  "complex_object": {
    "user_firstname": "Peter",
    "has_card": true
  }
}
```
{: codeblock}

結果是下列環境定義：

```json
{
  "complex_object": {
    "user_firstname": "Peter",
    "user_lastname": "Pan",
    "has_card": true
  }
}
```
{: codeblock}

如需您可以對物件執行的方法的相關資訊，請參閱[表示式語言方法](/docs/services/assistant?topic=assistant-dialog-methods#dialog-methods-objects)。

#### 更新陣列

如果對話環境定義資料包含值的陣列，您可以藉由附加值、移除值或取代所有值來更新陣列。

選擇下列其中一個動作來更新陣列。在每一種情況下，我們都會看到在動作前面的陣列、動作，以及套用動作之後的陣列。

- **附加**：若要將值新增至陣列尾端，請使用 `append` 方法。

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

- **移除**：若要移除元素，請使用 `remove` 方法，並在陣列中指定其值或位置。

    - **依值移除**：會依元素值從陣列中移除元素。

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

    - **依位置移除**：會依元素的索引位置從陣列中移除元素。

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

- **改寫**：若要改寫陣列中的值，只需要將陣列設為新值：

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
            "toppings_array": ["ketchup", "tomatoes"]
          }
        }
        ```
        {: codeblock}

    結果：

        ```json
        {
          "context": {
            "toppings_array": ["ketchup", "tomatoes"]
          }
        }
        ```
        {: codeblock}

如需您可以對陣列執行的方法的相關資訊，請參閱[表示式語言方法](/docs/services/assistant?topic=assistant-dialog-methods#dialog-methods-arrays)。

### 將某個環境定義變數設為等於另一個
{: #dialog-runtime-var-equals-var}

當您將某個環境定義變數設為等於另一個環境定義變數時，要定義一個指標，從某個環境定義變數指向另一個。如果其中一個變數的值後來變更，則也會一併變更另一個變數的值。

例如，如果您如下所示指定環境定義變數，則當 `$var1` 或 `$var2` 的值後來變更時，另一個變數的值也會一併變更。

|變數          |值                                  |
|-----------|--------|
| var2      | var1   |

請不要將某個變數設為等於另一個變數，以用來擷取復原點值。例如，當處理陣列時，如果您要在對話的特定點中擷取儲存在環境定義變數中的陣列值，然後儲存以供稍後使用，則可以改為根據變數的現行值來建立新的變數。

例如，若要在對話流程的某個時間點建立陣列值的副本，請新增一個新陣列，並移入現有陣列的值。若要這樣做，您可以使用下列語法：

```json
{
"context": {
            "var2": "<? output.var2?:new JsonArray().append($var1) ?>"
 }
 }
 ```
{: codeblock}

## 離題
{: #dialog-runtime-digressions}

如果使用者目前正在進行設計用來處理某個目標的對話流程，而突然地切換主題來起始設計用來處理不同目標的對話流程，則發生離題。對話一律支援使用者變更主題。如果所處理對話分支中沒有任何節點符合使用者最新輸入的目標，則交談會回到樹狀結構，以檢查適當相符項的根節點條件。每個節點可用的離題設定甚至可讓您修改此行為。

使用離題設定，您可以容許交談回到離題發生時所岔斷的對話流程。例如，使用者可能正在訂購新電話，但切換主題詢問有關平板電腦的資訊。您的對話可以回答有關平板電腦的問題，然後將使用者帶回到離開訂購電話過程中的位置。容許發生並返回離題可讓使用者進一步控制執行時的交談流程。他們可以變更主題，並遵循無關主題的整個對話流程，然後回到先前的位置。結果是更接近地模擬人與人交談的對話流程。

![顯示提供晚餐預約詳細資料的人員詢問素食者選項，得到答案，然後返回提供預約詳細資料。](images/digression.gif){: gif}

動畫影像使用對話樹狀結構使用者介面的模型，來說明離題的概念。它會顯示使用者如何與對話節點互動，而對話節點配置成容許返回進行中對話流程的離題。使用者會開始提供預約晚餐所需的資訊。在於 #reservation 節點填入空位的過程中，使用者提出有關素食者菜單選項的問題。對話會回答使用者的新問題，方法是在根節點中尋找可解決它的節點（設定 #cuisine 目的之條件的節點）。然後，它會藉由顯示原始對話節點中下一個空白位置的提示，來返回進行中的交談。

請觀看此視訊以進一步瞭解。

<iframe class="embed-responsive-item" id="youtubeplayer" title="離題概觀" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/I3K7mQ46K3o?rel=0" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

- [開始之前](#dialog-runtime-digression-prereqs)
- [自訂離題](#dialog-runtime-enable-digressions)
- [離題用法提示](#dialog-runtime-digress-tips)
- [停用離題到根節點](#dialog-runtime-disable-digressions)
- [離題指導教學](#dialog-runtime-digression-tutorial)
- [設計考量](#dialog-runtime-digression-design-considerations)

### 開始之前
{: #dialog-runtime-digression-prereqs}

在您測試整體對話時，決定何時及何處可以容許離題並從離題返回。下列離題控制項會自動套用至節點。只有在您要變更此預設行為時，才會採取動作。

- 依預設，對話中的每個根節點都配置成容許發生離題。子節點不能是離題的目標。
- 含空位的節點被配置成防止脫離。所有其他節點都配置成容許脫離。不過，在下列情況下，交談不能脫離節點：

  - 如果現行節點的任何子節點包含 `anything_else` 或 `true` 條件

    這些條件的特殊在於一律評估為 true。因為它們的已知行為，通常會將其用於對話中，以強制母節點連續評估特定子節點。在此情況下，為了防止岔斷現有對話流程邏輯，不容許使用離題。您必須先將子節點的條件變更為其他條件，才能啟用脫離這類節點。

  - 如果節點配置成跳至另一個節點，或在處理之後跳過使用者輸入

    節點的最終步驟區段指定在處理節點之後應該發生什麼情況。對話配置成直接跳至另一個節點時，通常確定會遵循特定順序。而且，節點配置成跳過使用者輸入時，相當於強制對話在現行節點之後連續處理第一個子節點。在上述任一情況下，為了防止岔斷現有對話流程邏輯，不容許使用離題。您必須先變更最終步驟區段中所指定的值，才能啟用脫離此節點。

### 自訂離題
{: #dialog-runtime-enable-digressions}

您未定義離題的開始及結束。使用者可以在執行時完整控制離題流程。您只需要指定每一個節點應該或不應該參與使用者造成的離題。針對每一個節點，您會配置：

- 離題是否可以從節點開始，並離開節點
- 於其他位置開始的離題是否可以設為目標，並進入節點
- 在現行對話流程完成之後，於其他位置開始並進入節點的離題是否必須返回岔斷的對話流程

若要變更個別節點的離題運作，請完成下列步驟：

1.  按一下節點以開啟其編輯視圖。

1.  按一下**自訂**，然後按一下**離題**標籤。

    根據所編輯的節點是根節點、子節點、含子項的節點還是含空位的節點，配置選項會不相同。

    **脫離此節點**

    如果先前所列的情況不適用，則可以進行下列選擇：

    - **所有節點類型**：選擇是否容許使用者在到達現行對話分支尾端之前先脫離現行節點。

    - **所有含子項的節點**：選擇如果已顯示現行節點的回應，而且其子節點伴隨著節點目標，是否要讓交談在離題之後回到現行節點。將*容許在此節點回應之後所觸發的離題返回* 切換開關設為**否**，以避免對話返回現行節點並繼續處理其分支。

      例如，如果使用者詢問 `Do you sell cupcakes?`，並在使用者變更主題之前顯示回應 `We offer cupcakes in a variety of flavors and sizes`，則您可能不會想要對話回到其離開的位置。特別是，如果子節點只處理使用者的可能後續問題，而且可以放心地予以忽略。

      不過，如果節點依賴其子節點來處理問題，則您可能要強制交談返回並繼續處理現行分支中的節點。例如，起始回應可能是 `We offer cupcakes in all shapes and sizes. Which menu do you want to see: gluten-free, dairy-free, or regular?`。如果使用者在此時變更主題，您可能會想要對話返回，讓使用者可以挑選菜單類型，並取得他們想要的資訊。

    - **含空位的節點**：選擇是否要容許使用者在填入所有空位之前先脫離節點。將*容許在填入空位時脫離* 切換開關設為**是**，以啟用脫離。

      如果已啟用，則交談從離題返回時，會顯示下一個未填入空位的提示，鼓勵使用者繼續提供資訊。如果已停用，則會忽略使用者所提交且未包含可填入空位之值的全部輸入。不過，您可以定義空位處理程式，來處理預期使用者在與節點互動時可能會詢問的自發問題。如需相關資訊，請參閱[新增空位](/docs/services/assistant?topic=assistant-dialog-slots#dialog-slots-add)。

      下圖顯示如何配置脫離含空位的 #reservation 節點（如先前的圖解所示）。

      ![顯示與含空位的節點的脫離設定。](images/digress-away-slots-full.png)

    - **含空位的節點**：如果使用者選取**僅從容許返回的節點的空位脫離**勾選框來返回現行節點，請選擇是否只容許使用者脫離。

      若已選取，則對話尋找節點以回答使用者的無關問題時，會忽略未配置成在離題之後返回的全部根節點。如果您要避免使用者在完成填入必要空位之前永久地離開節點，請選取此勾選框。

    **離題到此節點**

    您可以針對離題到節點的運作方式進行下列選擇：

    - 避免使用者離題到節點。如需詳細資料，請參閱[停用離題到根節點](#dialog-runtime-disable-digressions)。

    - 若已啟用離題到節點，請選擇對話是否必須回到它所脫離的對話流程。若已選取，在處理現行節點的分支之後，對話流程會回到岔斷節點。若之後要讓對話返回，請選取**在離題之後返回**。

    下圖顯示如何配置離題到 #cuisine 節點（如先前的圖解所示）。

    ![顯示與含空位的節點的脫離設定。](images/digress-into-cuisine-full.png)

1.  按一下**套用**。

1.  使用「試用」窗格，以測試離題運作方式。

    同樣地，您無法定義離題的開始及結束。使用者可控制在何處及何時發生離題。您只能套用設定，用於判定單一節點如何參與離題。因為離題如此的無法預期，所以很難知道您的配置決策對整體交談的影響。若要真正得知所進行選擇的影響，您必須測試對話。

節點 #reservation 及 #cuisine 代表兩個對話分支，可參與單一使用者指示的離題。針對每一個個別節點所配置的離題設定，即在執行時可能實現這類型離題的原因。

![顯示兩個對話，一個設定脫離 reservation 空位節點，另一個則設定離題到 cuisine 節點。](images/digression-settings.png)

### 離題用法提示
{: #dialog-runtime-digress-tips}

本節說明您在使用離題時可能遇到之狀況的解決方案。

- **自訂返回訊息**：對於您啟用從離題處返回的任何節點，請考慮新增文字，讓使用者知道他們正在返回到在先前對話流程中離開的位置。在文字回應中，使用可讓您新增兩個回應版本的特殊語法。

  如果您未採取動作，則相同的文字回應會顯示第二次，讓使用者知道他們已回到所離開的節點。指定當使用者返回時所要顯示的唯一訊息，即可讓使用者清楚他們已回到原本的交談執行緒。

  例如，如果節點的原始文字回應為 `What's the order number?`，則當使用者返回節點時，您可能想要顯示如下的訊息：`Now let's get back to where we left off. What is the order number?`。

  若要這樣做，請使用下列語法來指定節點文字回應：

  `<? (returning_from_digression)? "post-digression message" : "first-time message" ?>`

  例如：

  ```bash
  <? (returning_from_digression)? "Now, let's get back to where we left off.
  What is the order number?" : "What's the order number?" ?>
  ```
  {: codeblock}

  在新增的文字回應中，不能包括 SpEL 表示式或速記語法。事實上，您完全不能使用速記語法。而是必須藉由將字串和完整 SpEL 表示式語法連結起來以形成完整回應的方法來建置訊息。
  {: note}
  
  例如，使用下列語法將環境定義變數包含在文字回應中，您通常會將它指定為 `What can I do for you, $username?`：

  ```bash
  <? (returning_from_digression)? "Where were we, " +
  context["username"] + "? Oh right, I was asking what can I do
  for you today." : "What can I do for you today, " +
  context["username"] + "?" ?>
  ```

  如需完整 SpEL 表示式語法的詳細資料，請參閱[存取物件的表示式](/docs/services/assistant?topic=assistant-expression-language#expression-language-shorthand-syntax)。

- **防止返回**：在某些情況下，根據使用者在現行對話流程中所做的選擇，您可能想要防止回到已岔斷的交談流程。您可以使用特殊語法來防止從特定節點返回。

  例如，您可能有一個節點，條件為 `#General_Connect_To_Agent` 或類似目的。觸發時，如果您想要先取得使用者的確認，再將使用者移轉至外部服務，則可以新增回應，例如，`Do you want me to transfer you to an agent now?`。然後，您可以新增兩個子節點，條件分別為 `#yes` 及 `#no`。
  
  管理這種分支類型離題的最佳方式，是將根節點設為容許離題返回。不過，在 `#yes` 節點上，請在回應中包含 SpEL 表示式 `<? clearDialogStack() ?>`。例如：
  
  ```bash
  OK. I will transfer you now. <? clearDialogStack() ?>
  ```
  {: codeblock}

  此 SpEL 表示式會防止這個節點發生離題返回。要求確認時，如果使用者說「是」，則會顯示適當的回應，且不會回復岔斷的對話流程。如果使用者說「否」，則使用者會回到已岔斷的流程。

### 停用離題到根節點
{: #dialog-runtime-disable-digressions}

流程離題到根節點時，會遵循針對該節點所配置的對話過程。因此，它可能會在到達節點分支尾端之前處理一系列的子節點，而且，如果配置為這樣做，則請回到已岔斷的對話流程。透過對話測試，您可能會發現根節點的觸發太過頻繁、或在未預期的時間觸發，或是其對話太過複雜，而導致使用者離題太遠無法成為良好的暫時離題候選。如果您決定不容許使用者離題到根節點，則可以將根節點配置成不容許離題進入。

若要完全停用離題到根節點，請完成下列步驟：

1.  按一下以開啟您要編輯的根節點。
1.  按一下**自訂**，然後按一下**離題**標籤。
1.  將*容許離題到此節點* 切換開關設為**關閉**。
1.  按一下**套用**。

如果您決定要避免離題到數個根節點，但不要個別編輯每一個根節點，則可以將數個節點新增至一個資料夾。從資料夾的*自訂* 頁面中，您可以將*容許離題到此節點* 切換開關設為**關閉**，一次將配置套用至所有節點。如需相關資訊，請參閱[使用資料夾組織對話](/docs/services/assistant?topic=assistant-dialog-build#dialog-build-folders)。

### 離題指導教學
{: #dialog-runtime-digression-tutorial}

請遵循[指導教學](/docs/services/assistant?topic=assistant-tutorial-digressions)，以匯入已定義一組節點的工作區。您可以逐步演練，這些練習說明離題如何運作。

### 設計考量
{: #dialog-runtime-digression-design-considerations}

- **避免備用節點大量生產**：許多對話設計程式都會在每個對話分支的尾端包含具有 `true` 或 `anything_else` 條件的節點，以避免使用者停滯在分支中。如果使用者輸入不符合您的預期且包含要處理的特定對話節點，則此設計會傳回一般訊息。不過，使用者無法脫離使用此方式的對話流程。

  評估使用此方式的全部分支，以判定是否最好容許脫離分支。如果使用者輸入不符合您的預期，您可能會在樹狀結構中找到一個完全不同的對話流程。您可以有效地讓對話的其餘部分運作以嘗試處理使用者的輸入，而非回應一般訊息。而根層次 `Anything else` 節點一律可以回應其他根節點無法處理的輸入。

- **重新考慮跳至封閉式節點**：許多對話都設計成詢問標準封閉式問句，例如 `Did I answer your question today?`。使用者無法脫離配置成跳至另一個節點的節點。因此，如果您配置所有最終分支節點以跳至一般封閉式節點，離題並不會發生。請考慮透過度量值或一些其他方法來追蹤使用者滿意度。

- **測試可能的離題鏈**：如果使用者脫離現行節點到容許脫離的另一個節點，則使用者可能可以脫離這個其他節點，並重複此型樣一次以上。如果離題鏈中的開始節點配置成在離題之後返回，則最後會將使用者帶回現行對話節點。事實上，離題鏈中已配置成不返回的所有後續節點，都不考慮作為離題目標。請測試多次離題的情境，以判斷個別節點是否如預期運作。


- **記住現行節點的優先順序較高**：請記住，如果現行流程無法處理使用者輸入，則只會將現行流程外部的節點視為離題目標。更重要的是，含空位的節點容許脫離，具體而言是可讓使用者清楚瞭解他們需要提供的資訊，以及新增在使用者提供值之後顯示的確認陳述。

  在空位填入處理程序期間，可以填入任何空位。因此，空位可能會非預期地擷取使用者輸入。例如，含空位的節點可收集預約晚餐所需的資訊。其中一個空位收集日期資訊。提供預約詳細資料時，使用者可能會詢問 `What's the weather meant to be tomorrow?`。您的根節點可能會設定可回答使用者之 #forecast 的條件。不過，因為使用者輸入包含 `tomorrow` 這個字，並且正在處理含空位的預約節點，所以助理會假設使用者正在提供或更新預約日期。*現行節點的優先順序一律較高。* 如果您定義清楚的確認陳述（例如，`Ok, setting the reservation date to tomorrow`)，則使用者更有可能意識到發生誤解並會予以更正。

  反之，填入空位時，如果使用者提供的值不是任何空位預期的，則可能符合使用者永遠不會離題到其中的完全無關的根節點。

  配置離題運作時，請務必執行多次測試。

- **何時使用離題，而非空位處理程式**：針對使用者可能在任何時間詢問的一般問題，請使用容許離題到其中的根節點，處理輸入，然後回到進行中的流程。針對含空位的節點，請嘗試預期使用者可能會在填入空位時詢問的相關問題類型，並將處理程式新增至節點來進行處理。

  例如，如果含空位的節點收集填寫保險理賠所需的資訊，則您可能要新增可處理一般保險問題的處理程式。不過，針對如何取得協助的問題、儲存位置或公司歷程，請使用根層次節點。

## 更正使用者輸入
{: #dialog-runtime-spell-check}

啟用*自動更正* 特性，以修正使用者在其提交作為使用者輸入之話語中所造成的拼字錯誤。啟用自動更正時，會自動更正拼錯的字組。它是用來評估輸入的已更正字組。提供更精確的輸入時，助理會更常辨識實體提及項目，並瞭解使用者的目的。

您只能對英文語言對話技能啟用此設定。它會針對新的英文對話技能自動啟用。
{: note}

啟用自動更正之後，就會使用下列方式來更正使用者輸入：

- 原始輸入：`letme applt for a memberdhip`
- 已更正的輸入：`let me apply for a membership`

當助理評估是否要更正字組的拼寫時，它不是根據簡單的字典查閱處理程序。相反地，它會使用「自然語言處理程序」和機率模型的組合，來評量詞彙實際上是否拼錯，應該予以更正。

### 啟用自動更正
{: #dialog-runtime-spell-check-enable}

若要啟用自動更正特性，請完成下列步驟：

1.  從「技能」頁面中，開啟您的技能。
1.  按一下**選項**標籤。
1.  開啟**自動更正**。

### 測試自動更正
{: #dialog-runtime-spell-check-test}

1.  從「試用」窗格中，提交包含一些拼字錯誤的話語。

    如果輸入中的字組拼錯，則會自動予以更正，並顯示 ![自動更正](images/auto-correct.png) 圖示。已更正的話語會畫上底線。
1.  將游標移至畫底線的話語上方來查看原始用字。

如果您預期有需要助理更正的拼字錯誤詞彙，但沒有，請檢閱助理用來決定是否要更正字組的規則，以查看該字組是否屬於服務故意不變更的字組種類中。

為了避免過度更正，助理不會更正下列輸入類型的拼字：

- 大寫字組
- 表情符號
- 位置實體，例如州/省及地址
- 度量或時間的數字及單位
- 適當的名詞，例如常用的名字或公司名稱
- 引號內的文字
- 包含特殊字元的字組，例如連字號 (-)、星號 (*) 和 & 符號，或 at 符號 (@)，包括電子郵件位址或 URL 中使用的字元。
- *屬於* 此技能的字組，表示這些字組具有隱含的重要性，因為它們出現在實體值、實體同義字或目的使用者範例中。

  環境定義實體的提及項目可以會在無意中被更正。原因是作為環境定義實體提及項目的詞彙並不是固定不變；這些詞彙無法像基於字典的詞彙清單一樣，透過拼字檢查程式功能預先確定並避免。如果在測試後發現過度更正某個環境定義實體的提及項目，請考慮將該實體替換為基於字典的實體。
  {: note}

如果未更正的字組未明顯地屬於其中一種輸入類型，則可能值得檢查實體是否已對它啟用模糊比對。

#### 拼字自動更正與模糊比對有何相關？
{: #dialog-runtime-spell-check-vs-fuzzy-matching}

模糊比對可協助助理在使用者輸入中辨識以字典為基礎的實體提及項目。它會使用字典查閱方法，將使用者輸入中的字組與技能訓練資料中的現有實體值或同義字進行比對。例如，如果使用者輸入 `boook`，而且您的訓練資料包含 `@reading_material` 實體及 `book` 值，則模糊比對會辨識出來兩個詞彙（`boook` 和 `book`）表示相同的事物。

同時啟用自動更正和模糊比對時，模糊比對功能會在觸發自動更正之前執行。如果它找到一個詞彙，它符合現有字典實體值或同義字，則會將這個詞彙新增至*屬於* 技能的字組清單中，因此不會予以更正。

同樣地，如果使用者輸入類似 `I wnt to buy a boook` 的句子，模糊比對會辨識出來詞彙 `boook` 表示與實體同義字 `book` 相同的事物，並將它新增至受保護的字組清單。助理會將輸入更正為 `I want to buy a boook`。請注意，它會更正 `wnt`，但*不會* 更正 `boook` 的拼字。如果在測試對話時看到此類型的結果，您可能會認為助理的行為有誤。但實際上助理並未做錯。由於模糊比對，它正確地將 `boook` 識別為 `@reading_material` 實體提及項目。且由於自動更正將詞彙修訂為 `want`，您的助理能夠將輸入對應到您的 `#buy_something` 目的。每個特性都會發揮自己的作用，來協助助理瞭解使用者輸入的意義。



#### 自動更正的運作方式
{: #dialog-runtime-spell-check-how-it-works}

一般來說，使用者輸入會依現狀儲存在訊息 `input` 物件的 `text` 欄位中。若且只有在以某種方式更正使用者輸入時，才會在 `input` 物件中建立新欄位，稱為 `original_text`。此欄位會儲存使用者的原始輸入，其中包含任何拼錯的字組。已更正的文字則會新增至 `input.text` 欄位。

## 澄清 ![僅限「加值」或「超值」方案](images/plus.png)
{: #dialog-runtime-disambiguation}

此特性僅適用於「加值」或「超值」使用者。
{: note}

當您啟用澄清時，指示助理在發現多個對話節點可以回應其輸入時，要求使用者提供協助。您的助理會與使用者分享一份頂端節點選項清單，並要求使用者挑選正確的節點選項，而不是猜測要處理哪個節點。

![顯示使用者與助理之間的交談範例，其中助理要求使用者進行釐清。](images/disambig-demo.png)

如果已啟用，除非符合下列條件，否則不會觸發澄清：

- 在使用者輸入中偵測到的一個以上第二名目的的信賴分數，大於最高目的信賴分數的 55%。
- 最高目的的信賴分數高於 0.2。

即使符合這些條件，除非對話中有兩個以上的獨立節點符合下列準則，否則不會發生澄清：

- 節點條件包括其中一個已觸發澄清的目的。否則，節點條件會評估為 true。例如，如果節點檢查實體類型，且在使用者輸入中提及實體，則它是適用的。
- 節點的*外部節點名稱* 欄位中有文字。

進一步瞭解

- [澄清範例](#dialog-runtime-disambig-example)
- [啟用澄清](#dialog-runtime-disambig-enable)
- [選擇節點](#dialog-runtime-choose-nodes)
- [處理以上皆非](#dialog-runtime-handle-none)
- [測試澄清](#dialog-runtime-disambig-test)

### 澄清範例
{: #dialog-runtime-disambig-example}

例如，您的對話有兩個節點，內含可解決取消要求的目的條件。條件為：

- eCommerce_Cancel_Product_Order
- Customer_Care_Cancel_Account

如果使用者輸入為 `i must cancel it today`，則可能會在輸入內偵測到下列目的：

`[`
`{"intent":"Customer_Care_Cancel_Account","confidence":0.6618281841278076},`
`{"intent":"eCommerce_Cancel_Product_Order","confidence":0.4330700159072876},`
`{"intent":"Customer_Care_Appointments","confidence":0.2902342438697815},`
`{"intent":"Customer_Care_Store_Hours","confidence":0.2550420880317688},`
`...]`

助理有 `0.6618281841278076` (66%) 的程度確信使用者目標符合 `#Customer_Care_Cancel_Account` 目的。如果任何其他目的的信賴分數大於 66% 的 55%，則符合成為澄清候選項的準則。

`0.66 x 0.55 = 0.36`

評分大於 0.36 的目的是適用的。

在我們的範例中，`#eCommerce_Cancel_Product_Order` 目的超過臨界值，信賴分數為 `0.4330700159072876`。

當使用者輸入為 `i must cancel it today` 時，會將這兩個對話節點視為可行的回應候選項。為了判斷要處理哪個對話節點，助理會要求使用者挑選一個。為了協助使用者在節點之間選擇，助理會提供每個節點執行作業的簡短摘要。顯示的摘要文字直接擷取自針對每個節點所指定的*外部節點名稱* 資訊。

![服務會提示使用者從對話選項清單中選擇，包括取消帳戶、取消產品訂單，以及以上皆非。](images/disambig-tryitout.png)

請注意，助理會將使用者輸入中的 `today` 一詞辨識為日期，亦即，`@sys-date` 實體的提及項目。如果您的對話樹狀結構包含一個節點，其條件為 `@sys-date` 實體，則它也會包含在澄清選項的清單中。這個影像顯示其包含在清單中，作為*擷取日期資訊* 選項。

![服務會提示使用者從對話選項清單中選擇，包括擷取日期資訊。](images/disambig-tryitout-date.png)

下列視訊提供澄清的概觀。

<iframe class="embed-responsive-item" id="youtubeplayer0" title="澄清概觀" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/VVyklAXlmbA?rel=0" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

### 啟用澄清
{: #dialog-runtime-disambig-enable}

若要啟用澄清，請完成下列步驟：

1.  針對您要啟用澄清的對話技能，開啟**選項**標籤。

    如果應用程式在達拉斯管理，若要啟用澄清，請按一下**對話**頁面的**設定**。
    {: note}

1.  在*澄清* 區段中，將切換開關切換為**開啟**。
1.  在**澄清訊息**欄位中，新增要顯示在對話節點選項清單前面的文字。例如，*您要做什麼？*
1.  在**其他任何內容**欄位中，新增要顯示為其他選項的文字，如果其他對話節點選項都無法反映出使用者想執行的作業，使用者即可挑選該文字。例如，*以上皆非*。

    請保持訊息簡短，以便與其他選項一同顯示在行內。訊息必須少於 512 個字元。如需當使用者選擇此選項時，助理會執行什麼作業的相關資訊，請參閱[處理以上皆非](#dialog-runtime-handle-none)。

1.  如果您要限制可以顯示給使用者的澄清選項數目，則在**建議數上限**欄位中，指定介於 2 到 5 之間的數字。 

    您的變更會自動儲存。

1.  現在，按一下**對話**標籤。檢閱對話，以決定您要助理要求協助的對話節點。

    - 您可以挑選位於樹狀結構階層之任何層次的節點。
    - 您可以挑選節點，其條件為目的、實體、特殊條件、環境定義變數或這些值的任何組合。

    如需提示，請參閱[選擇節點](#dialog-runtime-choose-nodes)。

    對於您要從澄清選項清單設為可用的每個節點，請完成下列步驟：

    

    1.  按一下以在編輯視圖中開啟節點。
    1.  在*外部節點名稱* 欄位中，說明此對話節點設計用來處理的使用者作業。例如，*取消帳戶*。

        ![顯示節點編輯視圖中要新增外部節點名稱資訊的位置。](images/disambig-node-purpose.png)

### 選擇節點
{: #dialog-runtime-choose-nodes}

選擇作為對話相異分支根節點的節點，以作為澄清選項。這些可包括作為其他節點子項的節點。重點是節點的條件設為某些相異值或用於識別該節點與其他節點的值。

{{site.data.keyword.conversationshort}} 工具可以辨識目的衝突，當兩個以上目的具有重疊的使用者範例時，即會發生衝突。先[解決任何這類衝突](/docs/services/assistant?topic=assistant-intents#intents-resolve-conflicts)確保目的本身盡可能是唯一的，以協助助理取得較高的目的信賴分數。
{: note}

請牢記：

- 對於條件設為目的的節點，如果助理確信節點的目的條件符合使用者目的，則會包含節點作為澄清選項。
- 對於具有布林條件（評估為 true 或 false 的條件）的節點，如果條件評估為 true，則會包含節點作為澄清選項。例如，當節點的條件設為實體類型時，如果在輸入中提及會觸發澄清的實體，則會包含該節點。
- 節點在樹狀結構階層中的順序會影響澄清。

  - 它會影響是否觸發澄清
  
    查看先前用來介紹澄清的[情境](#dialog-runtime-disambig-example)。如果條件為 `@sys-date` 的節點在對話樹狀結構中的位置，高於條件為 `#Customer_Care_Cancel_Account` 及 `#eCommerce_Cancel_Product_Order` 目的的節點，則當使用者輸入 `i must cancel it today` 時，永遠不會觸發澄清。那是因為對應節點在樹狀結構中的位置，導致助理認為日期提及項目 (`today`) 比目的參照還要重要。

  - 它會影響要包含在澄清選項清單中的節點
  
    有時，節點不會如預期列出為澄清選項。如果因故不符資格無法包含在澄清清單的節點也參照條件值，即會發生此狀況。例如，實體提及項目可能觸發位於樹狀結構中較高位置但未啟用澄清的節點。對於*已啟用* 澄清但位於樹狀結構中較低位置的節點，如果相同的實體是該節點的唯一條件，則因為助理永遠不會呼叫該節點，所以該節點不會新增為澄清選項。它符合先前節點，且已省略，因此助理不會處理較晚的節點。

針對您接受澄清的每個節點，測試您預期在其中將節點包含在澄清選項清單的情境。測試讓您有機會調整節點順序或其他可能會影響澄清在執行時期之運作效能的因素。


### 處理以上皆非
{: #dialog-runtime-handle-none}

當使用者按一下*以上皆非* 選項時，助理會從訊息中刪除使用者輸入中所辨識的目的，然後重新提交。此動作通常會觸發對話樹狀結構中的任例其他節點。


若要自訂此狀況中傳回的回應，您可以新增一個根節點，內含的條件會檢查不含已辨識目的（記住，該目的已被刪除）的使用者輸入，並包含 `suggestion_id` 內容。觸發澄清時，助理會新增 `suggestion_id` 內容。
{: tip}

新增具有下列條件的根節點：

```json
intents.size()==0 && input.suggestion_id
```
{: codeblock}

只有已觸發一組澄清選項（且使用者已指出其中沒有任何澄清選項符合其目標）的輸入，才符合此條件。

新增回應，讓使用者知道您瞭解沒有任何建議選項符合他們的需求，並採取適當的動作。


重申，樹狀結構中的節點位置至關重要。如果節點（條件設為使用者輸入中所提及的實體類型）在樹狀結構中的位置高於此節點，則會改為顯示其回應。


### 測試澄清
{: #dialog-runtime-disambig-test}

若要測試澄清，請完成下列步驟：

1.  從「試用」窗格中輸入一句您認為是良好澄清候選項的測試話語，這表示會配置兩個以上的對話節點來處理類似的話語。


1.  如果回應所包含的對話節點選項清單，無法讓您如預期選擇，請先確認您已在每個節點的外部節點名稱欄位中新增了摘要資訊。


1.  如果仍然未觸發澄清，可能是節點的信賴分數值不像您認為的那麼接近。

    - 若要取得符合給定使用者輸入之澄清臨界值的目的清單，您可以在節點的文字回應中使用下列 SpEL 表示式。

      ```json
      The following intents meet the disambiguation threshold: <? intents.filter("x", "x.confidence > intents[0].confidence * 0.55") ?>
      ```
      {: codeblock}

      此表示式取得在使用者輸入中辨識到的目的陣列中第一個目的的信賴分數。它接著會將最熱門目的的信賴分數乘上 0.55 以取得信賴臨界值，陣列中的其他目的必須符合此臨界值才會執行澄清。最後，它會過濾目的陣列，僅包括信賴分數高於臨界值的目的。

      藉由此表示式傳回的陣列，您可以瞭解哪些目的包括為澄清選項以及這些目的的數量（如果已啟用澄清的節點的對話節點條件中使用每個目的）。將表示式新增至您確信將測試輸入所觸發的節點。

      如需表示式中所使用 `JSONArray.filter` 方法的詳細資料，請參閱[表示式語言方法](/docs/services/assistant?topic=assistant-dialog-methods#dialog-methods-array-filter)。

    - 若要查看使用者輸入中偵測到的所有目的的信賴分數，請將 `<? intents ?>` 暫時新增至您已知將觸發之節點的節點回應結尾。

      此 SpEL 表示式顯示在使用者輸入中偵測到作為陣列的目的。陣列包括目的名稱以及助理對於目的反映使用者預期目標這點的信賴水準。

    - 若要查看在使用者輸入中偵測到哪些實體（如果有的話），您可以暫時將現行回應取代為單一文字回應，其中包含 SpEL 表示式 `<? entities ?>`。

      此 SpEL 表示式顯示在使用者輸入中偵測到作為陣列的實體。陣列包括實體名稱、實體提及項目在使用者輸入字串內的位置、實體提及項目字串，以及助理對於此術語是所指定實體類型之提及項目這點的信賴水準。

    - 若要一次查看所有構件的詳細資料，包括其他內容，例如，呼叫時給定環境定義變數的值，您可以檢查整個 API 回應。請參閱[檢視 API 呼叫詳細資料](/docs/services/assistant?topic=assistant-dialog-tips#dialog-tips-inspect-api)。

1.  對於您預期將列出為澄清選項的至少一個節點，暫時移除您新增至*外部節點名稱* 欄位中的說明。

1.  再次將測試話語輸入至「試用」窗格中。

    如果您已新增 `<? intents ?>` 表示式至回應中，則傳回的文字會包括助理在測試話語中辨識出的目的清單，並包括每一個目的的信賴分數。

    ![服務傳回目的陣列，包括 Customer_Care_Cancel_Account 及 eCommerce_Cancel_Product_Order。](images/disambig-show-intents.png)

完成測試之後，請移除附加到節點回應的所有 SpEL 表示式，或加回以表示式取代的所有原始回應，然後重新移入您已移除其中文字的所有*外部節點名稱* 欄位。
