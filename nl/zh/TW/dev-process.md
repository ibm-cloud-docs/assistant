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

# 開發處理程序
{: #dev-process}

建置、部署及漸進式改善交談助理時，請使用 {{site.data.keyword.conversationshort}} 來運用 AI。
{: shortdesc}

![顯示開發步驟流程，從開發訓練資料開始，結束於部署至正式作業](images/dev-process.png)

## 工作流程
{: #dev-process-workflow}

助理專案的一般工作流程包括下列步驟：

1.  定義一組嚴密的重要客戶需求，您想要助理代表您處理這些需求，包括它可以為客戶起始或完成的所有商業程序。一開始定義的內容不要太多。
1.  建立一些目的，代表您在前一個步驟中所識別的客戶需求。例如，`#about_company` 或 `#place_order` 這類目的。

    ![僅限加值或超值方案](images/plus.png) 使用目的建議特性來發掘現有客服中心日誌，以尋找您使用案例的最佳目的。如需詳細資料，請參閱[定義目的時取得協助](/docs/services/assistant?topic=assistant-intent-recommendations)。
    {: tip}

1.  建置一個對話，利用簡單回應或首先收集相關資訊的對話流程，來偵測已定義的目的並進行處理。
1.  定義更清楚瞭解使用者意思所需的任何實體。

    發掘一般實體值提及項目的現有目的使用者範例。使用註釋來定義實體不僅會擷取實體值的文字，還會擷取實體值通常用於句子中的環境定義。

    若為字典型實體，請使用同義字建議來擴充實體定義。
    {: tip}

1.  測試您在「試用」窗格中新增至助理的每一項功能，並以漸進方式逐步進行。
1.  當您有一個運作中的助理可以順利處理重要作業時，請新增一個將助理部署至開發環境的整合。測試已部署的助理，並進行修正。

1.  在您建置有效的助理之後，請取得對話技能的 Snapshot，並將它另存為一個版本。

    當您到達開發里程碑時請儲存一個版本，可讓您在後續對技能所做的變更降低其效果時回到該版本。請參閱[建立技能版本](/docs/services/assistant?topic=assistant-versions)。
1.  將助理版本部署至測試環境，然後進行測試。

    如果您使用 Web 管理的會談小組件，則可以與其他人共用 URL，以在測試時取得其協助。
1.  使用「分析」標籤中的度量來尋找可以改善的區域，並進行調整。

    如果您需要測試替代方法來解決問題，請為每個解決方案建立一個版本，讓您可以獨立部署及測試每個解決方案，並比較結果。
1.  當您對助理的效能感到滿意時，請將最佳版本的助理部署至正式作業環境。
1.  監視來自使用者與已部署助理進行之交談的日誌。

    您可以從技能開發版本的「分析」標籤，檢視在正式作業中執行的技能版本的日誌。在發現錯誤分類或其他問題時，您可以在技能的開發版本中進行更正，然後在測試之後，將改良的版本部署至正式作業。如需詳細資料，請參閱[跨助理改善](/docs/services/assistant?topic=assistant-logs#logs-deploy-id)。

分析日誌及改善對話技能的處理程序不斷進行中。一段時間之後，您可能會想要擴展助理可以為您處理的作業。客戶也需要變更。在產生新的需求時，已部署助理所產生的度量可協助您識別它們，並在後續反覆處理它們。
