---

copyright:
  years: 2015, 2019
lastupdated: "2019-02-28"

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

# 建立技能版本
{: #versions}

版本可協助您管理對話技能開發專案的工作流程。
{: shortdesc}

建立一個技能版本，在開發過程的關鍵點，擷取技能的訓練資料（目的和實體）和對話的 Snapshot。能夠在特定時間點儲存進行中的技能，這在您開始微調助理時特別有幫助。您經常需要進行變更並即時查看變更的影響，然後您才可以判斷變更是否改善或降低助理的效率。您可以根據測試環境部署中的發現項目，做出明智的決策，來決定是否將給定的變更部署至已部署在正式作業環境中的助理。

如果您有免費（精簡）方案，則無法建立技能版本。
{: note}

這個 2 分半鐘的視訊說明使用版本可以對您提供什麼幫助。

<iframe class="embed-responsive-item" id="youtubeplayer" title="建立技能版本" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/FDolnBxvcZ8" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

## 建立版本
{: #versions-create}

您可以使用 {{site.data.keyword.conversationshort}} 工具，一次只編輯一個對話技能版本。進行中的版本稱為*開發* 版本。

當您儲存版本時，也會儲存您套用至開發版本的所有技能設定。

若要建立對話技能版本，請遵循下列步驟：

1.  從技能的標頭中，按一下**儲存新版本**，然後說明技能的現行狀態。

    新增有效的說明可協助您以後區分多個版本。

1.  按一下**儲存**。

Snapshot 取自於現行技能，並儲存為新版本。您仍保持在技能的開發版本。您所做的全部變更都會繼續套用至開發版本，而不是套用至您所儲存的版本。若要存取您所儲存的版本，請移至**版本歷程**頁面。

## 部署技能版本
{: #versions-deploy}

1.  從技能的標頭中，按一下**版本歷程**標籤。
1.  從您要部署的版本中按一下 ![按一下以檢視動作](images/kebab-react.png) 圖示，然後選擇**指派給助理**。

    即會顯示一份您可以鏈結這個版本的助理清單。該清單僅限於那些不具有任何相關聯技能的助理，或與此技能的不同版本相關聯的助理。
1.  按一下一個以上助理的勾選框，然後按一下**指派**。

追蹤此版本何時部署至助理以及持續多久時間。您可能會想分析在使用者與這個特定技能版本之間所發生的使用者交談。您可以從**分析**頁面取得此資訊。不過，當您挑選資料來源時，不會列出版本。您必須選擇您已部署此版本的助理名稱。然後，您可以過濾度量資料，僅顯示那些在此技能版本已部署至助理的時間範圍的開始與結束日期之間發生的交談。
{: important}

## 技能版本限制
{: #skill-version-limits}

您可以為單一技能建立的版本數目，取決於您的 {{site.data.keyword.conversationshort}} 方案。

|服務方案                             | 每個技能的版本數 |
|------------------|-------------------:|
|超值                                 |                 50 |
|加值                                 |                 10 |
|標準                                 |                 10 |
|精簡              |0 |
{: caption="服務方案詳細資料" caption-side="top"}

## 交換技能
{: #versions-swap-skills}

如果您發現舊版的技能在辨識和處理客戶需求方面做得比新版本更好，您可以交換鏈結至助理的技能來改用舊版。

請遵循您用來[部署技能版本](#versions-deploy)的相同步驟來變更已鏈結至助理的版本。

## 存取儲存的版本
{: #versions-view}

檢視已儲存版本的唯一方式是使用已儲存的版本改寫技能的進行中開發版本。（但是，不在您儲存現行開發版本中所完成的任何工作之前）。

您無法編輯已儲存的版本。若要達到相同的目標，您可以使用已儲存的版本作為新版本的基礎，來納入您要做的所有變更。若要從已儲存的版本開始開發工作，請使用已儲存的版本改寫技能的進行中開發版本。

1.  儲存自您前次建立版本以來對技能所做的全部變更。

    立即儲存一個技能版本。否則，當您遵循這些步驟時，您所做的一切將會流失。{: important}

1.  從您要編輯的版本中，按一下**技能動作** ![技能動作](images/kebab-react.png) 圖示，然後選擇**複製到開發**，並確認動作。

    頁面會重新整理，以回復到當初建立版本時該技能所處的狀態。

如果您想要儲存您對此版本所做的全部變更，您必須將該技能另存為新版本。您無法將變更套用至已儲存的版本。

當您從助理頁面按一下技能磚來開啟技能時，即會顯示技能的開發版本。即使您已建立新版本與助理的關聯，當您存取該技能時，仍會開啟它的開發版本。
{: important}
