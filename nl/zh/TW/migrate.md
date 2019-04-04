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

# 移轉
{: #migrate}

移轉 {{site.data.keyword.conversationshort}} 服務實例，將它從其現行 Cloud Foundry 組織及空間移至資源群組。
{: shortdesc}

將 {{site.data.keyword.cloud_notm}} 從使用 Cloud Foundry 移至使用資源群組。

資源群組提供比 Cloud Foundry 更多的好處，如下所示：

- 使用 IBM Cloud Identity and Access Management (IAM) 可進行精細的存取控制
- 能夠將服務實例連接至不同地區的應用程式及服務
- 簡化每個群組的使用資料的擷取

如果您在 2018 年 11 月之前建立服務實例，則視管理實例的所在位置而定，有可能是使用 Cloud Foundry 而非資源群組。如需每個位置何時開始在新實例中使用 IAM 的相關資訊，請參閱[資料中心](/docs/services/assistant?topic=assistant-services-information#services-information-regions)。

## 移轉服務實例
{: #migrate-task}

如果您想要在開始之前進一步瞭解移轉處理程序，請參閱 [IBM Cloud 移轉文件 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](/docs/resources?topic=resources-migrate)。

只有建立該實例或對該實例具有`開發人員`角色存取權的人員或群組，才能移轉實例。
{: note}

若要移轉服務實例，請完成下列步驟：

1.  首先，決定您要將服務實例移至哪一個資源群組。

    如需提示，請參閱[在資源群組中組織資源的最佳作法 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](/docs/resources?topic=resources-bp_resourcegroups)。

1.  從 IBM Cloud 儀表板服務清單中，對您要移轉的實例按一下「移轉」圖示 ![移轉](images/migrate.svg)，然後按一下蹦現畫面中的**移轉**。

    在 2018 年 5 月 7 日之前於雪梨資料中心或在 2018 年 12 月 13 日之前於倫敦資料中心建立的所有服務實例，都已聯合至達拉斯資料中心。移轉基於雪梨或基於倫敦的 Cloud Foundry 服務實例時，它會轉換為在達拉斯管理的資源。
    {: note}

1.  按一下**繼續**，然後選擇資源群組。

    如果您尚未建立資源群組，則可以立即建立一個。已提供**預設**資源群組。請花點時間瞭解如何使用群組，必要的話請建立一個。之後*無法* 變更您在這裡選擇的資源群組。

1.  按一下**移轉**。

    當處理程序完成時，會顯示一則訊息。如果您有其他要移轉的服務實例，可以繼續移轉其他服務實例，或按一下**完成**。

您已移轉的舊（基於 Cloud Foundry 組織）服務實例會繼續列在「儀表板」的 Cloud Foundry 服務區段中，現在它顯示為該實例的新（基於資源群組）版本的*別名*。

![顯示現行服務實例現在是基於資源的實例的別名](images/alias.png)

如需別名的相關資訊，請參閱 [IBM Cloud Connections 文件 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://cloud.ibm.com/docs/resources/connecting_apps#what_is_alias)。

您必須開啟該服務實例基於資源群組的新版本，才能存取**啟動工具**按鈕。新的實例會列在 {{site.data.keyword.Bluemix_notm}} 儀表板的「服務」區段中。

## 鑑別
{: #migrate-auth-support}

如果您的現有應用程式使用基本鑑別來存取服務，則在移轉服務實例之後，它們可以繼續傳遞使用者名稱和密碼來鑑別該服務實例。您可以從別名的**服務認證**頁面中查看認證資訊。

您的新實例會使用 IBM Cloud Identity and Access Management (IAM) 來管理鑑別，這是一種加強機制，它使用 API 金鑰而非使用者名稱和密碼認證。您可以從新服務實例的**服務認證**頁面中查看 API 金鑰資訊。

請考慮更新現有的自訂應用程式，讓它們使用新的鑑別方法，以充分運用它提供的改良安全性。您對新實例的對話技能所做的全部變更，都會反映在也使用基本鑑別認證的應用程式中。在您更新所有應用程式以使用新的 API 金鑰方法之後，您不需要別名就可以將它刪除。
