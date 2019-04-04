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

# 資訊安全
{: #information-security}

IBM 致力於為我們的客戶及夥伴提供創新的資料隱私權、安全及控管解決方案。
{: shortdesc}

**注意事項：**
「客戶」負責確保其自己符合各種法令規章，包括「歐盟一般資料保護法規 (GDPR)」。有關任何可能影響「客戶」業務之相關法令規章之確認與解釋，以及任何「客戶」為遵循該等法令規章所需採取之行動，「客戶」應自行負責向法律顧問取得諮詢意見。

本文所述的產品、服務及其他功能並不適合所有「客戶」情況，其可用性可能受限。IBM 不提供法律、會計或審核意見，亦不聲明或保證其服務或產品可確保「客戶」符合任何法令規章。

如果您需要針對建立的 {{site.data.keyword.cloud}} {{site.data.keyword.watson}} 資源要求 GDPR 支援

- 在歐盟中，請參閱[針對在歐盟中建立的 IBM Cloud Watson 資源要求支援 ![外部鏈結圖](../../icons/launch-glyph.svg "外部鏈結圖示")](https://cloud.ibm.com/docs/services/watson/getting-started-gdpr-sar#request-EU){: new_window}。
- 在歐盟外部，請參閱[針對歐盟外的資源要求支援 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://cloud.ibm.com/docs/services/watson/getting-started-gdpr-sar#request-non-EU){: new_window}。

## 歐盟一般資料保護法規 (GDPR)
{: #information-security-gdpr}

IBM 致力於為我們的客戶及合作夥伴提供創新的資料隱私權、安全及控管解決方案，以協助他們邁向 GDPR 法規遵循。

進一步瞭解 IBM 對 GDPR 準備的行程，以及我們的 GDPR 功能及產品與服務，以支援您遵循法規 [這裡 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](../../icons/launch-glyph.svg "外部鏈結圖")](http://www.ibm.com/gdpr){: new_window}.

## 在 {{site.data.keyword.conversationshort}} 中標示及刪除資料
{: #information-security-gdpr-wa}

請不要將個人資料新增至您建立的訓練資料（實體與目的，包括使用者範例）。具體來說，對於包含實際使用者話語，且您為了發掘使用者範例建議而上傳的檔案，請務必移除其中的所有個人識別資訊。

**附註：**實驗性及測試版特性不適用於正式作業環境，因此，在標示及刪除資料時，不保證如預期般運作。在實作需要標示及刪除資料的解決方案時，不應使用實驗性及測試版特性。

如果您需要從 {{site.data.keyword.conversationshort}} 實例中移除客戶的訊息資料，只要在訊息傳送至服務時，建立訊息與客戶 ID 的關聯，即可根據用戶端的客戶 ID 來執行此作業。

**附註：**「預覽鏈結」和自動 Facebook 整合特性，不支援根據客戶 ID 來標示資料然後予以刪除。在需要可以根據客戶 ID 來進行刪除的解決方案中，不應使用這些特性。

### 開始之前
{: #information-security-delete-user-data-prereqs}

為了能夠刪除與特定使用者相關聯的訊息資料，您必須先建立所有訊息與每位使用者之唯一**客戶 ID** 的關聯。若要為使用 `/message` API 進行傳送的任何訊息指定**客戶 ID**，請在您的標頭中併入 `X-Watson-Metadata: customer_id` 內容。例如：

```
curl -X POST -u "apikey:3Df... ...Y7Pc9"
 --header
   'Content-Type: application/json'
   'X-Watson-Metadata: customer_id=abc'
 --data
   '{"input":{"text":"hello"}}'
  'https://gateway-eu-de.watsonplatform.net/assistant/api/v1/workspaces/{workspaceID}/message?version=2018-09-20'
```
{: codeblock}

`customer_id` 字串不可包括分號 (`;`) 或等號 (`=`) 字元。您負責確保每個 `customer ID` 內容在您客戶之間是唯一的。
{: note}

利用以分號區隔的 `customer_id={value}` 配對，即可傳遞多個**客戶 ID** 值。例如：`'X-Watson-Metadata: customer_id=abc;customer_id=xyz'`

如果您將搜尋技能新增至助理，則提交至助理的使用者輸入會傳遞至 {{site.data.keyword.discoveryshort}} 服務作為搜尋查詢。如果 {{site.data.keyword.conversationshort}} 整合提供客戶 ID，則產生的 /message API 要求會將客戶 ID 包括在標頭中，而 ID 會傳遞至 {{site.data.keyword.discoveryshort}} /query API 要求。若要刪除與特定客戶相關聯的任何查詢資料，您必須將刪除要求直接傳送至鏈結您助理的 {{site.data.keyword.discoveryshort}} 服務實例。如需詳細資料，請參閱 {{site.data.keyword.discoveryshort}} [資訊安全](https://cloud.ibm.com/docs/services/discovery/information-security#gdpr-discovery)主題。

### 查詢使用者資料
{: #information-security-query-customer-id}

使用第 1 版的 `/logs` 方法 `filter` 參數，來搜尋應用程式日誌中的特定使用者資料。例如，若要搜尋符合 `my_best_customer` 的 `customer_id` 特定資料，查詢可能是：

``` curl
curl -X GET -u "apikey:3Df... ...Y7Pc9"
'https://gateway-eu-de.watsonplatform.net/assistant/api/v1/workspaces/{workspaceID}/logs?version=2018-09-20&filter=customer_id::my_best_customer'
```
{: codeblock}

如需其他詳細資料，請參閱[過濾查詢參考資料](/docs/services/assistant?topic=assistant-filter-reference)。

### 刪除資料
{: #information-security-delete-data}

若要刪除與特定使用者相關聯、且服務可能已儲存的任何訊息日誌，請使用 `DELETE /user_data` 第一版 API 方法。藉由與要求一起傳遞 `customer_id` 參數的方式，來指定使用者的客戶 ID。

只有使用 `POST /message` API 端點與關聯的客戶 ID 所新增的資料，才能使用這個 delete 方法來進行刪除。其他方法所新增的資料無法根據客戶 ID 進行刪除。例如，從客戶交談所新增的實體及目的，無法以這個方法刪除。那些方法不支援「個人資料」。

**重要事項**：指定 `customer_id` 會跨整個 {{site.data.keyword.conversationshort}} 實例（而非只在一個技能內），刪除具有該 `customer_id` 且在刪除要求之前即已收到的*所有* 訊息。

例如，若要從 {{site.data.keyword.conversationshort}} 實例中，刪除與客戶 ID 為 `abc` 之使用者相關聯的任何訊息資料，請傳送下列 cURL 指令：

```
curl -X DELETE -u "apikey:3Df... ...Y7Pc9"
 'https://gateway-eu-de.watsonplatform.net/assistant/api/v1/user_data?customer_id=abc&version=2018-09-20'
```
{: codeblock}

傳回空的 JSON 物件 `{}`。

如需相關資訊，請參閱 [API 參考資料](https://cloud.ibm.com/apidocs/assistant?curl=#delete-labeled-data)。

**附註：**刪除要求是以批次處理，最多可能需要 24 個小時才能完成。
