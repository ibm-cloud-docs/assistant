---

copyright:
  years: 2015, 2019
lastupdated: "2019-07-26"

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

# 建立助理
{: #assistant-add}

建立一個具有因應客戶商業目標所需技能的助理。
{: shortdesc}

若要先進一步瞭解什麼是助理，請參閱[助理](/docs/services/assistant?topic=assistant-assistants)。

請遵循下列步驟來建立助理：

1.  按一下**建立助理**。

1.  新增新助理的詳細資料：

    - **名稱**：名稱長度不得超過 100 個字元。名稱是必要項目。
    - **說明**：選用說明長度不得超過 200 個字元。

    系統會自動為您建立含 IBM 品牌的公用網頁，讓您和您的團隊可以用來測試您的助理。如果您不想要建立預覽網頁，請取消選取**啟用預覽鏈結**勾選框。

1.  按一下**建立助理**。

1.  藉由選擇要新增的下列其中一種技能類型，以將技能新增至助理。

    **附註**：您可以選擇新增現有技能，或建立新的技能。

    - **新增對話技能**：使用 Watson 自然語言處理程序和機器學習技術來瞭解使用者的問題和要求，並使用您編寫的回答對其進行回應。

      從這裡新增對話技能時，您會取得開發版本。如果您要新增特定對話技能版本，請改為從技能的*版本* 頁面中新增。

    - **新增搜尋技能** ![僅限加值或超值方案](images/plus.png)：對於給定的使用者查詢，使用 {{site.data.keyword.discoveryfull}} 服務在您識別的資料來源中擷取資訊，並將其發現的任何相關資訊作為對使用者的回應進行共用。

      只有在您是加值或超值方案使用者時，才會顯示此選項。
      {: note}

    請參閱[建立技能](/docs/services/assistant?topic=assistant-skill-add)。

## 助理限制
{: #assistant-add-limits}

可以建立的助理數取決於 {{site.data.keyword.conversationshort}} 方案類型。

| 方案 | 每個服務實例的助理數 |
|--------------|--------------------------------:|
|超值              |100 |
|加值              |100 |
|標準              |100 |
|加值試用   |                               5 |
|精簡*            |100 |
{: caption="方案詳細資料" caption-side="top"}

*閒置 30 天之後，可能會刪除「精簡」方案服務實例中未用的助理，以釋出空間。

如需主旨的相關資訊，請參閱[變更閒置逾時設定](/docs/services/assistant?topic=assistant-assistant-settings)。

您可以將每種類型的一個技能連接至助理。您可以建置的技能數目因您具有的方案而有所不同。如需詳細資料，請參閱[技能限制](/docs/services/assistant?topic=assistant-skill-add#skill-add-limits)。

## 刪除助理
{: #assistant-add-delete}

刪除助理時，也會自動刪除您針對助理而定義的所有整合。不會刪除您新增至助理的技能。

若要刪除助理，請遵循下列步驟：

1.  從「助理」標籤中，尋找您要刪除的助理。

1.  按一下 ![開啟及關閉選項清單](images/kabob-beta.png) 圖示，然後選擇**刪除**。確認刪除。

## 重新命名助理
{: #assistant-add-rename}

您可以在建立助理之後，變更助理的名稱及其關聯的說明。

若要重新命名助理，請遵循下列步驟：

1.  從「助理」標籤中，尋找您要重新命名的助理。

1.  按一下 ![開啟及關閉選項清單](images/kabob-beta.png) 圖示，然後選擇**重新命名**。

1.  編輯名稱，然後按一下**重新命名**以儲存您的變更。

### 變更與助理相關聯的技能
{: #assistant-add-swap-skill}

您可以將每種技能類型的一個技能新增給助理。如果您要變更助理正在使用的技能，可以將某種技能換成另一種技能。

1.  在「助理」標籤中，開啟助理。

1.  按一下要交換的技能的 ![開啟及關閉選項清單](images/kabob-beta.png) 圖示，然後選擇**交換技能**。

    若要將現行對話技能換成不同版本的技能，請選擇**變更技能版本**。

1.  選擇要改用的現有技能，或[建立技能](/docs/services/assistant?topic=assistant-skill-add)。

### 切換服務實例
{: #assistant-add-switch-instance}

如果您有多個服務實例，則可以檢查頁面標頭，以找出您目前使用的實例。如果您正在使用技能，請先按一下**技能**瀏覽途徑鏈結。橫幅即會顯示現行實例名稱。若要切換至不同的服務實例，請按一下**變更**，然後選擇適當的實例。
