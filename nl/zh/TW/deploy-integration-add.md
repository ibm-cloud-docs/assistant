---

copyright:
  years: 2015, 2019
lastupdated: "2019-07-19"

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

# 新增整合
{: #deploy-integration-add}

若要部署您的技能，請將其新增至助理，然後新增整合至助理，以將您的機器人發佈到客戶尋求協助的頻道。
{: shortdesc}

## 服務人員平台整合運作方式
{: #deploy-integration-service-desk-integrations}

觀看此 3 分鐘的視訊，進一步瞭解將助理與服務人員平台（如 Intercom）整合。

<iframe class="embed-responsive-item" id="youtubeplayer" title="服務人員整合運作方式概觀" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/pJSCZLQVgCY?rel=0" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

若要瞭解將服務人員與助理整合對您的企業有何助益，請[閱讀本部落格文章 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://medium.com/ibm-watson/contact-center-post-394dff427c8){: new_window}。

## 新增整合
{: #deploy-integration-add-task}

請遵循下列步驟將整合新增至助理：

1.  按一下**助理**標籤。

    如果您看不到**助理**標籤，請按一下頁面標題中的瀏覽途徑功能表鏈結。
    {: note}

1.  按一下以開啟您要部署的助理磚。

1.  移至「整合」區段。

    **何謂「預覽鏈結」整合？** 建立助理時，會自動為您佈建測試 Web 網站（除非您選擇不啟用預覽鏈結）。它有一個簡單的會談小組件介面，可用來與您的助理互動，以作為測試使用。您也可以與團隊成員共用這個含 IBM 品牌的網站 URL。

1.  按一下**新增整合**。

1.  按一下要與助理整合的頻道磚。選項包括：

    - [自訂應用程式](/docs/services/assistant?topic=assistant-deploy-custom-app)
    - [Facebook Messenger](/docs/services/assistant?topic=assistant-deploy-facebook)
    - [Intercom](/docs/services/assistant?topic=assistant-deploy-intercom)  ![僅限「加值」或「超值」方案](images/plus.png)
    - [預覽鏈結](/docs/services/assistant?topic=assistant-deploy-web-link)
    - [Slack](/docs/services/assistant?topic=assistant-deploy-slack)
    - [Voice Agent（電話系統）![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示 ")](https://cloud.ibm.com/catalog/services/voice-agent-with-watson){: new_window}

      開啟 {{site.data.keyword.cloud}} 上的 **Voice Agent with Watson** 服務概觀頁面。
    - [WordPress 外掛程式 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://wordpress.org/plugins/conversation-watson/){: new_window}

      開啟 WordPress 網站上的 IBM Watson Assistant 外掛程式概觀頁面。

1.  遵循畫面上提供的指示來完成整合處理程序。

在整合助理之後，請從目標頻道進行測試，以確保助理如預期般運作。

如需如何跨所有整合類型以一致方式啟動對話的提示，請參閱[啟動對話](/docs/services/assistant?topic=assistant-dialog-start)。

## 整合限制
{: #deploy-integration-add-limits}

您可以在單一服務實例中建立的整合數目，取決於您的 {{site.data.keyword.conversationshort}} 方案。

|服務方案                             | 每個助理的整合數  |
|------------------|---------------------------:|
|超值              |100 |
|加值              |100 |
|標準              |100 |
|加值試用   |100 |
|精簡              |100 |
{: caption="服務方案詳細資料" caption-side="top"}
