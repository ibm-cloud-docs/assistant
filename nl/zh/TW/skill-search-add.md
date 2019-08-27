---

copyright:
  years: 2015, 2019
lastupdated: "2019-08-02"

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

# 建立搜尋技能 ![僅限加值或超值方案](images/plus.png)
{: #skill-search-add}

助理使用*搜尋技能* 將複雜的客戶查詢遞送至 {{site.data.keyword.discoveryfull}} 服務。{{site.data.keyword.discoveryshort}} 將使用者輸入視為搜尋查詢。它會尋找與外部資料來源的查詢相關的資訊，並將它傳回給助理。
{: shortdesc}

此特性僅適用於「加值」或「超值」方案使用者。
{: note}

將搜尋技能新增至助理，這樣助理就不必說類似下面的內容：`I'm sorry. I can't help you with that`。助理可以改為查詢現有公司文件或資料，以瞭解是否可以找到任何有用資訊並與客戶共用。

![在預覽鏈結整合中顯示搜尋結果](images/search-skill-preview-link.png)

下列 4 分鐘的視訊提供搜尋技能概觀。

<iframe class="embed-responsive-item" id="youtubeplayer" title="搜尋技能概觀" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/ZcgGf8J2Cfw?rel=0" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

若要進一步瞭解搜尋技能對您的企業有何助益，請[閱讀本部落格文章 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://medium.com/ibm-watson/adding-search-to-watson-assistant-99e4e81839e5){: new_window}。

## 運作方式
{: #skill-search-add-how}

搜尋技能會從您使用 {{site.data.keyword.discoveryshort}} 服務所建立的資料集合中搜尋資訊。

{{site.data.keyword.discoveryshort}} 是用來搜索、轉換及正規化非結構化資料的一項服務。該產品會套用資料分析及認知直覺，來強化您的資料，讓您稍後更容易找到並擷取有意義的資訊。若要進一步瞭解 {{site.data.keyword.discoveryshort}}，請參閱[產品說明文件 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](/docs/services/discovery?topic=discovery-about){: new_window}。

一般而言，新增至 {{site.data.keyword.discoveryshort}} 並從助理存取的資料集合的類型包含您公司擁有的資訊。這些專有資訊可能包括常見問題、銷售宣傳材料、技術手冊或主題專家所撰寫的文章。發掘這種密集專有資訊集合，可快速找到客戶問題的回答。

下圖說明當對話技能和搜尋技能同時新增給助理時，使用者輸入的處理方式。

![圖表，顯示對話如何回答某些使用者輸入以及如何透過搜尋回答其他問題。](images/search-skill-diagram.png)

## 開始之前
{: #skill-search-add-prereqs}

如果您沒有 {{site.data.keyword.discoveryshort}} 服務實例，則在此處理程序期間將為您佈建免費精簡方案實例。如果您具有現有的 {{site.data.keyword.discoveryshort}} 服務實例，請連接至該服務實例；在此處理程序期間，不會要求您建立新的實例。

如果您先建立 Discovery 實例，則請不要將名為 *Watson Discovery News* 的預先強化的資料來源新增至實例。它不是可以從 {{site.data.keyword.conversationshort}} 搜尋的資料類型。
{: tip}

## 建立搜尋技能
{: #skill-search-add-task}

1.  按一下**技能**標籤，然後按一下**建立技能**。

1.  按一下*搜尋技能* 磚，然後按**下一步**。

    只有在您是加值或超值方案使用者時，才能選取「搜尋技能」。
    {: note}

1.  指定新技能的詳細資料：
    - **名稱**：名稱長度不得超過 100 個字元。名稱是必要項目。
    - **說明**：選用說明長度不得超過 200 個字元。

1.  按一下**繼續**。

其餘的步驟會根據您是否有權存取已建立集合的現有 {{site.data.keyword.discoveryshort}} 服務實例而有所不同。請根據您的狀況，遵循適當的程序：

- [連接至現有 Watson Discovery 實例](#skill-search-add-connect-discovery)
- [建立 Watson Discovery 實例](#skill-search-add-create-discovery)

## 連接至現有 Watson Discovery 服務實例
{: #skill-search-add-connect-discovery}

1.  選擇您要從中擷取資訊的 {{site.data.keyword.discoveryshort}} 服務實例。
{: #choose-d-instance}

    清單中即會顯示您有權存取的任何 {{site.data.keyword.discoveryshort}} 服務實例。

    如果您看到一則警告指出您有些 {{site.data.keyword.discoveryshort}} 服務實例還沒有設定認證，這表示您可以存取您從未直接在 {{site.data.keyword.cloud_notm}} 儀表板中開啟過的至少一個實例。您必須存取服務實例，才能為它建立認證。而且，在 {{site.data.keyword.conversationshort}} 可以代表您建立與 {{site.data.keyword.discoveryshort}} 服務實例的連線之前，認證必須存在。如果您認為清單中遺漏某個 {{site.data.keyword.discoveryshort}} 服務實例，請直接從 {{site.data.keyword.cloud}} 儀表板開啟該實例，來產生它的認證。
    {: note}

1.  執行下列其中一個動作，以指出要使用的資料集合：
{: #pick-data-collection}

    - 選擇現有資料集合。

      您可以按一下*在 Discovery 中開啟* 鏈結，來檢閱資料集合的配置，然後再決定要使用哪一個資料集合。

      移至[配置搜尋](#search-skill-add-configure)。

    - 如果您沒有集合，或不想使用列出的任何資料集合，請按一下**建立新的集合**來新增一個集合。遵循[建立資料集合](#search-skill-add-create-discovery-collection)中的程序。

      如果您已達到容許您根據 {{site.data.keyword.discoveryshort}} 服務方案所建立的集合數限制，則不會顯示**建立新的集合**按鈕。如需方案限制詳細資料，請參閱 [{{site.data.keyword.discoveryshort}} 計價方案 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](/docs/services/discovery/discovery-about?topic=discovery-discovery-pricing-plans){: new_window}。
      {: note}

## 建立 Watson Discovery 服務實例
{: #skill-search-add-create-discovery}

1.  若要建立 {{site.data.keyword.discoveryshort}} 服務實例，請按一下**建立新的集合**。

    如果您沒有現有的 {{site.data.keyword.discoveryshort}} 服務實例，則會為您建立免費的 {{site.data.keyword.discoveryshort}} 服務實例。

    不論您具有哪種類型的 {{site.data.keyword.conversationshort}} 服務方案，都會在 {{site.data.keyword.Bluemix_notm}} 中佈建該服務的精簡方案實例。
    {: note}

1.  檢閱使用該實例的條款，然後按一下**接受**以繼續進行。

1.  [建立資料集合](#skill-search-add-create-discovery-collection)。

## 建立資料集合
{: #skill-search-add-create-discovery-collection}

如果您有 Discovery 服務精簡方案，則有機會升級方案。如果現在不想升級，請按一下**開始使用**。

1.  若要建立 {{site.data.keyword.discoveryshort}} 集合，請執行下列其中一項：

      - 若要從儲存在某資料來源類型的資料（其中 {{site.data.keyword.discoveryshort}} 提供內建支援）建立資料集合，請挑選資料來源類型。

        1.  提供您所選擇資料來源的必要資訊，然後按一下**連接**。

            如需詳細資料，請參閱[連接至資料來源 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](/docs/services/discovery?topic=discovery-sources){: new_window}。
        1.  指出您希望資料來源的資料與您在 {{site.data.keyword.discoveryshort}} 中建立的集合同步化的頻率。
        1.  指定您要從資料來源擷取並包含在 {{site.data.keyword.discoveryshort}} 集合中的資訊。

            顯示的選項會視資料來源類型而有所不同。

            - 如果是 Salesforce 資料來源，請選取要從原始文件擷取的物件類型。您可以選取[案例物件類型 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://developer.salesforce.com/docs/atlas.en-us.object_reference.meta/object_reference/sforce_api_objects_case.htm#!) 來代表*案例*，例如，客戶的議題或問題。
            - 如果是 Sharepoint 資料來源，請指定路徑。
            - 如果是檔案儲存庫，請指定目錄或檔案。
            - 對於 Web 搜索資料來源，請指定要搜索的網站的基本 URL。將搜索指定的網頁及其鏈結到的任何頁面，並且每個網頁都會建立一份文件。

            給 Watson 幾分鐘的時間來開始建立文件。開始汲取來源後，{{site.data.keyword.discoveryshort}} 詳細資料頁面上顯示的文件數就會增加。您可能需要重新整理頁面。
            
            若要取得建立資料來源的協助，請參閱[疑難排解](#skill-search-add-troubleshoot)。

        1.  按一下**儲存並同步物件**。

            即會建立資料集合。在處理程序完成之後，摘要頁面即會顯示在 {{site.data.keyword.discoveryshort}} 的個別 Web 瀏覽器標籤中。

      - 若要藉由上傳文件來建立集合，請按一下**上傳文件**。

        1.  首先，您要定義集合，然後上傳文件。請提供下列資訊：

            - 集合名稱。此名稱在此服務實例中必須是唯一的。
            - 語言。選取要新增到此集合的檔案的語言。如需 {{site.data.keyword.discoveryshort}} 所支援語言的相關資訊，請參閱[語言支援 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](/docs/services/discovery?topic=discovery-language-support){: new_window}。

              如果您要上傳 PDF 文件且想要從中擷取參與方、本質和種類資訊，請展開**進階**區段，然後按一下**搭配使用預設合約配置與此集合**。如需詳細資料，請參閱[集合需求 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](/docs/services/discovery?topic=discovery-element-classification#element-collection){: new_window}。
        1.  上傳文件。

            支援的檔案類型包括 PDF、HTML、JSON 及 DOC 檔案。如需詳細資料，請參閱[新增內容 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](/docs/services/discovery?topic=discovery-addcontent){: new_window}。
            {: note}

            已上傳文件未持續進行同步化。如果您要讓文件變更生效，請上傳文件的最新版本。

等待集合完全汲取後，再回到 {{site.data.keyword.conversationshort}}。

### 資料集合建立範例
{: #skill-search-add-json-collection-example}

例如，您可能具有與下面類似的 JSON 檔案：

```bash
{
  "Title": "About",
  "Shortdesc": "IBM Watson Assistant is a cognitive bot that you can customize for your business needs, and deploy across multiple channels to bring help to your customers where and when they need it.",
  "Topics": "overview",
  "url": "https://cloud.ibm.com/docs/services/assistant?topic=assistant-index"
}
```
{: codeblock}

如果上傳的 JSON 檔案包含重複名稱值，則搜尋只會檢索及傳回第一次出現的名稱和值配對。請將檔案分為多個 JSON 檔案，然後上傳該集。
{: tip}

## 配置搜尋
{: #skill-search-add-configure}

1.  在 {{site.data.keyword.discoveryshort}} 實例中，按一下**在 Watson Assistant 中完成設定**。

1.  在 {{site.data.keyword.conversationshort}} 搜尋技能頁面中，按一下**配置**。

1.  選擇要從中擷取文字的 {{site.data.keyword.discoveryshort}} 集合欄位，擷取的文字將包含在傳回給使用者的搜尋結果中。

    可用的欄位會因您所汲取的資料而有所不同。

    每個搜尋結果可能由下列各區段組成：

    - **標題**：搜尋結果標題。使用集合中的標題、名稱或類似欄位類型作為搜尋結果標題。

      必須為標題選取某些內容，否則在 Facebook 和 Slack 整合中不會顯示搜尋結果回應。
    - **內文**：搜尋結果說明。請使用集合中的摘要、彙總或強調顯示欄位作為搜尋結果內文。

       您必須為內文選取某些內容，否則在 Facebook 和 Slack 整合中不會顯示搜尋結果回應。
    - **URL**：可以將您要包含在搜尋結果結尾的任何標底內容移入此欄位。

       例如，建議您包括原生資料來源中原始資料物件的超文字鏈結。大部分線上資料來源會為儲存庫中的物件提供自我參照的公用 URL，以支援直接存取。如果您新增 URL，該 URL 必須有效且可以聯繫。否則，Slack 整合不會在其回應中包含該 URL，並且 Facebook 整合不會傳回任何回應。

       URL 欄位是空的時，Facebook 和 Slack 整合可以順利顯示搜尋結果回應。
  
    您必須至少為其中一個搜尋結果區段選擇值。
    {: important}

    如需協助，請參閱[集合欄位選擇的提示](#skill-search-add-field-tips)。

    如果下拉欄位中沒有可用的選項，請提供 {{site.data.keyword.discoveryshort}} 更多的時間來完成集合的建立。等待一段時間後，如果仍未建立集合，則您的集合可能不包含任何文件，或者可能會有一些需要先處理的汲取錯誤。

    若要繼續使用[上傳的 JSON 檔案範例](#skill-search-add-json-collection-example)，對映最好使用 *Title*、*Shortdesc* 和 *url* 欄位。

    ![顯示已選取 Title、Shortdesc 和 url 欄位，並已使用這些欄位中的資訊移入預覽搜尋卡](images/search-skill-configure-fields.png)

    新增欄位對映後，將顯示搜尋結果的預覽，其中包含資料集合的對應欄位中的資訊。此預覽顯示傳回給使用者的搜尋結果回應中包含的內容。

    若要取得配置搜尋的協助，請參閱[疑難排解](#skill-search-add-troubleshoot)。

1.  根據搜尋成功與否，撰寫不同的訊息，以與使用者共用。

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
      <td>由於某種原因，無法完成搜尋</td>
      <td>我有一些資訊或許可以處理您的查詢，但目前無法搜尋我的知識庫。</td>
    </tr>
    </table>

1.  按一下**試用**，以開啟「試用」窗格進行測試。輸入測試訊息，以查看在搜尋中套用您的配置選擇時所傳回的結果。視需要進行調整。

1.  按一下**建立**。

如果您稍後要變更搜尋結果卡的配置，請重新開啟搜尋技能，然後進行編輯。您不需要在進行變更時儲存變更；系統會自動套用變更。當您滿意搜尋結果時，請按一下**儲存**來完成搜尋技能的配置。

如果您決定要連接至其他 {{site.data.keyword.discoveryshort}} 服務實例或資料集合，請建立新的搜尋技能，並將其配置為連接至其他實例。**無法**在建立搜尋技能後為其變更服務實例或資料集合詳細資料。
{: important}

### 集合欄位選擇的提示
{: #skill-search-add-field-tips}

要從中擷取資料的適當集合欄位，根據集合的資料來源以及資料來源的強化方式。選擇資料集合類型後，集合欄位值會預先移入來源欄位，這些來源欄位被視為最有可能包含給定集合資料來源類型的有用資訊。不過，您比任何人都更瞭解您的資料。因此，您可以將來源欄位變更為包含能符合您需求的最佳資訊的欄位。

若要進一步瞭解集合中的文件結構，包括含有您可能想要擷取之資訊的欄位名稱，請在 {{site.data.keyword.discoveryshort}} 中開啟集合，然後按一下「檢視資料綱目」圖示 ![「檢視資料綱目」圖示](images/icon-view-data-schema.png)。

來源欄位是在建立集合時所建立。若要進一步瞭解所產生的欄位（例如 `enriched_text.concepts.text`），請參閱[配置服務 > 新增強化 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](/docs/services/discovery?topic=discovery-configservice#adding-enrichments){: new_window}。

## 疑難排解
{: #skill-search-add-troubleshoot}

請檢閱此資訊，以取得執行一般作業的協助。

- **建立 Web 搜索資料集合**：建立 Web 搜索資料來源時要瞭解的事項：

    - 對於 {{site.data.keyword.discoveryshort}} 精簡方案，建立的文件數不能超過 1,000 個。 
    - 若要增加可用於資料集合的文件數，請按一下「新增 URL 群組」，在其中會列出要搜索但並未從初始種子 URL 鏈結到的頁面的 URL。
    - 若要減少可用於資料集合的文件數，請指定基本 URL 的子網域。或者，在 Web 搜索設定中，限制 Watson 可以從原始頁面進行的躍點數目。您也可以指定要從搜索中明確排除的子網域。
    - 如果等待幾分鐘並重新整理頁面後仍未列出任何文件，請確定可以從 URL 的頁面來源中取得要汲取的內容。某些網頁內容是動態產生的，因此無法進行搜索。

- **為上傳的文件配置搜尋結果**：如果您使用已上傳文件的集合，並且無法取得正確的搜尋結果或結果不夠明確，請考慮在建立資料集合時使用*智慧型文件瞭解*。 

  此特性可讓您根據文字格式以對文件進行註釋。例如，您可以教導 {{site.data.keyword.discoveryshort}} 學習任何 28 點粗體字型的文字都是文件標題。如果在汲取時將此資訊套用至集合，則稍後可以使用 *title* 欄位作為搜尋結果的標題區段的來源。 
  
  您也可以使用「智慧型文件瞭解」，以將大型文件分割成更易於搜尋的區段。如需相關資訊，請參閱 {{site.data.keyword.discoveryshort}} 文件中的[智慧型文件瞭解 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](/docs/services/discovery?topic=discovery-sdu) 主題。

- **改善搜尋結果**：如果您對所看到的結果不滿意，請檢查下列資訊以取得協助。

  - 從對話節點呼叫搜尋技能，並指定過濾器詳細資料。 

    在對話節點搜尋技能回應中，可以指定完整的 {{site.data.keyword.discoveryshort}} 查詢語法過濾器以協助縮小結果範圍。 
    
    例如，您可以定義過濾器，用於過濾出資料集合中未在文件標題或其他某個 meta 資料欄位中提及目的的任何文件。或者，過濾器可以過濾出未將某個實體識別為資料集合 meta 資料中的已知實體的文件，或過濾出未在文件全文中的任何位置提及該實體的文件。如需如何新增搜尋技能回應類型的詳細資料，請參閱[新增複合式回應](https://cloud.ibm.com/docs/services/assistant?topic=assistant-dialog-overview#dialog-overview-multimedia-add)。

    如需改善結果的相關提示，請閱讀[從 Watson Discovery 改善自然語言查詢結果 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://developer.ibm.com/blogs/improving-your-natural-language-query-results-from-watson-discovery/) 部落格文章。

## 後續步驟
{: #skill-search-add-next-steps}

在建立技能之後，它會顯示為「技能」頁面上的磚。

除非將搜尋技能新增至助理並已部署助理，否則此搜尋技能無法與客戶互動。請參閱[建立助理](/docs/services/assistant?topic=assistant-assistant-add)。

### 將技能新增至助理
{: #skill-search-add-to-assistant}

您可以將某種技能新增至助理。開啟助理磚，並在其中將技能新增至助理。您無法從技能配置頁面內選擇將使用技能的助理。

多個助理可以使用一個搜尋技能。

1.  從「助理」標籤中，按一下以開啟您要新增其技能的助理磚。

1.  按一下**新增搜尋技能**。

1.  按一下**新增現有技能**。

    從顯示的可用技能中，按一下您要新增的技能。

將搜尋技能新增至助理之後，將自動為該助理啟用該技能，如下所示：

- 如果助理只有搜尋技能，則提交給助理的其中一個整合頻道的任何使用者輸入都會觸發搜尋技能。

- 如果助理同時具有對話技能及搜尋技能，則任何使用者輸入都會先觸發對話技能。對話會處理自己對於可以正確進行回答有高信賴度的任何使用者輸入。通常會觸發對話樹狀結構中 `anything_else` 節點的任何查詢都會改為傳送到搜尋技能。

  您可以避免從 `anything_else` 節點觸發搜尋，方法是遵循[停用搜尋](#search-skill-add-disable)中的步驟。
  {: note}

- 如果您要對特定問題觸發特定搜尋查詢，請將搜尋技能回應類型新增至適當的對話節點。如需詳細資料，請參閱[回應](/docs/services/assistant?topic=assistant-dialog-overview#dialog-overview-multimedia)。

## 搜尋觸發程式
{: #skill-search-add-trigger}

搜尋技能是使用下列方式來觸發：

- **任何其他節點**：沒有任何對話節點可以處理使用者查詢時，會搜尋外部資料來源來尋找相關答案。

  助理不會顯示標準訊息，例如 `I don't know how to help you with that.`，而是說，`Maybe this information can help:`，後面會接著搜尋所傳回的段落。如果搜尋技能鏈結至您的助理，則每當觸發 `anything_else` 節點時，不會顯示節點回應，而是改為發生搜尋。助理會將使用者輸入當作查詢傳遞至搜尋技能，並傳回搜尋結果作為回應。

  您可以避免從 `anything_else` 節點觸發搜尋，方法是遵循[停用搜尋](#search-skill-add-disable)中的步驟。
  {: note}

- **搜尋回應類型**：如果您將搜尋回應類型新增至對話節點，則助理會從外部資料來源擷取段落，並傳回它作為特定問題的回應。只有在處理個別對話節點時，才會進行此類型的搜尋。

  如果您要在觸發搜尋之前縮小使用者查詢，則此方法非常實用。例如，對話分支可能會收集客戶想要購買之裝置類型的相關資訊。若您知道廠牌和機型，您就可以在查詢中傳送機型關鍵字，該查詢將提交至搜尋技能，而獲得更適當的結果。
- **僅搜尋技能**：如果只有搜尋技能鏈結至助理，而沒有任何對話技能鏈結至助理，則當從助理的其中一個整合頻道收到任何使用者輸入時，會將搜尋查詢傳送至 {{site.data.keyword.discoveryshort}} 服務。

## 測試搜尋技能
{: #search-skill-add-test}

配置搜尋後，您可以使用搜尋技能的「試用」窗格來傳送測試查詢，以查看從 {{site.data.keyword.discoveryshort}} 傳回的搜尋結果。

若要測試客戶提問並由對話回答或觸發搜尋時的完整體驗，請使用頻道整合，例如預覽鏈結。

您無法從對話「試用」窗格測試完整端對端使用者體驗。搜尋技能是個別配置且連接至助理。對話技能無法瞭解搜尋的詳細資料，因此無法在其「試用」窗格中顯示搜尋結果。
{: important}

配置至少一個整合頻道來測試搜尋技能。在頻道中，輸入用於觸發搜尋的查詢。如果您從對話起始任何類型的搜尋，請測試對話，以確定依預期觸發搜尋。如果您未使用搜尋回應類型，請測試只有在沒有現有對話節點可以處理使用者輸入時才觸發搜尋。同時，每當觸發搜尋時，請確保它傳回有意義的結果。

## 將更多要求傳送至搜尋技能
{: #search-skill-add-increase-flow}

如果您要降低對話技能的回應頻率，並改為將更多查詢傳送給搜尋技能，則可以配置對話。

您必須將對話技能及搜尋技能新增至助理，此方法才有效。

請遵循此程序，藉由將信賴度層次臨界值從預設設定 0.2 重設為 0.5，以降低對話的回應可能性。將信賴度層次臨界值變更為 0.5，即指示助理不要使用對話中的回答進行回應，除非助理確信對話可以理解使用者目的且可以進行處理的信賴度層次超過 50%。

1.  在對話技能的*對話* 頁面中，請確定對話樹狀結構中的最後一個節點具有 `anything_else` 條件。

    只要處理此節點時，都會觸發搜尋技能。

1.  將一個資料夾新增至對話。使該資料夾位於要取消強調的第一個對話節點上方。將下列條件新增至資料夾：

    `intents[0].confidence > 0.5`

    此條件會套用至資料夾中的所有節點。只有在助理確信自己理解使用者目的的信賴度至少為 50% 時，此條件才會指示助理處理該資料夾中的節點。

1.  將不要助理經常處理的任何對話節點都移入資料夾中。

變更對話後，測試助理以確定搜尋技能的觸發頻率與您所要的觸發頻率相符。

另一種方法是教導對話學習要忽略的主題。若要這樣做，您可以將想要助理立即傳送到搜尋技能的話語新增為對話技能的「試用」窗格中的測試話語。然後，您可以選取「試用」窗格中的**標示為不相關**選項，以教導對話不要回應此話語或與它類似的其他話語。如需相關資訊，請參閱[教導助理學習要忽略的主題](/docs/services/assistant?topic=assistant-logs#logs-mark-irrelevant)。

## 停用搜尋
{: #search-skill-add-disable}

您可以停用觸發搜尋技能。

設定整合時，建議您暫時停用觸發搜索技能。或者，建議您僅針對可以在對話中識別到的特定使用者查詢觸發搜尋，並使用搜尋技能回應類型進行回答。

若要防止觸發搜尋技能，請完成下列步驟：

1.  在**助理**頁面中，按一下助理的功能表，然後選擇**設定**。
1.  開啟*搜尋技能*頁面，然後按一下以將切換開關切換為**已停用**。
