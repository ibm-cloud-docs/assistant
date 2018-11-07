---

copyright:
  years: 2015, 2018
lastupdated: "2018-02-09"

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

# 使用 {{site.data.keyword.conversationshort}} 連接器部署至頻道

開發工作區之後，您可以使用 {{site.data.keyword.conversationshort}} 連接器快速地將它連接至通訊頻道（例如 Slack 或 Facebook Messenger）。{{site.data.keyword.conversationshort}} 連接器是一組 {{site.data.keyword.openwhisk}} 元件，可調解 {{site.data.keyword.conversationshort}} 工作區與您擁有的 Slack 或 Facebook 應用程式之間的通訊，並將階段作業資料儲存至 Cloudant 資料庫。

將工作區連接至頻道應用程式，即可讓它成為 Slack 或 Facebook Messenger 使用者可與其互動的聊天機器人。對話的回應可包含多媒體及互動式回應，例如影像及可按式按鈕（如需定義多媒體回應的相關資訊，請參閱[多媒體回應](dialog-multimedia.html)。）

![{{site.data.keyword.openwhisk_short}} 部署概觀圖](images/deploytochannel_diagram.png)

{{site.data.keyword.conversationshort}} 連接器有一些限制：

- 您必須建立 Slack 或 Facebook 應用程式，或具有管理許可權，才能修改您要使用的應用程式。
- 因為 {{site.data.keyword.openwhisk_short}} 限制，所以此工具目前僅適用於 {{site.data.keyword.Bluemix_notm}} 美國南部地區。

## 使用 {{site.data.keyword.conversationshort}} 工具部署至 Slack

{{site.data.keyword.conversationshort}} 工具提供一種快速方式，以使用 {{site.data.keyword.conversationshort}} 連接器來部署機器人。此工具會引導您逐步收集配置頻道應用程式及連接器元件的必要資訊，然後將必要元件自動部署至 IBM Cloud。

**附註：**{{site.data.keyword.conversationshort}} 工具介面目前只支援 Slack，但您可以使用自動化的 {{site.data.keyword.Bluemix_short}} 處理程序來部署 Facebook Messenger。如需部署 Facebook Messenger 的相關資訊，請參閱 {{site.data.keyword.conversationshort}} 連接器 [GitHub 儲存庫 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/watson-developer-cloud/conversation-connector/blob/master/channels/facebook/README.md){: new_window} 中的文件。

若要使用自動化部署工具來部署機器人，請執行下列動作：

1. 在 {{site.data.keyword.conversationshort}} 工具中，開啟您要部署的工作區。
1. 按一下左上角的功能表圖示，然後選取**部署**。

   ![快速部署功能表選項](images/deploy_menu_testdeploy.png)

1. 在**使用 {{site.data.keyword.openwhisk_short}} 部署**下，按一下**部署至 Slack**。
1. 在 Slack 頁面上，按一下**部署至 Slack 應用程式**，並遵循指示進行。

   ![部署至 Slack 應用程式按鈕](images/deploy_deploytoslack.png)

## 手動部署

您可以將必要元件手動配置及部署至 IBM Cloud，而不是使用自動化工具將工作區部署為機器人。在以下數種狀況下，您可能會想要執行此作業：

- **局部重新部署**。您可能想要重新部署現有部署的部分元件，以變更配置、更正問題或套用新版本的修正程式。
- **使用新的功能擴充機器人**。您可以修改提供的元件來新增功能，例如，先前處理使用者輸入，再將它傳送至 {{site.data.keyword.conversationshort}} 工作區。
- **部署至新的頻道**。如果您要將機器人部署至 Slack 或 Facebook Messenger 以外的頻道，則可以遵循現有元件的型樣來開發您自己的元件。

{{site.data.keyword.conversationshort}} 連接器元件是在可下載或複製的公用 GitHub 儲存庫中進行管理。如需相關資訊，請參閱[儲存庫 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/watson-developer-cloud/conversation-connector){: new_window} 中提供的文件。

## 與機器人聊天

完成部署處理程序之後，您可以使用機器人使用者名稱來與 {{site.data.keyword.conversationshort}} 工作區互動，就像與任何其他 Slack 或 Facebook Messenger 機器人一樣。

請記住，{{site.data.keyword.conversationshort}} 連接器會分別保留與機器人互動之每一位使用者的狀態（如果是 Slack，則單一使用者與機器人可能會有多個唯一交談，一個透過直接訊息，以及每個頻道一個交談。）這表示會無限期維護您在對話環境定義中儲存的全部變數，除非您清除這些變數。

如果您需要將交談重設為已知的起始狀態，則可以在對話內這樣做。請確定對話具有在交談結束時執行的節點，或是在您需要重新開始的任何其他時間執行的節點。請更新此節點的 JSON，將所有環境定義變數重設為適當的起始值（必要的話，您可以使用**跳至**動作或特殊「結束交談」目的來執行此節點。）

例如，如果您的工作區使用稱為 `drink_order` 的環境定義變數來儲存使用者的飲料選擇，則可以使用 `context.remove` 方法，在交談結束時刪除此變數：

```json
"context": {
    "reset_drink_order": "<?context.remove('drink_order')?>"
 }
```
{: codeblock}

如需修改環境定義變數值的相關資訊，請參閱[更新環境定義變數值](dialog-runtime.html#context-update)。

您也可以編輯已部署的 {{site.data.keyword.openwhisk_short}} 動作，以清除環境定義，或對機器人行為進行其他變更。如需相關資訊，請參閱 [GitHub 儲存庫![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/watson-developer-cloud/conversation-connector){: new_window} 中提供的文件。
