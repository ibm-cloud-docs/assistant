---

copyright:
  years: 2015, 2018
lastupdated: "2018-02-13"

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

# 使用型錄

***型錄*** 提供一種簡單的方式，將一般目的新增至 {{site.data.keyword.conversationshort}} 服務工作區。
{: shortdesc}

## 將型錄新增至工作區
{: #add-catalog}

使用 {{site.data.keyword.conversationshort}} 工具來新增型錄。

1.  在 {{site.data.keyword.conversationshort}} 工具中，開啟工作區，然後選取導覽列中的**型錄**標籤。如果看不到**型錄**，請使用 ![功能表](images/Menu_16.png) 功能表來開啟頁面。

1.  選取型錄（例如*計費*），以查看與它一起提供的目的。

    ![顯示可用型錄的畫面擷取](images/catalog_overview.png)

    您將會看到*計費* 種類中所定義之目的的相關資訊。

    ![顯示「計費」種類目的的畫面擷取](images/catalog_open.png)

    從型錄新增的目的會依其命名慣例來與其他目的區分；在此案例為 `#Billing_ . . .`

1.  選取 ![關閉箭頭](images/close_arrow.png)，以回到**型錄**標籤。

1.  接下來，請按一下`新增至機器人`按鈕，以將*計費* 型錄新增至工作區。您將會看到一則指出已將*計費* 目的新增至工作區的訊息。

    ![顯示「新增至機器人」按鈕的畫面擷取](images/catalog_addtobot.png)

1.  接下來，請選取**目的**標籤，並驗證已將*計費* 目的新增至工作區。

    ![顯示「目的」標籤上所列「計費」目的的畫面擷取](images/catalog_intents.png)

### 結果

*計費* 型錄中的目的已新增至工作區的**目的**標籤，而且系統會開始利用新資料來進行自我訓練。

## 編輯型錄範例

與任何其他目的類似，將*計費* 型錄目的新增至工作區之後，您即可進行下列變更：

- 重新命名目的。
- 刪除目的。
- 新增、編輯或刪除範例。
- 將範例移至不同的目的。

您可以按 Tab 鍵從目的名稱移至每一個範例，並在選擇時編輯範例。

若要移動或刪除範例，請藉由選取此勾選框來選取範例，然後選取**移動**或**刪除**。

  ![顯示如何移動或刪除範例的畫面擷取](images/catalog_edit.png)
