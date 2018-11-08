---

copyright:
  years: 2015, 2018
lastupdated: "2018-02-16"

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

# 오브젝트에 액세스하는 표현식

SpEL(Spring Expression) 언어를 사용하여 오브젝트의 특성 및 오브젝트에 액세스하는 표현식을 작성할 수 있습니다. 자세한 정보는 [SpEL(Spring Expression Language) ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](http://docs.spring.io/spring/docs/current/spring-framework-reference/html/expressions.html){: new_window}을 참조하십시오.
{: shortdesc}

## 평가 구문

다른 변수 내에서 변수 값을 늘리거나 특성 및 글로벌 오브젝트의 메소드를 호출하려면, `<? expression ?>` 표현식 구문을 사용하십시오. 예:

- **특성 확장**
    - `"output":{"text":"Your name is <? context.userName ?>"}`

- **글로벌 오브젝트의 특성에 대한 메소드 호출**
    - `"context":{"email": "<? @email.literal ?>"}`

## 단축 구문
{: #shorthand-syntax}

SpEL 단축 구문을 사용하여 다음 오브젝트를 빠르게 참조하는 방법을 학습하십시오.

- [컨텍스트 변수](expression-language.html#shorthand-context)
- [엔티티](expression-language.html#shorthand-entities)
- [인텐트](expression-language.html#shorthand-intents)

### 컨텍스트 변수의 단축 구문
{: #shorthand-context}

다음 표에서는 조건 표현식에서 컨텍스트 변수를 작성하는 데 사용할 수 있는 단축 구문의 예제를 표시합니다.

| 단축 구문                  | SpEL의 전체 구문                     |
|----------------------------|-----------------------------------------|
| `$card_type`               | `context['card_type']`                  |
| `$(card-type)`             | `context['card-type']`                  |
| `$card_type:VISA`          | `context['card_type'] == 'VISA'`        |
| `$card_type:(MASTER CARD)` | `context['card_type'] == 'MASTER CARD'` |

컨텍스트 변수 이름에 하이픈 또는 마침표와 같은 특수 문자를 포함할 수 있습니다. 하지만 이렇게 하면 SpEL 표현식을 평가할 때 문제점이 발생할 수 있습니다. 예를 들어, 하이픈이 빼기 부호로 해석될 수 있습니다. 이와 같은 문제점을 방지하려면 전체 표현식 구문 또는 단축 구문 `$(variable-name)`을 사용하여 변수를 참조하고, 이름에 다음 특수 문자는 사용하지 마십시오.

- 소괄호 ()
- 둘 이상의 아포스트로피 ''
- 따옴표 "

### 엔티티의 단축 구문
{: #shorthand-entities}

다음 표에서는 엔티티를 참조할 때 사용할 수 있는 단축 구문의 예제를 표시합니다.

| 단축 구문           | SpEL의 전체 구문                      |
|---------------------|------------------------------------------|
| `@year`             | `entities['year']?.value`                |
| `@year == 2016`     | `entities['year']?.value == 2016`        |
| `@year != 2016`     | `entities['year']?.value != 2016`        |
| `@city == 'Boston'` | `entities['city']?.value == 'Boston'`    |
| `@city:Boston`      | `entities['city']?.contains('Boston')`   |
| `@city:(New York)`  | `entities['city']?.contains('New York')` |

SpEL에서 물음표`(?)`를 사용하면 엔티티 오브젝트가 널일 때 널 포인터 예외가 트리거되지 않습니다.

검사할 엔티티 값에 `)` 문자가 있는 경우 비교를 위해 `:` 연산자를 사용할 수 없습니다.  예를 들어, 도시 엔티티가 `Dublin (Ohio)`인지 여부를 검사하려면 `@city:(Dublin (Ohio))` 대신 `@city == 'Dublin (Ohio)'`를 사용해야 합니다.

### 인텐트의 단축 구문
{: #shorthand-intents}

다음 표에서는 인텐트를 참조할 때 사용할 수 있는 단축 구문의 예제를 표시합니다.

| 단축 구문               | SpEL의 전체 구문 |
|-------------------------|---------------------|
| `#help`                 | `intent == 'help'`  |
| `! #help`               | `intent != 'help'`  |
| `NOT #help`             | `intent != 'help'`  |
| `#help` 또는 `#i_am_lost` | <code>(intent == 'help' \|\| intent == 'I_am_lost')</code> |

## 기본 제공 글로벌 변수
{: #builtin-vars}

표현식 언어를 사용하여 다음 글로벌 변수에 대한 특성 정보를 추출할 수 있습니다.

| 글로벌 변수          | 정의 |
|----------------------|------------|
| *context*            | 처리된 대화 메시지의 JSON 오브젝트 파트입니다. |
| *entities[ ]*        | 첫 번째 요소에 대한 기본 액세스를 지원하는 엔티티의 목록입니다. |
| *input*              | 처리된 대화 메시지의 JSON 오브젝트 파트입니다. |
| *intents[ ]*         | 첫 번째 요소에 대한 기본 액세스를 지원하는 인텐트의 목록입니다. |
| *output*             | 처리된 대화 메시지의 JSON 오브젝트 파트입니다. |

## 엔티티 액세스
{: #access-entity}

엔티티 배열은 사용자 입력에서 인식된 하나 이상의 엔티티를 포함합니다. 

대화 상자를 테스트하는 동안 대화 상자 노드 응답에서 다음 표현식을 지정하여 사용자 입력에서 인식되는 엔티티의 세부사항을 볼 수 있습니다.

```json
<? entities ?>
```
{: codeblock}

사용자 입력 *Hello now*의 경우 서비스가 @sys-date 및 @sys-time 시스템 엔티티를 인식하므로 응답에 다음 엔티티 오브젝트가 포함됩니다.

```json
[
{"entity":"sys-date","location":[6,9],"value":"2017-08-07",
  "confidence":1,"metadata":{"calendar_type":"GREGORIAN",
  "timezone":"America/New_York"}},
{"entity":"sys-time","location":[6,9],"value":"15:01:00",
  "confidence":1,"metadata":{"calendar_type":"GREGORIAN",
  "timezone":"America/New_York"}}
]
```
{: codeblock}

### 입력의 엔티티 배치가 문제가 되는 경우

입력의 엔티티 배치가 문제가 되는 경우 전체 SpEL 표현식을 사용하십시오. `entities['city']?.contains('Boston')` 조건은 배치에 관계없이 하나 이상의 'Boston' 도시 엔티티가 모든 @city 엔티티에 있는 경우 true를 리턴합니다.

예를 들어, 사용자가 `"I want to go from Toronto to Boston."`을 제출합니다. `@city:Toronto` 및 `@city:Boston` 엔티티가 발견되어 다음 엔티티에 표시됩니다.

- `entities.city[0].value = 'Toronto'`
- `entities.city[1].value = 'Boston'`

Boston이 발견된 두 번째 엔티티인 경우에도 대화 상자 노드의 `@city.contains('Boston')` 조건이 true를 리턴합니다.

### 엔티티 특성

각 엔티티에는 이와 연관된 특성 세트가 있습니다. 해당 특성을 통해 엔티티에 대한 정보에 액세스할 수 있습니다.

| 특성                  | 정의 | 사용 팁 |
|-----------------------|------------|------------|
| *confidence*          | 인식된 엔티티에서 서비스 신뢰도를 나타내는 백분율입니다. 엔티티의 유사 일치를 활성화한 경우를 제외하고 엔티티의 신뢰도는 0 또는 1입니다. 유사 일치를 사용하면 기본 신뢰수준 임계값이 0.3입니다. 유사 일치 사용 여부에 관계없이 시스템 엔티티의 신뢰수준은 항상 1.0입니다. | 신뢰수준이 지정한 백분율 이하인 경우 false를 리턴하도록 하기 위해 조건에 이 특성을 사용할 수 있습니다. |
| *location*            | 발견된 엔티티 값이 입력 텍스트에서 시작되고 끝나는 위치를 표시하는 0 기반 문자 오프셋입니다. | `.literal`을 사용하여 위치 특성에 저장된 시작과 종료 인덱스 값 사이에서 텍스트 범위를 추출하십시오. |
| *value*               | 입력에서 식별되는 엔티티 값입니다. | 연관된 동의어 중 하나에 대해 일치가 이루어지는 경우에도 이 특성은 훈련 데이터에 정의된 대로 엔티티 값을 리턴합니다. `.values`를 사용하여 사용자 입력에 있을 수 있는 엔티티의 다중 발생을 캡처할 수 있습니다. |

### 엔티티 특성 사용 예제
다음 예제에서 작업공간에 값 JFK 및 동의어 'Kennedy Airport"가 포함된 공항 엔티티가 포함되어 있습니다. 사용자 입력은 *I want to go to Kennedy Aiport*입니다.

- 'JFK' 엔티티가 사용자 입력에서 인식되는 경우 특정 응답을 리턴하기 위해 응답 조건에 다음 표현식을 추가할 수 있습니다.
  `entities.airport[0].value == 'JFK'`
  또는
  `@airport = "JFK"`
- 대화 상자 응답에서 사용자가 지정한 대로 엔티티 이름을 리턴하려면 .literal 특성을 사용하십시오.
  `So you want to go to <?entities.airport[0].literal?>...`
  또는
  `So you want to go to @airport.literal ...`

  두 형식 모두 응답에서 `So you want to go to Kennedy Airport...'로 평가됩니다.

- `@airport:(JFK)` 또는 `@airport.contains('JFK')`와 같은 표현식은 항상 엔티티(이 예제의 경우 `JFK`)의 **값**을 참조합니다.
- 유사 일치를 사용하는 경우 입력에서 공항으로 식별되는 용어를 더 제한하기 위해 노드 조건에 이 표현식을 지정할 수 있습니다(예: `@airport && @airport.confidence > 0.7`). 서비스는 입력 텍스트에 공항 참조가 포함된다는 내용을 70% 신뢰하는 경우에만 실행됩니다.

이 예제에서 사용자 입력은 *Are there places to exchange currency at JFK, Logan, and O'Hare?*입니다.

- 사용자 입력에서 엔티티 유형의 다중 발생을 캡처하려면 다음과 같은 구문을 사용하십시오.

    ```json
    "context":{
      "airports":"@airport.values"
    }
    ```

  대화 상자 응답에서 캡처된 목록을 나중에 참조하려면 다음 구문을 사용하십시오.
  `You asked about these airports: <? $airports.join(', ') ?>.`
  다음과 같이 표시됩니다.
  `You asked about these airports: JFK, Logan, O'Hare.`

## 인텐트에 액세스
{: #access-intent}

인텐트 배열에 사용자 입력에서 인식된 인텐트가 하나 이상 포함되어 있으며, 내림차순의 신뢰도로 정렬됩니다. 

각 인텐트에는 하나의 특성(예:`confidence` 특성)만 있습니다. 신뢰도 특성은 인식된 인텐트에서 서비스 신뢰도를 나타내는 백분율입니다.

대화 상자를 테스트하는 동안 대화 상자 노드 응답에 다음 표현식을 지정하여 사용자 입력에서 인식된 인텐트의 세부사항을 볼 수 있습니다.

```json
<? intents ?>
```
{: codeblock}

사용자 입력, *Hello now*의 경우, 서비스는 #greeting 인텐트와 정확한 일치를 찾습니다. 따라서, 먼저 #greeting 인텐트 오브젝트 세부사항을 나열합니다. 응답에는 자신의 신뢰도에 상관없이 스킬에서 정의된 상위 10 가지 다른 인텐트도 포함됩니다. (이 예제에서 다른 인텐트의 신뢰도는 0으로 설정됩니다. 이는 첫 번째 인텐트가 정확한 일치이기 때문입니다. ) "연습" 분할창이 요청과 함께 `alternate_intents:true` 매개 변수를 보내기 때문에 상위 10 개 인텐트가 리턴됩니다. API를 직접 사용하고 상위 10 개의 결과를 보려면 호출에서이 매개변수를 지정해야 합니다. `alternate_intents`가 false인 경우(기본값), 신뢰도가 0.2 이상인 인텐트만 배치에 리턴됩니다. 

```json
[{"intent":"greeting","confidence":1},
{"intent":"yes","confidence":0},
{"intent":"pizza-order","confidence":0}]
```
{: codeblock}

다음 예제에서는 인텐트 값을 검사하는 방법을 표시합니다.

- `intents[0] == 'Help'`
- `intent == 'Help'`

인텐트가 발견되지 않은 경우 `intent == 'help'`에 예외가 발생하지 않으므로 `intent == 'help'`는 `intents[0] == 'help'`와 다릅니다. 이는 인텐트 신뢰도가 임계값을 초과하는 경우에만 true로 평가됩니다.  원하는 경우 조건에 사용자 정의 신뢰수준을 지정할 수 있습니다(예: `intents.size() > 0 && intents[0] == 'help' && intents[0].confidence > 0.1`).

## 입력 액세스
{: #access-input}

입력 JSON 오브젝트에는 하나의 특성(텍스트 특성)만 포함됩니다. 텍스트 특성은 사용자 입력의 텍스트를 나타냅니다.

### 입력 특성 사용 예제

다음 예제에서는 입력에 액세스하는 방법을 표시합니다.

- 사용자 입력이 "Yes"인 경우 노드를 실행하려면 노드 조건에 다음 표현식을 추가하십시오.
  `input.text == 'Yes'`

[문자열 메소드](/docs/services/conversation/dialog-methods.html#strings)를 사용하여 사용자 입력에서 텍스트를 평가하거나 조작할 수 있습니다. 예:

- 사용자 입력에 "Yes"가 포함되는지 여부를 검사하려면 `input.text.contains( 'Yes' )`를 사용하십시오.
- 사용자 입력이 숫자인 경우(`input.text.matches( '[0-9]+' )`) true를 리턴합니다.
- 입력 문자열에 10개의 문자가 있는지 여부를 검사하려면 `input.text.length() == 10`을 사용하십시오.
