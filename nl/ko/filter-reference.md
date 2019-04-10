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

# 필터 조회 참조
{: #filter-reference}

{{site.data.keyword.conversationshort}} 서비스 REST API가 필터 조회를 통해 강력한 로그 검색 기능을 제공합니다. /logs API `filter` 매개변수를 사용하여 스킬 로그에서 지정된 조회와 일치하는 이벤트를 검색할 수 있습니다.

`filter` 매개변수는 결과를 지정된 필터와 일치하는 결과로 제한하는 캐시 가능한 조회입니다. JSON 응답 모델의 일부인 오브젝트에서 필터링할 수 있습니다(예: 사용자 입력 텍스트, 발견된 인텐트 및 엔티티 또는 신뢰도 점수).

다양한 종류의 필터 조회의 예제를 보려면 [예제](#filter-reference-examples)를 참조하십시오.

/logs `GET` 메소드 및 해당 응답 모델에 대한 자세한 정보는 [API 참조 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://cloud.ibm.com/apidocs/assistant?curl=#list-log-events-in-a-workspace){: new_window}를 참조하십시오.

## 필터 조회 구문
{: #filter-reference-syntax}

다음 예제에서는 필터 조회의 일반 양식을 표시합니다.

|위치           |조회 연산자 |용어         |
|:------------------:|:--------------:|:------------:|
|`request.input.text`|`::`            |`"IBM Watson"`|

- _위치_ 필터링할 필드(이 예제의 경우 `request.input.text`)를 식별합니다.
- _조회 연산자_ 사용할 일치 유형을 지정합니다(유사 일치 또는 정확한 일치).
- _용어_ 일치에 대해 필드를 평가하는 데 사용할 표현식 또는 값을 지정합니다. 용어에는 [다음 섹션](#filter-reference-operators)에 설명된 대로 리터럴 텍스트와 연산자가 포함될 수 있습니다.

인텐트 또는 엔티티로 필터링하려면 다른 필드에 대한 필터링과 약간 다른 구문을 사용해야 합니다. 자세한 정보는 [인텐트 또는 엔티티로 필터링](#filter-reference-intent-entity)을 참조하십시오.

**참고:** 필터 조회 구문은 HTTP 조회에서 허용되지 않는 몇 가지 문자를 사용합니다. 모든 특수 문자(공백과 따옴표 포함)는 HTTP 조회의 일부로 전송될 때 URL 인코딩됩니다. 예를 들어, 필터 `response_timestamp<2016-11-01`는 `response_timestamp%3C2016-11-01`로 지정됩니다.

## 연산자
{: #filter-reference-operators}

필터 조회에서 다음 연산자를 사용할 수 있습니다.

| 연산자              | 설명 |
|:-------------------:|-----------|
| `:` | 유사 일치 조회 연산자. 조회 용어 또는 조회 용어의 문법적 변형이 포함된 모든 값을 일치시키려는 경우 조회 용어 앞에 `:`을 추가하십시오. 유사 일치는 사용자 입력 텍스트, 응답 출력 텍스트 및 엔티티 값에서 사용 가능합니다. |
| `::` | 정확한 일치 조회 연산자. 조회 용어와 정확하게 같은 값만 일치시키려면 조회 용어 앞에 `::`을 추가하십시오. |
| `:!` | 부정 유사 일치 조회 연산자. 조회 용어 또는 조회 용어의 문법적 변형이 포함되지 _않은_ 값만 일치시키려면 조회 용어 앞에 `:!`를 추가하십시오. |
| `::!` | 부정 정확한 일치 조회 연산자. 조회 용어와 정확하게 일치하지 _않는_ 같은 값만 일치시키려면 조회 용어 앞에 `::!`를 추가하십시오. |
| `<=`, `>=`, `>`, `<` | 비교 연산자. 산술 비교를 기반으로 일치시키려면 조회 용어 앞에 이러한 연산자를 추가하십시오. |
| `\` | 이스케이프 연산자. 연산자로 구문 분석할 제어 문자를 포함하는 조회에서 사용하십시오. 예를 들어, `\!hello`가 텍스트 `!hello`와 일치합니다. |
| `""` | 리터럴 구. 공백 또는 기타 특수 문자가 포함된 조회 용어를 묶으려면 사용하십시오. 따옴표 내 특수 문자는 API에서 구문 분석되지 않습니다. |
| `~` | 근사 일치. 조회 용어의 끝에 이 연산자와 `1` 또는 `2`를 차례로 추가하여 조회 문자열과 응답 오브젝트의 일치 간에 허용된 단일 문자 차이의 수를 지정하십시오. 예를 들어, `car~1`은 `car`, `cat` 또는 `cars`와 일치하지만 `cats`와는 일치하지 않습니다. 이 연산자는 `log_id` 또는 임의 날짜나 시간 필드에서, 또는 유사 일치를 사용하여 필터링하는 경우 유효하지 않습니다. |
| `*` | 와일드카드 연산자. 0개 이상의 문자 순서와 일치시킵니다. 이 연산자는 `log_id` 또는 임의 날짜나 시간 필드에서 필터링하는 경우 유효하지 않습니다. |
| `()`, `[]` | 그룹화 연산자. 부울 연산자를 사용하여 여러 표현식의 논리 그룹을 묶으려면 사용하십시오. |
| `|` | 부울 _or_ 연산자. |
| `,` | 부울 _and_ 연산자. |

### 인텐트 또는 엔티티별 필터링
{: #filter-reference-intent-entity}

인텐트와 엔티티를 내부적으로 저장하는 방법의 차이 때문에 특정 인텐트 또는 엔티티에서 필터링하기 위한 구문이 리턴된 JSON의 다른 필드에 사용되는 구문과 다릅니다. `인텐트` 또는 `엔티티` 콜렉션 내에 `인텐트` 또는 `엔티티` 필드를 지정하려면 점 대신 `:` 일치 연산자를 사용해야 합니다.

예를 들어, 이 조회는 응답에 발견된 인텐트 `hello`가 포함된 로그된 이벤트와 일치합니다.

`response.intents:intent::hello`

점 대신 `:` 연산자를 사용하십시오(intents:intent).

동일한 패턴을 사용하여 응답에서 발견된 인텐트 또는 엔티티의 필드를 일치시키십시오. 예를 들어, 이 조회는 응답에 값이 `soda`인 발견된 엔티티가 포함된 로그된 이벤트와 일치합니다.

`response.entities:value::soda`

마찬가지로, 이 예제에서 요청의 일부로 전송된 인텐트 또는 엔티티를 필터링할 수 있습니다.

`request.intents:intent::hello`

인텐트에 대한 필터링은 발견된 모든 인텐트에 대해 작동합니다. 신뢰도가 가장 높은 발견된 인텐트만 필터링하기 위해 `response.top_intent` 단축 구문을 사용할 수 있습니다. 예:

`response.top_intent::goodbye`

### 고객 ID별 필터링
{: #filter-reference-customer-id}

고객 ID별로 필터링하려면 특수 위치 `customer_id`를 사용하십시오. (고객 ID를 사용한 메시지 레이블 지정에 대한 자세한 정보는 [정보 보안](/docs/services/assistant?topic=assistant-information-security)을 참조하십시오.)

### 다른 필드에서 필터링
{: #filter-reference-fields}

로그 데이터의 다른 필드를 필터링하려면 /logs API의 JSON 응답에서 중첩된 오브젝트의 레벨을 식별하는 경로로 위치를 지정하십시오. JSON 데이터에서 연속 중첩 레벨을 지정하려면 점(`.`)을 사용하십시오. 예를 들어, 위치 `request.input.text`는 다음 JSON 단편에 표시된 대로 사용자 입력 텍스트 필드를 식별합니다.

```json
  "request": {
    "input": {
      "text": "Good morning"
    }
  }
```

## 예제
{: #filter-reference-examples}

다음 예제에서는 이 구문을 사용하는 다양한 유형의 조회를 보여줍니다.

| 설명    | 조회      |
|---------|-----------|
| 응답 날짜가 2017년 7월입니다. | `response_timestamp>=2017-07-01,response_timestamp<2017-08-01` |
| 응답 시간소인은 `2016-11-01T04:00:00.000Z` 전입니다. | `response_timestamp<2016-11-01T04:00:00.000Z` |
| 고객 ID `my_id`로 메시지의 레이블이 지정됩니다. | `customer_id::my_id` |
| 사용자 입력 텍스트에 단어 "order" 또는 문법적 변형(예: `orders` 또는 `ordering`)이 포함됩니다. | `request.input.text:order` |
| 응답의 인텐트 이름이 `place_order`와 정확하게 일치합니다. | `response.intents:intent::place_order` |
| 응답의 엔티티 이름이 `beverage`와 정확하게 일치합니다.  | `response.entities:entity::beverage` |
| 사용자 입력 텍스트에 단어 "order" 또는 문법적 변형이 포함되지 않습니다. | `request.input.text:!order` |
| 신뢰도가 가장 높은 발견된 인텐트의 이름이 `hello`와 정확하게 일치하지 않습니다. | `response.top_intent::!hello` |
| 사용자 입력 텍스트에 문자열 `!hello`가 포함됩니다. | `request.input.text:\!hello` |
| 사용자 입력 텍스트에 문자열 `IBM Watson`이 포함됩니다. | `request.input.text:"IBM Watson"` |
| 사용자 입력 텍스트에 `Watson`과 두 개 이하의 단일 문자에서 차이가 있는 문자열이 포함됩니다. | `request.input.text:Watson~2` |
| 사용자 입력 텍스트에 `comm`, 0개 이상의 추가 문자, `s`가 차례로 오는 문자열이 포함됩니다. | `request.input.text:comm*s` |
| 컨텍스트의 배치 ID가 `my_app`과 일치합니다. | `request.context.metadata.deployment::my_app` |
| 응답의 인텐트 신뢰도 값이 0.8보다 큽니다. | `response.intents:confidence>0.8` |
| 응답의 인텐트 이름이 `hello` 또는 `goodbye`와 정확하게 일치합니다. | `response.intents:intent::(hello|goodbye)` |
| 응답의 인텐트 이름이 `hello`이며 신뢰도 값은 0.8 이상입니다. | `response.intents:(intent:hello,confidence>=0.8)` |
| 응답의 인텐트 이름이 `order`와 정확히 일치하며, 응답의 엔티티 이름은 `beverage`와 정확히 일치합니다. | `[response.intents:intent::order,response.entities:entity::beverage]` |
