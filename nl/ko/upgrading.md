---

copyright:
  years: 2015, 2018
lastupdated: "2018-02-09"

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

# 업그레이드

서비스 플랜 업그레이드 방법에 관해 학습합니다.
{: shortdesc}

## 플랜 업그레이드
{: #plan-upgrade}

라이트 플랜에서 {{site.data.keyword.conversationshort}} 서비스를 광범위하게 탐색할 수 있습니다. 하지만 수행할 수 있는 작업에는 한계가 있습니다.

### 아티팩트별 한계
플랜당 아티팩트 한계에 대한 정보는 다음 주제를 참조하십시오.

- [작업공간](configure-workspace.html#workspace-limits)
- [대화 상자 노드](dialog-build.html#dialog-node-limits)
- [인텐트](intents.html#intent-limits)
- [엔티티](entities.html#entity-limits)
- [로그](logs_convo.html#log-limits)

### API 호출 한계
인스턴스마다 허용되는 API 호출의 수는 서비스 플랜에 따라 다릅니다. 세부사항은 플랜 설명을 참조하십시오.

라이트 플랜이 있고 API 호출 한계에 도달하지만 예상보다 적은 호출을 작성했다고 로그에 표시되는 경우, 라이트 플랜이 7일 동안만 로그 정보를 저장한다는 사실을 기억하십시오.

플랜을 업그레이드하려면 다음 단계를 완료하십시오.

1.  {{site.data.keyword.Bluemix_notm}} 메뉴에서 **서비스** > **대시보드**를 선택하십시오.
1.  업그레이드할 서비스 인스턴스를 선택하여 여십시오.
1.  탐색 분할창에서 **플랜**을 클릭하십시오.
   여기서 현재 플랜과 사용 가능한 다른 플랜 옵션을 보고 변경할 수 있습니다.

등록에 대한 일반적인 질문의 응답은 [청구 및 사용 관리(![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘"))](/docs/billing-usage/how_charged.html){: new_window}를 참조하십시오.

다음 링크에서 IBM Cloud에서 호스팅하는 서비스에 대해 자세히 알아볼 수 있습니다. 

- [Cloud Services terms](http://www.ibm.com/software/sla/sladb.nsf/sla/saas)
- [Cloud Services data security and privacy](http://www.ibm.com/software/sla/sladb.nsf/sla/csdsp)

## 작업공간 업그레이드
{: #upgrade-workspace}

{{site.data.keyword.conversationshort}} 서비스는 기능을 정기적으로 추가하고 업데이트합니다. 이러한 변경사항 중 일부는 자동으로 작업공간에 적용되는 반면, 영향이 큰 업데이트에는 수동 업데이트가 필요합니다.

업그레이드 아이콘(![업그레이드 아이콘](images/upgrade.png))이 표시된 경우에만 작업공간에 대한 업그레이드를 사용할 수 있습니다. 

**참고**: 작업공간을 업그레이드한 후 작업공간을 이전 버전으로 되돌릴 수 없습니다. 

작업공간을 업그레이드하려면 다음 단계를 완료하십시오.
1.  [작업공간 복제](configure-workspace.html#exporting-and-copying-workspaces).
2.  복제 작업공간 업그레이드.

     작업공간을 업그레이드하는 경우 최신 버전의 API가 도구에서 사용 가능하며 "연습" 분할창이 최신 기능을 사용하도록 시작됩니다.
3.  업그레이드된 작업공간을 테스트하십시오. 
4.  업그레이드가 애플리케이션에 미치는 영향을 이해하기 위해 복제 작업공간을 평가한 후 기본 작업공간에 업그레이드를 적용하십시오.
5.  최신 API 버전을 사용하도록 메시지 API 호출을 변경하여 애플리케이션을 업그레이드하십시오.
