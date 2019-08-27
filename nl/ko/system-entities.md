---

copyright:
  years: 2015, 2019
lastupdated: "2019-08-06"

keywords: system entity, sys-number, sys-date, sys-time

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

# 시스템 엔티티 세부사항
{: #system-entities}

바로 사용할 수 있도록 IBM에서 제공하는 시스템 엔티티에 대해 자세히 알아보십시오. 이러한 기본 제공 유틸리티 엔티티는 어시스턴트가 대화에서 고객이 일반적으로 사용하는 용어 및 참조(예: 숫자 및 날짜)를 인식하는 데 도움을 줍니다.
{: shortdesc}

시스템 엔티티는 [지원되는 언어](/docs/services/assistant?topic=assistant-language-support) 주제에 설명된 언어에서 사용 가능합니다.

대화 스킬이 영어 또는 독일어인 경우 업데이트된 시스템 엔티티를 사용해 볼 수 있습니다. 자세한 내용은 [새 시스템 엔티티](/docs/services/assistant?topic=assistant-beta-system-entities)를 참조하십시오.

사용 방법에 대한 자세한 정보는 [엔티티 작성](/docs/services/assistant?topic=assistant-entities#entities-enable-system-entities)을 참조하십시오.

## @sys-currency 엔티티
{: #system-entities-sys-currency}

@sys-currency 시스템 엔티티는 통화 기호 또는 통화 특정 용어가 포함된 발화(utterance)에 표시된 통화 값을 발견합니다. 숫자 값이 리턴됩니다.

### 인식되는 형식
{: #system-entities-sys-currency-formats}

- 20 cents
- Five dollars
- $10

### 메타데이터
{: #system-entities-sys-currency-metadata}

- `.numeric_value`: 정수 또는 실수인 표준 숫자 값(기본 단위)
- `.unit`: 기본 단위 통화 코드(예: 'USD' 또는 'EUR')

### 리턴
{: #system-entities-sys-currency-returns}

입력 `twenty dollars` 또는 `$1,234.56`의 경우, @sys-currency는 다음 값을 리턴합니다.

| 속성                   | 유형   | `twenty dollars`에 대해 리턴됨 | `$1,234.56`에 대해 리턴됨 |
|-----------------------------|--------|-------------------------------|-------------------------:|
| @sys-currency               | 문자열 | 20                            |                  1234.56 |
| @sys-currency.literal       | 문자열 | twenty dollars                |                $1,234.56 |
| @sys-currency.numeric_value | 숫자 | 20                            |                  1234.56 |
| @sys-currency.location      | 배열  | [0,14]                        |                    [0,9] |
| @sys-currency.unit          | 문자열 | USD*                          |                      USD |

*@sys-currency.unit은 항상 3자의 ISO 통화 코드를 리턴합니다.

입력 `veinte euro` 또는 <code>&euro;1.234,56</code>(스페인어)의 경우 @sys-currency가 다음 값을 리턴합니다.

| 속성                   | 유형   | `veinte euro`에 대해 리턴됨 | <code>&euro;1.234,56</code>에 대해 리턴됨 |
|-----------------------------|--------|-----------------------------|-------------------------:|
| @sys-currency               | 문자열 | 20                          |                  1234.56 |
| @sys-currency.literal       | 문자열 | veinte euro                 |                &euro;1.234,56 |
| @sys-currency.numeric_value | 숫자 | 20                          |                  1234.56 |
| @sys-currency.location      | 배열  | [0,11]                       |                     [0,9]|
| @sys-currency.unit          | 문자열 | EUR*                        |                     EUR  |
*@sys-currency.unit은 항상 3자의 ISO 통화 코드를 리턴합니다.

지원되는 다른 언어 및 자국 통화에 대해서도 동등한 결과를 얻습니다.

### @system-currency 사용 팁
{: #system-entities-currencty-usage-tips}

- 통화 값은 @sys-number 엔티티의 인스턴스로 인식됩니다. 개별 조건을 사용하여 통화 값과 숫자 둘 다 검사하는 경우, 숫자를 검사하는 조건 위에 통화를 검사하는 조건을 배치하십시오.

  이 임시 해결책은 개정된 시스템 엔티티를 사용하는 경우에는 필요하지 않습니다. 자세한 내용은 [새 시스템 엔티티](/docs/services/assistant?topic=assistant-beta-system-entities)를 참조하십시오.
  {: note}

- @sys-currency 엔티티를 노드 조건으로 사용하고 사용자가 `$0`을 값으로 지정하면, 값이 통화로 올바르게 인식되지만 조건은 통화 0이 아닌 숫자 0으로 평가됩니다. 결과적으로 조건에서 `null`이 false로 평가되고 노드는 처리되지 않습니다. 0을 올바르게 처리하는 방식으로 통화 값을 검사하려면 대신 노드 조건에서 `@sys-currency >=0` 표현식을 사용하십시오.

## @sys-date 및 @sys-time 엔티티
{: #system-entities-sys-date-time}

`@sys-date` 시스템 엔티티는 `Friday`, `today` 또는 `November 1`과 같은 멘션을 추출합니다. 이 엔티티의 값은 해당 날짜를 "yyyy-MM-dd" 형식의 문자열(예: "2016-11-21")로 저장합니다. 시스템은 날짜의 누락된 요소(예: "November 21"의 연도)를 현재 날짜 값으로 보완합니다.

**참고:** - 영어 로케일의 경우에만 날짜 입력을 위한 기본 시스템 동작이 MM/DD/YYYY입니다. 처음 두 숫자가 12보다 큰 경우에만 DD/MM/YYYY로 변경됩니다. 저장되는 값의 형식은 여전히 "yyyy-MM-dd"입니다.

`@sys-time` 시스템 엔티티는 `2pm`, `at 4` 또는 `15:30`과 같은 멘션을 추출합니다. 이 엔티티의 값은 "HH:mm:ss" 형식의 문자열로 시간을 저장합니다. 예를 들어, "13:00:00"입니다.

### 날짜-시간 멘션
{: #system-entities-date-time-mentions}

날짜 및 시간 멘션(예: `now` 또는 `two hours from now`)은 두 개의 개별 엔티티 멘션으로 추출됩니다(하나의 `@sys-date` 및 하나의 `@sys-time`). 이러한 두 개의 멘션은 전체 날짜-시간 멘션을 포괄하는 동일한 리터럴 문자열을 공유하는 경우를 제외하고 서로 연결되지 않습니다.

다중 단어 날짜-시간 멘션(예: `on Monday at 4pm`)도 두 개의 @sys-date 및 @sys-time 멘션으로 추출됩니다. 또한 연속하여 함께 멘션되는 경우 전체 날짜-시간 멘션을 포괄하는 단일 리터럴 문자열을 공유합니다.

### 날짜 및 시간 범위
{: #system-entities-date-time-ranges}

날짜 범위 멘션(예: `the weekend`, `next week` 또는 `from Monday to Friday`)은 범위의 시작과 끝을 표시하는 `@sys-date` 엔티티 멘션의 쌍으로 추출됩니다. 마찬가지로 시간 범위의 멘션(`from 2 to 3`)은 두 개의 `@sys-time` 엔티티로 추출되며, 시작 시간과 끝 시간을 표시합니다. 쌍으로 된 두 개의 엔티티는 전체 날짜 또는 시간 범위 멘션에 해당하는 리터럴 문자열을 공유합니다.

### `지난` 및 `다음` 날짜와 시간
{: #system-entities-last-next}

일부 로케일에서 "last Monday"와 같은 구는 이전 주의 월요일만 지정하는 데 사용됩니다. 이와 반대로 다른 로케일은 "last Monday"를 사용하여 월요일이었던 지난 요일을 지정하지만 같은 주 또는 이전 주의 월요일일 수 있습니다.

한 예로, 6월 16일 금요일인 경우 어떤 로케일에서는 "last Monday"가 6월 12일 또는 6월 5일을 나타내는 반면, 다른 로케일에서는 6월 5일만 나타냅니다(이전 주). 동일한 로직이 "next Monday"와 같은 구에서도 유효합니다.

{{site.data.keyword.conversationshort}} 서비스는 "지난" 날짜 및 "다음" 날짜를 참조된 가장 가까운 지난 요일이나 다른 요일을 나타내는 것으로 간주하며, 이 날짜는 같은 주나 이전 주의 요일일 수 있습니다.

"for the last 3 days" 또는 "in the next 4 hours"와 같은 시간 구에서 로직은 동일합니다. 예를 들어, "in the next 4 hours"의 경우, 두 개의 `@sys-time` 엔티티(현재 시간의 엔티티와 현재 시간보다 4시간 뒤의 엔티티)가 생깁니다.

### 시간대
{: #system-entities-time-zones}

현재 시간과 관련된 날짜나 시간의 언급은 선택한 시간대와 관련하여 해결됩니다. 기본적으로 UTC(GMT)입니다. 즉, 기본적으로 UTC와 다른 시간대에 있는 REST API 클라이언트는 현재 UTC 시간에 따라 추출된 `now` 값을 관찰합니다.

선택적으로 REST API 클라이언트는 로컬 시간대를 컨텍스트 변수 `$timezone`으로 추가할 수 있습니다. 이 컨텍스트 변수는 모든 클라이언트 요청과 함께 보내야 합니다. 예를 들어, `$timezone` 값은 `America/Los_Angeles`, `EST` 또는 `UTC`여야 합니다. 지원되는 시간대의 전체 목록은 [지원되는 시간대](/docs/services/assistant?topic=assistant-time-zones)를 참조하십시오.

`$timezone` 변수가 제공되는 경우 관련 @sys-date 및 @sys-time 멘션의 값은 UTC 대신 클라이언트 시간대에 따라 계산됩니다.

#### 시간대와 관련된 멘션의 예제
{: #system-entities-time-zone-examples}

- now
- in two hours
- today
- tomorrow
- 2 days from now

### 인식되는 형식
{: #system-entities-sys-date-time-formats}

- November 21
- 10:30
- at 6 pm
- this weekend

### 리턴
{: #system-entities-sys-date-time-returns}

입력 `November 21`의 경우 @sys-date가 다음 값을 리턴합니다.

| 속성               | 유형   | `November 21`에 대해 리턴됨 |
|-------------------------|--------|---------------------------:|
| @sys-date.literal       | 문자열 |                November 21 |
| @sys-date               | 문자열 |                20xx-11-21 *|
| @sys-date.location      | 배열 |                    [0,11]  |
| @sys-date.calendar_type | 문자열 |                  GREGORIAN |

- @sys-date는 항상 yyyy-MM-dd 형식의 날짜를 리턴합니다.
- \* 다음 일치 날짜를 리턴합니다. 날짜가 이미 올해를 지난 경우 다음 연도의 날짜를 리턴합니다.

입력 `at 6 pm`의 경우 @sys-time이 다음 값을 리턴합니다.

| 속성               | 유형   | `at 6 pm`에 대해 리턴됨 |
|-------------------------|--------|-----------------------:|
| @sys-time.literal       | 문자열 |                at 6 pm |
| @sys-time               | 문자열 |               18:00:00 |
| @sys-time.location      | 배열 |                   [0,7]|
| @sys-time.calendar_type | 문자열 |              GREGORIAN |

- @sys-time은 항상 HH:mm:ss 형식으로 시간을 리턴합니다.

날짜 및 시간 값 처리에 대한 정보는 [날짜 및 시간 메소드 참조](/docs/services/assistant?topic=assistant-dialog-methods#dialog-methods-date-time)를 참조하십시오.
{: tip}

## @sys-location 엔티티
{: #system-entities-sys-location}

**베타([지원되는 언어](/docs/services/assistant?topic=assistant-language-support) 주제에 설명된 언어의 경우)**: @sys-location 시스템 엔티티는 사용자 입력에서 장소 이름(국가, 시/도, 도시, 소도시 등)을 추출합니다. 엔티티 값은 위치의 시스템 표준 값이 아닙니다.

### 인식되는 형식
{: #system-entities-sys-location-formats}

- Boston
- 대표전화서비스: 02-3781-7114
- New South Wales

문자열 값 처리에 대한 정보는 [문자열 메소드 참조](/docs/services/assistant?topic=assistant-dialog-methods#dialog-methods-strings)를 참조하십시오.
{: tip}

## @sys-number 엔티티
{: #system-entities-sys-number}

@sys-number 시스템 엔티티는 숫자나 단어를 사용하여 작성된 숫자를 발견합니다. 어느 경우에나 숫자 값이 리턴됩니다.

### 인식되는 형식
{: #system-entities-sys-number-formats}

- 21
- twenty one
- 3.13

### 메타데이터
{: #system-entities-sys-number-metadata}

- `.numeric_value` - 정수 또는 실수인 표준 숫자 값

### 리턴
{: #system-entities-sys-number-returns}

입력 `twenty` 또는 `1,234.56`의 경우 @sys-number가 다음 값을 리턴합니다.

| 속성                   | 유형   | `twenty`에 대해 리턴됨 | `1,234.56`에 대해 리턴됨 |
|-----------------------------|--------|-------------------|------------------------:|
| @sys-number               | 문자열 | 20                |                 1234.56 |
| @sys-number.literal       | 문자열 | twenty            |                1,234.56 |
| @sys-number.location      | 배열 | [0,6]             |                   [0,8]|
| @sys-number.numeric_value | 숫자 | 20                |                 1234.56 |

입력 `veinte` 또는 `1.234,56`(스페인어)의 경우 @sys-number가 다음 값을 리턴합니다.

| 속성                   | 유형   | `veinte`에 대해 리턴됨 | `1.234,56`에 대해 리턴됨 |
|-----------------------------|--------|-----------------------|------------------------:|
| @sys-number               | 문자열 |20                    |                 1234.56 |
| @sys-number.literal       | 문자열 |veinte                |                1.234,56 |
| @sys-number.location      | 배열  |[0,6]                 |                   [0,8] |
| @sys-number.numeric_value | 숫자 |20                    |                 1234.56 |

지원되는 다른 언어에 대해서도 동등한 결과를 얻습니다.

### @system-number 사용 팁
{: #system-entities-sys-number-usage-tips}

- @sys-number 엔티티를 노드 조건으로 사용하고 사용자가 0을 값으로 지정하면, 0 값이 숫자로 올바르게 인식됩니다. 그러나 0은 조건에 대한 `null` 값으로 해석되며, 이는 노드가 처리되지 않게 합니다. 0을 올바르게 처리하는 방식으로 숫자를 검사하려면 대신 노드 조건에서 `@sys-number >=0` 표현식을 사용하십시오.

- @sys-number를 사용하여 조건에서 숫자 값을 비교하는 경우 숫자의 존재에 대한 검사를 별도로 포함해야 합니다. 숫자가 없으면 @sys-number가 널로 평가되며, 그 결과 숫자가 없더라도 비교 시 true로 평가됩니다.

  예를 들어, 숫자가 없는 경우 `@sys-number`가 널로 평가되므로 `@sys-number<4`를 단독으로 사용하지 마십시오. 널이 4보다 작으므로, 숫자가 없더라도 조건은 true로 평가됩니다.

  대신 `@sys-number AND @sys-number<4`를 사용하십시오. 숫자가 없는 경우 첫 번째 조건이 false로 평가되며 그에 따라 전체 조건이 false로 평가됩니다.

숫자 값 처리에 대한 정보는 [숫자 메소드 참조](/docs/services/assistant?topic=assistant-dialog-methods#dialog-methods-numbers)를 참조하십시오.
{: tip}

## @sys-percentage 엔티티
{: #system-entities-sys-percentage}

@sys-percentage 시스템 엔티티는 백분율 기호가 포함된 발화에 표시되거나 단어 `percent`를 사용하여 작성된 백분율을 발견합니다. 어느 경우에나 숫자 값이 리턴됩니다.

### 인식되는 형식
{: #system-entities-sys-percentage-formats}

- 15%
- 10 percent

### 메타데이터
{: #system-entities-sys-percentage-metadata}

`.numeric_value`: 정수 또는 실수인 표준 숫자 값

### 리턴
{: #system-entities-sys-percentage-returns}

입력 `1,234.56%`의 경우 @sys-percentage가 다음 값을 리턴합니다.

| 속성                     | 유형   | `1,234.56%`에 대해 리턴됨 |
|-------------------------------|--------|-------------------------:|
| @sys-percentage               | 문자열 |                  1234.56 |
| @sys-percentage.literal       | 문자열 |                1,234.56% |
| @sys-percentage.location      | 배열  |                    [0,9] |
| @sys-percentage.numeric_value | 숫자 |                  1234.56 |

입력 `1.234,56%`(스페인어)의 경우 @sys-currency가 다음 값을 리턴합니다.

| 속성                     | 유형   | `1.234,56%`에 대해 리턴됨 |
|-------------------------------|--------|-------------------------:|
| @sys-percentage               | 문자열 |                  1234.56 |
| @sys-percentage.literal       | 문자열 |                1.234,56% |
| @sys-percentage.location      | 배열  |                    [0,9] |
| @sys-percentage.numeric_value | 숫자 |                  1234.56 |

지원되는 다른 언어에 대해서도 동등한 결과를 얻습니다.

### @system-percentage 사용 팁
{: #system-entities-sys-percentage-usage-tips}

- 백분율 값은 @sys-number 엔티티의 인스턴스로 인식됩니다. 개별 조건을 사용하여 백분율 값과 숫자 둘 다 검사하는 경우, 숫자를 검사하는 조건 위에 백분율을 검사하는 조건을 배치하십시오.

      이 임시 해결책은 개정된 시스템 엔티티를 사용하는 경우에는 필요하지 않습니다. 자세한 내용은 [새 시스템 엔티티](/docs/services/assistant?topic=assistant-beta-system-entities)를 참조하십시오.
  {: note}

- @sys-percentage 엔티티를 노드 조건으로 사용하고 사용자가 `0%`를 값으로 지정하는 경우, 값이 백분율로 올바르게 인식되지만 조건은 백분율 0%가 아닌 숫자 0으로 평가됩니다. 결과적으로 조건의 `null`이 false로 평가되고 노드가 처리되지 않습니다. 0을 올바르게 처리하는 방식으로 백분율을 검사하려면 대신 노드 조건에서 `@sys-percentage >=0` 표현식을 사용하십시오.

- `1-2%`와 같은 값을 입력하는 경우 값 `1%` 및 `2%`가 시스템 엔티티로 리턴됩니다. 인덱스는 1%와 2% 사이의 범위 전체에 있으며 두 엔티티 모두 동일한 인덱스를 가집니다.

## @sys-person 엔티티
{: #system-entities-sys-person}

**베타([지원되는 언어](/docs/services/assistant?topic=assistant-language-support) 주제에 설명된 언어의 경우)**: @sys-person 시스템 엔티티는 사용자 입력에서 이름을 추출합니다. 이름은 개별적으로 인식되므로, "Joe"가 "Joseph"으로 간주되지 않으며, 그 반대의 경우도 마찬가지입니다. 엔티티의 값은 이름의 시스템 표준 값이 아닙니다.

### 인식되는 형식
{: #system-entities-sys-person-formats}

- Ronald
- Jane Doe
- Vijay

문자열 값 처리에 대한 정보는 [문자열 메소드 참조](/docs/services/assistant?topic=assistant-dialog-methods#dialog-methods-strings)를 참조하십시오.
{: tip}
