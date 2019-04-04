---

copyright:
  years: 2015, 2019
lastupdated: "2019-03-01"

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

# 與 Slack 整合
{: #deploy-slack}

Slack 是一種雲端型傳訊應用程式，可協助人員彼此分工合作。
{: shortdesc}

在您配置對話技能並將其新增至助理之後，即可將助理與 Slack 整合。

1.  從「助理」標籤中，按一下以開啟您要部署的助理磚。

1.  從「整合」區段中，按一下**新增整合**。

1.  按一下 *Slack* 的**選取整合**按鈕。

1.  遵循畫面上提供的指示來完成整合處理程序。

如果您想要跟隨其他人逐步進行部署步驟，請觀看此視訊。

<iframe class="embed-responsive-item" id="youtubeplayer" title="逐步進行 Slack 部署步驟" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/RBGBPJ8h4HQ?rel=0" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

持續時間：9.5 分鐘

## 對話考量
{: #deploy-slack-dialog}

您新增至對話的複合式回應會依預期顯示在 Slack 頻道中，但有下列例外：

- **連接至真人服務專員**：忽略此回應類型。

- **影像**：需要影像標題。如果您未提供標題，則不會顯示影像。

- **選項**：此回應類型會顯示使用者可從中選擇的選項清單。

  - 標題是**必要項目**，會顯示在選項清單之前。
  - 無論您是否指定說明，都不會顯示。
  - 在使用者按一下其中一個按鈕之後，按鈕選項即會消失，並且會由使用者選項所產生的使用者輸入取代。如果您在單一回應中包含多個回應類型，請將選項回應類型置於最後。否則，輸出將會混合包含回應和使用者輸入，因而可能混淆使用者。

- **暫停**：此回應類型會暫停助理在 Slack 頻道中的活動。不過，無論您是否選擇顯示鍵入，都不會顯示任何表示助理已暫停的可見指示器。

如需回應類型的相關資訊，請參閱[複合式回應](/docs/services/assistant?topic=assistant-dialog-overview#dialog-overview-multimedia)。

## 與助理會談
{: #deploy-slack-try}

若要開始與助理進行會談，請完成下列步驟：

1.  開啟 Slack，然後移至與您的應用程式相關聯的工作區。
1.  從「應用程式」區段中，按一下您建立的應用程式。
1.  與助理進行會談。

Slack 整合不會處理對話的 Welcome 節點。歡迎訊息不會顯示在 Slack 頻道會談中，好像它是在工具內的「試用」窗格中，或是在「預覽鏈結」整合網頁中。不會從這裡觸發它，因為在使用者所啟動的對話流程中，會跳過含 `welcome` 特殊條件的節點。Slack 會等待使用者起始交談。如果您需要在交談開始時設定環境定義變數的預設值，請不要在 Welcom 節點中設定它們。如需相關資訊，請參閱[啟動對話](/docs/services/assistant?topic=assistant-dialog-start)。
{: note}

在閒置 60 分鐘（「精簡」及「標準」方案則為 5 分鐘）之後，現行階段作業的對話流程會重新啟動。這表示，如果使用者停止與助理互動，則在 60（或 5）分鐘之後，前一次交談期間設定的任何環境定義變數值都會設為空值或設回其預設值。
