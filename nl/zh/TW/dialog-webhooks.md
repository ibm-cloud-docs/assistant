---

copyright:
  years: 2015, 2019
lastupdated: "2019-08-09"

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

# 從對話節點進行程式化呼叫
{: #dialog-webhooks}

若要進行程式化呼叫，請定義 Webhook，以將 POST 要求外呼傳送至執行程式化功能的外部應用程式。然後，您可以從一個以上的對話節點呼叫 Webhook。

Webhook 是一種機制，容許您根據程式中發生的情況呼叫外部程式。在對話技能中使用 Webhook 時，如果助理處理的是已啟用 Webhook 的節點，則會觸發 Webhook。Webhook 會收集指定的資料或在交談期間從使用者處收集到的資料，並將其儲存在環境定義變數中。Webhook 將這些資料作為 HTTP POST 要求的一部分傳送到在 Webhook 定義中指定的 URL。接收 Webhook 的 URL 是接聽器。該 URL 如 Webhook 定義中所指定，使用傳遞給它的資訊來執行預先定義動作，並且可以選擇性傳回回應。

請觀看此視訊以進一步瞭解。

<iframe class="embed-responsive-item" id="youtubeplayer" title="Webhook 展示" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/j8TBqD2rx2o?rel=0" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

您可以使用 Webhook 來執行下列類型的作業：

- 驗證您從使用者收集的資訊。
- 與外部 Web 服務互動以取得資訊。例如，您可以從空中交通服務查看班機預計抵達的時間，或從氣象服務取得天氣預報。
- 將要求傳送至外部應用程式（例如餐廳預約網站），以代表使用者完成簡單交易。
- 觸發 SMS 通知。
- 觸發 {{site.data.keyword.openwhisk}} Web 動作。

不能使用 Webhook 來呼叫使用以記號為基礎的 Identity and Access Management (IAM) 認證的 {{site.data.keyword.openwhisk_short}} 動作。
{: note}

如需如何呼叫用戶端應用程式的相關資訊，請參閱[從對話節點呼叫用戶端應用程式](/docs/services/assistant?topic=assistant-dialog-actions-client)。

## 定義 Webhook
{: #dialog-webhooks-create}

您可以為對話技能定義一個 Webhook URL，然後從一個以上的對話節點呼叫該 Webhook。

對外部服務的程式化呼叫必須符合下列需求：

- 呼叫必須是 POST HTTP 要求。
- 要求和回應的格式必須為 JSON。例如：`Content-Type: application/json`。
- 呼叫必須在 **5 秒內**傳回。

  對於您需要呼叫但較不具效率的服務，您可以透過用戶端應用程式來管理呼叫，並以個別步驟將資訊傳遞給對話。如需相關資訊，請參閱[從對話節點呼叫用戶端應用程式](/docs/services/assistant?topic=assistant-dialog-actions-client)。
  {: tip}

若要新增 Webhook 詳細資料，請完成下列步驟：

1.  從您要新增 Webhook 的技能，按一下**選項**標籤。

1.  按一下 **Webhook**。

1.  在 **URL** 欄位中，新增您要傳送 HTTP POST 要求呼叫的目標外部應用程式 URL。

    例如，若要呼叫 Language Translator 服務，請指定服務實例的 URL。

    ```bash
    https://gateway.watsonplatform.net/language-translator/api/v3/translate?version=2018-05-01
    ```
    {: codeblock}

    如果您呼叫的外部應用程式傳回回應，它必須能夠以 JSON 格式將回應傳送回來。例如，對於 Language Translator 服務，必須指定要傳回的結果的格式。作法是將標頭傳遞給服務。

1.  在「標頭」區段中，按一下**新增標頭**來新增要傳遞給服務的任何標頭，一次新增一個標頭。

    例如，新增標頭，指出您要以 JSON 格式傳回產生的值。

    <table>
    <caption>標頭範例</caption>
      <tr>
      <th>標頭名稱</th>
      <th>標頭值</th>
      </tr>
      <tr>
      <td>Content-Type</td>
      <td>application/json</td>
      </tr>
    </table>
    
1.  如果外部服務要求您在要求中傳遞基本鑑別認證，請提供這些認證。請按一下**新增授權**，將您的認證新增至**使用者名稱**及**密碼**欄位，然後按一下**儲存**。 

    產品將根據認證建立 Base64 編碼的 ASCII 字串，產生標頭並將其新增到頁面。 

    <table>
    <caption>標頭範例</caption>
      <tr>
      <th>標頭名稱</th>
      <th>標頭值</th>
      </tr>
      <tr>
      <td>Authorization</td>
      <td>Basic `<encoded-api-key>`</td>
      </tr>
    </table>
    
    對於測試目的，可以提交基於基本鑑別的 API 金鑰。按一下**新增授權**，然後將 `apikey` 新增至**使用者名稱**欄位，並將 API 金鑰值貼入**密碼**欄位。按一下**儲存**。
    {: tip}

您的 Webhook 詳細資料會自動儲存。

## 將 Webhook 外呼新增至對話節點
{: #dialog-webhooks-dialog-node-callout}

若要從對話節點使用 Webhook，您必須在節點上啟用 Webhook，然後新增外呼的詳細資料。

1.  按一下**對話**標籤。

1.  找到要在其中新增外呼的對話節點。每當在與使用者的交談期間觸發此節點時，都將對 Webhook 進行外呼。

    例如，建議您從 `#General_Greetings` 節點傳送 Webhook 外呼。

1.  按一下以開啟對話節點，然後按一下**自訂**。

1.  向下捲動到 *Webhook* 區段、將開關切換為**開啟**，然後按一下**套用**。

    如果您尚未啟用*多個條件式回應* 設定，系統會自動將此設定切換為開啟，而且您無法將其停用。啟用此設定是為了支援新增根據 Webhook 呼叫成功或失敗而執行的不同回應。如果已為節點指定回應，則這將成為第一個條件式回應。
    {: note}

1.  在*參數* 區段中，將您要傳遞給外部應用程式的任何資料，新增為索引鍵及值的配對。

    例如，如果您呼叫 Language Translator 服務，則必須提供下列參數的值：

    <table>
    <caption>參數範例</caption>
      <tr>
        <th>索引鍵</th>
        <th>值</th>
        <th>說明</th>
      </tr>
      <tr>
        <td>model_id</td>
        <td>"en-es"</td>
        <th>識別輸入和輸出語言。在此範例中，要求是將英文 (en) 文字翻譯為西班牙文 (es)。</th>
      </tr>
      <tr>
        <td>text</td>
        <td>"How are you?"</td>
        <th>此參數包含您要服務翻譯的字串。您可以將此值寫在程式中、傳遞環境定義變數（例如 $saved_text），或將 `<? input.text ?>` 指定為此值，直接將使用者輸入傳遞到服務。</th>
      </tr>
    </table>

    在更複雜的使用案例中，您可能會在交談期間收集使用者的資訊，例如使用者的旅行計畫。您可以收集日期和目的地資訊，並將其儲存在環境定義變數中，以作為參數傳遞到外部應用程式。

    <table>
    <caption>旅行參數範例</caption>
      <tr>
        <th>索引鍵</th>
        <th>值</th>
      </tr>
      <tr>
        <td>depart_date</td>
        <td>$departure</td>
      </tr>
    <tr>
        <td>arrive_date</td>
        <td>$arrival</td>
      </tr>
    <tr>
        <td>origin</td>
        <td>$origin</td>
      </tr>
    <tr>
        <td>destination</td>
        <td>$destination</td>
      </tr>
    </table>

1.  外呼所做的任何回應都會儲存至傳回變數。您可以重新命名自動新增至**傳回變數**欄位的變數。如果外呼導致錯誤，則此變數會設定為 `null`。

    產生的變數名稱的語法為 `Webhook_result_n`，其中每次將 Webhook 外呼新增至對話節點時，附加的 `_n` 都會遞增。此命名慣例確定環境定義變數名稱在整個對話技能中是唯一的。如果您變更名稱，請務必使用唯一名稱。

1.  在條件式回應區段中，會自動新增兩個回應條件，一個回應在 Webhook 外呼成功時顯示，並送回一個傳回變數。另一個回應在外呼失敗時顯示。您可以編輯這些回應，也可將更多條件式回應新增至節點。

    如果外呼傳回回應，而且您知道 JSON 回應的格式，則您可以編輯對話節點回應，使其只包含您要與使用者共用的回應的區段。 
    
    例如，Language Translator 服務會傳回與下面類似的物件：
    
    ```json
       {
       "translations":[
          {"translation":"¿Cómo estás?"}
       ],
       "word_count":3,
       "character_count":12
       }
    ```
    {: codeblock}
    
    使用僅擷取譯文值的 SpEL 表示式。

    <table>
    <caption>條件式回應範例</caption>
      <tr>
        <th>條件</th>
        <th>回應</th>
      </tr>
      <tr>
        <td>$webhook_result_1</td>
        <td>西班牙文的字組為：<? $webhook_result_1.translations[0].translation ?>。</td>
      </tr>
      <tr>
        <td>anything_else</td>
        <td>對外部應用程式的呼叫已失敗。請稍後重試。</td>
      </tr>
    </table>

    如果您將建議的格式用於回應，並且傳回先前顯示的翻譯回應，則助理對使用者的回應將為：`Your words in Spanish: ¿Cómo estás?`

1.  完成時，按一下 X 關閉節點。您的變更會自動儲存。

## 測試 Webhook
{: #dialog-webhooks-test}

第一次新增 Webhook 外呼時，查看外部應用程式回應中所傳回的確切內容、資料及其格式十分有用。若要這麼做，請將表示式 `$Webhook_result_n` 新增為成功外呼條件式回應的文字回應，其中 `n` 是要測試的 Webhook 的適當號碼。

此回應會將傳回變數的完整主體傳回，因此您可以查看外呼所送回的內容，並決定要與使用者共用的內容。然後，您可以使用[表示式語言方法](/docs/services/assistant?topic=assistant-dialog-methods)中記錄的方法從回應中僅擷取您關注的資訊。

測試某些使用者輸入是否會在外呼中產生錯誤，並建置處理這些狀況的方式。外部應用程式產生的錯誤會儲存在 `output.webhook_error.<result_variable>` 中。在測試以擷取這類錯誤時，您可以使用與下面類似的條件式回應：

|條件|回應|
|-----------|----------|
|output.webhook_error| 外呼已產生下列錯誤：<? output.webhook_error.webhook_result_1 ?>。|

例如，您可能沒有正確鑑別要求 (401)，或者可能嘗試使用已由外部應用程式使用的名稱來傳遞參數。部署 Webhook 之前，請測試 Webhook 以探索並修正這些類型的錯誤。

## 移除 Webhook
{: #dialog-webhooks-delete}

如果您決定不要從對話節點進行 Webhook 呼叫，請開啟節點的*自訂* 頁面，然後將 Webhook 切換為**關閉**。

即會從對話節點編輯器中移除*參數* 區段和**傳回變數**欄位。不過，仍會保留為您新增或您自己新增的任何條件式回應。

*多個條件式回應* 區段又將變為可編輯。您可以選擇關閉此特性。如果您這樣做，則只有第一個條件式回應會儲存為節點的唯一文字回應。

若要變更從對話節點呼叫的外部服務，請編輯**選項**標籤的 Webhook 頁面上定義的 Webhook 詳細資料。如果新服務預期向其傳遞不同的參數，則請務必更新呼叫該服務的所有對話節點。

## 呼叫 IBM Cloud Functions
{: #dialog-webhooks-cf}

您將編寫 Webhook URL，並根據是要呼叫標準動作還是 Web 動作，以不同方式提供標頭。

### 呼叫 Web 動作
{: #dialog-webhooks-cf-web-action}

下列提示將協助您從對話呼叫 {{site.data.keyword.openwhisk_short}} Web 動作。 

1.  開啟技能的**選項**頁面，然後按一下 **Webhook**。

1.  在 **URL** 欄位中，新增您要傳送 HTTP POST 要求呼叫的目標外部應用程式 URL。

    例如，若要呼叫 {{site.data.keyword.openwhisk_short}} Web 動作，請指定公用 Web 動作的 URL。例如：

    ```bash
    https://us-south.functions.cloud.ibm.com/api/v1/web/my_org_dev/default/Hello%20World.json
    ```
    {: codeblock}

    如果您呼叫的外部應用程式傳回回應，它必須能夠以 JSON 格式將回應傳送回來。

    請注意，此範例中的要求 URL 以 `.json` 結尾。藉由指定此副檔名，您可以充分運用 Web 動作的特性來指定回應的所需內容類型。指定此副檔名類型時，確定在 Web 動作能以多種格式傳回回應時傳回 JSON 回應。如需詳細資料，請參閱[額外特性](/docs/openwhisk?topic=cloud-functions-actions_web#actions_web_extra){: new_window}。
    {: tip}

1.  您不需要新增任何標頭。

    不需要鑑別 {{site.data.keyword.openwhisk_short}} Web 動作，因此您不需要定義 Authorization 標頭。

    您的 Webhook 詳細資料會自動儲存。

1.  按一下**對話**標籤。

1.  按一下以開啟您要從該處呼叫 Web 動作的對話節點，然後按一下**自訂**。

1.  向下捲動到 *Webhook* 區段、將開關切換為**開啟**，然後按一下**套用**。

1.  在*參數* 區段中，將您要傳遞給外部應用程式的任何資料，新增為索引鍵及值的配對。

    例如，如果您呼叫 Hello World {{site.data.keyword.openwhisk_short}} Web 動作，則可能會想要新增下列資訊以在該應用程式接受的訊息參數中進行傳遞：

    <table>
    <caption>參數範例</caption>
      <tr>
        <th>索引鍵</th>
        <th>值</th>
      </tr>
      <tr>
        <td>message</td>
        <td>"hello"</td>
      </tr>
    </table>

    呼叫 {{site.data.keyword.openwhisk_short}} Web 動作時，傳遞的參數具有的索引鍵不能與 Web 動作中定義的參數相同。如需詳細資料，請參閱[受保護的參數](/docs/openwhisk?topic=cloud-functions-actions_web#actions_web_protect){: new_window}。
    {: note}

1.  您可以編輯對話節點回應，使其只包含您要顯示給使用者看的回應區段。 

    Hello World {{site.data.keyword.openwhisk_short}} Web 動作在其回應中包含訊息名稱和值配對以及其他資訊。若只要顯示訊息的內容，您可以使用 `<return-variable>.message` 語法，只從回應物件中擷取訊息區段。

    <table>
    <caption>條件式回應範例</caption>
      <tr>
        <th>條件</th>
        <th>回應</th>
      </tr>
      <tr>
        <td>$webhook_result_1</td>
        <td>應用程式已傳回 "$webhook_result_1.message"。</td>
      </tr>
      <tr>
        <td>anything_else</td>
        <td>對外部應用程式的呼叫已失敗。請稍後重試。</td>
      </tr>
    </table>

1.  完成時，按一下 X 關閉節點。您的變更會自動儲存。

### 呼叫標準動作
{: #dialog-webhooks-cf-action}

可以呼叫 Cloud Foundry 管理的動作，但不能呼叫使用以記號為基礎的 Identity and Access Management (IAM) 鑑別的動作。

{{site.data.keyword.openwhisk_short}} 動作同時支援同步或非同步呼叫。不過，不能使用 Webhook 對 {{site.data.keyword.openwhisk_short}} 動作發出非同步呼叫。傳送非同步要求時，只會傳回啟動 ID。

若要對 Cloud Foundry 管理的 {{site.data.keyword.openwhisk_short}} 動作發出同步呼叫，請完成下列步驟：

1.  從您要新增 Webhook 的技能，按一下**選項**標籤。

1.  按一下 **Webhook**。

1.  在 **URL** 欄位中，指定動作的 URL。 

    將 `?blocking=true` 參數附加到動作 URL，以強制進行同步呼叫。

    此語法會傳送同步要求：

    ```bash
    https://us-south.functions.cloud.ibm.com/api/v1/namespaces/my_org_dev/actions/Hello%20World?blocking=true
    ```
    {: codeblock}

    呼叫動作時，必須提供與動作相關聯的 API 金鑰作為標頭，這將在下一步中進行說明。

1.  提供授權詳細資料及要求。

    若要呼叫 Cloud Foundry 所管理的 {{site.data.keyword.openwhisk_short}} 動作，請完成下列步驟：

    1.  取得 API 金鑰。在「{{site.data.keyword.openwhisk_short}} 端點」頁面中，找到 CURL 區段，然後按一下眼睛圖示以顯示金鑰詳細資料。複製 curl 指令中 `-u` 參數後列出的 `user ID:password` 金鑰。
    
    1.  請按一下**新增授權**，將您的認證新增至**使用者名稱**及**密碼**欄位，然後按一下**儲存**。 

    這將對認證進行編碼，產生標頭並將其新增到頁面。 
    
    ![顯示「選項」頁面的 URL 欄位和「標頭」區段。](images/webhook-to-cfaction.png)

    <!-- - If you are calling a {{site.data.keyword.openwhisk_short}} action that is managed by IBM Cloud Identity and Access Management (IAM) instead of CLoud Foundry, then you must provide an IAM bearer token to authenticate the request. 
    
      1. Follow the instructions in [Passing an IBM Cloud IAM token to authenticate with a service's API](/docs/iam?topic=iam-iamapikeysforservices#token_auth). 
      
      1. To pass the IAM bearer token, use a header like this:
    
         <table>
         <caption>Header example</caption>
           <tr>
             <th>Header name</th>
             <th>Header value</th>
           </tr>
           <tr>
             <td>Authorization</td>
             <td>Bearer `<IAM token>`</td>
           </tr>
         </table>

        For more information about how to manage IAM tokens, see this [IBM Developer article](https://developer.ibm.com/tutorials/accessing-iam-based-services-from-ibm-cloud-functions/){: external}.
    -->
您的 Webhook 詳細資料會自動儲存。

1.  按一下**對話**標籤。

1.  按一下以開啟您要從該處呼叫 Web 動作的對話節點，然後按一下**自訂**。

1.  向下捲動到 *Webhook* 區段、將開關切換為**開啟**，然後按一下**套用**。

1.  在*參數* 區段中，將您要傳遞給外部應用程式的任何資料，新增為索引鍵及值的配對。

    呼叫 {{site.data.keyword.openwhisk_short}} 動作時，您*可以* 傳遞的參數具有的索引鍵不能與動作中定義的參數相同。與 Web 動作的參數不同的是，不會保留這些參數。

1.  您可以編輯對話節點回應，使其只包含您要顯示給使用者看的回應區段。 

    例如，在條件式回應區段中，您可以使用語法為 `$webhook_result.response.result.message` 的表示式，僅擷取傳回的訊息。

    <table>
    <caption>條件式回應範例</caption>
      <tr>
        <th>條件</th>
        <th>回應</th>
      </tr>
      <tr>
        <td>$webhook_result_1</td>
        <td>應用程式已傳回 "$webhook_result.response.result.message"。</td>
      </tr>
      <tr>
        <td>anything_else</td>
        <td>對外部應用程式的呼叫已失敗。請稍後重試。</td>
      </tr>
    </table>

1.  完成時，按一下 X 關閉節點。您的變更會自動儲存。
