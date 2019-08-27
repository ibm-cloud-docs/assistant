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

# IBM Cloud 服務資訊
{: #services-information}

助理是由 {{site.data.keyword.cloud}} 管理的完全託管機器人，這表示您不需要擔心如何設定或維護基礎架構來支援它。
{: shortdesc}

## 服務方案資訊
{: #services-information-plans}

探索 {{site.data.keyword.conversationshort}} [服務方案選項 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://www.ibm.com/cloud/watson-assistant/pricing/){: new_window}。

在建立服務實例之前，請決定您要如何組織 {{site.data.keyword.cloud_notm}} 帳戶中的資源。如果您未定義自己的資源群組，則會使用**預設**資源群組，且您以後*無法* 變更它。如需詳細資料，請參閱[在資源群組中組織資源的最佳作法 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://cloud.ibm.com/docs/resources/bestpractice_rgs#bp_resourcegroups){: new_window}。所有使用者都必須擁有「操作員」平台存取角色。（{{site.data.keyword.conversationshort}} 不會運用服務存取角色。）

若要找出現行實例所屬的服務方案，請完成下列步驟：

1.  記下目前所使用實例的名稱（您可以從主要「技能」或「助理」頁面中尋找和變更實例）。
1.  移至 [IBM Cloud 資源清單 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://cloud.ibm.com/resources) 頁面。
1.  展開**服務**區段，並找到您先前記下的實例名稱，然後按一下該名稱以查看相關聯的方案資訊。

### 依構件類型的方案限制
{: #services-information-limits}

每個方案之構件限制的相關資訊，可從說明如何建立構件的主題取得，因此，您可以在需要知道時參照這些限制。以下是這些主題的鏈結：

- [助理](/docs/services/assistant?topic=assistant-assistant-add#assistant-add-limits)
- [對話節點](/docs/services/assistant?topic=assistant-dialog-build#dialog-build-node-limits)
- [實體](/docs/services/assistant?topic=assistant-entities#entities-limits)
- [閒置逾時](/docs/services/assistant?topic=assistant-assistant-settings#assistant-settings-session-limits)
- [目的](/docs/services/assistant?topic=assistant-intents#intents-limits)
- [整合](/docs/services/assistant?topic=assistant-deploy-integration-add#deploy-integration-add-limits)
- [日誌](/docs/services/assistant?topic=assistant-logs#logs-limits)
- [技能](/docs/services/assistant?topic=assistant-skill-add#skill-add-limits)
- [版本](/docs/services/assistant?topic=assistant-versions#versions-limits)

### API 呼叫限制
{: #services-information-api-limits}

每個實例容許的 API 呼叫數目，取決於您的服務方案。如需詳細資料，請參閱方案說明。

如果您有「精簡」方案並達到 API 呼叫限制，但日誌顯示您進行的呼叫數低於預期，則請記住「精簡」方案只會儲存 7 天的日誌資訊。

如果您要從某個方案升級至另一個方案，請參閱[升級](/docs/services/assistant?topic=assistant-upgrade)。

### 加值和超值方案特性 ![僅限加值或超值方案](images/plus.png)
{: #services-information-premium}

下列特性僅適用於加值或超值方案使用者。

- [澄清](/docs/services/assistant?topic=assistant-dialog-runtime#dialog-runtime-disambiguation)
- [目的衝突解決](/docs/services/assistant?topic=assistant-intents#intents-resolve-conflicts)
- [目的建議和目的使用者範例建議](/docs/services/assistant?topic=assistant-intent-recommendations)
- [Intercom 整合](/docs/services/assistant?topic=assistant-deploy-intercom)
- [搜尋技能](/docs/services/assistant?topic=assistant-skill-search-add)

### 使用者型方案
{: #services-information-user-based-plans}

API 型方案是依指定時間範圍內所發出的 API 呼叫次數來測量用量，而新的「加值」方案和更新的「超值」方案則不同，它們是使用以使用者為基礎的計費。它們根據在指定時間範圍內與助理進行互動的唯一使用者數目來測量用量。

{{site.data.keyword.conversationshort}} 會檢查此訂單的 API 要求的下列資訊，以進行計費：

  1.  **user_id**：在 API 中定義的內容，會以 /message API 呼叫的環境定義物件傳送。使用此內容是確保您能將 /message API 呼叫準確地歸因於唯一使用者的最佳方法。如需使用者 ID 內容的相關資訊，請參閱 API 參考文件：
  
    - `context.global.system.user_id`：[第 2 版 API](https://cloud.ibm.com/apidocs/assistant-v2#send-user-input-to-assistant)
    - `context.metadata.user_id`：[第 1 版 API](https://cloud.ibm.com/apidocs/assistant#get-response-to-user-input)

  1.  **session_id**：在第 2 版 API 中定義的內容，能識別使用者與助理之間的單一交談。在內建整合所產生的 /message API 呼叫中，會提供一個階段作業 ID。當使用者關閉聊天視窗或聊天閒置了 60 分鐘之後，階段作業即會結束。

  1.  **conversation_id**：在第 1 版 API 中定義的內容，它儲存在 /message API 呼叫的環境定義物件中。此內容可用來識別與某位使用者的單一對話交流相關聯的多個 /message API 呼叫。不過，只有在您明確保留 ID，並在同一次交談的每個要求中將它傳回，才會使用相同的 ID。否則，對於每個新的 /message API 呼叫都會產生新的 ID。

若要從新的使用者型服務方案中獲得最大好處，請設計任何自訂應用程式，用它來部署助理去擷取唯一使用者 ID 或階段作業 ID，並將資訊傳遞至 {{site.data.keyword.conversationshort}}。

## 鑑別 API 呼叫
{: #services-information-authenticate-api-calls}

您的服務實例使用的鑑別機制會影響您在發出 API 呼叫時必須如何提供認證。

1.  取得服務認證。

    - 在 [{{site.data.keyword.Bluemix_notm}} 資源清單 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://cloud.ibm.com){: new_window} 中，尋找並按一下服務實例。

    - 按一下以開啟您的服務實例，按一下**服務認證**，然後按一下**檢視認證**。

      **Cloud Foundry 認證**

      ![顯示由 Cloud Foundry 管理實例的服務認證頁面。](images/cf-cred-ui.png)

      **IAM 認證**

      ![顯示由 IAM 管理實例的服務認證頁面。](images/iam-creds.png)

1.  在 API 呼叫中使用這些認證。

    **Cloud Foundry API 呼叫**

    提供您的使用者名稱和密碼認證。

    ```curl
    curl -X GET \
    --user {username}:{password} \
    'https://gateway.watson.net/assistant/api/v1/workspaces?version=2018-09-20'
    ```
    {: codeblock}

     **IAM API 呼叫**

    - 基本 URL 必須包含位置。使用語法 `gateway-<location>.watsonplatform.net` 來指定在其中建立服務實例的位置。位置碼列在*資料中心位置* 表格中。
    - 請在標頭中提供適當的記號類型。您可以傳遞載送記號或 API 金鑰。

      - 記號支援已鑑別要求，而不需要在每個呼叫中內含服務認證。下列範例顯示使用載送記號。

        ```curl
        curl -X GET \
        'https://gateway-syd.watsonplatform.net/assistant/api/v1/workspaces?version=2018-09-20' \
        --header 'Authorization: Bearer eyJhbGciOiJIUz......sgrKIi8hdFs'
        ```
        {: codeblock}

      - API 金鑰使用基本鑑別。下列範例顯示使用 apikey。

        ```curl
        curl -X GET -u "apikey:3Df... ...Y7Pc9" \
        'https://gateway-us-east.watsonplatform.net/assistant/api/v1/workspaces?version=2018-09-20' \
        ```
        {: codeblock}

        使用任何 Watson SDK 時，您可以傳遞 API 金鑰，並讓 SDK 管理記號的生命週期。
        {: note}

        無法使用「Cloud Foundry 指令行介面 (CLI)」來管理 IAM 資源。例如，建立或管理服務實例的 Cloud Foundry CLI 指令（開頭為 `cf`）不適用於由使用 IAM 的位置所管理的實例。相反地，您必須使用 {{site.data.keyword.cloud_notm}} CLI 及其相關聯的指令。如需詳細資料，請參閱[使用資源和資源群組 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](/docs/cli/reference/ibmcloud?topic=cloud-cli-ibmcloud_commands_resource)。

        如需相關資訊，請參閱[使用 IAM 記號鑑別 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](/docs/services/watson?topic=watson-iam){: new_window}。

    如需範例，請參閱 API 參考資料中您的語言所適用的[鑑別 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示 ")](https://{DomainName}/apidocs/assistant-v2#authentication){: new_window}。

### 資料中心
{: #services-information-regions}

{{site.data.keyword.cloud_notm}} 具有全球資料中心網路，為其雲端服務提供效能優勢。如需詳細資料，請參閱 [{{site.data.keyword.cloud_notm}} 全球資料中心 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://www.ibm.com/cloud/data-centers/){: new_window}。

{{site.data.keyword.cloud_notm}} 將使用 Cloud Foundry 管理使用者存取變更為使用記號型 Identity and Access Management (IAM) 鑑別。IAM 已在不同時間於不同位置推出。您可以移轉服務實例，將它從其現行 Cloud Foundry 組織及空間移至資源群組。如需詳細資料，請參閱[移轉](/docs/services/watson?topic=watson-migrate)。

您可以建立在下列資料中心位置管理的 {{site.data.keyword.conversationshort}} 服務實例：

|位置               | 位置碼 | 鑑別類型 | IAM 採用日期 | 附註 |
|-------------|---------------|---------------------|-------------------|-------|
| 達拉斯      | us-south      | IAM                 |2018 年 10 月 30 日| N/A |
| 法蘭克福    | eu-de         | IAM                 |2018 年 10 月 30 日| N/A |
| 雪梨        | au-syd        | IAM                 |2018 年 5 月 7 日| 在 5 月 7 日之前建立的實例已聯合至達拉斯|
| 東京        | jp-tok        | IAM                 |2018 年 11 月 8 日| N/A |
| 倫敦        | eu-gb、lon    | IAM                 |2018 年 12 月 13 日| 在 12 月 13 日之前於英國地區建立的實例已聯合至美國南部地區|
| 華盛頓特區  | us-east    | IAM                 |2018 年 6 月 14 日| N/A |
{: caption="資料中心位置" caption-side="top"}

如需管理其他 {{site.data.keyword.cloud_notm}} 服務之資料中心的相關資訊，請參閱[各地區的服務 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://cloud.ibm.com/docs/resources/services_region#services_region){: new_window}。

## 條款及安全
{: #services-information-terms}

若要進一步瞭解服務條款及資料安全，請閱讀下列資訊：

- [服務條款 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://www-03.ibm.com/software/sla/sladb.nsf/sla/home?OpenDocument){: new_window}
- [資料安全和隱私 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://www.ibm.com/software/sla/sladb.nsf/sla/csdsp?OpenDocument){: new_window}
- [資訊安全](/docs/services/assistant?topic=assistant-information-security)

如需 {{site.data.keyword.cloud_notm}} 的相關資訊，請參閱[平台概觀 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](/docs/overview?topic=overview-whatis-platform){: new_window}。

## 還有其他問題嗎？ 
{: #services-information-sales}

請與 [IBM 業務代表 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://www-01.ibm.com/marketing/iwm/dre/signup?source=urx-20970){: new_window} 聯絡。
