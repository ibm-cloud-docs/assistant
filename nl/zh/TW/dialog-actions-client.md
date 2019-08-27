---

copyright:
  years: 2015, 2019
lastupdated: "2019-08-01"

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

# 呼叫用戶端應用程式 ![測試版](images/beta.png)
{: #dialog-actions-client}

將用戶端動作新增至用於暫停對話的對話節點，讓用戶端處理程序可以執行，並在一輪對話內進行處理的過程中傳回結果。
{: shortdesc}

用戶端動作以外部用戶端應用程式可以瞭解的標準化格式定義程式化呼叫。您的外部用戶端應用程式必須使用所提供的資訊來執行程式化呼叫或功能，然後將結果傳回至對話。

此動作不會直接呼叫自身。它基本上會告知對話在這裡暫停，並等待外部用戶端應用程式執行某些動作。用戶端應用程式執行的程式可以是您選擇的任何程式。請務必根據本主題稍後概述的 JSON 格式化規則，來指定呼叫名稱和參數詳細資料，以及錯誤訊息變數名稱。

您可以呼叫用戶端應用程式來執行下列類型的作業：

- 驗證您從使用者收集的資訊。
- 對使用者輸入執行運算或字串操作，因為這些使用者輸入太過複雜，以致支援的 SpEL 表示式方法無法處理它們。
- 從其他應用程式或服務取得資料。

如需如何呼叫外部服務（例如 {{site.data.keyword.openwhisk_short}} Web 動作）的相關資訊，請參閱[從對話節點進行程式化呼叫](/docs/services/assistant?topic=assistant-dialog-webhooks)。

下圖說明如何使用 client 呼叫來取得天氣預報資訊，並將它傳回給使用者。

![顯示某人要求天氣預報，對話將要求傳送至用戶端應用程式，這會將它傳送至外部服務](images/forecast.png)

請注意，用戶端動作呼叫的是 `MyWeatherFunction` 程式，這是用戶端應用程式執行的程式。用戶端應用程式將呼叫外部 Web 服務 (`/weather`) 來取得實際的預報資訊。然後，用戶端應用程式會將回應傳回給對話。將用戶端動作新增到對話時，用戶端應用程式必須存在，而應用程式自行執行處理，或是在對話和要使用的任何外部後端服務之間傳遞資訊。

## 程序
{: #dialog-actions-client-call}

若要從對話節點對用戶端應用程式進行程式化呼叫，請完成下列步驟：

1.  在您要從中進行程式化呼叫的對話節點中，開啟 JSON 編輯器。

    - 若要進行在評估節點回應之後執行的程式化呼叫，請針對節點回應開啟 JSON 編輯器。

      ![顯示如何存取與標準節點回應相關聯的 JSON 編輯器。](images/contextvar-json-response.png)

      如果節點的**多個回應**設定是**開啟**，則您必須按一下**編輯回應** ![編輯回應](images/edit-slot.png) 圖示，才會顯示**選項** ![進階回應](images/kabob.png) 功能表。

      ![顯示如何存取與標準節點相關聯的 JSON 編輯器，此標準節點具有多個針對它而啟用的條件式回應。](images/contextvar-json-multi-response.png)

    如果您要在同一次對話內顯示或進一步處理回應，則必須新增執行這項作業的第二個節點，並從這個節點跳過去。
    {: tip}

    - 若要進行個別空位可使用的呼叫，請按一下空位的**編輯空位** 圖示 ![「編輯空位」圖示](images/edit-slot.png)，然後執行下列其中一個事項：

      - 若要進行在將空位條件評估為 true 之後執行的程式化呼叫，請開啟與空位條件相關聯的 JSON 編輯器。

        ![顯示如何存取與空位條件相關聯的 JSON 編輯器。](images/contextvar-json-slot-condition.png)

      - 若要進行在順利填入空位之後執行的程式化呼叫，請開啟與「找到」回應相關聯的 JSON 編輯器。若要這樣做，請從空位的**選項** ![「選項」圖示](images/kabob.png) 功能表中按一下**啟用條件式回應**。針對「找到」回應，按一下**編輯回應** ![編輯回應](images/edit-slot.png) 圖示。從「找到」回應的**選項** ![「選項」圖示](images/kabob.png) 功能表中，按一下**開啟 JSON 編輯器**。

        ![顯示如何存取與空位條件式回應相關聯的 JSON 編輯器。](images/contextvar-json-slot-multi-response.png)

1.  使用下列語法來定義程式化呼叫。

    ```json
    {
      "context": {
      "variable_name" : "variable_value"
      },
      "actions": [
        {
          "name":"<actionName>",
          "type":"client",
          "parameters": {
            "<parameter_name>":"<parameter_value>",
            "<parameter_name>":"<parameter_value>"
          },
          "result_variable": "<result_variable_name>"
        }
      ],
      "output": {
        "text": "response text"
      }
    }
    ```
    {: codeblock}

    `actions` 陣列指定要從對話進行的程式化呼叫。它最多可以定義五個不同的程式化呼叫。在 JSON 陣列中指定下列名稱/值配對：

    - `<actionName>`：必要。要呼叫的動作或服務的名稱。請使用所需的任何語法指定名稱。目標是指定用戶端應用程式將辨識及知道如何處理的名稱。名稱長度不得超過 256 個字元。

      例如：`calculateRate`

    - `<type>`：指出要進行的呼叫類型。（選用）指定 `client`，這是預設值。

      以外部用戶端應用程式瞭解的標準化格式傳送訊息回應與程式化呼叫資訊。您的用戶端應用程式必須使用所提供的資訊來執行程式化呼叫或功能，並將結果傳回至對話。回應內文中的 JSON 物件指定要呼叫的服務或函數、任何要與呼叫一起傳遞的關聯參數，以及要傳回的結果格式。

    - `<action_parameters>`：外部程式所預期的所有參數，指定為 JSON 物件。只有在外部程式需要參數時，才需要參數。

    - `<result_variable_name>`：用來參照外部服務或程式所傳回的 JSON 物件的名稱。結果會新增到 `/message` 回應的 context 區段。換言之，結果會儲存為環境定義變數，以顯示在節點回應中，或由稍後觸發的對話節點進行存取。環境定義變數的任何現有值都會改寫為動作所傳回的值。您可以使用下列語法來指定 `result_variable_name`：

      - `my_result`
      - `$my_result`

      名稱的長度不能超過 64 個字元。變數名稱不得包含下列字元：括弧 `()`、方括弧 (`[]`)、單引號 (`'`)、引號 (`"`) 或反斜線 (`\`)。

      如果您要將結果儲存至 `/message` 回應的輸出或輸入區段，則可以新增下列其中一個位置關鍵字，作為 `result_variable_name` 的字首：

       - `output.`：將結果新增至 /message 回應的輸出區段。例如，`output.my_result`。
       - `input.`：將結果新增至 /message 回應的輸入區段。例如，`input.my_result`。

      還可以指定 `context.` 位置關鍵字字首。例如，`context.my_result`。不過，您不需要這樣做，因為結果依預設會新增至環境定義。

      您可以在變數名稱中包含句點，以建立巢狀 JSON 物件。例如，您可以定義這些變數，從天氣服務的兩個不同要求中，擷取今天及明天的天氣預報結果：

      - `context.weather.today`
      - `context.weather.tomorrow`

      結果（`temp` 及 `rain` 參數值）會以下列結構儲存在環境定義中：

      ```json
      {
        "weather": {
          "today": {
            "temp": "20",
            "rain": "30"
          },
          "tomorrow": {
            "temp": "23",
            "rain": "80"
          }
        }
      }
      ```
      {: codeblock}

      如果單一 JSON 動作陣列中的多個動作將其程式化呼叫結果新增至相同的環境定義變數，則環境定義的更新順序會有關。根據動作類型，陣列中動作的定義順序可決定環境定義變數值的設定順序。陣列中最後一個動作所傳回的環境定義變數值會改寫任何其他動作所計算的值。

## Client 呼叫範例
{: #dialog-actions-client-example}

下列範例顯示外部天氣服務呼叫的可能結構。它會新增至與節點回應相關聯的 JSON 編輯器。在觸發節點層次回應時，空位已收集並儲存來自使用者的日期及位置資訊。此範例假設將呼叫的程式命名為 `MyWeatherFunction`，以及接受 `location` 和 `date` 參數，並傳回 JSON 物件：`{"forecast": "<value>"}`。

``` json
{
  "actions": [
    {
      "name": "MyWeatherFunction",
      "type": "client",
      "parameters": {
        "date": "$date",
        "location": "$location"
      },
      "result_variable": "context.my_forecast"
    }
  ]
}
```
{: codeblock}

通常，只有在需要新使用者輸入時（例如執行母項之後，以及執行它的其中一個子節點之前），服務才會從 POST  `/message` 要求傳回至用戶端。不過，如果您將 client 動作新增至節點，則在評估之後，服務一律會傳回至用戶端，以傳回動作呼叫的結果。為了避免在不應該輸入時等待使用者輸入（例如，針對配置成直接跳至子節點的節點），服務會將下列值新增至訊息環境定義：

```json
  {
    "context": {
      "skip_user_input": true
    }
  }
```
{: codeblock}

如果您要用戶端執行動作，但不想要取得使用者輸入，則可以遵循相同的慣例，並將 `skip_user_input` 環境定義變數新增至母節點，以與用戶端應用程式進行通訊。

您的用戶端應用程式應該一律檢查環境定義上的 `skip_user_input` 變數。如果存在的話，則會知道不要要求來自使用者的新輸入，而是執行動作，並將其結果新增至訊息，然後將它傳遞回服務。新的 POST 訊息要求應該包含前一個 POST 訊息回應所傳回的訊息（即環境定義、輸入、目的、實體及選用的輸出區段），且它應該包含從程式化呼叫所傳回的結果，而不是定義要進行的程式化呼叫的 JSON 物件。

在此節點之後跳至的子節點中，新增回應來顯示使用者：

``` json
{
  "output": {
    "text": {
      "values": [
        "It will be $my_forecast $date.literal in $location.literal."
      ]
    }
  }
```
{: codeblock}
