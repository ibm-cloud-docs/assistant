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

# 개선 컴포넌트 정보

{{site.data.keyword.conversationshort}}의 개선 컴포넌트는 작업공간 사용자와의 대화 히스토리를 제공합니다. 이 히스토리를 사용하여 사용자 입력에 대한 작업공간의 이해를 향상시킬 수 있습니다.
{: shortdesc}

**개선** 패널 섹션:

* [개요](logs_oview.html): 작업공간과 사용자의 상호작용 요약입니다.
* [사용자 대화](logs_convo.html): 사용자 표현의 목록입니다. 개이별 사용자 표현을 보면서 인텐트와 엔티티를 업데이트할 수 있습니다.**참고**: 단일 사용자 대화는 여러 표현들로 구성될 수 있습니다.
* [권장사항](logs_recommend.html): 시스템을 개선하는 방법입니다. 프리미엄 사용자만 사용 가능합니다.

## 작업 공간에서 개선
{: #deploy_id}

표현 데이터를 사용하여 작업공간에서 개선할 수 있는 방법을 이해하려면 {{site.data.keyword.conversationshort}}서비스와 관련된 다음 정의를 검토하십시오.

* ***인스턴스***: 고유한 신임 정보로 액세스할 수 있는 {{site.data.keyword.conversationshort}} 배치. {{site.data.keyword.conversationshort}} 인스턴스는 여러 작업공간으로 구성될 수 있음
* ***작업공간***: 작업 공간은 {{site.data.keyword.conversationshort}} 컨텐츠의 모델 중 하나입니다. 이것은 봇과 유사합니다.
* ***작업공간 ID***: 작업공간의 고유 ID
* ***배치 ID***: 배치 ID는 표현이 나온 배치 환경을 식별하는 데 도움이 되는 사용자 표현과 함께 전달되는 고유한 레이블입니다.
* ***표현***: 표현은 사용자가 작업공간에 보내는 단일 메시지입니다.

작업공간을 작성하는 것은 매우 반복적인 프로세스입니다. 작업공간을 개발하는 동안 *연습* 분할 창을 사용하여 작업공간이 테스트 입력의 올바른 의도와 엔티티를 인식하는지 확인하고 필요에 따라 수정합니다.

**개선** 패널에서는 사용자와의 실제 상호작용에 대한 정보를 보고 작업공간에서 인텐트와 엔티티가 인식되는 정확성을 향상시키기 위해 유사한 수정을 할 수 있습니다. 사용자가 질문하는 *방법*이나 무작위로 발언하는 방식을 정확히 알기가 어렵기 때문에 작업공간을 향상시키기 위해 자주 **개선** 패널을 방문하는 것이 중요합니다. 

여러 작업 공간을 포함하는 {{site.data.keyword.conversationshort}} 인스턴스의 경우, 한 작업공간의 표현 데이터를 사용하여 동일한 인스턴스 내의 다른 작업공간을 향상시키는 것이 유용한 경우가 있습니다. **참고**: {{site.data.keyword.conversationshort}} 프리미엄 사용자인 경우, 프리미엄 인스턴스는 다른 프리미엄 인스턴스에서 작업공간의 데이터를 로그에 기록할 수 있도록 선택적으로 구성할 수 있습니다. 

예를 들어, *HelpDesk*라는 {{site.data.keyword.conversationshort}} 인스턴스가 있습니다. HelpDesk 인스턴스에 프로덕션 작업공간과 개발 작업공간 모두들 가질 수 있습니다. 개발 작업공간에서 작업할 때 프로덕션 작업공간의 `Deployment ID`를 기반으로 표현을 필터링하여 프로덕션 작업공간 표현을 사용하여 개발 작업공간을 개선할 수 있습니다. 

![데이터 소스 링크](images/data_source_1.png)

개발 작업공간 내에서 수행한 편집 작업은 프로덕션 작업공간 표현을 사용하는 경우에도 개발 작업공간에만 영향을 미칩니다. 추가 정보는 [데이터 소스 선택](logs_convo.html#select-source)을 참조하십시오.

`/message` API를 사용하여 전송된 표현의 배치 ID를 지정하려면, 이 예제에서와 같이 [컨텍스트 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.ibm.com/watson/developercloud/conversation/api/v1/#send_message){: new_window}의 메타 데이터 오브젝트 내에 배치 특성을 포함하십시오. 

```
"context" : {
  "username" : "jane_doe@myemail.com",
  "member_type" : "gold",
  "metadata" : {
       "deployment": "HelpDesk-Production"
  }
}
```
{: #codeblock}
