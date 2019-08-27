---

copyright:
  years: 2015, 2019
lastupdated: "2019-08-02"

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

# 建立技能
{: #skill-add}

自訂助理，方法為將滿足客戶目標所需的技能新增至助理。
{: shortdesc}

您可以建立下列類型的技能：

- **對話技能**：使用 Watson 自然語言處理程序和機器學習技術來瞭解使用者的問題和要求，並使用您編寫的回答對其進行回應。

- **搜尋技能** ![僅限加值或超值方案](images/plus.png)：對於給定的使用者查詢，使用 {{site.data.keyword.discoveryfull}} 服務來搜尋自助服務內容的資料來源並傳回回答。

  只有加值或超值方案的使用者才能建立這類型的技能。
  
  一般而言，您可以先建立每種類型的技能。然後，為對話技能建置對話時，可決定何時起始搜尋技能。對於一些問題或要求，使用寫在程式中或以程式設計方式衍生的回應（在對話技能中定義）就已足夠。對於另一些問題或要求，建議您藉由傳回一整段相關資訊（即使用搜尋技能從外部資料來源中擷取的資訊）來提供更充分的回應。

## 建立技能
{: #skill-add-task}

您可以將每種技能類型的一個技能新增給助理。

若要建立技能，請完成下列步驟：

1.  按一下**技能**標籤，然後按一下**建立技能**。

1.  選擇要建立的技能類型，然後按**下一步**。

    遵循適當程序中的剩餘步驟來完成技能建立處理程序。

      - [對話技能](/docs/services/assistant?topic=assistant-skill-dialog-add)
      - [搜尋技能](/docs/services/assistant?topic=assistant-skill-search-add)

## 技能限制
{: #skill-add-limits}

可以建立的技能數取決於 {{site.data.keyword.conversationshort}} 方案類型。可供您使用的任何樣本對話技能都不會計入限制，除非您使用這些技能。技能版本不作為技能進行計數。

| 方案 | 每個服務實例的技能數 |
|------------------|----------------------------:|
|超值              |                          50 |
|加值              |                          50 |
|標準              |20                          |
|精簡`*` 和加值試用|                               5 |
{: caption="方案詳細資料" caption-side="top"}

`*` 閒置 30 天之後，可能會刪除精簡方案服務實例中未用的技能，以釋出空間。

## 刪除技能
{: #skill-add-delete}

除非助理正在使用，否則您可以刪除任何您可存取的技能。如果正在使用，您必須先將它從使用它的助理中移除，您才能予以刪除。

在刪除之前，務必先檢查是否有其他人正在使用該技能。
{: tip}

若要刪除技能，請完成下列步驟：

1.  瞭解是否有任何助理正在使用該技能。從「技能」標籤中，尋找您要刪除的技能磚。**助理**欄位會列出目前使用該技能的助理。

1.  如果您要刪除的技能與助理相關聯，請完成下列步驟，將它從助理中移除：

    - 在您要移除技能之前，請先跟使用該技能的助理擁有者洽詢。
    - 開啟「助理」標籤，然後按一下以開啟助理磚。
    - 尋找您要刪除的技能磚。按一下 ![開啟及關閉選項清單](images/kabob-beta.png) 圖示，然後選擇**移除**。
    - 針對使用該技能的任何其他助理，重複先前的步驟。
    - 回到「技能」標籤，尋找您要刪除的技能磚。

1.  按一下 ![開啟及關閉選項清單](images/kabob-beta.png) 圖示，然後選擇**刪除**。確認刪除。
