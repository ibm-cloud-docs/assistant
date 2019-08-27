---

copyright:
  years: 2015, 2019
lastupdated: "2019-07-16"

subcollection: assistant


---

{:curl: #curl .ph data-hd-programlang='curl'}
{:javascript: #javascript .ph data-hd-programlang='javascript'}
{:java: #java .ph data-hd-programlang='java'}
{:python: #python .ph data-hd-programlang='python'}
{:swift: data-hd-programlang='swift'}
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:codeblock: .codeblock}
{:download: .download}
{:deprecated: .deprecated}
{:important: .important}
{:note: .note}
{:tip: .tip}

# {{site.data.keyword.conversationshort}} API 概觀
{: #api-overview}

您可以使用 {{site.data.keyword.conversationshort}} REST API 和對應的 SDK，來開發與服務互動的應用程式。

## 用戶端應用程式

若要在運行環境建置與助理通訊的虛擬助理或其他用戶端應用程式，請使用新的第 2 版 API。藉由使用此 API，您可以開發面向使用者的用戶端（可進行部署以供正式作業使用）、分配管理系統在助理與另一個服務（例如會談服務或後端系統）之間進行通訊的應用程式，或一個測試應用程式。

藉由使用第 2 版運行環境 API 與助理進行通訊，應用程式可以充分運用下列特性：

- **自動狀態管理**。第 2 版運行環境 API 會管理與一般使用者的每個階段作業，以儲存並維護助理完成完整階段作業所需的所有環境定義資料。

- **使用助理輕鬆部署**。除了支援自訂用戶端之外，還可以輕鬆地將助理部署到 Slack 和 Facebook Messenger 這類常用傳訊頻道。

- **版本化**。使用對話技能版本化，您可以儲存技能的 Snapshot，並將助理鏈結到該特定版本。然後，您可以繼續更新開發版本，而不會影響正式作業助理。

- **搜尋功能**。第 2 版運行環境 API 可用於接收來自對話技能和搜尋技能的回應。如果提交的查詢是對話技能無法回答的，則助理可以使用搜尋技能在配置的資料來源中尋找最佳回答（搜尋技能是僅適用於「加值」或「超值」方案使用者的測試版特性）。

如需第 2 版 API 的詳細資料，請參閱 {{site.data.keyword.conversationshort}} [第 2 版 API 參考資料 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://{DomainName}/apidocs/assistant-v2){: new_window}。

**附註**：{{site.data.keyword.conversationshort}} 第 1 版 API 仍然支援舊式 `/message` 方法，用於將使用者輸入直接傳送至對話技能所使用的工作區。主要是基於舊版相容性，而支援第 1 版運行環境 API。如果您使用第 1 版 `/message` 方法，則必須實作您自己的狀態管理，而無法充分運用助理的版本化或其他任何特性。

## 編寫應用程式

第 1 版 API 提供多種方法，可讓應用程式建立或修改對話技能，作為使用 {{site.data.keyword.conversationshort}} 使用者介面以圖形方式建置技能的替代方案。編寫應用程式會使用 API 來建立及修改構成對話技能的技能、目的、實體、對話節點及其他構件。如需相關資訊，請參閱[第 1 版 API 參考資料 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://{DomainName}/apidocs/assistant){: new_window}。

  **附註：**第 1 版編寫方法會建立和修改工作區，而非技能。工作區是對話技能內對話及訓練資料（例如目的及實體）的容器。如果您使用 API 來建立新工作區，則它會在 {{site.data.keyword.conversationshort}} 使用者介面中顯示為新的對話技能。

如需可用 API 方法的清單，請參閱 [API 方法摘要](/docs/services/assistant?topic=assistant-api-methods)。
