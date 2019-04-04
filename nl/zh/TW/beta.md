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

# 測試版
{: #beta}

![測試版](images/beta.png) IBM 發行分類為測試版的評估的服務、特性及語言支援。這些特性可能不穩定、經常變更，且可能接到簡短通知就中斷服務。測試版特性也可能未提供正式發行特性所提供但不要用於正式作業環境的相同層次的效能或相容性。只有 [IBM Developer Answers ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://developer.ibm.com/answers/topics/watson-assistant/){: new_window} 才支援測試版特性。

## 可用的測試版特性
{: #beta-features}

下列特性僅供測試版程式的參與者使用。若要瞭解如何要求存取權，請參閱[參與測試版程式](/docs/services/assistant?topic=assistant-feedback#feedback-beta)。

- 您使用搜尋技能的方式已變更。您現在可以將某個搜尋技能和某個對話技能新增至相同的助理。當您新增這兩者時，如果對話技能的對話中的任何節點都無法處理使用者輸入，則會觸發搜尋。您可以從下列主題進一步瞭解：

  - [搜尋技能](/docs/services/assistant?topic=assistant-skill-search-add)
  - [對話技能](/docs/services/assistant?topic=assistant-beta-skill-dialog-add)

  此特性發行後，它僅適用於「加值」或「超值」方案使用者。

- 「對話」建置器的使用者介面已更新為使用 React JavaScript 程式庫。現在，在封裝的元件中提供了對話功能，這些元件會管理自己的狀態，從而產生更具回應力的使用者體驗。

- 您可以配置技能來更正使用者輸入的拼字錯誤。如需詳細資料，請參閱[更正使用者輸入](/docs/services/assistant?topic=assistant-beta-spell-check)。

- 運用現有的客戶支援中心會談記錄，以找出適當的一組目的及使用者範例，用來訓練您的助理。如需詳細資料，請參閱[建置目的時取得協助](/docs/services/assistant?topic=assistant-beta-intent-recommendations)。
