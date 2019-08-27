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

# 進階作業
{: #logs-resources}

瞭解 API 和您可以用來存取及分析日誌資料的其他工具。
{: shortdesc}

## API
{: #logs-resources-api}

您可以使用 `/logs` API，來列出您的使用者與助理之間所發生的交談記錄中的事件。如需詳細的 API 參考文件，請參閱[列出日誌事件 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://cloud.ibm.com/apidocs/assistant#list-log-events-in-a-workspace)。

儲存日誌的天數會因服務方案類型而有所不同。如需詳細資料，請參閱[日誌限制](/docs/services/assistant?topic=assistant-logs#logs-limits)。

如需能夠執行以匯出日誌並將其轉換為 CSV 格式的 Python Script，請從 [Watson Assistant GitHub ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/watson-developer-cloud/community/blob/master/watson-assistant/export_logs.py) 儲存庫下載 `export_logs.py` 檔案。

## 日誌相關術語
{: #logs-resources-terminology}

首先，請檢閱與 {{site.data.keyword.conversationshort}} 日誌相關聯的術語定義：

- ***助理***：應用程式 - 有時稱為「聊天機器人」- 會實作您的 {{site.data.keyword.conversationshort}} 內容。
- ***交談***：一組訊息，包含個別使用者傳送給助理的訊息，以及助理送回的訊息。
- ***交談 ID***：新增至個別訊息呼叫以鏈結相關訊息交換的唯一 ID。應用程式開發人員利用第 1 版 {{site.data.keyword.conversationshort}} API，藉由在環境定義物件的 meta 資料中包括此 ID，將此值新增至交談中的訊息呼叫。
- ***客戶 ID***：可用來標示客戶資料的唯一 ID，如果客戶要求移除其資料，隨後即可將它刪除。
- ***部署 ID***：指出應用程式開發人員使用第 1 版 {{site.data.keyword.conversationshort}} API 的唯一標籤，隨每則使用者訊息一同傳遞，以協助識別產生該訊息的部署環境。
- ***實例***：您的 {{site.data.keyword.conversationshort}} 部署，可使用唯一的認證來進行存取。{{site.data.keyword.conversationshort}} 實例可能包含多個助理。
- ***訊息***：訊息是使用者傳送給助理的單一話語。
- ***技能 ID***：技能的唯一 ID。
- ***使用者***：使用者是與您的助理互動的任何人；通常是您的客戶。
- ***使用者 ID***：這個唯一標籤可用來追蹤特定使用者的服務水準使用情形。
- ***工作區 ID***：工作區的唯一 ID。雖然在 11 月 9 日之前建立的任何工作區都會在產品使用者介面中顯示為技能，但技能和工作區並不是一回事。技能實際上是第 1 版工作區的封套。

**重要事項**：**使用者 ID** 內容*不* 等於**客戶 ID** 內容，但兩者都可以與訊息一起傳遞。**使用者 ID** 欄位是用來追蹤計費用途的用量層次，而**客戶 ID** 欄位是用來支援對與一般使用者相關聯的訊息所進行的標籤作業及後續刪除。客戶 ID 在所有 Watson 服務之間的使用是一致的，並且指定在 `X-Watson-Metadata` 標頭中。使用者 ID 由 {{site.data.keyword.conversationshort}} 服務專用，並傳入每個 /message API 呼叫的環境定義物件。

## 啟用使用者度量
{: #logs-resources-user-id}

例如，使用者度量可讓您在[概觀頁面](/docs/services/assistant?topic=assistant-logs-overview)查看與您的助理互動的唯一使用者數目，或在給定時間間隔內每位使用者的平均交談次數。使用者度量是利用唯一的 `User ID` 參數來啟用。

若要為使用 `/message` API 傳送的訊息指定 `User ID`，請在 [context ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://cloud.ibm.com/apidocs/assistant?curl=#get-response-to-user-input){: new_window} 的 metadata 物件內包含 `user_id` 內容，如此範例所示：

```
"context" : {
            "metadata" : {
       "user_id": "{UserID}"
  }
}
```
{: codeblock}

## 建立訊息資料與使用者的關聯以進行刪除
{: #logs-resources-customer_id}

有時候您會想要從 {{site.data.keyword.conversationshort}} 實例中完整移除一組使用者資料。使用刪除特性後，「概觀」度量將不再反映那些已刪除的訊息；例如，它們將會有較少的「交談總數」。

### 開始之前
{: #logs-resources-delete-customer-id-prereqs}

若要刪除一位以上人員的訊息，首先您需要建立訊息與每個人的唯一**客戶 ID** 的關聯。若要為使用 `/message` API 進行傳送的任何訊息指定**客戶 ID**，請在您的標頭中包含 `X-Watson-Metadata: customer_id` 內容。您可以使用 `customer_id` 來傳遞多個**客戶 ID** 項目，這些項目含有以分號區隔的 `field=value` 配對，如下列範例所示：

```
curl -X POST -u "apikey:3Df... ...Y7Pc9"
 --header
   'Content-Type: application/json'
   'X-Watson-Metadata: customer_id={first-customer-ID};customer_id={second-customer-ID}'
 --data '{"input":{"text":"hello"}}' 'https:// gateway-us-south.watsonplatform.net/assistant/api/v1/workspaces/{workspaceID}/message?version=2018-09-20'
```
{: codeblock}

`customer_id` 字串不可包括分號 (`;`) 或等號 (`=`) 字元。您負責確保每個 `Customer ID` 參數在您客戶之間是唯一的。
{: note}

若要使用 `customer_id` 值來刪除訊息，請參閱[資訊安全](/docs/services/assistant?topic=assistant-information-security#information-security-gdpr-wa)主題。

## Jupyter Notebook
{: #logs-resources-jupyter-notebooks}

IBM 建立的 Jupyter Notebook 可讓您更詳細地分析日誌資料。Jupyter Notebook 是用於互動式運算的 Web 型環境。您可以執行一小段用來處理資料的程式碼，然後立即檢視運算結果。

有一組記事本可讓您與標準 Python 工具搭配使用，還有一組是為了 {{site.data.keyword.DSX_full}} 的最佳使用而設計。這個 {{site.data.keyword.DSX_short}} 產品提供環境讓您挑選所需的工具，用來分析及視覺化資料、清理及製作資料、汲取串流資料，或建立、訓練及部署機器學習模型。如需詳細資料，請參閱[產品說明文件 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://dataplatform.cloud.ibm.com/docs/content/getting-started/welcome-main.html){:new_window}。

若要進一步瞭解記事本如何協助您改善助理，請[閱讀本部落格文章 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://medium.com/ibm-watson/continuously-improve-your-watson-assistant-with-jupiter-notebooks-60231df4f01f)。

下列是可用的記事本：

- **測量**：收集聚焦於涵蓋範圍（助理有足夠信心回應使用者的頻率）和有效性（當助理回應時，該回應是否能滿足使用者的需要）的度量。

- **有效性**：執行日誌的更深入分析，以協助您瞭解可以採取哪些步驟來改善助理。

[Watson Assistant Continuous Improvement 最佳作法手冊 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://www.ibm.com/common/ssi/cgi-bin/ssialias?htmlfid=54022554USEN&) 說明如何充分利用記事本。

### 將記事本與 {{site.data.keyword.DSX}} 搭配使用
{: #logs-resources-notebooks-studio}

如果您選擇使用為了搭配 {{site.data.keyword.DSX}} 而設計的記事本，則步驟大約如下：

1.  建立 {{site.data.keyword.DSX}} 帳戶[建立專案 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://dataplatform.cloud.ibm.com/docs/content/getting-started/projects.html?context=analytics){:new_window}，然後在其中新增 Cloud Object Storage 帳戶。
1.  從 {{site.data.keyword.DSX}} 社群取得[測量 Watson Assistant Performance 記事本 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")]( https://dataplatform.cloud.ibm.com/exchange/public/entry/view/133dfc4cd1480bbe4eaa78d3f635e568)。
1   遵循記事本提供的逐步指示，以分析日誌中的對話交換子集。

    利用更容易瞭解助理的涵蓋範圍和有效性的方式，將見解視覺化。
1.  從無效交談中匯出一組日誌範例，然後分析並予以註釋。

    例如，指出回應是否正確。如果正確，請標示它是否有幫助。如果回應不正確，請識別主要原因，例如，偵測到錯誤目的或實體，或觸發了錯誤的對話節點。識別主要原因之後，請指出什麼才是正確的選擇。
1.  將註釋的試算表提供給[分析 Watson Assistant 有效性記事本](https://dataplatform.cloud.ibm.com/exchange/public/entry/view/133dfc4cd1480bbe4eaa78d3f636921c)。

此處理程序可協助您瞭解可以採取哪些步驟來改善助理。無法將您學到的內容自動套用回服務實例。追蹤您為了改善系統所做的全部變更，這樣之後您就可以直接將它們套用至對話技能的訓練資料。

### 將記事本與標準 Python 工具搭配使用
{: #logs-resources-notebooks-python}

如果您選擇使用標準 Python 工具來執行記事本，可以從 [IBM GitHub 儲存庫](https://github.com/watson-developer-cloud/assistant-improve-recommendations-notebook/tree/master/notebook)取得記事本。請務必依下列順序執行它們：

1.  測量 Notebook.ipynb
1.  有效性 Notebook.ipynb
