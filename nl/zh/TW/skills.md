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

# 技能
{: #skills}

對話技能包含可讓助理協助您客戶的訓練資料和邏輯。
{: shortdesc}

對話技能包含下列類型的構件：

- [**目的**](/docs/services/assistant?topic=assistant-intents)：*目的* 代表使用者輸入的用途，例如關於公司位置或帳單付款的問題。您可以為您想要應用程式支援的每一種使用者要求類型定義一個目的。在工具中，目的名稱前面一律會加上 `#` 字元。若要訓練對話技能以辨識您的目的，請提供許多使用者輸入範例，並指出它們所對映的目的。

  提供了*內容型錄*，其包含預先建置的一般目的，您可以將其新增至應用程式，而不必自行建置。例如，大部分應用程式都需要一個能開始與使用者對話的問候語目的。您可以新增**一般**內容型錄，以新增一個問候使用者及執行其他有用動作（例如結束交談）的目的。

- [**對話**](/docs/services/assistant?topic=assistant-dialog-build)：*對話* 是一個分支交談流程，定義應用程式在辨識出已定義的目的及實體時如何回應。您可以使用此工具中的對話編輯器來建立與使用者的交談，並根據您在其輸入中辨識的目的及實體來提供回應。

  ![僅限使用目的和對話的基本實作圖表](images/basic-impl.png)

若要讓對話技能處理更細微的問題，請定義實體並從您的對話參照它們。

- [**實體**](/docs/services/assistant?topic=assistant-entities)：*實體* 代表與目的相關並且為目的提供特定環境定義的術語或物件。例如，實體可能代表使用者要尋找公司位置的城市，或帳單付款金額。在工具中，實體名稱前面一律會加上 `@` 字元。

  您可以藉由提供實體術語值和同義字、實體型樣，或識別實體通常用於句子的環境定義，來訓練技能辨識您的實體。若要微調您的對話，請返回並新增節點，以檢查使用者輸入中除了目的之外還有哪些實體提及項目。

![使用目的、實體和對話的更複雜實作圖表。](images/complex-impl.png)

當您新增資訊時，該技能會使用此唯一資料來建置機器學習模型，以辨識這些和類似的使用者輸入。每當您新增或變更訓練資料時，即會觸發訓練處理程序，以確保當客戶需求及其想要討論的主題變更時，基礎模型能持續保持最新狀態。

如需協助建立對話技能，請參閱[建立技能](/docs/services/assistant?topic=assistant-skill-add)。
