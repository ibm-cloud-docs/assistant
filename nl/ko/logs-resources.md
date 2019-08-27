---

copyright:
  years: 2015, 2019
lastupdated: "2019-05-28"

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

# 고급 태스크
{: #logs-resources}

로그 데이터에 액세스하고 분석하는 데 사용할 수 있는 API 및 기타 도구에 대해 알아봅니다.
{: shortdesc}

## API
{: #logs-resources-api}

`/logs` API를 사용하여 사용자와 어시스턴트 간에 발생한 대화의 기록에서 이벤트를 나열할 수 있습니다. 자세한 API 참조 문서는 [로그 이벤트 나열 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://cloud.ibm.com/apidocs/assistant#list-log-events-in-a-workspace)을 참조하십시오.

로그가 저장되는 일 수는 서비스 플랜 유형별로 다릅니다. 세부사항은 [로그 한계](/docs/services/assistant?topic=assistant-logs#logs-limits)를 참조하십시오.

로그를 내보내고 로그를 CSV 형식으로 변환하기 위해 실행할 수 있는 Python 스크립트를 얻으려면 [Watson Assistant GitHub ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/watson-developer-cloud/community/blob/master/watson-assistant/export_logs.py) 저장소에서 `export_logs.py` 파일을 다운로드하십시오.

## 로그 관련 용어
{: #logs-resources-terminology}

먼저, {{site.data.keyword.conversationshort}} 로그와 연관된 용어의 정의를 검토하십시오.

- ***어시스턴트***: {{site.data.keyword.conversationshort}} 컨텐츠를 구현하는 애플리케이션이며 '챗봇'이라고도 합니다.
- ***대화***: 개별 사용자가 어시스턴트에 전송하는 메시지와 어시스턴트가 반송하는 메시지로 구성되는 메시지 세트입니다.
- ***대화 ID***: 관련 메시지 교환을 함께 링크하기 위해 개별 메시지 호출에 추가되는 고유 ID입니다. V1 버전의 {{site.data.keyword.conversationshort}} API를 사용 중인 앱 개발자가 컨텍스트 오브젝트의 메타데이터에 있는 ID를 포함하여 대화의 메시지 호출에 이 값을 추가합니다.
- ***고객 ID***: 나중에 고객이 해당 데이터의 제거를 요청하는 경우에 삭제될 수 있도록 컴퓨터 데이터에 레이블을 지정하는 데 사용될 수 있는 고유 ID입니다.
- ***배치 ID***: V1 버전의 {{site.data.keyword.conversationshort}} API를 사용 중인 앱 개발자가 메시지를 생성한 배치 환경을 식별하는 데 도움이 되도록 각 사용자 메시지와 함께 전달하는 고유 레이블입니다.
- ***인스턴스***: 고유한 인증 정보로 액세스할 수 있는 {{site.data.keyword.conversationshort}} 배치. {{site.data.keyword.conversationshort}} 인스턴스에는 여러 어시스턴트가 포함될 수 있습니다.
- ***메시지***: 메시지는 사용자가 어시스턴트에 전송하는 단일 발화(utterance)입니다.
- ***스킬 ID***: 스킬의 고유 ID입니다.
- ***사용자***: 사용자는 어시스턴트와 상호작용하는 모든 사람이며 종종 고객입니다.
- ***사용자 ID***: 특정 사용자의 서비스 사용량 레벨을 추적하는 데 사용되는 고유 레이블입니다.
- ***작업공간 ID***: 작업 공간의 고유 ID입니다. 11월 9일 이전에 작성한 작업공간은 제품 사용자 인터페이스에서 스킬로 표시되지만 스킬과 작업공간은 동일하지 않습니다. 스킬은 실질적으로 V1 작업공간의 랩퍼입니다.

**중요**: **사용자 ID** 특성이 **고객 ID** 특성과 동등하지 *않지만* 둘 다 메시지와 함께 전달될 수 있습니다. **사용자 ID** 필드는 청구 목적으로 사용량 레벨을 추적하는 데 사용되지만 **고객 ID** 필드는 일반 사용자와 연관된 메시지의 레이블 지정과 후속 삭제를 지원하는 데 사용됩니다. 고객 ID는 모든 Watson 서비스에서 일관되게 사용되며 `X-Watson-Metadata` 헤더에 지정됩니다. 사용자 ID는 {{site.data.keyword.conversationshort}} 서비스에서 독점적으로 사용되며 각 /message API 호출의 컨텍스트 오브젝트로 전달됩니다.

## 사용자 메트릭 사용
{: #logs-resources-user-id}

예를 들어, 사용자 메트릭을 사용하면 [개요 페이지](/docs/services/assistant?topic=assistant-logs-overview)에서 지정된 시간 간격 동안 어시스턴스와 교류한 고유 사용자 수 또는 사용자당 평균 대화 수를 볼 수 있습니다. 사용자 메트릭은 고유 `사용자 ID` 매개변수를 사용하여 사용으로 설정됩니다.

`/message` API를 사용하여 전송된 메시지에 대한 `사용자 ID`를 지정하려면 다음 예에서와 같이 [context ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://cloud.ibm.com/apidocs/assistant?curl=#get-response-to-user-input){: new_window}의 metadata 오브젝트 내부에 `user_id` 특성을 포함시키십시오.

```
"context" : {
  "metadata" : {
       "user_id": "{UserID}"
  }
}
```
{: codeblock}

## 삭제를 위해 메시지 데이터를 사용자와 연관
{: #logs-resources-customer_id}

{{site.data.keyword.conversationshort}} 인스턴스에서 사용자 데이터 세트를 완전히 제거하려는 경우가 발생할 수 있습니다. 삭제 기능이 사용되는 경우 개요 메트릭에 삭제된 메시지가 더 이상 반영하지 않습니다. 예를 들어, 개요 메트릭의 총 대화 수가 줄어듭니다.

### 시작하기 전에
{: #logs-resources-delete-customer-id-prereqs}

하나 이상의 개인에 대한 메시지를 삭제하려면 먼저 메시지를 각 개인의 고유 **고객 ID**와 연관시켜야 합니다. `/message` API를 사용하여 전송된 메시지에 대한 **고객 ID**를 지정하려면 `X-Watson-Metadata: customer_id` 특성을 헤더에 포함시키십시오. 다음 예에서와 같이 `customer_id`를 사용하여 세미콜론으로 구분된 `field=value` 쌍이 포함된 여러 **고객 ID** 항목을 전달할 수 있습니다.

```
curl -X POST -u "apikey:3Df... ...Y7Pc9"
 --header
   'Content-Type: application/json'
   'X-Watson-Metadata: customer_id={first-customer-ID};customer_id={second-customer-ID}'
 --data '{"input":{"text":"hello"}}' 'https:// gateway-us-south.watsonplatform.net/assistant/api/v1/workspaces/{workspaceID}/message?version=2018-09-20'
```
{: codeblock}

`customer_id` 문자열에는 세미콜론(`;`) 또는 등호(`=`) 문자가 포함될 수 없습니다. 각 `고객 ID` 매개변수가 고객마다 고유한지 확인해야 합니다.
{: note}

`customer_id` 값을 사용하여 메시지를 삭제하려면 [정보 보안](/docs/services/assistant?topic=assistant-information-security#information-security-gdpr-wa) 주제를 참조하십시오.

## Jupyter 노트북
{: #logs-resources-jupyter-notebooks}

IBM에서 로그 데이터를 보다 자세하게 분석하는 데 사용할 수 있는 Jupyter 노트북을 작성했습니다. Jupyter 노트북은 대화식 컴퓨팅을 위한 웹 기반 환경입니다. 데이터를 처리하는 작은 코드 조각을 실행하고 계산 결과를 즉시 확인할 수 있습니다.

표준 Python 도구에서 사용할 수 있는 노트북 세트와 {{site.data.keyword.DSX_full}}에서 최적으로 사용하도록 디자인된 세트가 있습니다. {{site.data.keyword.DSX_short}}는 데이터를 분석하고 시각화하거나, 데이터를 정리하고 형성하거나, 스트리밍 데이터를 수집하거나, 기계 학습 모델을 작성, 훈련 및 배치하는 데 필요한 도구를 선택할 수 있는 환경을 제공하는 제품입니다. 세부사항은 [제품 문서 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://dataplatform.cloud.ibm.com/docs/content/getting-started/welcome-main.html){:new_window}를 참조하십시오.

노트북이 어시스턴트를 향상하도록 도와주는 방법에 대해 자세히 알아보려면 [이 블로그 게시물![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://medium.com/ibm-watson/continuously-improve-your-watson-assistant-with-jupiter-notebooks-60231df4f01f)을 읽으십시오. 

사용 가능한 노트북은 다음과 같습니다.

- **측정**: 적용 범위(어시스턴트가 사용자에게 충분히 응답할 수 있다고 확신하는 빈도) 및 효율성(어시스턴트가 응답하는 시기, 응답이 사용자 요구사항을 충족하는지 여부)에 중점을 둔 메트릭을 수집합니다.

- **효율성**: 어시스턴트를 개선하기 위해 수행할 수 있는 단계를 이해하는 데 도움이 되도록 더 심층적인 로그 분석을 수행합니다.

[Watson Assistant Continuous Improvement Best Practices 안내서 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.ibm.com/common/ssi/cgi-bin/ssialias?htmlfid=54022554USEN&)에 노트북을 최대한 활용하는 방법이 설명되어 있습니다.

### {{site.data.keyword.DSX}}에서 노트북 사용
{: #logs-resources-notebooks-studio}

{{site.data.keyword.DSX}}에 사용하도록 디자인된 노트북을 사용하도록 선택하는 경우, 단계는 대략 다음과 같습니다.

1.  {{site.data.keyword.DSX}} 계정을 작성하고 [프로젝트를 작성한 후 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://dataplatform.cloud.ibm.com/docs/content/getting-started/projects.html?context=analytics){:new_window} Cloud Object Storage 계정을 추가하십시오.
1.  {{site.data.keyword.DSX}} 커뮤니티에서 [Watson Assistant 성능 측정 노트북  ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")]( https://dataplatform.cloud.ibm.com/exchange/public/entry/view/133dfc4cd1480bbe4eaa78d3f635e568)을 가져오십시오.
1   노트북과 함께 제공된 단계별 지시사항에 따라 로그의 대화 교환 서브세트를 분석하십시오.

    어시스턴트의 적용 범위와 효과를 더 쉽게 이해할 수 있도록 하는 방식으로 인사이트가 시각화됩니다.
1.  유효하지 않은 대화에서 샘플 로그 세트를 내보낸 후 분석하고 어노테이션을 작성하십시오.

    예를 들어, 응답이 올바른지 여부를 표시하십시오. 올바른 경우 유용한지 여부를 표시하십시오. 응답이 올바르지 않으면 근본 원인(예: 잘못된 인텐트 또는 엔티티가 발견되었거나 잘못된 대화 노드가 트리거됨)을 식별하십시오. 근본 원인을 식별한 후 올바른 선택사항이 무엇이었는지 표시하십시오.
1.  [Analyze Watson Assistant 효율성 노트북](https://dataplatform.cloud.ibm.com/exchange/public/entry/view/133dfc4cd1480bbe4eaa78d3f636921c)에 어노테이션이 있는 스프레드시트를 제공하십시오.

이 프로세스는 어시스턴트를 개선하기 위해 수행할 수 있는 단계를 이해하는 데 도움이 됩니다. 학습한 내용을 서비스 인스턴스에 자동으로 다시 적용할 수 있는 방법은 없습니다. 나중에 직접 대화 스킬의 훈련 데이터에 적용할 수 있도록 시스템을 개선하기 위해 작성한 변경사항을 추적하십시오.

### 표준 Python 도구에서 노트북 사용
{: #logs-resources-notebooks-python}

표준 Python 도구를 사용하여 노트북을 실행하도록 선택하는 경우 [IBM GitHub 저장소](https://github.com/watson-developer-cloud/assistant-improve-recommendations-notebook/tree/master/notebook)에서 노트북을 가져올 수 있습니다. 다음 순서대로 실행해야 합니다.

1.  Measure Notebook.ipynb
1.  Effectiveness Notebook.ipynb
