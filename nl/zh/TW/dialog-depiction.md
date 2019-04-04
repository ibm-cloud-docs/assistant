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

# 對話描述
{: #dialog-depiction}

![具有範例內容的範例對話樹狀結構](images/dialog-depiction-full.png)

此圖顯示以圖形使用者介面對話編輯器工具建置的對話樹狀結構模型。它包含兩個根對話節點。一般對話樹狀結構可能有好多節點，但這個描述讓您一瞥節點子集可能看起來像什麼。

- 第一個根節點設定目的值的條件。它有兩個子節點，各設定一個實體值的條件。第二個子節點定義兩個回應。如果環境定義變數的值符合條件中指定的值，則會將第一個回應傳回給使用者。否則，會傳回第二個回應。

  這個標準類型的節點有助於擷取特定主題的相關問題，然後在根回應中，詢問子節點所處理的後續問題。例如，它可能會辨識關於折扣的使用者問題，並詢問有關該使用者是否為公司具有特殊折扣安排的任何協會成員的後續問題。子節點會根據使用者如何回答有關協會成員資格的問題來提供不同的回應。

- 第二個根節點是含空位的節點。它也會設定目的值的條件。它會定義一組空位，您想要從使用者收集的每段資訊都有一個空位。每個空位都會詢問一個問題，以從使用者獲得答案。它會在使用者對提示的回覆中尋找特定的實體值，該值隨後會儲存在空位環境定義變數中。

  此類型的節點有助於收集您代表使用者執行交易時可能需要的詳細資料。例如，如果使用者的目的是預訂航班，則空位可以收集起點和目的地位置資訊、旅行日期等等。
