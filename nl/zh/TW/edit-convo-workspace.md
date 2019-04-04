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

# 存取工作區
{: #edit-convo-workspace}

如果您使用舊版的服務（甚至是在它被稱為 {{site.data.keyword.watson}} Conversation 時的版本）建立了工作區，您可以繼續使用該工作區。
{: shortdesc}

## 變更交談工作區
{: #edit-convo-workspace-task}

您的工作區仍然可用；只是它現在稱為*技能*。若要變更舊式工作區，請完成下列步驟：

1.  從 {{site.data.keyword.conversationshort}} 首頁中，按一下**技能**。

    使用 {{site.data.keyword.conversationshort}} 服務正式發行版本所建立的所有工作區，都會顯示成對話技能磚。
1.  按一下代表您要編輯之工作區的對話技能。
1.  視需要進行任何變更或新增至訓練資料或對話。
1.  儲存變更。

完成訓練之後，您的更新即可用於您從中呼叫服務的自訂應用程式。如需相關資訊，請參閱 [Watson 助理 API 概觀](/docs/services/assistant?topic=assistant-api-overview)。

## 限制
{: #edit-convo-workspace-cons}

使用最新版本的服務，您可以繼續執行舊版服務可執行的所有作業，但有更大的彈性。可能是您使用舊版的服務建立了工作區，且正在從您不要取代的現有應用程式呼叫它嗎？它已在個別對話上管理狀態並執行有用的功能，變成您想要繼續控制。您仍然可以使用最新版本的 /message API 來編排呼叫。優點是您不需要這麼做。在最新版本中，您可以一次支援多個具有相同基礎對話技能的整合頻道。

如果您選擇跳過建立助理的步驟，並在其中新增對話技能，則會錯失助理所提供的簡便性。也就是說，您**不能**一次透過多個整合頻道，使用單一交談與客戶互動，然後快速擴充或切換至受使用者歡迎的新頻道。

## 技能及工作區
{: #edit-convo-workspace-names}

工具中所呈現的對話技能，實際上是第 1 版工作區的封套。雖然目前第 2 版 API 沒有 API 方法可用來編寫技能，但是您可以繼續使用第 1 版 API 來編寫工作區。

- 當您開啟工具時，在 11 月 9 日之前建立的所有工作區都會顯示為技能。技能名稱取自工作區名稱。不過，如果您後來變更了技能名稱或說明，這些變更並不會影響工作區。同樣地，如果您使用 API 來編輯工作區名稱或說明，這些變更也不會影響技能。
- 從工具中，技能的 API 詳細資料會同時顯示技能 ID 和工作區 ID。
