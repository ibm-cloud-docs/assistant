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

# 高可用性和灾难恢复
{: #recovery}

{{site.data.keyword.conversationfull}} 在多个 {{site.data.keyword.cloud}} 位置（例如，达拉斯和华盛顿）具有高可用性。但是，要从影响整个位置的潜在灾难中恢复，需要进行规划和准备。
{: shortdesc}

您负责了解 {{site.data.keyword.conversationshort}} 的配置、定制和使用情况。您还应负责准备好在新位置重新创建服务实例，并在任何位置复原数据。有关更多信息，请参阅[如何确保停机时间为零？![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](/docs/overview?topic=overview-zero-downtime#zero-downtime){: new_window}。

## 高可用性
{: #recovery-ha}

{{site.data.keyword.conversationshort}} 支持高可用性，没有单点故障。该服务使用 {{site.data.keyword.cloud_notm}} 提供的多专区区域功能，自动、透明地实现高可用性。

{{site.data.keyword.cloud_notm}} 支持不在单个位置中分担单点故障的多个专区。它还在一个区域中的各个专区之间提供自动负载均衡。

## 灾难恢复
{: #recovery-dr}

如果 {{site.data.keyword.cloud_notm}} 位置遇到重大故障，包括潜在的数据丢失，那么灾难恢复可能会成为问题。由于多专区区域功能在多个位置不可用，因此如果某个位置变为不可用，您必须等待 IBM 将该位置恢复为联机。如果底层数据服务受到故障影响，那么还必须等待 IBM 复原这些数据服务。

如果发生灾难性故障，IBM 可能无法通过数据库备份来恢复数据。在这种情况下，您需要复原数据，以将服务实例恢复为其最新状态。您可以将数据复原到同一位置，也可以复原到其他位置。

灾难恢复计划应包括了解、保留和准备好复原在 {{site.data.keyword.cloud_notm}} 上保留的所有数据。有关如何备份服务实例的信息，请参阅[备份和复原数据](/docs/services/assistant?topic=assistant-backup)。
