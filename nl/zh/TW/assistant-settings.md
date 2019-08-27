---

copyright:
  years: 2015, 2019
lastupdated: "2019-07-29"

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

# 變更閒置逾時設定 ![僅限加值或超值方案](images/plus.png)
{: #assistant-settings}

使用者透過其中一個內建整合與助理互動時，如果使用者在特定時段內未與助理互動，則會談階段作業會結束。您可以藉由變更閒置逾時設定，指定等待多長時間後重設會談階段作業。
{: shortdesc}

重設會談階段作業時，對話會遺失先前與使用者交換期間儲存的任何環境定義資訊。例如，如果對話詢問使用者的姓名，然後在對話的其餘部分中依該名稱呼叫使用者，則在會談階段作業結束並開始新的階段作業之後，對話將再次從詢問使用者的姓名開始。

## 階段作業限制
{: #assistant-settings-session-limits}

容許的閒置逾時長度根據服務實例方案類型而有所不同。下表列出限制。

|服務方案          |會談階段作業預設閒置期間|會談階段作業閒置期間上限|
|--------------|--------------------------------:|----------------------------:|
|超值              |       60 分鐘 | 24 小時  |
|加值              |       60 分鐘 | 24 小時  |
|標準              |        5 分鐘 |        5 分鐘 |
|加值試用   |        5 分鐘 |        5 分鐘 |
|精簡*            |        5 分鐘 |        5 分鐘 |
{: caption="服務方案詳細資料" caption-side="top"}

## 變更設定
{: #assistant-settings-timeout-task}

1.  按一下助理的功能表，然後選擇**設定**。

1.  變更**逾時限制**欄位中指定的時間。

1.  關閉「設定」頁面。這將自動儲存您的變更。
