---

copyright:
  years: 2015, 2018
lastupdated: "2018-01-26"

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

# 使用 API 修改對話

{{site.data.keyword.conversationshort}} REST API 支援以程式設計方式修改對話，而不使用 {{site.data.keyword.conversationshort}} 工具。您可以使用 /dialog_nodes API 來建立、刪除或修改對話節點。

請記住，對話是交互連接節點的樹狀結構，且必須符合某些規則才有效。這表示您對對話節點所做的任何變更都可能會對其他節點或對話的結構造成重疊顯示影響。使用 /dialog_nodes API 修改對話之前，請確定您瞭解所做的變更對其餘對話的影響。您可以藉由匯出現行對話所在的工作區，來進行現行對話的備份。如需詳細資料，請參閱[匯出及複製工作區](configure-workspace.html#exporting-and-copying-workspaces)。

有效的對話一律會滿足下列準則：

- 每一個對話節點都有唯一的 ID（`dialog_node` 內容）。
- 子節點知道其母節點（`parent` 內容）。不過，母節點並不知道其子項。
- 節點知道其緊鄰的上一個同層級（如果有的話）（`previous_sibling` 內容）。這表示共用相同母項的所有同層級會形成一份鏈結的清單，而且每一個節點都指向上一個節點。
- 給定母項中只能有一個子項是第一個同層級（表示其 `previous_sibling` 為空值）。
- 節點無法指向作為不同母項之子項的上一個同層級。
- 兩個節點不得指向相同的上一個同層級。
- 節點可以指定另一個接下來要執行的節點（`next_step` 內容）。
- 節點不能是它自己的母項或它自己的同層級。
- 節點必須要有包含下列其中一個值的 type 內容。如果未指定 type 內容，則類型是 `standard`。

  - `event_handler`：針對訊框節點或個別空位節點所定義的處理程式。

    從工具中，您可以按一下含空位的節點中的**管理處理程式**鏈結，來定義訊框節點處理程式。（工具使用者介面不會公開空位層次事件處理程式，但您可以透過 API 定義一個空位層次事件處理程式。）

  - `frame`：有一個以上 `slot` 類型之子節點的節點。必須先填入所有必要子空位節點，服務才能結束訊框節點。

    在工具中，訊框節點類型會呈現為含空位的節點。包含空位的節點會呈現為 type=`frame`的節點。它是每一個空位的母節點，空位呈現為 `slot` 類型的子節點。

  - `response_condition`：條件式回應。

    在工具中，您可以將一個以上的條件式回應新增至節點。在基礎 JSON 中，您定義的每一個條件式回應都會呈現為 type=`response_condition` 的個別節點。

  - `slot`：`frame` 類型之節點的子節點。

    在工具中，此節點類型呈現為新增至單一節點之多個空位的其中一個。在 JSON 中，此單一節點會呈現為 `frame` 類型的母節點。

  - `standard`：一般對話節點。這是預設類型。

- 針對 `slot` 類型且具有相同母節點的節點，同層級順序（由 `previous_sibling` 內容所指定）會反映空位的處理順序。
- `slot` 類型的節點必須具有 `frame` 類型的母節點。
- `frame` 類型的節點必須至少要有一個 `slot` 類型的子節點。
- `response_condition` 類型的節點必須具有 `standard` 或 `frame` 類型的母節點。
- `response_condition` 及 `event_handler` 類型的節點不能有子項。
- `event_handler` 類型的節點也必須具有 `event_name` 內容，此內容包含下列其中一個值以識別節點事件的類型：

  - `filled`：定義如果使用者提供的值符合空位之*檢查* 欄位中指定的條件並已填入空位時要執行的動作。只有在已針對空位定義「找到」條件時，才會有具有此名稱的處理程式。
  - `focus`：定義要顯示的問題，用來提示使用者提供空位所需的資訊。只有在需要空位時，才會有具有此名稱的處理程式。
  - `generic`：定義要監看的條件，以處理使用者在填入空位或含空位之節點時可能會詢問的無關問題。
  - `input`：更新訊息環境定義，以包含環境定義變數，以及收集自使用者以填入空位的值。訊框節點中的每一個空位都必須要有具有此名稱的處理程式。
  - `nomatch`：定義如果使用者的空位提示回應未包含有效值時要執行的動作。只有在已針對空位定義「找不到」條件時，才會有具有此名稱的處理程式。

  下圖說明工具使用者介面中，您定義針對每一個具名事件所觸發之程式碼的位置。

  ![編寫具名事件處理程式所觸發之程式碼的使用者介面位置](images/api-event-handlers.png)

- `event_handler` 類型且 event_name 為 `generic` 的節點可以有 `slot` 或 `frame` 類型的母項。
- `event_handler` 且 event_name 為 `focus`、`input`、`filled` 或 `nomatch` 類型的節點必須要有 `slot` 類型的母項。
- 如果多個具有相同 event_name 的 event_handler 與相同母節點相關聯，則同層級順序會反映事件處理程式的執行順序。
- 不論節點定義的位置為何，具有相同母空位節點的 `event_handler` 節點的執行順序會相同。會依 event_name 的下列順序觸發事件：

  1. focus
  1. input
  1. filled
  1. generic*
  1. nomatch

  *如果已針對此空位或母訊框定義 event_name 為 `generic` 的 `event_handler`，則它會在 filled 與 nomatch event_handler 節點之間執行。

下列範例顯示各種修改如何導致可能的重疊顯示變更。

## 建立節點
{: #create-node}

請考量下列簡單對話樹狀結構：

![範例對話](images/dialog_api_1.png)

我們可以藉由向具有下列內文的 /dialog_nodes 提出 POST 要求，來建立新的節點：

```json
{
  "dialog_node": "node_8"
}
```

對話現在看起來像這樣：

![範例對話 2](images/dialog_api_2.png)

因為已建立 **node_8**，但未指定 `parent` 或 `previous_sibling` 的值，所以它現在是對話中的第一個節點。請注意，除了已建立 **node_8** 之外，服務也修改了 **node_1**，使其 `previous_sibling` 內容指向新的節點。

您可以指定母項及上一個同層級，以在對話中的它處建立節點：

```json
{
  "dialog_node": "node_9",
  "parent": "node_2",
  "previous_sibling": "node_5"
}
```

您指定給 `parent` 及 `previous_node` 的值必須有效：

- 兩個值都必須參照現有節點。
- 指定的母項必須與上一個同層級的母項相同（如果上一個同層級沒有母項，則為 `null`）。
- 母項不能是 `response_condition` 或 `event_handler` 類型的節點。

產生的對話看起來像這樣：

![範例對話 3](images/dialog_api_3.png)

除了建立 **node_9** 之外，服務也會自動更新 *node_6* 的 `previous_sibling` 內容，讓它指向新的節點。

## 將節點移至不同的母項
{: #change-parent}

使用具有下列內文的 POST /dialog_nodes/node_5 方法，將 **node_5** 移至不同的母項：

```json
{
  "parent": "node_1"
}
```

指定的 `parent` 值必須有效：
- 它必須參照現有節點。
- 它不得參照正在修改的節點（節點不能是它自己的母項）。
- 它不得參照正在修改之節點的後代。
- 它不得參照 `response_condition` 或 `event_handler` 類型的節點。

這會導致如下的已變更結構：

![範例對話 4](images/dialog_api_4.png)

這裡發生幾個事件：
- 將 **node_5** 移至其新的母項之後，**node_7** 會隨著它一起移動（因為 **node_7** 的 `parent` 值未變更）。在移動節點時，該節點仍然會保留其所有後代。
- 因為我們未指定 **node_5** 的 `previous_sibling` 值，所以它現在是 **node_1** 下的第一個同層級。
- **node_4** 的 `previous_sibling` 內容已更新為 `node_5`。
- **node_9** 的 `previous_sibling` 內容已更新為 `null`，因為它現在是 **node_2** 下的第一個同層級。

## 重新排序同層級
{: #change-sibling}

現在，讓我們將 **node_5** 設為第二個同層級，而非第一個同層級。作法是使用具有下列內文的 POST /dialog_nodes/node_5 方法：

```json
{
  "previous_sibling": "node_4"
}
```

在修改 `previous_sibling` 時，新值必須有效：
- 它必須參照現有節點
- 它不得參照正在修改的節點（節點不能是它自己的同層級）。
- 它必須參照相同母項的子項（所有同層級都必須具有相同的母項）

結構變更如下：

![範例對話 5](images/dialog_api_5.png)

請注意，**node_7** 同樣會跟隨其母項。此外，**node_4** 已經過修改，使得其 `previous_sibling` 是 `null`，因為它現在是第一個同層級。

## 刪除節點
{: #delete-node}

現在讓我們使用 DELETE /dialog_nodes/node_1 方法來刪除 **node_1**。

結果如下：

![範例對話 6](images/dialog_api_6.png)

請注意，已全部刪除 **node_1**、**node_4**、**node_5** 及 **node_7**。在刪除節點時，也會刪除該節點的所有後代。因此，如果刪除根節點，則實際上是刪除對話樹狀結構的整個分支。對已刪除節點的任何其他參照（例如 `next_step` 參照）都會變更為 `null`。

此外，**node_2** 會更新為指向 **node_8**，以作為其新的上一個同層級。

## 重新命名節點
{: #rename-node}

最後，使用具有下列內文的 POST /dialog_nodes/node_2 方法，來重新命名 **node_2**：

```json
{
  "dialog_node": "node_X"
}
```

![範例對話 7](images/dialog_api_7.png)

對話的結構未變更，但同樣地，多個節點已修改為反映變更過的名稱：

- **node_9** 及 **node_6** 的 `parent` 內容
- **node_3** 的 `previous_sibling` 內容

對已刪除節點的任何其他參照（例如 `next_step` 參照）也會變更。
