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

# 關於
{: #index}

{{site.data.keyword.conversationfull}} 是一種認知機器人，您可以針對商業需求予以自訂，並且跨多個頻道進行部署，以隨時隨地提供客戶所需的協助。
{: shortdesc}

## 運作方式
{: #index-how-it-works}

此圖顯示整體架構：

![服務流程圖](images/arch-overview.png)

- 使用者透過下列一個以上**整合**點與助理互動：

  - 您直接發佈至現有社交媒體傳訊平台（例如 Slack 或 Facebook Messenger）的聊天機器人。
  - 由 IBM Cloud 管理的簡單聊天機器人使用者介面。
  - 您開發的自訂應用程式，例如行動式應用程式或使用語音介面的機器人。

- **助理**會接收使用者輸入，然後將其遞送至對話技能。

- 對話**技能**會進一步解譯使用者輸入，然後指示交談的流程，並收集回應或代表使用者執行交易所需的所有資訊。

## 實作
{: #index-mplementation}

以下是實作助理的方式：

- **建立對話技能**。使用直覺式圖形工具來定義訓練資料，以及助理與客戶之間的交談對話。

  訓練資料是由下列構件組成：

  - **目的**：您預期使用者與服務互動時將達到的目標。針對使用者輸入中可識別的每一個目標定義一個目的。例如，您可以定義名為 *store_hours* 的目的，以回答有關商店時數的問題。針對每一個目的，您都可以新增範例詞語，用於反映輸入客戶可能用來詢問其所需的資訊（例如 `What time do you open?`）

    或者，使用 IBM 提供的預先建置**內容型錄**，來開始使用處理一般客戶目標的資料。

  - **對話**：使用對話工具來建置納入您目的的對話流程。在工具中，對話流程會以圖形形式呈現為樹狀結構。您可以新增分支來處理您要服務處理的每一個目的。

  - **實體**：實體代表提供目的之環境定義的術語或物件。例如，實體可能是城市名稱，可協助您的對話識別使用者要知道其商店時數的商店。新增實體之後，請更新對話以使用這些實體。新增對話節點，根據在使用者輸入中找到的實體，處理要求的許多可能排列。

    在新增訓練資料時，會自動將自然語言分類器新增至技能，並對其進行訓練，以瞭解您指出服務應該接聽及回應的要求類型。

- **建立助理**。

- **新增對話技能至助理。**

- **整合助理。**建立頻道整合，以將所配置的助理直接部署至社交媒體或傳訊頻道。

  已部署的助理是由 {{site.data.keyword.cloud_notm}}（IBM 雲端運算平台）進行管理。（如需相關資訊，請參閱[平台概觀 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://cloud.ibm.com/docs/overview/ibm-cloud#overview)）。

遵循下列鏈結，以深入閱讀這些實作步驟：

- [目的建立概觀](/docs/services/assistant?topic=assistant-intents#intents-described)
- [對話概觀](/docs/services/assistant?topic=assistant-dialog-overview)
- [實體建立概觀](/docs/services/assistant?topic=assistant-entities#entities-described)
- [助理概觀](/docs/services/assistant?topic=assistant-assistant-add)
- [新增整合](/docs/services/assistant?topic=assistant-deploy-integration-add)

## 我的工作區在哪裡？
{: #index-existing-customers}

如果您在舊版服務中建立*工作區*，不用擔心；您仍然可以取得該工作區。工作區現在稱為*技能*。若要移至您的工作區，請按一下**技能**標籤。

## 瀏覽器支援
{: #index-browser-support}

{{site.data.keyword.conversationshort}} 服務工具所需的瀏覽器軟體層次與 {{site.data.keyword.Bluemix_notm}} 所需的層次相同。如需詳細資料，請參閱 {{site.data.keyword.Bluemix_notm}} [必要條件 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://cloud.ibm.com/docs/overview/prereqs#browsers){: new_window} 主題。

## 語言支援
{: #index-lang-support}

[支援的語言](/docs/services/assistant?topic=assistant-language-support)主題中詳述依特性的語言支援。

## 條款與注意事項
{: #index-notices}

如需服務條款的相關資訊，請參閱 [IBM Cloud 條款與注意事項 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](/docs/overview/terms-of-use?topic=overview-terms)。

## 後續步驟
{: #index-next-steps}

- [開始使用](/docs/services/assistant?topic=assistant-getting-started)服務。
- 檢視[開發人員資源 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示") 的清單](https://www.ibm.com/watson/developer-resources/){: new_window}。

還有其他問題嗎？請與 [IBM 業務代表 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://www-01.ibm.com/marketing/iwm/dre/signup?source=urx-20970){: new_window} 聯絡。
