---

copyright:
  years: 2015, 2018
lastupdated: "2018-01-24"

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

# 在 Slack 中測試

您可以使用測試部署工具，將 {{site.data.keyword.conversationshort}} 工作區當作機器人使用者整合至 Slack 團隊。如果您要使用 Slack 機器人作為工作區的使用者介面來快速測試，請使用此方法。

測試部署工具會使用 {{site.data.keyword.openwhisk}} 服務，將預先建置的 Slack 應用程式當作機器人使用者部署至您的團隊。此應用程式會處理與 {{site.data.keyword.conversationshort}} 工作區的通訊。

![測試部署概觀圖](images/testdeploy_diagram.png)

請注意，測試部署工具有一些限制：

- 您無法使用此工具來發佈應用程式，以供其他團隊使用。
- 如果您使用此方法，將多個工作區部署至同一個團隊，則所有工作區都會回應 `@ibmwatson_bot` 使用者名稱。建議您使用此工具，一次只將一個工作區部署至每一個 Slack 團隊。
- 您必須具有將應用程式安裝至 Slack 團隊的許可權。如果您不確定是否具有此許可權，請詢問 Slack 管理者。
- 預先建置的 Slack 應用程式僅用於測試，而且可能不是隨時都可用。
- 因為 {{site.data.keyword.openwhisk_short}} 限制，所以此工具目前僅適用於 {{site.data.keyword.Bluemix_notm}} 美國南部地區。

若要以機器人使用者身分來安裝應用程式，請執行下列動作：

1. 在 {{site.data.keyword.conversationshort}} 工具中，開啟您要在 Slack 中測試的工作區。
1. 按一下左上角的功能表圖示，然後選取**部署**。即會開啟「部署選項」頁面。

   ![快速部署功能表選項](images/deploy_menu_testdeploy.png)

1. 在**使用 {{site.data.keyword.openwhisk_short}} 部署**下，按一下**在 Slack 中測試**，並遵循指示進行。

   ![建立 Slack 測試按鈕](images/testdeploy_testinslack.png)

## 與機器人聊天

完成部署處理程序之後，您可以使用 `@ibmwatson_bot` 使用者名稱來與 {{site.data.keyword.conversationshort}} 工作區互動，就像與任何其他 Slack 機器人一樣。

請謹記，部署至團隊的機器人會保留特定頻道內每一位使用者的狀態。這表示會無限期維護您在對話環境定義中儲存的任何變數，除非您的對話清除這些變數。

如果您需要將交談重設為已知的起始狀態，則必須在對話內這麼做。請確定對話具有在交談結束時執行的節點，或您需要重新開始的任何其他時間。請更新此節點的 JSON，將所有環境定義變數重設為適當的起始值。（必要的話，您可以使用**跳至**動作或特殊「結束交談」目的來執行此節點。）

例如，如果您的工作區使用稱為 `drink_order` 的環境定義變數來儲存使用者的飲料選擇，則可以使用 `context.remove` 方法，在交談結束時刪除此變數：

```json
"context": {
   "reset_drink_order": "<?context.remove('drink_order')?>"
 }
```
{: codeblock}

如需修改環境定義變數值的相關資訊，請參閱[更新環境定義變數值](dialog-overview.html#updating-a-context-variable-value)。

**附註：**在完成工作區的測試時，您可以回到測試部署工具，然後按一下**刪除測試**，來刪除測試部署。請記住，您還必須個別取消授權 Slack 團隊中的機器人應用程式。
