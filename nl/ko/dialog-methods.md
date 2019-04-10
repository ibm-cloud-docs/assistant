---

copyright:
  years: 2015, 2019
lastupdated: "2019-02-28"

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

# 표현식 언어 메소드
{: #dialog-methods}

컨텍스트 변수, 조건 또는 응답의 다른 위치에서 참조할 사용자 발화(utterance)에서 추출한 값을 처리할 수 있습니다.
{: shortdesc}

## 평가 구문
{: #dialog-methods-evaluation-syntax}

다른 변수 내에서 변수 값을 늘리거나 텍스트 또는 컨텍스트 변수를 출력하는 메소드를 적용하려면, `<? expression ?>` 표현식 구문을 사용하십시오. 예:

- **숫자 특성 증분**
    - `"output":{"number":"<? output.number + 1 ?>"}`
- **오브젝트에서 메소드 호출**
    - `"context":{"toppings": "<? context.toppings.append( 'onions' ) ?>"}`

다음 섹션은 값을 처리하는데 사용할 수 있는 메소드를 설명합니다. 메소드는 데이터 유형별로 구성됩니다.

- [배열](#dialog-methods-arrays)
- [날짜 및 시간](#dialog-methods-date-time)
- [숫자](#dialog-methods-numbers)
- [오브젝트](#dialog-methods-objects)
- [문자열](#dialog-methods-strings)

## 배열
{: #dialog-methods-arrays}

배열 값을 설정하는 동일한 노드 내 노드 조건 또는 응답 조건에서 배열의 값을 검사하는 데 다음 메소드를 사용할 수 없습니다.

### JSONArray.append(object)

이 메소드는 JSONArray에 새 값을 추가하고 수정된 JSONArray를 리턴합니다.

다음 대화 런타임 컨텍스트의 경우:

```json
{
  "context": {
    "toppings_array": ["onion", "olives"]
  }
}
```
{: codeblock}

이를 다음과 같이 업데이트합니다.

```json
{
  "context": {
    "toppings_array": "<? $toppings_array.append('ketchup', 'tomatoes') ?>"
  }
}
```
{: codeblock}

결과:

```json
{
  "context": {
    "toppings_array": ["onion", "olives", "ketchup", "tomatoes"]
  }
}
```
{: codeblock}

### JSONArray.clear()

이 메소드는 배열에서 모든 값을 지우고 널을 리턴합니다.

출력에서 다음 표현식을 사용하여 해당 값의 컨텍스트 변수($toppings_array)에 저장한 배열을 지우는 필드를 정의합니다.

```json
{
  "output": {
    "array_eraser": "<? $toppings_array.clear() ?>"
  }
}
```
{: codeblock}

차후에 $toppings_array 컨텍스트 변수를 참조하는 경우, '[]'만 리턴합니다.

### JSONArray.contains(Object value)

이 메소드는 입력 JSONArray에 입력 값이 포함되어 있는 경우 true를 리턴합니다.

이전 노드 또는 외부 애플리케이션에서 설정된 대화 런타임 컨텍스트의 경우:

```json
{
  "context": {
    "toppings_array": ["onion", "olives", "ham"]
  }
}
```
{: codeblock}

대화 노드 또는 응답 조건:

```json
$toppings_array.contains('ham')
```
{: codeblock}

결과: 배열에 요소 ham이 포함되어 있으므로 `True`입니다.

### JSONArray.containsIntent(String intent_name, Double min_score, [Integer top_n])
{: #dialog-methods-array-containsIntent}

이 메소드는 `intents` JSONArray가 지정된 인텐트를 특별히 포함하고 해당 인텐트의 신뢰도 점수가 지정된 최소 점수 이상인 경우 그러한 인텐트가 지정된 최소 점수와 같거나 높은 신뢰도 점수를 갖는 경우에 `true`를 리턴합니다. 선택적으로 배열의 최상위 요소 개수 내에 인텐트가 포함되어야 함을 표시하도록 숫자를 지정할 수 있습니다.

지정된 인텐트가 배열에 없는 경우, 최소 신뢰도 점수 이상인 신뢰도 점수가 없는 경우 또는 지정된 인덱스 위치보다 배열에서 인텐트가 더 낮은 경우, `false`를 리턴합니다.

서비스는 사용자 입력이 제출될 때마다 서비스가 입력에서 발견하는 인텐트를 나열하는 `intents` 배열을 자동으로 생성합니다. 이 배열은 서비스에서 발견되는 모든 인텐트를 가장 높은 신뢰도 순서로 나열합니다.

노드 조건에서 이 메소드를 사용하여 인텐트의 존재 여부를 확인할 뿐만 아니라, 노드가 처리되고 응답이 리턴되기 전에 충족해야 하는 신뢰도 점수 임계값을 설정할 수 있습니다.

예를 들어, 다음 조건이 충족되는 경우에만 대화 노드를 트리거할 때 노드 조건에서 다음 표현식을 사용하십시오.

- `#General_Ending` 인텐트가 있습니다.
- `#General_Ending` 인텐트의 신뢰도 점수가 80% 이상입니다.
- `#General_Ending` 인텐트가 인텐트 배열의 상위 2번째 인텐트입니다.

```bash
intents.containsIntent("General_Ending", 0.8, 2)
```
{: codeblock}

### JSONArray.filter(temp, "temp.property operator comparison_value")
{: #dialog-methods-array-filter}

각 배열 요소 값을 사용자가 지정하는 값과 비교하여 배열을 필터링합니다. 이 메소드는 [콜렉션 투영법(collection projection)](#collection-projection)과 유사합니다. 콜렉션 투영법은 배열 요소 이름-값 쌍의 이름을 기반으로 필터링된 배열을 리턴합니다. 필터 메소드는 배열 요소 이름-값 쌍의 값을 기반으로 필터링된 배열을 리턴합니다. 

필터 표현식은 다음 값으로 구성됩니다.

- `temp`: 각 배열 요소가 평가될 때 임시로 사용되는 변수의 이름입니다. 예: `city`.
- `property`: `comparison_value`와 비교할 요소 특성입니다. 첫 번째 매개변수에서 이름을 지정하는 임시 변수의 특성으로 특성을 지정하십시오. `temp.property` 구문을 사용하십시오. 예를 들어, `latitude`가 배열의 이름-값 쌍에 올바른 요소 이름인 경우, 특성을 `city.latitude`로 지정합니다.
- `operator`: 특성 값과 `comparison_value`를 비교하는 데 사용할 연산자입니다.

    지원되는 연산자는 다음과 같습니다.

    <table>
    <caption>지원되는 필터 연산자</caption>
    <tr>
      <th>연산자</th>
      <th>설명</th>
    </tr>
    <tr>
      <td>`==`</td>
      <td>같음</td>
    </tr>
    <tr>
      <td>`>`</td>
      <td>보다 큼</td>
    </tr>
    <tr>
      <td>`<`</td>
      <td>보다 작음</td>
    </tr>
    <tr>
      <td>`>=`</td>
      <td>크거나 같음</td>
    </tr>
    <tr>
      <td>`<=`</td>
      <td>작거나 같음</td>
    </tr>
    <tr>
      <td>`!=`</td>
      <td>같지 않음</td>
    </tr>
    </table>

- `comparison_value`: 각각의 배열 요소 특성 값을 비교할 값입니다. 사용자 입력에 따라 변경할 수 있는 값을 지정하려면 컨텍스트 변수 또는 엔티티를 값으로 사용하십시오. 변경 가능한 값을 지정하는 경우, `comparison_value` 값이 평가 시에 유효함을 보장하는 로직을 추가하십시오. 아닌 경우, 오류가 발생합니다.

#### 필터 예제 1

예를 들어, 필터 메소드를 사용하여 인구가 500만이 넘는 도시를 포함하는 더 작은 배열을 리턴하도록 구/군/시 이름 및 해당 인구수 세트를 포함하는 배열을 평가할 수 있습니다.

다음 `$cities` 컨텍스트 변수에는 오브젝트 배열이 포함되어 있습니다. 각 오브젝트에는 `name` 및 `population` 특성이 포함되어 있습니다.

```json
[
   {
      "name":"Tokyo",
      "population":9273000
   },
   {
      "name":"Rome",
      "population":2868104
   },
   {
      "name":"Beijing",
      "population":20693000
   },
   {
      "name":"Paris",
      "population":2241346
   }
]
```
{: codeblock}

다음 예제에서 임의의 임시 변수 이름은 `city`입니다. SpEL 표현식은 인구 5백만 이상의 도시만 포함하도록 `$cities` 배열을 필터링합니다.

```bash
$cities.filter("city", "city.population > 5000000")
```
{: codeblock}

이 표현식은 다음과 같은 필터링된 배열을 리턴합니다.

```json
[
   {
      "name":"Tokyo",
      "population":9273000
   },
   {
      "name":"Beijing",
      "population":20693000
   }
]
```
{: codeblock}

콜렉션 투영법을 사용하여 필터 메소드에서 리턴하는 배열의 도시 이름만 포함하는 새 배열을 작성할 수 있습니다. 그런 다음 `join` 메소드를 사용하여 배열에서 두 개의 이름 요소 값을 문자열로 표시하고 값을 쉼표와 공백으로 구분할 수 있습니다.

```bash
The cities with more than 5 million people include <?  T(String).join(", ",($cities.filter("city", "city.population > 5000000")).![name]) ?>.
```
{: codeblock}

결과 응답은 `The cities with more than 5 million people include Tokyo, Beijing.`입니다.

#### 필터 예제 2

필터 메소드의 장점은 `comparison_value` 값을 하드 코딩하지 않아도 된다는 점입니다. 이 예제에서는 하드 코딩된 값 5000000이 대신 컨텍스트 변수로 대체됩니다.

이 예제에서는 `$population_min` 컨텍스트 변수에 `5000000`이 포함됩니다. 임의의 임시 변수 이름은 `city`입니다. SpEL 표현식은 인구 5백만 이상의 도시만 포함하도록 `$cities` 배열을 필터링합니다.

```bash
$cities.filter("city", "city.population > $population_min")
```
{: codeblock}

이 표현식은 다음과 같은 필터링된 배열을 리턴합니다.

```json
[
   {
      "name":"Tokyo",
      "population":9273000
   },
   {
      "name":"Beijing",
      "population":20693000
   }
]
```
{: codeblock}

숫자 값을 비교할 때 필터 메소드가 트리거되기 전에 비교에 포함된 컨텍스트 변수를 올바른 값으로 설정해야 합니다. `null`을 비교하는 배열 요소에 null이 포함될 가능성이 있는 경우 이 값이 올바른 값일 수 있습니다. 예를 들어, 도쿄에 대한 인구 이름 및 값 쌍이 `"population":null`이고 비교 표현식이 `"city.population == $population_min"`인 경우, `null`이 `$population_min` 컨텍스트 변수에 올바른 값입니다.
{: tip}

다음과 같은 대화 노드 응답 표현식을 사용할 수 있습니다.

```bash
The cities with more than $population_min people include <?  T(String).join(", ",($cities.filter("city", "city.population > $population_min")).![name]) ?>.
```
{: codeblock}

결과 응답은 `The cities with more than 5000000 people include Tokyo, Beijing.`입니다.

#### 필터 예제 3

이 예제에서, 엔티티 이름이 `comparison_value`으로 사용됩니다. 사용자 입력은 `What is the population of Tokyo?`입니다. 임의의 임시 변수 이름은 `y`입니다. `Tokyo`를 포함하여 도시 이름을 인식하는 `@city`라는 이름의 엔티티를 작성했습니다.

```bash
$cities.filter("y", "y.name == @city")
```

이 표현식은 다음과 같은 배열을 리턴합니다.

```json
[
   {
      "name":"Tokyo",
      "population":9273000
   }
]
```
{: codeblock}

콜렉션 투영법을 사용하여 원래 배열의 인구 요소만 포함하는 배열을 가져오고 `get` 메소드를 사용하여 인구 요소의 값을 리턴할 수 있습니다.

```bash
The population of @city is: <? ($cities.filter("y", "y.name == @city").![population]).get(0) ?>.
```
{: codeblock}

표현식은 `The population of Tokyo is 9273000.`을 리턴합니다.

### JSONArray.get(Integer)

이 메소드는 JSONArray에서 입력 인덱스를 리턴합니다.

이전 노드 또는 외부 애플리케이션에서 설정된 대화 런타임 컨텍스트의 경우:

```json
{
  "context": {
    "name": "John",
    "nested": {
      "array": [ "one", "two" ]
    }
  }
}
```
{: codeblock}

대화 노드 또는 응답 조건:

```json
$nested.array.get(0).getAsString().contains('one')
```
{: codeblock}

결과:
중첩된 배열에 `one`이 값으로 포함되어 있으므로 `True`입니다.

응답:

```json
"output": {
  "generic":[
      {
      "values": [
        {
        "text" : "The first item in the array is <?$nested.array.get(0)?>"
        }
      ],
      "response_type": "text",
      "selection_policy": "sequential"
    }
  ]
  }
```
{: codeblock}

### JSONArray.getRandomItem()

이 메소드는 입력 JSONArray에서 랜덤 항목을 리턴합니다.

다음 대화 런타임 컨텍스트의 경우:

```json
{
  "context": {
    "toppings_array": ["onion", "olives", "ham"]
  }
}
```
{: codeblock}

대화 노드 출력:

```json
{
  "output": {
  "generic":[
      {
      "values": [
        {
    "text": "<? $toppings_array.getRandomItem() ?> is a great choice!"
        }
      ],
      "response_type": "text",
      "selection_policy": "sequential"
    }
  ]
  }
}
```
{: codeblock}

결과: `"ham is a great choice!"` 또는 `"onion is a great choice!"` 또는 `"olives is a great choice!"`

**참고:** 결과 출력 텍스트는 임의로 선택됩니다.

### JSONArray.indexOf(value)
{: #dialog-methods-array-indexOf}

이 메소드는 값을 배열에서 찾을 수 없는 경우 매개변수 또는 `-1`로 지정하는 값과 일치하는 배열에 있는 요소의 인덱스 번호를 리턴합니다.
값은 String(`"School"`), Integer(`8`) 또는 Double(`9.1`)이 될 수 있습니다.
값은 정확히 일치해야 하며 대소문자를 구분합니다.

예를 들어, 다음 컨텍스트 변수는 배열을 포함합니다.

```json
{
  "context": {
    "array1": ["Mary","Lamb","School"],
    "array2": [8,9,10],
    "array3": [8.1,9.1,10.1]
  }
}
```

다음 표현식을 사용하여 값이 지정된 배열 인덱스를 판별할 수 있습니다.

```bash
<? $array1.indexOf("Mary") ?> returns `0`
<? $array2.indexOf(9) ?> returns `1`
<? $array3.indexOf(10.1) ?> returns `2`
```

예를 들어, 이 메소드는 인텐트 배열에서 요소의 인덱스를 가져오는 데 유용할 수 있습니다. 사용자 입력을 평가하여 특정 인텐트의 배열 인덱스 번호를 판별할 때마다 생성되는 인텐트 배열에 `indexOf` 메소드를 적용할 수 있습니다.

```bash
intents.indexOf("General_Greetings")
```
{: codeblock}

특정 인텐트에 대한 신뢰도 점수를 알고 싶은 경우, 구문 `intents[`*`index`*`].confidence`가 있는 표현식에 위의 표현식을 *`index`* 값으로 전달할 수 있습니다. 예:

```bash
intents[intents.indexOf("General_Greetings")].confidence
```
{: codeblock}

### JSONArray.join(String delimiter)

이 메소드는 이 배열의 모든 값을 하나의 문자열로 결합합니다. 값은 문자열로 변환되고 입력 구분 기호로 구분됩니다.

다음 대화 런타임 컨텍스트의 경우:

```json
{
  "context": {
    "toppings_array": ["onion", "olives", "ham"]
  }
}
```
{: codeblock}

대화 노드 출력:

```json
{
  "output": {
  "generic":[
      {
      "values": [
        {
    "text": "This is the array: <? $toppings_array.join(';') ?>"
        }
      ],
      "response_type": "text",
      "selection_policy": "sequential"
    }
  ]
  }
}
```
{: codeblock}

결과:

```json
This is the array: onion;olives;ham;
```
{: codeblock}

JSON 배열에 여러 값을 저장하는 변수를 정의하는 경우, 배열에서 값의 서브세트를 리턴한 후 join() 메소드를 사용하여 이를 적절하게 형식화할 수 있습니다.

#### 콜렉션 투영법(collection projection)
{: #dialog-methods-collection-projection}

`collection projection` SpEL 표현식은 오브젝트를 포함하고 있는 배열에서 서브콜렉션을 추출합니다. 콜렉션 투영법의 구문은 `array_that_contains_value_sets.![value_of_interest]`입니다.

예를 들어, 다음 컨텍스트 변수는 항공편 정보를 저장하는 JSON 배열을 정의합니다. 항공편, 시간 및 항공편 코드당 2개의 데이터 점이 있습니다.

```json
"flights_found": [
  {
    "time": "10:00",
    "flight_code": "OK123"
  },
  {
    "time": "12:30",
    "flight_code": "LH421"
  },
  {
    "time": "16:15",
    "flight_code": "TS4156"
  }
]
```
{: codeblock}

항공편 코드만 리턴하려면 다음 구문을 사용하여 콜렉션 투영법 표현식을 작성할 수 있습니다.

```
<? $flights_found.![flight_code] ?>
```

이 표현식은 `flight_code` 값의 배열을 `["OK123","LH421","TS4156"]`으로 리턴합니다. 자세한 내용은 [SpEL 콜렉션 투영법 문서](https://docs.spring.io/spring/docs/3.0.x/reference/expressions.html)를 참조하십시오.

리턴되는 배열의 값에 `join()` 메소드를 적용하는 경우, 항공편 코드는 쉼표로 구분된 목록으로 표시됩니다. 예를 들어, 응답에서 다음 구문을 사용할 수 있습니다.

```
The flights that fit your criteria are:
  <? T(String).join(",", $flights_found.![flight_code]) ?>.
```
{: codeblock}

결과: `The flights that match your criteria are: OK123,LH421,TS4156.`

### JSONArray.joinToArray(template)
{: #dialog-methods-joinToArray}

이 메소드는 템플리트에서 정의한 형식을 배열에 적용하고 스펙에 따라 형식화된 배열을 리턴합니다. 예를 들어, 이 메소드는 대화 응답에서 리턴하려는 배열 값에 형식화를 적용하는 데 유용합니다.

템플리트를 문자열, JSON 오브젝트 또는 JSON 배열로 지정할 수 있습니다. 템플리트에서 편집 중인 배열에서 값을 참조하려면 다음 구문 규칙을 따르십시오.

- `%`: 편집 중인 배열에서 리턴하려는 요소 또는 요소 특성의 시작 또는 끝을 표시합니다.
- `e`: 형식화를 적용할 배열 요소를 일시적으로 표시합니다. 이 임시 변수 이름을 `e`에서 변경할 수 없습니다.

예를 들어, 세 개의 항공편에 대한 항공편 세부사항 목록이 있는 배열이 포함된 컨텍스트 변수가 있습니다.

```json
"flights": [
      {
        "flight": "DL1040",
        "origin": "JFK",
        "carrier": "Alitalia",
        "duration": 485,
        "destination": "FCO",
        "arrival_date": "2019-02-03",
        "arrival_time": "07:00",
        "departure_date": "2019-02-02",
        "departure_time": "16:45"
      },
      {
        "flight": "DL1710",
        "origin": "JFK",
        "carrier": "Delta",
        "duration": 379,
        "destination": "LAX",
        "arrival_date": "2019-02-02",
        "arrival_time": "10:19",
        "departure_date": "2019-02-02",
        "departure_time": "07:00"
      },
      {
        "flight": "DL4379",
        "origin": "BOS",
        "carrier": "Virgin Atlantic",
        "duration": 385,
        "destination": "LHR",
        "arrival_date": "2019-02-03",
        "arrival_time": "09:05",
        "departure_date": "2019-02-02",
        "departure_time": "21:40"
      }
    ]
```
{: codeblock}

항공편 코드 목록만 리턴하려고 합니다. 각 배열에서 `flight` 요소의 값만을 추출하여 목록에서 리턴하기 위해 다음 표현식을 사용할 수 있습니다.

```
The available flights are <? $flights.joinToArray("%e.flight%"). ?>
```
{: codeblock}

대화 노드 응답은 `The available flights are ["DL1040","DL1710","DL4379"]`입니다.

배열을 텍스트로 표시하려면 다음과 같이 표현식에서 `join` 메소드를 사용하십시오.

```
The available flights are <? $flights.joinToArray("%e.flight%").join(", "). ?>
```
{: codeblock}

응답은 `The available flights are DL1040, DL1710, DL4379.`입니다.

#### 복합 템플리트
{: #dialog-methods-complex-template}

메소드 매개변수에 직접 템플리트 세부사항을 지정하는 대신 더 복잡한 템플리트를 작성하기 위해 컨텍스트 변수를 작성할 수 있습니다.

이 템플리트 컨텍스트 변수는 배열 요소의 서브세트를 포함하며 그 앞에 레이블을 추가하므로, 정보가 응답에서 읽기 가능한 목록으로 표시됩니다.

```json
"template": "<br/>Flight number: %e.flight% <br/> Airline: %e.carrier% <br/> Departure date: %e.departure_date% <br/> Departure time: %e.departure_time% <br/> Arrival time: %e.arrival_time% <br/>"
```
{: codeblock}

`<br/>` HTML 태그(행 바꾸기에 사용됨)는 Facebook 및 Slack과 같은 일부 통합 채널에서 렌더링되지 *않습니다*.
{: note}

대화 노드 응답에서 이 표현식을 사용하여 `$template`에 정의된 템플리트를 `$flights`의 배열에 적용하십시오.

```
The flight info is <? $flights.joinToArray($template).join(" ") ?>
```
{: codeblock}

결과는 다음과 같습니다.

```
The flight info is
Flight number: DL1040
Airline: Alitalia
Departure date: 2019-02-02
Departure time: 16:45
Arrival time: 07:00

Flight number: DL1710
Airline: Delta
Departure date: 2019-02-02
Departure time: 07:00
Arrival time: 10:19

Flight number: DL4379
Airline: Virgin Atlantic
Departure date: 2019-02-02
Departure time: 21:40
Arrival time: 09:05
```
{: screen}

이 메소드의 장점은 배열의 값이 얼마나 자주 변경되는지 또는 배열의 요소 수가 증가하는지가 중요하지 않다는 점입니다. 각 배열 요소에 적어도 템플리트가 참조하는 특성의 서브세트가 포함되어 있는 한, 표현식이 작동합니다.

#### JSON 오브젝트 템플리트 예제
{: #dialog-methods-object-template}

이 예제에서, 템플릿 컨텍스트 변수는 `$flights` 컨텍스트 변수의 배열에 지정된 각각의 항공편 요소로부터 항공편 번호, 도착 및 출발 날짜와 시간을 추출하는 JSON 오브젝트로서 정의됩니다. 예를 들어, 이 방법을 사용하여 두 개의 다른 항공사에서 관리하는 항공편의 항공편 세부사항에 표준 형식을 적용하고 항공편 정보를 웹 서비스에서 다르게 형식화할 수 있습니다.

```json
"template": {
      "departure": "Flight %e.flight% departs on %e.departure_date% at %e.departure_time%.",
      "arrival": "Flight %e.flight% arrives on %e.arrival_date% at %e.arrival_time%."
    }
```
{: codeblock}

리턴된 배열에서 오브젝트를 읽고 대화 봇의 응답에 적합하게 값을 형식화하도록 사용자 정의 클라이언트 애플리케이션을 설계할 수 있습니다. 대화 노드 응답은 다음 표현식을 사용하여 항공편 도착 세부사항 오브젝트를 배열로 리턴할 수 있습니다.

```
<? $flights.joinToArray($template) ?>
```
{: screen}

다음은 대화 노드 응답입니다.

```json
[
  {
    "arrival":"Flight DL1040 arrives on 2019-02-03 at 07:00.",
    "departure":"Flight DL1040 departs on 2019-02-02 at 16:45."
    },
  {
    "arrival":"Flight DL1710 arrives on 2019-02-02 at 10:19.",
    "departure":"Flight DL1710 departs on 2019-02-02 at 07:00."
    },
  {
    "arrival":"Flight DL4379 arrives on 2019-02-03 at 09:05.",
    "departure":"Flight DL4379 departs on 2019-02-02 at 21:40."
    }
  ]
  ```

`arrival` 요소와 `departure` 요소의 순서가 응답에서 바뀌었습니다. 일반적으로 서비스는 JSON 오브젝트에서 요소를 다시 정렬합니다. 요소를 특정 순서로 리턴하려면 그 대신 JSON 배열 또는 문자열 값을 사용하여 템플리트를 정의하십시오.

### JSONArray.remove(Integer)

이 메소드는 JSONArray에서 인덱스 위치의 요소를 제거하고 업데이트된 JSONArray를 리턴합니다.

다음 대화 런타임 컨텍스트의 경우:

```json
{
  "context": {
    "toppings_array": ["onion", "olives"]
  }
}
```
{: codeblock}

이를 다음과 같이 업데이트합니다.

```json
{
  "context": {
    "toppings_array": "<? $toppings_array.remove(0) ?>"
  }
}
```
{: codeblock}

결과:

```json
{
  "context": {
    "toppings_array": ["olives"]
  }
}
```
{: codeblock}

### JSONArray.removeValue(object)

이 메소드는 JSONArray에서 값의 첫 번째 발생을 제거하고 업데이트된 JSONArray를 리턴합니다.

다음 대화 런타임 컨텍스트의 경우:

```json
{
  "context": {
    "toppings_array": ["onion", "olives"]
  }
}
```
{: codeblock}

이를 다음과 같이 업데이트합니다.

```json
{
  "context": {
    "toppings_array": "<? $toppings_array.removeValue('onion') ?>"
  }
}
```
{: codeblock}

결과:

```json
{
  "context": {
    "toppings_array": ["olives"]
  }
}
```
{: codeblock}

### JSONArray.set(Integer index, Object value)

이 메소드는 JSONArray의 입력 인덱스를 입력 값으로 설정하고 수정된 JSONArray를 리턴합니다.

다음 대화 런타임 컨텍스트의 경우:

```json
{
  "context": {
    "toppings_array": ["onion", "olives", "ham"]
  }
}
```
{: codeblock}

대화 노드 출력:

```json
{
  "context": {
    "toppings_array": "<? $toppings_array.set(1,'ketchup')?>"
  }
}
```
{: codeblock}

결과:

```json
{
  "context": {
    "toppings_array": ["onion", "ketchup", "ham"]
  }
}
```
{: codeblock}

### JSONArray.size()

이 메소드는 JSONArray의 크기를 정수로 리턴합니다.

다음 대화 런타임 컨텍스트의 경우:

```json
{
  "context": {
    "toppings_array": ["onion", "olives"]
  }
}
```
{: codeblock}

이를 다음과 같이 업데이트합니다.

```json
{
  "context": {
    "toppings_array_size": "<? $toppings_array.size() ?>"
  }
}
```
{: codeblock}

결과:

```json
{
  "context": {
    "toppings_array_size": 2
  }
}
```
{: codeblock}

### JSONArray split(String regexp)

이 메소드는 입력 정규식을 사용하여 입력 문자열을 분할합니다. 결과는 문자열의 JSONArray입니다.

다음 입력의 경우:

```
"bananas;apples;pears"
```
{: codeblock}

다음 구문을 사용합니다.

```json
{
  "context": {
    "array": "<?input.text.split(";")?>
  }
}
```
{: codeblock}

다음 출력이 결과입니다.

```json
{
  "context": {
    "array": [ "bananas", "apples", "pears" ]
  }
}
```
{: codeblock}

### com.google.gson.JsonArray support
{: #dialog-methods-com.google.gson.JsonArray}

내장 메소드 외에도, `com.google.gson.JsonArray` 클래스의 표준 메소드를 사용할 수 있습니다.

#### 새 배열

new JsonArray().append('value')

사용자가 제공하는 값으로 채워지는 새 배열을 정의하려면, 배열을 인스턴스화할 수 있습니다. 또한 이를 인스턴스화할 때 배열에 플레이스홀더 값을 추가해야 합니다. 다음 구문을 사용하여 수행할 수 있습니다.

```json
{
  "context":{
    "answer": "<? output.answer?:new JsonArray().append('temp_value') ?>"
  }
```
{: codeblock}

## 날짜 및 시간
{: #dialog-methods-date-time}

날짜 및 시간에 대한 작업에 몇 가지 메소드를 사용할 수 있습니다.

사용자 입력에서 날짜 및 시간 정보를 인식하고 추출하는 방법에 대한 정보는 [@sys-date 및 @sys-time 엔티티](/docs/services/assistant?topic=assistant-system-entities#system-entities-sys-date-time)를 참조하십시오.

### .after(String date or time)

날짜/시간 값이 날짜/시간 인수 다음에 있는지 여부를 판별합니다.

### .before(String date or time)
날짜/시간 값이 날짜/시간 인수 앞에 있는지 여부를 판별합니다.

예:
- @sys-time.before('12:00:00')
- @sys-date.before('2016-11-21')

- 여러 항목(`time vs. date`, `date vs. time` 및 `time vs. date and time`)을 비교하는 경우 메소드는 false를 리턴하며 예외가 응답 JSON 로그 `output.log_messages`에 인쇄됩니다.

  예를 들어, `@sys-date.before(@sys-time)`입니다.
- `date and time vs. time`을 비교하는 경우 메소드가 날짜는 무시하고 시간만 비교합니다.

### now()

`yyyy-MM-dd HH:mm:ss` 형식의 현재 날짜 및 시간이 포함된 문자열을 리턴합니다.

- 정적 함수입니다.
- 다른 날짜/시간 메소드는 이 함수를 통해 리턴되는 날짜-시간 값에서 호출될 수 있으며 해당 인수로 전달될 수 있습니다.
- 컨텍스트 변수 `$timezone`이 설정되어 있으면 이 함수가 고객 시간대의 날짜 및 시간을 리턴합니다.

출력 필드에 사용되는 `now()`가 있는 대화 노드 예제:

```json
{
  "conditions": "#what_time_is_it",
  "output": {
    "generic":[
      {
        "values": [
          {
          "text": "<? now() ?>"
          }
        ],
        "response_type": "text",
        "selection_policy": "sequential"
      }
    ]
  }
}
```
{: codeblock}

노드 조건의 `now()` 예제(아직 오전인지 여부 결정):

```json
{
  "conditions": "now().before('12:00:00')",
  "output": {
      "generic":[
      {
        "values": [
          {
          "text": "Good morning!"
          }
        ],
        "response_type": "text",
        "selection_policy": "sequential"
        }
      ]
  }
}
```
{: codeblock}

### .reformatDateTime(String format)

날짜 및 시간 문자열을 사용자 출력에 대해 원하는 형식으로 형식화합니다.

지정된 형식에 따라 형식화된 문자열을 리턴합니다.

- 12/31/2016의 경우 `MM/dd/yyyy`
- 10pm의 경우 `h a`

요일을 다음과 같이 리턴합니다.

- 화요일의 경우 `E`
- 요일 인덱스(1 = 월요일, ..., 7 = 일요일)의 경우 `u`

예를 들어, 이 컨텍스트 변수 정의는 17:30:00 값을 *5:30 PM*으로 저장하는 $time 변수를 작성합니다.

```json
{
  "context": {
    "time": "<? @sys-time.reformatDateTime('h:mm a') ?>"
  }
}
```
{: codeblock}

형식은 Java [SimpleDateFormat ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](http://docs.oracle.com/javase/6/docs/api/java/text/SimpleDateFormat.html){: new_window} 규칙을 따릅니다.

**참고**: 시간만 형식화하려는 경우, 날짜는 `1970-01-01`로 처리됩니다.

### .sameMoment(String date/time)

- 날짜/시간 값이 날짜/시간 인수와 같은지 여부를 판별합니다.

### .sameOrAfter(String date/time)

- 날짜/시간 값이 날짜/시간 인수 이후인지 여부를 판별합니다.
- `.after()`와 유사합니다.

### .sameOrBefore(String date/time)

- 날짜/시간 값이 날짜/시간 인수 이전인지 여부를 판별합니다.

### today()

`yyyy-MM-dd` 형식의 현재 날짜가 포함된 문자열을 리턴합니다.

- 정적 함수입니다.
- 다른 날짜 메소드는 이 함수를 통해 리턴되는 날짜 값에서 호출될 수 있으며 해당 인수로 전달될 수 있습니다.
- 컨텍스트 변수 `$timezone`이 설정되어 있으면 이 함수가 고객 시간대의 날짜를 리턴합니다. 그렇지 않은 경우, `GMT` 시간대가 사용됩니다.

출력 필드에 사용되는 `today()`가 있는 대화 노드의 예제:

```json
{
  "conditions": "#what_day_is_it",
  "output": {
    "generic":[
      {
        "values": [
          {
          "text": "Today's date is <? today() ?>."
          }
        ],
        "response_type": "text",
        "selection_policy": "sequential"
      }
    ]
  }
}
```
{: codeblock}

결과: `Today's date is 2018-03-09.`

## 날짜 및 시간 계산
{: #dialog-methods-calculations}

다음 메소드를 사용하여 날짜를 계산하십시오.

| 메소드                  | 설명        |
|-------------------------|-------------|
| `<date>.minusDays(n)`   | 지정된 날짜에서 n일 전인 날의 날짜를 리턴합니다. |
| `<date>.minusMonths(n)` | 지정된 날짜에서 n달 전인 날의 날짜를 리턴합니다. |
| `<date>.minusYears(n)`  | 지정된 날짜에서 n년 전인 날의 날짜를 리턴합니다. |
| `<date>.plusDays(n)`   | 지정된 날짜에서 n일 후인 날의 날짜를 리턴합니다. |
| `<date>.plusMonths(n)` | 지정된 날짜에서 n달 후인 날의 날짜를 리턴합니다. |
| `<date>.plusYears(n)`  | 지정된 날짜에서 n년 후인 날의 날짜를 리턴합니다. |

여기서 `<date>`는 `yyyy-MM-dd` 또는 `yyyy-MM-dd HH:mm:ss` 형식으로 지정됩니다.

내일 날짜를 사용하려면 다음 표현식을 지정하십시오.

```json
{
  "output": {
    "generic":[
      {
        "values": [
          {
          "text": "Tomorrow's date is <? today().plusDays(1) ?>."
          }
        ],
        "response_type": "text",
        "selection_policy": "sequential"
      }
    ]
  }
}
```
{: codeblock}

오늘이 2018년 3월 9일인 경우, 결과: `Tomorrow's date is 2018-03-10.`   

오늘부터 1주일 후의 날짜를 가져오려면 다음 표현식을 지정하십시오.

```json
{
  "output": {
    "generic":[
      {
        "values": [
          {
          "text": "Next week's date is <? @sys-date.plusDays(7) ?>."
          }
        ],
        "response_type": "text",
        "selection_policy": "sequential"
      }
    ]
  }
}
```
{: codeblock}

@sys-date 엔티티에서 캡처한 날짜가 오늘 날짜 2018년 3월 9일인 경우, 결과: `Next week's date is 2018-03-16.`

지난 달의 날짜를 사용하려면 다음 표현식을 지정하십시오.

```json
{
  "output": {
    "generic":[
      {
        "values": [
          {
          "text": "Last month the date was <? today().minusMonths(1) ?>."
          }
        ],
        "response_type": "text",
        "selection_policy": "sequential"
      }
    ]
  }
}
```
{: codeblock}

오늘이 2018년 3월 9일인 경우, 결과: `Last month the date was 2018-02-9.`   

다음 메소드를 사용하여 시간을 계산하십시오.

| 메소드                  | 설명        |
|-------------------------|-------------|
| `<time>.minusHours(n)`   | 지정된 시간에서 n시간 전인 시간을 리턴합니다. |
| `<time>.minusMinutes(n)` | 지정된 시간에서 n분 전인 시간을 리턴합니다. |
| `<time>.minusSeconds(n)`  | 지정된 시간에서 n초 전인 시간을 리턴합니다. |
| `<time>.plusHours(n)`   | 지정된 시간에서 n시간 후인 시간을 리턴합니다. |
| `<time>.plusMinutes(n)` | 지정된 시간에서 n분 후인 시간을 리턴합니다. |
| `<time>.plusSeconds(n)`  | 지정된 시간에서 n초 후인 시간을 리턴합니다. |

여기서, `<time>`은 `HH:mm:ss` 형식으로 지정됩니다.

지금부터 한 시간 후의 시간을 알려면 다음 표현식을 지정하십시오.

```json
{
  "output": {
    "generic":[
      {
        "values": [
          {
          "text": "One hour from now is <? now().plusHours(1) ?>."
          }
        ],
        "response_type": "text",
        "selection_policy": "sequential"
      }
    ]
  }
}
```
{: codeblock}

오전 8시인 경우, 결과: `One hour from now is 09:00:00.`

30분 전 시간을 알려면 다음 표현식을 지정하십시오.

```json
{
  "output": {
    "generic":[
      {
        "values": [
          {
          "text": "A half hour before @sys-time is <? @sys-time.minusMinutes(30) ?>."
          }
        ],
        "response_type": "text",
        "selection_policy": "sequential"
      }
    ]
  }
}
```
{: codeblock}

@sys-time 엔티티에서 캡처한 시간이 오전 8시인 경우, 결과: `A half hour before 08:00:00 is 07:30:00.`

리턴되는 시간을 다시 형식화하기 위해 다음 표현식을 사용할 수 있습니다.

```json
{
  "output": {
    "generic":[
      {
        "values": [
          {
          "text": "6 hours ago was <? now().minusHours(6).reformatDateTime('h:mm a') ?>."
          }
        ],
        "response_type": "text",
        "selection_policy": "sequential"
      }
    ]
  }
}
```
{: codeblock}

오후 2시 19분인 경우, 결과: `6 hours ago was 8:19 AM.`

### 시간 범위에 대한 작업
{: #dialog-methods-time-spans}

오늘 날짜가 특정 시간 프레임 내에 있는지 여부에 따라 응답을 표시하기 위해 시간 관련 메소드를 조합하여 사용할 수 있습니다. 예를 들어, 매년 명절 기간에 특별 오퍼를 실행하는 경우 오늘 날짜가 올해 11월 25일에서 12월 24일 사이인지 여부를 확인할 수 있습니다. 먼저 중요한 날짜를 컨텍스트 변수로 정의하십시오.

다음 시작 및 종료 날짜 컨텍스트 변수 표현식에서, 날짜는 동적으로 파생된 현재 연도 값을 하드 코딩된 월 및 일 값과 연결하여 작성됩니다.

```json
"context": {
   "end_date": "<? now().reformatDateTime('Y') + '-12-24' ?>",
   "start_date": "<? now().reformatDateTime('Y') + '-11-25' ?>"
 }
```

응답 조건에서, 현재 날짜가 컨텍스트 변수로 정의된 시작 및 종료 날짜 사이에 있는 경우에만 응답을 표시하도록 나타낼 수 있습니다.

`now().after($start_date) && now().before($end_date)`

### java.util.Date support
{: #dialog-methods-java.util.Date}

내장 메소드 외에도, `java.util.Date` 클래스의 표준 메소드를 사용할 수 있습니다.

오늘부터 일주일 후의 날짜를 가져오려면 다음 구문을 사용할 수 있습니다.

```json
{
  "context": {
    "week_from_today": "<? new Date(new Date().getTime() +
      (7 * (24*60*60*1000L))) ?>"
  }
}
```
{: codeblock}

이 표현식은 먼저 밀리 초 단위로 현재 날짜를 가져옵니다(1970 년 1 월 1 일 00:00:00 GMT 이후). 또한 7 일간의 밀리 초를 계산합니다. (`(24*60*60*1000L)`은 하루를 밀리 초 단위로 나타냅니다.) 그런 다음 현재 날짜에 7일을 추가합니다. 결과는 오늘부터 일주일 후에 해당하는 날의 전체 날짜입니다. 예: `Fri Jan 26 16:30:37 UTC 2018`. 시간은 UTC 시간대입니다. 7을 전달할 수 있는 변수(예:`$number_of_days`)로 변경할 수 있습니다. 이 표현식을 평가하기 전에 해당 값이 설정되었는지 확인하십시오.

나중에 날짜를 서비스에서 생성한 다른 날짜와 비교하려는 경우, 날짜를 재형식화해야 합니다. 시스템 엔티티(`@sys-date`) 및 기타 내장 메소드(`now()`)는 날짜를 `yyyy-MM-dd` 형식으로 변환합니다.

```json
{
  "context": {
    "week_from_today": "<? new Date(new Date().getTime() +
      (7 * (24*60*60*1000L))).format('yyyy-MM-dd') ?>"
  }
}
```
{: codeblock}

날짜를 재형식화하면, 결과는 `2018-01-26`입니다. 이제 응답 조건에서 `@sys-date.after($week_from_today)`와 같은 표현식을 사용하여 사용자 입력에 지정된 날짜와 컨텍스트 변수에 저장된 날짜를 비교할 수 있습니다.

다음 표현식은 지금부터 3시간 후를 계산합니다.

```json
{
  "context": {
    "future_time": "<? new Date(new Date().getTime() + (3 * (60*60*1000L)) -
      (5 * (60*60*1000L))).format('h:mm a') ?>"
  }
}
```
{: codeblock}

`(60*60*1000L)` 값은 밀리 초 단위로 시간을 나타냅니다. 이 표현식은 현재 시간에 3시간을 추가합니다. 그런 다음 UTC 표준 시간대에서 EST 표준 시간대로 5시간을 빼서 다시 계산합니다. 또한, 날짜 값이 AM 또는 PM에 시간과 분을 포함하도록 재형식화합니다.

## 숫자
{: #dialog-methods-numbers}

이러한 메소드를 사용하여 숫자 값을 가져오고 재형식화할 수 있습니다.

사용자 입력에서 숫자를 인식하고 추출할 수 있는 시스템 엔티티에 대한 정보는 [@sys-number 엔티티](/docs/services/assistant?topic=assistant-system-entities#system-entities-sys-number)를 참조하십시오.

서비스가 사용자 입력에서 특정 숫자 형식을 인식하도록 하려면, 이를 캡처하기 위한 패턴 엔티티 작성을 고려하십시오. 세부사항은 [엔티티 작성](/docs/services/assistant?topic=assistant-entities)을 참조하십시오.

예를 들어, 숫자를 통화 값으로 재형식화하기 위해 숫자에 대한 10진수 배치를 변경하려는 경우, [String format() 메소드](#dialog-methods-java.lang.String)를 참조하십시오.

### toDouble()

  오브젝트 또는 필드를 실수 유형으로 변환합니다. 오브젝트 또는 필드에서 이 메소드를 호출할 수 있습니다. 변환에 실패하면 *널*이 리턴됩니다.

### toInt()

  오브젝트 또는 필드를 정수 유형으로 변환합니다. 오브젝트 또는 필드에서 이 메소드를 호출할 수 있습니다. 변환에 실패하면 *널*이 리턴됩니다.

### toLong()

  오브젝트 또는 필드를 숫자(Long) 유형으로 변환합니다. 오브젝트 또는 필드에서 이 메소드를 호출할 수 있습니다. 변환에 실패하면 *널*이 리턴됩니다.

  SpEL 표현식에서 Long 숫자 유형을 지정하는 경우 숫자에 `L`을 추가해야합니다. 예: `5000000000L`. 이 구문은 32비트 정수 유형에 맞지 않는 모든 숫자에 대해 필요합니다. 예를 들어, 2 ^ 31 (2,147,483,648)보다 크거나 -2 ^ 31 (-2,147,483,648)보다 작은 숫자는 Long 숫자 유형으로 간주됩니다. Long 숫자 형식의 최소 값은 -2 ^ 63이고 최대 값은 2 ^ 63-1입니다.

### Java 숫자 지원
{: #dialog-methods-java.lang.Number}

### java.lang.Math()

기본 숫자 연산을 수행합니다.

다음을 포함하여 Class 메소드를 사용할 수 있습니다.

- max()

```json
{
  "context": {
    "bigger_number": "<? T(Math).max($number1,$number2) ?>"
  },
  "output": {
    "generic":[
      {
        "values": [
          {
          "text": "The bigger number is $bigger_number."
          }
        ],
        "response_type": "text",
        "selection_policy": "sequential"
      }
    ]
  }
}
```
{: codeblock}

- min()

```json
{
  "context": {
    "smaller_number": "<? T(Math).min($number1,$number2) ?>"
  },
  "output": {
    "generic":[
      {
        "values": [
          {
          "text": "The smaller number is $smaller_number."
          }
        ],
        "response_type": "text",
        "selection_policy": "sequential"
      }
    ]
  }
}
```
{: codeblock}

- pow()

```json
{
  "context": {
    "power_of_two": "<? T(Math).pow($base.toDouble(),2.toDouble()) ?>"
  },
  "output": {
    "generic":[
      {
        "values": [
          {
          "text": "Your number $base to the second power is $power_of_two."
          }
        ],
        "response_type": "text",
        "selection_policy": "sequential"
      }
    ]
  }
}
```
{: codeblock}

기타 메소드에 관한 정보는 [java.lang.Math 참조 문서](https://docs.oracle.com/javase/7/docs/api/java/lang/Math.html)를 참조하십시오.

### java.util.Random()

난수를 리턴합니다. 다음 구문 옵션 중 하나를 사용할 수 있습니다.

- 랜덤 부울 값(true 또는 false)을 리턴하려면 `<?new Random().nextBoolean()?>`를 사용하십시오.
- 0(포함됨)과 1(제외됨) 사이의 실수형 난수를 리턴하려면 `<?new Random().nextDouble()?>`를 사용하십시오.
- 0(포함됨)과 지정한 수 사이의 정수형 난수를 리턴하려면 `<?new Random().nextInt(n)?>`를 사용하십시오. 여기서 n은 숫자 범위(1을 더함)의 최대 수입니다. 예를 들어, 0과 10 사이의 난수를 리턴하려면 `<?new Random().nextInt(11)?>`를 지정하십시오.
- 전체 정수 값 범위(-2147483648에서 2147483648까지)에서 정수형 난수를 리턴하려면 `<?new Random().nextInt()?>`를 사용하십시오.

예를 들어, #random_number 인텐트로 트리거되는 대화 노드를 작성할 수 있습니다. 첫 번째 응답 조건은 다음과 같습니다.

```json
Condition = @sys-number
{
  "context": {
    "answer": "<? new Random().nextInt(@sys-number.numeric_value + 1) ?>"
  },
  "output": {
    "generic":[
      {
        "values": [
          {
          "text": "Here's a random number between 0 and @sys-number.literal: $answer."
          }
        ],
        "response_type": "text",
        "selection_policy": "sequential"
      }
    ]
  }
}
```
{: codeblock}

기타 메소드에 관한 정보는 [java.util.Random 참조 문서](https://docs.oracle.com/javase/7/docs/api/java/util/Random.html)를 참조하십시오.

다음 클래스의 표준 메소드도 사용할 수 있습니다.

- `java.lang.Byte`
- `java.lang.Integer`
- `java.lang.Long`
- `java.lang.Double`
- `java.lang.Short`
- `java.lang.Float`

## 오브젝트
{: #dialog-methods-objects}

### JSONObject.clear()

이 메소드는 JSON 오브젝트에서 모든 값을 지우고 널을 리턴합니다.

예를 들어, $user 컨텍스트 변수에서 현재 값을 지우려고 합니다.

```json
{
  "context": {
    "user": {
      "first_name": "John",
      "last_name": "Snow"
    }
  }
}
```
{: codeblock}

출력에서 다음 표현식을 사용하여 해당 값의 오브젝트를 지우는 필드를 정의하십시오.

```json
{
  "output": {
    "object_eraser": "<? $user.clear() ?>"
  }
}
```
{: codeblock}

차후에 $user 컨텍스트 변수를 참조하는 경우, `{}`만 리턴합니다.

API `/message` 호출의 본문에 있는 `context` 또는 `output` JSON 오브젝트에서 `clear()` 메소드를 사용할 수 있습니다.

#### 컨텍스트 지우기
{: #dialog-methods-clearing-context}

`clear()` 메소드를 사용하여 `context` 오브젝트를 지우는 경우, 다음을 제외한 **모든** 변수를 지웁니다.

 - `context.conversation_id`
 - `context.timezone`
 - `context.system`

**경고**: 모든 컨텍스트 변수 값은 다음을 의미합니다.

  - 현재 세션 중에 트리거된 노드의 변수에 대해 설정된 모든 기본값.
  - 현재 세션 중에 사용자 또는 외부 서비스에서 제공한 정보로 기본값에 수행된 모든 업데이트.

이 메소드를 사용하려면 출력 오브젝트에서 정의하는 변수의 표현식에서 이를 지정할 수 있습니다. 예:

```json
{
  "output": {
    "generic":[
      {
        "values": [
          {
          "text": "Response for this node."
          }
        ],
        "response_type": "text",
        "selection_policy": "sequential"
      }
    ],
    "context_eraser": "<? context.clear() ?>"
  }
}

```

#### 출력 지우기
{: #dialog-methods-clearing-output}

`clear()` 메소드를 사용하여 `output` 오브젝트를 지우는 경우, 현재 노드에서 정의하는 텍스트 응답과 출력 오브젝트를 지우는 데 사용하는 변수를 제외한 모든 변수를 지웁니다. 아래 변수도 지우지 않습니다.

- `output.nodes_visited`
- `output.nodes_visited_details`

이 메소드를 사용하려면 출력 오브젝트에서 정의하는 변수의 표현식에서 이를 지정할 수 있습니다. 예:

```json
{
  "output": {
    "generic":[
      {
        "values": [
          {
          "text": "Have a great day!"
          }
        ],
        "response_type": "text",
        "selection_policy": "sequential"
      }
    ],
    "output_eraser": "<? output.clear() ?>"
  }
}
```

트리에 있는 노드가 이전에 `I'm happy to help.`를 정의한 다음, 위에 정의된 JSON 출력 오브젝트가 있는 노드로 이동하는 경우 `Have a great day.`만 응답으로 표시됩니다. `I'm happy to help.` 출력은 표시되지 않습니다. 이유는 이 출력이 지워지고 `clear()` 메소드를 호출하는 노드의 텍스트 응답으로 대체되기 때문입니다.

### JSONObject.has(String)

이 메소드는 복합 JSONObject에 입력 이름의 특성이 있는 경우 true를 리턴합니다.

다음 대화 런타임 컨텍스트의 경우:

```json
{
  "context": {
    "user": {
      "first_name": "John",
      "last_name": "Snow"
    }
  }
}
```
{: codeblock}

대화 노드 출력:

```json
{
  "conditions": "$user.has('first_name')"
}
```
{: codeblock}

결과: 사용자 오브젝트에 `first_name` 특성이 있으므로 조건은 true입니다.

### JSONObject.remove(String)

이 메소드는 입력 `JSONObject`에서 이름의 특성을 제거합니다. 이 메소드에서 리턴되는 `JSONElement`는 제거되는 `JSONElement`입니다.

다음 대화 런타임 컨텍스트의 경우:

```json
{
  "context": {
    "user": {
      "first_name": "John",
      "last_name": "Snow"
    }
  }
}
```
{: codeblock}

대화 노드 출력:

```json
{
  "context": {
    "attribute_removed": "<? $user.remove('first_name') ?>"
  }
}
```
{: codeblock}

결과:

```json
{
  "context": {
    "user": {
      "last_name": "Snow"
    },
    "attribute_removed": {
      "first_name": "John"
    }
  }
}
```
{: codeblock}

### com.google.gson.JsonObject 지원
{: #dialog-methods-com.google.gson.JsonObject}

내장 메소드 외에도, `com.google.gson.JsonObject` 클래스의 표준 메소드를 사용할 수 있습니다.

## 문자열
{: #dialog-methods-strings}

텍스트로 작업하는 데 도움이 되는 메소드가 있습니다.

사용자 입력에서 특정 유형의 문자열(사용자 이름 및 위치)을 인식하고 추출하는 방법에 대한 정보는 [시스템 엔티티](/docs/services/assistant?topic=assistant-system-entities)를 참조하십시오.

**참고:** 정규식과 관련된 메소드의 경우, 정규식을 지정할 때 사용할 구문에 대한 세부사항은 [RE2 구문 참조 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/google/re2/wiki/Syntax){: new_window}를 참조하십시오.

### String.append(Object)

이 메소드는 입력 오브젝트를 문자열에 문자열로 추가하고 수정된 문자열을 리턴합니다.

다음 대화 런타임 컨텍스트의 경우:

```json
{
  "context": {
    "my_text": "This is a text."
  }
}
```
{: codeblock}

다음 구문을 사용합니다.

```json
{
  "context": {
    "my_text": "<? $my_text.append(' More text.') ?>"
  }
}
```
{: codeblock}

다음 출력이 결과입니다.

```json
{
  "context": {
    "my_text": "This is a text. More text."
  }
}
```
{: codeblock}

### String.contains(String)

이 메소드는 문자열에 입력 하위 문자열이 포함되어 있는 경우 true를 리턴합니다.

입력: "Yes, I'd like to go."

다음 구문을 사용합니다.

```json
{
  "conditions": "input.text.contains('Yes')"
}
```
{: codeblock}

결과: 조건이 `true`입니다.

### String.endsWith(String)

이 메소드는 문자열이 입력 하위 문자열로 끝나는 경우 true를 리턴합니다.

다음 입력의 경우:

```
"What is your name?".
```
{: codeblock}

다음 구문을 사용합니다.

```json
{
  "conditions": "input.text.endsWith('?')"
}
```
{: codeblock}

결과: 조건이 `true`입니다.

### String.extract(String regexp, Integer groupIndex)

이 메소드는 입력 정규식의 지정된 그룹 인덱스에 따라 추출된 문자열을 리턴합니다.

다음 입력의 경우:

```
"Hello 123456".
```
{: codeblock}

다음 구문을 사용합니다.

```json
{
  "context": {
    "number_extract": "<? input.text.extract('[\\d]+',0) ?>"
  }
}
```
{: codeblock}

  **중요:** `\\d`를 정규식으로 처리하려면 다른 `\\`: `\\\\d`를 추가하여 백슬래시를 둘 다 이스케이프 처리해야 합니다.

결과:

```json
{
  "context": {
    "number_extract": "123456"
  }
}
```
{: codeblock}

### String.find(String regexp)

이 메소드는 문자열의 세그먼트가 입력 정규식과 일치하는 경우 true를 리턴합니다.  이 메소드를 JSONArray 또는 JSONObject 요소에 대해 호출할 수 있으며, 비교하기 전에 배열 또는 오브젝트를 문자열로 변환합니다.

다음 입력의 경우:

```
"Hello 123456".
```
{: codeblock}

다음 구문을 사용합니다.

```json
{
  "conditions": "input.text.find('^[^\d]*[\d]{6}[^\d]*$')"
}
```
{: codeblock}

결과: 입력 텍스트의 숫자 부분이 정규식 `^[^\d]*[\d]{6}[^\d]*$`와 일치하므로 조건은 true입니다.

### String.isEmpty()

이 메소드는 문자열이 널이 아닌 빈 문자열인 경우 true를 리턴합니다.

다음 대화 런타임 컨텍스트의 경우:

```json
{
  "context": {
    "my_text_variable": ""
  }
}
```
{: codeblock}

다음 구문을 사용합니다.

```json
{
  "conditions": "$my_text_variable.isEmpty()"
}
```
{: codeblock}

결과: 조건이 `true`입니다.

### String.length()

이 메소드는 문자열의 문자 길이를 리턴합니다.

다음 입력의 경우:

```
"Hello"
```
{: codeblock}

다음 구문을 사용합니다.

```json
{
  "context": {
    "input_length": "<? input.text.length() ?>"
  }
}
```
{: codeblock}

다음 출력이 결과입니다.

```json
{
  "context": {
    "input_length": 5
  }
}
```
{: codeblock}

### String.matches(String regexp)

이 메소드는 문자열이 입력 정규식과 일치하는 경우 true를 리턴합니다.

다음 입력의 경우:

```
"Hello".
```
{: codeblock}

다음 구문을 사용합니다.

```json
{
  "conditions": "input.text.matches('^Hello$')"
}
```
{: codeblock}

결과: 입력 텍스트가 정규식 `\^Hello\$`와 일치하므로 조건은 true입니다.

### String.startsWith(String)

이 메소드는 문자열이 입력 하위 문자열로 시작되는 경우 true를 리턴합니다.

다음 입력의 경우:

```
"What is your name?".
```
{: codeblock}

다음 구문을 사용합니다.

```json
{
  "conditions": "input.text.startsWith('What')"
}
```
{: codeblock}

결과: 조건이 `true`입니다.

### String.substring(Integer beginIndex, Integer endIndex)

이 메소드는 `beginIndex`의 문자와 `endIndex` 앞에 인덱스로 설정된 마지막 문자가 포함된 하위 문자열을 가져옵니다. endIndex 문자는 포함되지 않습니다.

다음 대화 런타임 컨텍스트의 경우:

```json
{
  "context": {
    "my_text": "This is a text."
  }
}
```
{: codeblock}

다음 구문을 사용합니다.

```json
{
  "context": {
    "my_text": "<? $my_text.substring(5, $my_text.length()) ?>"
  }
}
```
{: codeblock}

다음 출력이 결과입니다.

```json
{
  "context": {
    "my_text": "is a text."
  }
}
```
{: codeblock}

### String.toLowerCase()

이 메소드는 소문자로 변환되는 원래 문자열을 리턴합니다.

다음 입력의 경우:

```
"This is A DOG!"
```
{: codeblock}

다음 구문을 사용합니다.

```json
{
  "context": {
    "input_lower_case": "<? input.text.toLowerCase() ?>"
  }
}
```
{: codeblock}

다음 출력이 결과입니다.

```json
{
  "context": {
    "input_upper_case": "this is a dog!"
  }
}
```
{: codeblock}

### String.toUpperCase()

이 메소드는 대문자로 변환되는 원래 문자열을 리턴합니다.

다음 입력의 경우:

```
"hi there".
```
{: codeblock}

다음 구문을 사용합니다.

```json
{
  "context": {
    "input_upper_case": "<? input.text.toUpperCase() ?>"
  }
}
```
{: codeblock}

다음 출력이 결과입니다.

```json
{
  "context": {
    "input_upper_case": "HI THERE"
  }
}
```
{: codeblock}

### String.trim()

이 메소드는 문자열의 시작과 끝에 있는 공백을 자르고 수정된 문자열을 리턴합니다.

다음 대화 런타임 컨텍스트의 경우:

```json
{
  "context": {
    "my_text": "   something is here    "
  }
}
```
{: codeblock}

다음 구문을 사용합니다.

```json
{
  "context": {
    "my_text": "<? $my_text.trim() ?>"
  }
}
```
{: codeblock}

다음 출력이 결과입니다.

```json
{
  "context": {
    "my_text": "something is here"
  }
}
```
{: codeblock}

### java.lang.String 지원
{: #java.lang.String}

내장 메소드 외에도, `java.lang.String` 클래스의 표준 메소드를 사용할 수 있습니다.

#### java.lang.String.format()

표준 Java 문자열 `format()` 메소드를 텍스트에 적용할 수 있습니다. 형식 세부사항을 지정하는 데 사용할 구문에 대한 정보는 [java.util.formatter 참조 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://docs.oracle.com/javase/7/docs/api/java/util/Formatter#syntax){: new_window}를 참조하십시오.

예를 들어, 다음 표현식은 3개의 10진 정수(1, 1 및 2)를 가져와서 이를 문장에 추가합니다.

```json
{
  "formatted String": "<? T(java.lang.String).format('%d + %d equals %d', 1, 1, 2) ?>"
}
```
{: codeblock}

결과: `1 + 1 equals 2`.

숫자에 대한 10진수 배치를 변경하려면 다음 구문을 사용하십시오.

```
{
  <? T(String).format('%.2f',<number to format>) ?>
}
```
{: codeblock}

예를 들어, 미화로 형식을 지정해야 하는 $number 변수가 `4.5`인 경우, 응답(예:`Your total is $<? T(String).format('%.2f',$number) ?>`)이 `Your total is $4.50`를 리턴합니다.    

## 간접 데이터 유형 변환
{: #dialog-methods-indirect-type-conversion}

예를 들어 노드 응답의 일부로 텍스트 내에 표현식을 포함하면 값은 문자열로 렌더링됩니다. 사용자가 원래의 데이터 유형에 렌더링되는 표현식을 원하는 경우, 이를 텍스트로 둘러싸지 마십시오.

예를 들어, 다음 표현식을 대화 노드 응답에 추가하여 사용자 입력에서 인식되는 엔티티를 문자열 형식으로 리턴할 수 있습니다.

```json
The entities are <? entities ?>.
```
{: codeblock}

사용자가 입력으로 *Hello now*를 지정하면, @sys-date 및 @sys-time 엔티티를 `now` 참조에서 트리거합니다. 엔티티 오브젝트는 배열이지만 표현식이 텍스트에 포함되어 있으므로 엔티티는 다음과 같은 문자열 형식으로 리턴됩니다.

```json
  The entities are 2018-02-02, 14:34:56.
```
{: codeblock}

응답에서 텍스트를 포함하지 않으면, 배열이 대신 리턴됩니다. 예를 들어, 응답이 텍스트로 둘러싸여 있지 않고 표현식으로만 지정됩니다.

```
  <? entities ?>
```
{: codeblock}

엔티티 정보는 원시 데이터 유형으로 배열로서 리턴됩니다.

```json
[
  {
    "entity":"sys-date","location":[6,9],"value":"2018-02-02","confidence":1,"metadata":{"calendar_type":"GREGORIAN","timezone":"America/New_York"}
  },
  {
    "entity":"sys-time","location":[6,9],"value":"14:33:22","confidence":1,"metadata":{"calendar_type":"GREGORIAN","timezone":"America/New_York"}
  }
  ]
```
{: codeblock}

또 다른 예제로서, 다음 $array 컨텍스트 변수는 배열이지만 $string_array 컨텍스트 변수는 문자열입니다.

```json
{
  "context": {
    "array": [
      "one",
      "two"
    ],
    "array_in_string": "this is my array: $array"
  }
}
```
{: codeblock}

시험 사용 분할창에서 다음 컨텍스트 변수값을 검사하는 경우 다음과 같이 지정된 값이 표시됩니다.

**$array** : `["one","two"]`

**$array_in_string** : `"this is my array: [\"one\",\"two\"]"`

나중에 `<? $array.removeValue('two') ?>`와 같은 $array 변수에서 배열 메소드를 수행할 수 있지만 $array_in_string 변수에서는 수행할 수 없습니다.
