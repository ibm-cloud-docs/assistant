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

# 備份及還原資料
{: #backup}

藉由匯出、然後再匯入資料，來備份及還原您的資料。
{: shortdesc}

您可以從 {{site.data.keyword.conversationshort}} 服務實例匯出下列資料：

- 對話技能訓練資料（目的及實體）
- 對話技能對話

您無法匯出下列資料：

<!--- Search skill -->
- 助理，包括任何已配置的整合

## 保留日誌
{: #backup-retain-logs}

如果您想要儲存使用者與您助理所擁有的交談日誌，可以使用 `/logs` API 來匯出日誌資料。如需詳細資料，請參閱 [API 參考資料 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://cloud.ibm.com/apidocs/assistant#list-log-events-in-a-workspace)。

若要取得技能的工作區 ID，請從技能磚中按一下 ![開啟及關閉選項清單](images/kabob-beta.png) 圖示，然後選擇**檢視 API 詳細資料**。
{: tip}

日誌的儲存時間因您的服務方案而有所不同。例如，「精簡」方案只提供過去 7 天的日誌。如需相關資訊，請參閱[日誌限制](/docs/services/assistant?topic=assistant-logs#logs-limits)。

您無法將日誌從某個技能匯入至另一個技能。

## 匯出對話技能
{: #backup-export-skill}

若要備份對話技能資料，請將技能匯出為 JSON 檔案，並儲存 JSON 檔案。

1.  在「技能」頁面上或在使用技能之助理的配置頁面上，尋找對話技能磚。

1.  按一下 ![開啟及關閉選項清單](images/kabob-beta.png) 圖示，然後選擇**下載 JSON**。

1.  指定 JSON 檔案的名稱及儲存位置，然後按一下**儲存**。

或者，您可以使用 `/workspaces` API 來匯出對話技能。包含 `export=true` 參數與 GET 工作區要求。如需詳細資料，請參閱 [API 參考資料 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://cloud.ibm.com/apidocs/assistant#get-information-about-a-workspace)。

## 匯入對話技能
{: #backup-import-skill}

若要恢復您從另一個服務實例或環境中匯出之對話技能的備份副本，請藉由匯入您所匯出之對話技能的 JSON 檔案，來建立新的對話技能。

如果 {{site.data.keyword.conversationshort}} 服務在您匯出技能與匯入技能之間發生變更，由於功能更新項目會定期套用至雲端管理之持續交付環境中的實例，因此您所匯入技能的功能可能與之前不同。
{: important}

1.  按一下**技能**標籤。

1.  按一下**建立新的項目**。

1.  按一下**匯入技能**，然後按一下**選擇 JSON 檔案**，並選取您要匯入的 JSON 檔案。

    **重要事項：**

    - 匯入的 JSON 檔必須使用不包含位元組順序標記 (BOM) 編碼的 UTF-8 編碼。
    - 技能 JSON 檔案的大小上限是 10 MB。如果您需要匯入較大的技能，請考慮在匯入技能之後個別匯入目的及實體。（您也可以使用 REST API 來匯入較大的技能。如需相關資訊，請參閱 [API 參考資料 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://cloud.ibm.com/apidocs/assistant#create-workspace){: new_window}。）
    - JSON 檔案不得包含定位點、換行或回車字元。

    選取**全部（目的、實體及對話）**，來匯入已匯出技能的完整副本。

    按一下**匯入**。

    如果您在匯入技能時遇到問題，請參閱[疑難排解技能匯入問題](/docs/services/assistant?topic=assistant-skill-add#skill-add-import-errors)。

1.  指定技能的詳細資料：

    - **名稱**：名稱長度不得超過 100 個字元。名稱是必要項目。
    - **說明**：選用說明長度不得超過 200 個字元。
    - **語言**：使用者輸入的語言，將訓練技能以瞭解該語言。預設值是英文。

在建立技能之後，它會顯示為「技能」頁面上的磚。

## 重建助理
{: #backup-recreate-assistant}

您現在可以重建助理。然後，您可以將匯入的對話技能鏈結至助理，並配置其整合。

如需詳細資料，請參閱[建立助理](/docs/services/assistant?topic=assistant-assistant-add)及[新增整合](/docs/services/assistant?topic=assistant-deploy-integration-add#deploy-integration-add-task)。

## 更新用戶端應用程式
{: #backup-update-api}

匯入所匯出的對話技能時，會建立一個新技能。新技能具有新的工作區 ID。如果您現有的用戶端應用程式使用第 1 版 API 來存取此技能，您必須更新所有工作區 ID 參照，以改用新的工作區 ID。

重建助理時，會為它提供新的助理 ID。如果您現有的用戶端應用程式使用第 2 版 API 來存取助理，您必須更新所有助理 ID 參照，以改用新的助理 ID。
