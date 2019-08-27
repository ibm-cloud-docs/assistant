---

copyright:
  years: 2015, 2019
lastupdated: "2019-04-12"

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

# 高可用性和災難回復
{: #recovery}

{{site.data.keyword.conversationfull}} 在多個 {{site.data.keyword.cloud}} 位置（例如，達拉斯和華盛頓特區）具有高可用性。不過，從影響整個位置的潛在災難回復需要規劃和準備。
{: shortdesc}

您負責瞭解 {{site.data.keyword.conversationshort}} 的配置、自訂和使用。同時負責備妥以在新位置重建服務實例，以及在任何位置還原資料。如需相關資訊，請參閱[如何確保運作零中斷？![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](/docs/overview?topic=overview-zero-downtime#zero-downtime){: new_window}。

## 高可用性
{: #recovery-ha}

{{site.data.keyword.conversationshort}} 支援高可用性，沒有單一失敗點。該服務使用 {{site.data.keyword.cloud_notm}} 提供的多區域地區特性，自動、透明地實現高可用性。

{{site.data.keyword.cloud_notm}} 會啟用未在單一位置內共用單一失敗點的多個區域。它也會提供地區內各個區域的自動負載平衡。

## 災難回復
{: #recovery-dr}

如果 {{site.data.keyword.cloud_notm}} 位置遇到包括潛在資料流失的重大故障，則災難回復可能會變成問題。因為多區域地區特性無法在多個位置使用，所以如果某個位置變為無法使用，您就必須等待 IBM 將該位置恢復為線上。如果失敗損壞基礎資料服務，則您還是必須等待 IBM 還原這些資料服務。

如果發生災難性失效，IBM 可能無法從資料庫備份來回復資料。在此情況下，您需要還原資料，以將服務實例回復至其最新狀態。您可以將資料還原至相同或不同的位置。

災難回復方案包括瞭解、保留和準備好還原在 {{site.data.keyword.cloud_notm}} 上維護的所有資料。如需如何備份服務實例的相關資訊，請參閱[備份和還原資料](/docs/services/assistant?topic=assistant-backup)。
