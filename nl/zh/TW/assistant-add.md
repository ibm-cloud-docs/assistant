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

# 建立助理
{: #assistant-add}

建立一個具有因應客戶商業目標所需技能的助理。
{: shortdesc}

請遵循下列步驟來建立助理：

1.  按一下**助理**標籤。

1.  執行下列其中一項作業：

    - 若要建立您可以檢閱並學習的範例助理，請按一下**新增範例**，然後選擇要建立的範例助理。

      即會新增範例助理。您可以跳過本程序中的其餘步驟。

      範例技能隨範例助理一起提供，並且會新增至您的技能清單。如果您已建立相同類型的範例技能，則現有技能會自動與這個新的範例助理相關聯。
      {: note}

    - 若要從頭開始建立助理，請按一下**建立新的項目**，然後完成本程序中的其餘步驟。

1.  指定新助理的詳細資料：
    - **名稱**：名稱長度不得超過 100 個字元。名稱是必要項目。
    - **說明**：選用說明長度不得超過 200 個字元。

    系統會自動為您建立含 IBM 品牌的公用網頁，讓您和您的團隊可以用來測試您的助理。如果您不想要建立預覽網頁，請取消選取**啟用預覽鏈結**勾選框。

1.  按一下**建立**。

1.  按一下**新增技能**，以將技能新增至助理。您可以選擇新增現有技能，或建立新的技能。

    從這裡新增技能時，您會取得開發版本。如果您想要新增特定的技能版本，請改為從技能的*版本歷程* 標籤中新增。

    如果您已建立或已獲授與開發人員角色，以存取任何使用 {{site.data.keyword.conversationshort}} 服務正式發行版本（早期稱為 Watson Conversation）所建置的工作區，則會看到這些工作區被列為現有對話技能。
    {: note}

    如需如何建立技能的相關資訊，請參閱[建立技能](/docs/services/assistant?topic=assistant-skill-add)。

## 助理限制
{: #assistant-add-limits}

您可以在單一服務實例中建立的助理數目，取決於您的 {{site.data.keyword.conversationshort}} 方案。

|服務方案                             | 每個服務實例的助理數 | 每個助理的整合數  | 會談階段作業閒置期間 |
|--------------|--------------------------------:|----------------------------:|-----------------:|
|超值                                 |100 |100 |       60 分鐘 |
|加值         |100 |100 |       60 分鐘 |
|標準                                 |100 |100 |        5 分鐘 |
|精簡*            |100 |100 |        5 分鐘 |
{: caption="服務方案詳細資料" caption-side="top"}

*閒置 30 天之後，可能會刪除「精簡」方案服務實例中未用的助理，以釋出空間。

您可以將某種技能連接至助理。您可以建置的技能數目因您具有的方案而有所不同。如需詳細資料，請參閱[技能限制](/docs/services/assistant?topic=assistant-skill-add#skill-add-limits)。

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

您可以將某種技能新增至助理。如果您要變更助理使用的技能，可以將某種技能換成另一種技能。

1.  從「助理」標籤中，按一下以開啟您要變更其技能的助理磚。

1.  按一下 ![開啟及關閉選項清單](images/kabob-beta.png) 圖示，然後選擇**交換技能**。若要將現行技能換成不同版本的技能，請選擇**變更技能版本**。

1.  選擇要改用的現有技能，或[建立技能](/docs/services/assistant?topic=assistant-skill-add)。

### 切換服務實例
{: #assistant-add-switch-instance}

如果您有多個服務實例，則可以檢查頁面標頭，以找出您目前使用的實例。如果您正在使用技能，請先按一下**技能**瀏覽途徑鏈結。橫幅即會顯示現行實例名稱。若要切換至不同的服務實例，請按一下**變更**，然後選擇適當的實例。
