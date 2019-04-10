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

# 업그레이드
{: #upgrade}

서비스 플랜 업그레이드 방법에 관해 학습합니다.
{: shortdesc}

## 플랜 업그레이드
{: #upgrade-plan}

{{site.data.keyword.conversationshort}} [서비스 플랜 옵션 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.ibm.com/cloud/watson-assistant/pricing/){: new_window}을 탐색하여 가장 적합한 플랜을 결정할 수 있습니다.

Cloud Foundry 기반 인스턴스를 플러스 플랜으로 업그레이드할 수 없습니다. 업그레이드하기 전에 리소스 그룹을 사용하도록 인스턴스를 마이그레이션해야 합니다. 세부사항은 [마이그레이션](/docs/services/assistant?topic=assistant-migrate)을 참조하십시오.
{: note}

플랜을 업그레이드하려면 다음 단계를 완료하십시오.

1.  {{site.data.keyword.Bluemix_notm}} 메뉴에서 **플랜 업그레이드**를 선택하십시오. 여기서 현재 플랜과 사용 가능한 다른 플랜 옵션을 보고 변경할 수 있습니다.

구독에 대한 일반적인 질문의 응답은 [청구 및 사용량 관리 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](/docs/billing-usage?topic=billing-usage-charges){: new_window}를 참조하십시오.

아직 궁금하신 사항이 있습니까? [IBM Sales ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://www-01.ibm.com/marketing/iwm/dre/signup?source=urx-20970){: new_window}에 문의하시기 바랍니다.

## 대화 스킬 업그레이드
{: #upgrade-skill}

{{site.data.keyword.conversationshort}} 서비스는 기능을 정기적으로 추가하고 업데이트합니다. 이러한 변경사항 중 일부는 자동으로 대화 스킬에 적용되는 반면 영향이 큰 업데이트의 경우 스킬에서 사용되는 기계 학습 모델을 수동으로 업데이트해야 합니다.

업그레이드 아이콘(![업그레이드 아이콘](images/upgrade.png))이 표시되는 경우에만 대화 스킬에 대한 업그레이드가 사용 가능합니다.

대화 스킬을 업그레이드하려면 다음 단계를 완료하십시오.

1.  [대화 스킬의 사본을 다운로드](/docs/services/assistant?topic=assistant-skill-add#skill-add-download-skill)한 후 새 스킬로 가져오십시오.
2.  대화 스킬의 새 사본을 업그레이드하십시오.

    스킬을 업그레이드하면 도구에서 최신 버전의 API가 사용으로 설정되며 "시험 사용" 분할창이 최신 기능을 사용하기 시작합니다.
3.  업그레이드된 스킬을 테스트하십시오.
4.  업그레이드된 스킬을 평가하여 업그레이드가 애플리케이션에 미치는 영향을 파악한 후 기본 대화 스킬에 업그레이드를 적용하십시오.
5.  애플리케이션을 업그레이드하십시오. 이를 수행하려면 최신 API 버전을 지정하는 데 사용하는 메시지 API 호출을 변경하십시오. API 버전 세부사항은 [릴리스 정보](/docs/services/assistant?topic=assistant-release-notes)를 참조하십시오.
