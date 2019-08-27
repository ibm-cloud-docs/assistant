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

# 與 Facebook Messenger 整合
{: #deploy-facebook}

Facebook Messenger 是一種行動傳訊應用程式，可協助企業及客戶直接彼此通訊。
{: shortdesc}

在您配置對話技能並將其新增至助理之後，即可將助理與 Facebook Messenger 整合。

目前沒有任何機制可用來識別透過 Facebook Messenger 與助理互動的使用者，這表示無法識別或刪除與特定使用者相關聯的資料。請不要將此整合方法用於必須符合 GDPR 標準的部署。如需詳細資料，請參閱[資訊安全](/docs/services/assistant?topic=assistant-information-security)。
{: important}

1.  從「助理」標籤中，按一下以開啟您要部署的助理磚。

1.  從「整合」區段中，按一下**新增整合**。

1.  按一下 **Facebook Messenger**。

1.  遵循畫面上提供的指示來完成整合處理程序。

如果您想要跟隨其他人逐步進行部署步驟，請觀看此視訊。

<iframe class="embed-responsive-item" id="youtubeplayer" title="逐步進行 Facebook 部署步驟" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/8o-FFU5sYNM?rel=0" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

持續時間：8 分鐘

## 對話考量
{: #deploy-facebook-dialog}

您新增至對話的複合式回應會依預期顯示在 Facebook 應用程式中，但有下列例外：

- **連接至真人服務專員**：忽略此回應類型。

- **影像**：此回應類型會將影像內嵌在回應中。無論您是否指定標題及說明，它們都不會顯示在影像之前。

- **選項**：此回應類型會顯示使用者可從中選擇的選項清單。

  - 標題是**必要項目**，會顯示在選項清單之前。
  - 無論您是否指定說明，都不會顯示。
  - 在使用者按一下其中一個按鈕之後，按鈕選項即會消失，並且會由使用者選項所產生的使用者輸入取代。如果助理或使用者輸入新的輸入，則按鈕產生的輸入會消失。因此，如果您在單一回應中包含多個回應類型，請將選項回應類型置於最後。否則，來自後續回應的內容（例如，來自文字回應類型的文字）會取代按鈕產生的文字。

- **暫停**：此回應類型會暫停助理在 Messenger 中的活動。不過，除非在暫停之後觸發了另一個回應類型，否則活動不會在暫停之後繼續。每當您包含此回應類型時，就會新增另一個不同的回應類型（例如，文字回應），並將它定位在此類型之後。

如需回應類型的相關資訊，請參閱[複合式回應](/docs/services/assistant?topic=assistant-dialog-overview#dialog-overview-multimedia)。

## 與助理會談
{: #deploy-facebook-try}

若要開始與助理進行會談，請完成下列步驟：

1.  開啟 Facebook Messenger。
1.  鍵入您先前建立的頁面名稱。
1.  在頁面出現之後，請按一下該頁面，然後開始與助理進行會談。

Facebook Messenger 整合不會處理對話的 Welcome 節點。歡迎訊息不會顯示在 Facebook 會談中，好像它是在「試用」窗格中，或是在「預覽鏈結」整合網頁中。不會從這裡觸發它，因為在使用者所啟動的對話流程中，會跳過含 `welcome` 特殊條件的節點。Facebook Messenger 會等待使用者起始交談。如果您需要在交談開始時設定環境定義變數的預設值，請不要在 Welcom 節點中設定它們。如需相關資訊，請參閱[啟動對話](/docs/services/assistant?topic=assistant-dialog-start)。
{: note}

在閒置 60 分鐘（「精簡」及「標準」方案則為 5 分鐘）之後，現行階段作業的對話流程會重新啟動。這表示，如果使用者停止與助理互動，則在 60（或 5）分鐘之後，前一次交談期間設定的任何環境定義變數值都會設為空值或設回其預設值。
