---

copyright:
  years: 2015, 2019
lastupdated: "2019-02-21"

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
{:deprecated: .deprecated}
{:important: .important}
{:note: .note}
{:tip: .tip}

# {{site.data.keyword.conversationshort}} API 概觀
{: #api-overview}

您可以使用 {{site.data.keyword.conversationshort}} REST API 和對應的 SDK，來開發與服務互動的應用程式。{{site.data.keyword.conversationshort}} API 有兩個版本，每一個版本支援不同的函數集：第 1 版及第 2 版。您應該使用的 API，取決於應用程式所需的方法類型：

- **運行環境方法**：讓用戶端應用程式可與現有助理或技能互動（但不修改）的方法。您可以使用這些方法來開發面向使用者的用戶端（可進行部署以供正式作業使用）、分配管理系統在助理與另一個服務（例如會談服務或後端系統）之間進行通訊的應用程式，或一個測試應用程式。

  {{site.data.keyword.conversationshort}} 第 2 版 API 可讓您存取在執行時期用來與助理互動的方法（例如 `/message`）。這是用於開發新用戶端應用程式的偏好 API。如需第 2 版 API 的詳細資料，請參閱 {{site.data.keyword.conversationshort}} [第 2 版 API 參考資料 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://{DomainName}/apidocs/assistant-v2){: new_window}。

  {{site.data.keyword.conversationshort}} 第 1 版 API 包含 `/message` 方法，用於將使用者輸入直接傳送至對話技能所使用的工作區，而略過助理。主要是基於舊版相容性，而支援第 1 版運行環境 API。如果您使用第 1 版 `/message` 方法，您的應用程式無法充分運用助理的編排和狀態管理功能。如需相關資訊，請參閱[使用第 1 版運行環境 API](/docs/services/assistant?topic=assistant-api-client#v1-api)。

- **編寫方法**：可讓應用程式建立或修改對話技能的方法，作為使用 {{site.data.keyword.conversationshort}} 工具以圖形方式建置技能的替代方案。編寫應用程式會使用各種方法，來建立及修改構成對話技能的技能、目的、實體、對話節點及其他構件。

  若要建置編寫應用程式，請使用第 1 版 API。如需相關資訊，請參閱[第 1 版 API 參考資料 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://{DomainName}/apidocs/assistant){: new_window}。


  **附註：**第 1 版編寫方法會與工作區互動，而非與技能互動。工作區是對話技能內對話及訓練資料（例如目的及實體）的容器。對於大部分用途而言，使用 API 時，您可以將工作區及對話技能想成可交換的；例如，如果您使用 API 來建立新工作區，則它會在 {{site.data.keyword.conversationshort}} 工具中顯示為新的對話技能。

如需可用 API 方法的清單，請參閱 [API 方法摘要](/docs/services/assistant?topic=assistant-api-methods)。
