---

copyright:
  years: 2015, 2018
lastupdated: "2018-01-26"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:swift: .ph data-hd-programlang='swift'}

# 關於

使用 {{site.data.keyword.conversationfull}} 服務，您可以建置瞭解自然語言輸入的解決方案，並使用機器學習來模擬人類之間的交談方式以對客戶進行回應。
{: shortdesc}

## 運作方式

此圖顯示完整解決方案的整體架構：![服務的流程圖](images/conversation_arch_overview.png)

- 使用者會透過您實作的使用者**介面**與應用程式互動。例如，簡單聊天視窗或行動應用程式，甚至是具有語音介面的機械裝置。

- **應用程式**會將使用者輸入傳送至 {{site.data.keyword.conversationshort}} 服務。
    - 應用程式會連接至*工作區*，這是對話流程及訓練資料的容器。
    - 服務會解譯使用者輸入、指示交談流程，並收集其所需的資訊。
    - 您可以連接其他 {{site.data.keyword.watson}} 服務來分析使用者輸入（例如 {{site.data.keyword.toneanalyzershort}} 或 {{site.data.keyword.speechtotextshort}}）。

- 根據使用者的目的及其他資訊，應用程式可以與**後端系統**互動。例如，回答問題、開立問題單、更新帳戶資訊，或下單。不限制您可以執行的動作。

## 實作

以下是實作交談的方式：

- **配置工作區。**利用簡易的圖形環境，設定交談的訓練資料及對話。

    訓練資料是由下列構件組成：
    - **目的**：您預期使用者與服務互動時將達到的目標。針對使用者輸入中可識別的每一個目標定義一個目的。例如，您可以定義名為 *store_hours* 的目的，以回答有關商店時數的問題。針對每一個目的，您都可以新增範例詞語，用於反映輸入客戶可能用來詢問其所需的資訊（例如 `What time do you open?`）
    - **實體**：實體代表提供目的之環境定義的術語或物件。例如，實體可能是城市名稱，可協助您的對話識別使用者要知道其商店時數的商店。

      在新增訓練資料時，會自動將自然語言分類器新增至工作區，並對其進行訓練，以瞭解您指出服務應該接聽及回應的要求類型。

    使用對話工具來建置納入目的及實體的對話流程。在工具中，對話流程會以圖形形式呈現為樹狀結構。您可以新增分支來處理您要服務處理的每一個目的。然後，您可以新增分支節點來處理根據其他因素（例如，使用者輸入中找到的實體，或者從應用程式或另一個外部服務傳遞至服務的資訊）之要求的多個可能排列。

- **部署工作區。**將已配置工作區部署給使用者，方法是將它連接至前端使用者介面、社交媒體或傳訊頻道。已部署的 {{site.data.keyword.conversationshort}} 服務實例是由 {{site.data.keyword.cloud_notm}}（IBM Cloud 運算平台）所管理（如需相關資訊，請參閱[平台概觀 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://console.bluemix.net/docs/overview/ibm-cloud.html#overview)）。

遵循下列鏈結，以深入閱讀這些實作步驟：

- [規劃目的及實體](intents-entities.html#planning-your-entities)
- [對話概觀](dialog-overview.html)
- [部署概觀](deploy.html)

## 瀏覽器支援

{{site.data.keyword.conversationshort}} 服務工具需要 {{site.data.keyword.Bluemix_notm}} 所需之相同層次的瀏覽器軟體。如需詳細資料，請參閱 {{site.data.keyword.Bluemix_notm}} [必要條件 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://console.bluemix.net/docs/overview/prereqs.html#browsers){: new_window} 主題。

## 語言支援

[支援的語言](lang-support.html)主題中詳述依特性的語言支援。

## 後續步驟

- [開始使用](getting-started.html)服務
- 嘗試一些[展示](sample-applications.html)。
- 檢視 [SDK ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://www.ibm.com/watson/developercloud/developer-tools.html){: new_window} 清單。

還有其他問題嗎？請與 [IBM 業務代表 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://www-01.ibm.com/marketing/iwm/dre/signup?source=urx-20970){: new_window} 聯絡。
