---

copyright:
  years: 2015, 2019
lastupdated: "2019-01-11"

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

# 啟動對話
{: #dialog-start}

您無法使用內建的 welcome 節點，以相同方式啟動所有整合的對話。請改用這個暫行解決方法。
{: shortdesc}

顯示您針對對話中 welcome 節點所定義的回應，以從工具內的「試用」窗格，以及從「預覽鏈結」整合中的會談小組件，起始交談。不過，它不會從許多其他頻道整合顯示，因為在使用者所啟動的對話流程中，會跳過含 `welcome` 特殊條件的節點。而且，已部署的助理通常會等待使用者起始與它們的交談，而不是以其他方式來進行交談。

不像 `welcome` 特殊條件，`conversation_start` 特殊條件一律在啟動對話時觸發。您可以使用含這兩個特殊條件（`welcome` 及 `conversation_start`）的節點組合，以一致的方式管理對話的啟動。

請完成下列步驟來管理對話啟動：

1.  在 Welcome 節點上新增對話節點，當您建立對話時，該節點會自動新增至對話樹狀結構的頂端。

1.  將這個新增節點的節點條件設為 `conversation_start`，這是先前所說明的特殊條件。

1.  在 `conversation_start` 節點中，定義您想使用對話的預設值來設定的任何環境定義變數。

1.  不要定義此節點的文字回應。

1.  將此節點配置成跳至對話樹狀結構中其正下方的 `Welcome` 節點，然後選取**如果機器人辨識（條件）**。

![對話樹狀結構的擷取畫面，其中 conversation_start 節點跳至其下方的 welcome 節點。](images/dialog-start.png)

此設計會產生一個對話，其運作方式如下：

- 不論整合類型為何，都會處理 `conversation_start` 節點，這表示已起始設定您在其中定義的任何環境定義變數。
- 在助理從中啟動對話流程的整合中，會觸發 `Welcome` 節點，並顯示其文字回應。
- 在使用者從中啟動對話流程的整合中，會先評估使用者的第一個輸入，然後再由可提供最佳回應的節點進行處理。
