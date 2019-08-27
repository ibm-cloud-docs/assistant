---

copyright:
  years: 2015, 2019
lastupdated: "2019-05-28"

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

# 使用內容型錄
{: #catalog}

***內容型錄*** 提供一種簡單的方式，將一般目的新增至 {{site.data.keyword.conversationshort}} 對話技能。
{: shortdesc}

您從型錄中新增的目的是用來提供起點。新增或編輯型錄目的，以針對您的使用案例進行修改。

## 將內容型錄新增至對話技能
{: #catalog-add}

1.  開啟對話技能，然後按一下**內容型錄**標籤。

1.  選取內容型錄（例如*銀行業*），以查看與它一起提供的目的。

    ![顯示可用型錄的畫面擷取](images/catalog_overview.png)

    您將會看到型錄中所含目的的相關資訊。

    ![顯示「銀行業」種類目的的畫面擷取](images/catalog_open.png)

    從內容型錄中新增的目的會依其名稱來與其他目的區分。每個目的名稱前面都會加上內容型錄名稱。

1.  選取 ![關閉箭頭](images/close_arrow.png)，以回到**內容型錄**標籤。

1.  接下來，按一下`新增至技能`按鈕，以將內容型錄新增至對話技能。

1.  現在，選取**目的**標籤，並驗證已新增且可以使用來自型錄的目的。

    ![顯示「目的」標籤上列出之「銀行業」目的的畫面擷取](images/catalog_intents.png)

系統會開始根據新資料自行訓練。

在您將型錄新增至技能之後，這些目的會變成訓練資料的一部分。如果 IBM 對內容型錄進行後續更新，則這些變更不會自動套用至您從型錄中新增的任何目的。
{: note}

## 編輯內容型錄範例
{: #catalog-edit-content}

如同任何其他目的，在將內容型錄目的新增至您的技能之後，您可以對它們進行下列變更：

- 重新命名目的。
- 刪除目的。
- 新增、編輯或刪除範例。
- 將範例移至不同的目的。
