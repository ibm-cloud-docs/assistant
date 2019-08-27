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

# 技能
{: #skills}

技能是人工智慧的容器，可讓助理協助您的客戶。
{: shortdesc}

助理會指示要求沿著最佳路徑來解決客戶問題。新增技能，讓助理能夠直接回答常見問題，或者針對更複雜的問題，參照更一般化的搜尋結果。

## 技能類型
{: #skills-types}

您可以將下列類型的技能新增至助理：

- **[對話技能](#skills-dialog-skill)**：瞭解來自使用者的一般問題或要求，並遵循您使用 Script 所編寫的對話來回答問題或滿足要求。

![僅限加值或超值方案](images/plus.png) 如果您是加值或超值方案使用者，則也可以建立下列類型的技能：

- **[搜尋技能](#skills-search-skill)**：藉由在外部資料來源中搜尋相關資訊、擷取段落，並將其作為助理的回應傳回，以回答使用者的問題。

### 對話技能
{: #skills-dialog-skill}

對話技能包含可讓助理協助您客戶的訓練資料和邏輯。
對話技能可包含下列類型的構件：

- [**目的**](/docs/services/assistant?topic=assistant-intents)：*目的* 代表使用者輸入的用途，例如關於公司位置或帳單付款的問題。您可以為您想要應用程式支援的每一種使用者要求類型定義一個目的。目的名稱前面一律會加上 `#` 字元。若要訓練對話技能以辨識您的目的，請提供許多使用者輸入範例，並指出它們所對映的目的。

  提供了*內容型錄*，其包含預先建置的一般目的，您可以將其新增至應用程式，而不必自行建置。例如，大部分應用程式都需要一個能開始與使用者對話的問候語目的。您可以新增**一般**內容型錄，以新增一個問候使用者及執行其他有用動作（例如結束交談）的目的。

- [**對話**](/docs/services/assistant?topic=assistant-dialog-build)：*對話* 是一個分支交談流程，定義應用程式在辨識出已定義的目的及實體時如何回應。您可以使用對話編輯器來建立與使用者的交談，並根據您在其輸入中辨識的目的及實體來提供回應。

  ![僅限使用目的和對話的基本實作圖表](images/basic-impl.png)

若要讓對話技能處理更細微的問題，請定義實體並從您的對話參照它們。

- [**實體**](/docs/services/assistant?topic=assistant-entities)：*實體* 代表與目的相關並且為目的提供特定環境定義的術語或物件。例如，實體可能代表使用者要尋找公司位置的城市，或帳單付款金額。實體名稱前面一律會加上 `@` 字元。

  您可以藉由提供實體術語值和同義字、實體型樣，或識別實體通常用於句子的環境定義，來訓練技能辨識您的實體。若要微調您的對話，請返回並新增節點，以檢查使用者輸入中除了目的之外還有哪些實體提及項目。

![使用目的、實體和對話的更複雜實作圖表。](images/complex-impl.png)

當您新增資訊時，該技能會使用此唯一資料來建置機器學習模型，以辨識這些和類似的使用者輸入。每當您新增或變更訓練資料時，即會觸發訓練處理程序，以確保當客戶需求及其想要討論的主題變更時，基礎模型能持續保持最新狀態。

如需協助建立對話技能，請參閱[建立對話技能](/docs/services/assistant?topic=assistant-skill-dialog-add)。

### 搜尋技能 ![僅限「加值」或「超值」方案](images/plus.png)
{: #skills-search-skill}

Watson Assistant 沒有針對問題的明確解決方案時，會將使用者問題遞送至搜尋技能，以在不同的自助服務內容來源中尋找回答。搜尋技能會與 {{site.data.keyword.discoveryfull}} 服務互動，以從配置的資料集合中擷取此資訊。

如果您已使用 {{site.data.keyword.discoveryshort}} 服務，則可以發掘可與客戶共用的來源資料的現有資料集合，以處理客戶的問題。

不過，您不需要具有 {{site.data.keyword.discoveryshort}} 服務實例。如果選擇建立搜尋技能，則會為您佈建免費的 {{site.data.keyword.discoveryshort}} 實例。您接著可以從資料來源來建立集合，並配置搜尋技能以搜尋此集合來尋找針對客戶查詢的回答。

下圖說明當對話技能和搜尋技能同時新增給助理時，使用者輸入的處理方式。不是對話設計為回答的問題的任何問題都會傳送到搜尋技能，搜尋技能會在 {{site.data.keyword.discoveryshort}} 資料集合中尋找相關回應。

![如何將問題遞送至搜尋技能的圖表。](images/search-skill-diagram.png)

如需協助建立搜尋技能，請參閱[建立搜尋技能](/docs/services/assistant?topic=assistant-skill-search-add)。
