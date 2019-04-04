---

copyright:
  years: 2015, 2019
lastupdated: "2019-02-28"

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

# 版本注意事項
{: #release-notes}

## 服務 API 版本化
{: #release-notes-api-version}

API 要求需要版本參數接受 `version=YYYY-MM-DD` 格式的日期。只要我們以舊版相容方式變更 API，就會發行 API 的新次要版本。

每個 API 要求都會傳送版本參數。服務會使用所指定日期的 API 版本，或該日期之前的最新版本。請不要預設為現行日期。改為指定符合應用程式相容版本的日期，而且在您的應用程式備妥可用於較新版本之前，請不要進行變更。

- 第 1 版的現行版本是 `2019-02-28`。
- 第 2 版的現行版本是 `2019-02-28`。
- {{site.data.keyword.conversationshort}} 工具中的「試用」窗格使用的版本是 `2018-07-10`。

## 測試版特性
{: #release-notes-beta}

IBM 發行分類為測試版的評估的服務、特性及語言支援。這些特性可能不穩定、經常變更，且可能接到簡短通知就中斷服務。測試版特性也可能未提供正式發行特性所提供但不要用於正式作業環境的相同層次的效能或相容性。只有 [IBM Developer Answers ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://developer.ibm.com/answers/topics/watson-assistant/){: new_window} 才支援測試版特性。

## 更新的模型
{: #release-notes-updated-models}

{{site.data.keyword.conversationshort}} 演算法可能會根據意見、科學加強功能及其他因素來定期修正及更新，以持續加強效能。這些版本注意事項將會傳遞模型的更新。

您已訓練的現有模型不會立即受到影響，但會將過期模型更新至現行模型（如果您尚未這麼做），在 60 天之後，新的模型就變成可用。

**附註：**這個更新聲明僅適用於「正式發行 (GA)」版語言及特性。

服務提供下列新特性及變更。請參閱[部落格 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://medium.com/ibm-watson/assistant/home)，以尋找有關最新特性如何協助您企業的深入資訊。

## 2019 年 3 月 1 日
{: #1March2019}

- **簡化導覽**：已移除含有個別*建置*、*改善* 和*部署* 標籤的資訊看板導覽。現在，您可以從主要技能頁面取得建置對話技能所需的所有工具。

- **「改善」頁面現在稱為「分析」**：該服務從使用者與您助理之間的交談所產生的參考度量，已從資訊看板的*改善* 標籤移至主要技能頁面上稱為**分析**的新標籤。

## 2019 年 2 月 28 日
{: #28February2019}

- **新的 API 版本**：現行 API 版本現在是 `2019-02-28`。此版本已做了下列變更：

    - 在含有空位的節點中評估條件的順序已變更。先前，如果您有一個容許脫離的含空位節點，則在可以評估任何空位層次的「找不到」條件之前，會先觸發 `anything_else` 根節點。已變更作業順序，以解決此行為。現在，當使用者從含空位的節點脫離時，會處理 `anything_else` 節點除外的所有根節點。接下來，會評估空位層次「找不到」條件。最後，會處理根層次 `anything_else` 節點。若要更充分瞭解含空位節點的完整作業順序，請參閱[空位用法提示](/docs/services/assistant?topic=assistant-dialog-slots#dialog-slots-node-level-handler)。

    - 在訊息的 `context` 或 `output` 物件中開頭為 # 記號的字串，不再被視為目的參照。
  
      先前，這些字串會自動被視為目的。例如，如果您指定環境定義變數，例如 `"color":"#FFFFFF"`，則十六進位顏色代碼 (#FFFFFF) 會被視為目的。該服務會檢查在使用者的輸入中是否偵測到名為 #FFFFFF 的目的，如果沒有，就會將 #FFFFFF 取代為 `false`。這樣的取代不會再發生。
  
      同樣地，如果您在節點回應的字串中包含了 # 記號，以前您必須在它前面使用反斜線 (`\`) 予以跳出。例如，`We are the \#1 seller of lobster rolls in Maine.`。現在您不再需要於文字回應中跳出 `#` 符號。

      這項變更不適用於節點或條件式回應條件。條件中指定的任何以 # 記號開頭的字串，都會繼續被視為目的參照。此外，您可以使用 SpEL 表示式語法，強制系統將訊息的 `context` 或 `output` 物件中的字串視為目的。例如，將目的指定為 `<? #intent-name ?>`。

## 2019 年 2 月 25 日
{: #25February2019}

**Slack 整合加強功能**：您現在可以選擇在 Slack 頻道中觸發助理的事件類型。先前，當您整合助理與 Slack 時，助理會透過直接訊息頻道與使用者互動。現在，您可以配置助理來接聽提及項目，並在其他頻道中提及時給予回應。您可以選擇使用一或兩種事件類型作為機制，讓助理與使用者互動。

## 2019 年 2 月 11 日
{: #11February2019}

**透過 Intercom 整合**：Intercom 是一個先進的客戶服務傳訊平台，與 IBM 合作在團隊中加入新的代理程式：虛擬 Watson Assistant。您可以將助理與 Intercom 應用程式整合，讓應用程式可以順利地在助理與真人支援服務專員之間傳遞使用者交談。此整合僅適用於「加值」和「超值」方案使用者。如需詳細資料，請參閱[透過 Intercom 整合](/docs/services/assistant?topic=assistant-deploy-intercom)。

## 2019 年 2 月 8 日
{: #8February2019}

- **將技能版本化**：您現在可以在開發過程的關鍵點，擷取技能的目的、實體、對話和配置設定的 Snapshot。透過版本化，您可以放心地發揮創意。您可以在測試環境中部署新的設計方法，在將任何更新項目套用至助理的正式作業部署之前進行驗證。如需詳細資料，請參閱[建立技能版本](/docs/services/assistant?topic=assistant-versions)。

- **阿拉伯文內容型錄**：阿拉伯文技能的使用者現在可以將預先建置的目的新增至其對話中。如需相關資訊，請參閱[使用內容型錄](/docs/services/assistant?topic=assistant-catalog)。

## 2019 年 1 月 17 日
{: #17January2019}

- **正式發行捷克文語言支援**：捷克文語言支援不再分類為測試版；它現在已正式發行。如需相關資訊，請參閱[支援的語言](/docs/services/assistant?topic=assistant-language-support)。

- **語言支援改善**：已更新語言瞭解元件，以改善下列特性：

  - 德文與韓文系統實體
  - 阿拉伯文、荷蘭文、法文、義大利文、日文、葡萄牙文及西班牙文的目的分類記號化

## 2019 年 1 月 4 日
{: #4January2019}

- **在華盛頓特區和倫敦位置的 IBM Cloud Functions**：您現在可以從倫敦和華盛頓特區資料中心管理的服務實例的助理對話中，對 IBM Cloud Functions 發出程式化呼叫。請參閱[從對話節點進行程式化呼叫](/docs/services/assistant?topic=assistant-dialog-actions)。

- **使用陣列的新方法**：下列 SpEL 表示式方法可讓您更容易在對話中使用陣列值：

  - **JSONArray.filter**：藉由將陣列中的每個值與可根據使用者輸入而改變的值做比較，來過濾陣列。
  - **JSONArray.includesIntent**：檢查 `intents` 陣列是否包含特定目的。
  - **JSONArray.indexOf**：取得陣列中特定值的索引號碼。
  - **JSONArray.joinToArray**：將格式化套用至從陣列傳回的值。

   如需詳細資料，請參閱[陣列方法文件](/docs/services/assistant?topic=assistant-dialog-methods#dialog-methods-arrays)。

## 2018 年 12 月 13 日
{: #13December2018}

- **倫敦資料中心**：您現在可以在沒有聯合的情況下，建立在倫敦資料中心管理的 {{site.data.keyword.conversationshort}} 服務實例。如需詳細資料，請參閱[資料中心](/docs/services/assistant?topic=assistant-services-information#services-information-regions)。

- **對話節點限制變更**：在新的「標準」方案實例中，對話節點限制暫時從 100,000 變更為 500。此限制變更之後將改回。如果您在此限制生效的時間範圍內建立了「標準」方案實例，您的對話可能會受到影響。此限制會影響 2018 年 12 月 10 日到 12 月 12 日之間建立的技能。將在一月移除所有受影響實例的下限。如果您需要在此之前提高下限，請開立支援問題單。

## 2018 年 12 月 1 日
{: #1December2018}

   若要判定對話技能中的對話節點數目，請執行下列其中一個動作：

   - 如果它尚未與助理相關聯，請從工具中，將對話技能新增至助理，然後從助理的主頁面檢視技能磚。*訓練資料* 區段會列出對話節點的數目。

   - 將 GET 要求傳送至 /dialog_nodes API 端點，並包括 `include_count=true` 參數。例如：

     ```curl
     curl -u "apikey:{apikey}" "https://gateway.watsonplatform.net/assistant/api/v1/workspaces/{workspace_id}/dialog_nodes?version=2018-09-20&include_count=true"
     ```

     在回應中，`pagination` 物件中的 `total` 屬性包含對話節點數目。

     如需如何編輯您要繼續使用之技能的相關資訊，請參閱[疑難排解技能匯入問題](/docs/services/assistant?topic=assistant-skill-add#skill-add-import-errors)。

## 2018 年 11 月 27 日
{: #27November2018}

- **提供新的服務方案「加值」方案**：新方案以低價提供高階特性。與先前的方案不同，「加值」方案是使用者型計費方案。它會根據在給定時段內與助理互動的唯一使用者數目來測量用量。若要充分利用此方案，如果您建置自己的用戶端應用程式，請設計您的應用程式，讓它為每位使用者定義唯一 ID，並在每一個 /message API 呼叫中傳遞此使用者 ID。對於內建的整合，階段作業 ID 可用來識別與助理的使用者互動。如需相關資訊，請參閱[使用者型方案](/docs/services/assistant?topic=assistant-services-information#services-information-user-based-plans)。

  <table>
  <caption>加值方案限制</caption>
    <tr>
      <th>構件</th>
      <th>限制</th>
    </tr>
    <tr>
      <td>助理</td>
      <td>100 </td>
    </tr>
    <tr>
       <td>環境定義實體</td>
       <td>20                </td>
    </tr>
    <tr>
       <td>環境定義實體註釋</td>
       <td>2,000 </td>
    </tr>
    <tr>
       <td>對話節點</td>
       <td>100,000 </td>
    </tr>
    <tr>
       <td>實體</td>
       <td>1,000</td>
    </tr>
    <tr>
       <td>實體同義字</td>
       <td>100,000 </td>
    </tr>
    <tr>
       <td>實體值</td>
       <td>100,000 </td>
    </tr>
    <tr>
       <td>目的</td>
       <td>2,000 </td>
    </tr>
    <tr>
       <td>目的使用者範例</td>
       <td>25,000 </td>
    </tr>
    <tr>
       <td>整合</td>
       <td>100 </td>
    </tr>
    <tr>
       <td>日誌</td>
       <td>30 天</td>
    </tr>
    <tr>
       <td>技能</td>
       <td>50</td>
    </tr>
  </table>

- **使用者型「超值」方案**：「超值」方案現在的計費是根據作用中的唯一使用者數目。如果您選擇使用此方案，請設計您要建置的任何自訂應用程式，適當地識別產生 /message API 呼叫的使用者。如需相關資訊，請參閱[使用者型方案](services-information#user-based-plans)。

  現有「超值」方案服務實例不受這項變更影響；它們繼續使用 API 型計費方法。只有現有的「超值」方案使用者會看到 API 型方案被列為*超值 (API)* 方案選項。
  {: note}

  如需所有可用的服務方案的相關資訊，請參閱{{site.data.keyword.conversationshort}}[服務方案選項 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://www.ibm.com/cloud/watson-assistant/pricing/){: new_window}。

- **目的使用者範例建議 ![僅限「加值」或「超值」方案](images/premium.png)**：您可以上傳包含原始使用者輸入的檔案（例如來自客服中心日誌的使用者查詢），讓服務針對目的使用者範例候選項進行分析及發掘。請參閱[從日誌檔新增範例](intent-recommendations#intent-recommendations-get-example-recommendations)。

## 2018 年 11 月 20 日
{: #20November2018}

**已停止建議**：已移除「改善」標籤上的「建議」區段。「建議」是僅適用於「超值」方案使用者的測試版特性。它建議使用者可採取哪些動作來改善其訓練資料。現在，可以從您進行實際訓練資料變更的工具部分提供建議，而不是在同一個位置合併建議。例如，新增實體同義字時，您現在可以選擇查看該服務所建議的同義術語清單。如果您要尋找其他方法更詳細地分析您的使用者交談日誌，請考慮使用 Jupyter Notebook。如需詳細資料，請參閱[進階作業](/docs/services/assistant?topic=assistant-logs-resources)。

## 2018 年 11 月 9 日
{: #9November2018}

- **主要使用者介面修訂**：{{site.data.keyword.conversationshort}} 服務具有新的外觀及新增特性。

  該版本工具在過去這幾個月內由測試版程式參與者進行了評估。

  - **技能**：您所認為的*工作區* 現在稱為*技能*。*對話技能* 是一個自然語言處理程序訓練資料和構件的容器，這些項目可讓助理瞭解使用者問題並做出回應。

    **我的工作區在哪裡？**您先前建立的任何工作區現在都列在您的服務實例中作為技能。按一下**技能**標籤，即可看到它們。如需相關資訊，請參閱[技能](/docs/services/assistant?topic=assistant-skills)。

  - **助理**：您現在只需兩個步驟就可以發佈技能。將您的技能新增至助理，然後設定一個以上整合來部署您的技能。助理在您的技能上方新增了一層功能，可讓服務編排及管理您的資訊流程。請參閱[助理](/docs/services/assistant?topic=assistant-assistants)。

  - **內建整合**：現在您不用去**部署**標籤部署工作區，而是要將對話技能新增至助理，然後將整合新增至助理，再透過助理將技能提供給使用者。您不需要建置自訂前端系統應用程式，以及管理從某個呼叫到下一個呼叫的交談狀態。不過，如果想要您仍然可以這麼做。如需相關資訊，請參閱[新增整合](/docs/services/assistant?topic=assistant-deploy-integration-add)。

  - **新的主要 API 版本**：已提供第 2 版 API。此版本可讓您存取在執行時期用來與助理互動的方法。在每個 API 呼叫中不再傳遞環境定義；將在助理層為您管理此階段作業狀態。
  
    工具中所呈現的對話技能，實際上是第 1 版工作區的封套。目前第 2 版 API 沒有 API 方法可用來編寫技能和助理。不過，您可以繼續使用第 1 版 API 來編寫工作區。如需詳細資料，請參閱 [API 概觀](/docs/services/assistant?topic=assistant-api-overview)。
    {: note}

  - **切換資料來源**：現在更容易使用來自不同技能的使用者交談日誌來改善某個技能的模型。您不需要根據部署 ID，只要挑選一個已新增及部署技能的助理名稱，即可使用其資料。請參閱[跨助理改善](/docs/services/assistant?topic=assistant-logs#logs-deploy-id)。

  下列視訊提供已更新 {{site.data.keyword.conversationshort}} 工具的 2 分鐘概觀。

  <iframe class="embed-responsive-item" id="youtubeplayer" title="產品概觀" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/OkW7gnHrw30?rel=0" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

  - **倫敦實例的預覽鏈結**：如果您的服務實例是在倫敦管理，則您必須編輯預覽鏈結 URL。此 URL 包含管理實例之地區的區域碼。因為倫敦的實例已聯合至達拉斯，所以您必須將 URL 中的 `eu-gb` 參照取代為 `us-south`，預覽網頁才能適當地呈現。

## 2018 年 11 月 8 日
{: #8November2018}

- **日文資料中心**：您現在可以建立在東京資料中心管理的 {{site.data.keyword.conversationshort}} 服務實例。如需詳細資料，請參閱[資料中心](/docs/services/assistant?topic=assistant-services-information#services-information-regions)。

## 2018 年 10 月 30 日
{: #30October2018}

- **新的 API 鑑別處理程序**：在下列地區，{{site.data.keyword.conversationshort}} 服務已從使用 Cloud Foundry 轉移成使用記號型 Identity and Access Management (IAM) 鑑別：

  - 達拉斯 (us-south)
  - 法蘭克福 (eu-de)

  對於新的服務實例，您可以使用 IAM 來進行鑑別。您可以傳遞載送記號或 API 金鑰。記號支援已鑑別要求，而不需要在每個呼叫中內含服務認證。API 金鑰使用基本鑑別。

  對於所有現有服務實例，您可以繼續使用服務認證 (`{username}:{password}`) 來進行鑑別。

  如需相關資訊，請參閱[鑑別 API 呼叫](/docs/services/assistant?topic=assistant-services-information#services-information-authenticate-api-calls)。

## 2018 年 10 月 25 日
{: #25October2018}

- **實體同義字建議現在推出更多語言版本**：已新增法文、日文及西班牙文等語言的同義字建議支援。

## 2018 年 9 月 26 日
{: #26September2018}

- **{{site.data.keyword.icpfull}} 中已提供 {{site.data.keyword.conversationfull}}**：如需相關資訊，請參閱 [{{site.data.keyword.icpfull}} 文件 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_2.1.0.3/featured_applications/watson_assistant.html)。

## 2018 年 9 月 21 日
{: #21September2018}

- **新的 API 版本**：現行 API 版本現在是 `2018-09-20`。在這個版本中，API 傳回的錯誤物件的 `errors[].path` 屬性是以 [JSON 指標 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://tools.ietf.org/html/rfc6901) 來表示，而不是以帶點表示法形式來表示。
- **Web 動作支援**：您現在可以從對話節點呼叫 {{site.data.keyword.openwhisk_short}} Web 動作。如需詳細資料，請參閱[從對話節點進行程式化呼叫](/docs/services/assistant?topic=assistant-dialog-actions)。

## 2018 年 8 月 15 日
{: #15August2018}

- **實體模糊符合支援的改善**：英文實體完全支援模糊符合，而拼錯特性不再是許多其他語言的僅限測試版特性。如需詳細資料，請參閱[支援的語言](/docs/services/assistant?topic=assistant-language-support)。

## 2018 年 8 月 6 日
{: #6August2018}

- **目的衝突解決 ![僅限「加值」或「超值」方案](images/premium.png)**：此工具現在可協助您解決兩個以上不同目的之使用者範例彼此類似的衝突。不清晰的使用者範例可能削弱訓練資料，讓服務更難以在執行時期將使用者輸入對映至適當的目的。如需詳細資料，請參閱[解決目的衝突](/docs/services/assistant?topic=assistant-intents#intents-resolve-conflicts)。

- **澄清** ![僅限「加值」或「超值」方案](images/premium.png)：啟用「澄清」功能，可讓您的助理需要在兩個以上可行對話節點之間做決定時要求使用者提供協助，以便做出回應。如需詳細資料，請參閱[澄清](/docs/services/assistant?topic=assistant-dialog-runtime#dialog-runtime-disambiguation)。

- **跳至修正**：已修正「對話」工具中的錯誤，該錯誤讓您無法配置跳至來瞄準有 `anything_else` 特殊條件之節點的回應。

- **離題回覆訊息**：您現在可以指定當使用者在離題之後回到節點時所要顯示的文字。使用者應已看過節點的提示。您可以稍微變更訊息，讓使用者知道他們會回到他們當初離開的位置。例如，指定類似這樣的回應：`Where were we? Oh, yes...`。如需詳細資料，請參閱[離題](dialog-runtime#digressions)。

## 2018 年 7 月 12 日
{: #12July2018}

- **複合式回應類型**：您現在可以在對話中新增複合式回應，除了文字之外，您還可以包括影像或按鈕這類元素。如需相關資訊，請參閱[複合式回應](/docs/services/assistant?topic=assistant-dialog-overview#dialog-overview-multimedia)。

- **環境定義實體（測試版）**：環境定義實體是指您藉由標示目的使用者範例中發生的實體類型的提及項目來定義的實體。這些實體類型會教導服務一些重要術語，還有這些重要術語一般出現在使用者話語中的環境定義，讓服務能夠完全根據在使用者輸入中參照的方式來辨識前所未見的實體提及項目。例如，如果您藉由標示 "Boston" 作為 @destination 實體，來註釋目的使用者範例 "I want a flight to Boston"，則該服務可辨識 "Chicago" 為 "I want a flight to Chicago" 這個使用者輸入中的 @destination"。此特性目前僅提供英文版。如需相關資訊，請參閱[新增環境定義實體](/docs/services/assistant?topic=assistant-entities#entities-create-annotation-based)。

  當您使用 Internet Explorer Web 瀏覽器存取工具時，無法在目的使用者範例中標示實體提及項目，也無法編輯使用者範例文字。
  {: note}

- **實體建議**：該服務現在可以為您建議實體值的同義字。建議程式會根據從大量現有資訊內文（包括大量書面文字來源）中所擷取的環境定義相似性來尋找相關的同義字，並使用自然語言處理技術來識別與實體值中的現有同義字類似的單字。如需相關資訊，請參閱[同義字](/docs/services/assistant?topic=assistant-entities#entities-synonyms)。

- **新的 API 版本**：現行 API 版本現在是 `2018-07-10`。此版本引進下列變更：

  - /message `output` 物件的內容從 `text` JSON 物件變更為 `generic` 陣列，其支援多個複合式回應類型，包括 `image`、`option`、`pause` 及 `text`。
  - 已新增環境定義實體的支援。

- **概觀頁面日期過濾器**：使用新的日期過濾器來選擇顯示資料的期間。這些過濾器會影響該頁面上顯示的所有資料：不只是圖形中顯示的交談次數，還會影響與圖形一起顯示的統計資料，以及前幾個目的及實體的清單。如需相關資訊，請參閱[控制項](logs-overview#controls)。

- **型樣限制已擴充**：使用**型樣**欄位來[定義實體值的特定型樣](/docs/services/assistant?topic=assistant-entities#entities-patterns)時，型樣（正規表示式）現在限制為 512 個字元。

## 2018 年 7 月 2 日
{: #2July2018}

- **條件式回應中的跳躍點**：您現在可以配置條件式回應，直接跳至另一個節點。如需詳細資料，請參閱[條件式回應](/docs/services/assistant?topic=assistant-dialog-overview#dialog-overview-multiple)。

## 2018 年 6 月 21 日
{: #21June2018}

- **系統實體的語言更新**：現在已正式發行荷蘭文和簡體中文語言支援。荷蘭文語言支援包括拼字錯誤的模糊符合。繁體中文語言支援包括測試版中[系統實體](/docs/services/assistant?topic=assistant-system-entities)的可用性。如需詳細資料，請參閱[支援的語言](/docs/services/assistant?topic=assistant-language-support)。

## 2018 年 6 月 14 日
{: #14June2018}

- **華盛頓特區資料中心開啟**：您現在可以建立在華盛頓特區資料中心管理的 {{site.data.keyword.conversationshort}} 服務實例。如需詳細資料，請參閱[資料中心](/docs/services/assistant?topic=assistant-services-information#services-information-regions)。

- **新的 API 鑑別處理程序**：對於在下列地區管理的服務實例，{{site.data.keyword.conversationshort}} 服務有新的 API 鑑別處理程序：

  - 華盛頓特區 (us-east)：自 2018 年 6 月 14 日開始
  - 澳洲雪梨 (au-syd)：自 2018 年 5 月 7 日開始

  {{site.data.keyword.cloud_notm}} 已移轉至記號型 Identity and Access Management (IAM) 鑑別。

  對於上列地區的新服務實例，您可以使用 IAM 來進行鑑別。您可以傳遞載送記號或 API 金鑰。記號支援已鑑別要求，而不需要在每個呼叫中內含服務認證。API 金鑰使用基本鑑別。

  對於其他地區所有新的及現有的服務實例，您可以繼續使用服務認證 (`{username}:{password}`) 來進行鑑別。

  使用任何 Watson SDK 時，您可以傳遞 API 金鑰，並讓 SDK 管理記號的生命週期。如需相關資訊和範例，請參閱 API 參考資料中的 [鑑別 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")]https://{DomainName}/apidocs/assistant-v2#authentication){: new_window}。

  如果您不確定要使用的鑑別類型，請按一下 [{{site.data.keyword.Bluemix_notm}} 資源清單 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://cloud.ibm.com/resources){: new_window} 上的服務實例，以檢視服務認證。

## 2018 年 5 月 25 日
{: #25May2018}

- **新的範例工作區**：原本提供讓您探索或作為自己工作區起點的範例工作區已變更。**汽車儀表板**範例已取代為**客戶服務**範例。新範例展現如何使用內容型錄目的和其他較新的特性來建置機器人。它可以回答一般問題，例如有關商店營業時間和位置的查詢，以及說明如何使用含空位的節點來安排店內預約。

- **HTML 呈現已新增至「試用」**：「試用」窗格現在可呈現內含在回應文字中的 HTML 格式。先前，如果您將超文字鏈結作為 HTML 錨點標籤內含在文字回應中，您會在測試期間看到「試用」窗格中有 HTML 原始檔。它以前看起來像這樣：

  `Contact us at <a href="https://www.ibm.com">ibm.com</a>.`

  現在，超文字鏈結的呈現就像在網頁上一樣。它顯示如下：

  `Contact us at` [ibm.com](https://www.ibm.com){: new_window}.

    請記住，您必須在您要對其部署交談的用戶端應用程式的回應中使用適當的語法類型。只有在用戶端應用程式可以正確地解譯 HTML 語法時，才使用此語法。其他整合頻道可能會預期其他格式。

- **部署變更**：已移除**在 Slack 中測試**選項。

## 2018 年 5 月 11 日
{: #11May2018}

- **資訊安全**：此文件中包含有關資料隱私權的一些新的詳細資料。深入閱讀[資訊安全](/docs/services/assistant?topic=assistant-information-security)。

## 2018 年 5 月 7 日
{: #7May2018}

- **澳洲雪梨資料中心開啟**：您現在可以建立在澳洲雪梨資料中心管理的 {{site.data.keyword.conversationshort}} 服務實例。如需詳細資料，請參閱 [IBM Cloud 全球資料中心 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示"](https://www.ibm.com/cloud/data-centers/)。

## 2018 年 4 月 4 日
{: #4April2018}

- **搜尋對話**：您現在可以[搜尋對話節點](/docs/services/assistant?topic=assistant-dialog-build#dialog-build-search)中的指定單字或詞組。

## 2018 年 3 月 15 日
{: #15March2018}

- **{{site.data.keyword.conversationfull}} 簡介**：{{site.data.keyword.ibmwatson}} Conversation 已重新命名。它現在稱為 {{site.data.keyword.conversationfull}}。名稱變更反映該服務擴充的事實，提供預先建置的內容及工具，來協助您更輕鬆地共用您所建置的虛擬助理。如需詳細資料，請閱讀[此部落格文章 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://www.ibm.com/blogs/watson/2018/03/the-future-of-watson-conversation-watson-assistant/)。

- **新的 REST API 和 SDK 可用於 Watson Assistant**：新 API 的功能與目前仍繼續支援的現有 Conversation API 一樣。如需 Watson Assistant API 的相關資訊，請參閱 [API 參考資料 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://{DomainName}/apidocs/assistant){: new_window}。


- **對話加強功能**：下列特性已新增至對話工具：

  - 現在提供了簡式變數名稱及值欄位，可讓您用來新增環境定義變數，或更新環境定義變數值。除非您想要，否則不需要開啟 JSON 編輯器。如需詳細資料，請參閱[定義環境定義變數](/docs/services/assistant?topic=assistant-dialog-runtime#dialog-runtime-context-var-define)。
  - 透過使用資料夾將相關對話節點分組，來組織您的對話。如需詳細資料，請參閱[使用資料夾來組織對話](dialog-build#folders)。
  - 新增了支援，可自訂每個對話節點如何參與使用者從指定的對話流程起始脫離。如需詳細資料，請參閱[離題](dialog-runtime#digressions)。

- **搜尋目的和實體**：已新增搜尋特性，可讓您[搜尋目的](intents#searching-intents)來取得使用者範例、目的名稱或說明，或[搜尋實體](/docs/services/assistant?topic=assistant-entities#entities-search)值和同義字。

- **內容型錄**：新的[內容型錄](/docs/services/assistant?topic=assistant-catalog#catalog-add)包含單一種類的預先建置一般目的和實體，您可以將其新增至應用程式。例如，大部分應用程式需要一般 #greeting-type 目的，由它開始與使用者的對話。您可以從內容型錄予以新增，而不必自己建置。

- **已加強使用者度量**：已加強「改善」元件，有更多的使用者度量和記載統計資料。例如，「概觀」頁面包括數個新的詳細圖形，可彙總使用者與應用程式之間的互動、給定時段的資料流量，以及在使用者交談中最常辨識的目的和實體。

## 2018 年 3 月 12 日
{: #12March2018}

- **新的日期和時間方法**：已新增方法，以更容易從對話執行日期計算。如需詳細資料，請參閱[日期和時間計算](/docs/services/assistant?topic=assistant-dialog-methods#dialog-methods-date-and-time-calculations)。

## 2018 年 2 月 16 日
{: #16February2018}

- **對話節點追蹤**：當您使用「試用」窗格來測試對話時，每一個回應旁會顯示一個位置圖示。您可以按一下圖示，來強調顯示服務穿越對話樹狀結構以到達回應的路徑。如需詳細資料，請參閱[建置對話](/docs/services/assistant?topic=assistant-dialog-build#dialog-build-test)。

- **新建 API 版本**：現行 API 版本現在是 `2018-02-16`。此版本引進下列變更：

  - 大部分的 GET 要求現在都支援新的 `include_audit` 參數。這個選用布林參數指定回應是否應該包含審核內容（`created` 及 `updated` 時間戳記）。預設值為 `false`。（如果您要使用 `2018-02-16` 之前的 API 版本，則預設值為 `true`。）如需相關資訊，請參閱 [API 參考資料 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://{DomainName}/apidocs/assistant){: new_window}。

  - 使用新版本的 API 呼叫回應只包含含非 `null` 值的內容。

  - 訊息回應的 `output.nodes_visited` 及 `output.nodes_visited_details` 內容現在包含具有下列類型（先前予以省略）的節點：

    - 具有 `type`=`response_condition` 的節點
    - 具有 `type`=`event_handler` 及 `event_name`=`input` 的節點

## 2018 年 2 月 9 日
{: #9February2018}

- **荷蘭文系統實體（測試版）**：已加強荷蘭文語言支援，測試版中包含[系統實體](/docs/services/assistant?topic=assistant-system-entities)的可用性。如需詳細資料，請參閱[支援的語言](/docs/services/assistant?topic=assistant-language-support)。

## 2018 年 1 月 29 日
{: #29January2018}

- {{site.data.keyword.conversationshort}} REST API 現在支援新的要求參數：
  - 若更新工作區以指出是否應該將新的工作區資料新增至現有資料，而非取代現有資料，則請使用 `append` 參數。如需相關資訊，請參閱[更新工作區 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://cloud.ibm.com/apidocs/assistant?curl=#update-workspace){: new_window}。
  - 若傳送訊息以指出回應是否應該包含處理訊息期間所造訪之節點的其他診斷相關資訊，請使用 `nodes_visited_details` 參數。如需相關資訊，請參閱[傳送訊息 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://cloud.ibm.com/apidocs/assistant?curl=#get-response-to-user-input){: new_window}。

## 2018 年 1 月 23 日
{: #23January2018}

- **無法擷取工作區清單**：如果您在使用工具時看到此錯誤訊息或類似的錯誤訊息，則可能表示您的階段作業已過期。從**使用者資訊**圖示 ![「使用者資訊」圖示功能表](images/user-icon.png) 中選擇**登出**以進行登出，然後重新登入。

## 2017 年 12 月 8 日
{: #8December2017}

- **跨實例的日誌資料存取（僅限超值使用者）**：如果您是「{{site.data.keyword.conversationshort}} 超值」使用者，則超值實例可以選擇性地配置成容許跨不同的超值實例來存取工作區中的日誌資料。

- **複製節點**：您現在可以複製節點來製作其副本及其子項。如果您所建置的節點具有您要在對話的其他位置重複使用的實用邏輯，則此特性非常實用。如需相關資訊，請參閱[複製對話節點](dialog-build#copy-node)。

- **擷取型樣實體中的群組**：您可以識別在針對實體所定義的正規表示式型樣中的群組。如果您想要稍後可以參照型樣的子區段，則識別群組非常實用。例如，您的實體可能有可擷取美國電話號碼的正規表示式型樣。如果您將號碼型樣的區域碼區段識別為群組，則可以隨後參照該群組，僅存取電話號碼的區域碼區段。如需相關資訊，請參閱[定義實體](/docs/services/assistant?topic=assistant-entities#entities-creating-task)。

## 2017 年 12 月 6 日
{: #6December2017}

- **{{site.data.keyword.openwhisk}} 整合（測試版）**：直接從對話節點呼叫 {{site.data.keyword.openwhisk}}（早期為 IBM OpenWhisk）動作。例如，此特性可讓您在對話節點內呼叫動作來擷取天氣資訊，然後設定對話回應中所傳回資訊的條件。目前，您可以從美國南部地區管理的 {{site.data.keyword.openwhisk_short}} 實例或從美國南部地區管理的多個 {{site.data.keyword.conversationshort}} 實例呼叫動作。如需詳細資料，請參閱[從對話節點進行程式化呼叫](/doc/services/assistant?topic=assistant-dialog-actions)。

## 2017 年 12 月 5 日
{: #5December2017}

- **重新設計的目的及實體使用者介面**：已重新設計 `Intents` 及 `Entities` 標籤，可在建立與編輯實體及目的時提供較簡單且更有效率的工作流程。如需使用這些標籤的相關資訊，請參閱[定義目的](intents#creating-intents)及[定義實體](/docs/services/assistant?topic=assistant-entities#entities-creating-task)。

## 2017 年 11 月 30 日
{: #30November2017}

- **東阿拉伯數字支援**：阿拉伯文系統實體現在支援東阿拉伯數字。

## 2017 年 11 月 29 日
{: #29November2017}

- **改善對跨工作區的使用者輸入的瞭解**：您現在可以改善具有傳送至實例內其他工作區之詞語的工作區。例如，您可能有多個版本的正式作業工作區及開發工作區；您可以使用相同的詞語資料來改善所有這些工作區。請參閱[跨工作區改善](/docs/services/assistant?topic=assistant-logs#logs-deploy-id)。

## 2017 年 11 月 20 日
{: #20November2017}

- **GB18030 法規遵循**：GB18030 是指定用於中文市場之擴充碼頁面的中文標準。此字碼頁標準對於軟體產業而言十分重要，因為 China National Information Technology Standardization Technical Committee 已管制 2001 年 9 月 1 日之後針對中文市場發行的任何軟體應用程式啟用 GB18030。{{site.data.keyword.conversationshort}} 服務支援此編碼，而且符合官方認證的 GB18030 標準。

## 2017 年 11 月 9 日
{: #9November2017}

- **目的範例可以直接參照實體**：您現在可以在目的範例中直接指定實體參照。{{site.data.keyword.conversationshort}} 服務分類器會使用該實體參照及其所有值或同義字，以訓練目的。如需相關資訊，請參閱[目的](/docs/services/assistant?topic=assistant-intents)主題中的[*作為範例的實體*](/docs/services/assistant?topic=assistant-intents#intents-entity-as-example)。

  目前，您只能直接參照您所定義的已關閉實體。您無法直接參照[型樣實體](/docs/services/assistant?topic=assistant-entities#entities-pattern)或[系統實體](/docs/services/assistant?topic=assistant-system-entities)。
  {: note}

## 2017 年 11 月 8 日
{: #8November2017}

- **{{site.data.keyword.conversationshort}} 連接器**：您可以使用新的 {{site.data.keyword.conversationshort}} 連接器工具將工作區連接至您所擁有的 Slack 或 Facebook Messenger 應用程式，讓它成為 Slack 或 Facebook Messenger 使用者可與之互動的聊天機器人。此工具僅適用於 {{site.data.keyword.Bluemix_notm}} 美國南部地區。

## 2017 年 11 月 3 日
{: #3November2017}

- **對話更新**：下列更新讓您更容易建置對話（如需詳細資料，請參閱[建置對話](/docs/services/assistant?topic=assistant-dialog-build)）。

    - 您可以將條件新增至空位，讓它只有在符合特定條件時才是必要項目。例如，只有在詢問婚姻狀態的前一個（必要）空位指出使用者已婚時，您才能讓詢問配偶姓名的空位成為必要項目。

    - 您現在可以選擇**跳過使用者輸入**作為節點的下一步。當您選擇此選項時，在處理現行節點之後，服務會直接跳至現行節點的第一個子節點。此選項類似現有的*跳至* 下一步選項，但它更具彈性。您不需要指定要跳至的確切節點。在執行時期，服務一律會跳至為第一個子節點的節點，即使在定義下一步行為之後重新排序子節點或新增節點。

    - 您可以新增空位的條件式回應。對於「找到」及「找不到」回應，您可以自訂根據是否符合特定條件的服務回應方式。此特性可讓您檢查可能的錯誤解譯，並先更正它們，再將使用者所提供的值儲存至空位的環境定義變數。例如，如果空位儲存使用者的年齡，並在*檢查* 欄位中使用 `@sys-number` 進行擷取，則您可以新增條件以檢查數目是否超過 100，並回應 *Please provide a valid age in years.* 這類陳述。如需詳細資料，請參閱[新增「找到」及「找不到」回應的條件](/docs/services/assistant?topic=assistant-dialog-slots#dialog-slots-handler-next-steps)。

    - 已重新設計您用來將條件式回應新增至節點的介面，可更輕鬆地列出每一個條件及其回應。若要新增節點層次條件式回應，請按一下**自訂**，然後啟用**多個回應**選項。

     **多個回應**切換開關僅針對節點層次回應，將此特性設為開啟或關閉。它未控制定義空位之條件式回應的能力。空位多個回應設定是分別進行控制。
     {: note}

    - 為了將您編輯空位的頁面保持簡單，您現在可以選取下列功能表選項：a.) 新增必須符合才能處理空位的條件，以及 b.) 新增空位之「找到」及「找不到」條件的條件式回應。除非您選擇新增此額外功能，否則不會顯示空位條件及多個回應欄位，這樣可以簡化頁面且容易使用。

## 2017 年 10 月 25 日
{: #25October2017}

- **簡體中文更新**：已加強簡體中文的語言支援。這包含使用字元層次字組內嵌的目的分類增進功能，以及系統實體的可用性。請注意，在此加強功能中可能已更新 {{site.data.keyword.conversationshort}} 服務學習模型，而且在重新訓練模型時，將會套用所有變更；如需相關資訊，請參閱[更新的模型](#release-notes-updated-models)。
- **西班牙文更新** - 已改善西班牙文的極大型資料集的目的分類。

## 2017 年 10 月 11 日
{: #11October2017}

- **韓文更新**：已加強韓文的語言支援。請注意，在此加強功能中可能已更新 {{site.data.keyword.conversationshort}} 服務學習模型，而且在重新訓練模型時，將會套用所有變更；如需相關資訊，請參閱[更新的模型](#release-notes-updated-models)。

## 2017 年 10 月 3 日
{: #3October2017}

- **型樣定義的實體（測試版）**：您現在可以使用正規表示式來定義實體的特定型樣。這可協助您識別遵循已定義型樣的實體，例如 SKU 或產品編號、電話號碼或電子郵件位址。如需其他詳細資料，請參閱[型樣定義的實體](/docs/services/assistant?topic=assistant-entities#entities-pattern)。
    - 您可以新增單一實體值的同義字或型樣，但不能同時新增兩者。
    - 每一個實體值最多都可以有 5 種型樣。
    - 每一種型樣（正規表示式）的長度限制為 128 個字元。
    - 透過 CSV 檔案的匯入或匯出作業目前不支援型樣。
    - REST API 不支援直接存取型樣，但您可以使用 `/values` 端點來擷取或修改型樣。

- **依字典過濾的模糊符合（僅限英文）**：針對英文，目前提供適用於實體的已改良模糊符合版本。這項改良可防止將一些常用的有效英文單字擷取為給定實體的模糊相符項。例如，模糊符合不會比對實體值 `like` 與 `hike` 或 `bike`（其為有效的英文單字），但會繼續比對 `lkie` 或 `oike` 這類範例。

## 2017 年 9 月 27 日
{: #27September2017}

- **條件建置器更新**：為協助您在對話節點中定義條件而顯示的控制項已更新。在您輸入 $ 開始新增環境定義變數之後，加強功能支援列出可用的環境定義變數名稱。

## 2017 年 8 月 31 日
{: #31August2017}

- **改善區段回復**：從「改善」區段的「概觀」頁面，暫時移除中間交談時間度量值及對應的過濾器。這項移除會防止計算某些度量值，從而導致中間交談時間度量值及一段時間交談圖形顯示不正確的資訊。IBM 很遺憾會從工具中移除功能，但致力於確保我們向使用者傳達準確的資訊。
- **對話節點名稱**：您現在可以將任何名稱指派給對話節點；它不需要是唯一的。而且，隨後可以變更此節點名稱，而不影響節點在內部的參照方式。您指定的名稱會儲存為工作區 JSON 檔案中節點的 title 屬性，而且系統會使用 name 屬性中所儲存的唯一 ID 來參照節點。

## 2017 年 8 月 23 日
{: #23August2017}

- **韓文、日文及義大利文更新**：已加強韓文、日文及義大利文的語言支援。請注意，在此加強功能中可能已更新 {{site.data.keyword.conversationshort}} 服務學習模型，而且在重新訓練模型時，將會套用所有變更；如需相關資訊，請參閱[更新的模型](#release-notes-updated-models)。

## 2017 年 8 月 10 日
{: #10August2017}

- **重音符號正規化**：在交談式設定中，與 {{site.data.keyword.conversationshort}} 服務互動時，使用者不一定會使用重音符號。因此，已對演算法進行更新，在進行目的偵測及實體識別時，會將重音及無重音版本的單字視為相同。

  不過，針對部分語言（例如西班牙文），有些重音符號可能會變更實體的意義。因此，進行實體偵測時，雖然原始實體可能隱含地具有重音符號，但是服務也可能符合相同實體的非重音版本，但具有略低的信任評分。

  例如，針對具有重音符號並對應於動詞 `barrer`（清理）過去式的 `barrió` 這個字，服務也可能符合 `barrio`（鄰近地區）這個字，但具有略低的信任。

  系統將提供具有完全相符項之實體中的最高信任評分。例如，如果 `barrió` 位於訓練集中，則偵測不到 `barrio`；而如果 `barrio` 位於訓練集中，則偵測不到 `barrió`。

  您應該使用適當的字元及重音符號來訓練系統。例如，如果您預期 `barrió` 作為回應，則應該將 `barrió` 放入訓練集中。

  雖然不是重音符號，但是同樣適用於使用西班牙文字母 `ñ` 與字母 `n` 的這類單字，例如 `uña` 與 `una`。在此情況下，字母 `ñ` 不只是具有重音符號的 `n`，還是唯一的西班牙文特有字母。

  如果您認為客戶不會使用適當的重音符號或錯字（例如 including，放置 `n`，而非 `ñ`），則可以啟用模糊符合，或者，您可以在訓練範例中明確包含它們。

  **附註：**針對葡萄牙文、西班牙文、法文及捷克文，會啟用重音符號正規化。

- **工作區拒絕旗標**：{{site.data.keyword.conversationshort}} REST API 現在支援工作區的拒絕旗標。此旗標指出 IBM 不會使用工作區訓練資料（例如目的及實體）來進行一般服務改善。如需相關資訊，請參閱 [API 參考資料 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://cloud.ibm.com/apidocs/assistant?curl=#data-collection){: new_window}

## 2017 年 8 月 7 日
{: #7August2017}

- **`Next` 及 `last` 日期解譯**：{{site.data.keyword.conversationshort}} 服務會將 `last` 及 `next` 日期視為參照最接近的前一天或隔天（可能在同一週或前一週）。如需相關資訊，請參閱[系統實體](/docs/services/assistant?topic=assistant-system-entities#system-entities-sys-datetime)主題。

## 2017 年 8 月 3 日
{: #3August2017}

- **其他語言的模糊符合（測試版）**：實體的模糊符合現在適用於其他語言（如[支援的語言](/docs/services/assistant?topic=assistant-language-support)主題中所述）。
- **部分符合（測試版 - 僅限英文）**：模糊符合現在會自動建議使用者定義實體中的子字串型同義字，並指派較低的信任評分（與確切的實體符合相較之下）。如需詳細資料，請參閱[模糊符合](/docs/services/assistant?topic=assistant-entities#entities-fuzzy-matching)。

## 2017 年 7 月 28 日
{: #28July2017}

- 設定工具的雙向喜好設定時，現在可以指定圖形使用者介面方向。
- 已更新工具的色系，讓它與其他 Watson 服務及產品保持一致。

## 2017 年 7 月 19 日
{: #19July2017}

- {{site.data.keyword.conversationshort}} REST API 現在支援存取對話節點。如需相關資訊，請參閱 [API 參考資料 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://cloud.ibm.com/apidocs/assistant?curl=#list-dialog-nodes){: new_window}。

## 2017 年 7 月 14 日
{: #14July2017}

- 已加強對話的空位功能。例如，已新增 *slot_in_focus* 內容，可用來定義僅套用至單一空位的條件。如需詳細資料，請參閱[使用空位收集資訊](/docs/services/assistant?topic=assistant-dialog-slots)。

## 2017 年 7 月 12 日
{: #12July2017}

- **捷克文支援**：已引進捷克文語言支援；如需其他詳細資料，請參閱[支援的語言](/docs/services/assistant?topic=assistant-language-support)主題。

## 2017 年 7 月 11 日
{: #11July2017}

- **在 Slack 中測試**：您可以使用新的**在 Slack 中測試**工具，將您的工作區快速部署為 Slack 機器人使用者，以進行測試。此工具僅適用於 {{site.data.keyword.Bluemix_notm}} 美國南部地區。
- **阿拉伯文更新**：已加強阿拉伯文語言支援，可包含每個目的的絕對評分，而且可以將目的標示為不相關；如需其他詳細資料，請參閱[支援的語言](/docs/services/assistant?topic=assistant-language-support)主題。請注意，在此加強功能中可能已更新 {{site.data.keyword.conversationshort}} 服務學習模型，而且在重新訓練模型時，將會套用所有變更；如需相關資訊，請參閱[更新的模型](#release-notes-updated-models)。

## 2017 年 6 月 23 日
{: #23June2017}

- **韓文更新**：已加強韓文語言支援；如需其他詳細資料，請參閱[支援的語言](/docs/services/assistant?topic=assistant-language-support)主題。請注意，在此加強功能中可能已更新 {{site.data.keyword.conversationshort}} 服務學習模型，而且在重新訓練模型時，將會套用所有變更；如需相關資訊，請參閱[更新的模型](#release-notes-updated-models)。

## 2017 年 6 月 22 日
{: #22June2017}

- **引進空位**：現在藉由新增空位，即可更輕鬆地向單一節點的使用者收集多個資訊片段。先前，您必須建立數個對話節點，才能涵蓋使用者提供該資訊的所有可能方式組合。使用空位，即可配置用於儲存使用者所提供之任何資訊的單一節點，並提示輸入使用者未提供的任何必要詳細資料。如需詳細資料，請參閱[使用空位收集資訊](/docs/services/assistant?topic=assistant-dialog-slots)。
- **簡化的對話樹狀結構** - 已重新設計對話樹狀結構來改善其可用性。樹狀結構視圖比較精簡，因此較容易查看您在其中的位置。而且，節點間之鏈結的呈現方式，更容易瞭解節點之間的關係。

## 2017 年 6 月 21 日
{: #21June2017}

- **阿拉伯文支援**：阿拉伯文語言支援現在已正式發行。如需詳細資料，請參閱[配置雙向語言](/docs/services/assistant?topic=assistant-language-support#language-support-configuring-bi-directional)。
- **語言更新**：{{site.data.keyword.conversationshort}} 服務演算法已更新，用來改善整體語言支援。如需詳細資料，請參閱[支援的語言](/docs/services/assistant?topic=assistant-language-support)主題。

## 2017 年 6 月 16 日
{: #16June2017}

- **建議（測試版 - 僅限超值使用者）**：「改善」畫面也包含**建議**頁面用來建議改善系統的方式，方法是分析使用者與聊天機器人的交談，並考慮系統的現行訓練資料及回應確定性。

## 2017 年 6 月 14 日
{: #14June2017}

- **其他語言的模糊符合（測試版）**：實體的模糊符合現在適用於其他語言（如[支援的語言](/docs/services/assistant?topic=assistant-language-support)主題中所述）。您可以開啟每個實體的模糊符合，以改善服務辨識使用者輸入中的術語的能力，而其使用的語法與實體類似，不需要完全相符。儘管存在拼字錯誤或些微的語法差異，但是此特性還是可以將使用者輸入對映至適當的對應實體。例如，如果您將 giraffe 定義為 animal 實體的同義字，而且使用者輸入包含術語 giraffes 或 girafe，則模糊符合可以將術語正確地對映至 animal 實體。如需詳細資料，請參閱[模糊符合](/docs/services/assistant?topic=assistant-entities#entities-fuzzy-matching)。

## 2017 年 6 月 13 日
{: #13June2017}

- **使用者交談**：「改善」畫面現在包含**使用者交談**頁面，此頁面提供與聊天機器人之間可依關鍵字、目的、實體或天數過濾的使用者互動清單。您可以開啟個別交談來更正目的，或新增實體值或同義字。
- **正規表示式變更**：SpEL 函數（例如 find、matches、extract、replaceFirst、replaceAll 及 split）所支援的正規表示式已變更。不再容許一組正規表示式建構（包括 look-ahead、look-behind、possessive repetition 及 backreference 建構）。這是必要的變更，可避免原始正規表示式程式庫中的安全漏洞。

## 2017 年 6 月 12 日
{: #12June2017}

- 您可以使用**精簡**方案（先前命名為「免費」方案）建立的工作區數目上限已從 3 個變更為 5 個。
- 您現在可以將任何名稱指派給對話節點；它不需要是唯一的。而且，隨後可以變更此節點名稱，而不影響節點在內部的參照方式。您指定的名稱被視為別名，系統會使用它自己的內部 ID 來參照節點。
- 在藉由編輯工作區詳細資料來建立工作區之後，即無法再變更工作區的語言。如果您需要變更語言，可以將工作區匯出為 JSON 檔案，更新 language 內容，然後將 JSON 檔案匯入為新的工作區。

## 2017 年 6 月 6 日
{: #6June2017}

- **學習**：有新的*瞭解 {{site.data.keyword.conversationfull}}* 頁面，提供開始使用資訊及服務文件和其他有用資源的鏈結。若要開啟此頁面，請按一下頁面標頭的 ![i 表示資訊。](images/info.png) 圖示。
- **大量匯出及刪除**：您現在可以同時將若干目的或實體匯出至 CSV 檔案，然後針對另一個 {{site.data.keyword.conversationshort}} 應用程式匯入及重複使用它們。您也可以同時選取要大量刪除的多個實體或目的。
- **韓文更新**：韓文記號器已更新，用來處理非正式語言支援。IBM 會持續改善實體識別及分類。
- **表情符號支援**：現在會正確地分類/擷取新增至目的範例或新增為實體值的表情符號。

  只有您訓練資料中內含的表情符號才能正確且一致地識別；表情符號支援可能無法將具有不同色調或其他變異的類似表情符號正確分類。
  {: note}

- **實體詞幹分析（測試版 - 僅限英文）**：模糊符合測試版特性可辨識實體，並根據實體值的詞幹形式進行比對。例如，此特性正確地將 'bananas' 辨識為與 'banana' 類似，並將 'run' 辨識為與 'running' 類似，因為它們共用一般詞幹形式。如需相關資訊，請參閱[模糊符合](/docs/services/assistant?topic=assistant-entities#entities-fuzzy-matching)。
- **工作區匯入進度**：在從 JSON 檔案匯入工作區時，會立即顯示工作區磚，其中會顯示匯入進度的相關資訊。
- **減少訓練時間**：現在會平行訓練多個模型，可明顯減少大型工作區的訓練時間。

## 2017 年 5 月 26 日
{: #26May2017}

- 現行 API 版本現在是 `2017-05-26`。此版本引進下列變更：
    - 已變更 ErrorResponse 物件的綱目。此變更會影響所有端點及方法。如需相關資訊，請參閱 [API 參考資料 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://{DomainName}/apidocs/assistant){: new_window}。
    - 已變更用來代表已匯出工作區 JSON 中對話節點的內部綱目。如果您使用 `2017-05-26` API 來匯入使用舊版本匯出的工作區，則可能無法正確地匯入部分對話節點。為獲得最佳結果，請一律使用用來匯出工作區的相同版本來匯入工作區。

## 2017 年 5 月 25 日
{: #25May2017}

- 您現在可以在「試用」窗格中管理環境定義變數。按一下**管理環境定義**鏈結即可開啟新的窗格，讓您在測試對話時可於其中設定及檢查環境定義變數的值。如需相關資訊，請參閱[測試對話](/docs/services/assistant?topic=assistant-dialog-build#dialog-build-test)。

## 2017 年 5 月 16 日
{: #16May2017}

- 現在，在開啟工具時，會提供一個 **Car Dashboard** 範例工作區。若要使用此範例作為您自己的工作區的起始點，請編輯工作區。如果您要將它用於多個工作區，請改為複製它。除非使用此範例工作區，否則此範例工作區不會計入訂閱工作區總計。
- 現在，導覽工具更為簡單。導覽功能表選項位於主頁面的側邊，而非頂端。在頁面頂端，會顯示用於顯示您所在位置的「瀏覽途徑」鏈結。您現在可以從「工作區」頁面切換不同的服務實例。若要快速到達該處，請按一下導覽功能表中的**回到工作區**。如果您有多個服務實例，則會顯示現行實例的名稱。您可以按一下它旁邊的**變更**鏈結，來選擇另一個實例。
- 現在，在建立對話時，會在其中新增兩個節點：1) **Welcome** 節點，位在對話樹狀結構頂端，包含向使用者顯示的問候語，2) **Anything else** 節點，位在樹狀結構底端，捕捉對話中其他節點未辨識的任何使用者查詢並予以回應。如需詳細資料，請參閱[建立對話](/docs/services/assistant?topic=assistant-dialog-build)。
- 在「試用」窗格中測試對話時，現在可以按向上鍵循環瀏覽先前的輸入，以尋找及重新提交最近的測試詞語。
- 現在提供 5 個系統實體（`@sys-date`、`@sys-time`、`@sys-currency`、`@sys-number`、`@sys-percentage`）的實驗性韓文語言支援。部分數值實體有一些已知問題，而且非正式語言輸入的支援有限。
- 「概觀」頁面可以從「改善」標籤中取得。此頁面提供與機器人互動的摘要。您可以檢視給定時段的資料流量，以及使用者交談中最常辨識到的目的及實體。如需相關資訊，請參閱[使用概觀頁面](/docs/services/assistant?topic=assistant-logs-overview)。

## 2017 年 4 月 27 日
{: #27April2017}

- 現在，僅針對英文，提供下列系統實體作為測試版特性：
    - sys-location：辨識使用者詞語中位置（例如鄉鎮、城市及國家/地區）的參照。
    - sys-person：辨識使用者詞語中人員姓名（姓氏及名字）的參照。

    如需相關資訊，請參閱[系統實體參照](/docs/services/assistant?topic=assistant-system-entities)。
- 實體的模糊符合是測試版特性（現在以英文提供）。您可以開啟每個實體的模糊符合，以改善服務辨識使用者輸入中的術語的能力，而其使用的語法與實體類似，不需要完全相符。儘管存在拼字錯誤或些微的語法差異，但是此特性還是可以將使用者輸入對映至適當的對應實體。例如，如果您將 **giraffe** 定義為 animal 實體的同義字，而且使用者輸入包含術語 *giraffes* 或 *girafe*，則模糊符合可以將術語正確地對映至 animal 實體。如需詳細資料，請參閱[定義實體](/docs/services/assistant?topic=assistant-entities#entities-fuzzy-matching)，並搜尋`模糊符合`。

## 2017 年 4 月 18 日
{: #18April2017}

- {{site.data.keyword.conversationshort}} REST API 現在支援存取下列資源：
    - 實體
    - 實體值
    - 實體值同義字
    - 日誌

    如需相關資訊，請參閱 [API 參考資料 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://{DomainName}/apidocs/assistant){: new_window}。
- /messages `POST` 方法的行為已變更如何處理指定為訊息輸入一部分的實體及目的：
    - 如果您在輸入時指定目的，則服務會使用您指定的目的，但會使用自然語言處理程序來偵測使用者輸入中的實體。
    - 如果您在輸入時指定實體，則服務會使用您指定的實體，但會使用自然語言處理程序來偵測使用者輸入中的目的。

        針對同時指定目的及實體的訊息，或針對未指定任一項的訊息，此行為未變更。
- 將使用者輸入標示為不相關的選項現在適用於所有支援的語言。這是測試版特性。
- 新的「認證」標籤提供單一位置，您可在其中尋找將應用程式連接至工作區所需的所有資訊（例如服務認證及工作區 ID）以及其他部署選項。若要存取工作區的「認證」標籤，請按一下 ![功能表](images/Menu_16.png) 圖示，然後選取**認證**。

## 2017 年 3 月 9 日
{: #9March2017}

{{site.data.keyword.conversationshort}} REST API 現在支援存取下列資源：

- 工作區
- 目的
- 範例
- 反例

如需相關資訊，請參閱 [API 參考資料 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://{DomainName}/apidocs/assistant){: new_window}。

## 2017 年 3 月 7 日
{: #7March2017}

- 使用 `.` 或 `..` 作為目的名稱會導致問題，不再予以支援。

    您無法重新命名或刪除具有此名稱的目的；若要變更名稱，請將您的目的匯出至檔案，並重新命名檔案中的目的，然後將更新的檔案匯入至工作區。

    付費客戶可以聯絡支援人員來瞭解資料庫變更。

## 2017 年 3 月 1 日
{: #1March2017}

- 在德文中，現在已啟用系統實體。

## 2017 年 2 月 22 日
{: #22February2017}

- 訊息現在限制為 2,048 個字元。

## 2017 年 2 月 3 日
{: #3February2017}

- 我們已變更目的的評分方式，並且新增將輸入標示為與應用程式不相關的能力。如需詳細資料，請參閱[定義目的](/docs/services/assistant?topic=assistant-intents)，並搜尋`標示為不相關`。

- 此版本對工作區進行了重大變更。為了從這些變更中獲益，您必須手動升級工作區。

- 已變更**跳至**動作的處理，可防止在特定情況下可能發生的迴圈。先前，如果您跳至某個節點的條件，而且該節點及其任何對等節點都沒有評估為 true 的條件，則系統會跳至根層次節點，並尋找其條件符合輸入的節點。在某些情況下，此處理已建立迴圈，因而導致對話無法進行。

  在新的處理程序下，如果目標節點及其對等節點都未評估為 true，則會結束對話。若要重新實作舊模型，請新增具有 `true` 條件的最終對等節點。在回應中，使用**跳至**動作，將目標設為對話樹狀結構中根層次之第一個節點的條件。

## 2017 年 1 月 11 日
{: #11January2017}

- 在此版本中，您可以在對話中自訂節點標題。

## 2016 年 12 月 22 日
{: #22December2016}

- 在此版本中，對話節點會顯示 `node title` 的新區段。無法使用自訂 `node title` 的能力。收合時，`node title` 會顯示對話節點的 `node condition`。如果沒有 `node condition`，則會將 "Untitled Node" 顯示為標題。

## 2016 年 12 月 19 日
{: #19December2016}

以下幾項變更讓對話編輯器更容易使用，且以更直覺的方式進行：

- 較大的編輯視圖可讓您更輕鬆地在處理節點時，檢視節點的所有詳細資料。
- 節點可以包含多個回應，每一個回應都是由個別條件觸發。如需相關資訊，請參閱[多個回應](/docs/services/assistant?topic=assistant-dialog-overview#dialog-overview-responses)。

## 2016 年 12 月 5 日
{: #5December2016}

- 支援新的語言，而且全部都是實驗性模式：德文、繁體中文、簡體中文及荷蘭文。
- 提供兩個新的系統實體：@sys-date 及 @sys-time。如需詳細資料，請參閱[系統實體](/docs/services/assistant?topic=assistant-system-entities)。

## 2016 年 10 月 21 日
{: #21October2016}

- {{site.data.keyword.conversationshort}} 服務現在提供系統實體，這些系統實體是可以跨任何使用案例使用的一般實體。如需詳細資料，請參閱[定義實體](/docs/services/assistant?topic=assistant-entities)，並搜尋`啟用系統實體`。
- 您現在可以在「改善」頁面上檢視與使用者的交談歷程。您可以使用此項目來瞭解機器人的行為。如需詳細資料，請參閱[改善技能](/docs/services/assistant?topic=assistant-logs-intro)。
- 您現在可以從逗點區隔值 (CSV) 檔案匯入實體，這在具有大量實體時會提供協助。如需詳細資料，請參閱[定義實體](/docs/services/assistant?topic=assistant-entities)，並搜尋`匯入實體`。

## 2016 年 9 月 20 日
{: #20September2016}

**新版本**：2016-09-20

若要充分運用新版本中的變更，請將 `version` 參數的值變更為新的日期。如果您尚未備妥要更新至此版本，請不要變更您的版本日期。

- version **2016-09-20**：`dialog_stack` 已從字串陣列變更為 JSON 物件陣列。

## 2016 年 8 月 29 日
{: #29August2016}

- 您可以將對話節點從某個分支移至另一個分支，作為同層級或對等節點。如需詳細資料，請參閱[移動對話節點](/docs/services/assistant?topic=assistant-dialog-build#dialog-build-move-node)。
- 您可以展開 JSON 編輯器視窗。
- 您可以檢視機器人交談的聊天日誌，以協助您瞭解其行為。您可以依目的、實體、日期及時間進行過濾。如需詳細資料，請參閱[改善技能](/docs/services/assistant?topic=assistant-logs-intro)

## 2016 年 7 月 11 日
{: #21July2016}

- 此「通用版」版本可讓您使用實體及對話來建立充分發揮作用的機器人。

## 2016 年 5 月 18 日
{: #18May2016}

- 此「實驗性」版本的 {{site.data.keyword.conversationshort}} 引進使用者介面，可讓您使用工作區、目的及範例。
