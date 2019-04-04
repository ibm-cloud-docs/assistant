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

# 建置搜尋技能
{: #skill-search-add}

助理使用*搜尋技能* 將複雜的客戶查詢遞送至 {{site.data.keyword.discoveryfull}} 服務。{{site.data.keyword.discoveryshort}} 將使用者輸入視為搜尋查詢。它會尋找與外部資料來源的查詢相關的資訊，並將它傳回給助理。
{: shortdesc}

此特性僅供測試版程式的參與者使用。若要瞭解如何要求存取權，請參閱[參與測試版程式](/docs/services/assistant?topic=assistant-feedback#feedback-beta)。

![測試版](images/beta.png) IBM 發行分類為測試版的評估的服務、特性及語言支援。這些特性可能不穩定、經常變更，且可能接到簡短通知就中斷服務。測試版特性也可能未提供正式發行特性所提供但不要用於正式作業環境的相同層次的效能或相容性。

您可以將某種搜尋技能新增至助理。如需每個方案之限制的相關資訊，請參閱[技能限制](/docs/services/assistant?topic=assistant-skill-add#skill-add-limits)。

搜尋技能會從您使用 {{site.data.keyword.discoveryshort}} 服務所建立的資料集合中搜尋資訊。{{site.data.keyword.discoveryshort}} 是用來搜索、轉換及正規化非結構化資料的一項服務。該服務會套用資料分析及認知直覺，來強化您的資料，讓您稍後更容易找到並擷取有意義的資訊。若要進一步瞭解 {{site.data.keyword.discoveryshort}}，請參閱[產品說明文件 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](/docs/services/discovery?topic=discovery-about)。

{{site.data.keyword.discoveryfull}} 服務是以下列方式觸發：

- **任何其他節點**：沒有任何對話節點可以處理使用者查詢時，會搜尋外部資料來源來尋找相關答案。助理不會顯示標準訊息，例如 `I don't know how to help you with that.`，而是說，`Maybe this information can help:`，後面會接著搜尋所傳回的段落。如果搜尋技能鏈結至您的助理，則每當觸發 `anything_else` 節點時，不會顯示節點回應，而是改為執行搜尋。助理會將使用者輸入當作查詢傳遞至搜尋技能，並傳回搜尋結果作為回應。
- **搜尋回應類型**：如果您將搜尋回應類型新增至對話節點，則該服務會從外部資料來源擷取段落，並傳回它作為特定問題的回應。只有在處理個別對話節點時，才會進行此類型的搜尋。如果您要在執行搜尋之前縮小使用者查詢，則此方法非常實用。例如，對話分支可能會收集客戶想要購買之裝置類型的相關資訊。若您知道廠牌和機型，您就可以在查詢中傳送機型關鍵字，該查詢將提交至搜尋技能，而獲得更適當的結果。
- **僅搜尋技能**：如果只有搜尋技能鏈結至助理，而沒有任何對話技能鏈結至助理，則當從其中一個助理的整合頻道接收到任何使用者輸入時，會將搜尋查詢提交至 {{site.data.keyword.discoveryshort}} 服務。

## 建立搜尋技能
{: #skill-search-add-task}

如果您尚未這麼做，請完成[入門指導教學](/docs/services/assistant?topic=assistant-getting-started#getting-started-prerequisites)中的必要步驟，以建立 {{site.data.keyword.conversationshort}} 服務實例，並啟動 {{site.data.keyword.conversationshort}} 工具。

您可以使用 {{site.data.keyword.conversationshort}} 工具來建立技能。請遵循下列步驟來建立搜尋技能：

1.  按一下**技能**標籤。

1.  按一下**建立新的項目**。

1.  按一下**新增**，以建立*搜尋技能*。

1.  指定新技能的詳細資料：
    - **名稱**：名稱長度不得超過 100 個字元。名稱是必要項目。
    - **說明**：選用說明長度不得超過 200 個字元。

1.  按一下**建立**。

其餘的步驟會根據您是否有權存取已建立集合的現有 {{site.data.keyword.discoveryshort}} 服務實例而有所不同。請根據您的狀況，遵循適當的程序：

- [連接至現有 Watson Discovery 實例](#skill-search-add-connect-discovery)
- [建立 Watson Discovery 實例](#skill-search-add-create-discovery)

## 連接至現有 Watson Discovery 服務實例
{: #skill-search-add-connect-discovery}

1.  選擇您要從中擷取資訊的 {{site.data.keyword.discoveryshort}} 服務實例。
{: #choose-d-instance}

    清單中即會顯示您有權存取的任何 {{site.data.keyword.discoveryshort}} 服務實例。

    如果您看到一則警告指出您有些 {{site.data.keyword.discoveryshort}} 服務實例還沒有設定認證，這表示您有權存取至少一個您從未直接從 {{site.data.keyword.cloud_notm}} 儀表板開啟的實例。您必須存取服務實例，才能為它建立認證。而且，在 {{site.data.keyword.conversationshort}} 可以代表您建立與 {{site.data.keyword.discoveryshort}} 服務實例的連線之前，認證必須存在。如果您認為某一個 {{site.data.keyword.discoveryshort}} 服務實例應該列出但卻沒有，請直接從 {{site.data.keyword.cloud_notm}} 儀表板開啟該實例，來產生它的認證。

1.  執行下列其中一個動作，以指出要使用的資料集合：
{: #pick-data-collection}

    - 選擇現有資料集合。

      您可以按一下*在 Discovery 中開啟* 鏈結，來檢閱資料集合的配置，然後再決定要使用哪一個資料集合。

      移至[配置搜尋](#beta-search-skill-add-configure)。

    - 如果您沒有集合，或不想使用列出的任何資料集合，請按一下**建立新的集合**來新增一個集合。遵循[建立資料集合](#beta-search-skill-add-create-discovery-collection)中的程序。

      如果您已達到容許您根據 {{site.data.keyword.discoveryshort}} 服務方案所建立的集合數限制，則不會顯示**建立新的集合**按鈕。如需方案限制詳細資料，請參閱 [{{site.data.keyword.discoveryshort}} 計價方案 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](/docs/services/discovery/discovery-about?topic=discovery-discovery-pricing-plans)。

## 建立 Watson Discovery 服務實例
{: #skill-search-add-create-discovery}

1.  若要建立 {{site.data.keyword.discoveryshort}} 服務實例，請按一下**建立新的集合**。

    即會為您建立 {{site.data.keyword.discoveryshort}} 服務的實例，而且配置頁面會開啟至新的 {{site.data.keyword.discoveryshort}} 服務實例。

    不論您使用哪一個 {{site.data.keyword.conversationshort}} 服務方案，都會在 {{site.data.keyword.Bluemix_notm}} 中佈建該服務的「精簡」方案實例。如果您要建立 {{site.data.keyword.discoveryshort}} 服務實例作為不同方案的一部分，請在這裡停止。請直接從 [{{site.data.keyword.Bluemix_notm}} 型錄 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://cloud.ibm.com/catalog/services/discovery) 建立服務實例，並改為遵循[連接至現有 {{site.data.keyword.discoveryshort}} 服務實例](#skill-search-add-connect-discovery)程序。{: important}

1.  檢閱使用該實例的條款，然後按一下**接受**以繼續進行。

1.  [建立資料集合](#skill-search-add-create-discovery-collection)。

## 建立資料集合
{: #skill-search-add-create-discovery-collection}

請不要嘗試將預先強化的資料來源 *Watson Discovery News* 新增至實例。它不是可以從 {{site.data.keyword.conversationshort}} 搜尋的資料類型。
{: important}

1.  若要建立 {{site.data.keyword.discoveryshort}} 集合，請執行下列其中一項：

      - 若要從儲存在某資料來源類型的資料（其中 {{site.data.keyword.discoveryshort}} 提供內建支援）建立資料集合，請按一下**連接至資料來源**。

        1.  挑選資料來源類型。
        1.  提供您所選擇資料來源的必要資訊，然後按一下**連接**。

            如需詳細資料，請參閱[連接至資料來源 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](/docs/services/discovery?topic=discovery-sources)。
        1.  指出您希望資料來源的資料與您在 {{site.data.keyword.discoveryshort}} 中建立的集合同步化的頻率。
        1.  指定您要從資料來源擷取並包含在 {{site.data.keyword.discoveryshort}} 集合中的資訊。

            顯示的選項會視資料來源類型而有所不同。

            - 如果是 Salesforce 資料來源，請選取要從原始文件擷取的物件類型。您可以選取[案例物件類型 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://developer.salesforce.com/docs/atlas.en-us.object_reference.meta/object_reference/sforce_api_objects_case.htm#!) 來代表*案例*，例如，客戶的議題或問題。
            - 如果是 Sharepoint 資料來源，請指定路徑。
            - 如果是檔案儲存庫，請指定目錄或檔案。

        1.  按一下**儲存並同步資料**。

            即會建立資料集合。在處理程序完成之後，摘要頁面即會顯示在 {{site.data.keyword.discoveryshort}} 工具的個別 Web 瀏覽器標籤中。
        1.  按一下**在 {{site.data.keyword.conversationshort}} 中配置您的技能**，以回到 {{site.data.keyword.conversationshort}} 工具。

      - 若要透過上傳文件來建立集合，請按一下**上傳您自己的資料**。

        1.  首先，您要定義集合，然後上傳文件。請提供下列資訊：

            - 集合名稱。此名稱在此服務實例中必須是唯一的。
            - 配置。您可以選擇使用預設配置範本或已儲存的配置。如需配置的相關資訊，請參閱[配置服務 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](/docs/services/discovery?topic=discovery-configservice)。
            - 語言。選取您要新增至此集合的檔案語言。如需 {{site.data.keyword.discoveryshort}} 所支援語言的相關資訊，請參閱[語言支援 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](/docs/services/discovery?topic=discovery-language-support)。
        1.  上傳文件。

            支援的檔案類型包括 PDF、HTML、JSON 及 DOC 檔案。如需詳細資料，請參閱[新增內容 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](/docs/services/discovery?topic=discovery-addcontent)。{: note}

            已上傳文件未持續進行同步化。如果您想要讓文件變更生效，請上傳文件的最新版本。

等待集合完全汲取後，再回到 {{site.data.keyword.conversationshort}}。

## 配置搜尋
{: #skill-search-add-configure}

1.  從 {{site.data.keyword.conversationshort}} 搜尋技能頁面按中，一下**配置**。

1.  根據搜尋成功與否，撰寫不同的訊息來分享給使用者。

    <table>
    <caption>搜尋結果訊息</caption>
    <tr>
      <th>欄位名稱</th>
      <th>情境</th>
      <th>範例訊息</th>
    </tr>
    <tr>
      <td>訊息</td>
      <td>傳回搜尋結果</td>
      <td>我發現此資訊可能有幫助：</td>
    </tr>
    <tr>
      <td>找不到任何結果</td>
      <td>找不到任何搜尋結果</td>
      <td>我在知識庫中搜尋或許能夠處理您查詢的資訊，但找不到任何可以分享的有用資訊。</td>
    </tr>
    <tr>
      <td>錯誤訊息</td>
      <td>該服務基於某種原因而無法執行搜尋</td>
      <td>我有一些資訊或許可以處理您的查詢，但目前無法搜尋我的知識庫。</td>
    </tr>
    </table>

1.  選擇您要從中擷取文字的 {{site.data.keyword.discoveryshort}} 集合欄位。

    可用的欄位會因您所汲取的資料以及您用來汲取它的配置而有所不同。

    每個搜尋結果可能由下列幾項資訊組成：

    - **標題**：搜尋結果標題。使用集合中的標題、名稱或類似欄位類型作為搜尋結果標題。

      必須選取 `None` 以外的值，Facebook 及 Slack 整合才能顯示回應。
    - **內文**：搜尋結果說明。請使用集合中的摘要、彙總或強調顯示欄位作為搜尋結果內文。

      必須選取 `None` 以外的值，Facebook 及 Slack 整合才能顯示回應。
    - **URL**：原生資料來源中的原始資料物件的超文字鏈結。大部分線上資料來源會為儲存庫中的物件提供自我參照的公用 URL，以支援直接存取。

      產生的 URL 必須有效，讓 Slack 整合可以聯繫上以在回應中包括 URL，以及讓 Facebook 整合可以顯示回應。`None` 是 Facebook 及 Slack 整合可接受的選擇。

    如需協助，請參閱[集合欄位選擇的提示](#skill-search-add-field-tips)。
  
    您必須至少為其中一個選項選擇 `None` 以外的值。

    如果下拉欄位中沒有可用的選項，您可能需要提供 {{site.data.keyword.discoveryshort}} 更多的時間來完成集合的建立。否則，您的集合可能不包含任何文件，或者可能會有一些需要先處理的汲取錯誤。

1.  在預覽窗格中，輸入測試訊息，以查看在搜尋中套用您的配置選擇時所傳回的結果。視需要進行調整。

1.  按一下**建立**。

如果您稍後要變更配置，請重新開啟搜尋技能，然後進行編輯。您不需要在進行變更時儲存變更；系統會自動套用變更。當您滿意搜尋結果時，請按一下**儲存**來完成搜尋技能的配置。

## 後續步驟
{: #skill-search-add-next-steps}

在建立技能之後，它會顯示為「技能」頁面上的磚。

除非將搜尋技能新增至助理並已部署助理，否則此搜尋技能無法與客戶互動。請參閱[建立助理](/docs/services/assistant?topic=assistant-assistant-add)。

當您將對話技能和搜尋技能都鏈結至助理時，如果由對話技能來處理使用者輸入，則會自動觸發搜尋技能，而無法由其任何對話節點來處理。這時會起始一個使用使用者輸入作為其查詢字串的搜尋，而非使用 `anything_else` 節點的一般回應來回覆。

如果您想要，您可以定義特定搜尋查詢，來回應特定節點條件。若要這樣做，請將搜尋回應類型新增至對話節點中。如需詳細資料，請參閱[回應](/docs/services/assistant?topic=assistant-dialog-overview#dialog-overview-multimedia)。

如果您從對話技能起始任何類型的搜尋，請測試對話，以確保依預期觸發搜尋。例如，如果您沒有使用搜尋回應類型，請測試只有在沒有現有對話節點可以處理使用者輸入時才觸發搜尋。同時，每當觸發搜尋時，請確保它傳回有意義的結果。

### 集合欄位選擇的提示
{: #skill-search-add-field-tips}

要從中擷取資料的適當集合欄位，根據集合的資料來源以及用來汲取資料來源的配置而有所不同。若要進一步瞭解集合中的文件結構，包括含有您可能想要擷取之資訊的欄位名稱，請在 {{site.data.keyword.discoveryshort}} 工具中開啟集合，然後按一下**檢視資料綱目**。

下表提供集合欄位，您可以在開始使用時試試這些欄位。這些建議假設您在建立集合時使用預設配置範本。

| 資料來源類型   | 標題 | 內文 | URL |
|--------------------|-------|------|-----|
| 上傳的 PDF 文件 | enriched_text.concepts.text |text          | None |
| Box                | 名稱 | description | listing_url |

集合欄位是在建立集合時所建立的。若要進一步瞭解使用預設配置範本進行汲取時所產生的欄位，例如 `enricheed_text.concepts.text`，請參閱[配置服務 > 新增強化 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](/docs/services/discovery?topic=discovery-configservice#adding-enrichments)。

### 將技能新增至助理
{: #skill-search-add-to-assistant}

您可以將某種技能新增至助理。您必須開啟助理磚，然後將技能從助理配置頁面新增至助理；您無法從技能配置頁面內選擇將使用技能的助理。

多個助理可以使用一個搜尋技能。

1.  從「助理」標籤中，按一下以開啟您要新增其技能的助理磚。

1.  按一下**新增搜尋技能**。

1.  按一下**新增現有技能**。

    從顯示的可用技能中，按一下您要新增的技能。

請至少配置一個測試整合頻道。您無法從「試用」窗格測試搜尋技能。請輸入觸發搜尋的查詢，從整合頻道中測試技能。請確定已適當地觸發搜尋，並傳回相關結果。

可共用的鏈結整合目前不適用於具有搜尋技能的助理。
{: important}
