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

# 고가용성 및 재해 복구
{: #recovery}

{{site.data.keyword.conversationfull}}는 여러 {{site.data.keyword.cloud}} 위치(예: Dallas 및 Washington, DC)에서 항상 사용이 가능합니다. 그러나 전체 위치에 영향을 주는 잠재적 재해로부터의 복구에는 계획과 준비가 필요합니다.
{: shortdesc}

사용자는 {{site.data.keyword.conversationshort}}에 대한 사용자의 구성, 사용자 정의 및 사용법에 대해 이해하고 있어야 합니다. 또한 새 위치에서 서비스 인스턴스를 다시 작성하고 임의의 위치에서 데이터를 복원할 준비가 되어 있어야 합니다. 자세한 정보는 [작동 중단이 발생하지 않도록 하는 방법![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](/docs/overview?topic=overview-zero-downtime#zero-downtime){: new_window}을 참조하십시오. 

## 고가용성
{: #recovery-ha}

{{site.data.keyword.conversationshort}}은 단일 실패 지점이 전혀 없는 고가용성을 지원합니다. 서비스는 {{site.data.keyword.cloud_notm}}에서 제공하는 다중 구역 지역 기능을 사용하여 고가용성을 자동으로 투명하게 달성합니다. 

{{site.data.keyword.cloud_notm}}를 통해서 단일 위치 내에서 단일 실패 지점을 공유하지 않는 다중 구역이 지원됩니다. 또한 지역 내 구역에서 자동 로드 밸런싱을 제공합니다. 

## 재해 복구
{: #recovery-dr}

재해 복구는 {{site.data.keyword.cloud_notm}} 위치에 데이터의 잠재적 손실을 포함하는 심각한 장애가 발생하는 경우 문제가 될 수 있습니다. 다중 구역 지역 기능은 여러 위치에서 사용할 수 없으므로 사용 불가능하게 되면 IBM이 위치를 다시 온라인으로 되돌릴 때까지 기다려야 합니다. 실패로 인해 기본 데이터 서비스가 손상되면 IBM이 이러한 데이터 서비스를 복원할 때까지 기다려야 합니다.

치명적인 장애가 발생하면 IBM은 데이터베이스 백업에서 데이터를 복구할 수 없습니다. 이 경우, 서비스 인스턴스를 가장 최근의 상태로 되돌리려면 데이터를 복원해야 합니다. 데이터를 동일하거나 다른 위치에 복원할 수 있습니다.

재해 복구 계획에는 {{site.data.keyword.cloud_notm}}에서 유지보수되는 모든 데이터를 복원하기 위한 인식, 보존, 준비가 포함됩니다. 서비스 인스턴스를 백업하는 방법에 대한 정보는 [데이터 백업 및 복원](/docs/services/assistant?topic=assistant-backup)을 참조하십시오. 
