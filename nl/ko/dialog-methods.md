---

copyright:
  years: 2015, 2018
lastupdated: "2018-02-05"

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

# 표현식 언어 메소드

컨텍스트 변수, 조건 또는 응답의 다른 위치에서 참조할 사용자 표현에서 추출한 값을 처리할 수 있습니다.
{: shortdesc}

## 평가 구문

다른 변수 내에서 변수 값을 늘리거나 텍스트 또는 컨텍스트 변수를 출력하는 메소드를 적용하려면, `<? expression ?>` 표현식 구문을 사용하십시오. 예:

- **숫자 특성 증분**
    - `"output":{"number":"<? output.number + 1 ?>"}`
- **오브젝트에서 메소드 호출**
    - `"context":{"toppings": "<? context.toppings.append( 'onions' ) ?>"}`

다음 섹션은 값을 처리하는데 사용할 수 있는 메소드를 설명합니다. 메소드는 데이터 유형별로 구성됩니다. 

- [배열](dialog-methods.html#arrays)
- [날짜 및 시간](dialog-methods.html#date-time)
- [숫자](dialog-methods.html#numbers)
- [오브젝트](dialog-methods.html#objects)
- [문자열](dialog-methods.html#strings)

## 배열
{: #arrays}

배열 값을 설정하는 동일한 노드 내 노드 조건 또는 응답 조건에서 배열의 값을 검사하는 데 다음 메소드를 사용할 수 없습니다.

### JSONArray.append(object)

이 메소드는 JSONArray에 새 값을 추가하고 수정된 JSONArray를 리턴합니다.

다음 대화 상자 런타임 컨텍스트의 경우:

```json
{
  "context": {
    "toppings_array": ["onion", "olives"]
  }
}
```
{: codeblock}

다음 업데이트를 작성하십시오.

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

### JSONArray.contains(object value)

이 메소드는 입력 JSONArray에 입력 값이 포함되어 있는 경우 true를 리턴합니다.

이전 노드 또는 외부 애플리케이션에서 설정된 대화 상자 런타임 컨텍스트의 경우:

```json
{
  "context": {
    "toppings_array": ["onion", "olives", "ham"]
  }
}
```
{: codeblock}

대화 상자 노드 또는 응답 조건:

```json
$toppings_array.contains('ham')
```
{: codeblock}

결과: 배열에 요소 햄이 포함되어 있으므로 `True`입니다.

### JSONArray.get(integer)

이 메소드는 JSONArray에서 입력 인덱스를 리턴합니다.

이전 노드 또는 외부 애플리케이션에서 설정된 대화 상자 런타임 컨텍스트의 경우:

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

대화 상자 노드 또는 응답 조건:

```json
$nested.array.get(0).getAsString().contains('one')
```
{: codeblock}

결과:
중첩된 배열에 `one`이 값으로 포함되어 있으므로 `True`입니다.

응답:

```json
"output": {
    "text": {
      "values": [
        "The first item in the array is <?$nested.array.get(0)?>"
      ],
      "selection_policy": "sequential"
    }
  }
```
{: codeblock}

### JSONArray.getRandomItem()

이 메소드는 입력 JSONArray에서 랜덤 항목을 리턴합니다.

다음 대화 상자 런타임 컨텍스트의 경우:

```json
{
  "context": {
    "toppings_array": ["onion", "olives", "ham"]
  }
}
```
{: codeblock}

대화 상자 노드 출력:

```json
{
  "output": {
    "text": "<? $toppings_array.getRandomItem() ?> is a great choice!"
  }
}
```
{: codeblock}

결과: `"ham is a great choice!"` 또는 `"onion is a great choice!"` 또는 `"olives is a great choice!"`

**참고:** 결과 출력 텍스트는 임의로 선택됩니다.

### JSONArray.join(string delimiter)

이 메소드는 이 배열의 모든 값을 문자열에 결합합니다. 값은 문자열로 변환되고 입력 구분 기호로 구분됩니다.

다음 대화 상자 런타임 컨텍스트의 경우:

```json
{
  "context": {
    "toppings_array": ["onion", "olives", "ham"]
  }
}
```
{: codeblock}

대화 상자 노드 출력:

```json
{
  "output": {
    "text": "This is the array: <? $toppings_array.join(';') ?>"
  }
}
```
{: codeblock}

결과:

```json
This is the array: onion;olives;ham;
```
{: codeblock}

### JSONArray.remove(integer)

이 메소드는 JSONArray에서 인덱스 위치의 요소를 제거하고 업데이트된 JSONArray를 리턴합니다.

다음 대화 상자 런타임 컨텍스트의 경우:

```json
{
  "context": {
    "toppings_array": ["onion", "olives"]
  }
}
```
{: codeblock}

다음 업데이트를 작성하십시오.

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

다음 대화 상자 런타임 컨텍스트의 경우:

```json
{
  "context": {
    "toppings_array": ["onion", "olives"]
  }
}
```
{: codeblock}

다음 업데이트를 작성하십시오.

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

### JSONArray.set(integer index, object value)

이 메소드는 JSONArray의 입력 인덱스를 입력 값으로 설정하고 수정된 JSONArray를 리턴합니다.

다음 대화 상자 런타임 컨텍스트의 경우:

```json
{
  "context": {
    "toppings_array": ["onion", "olives", "ham"]
  }
}
```
{: codeblock}

대화 상자 노드 출력:

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

다음 대화 상자 런타임 컨텍스트의 경우:

```json
{
  "context": {
    "toppings_array": ["onion", "olives"]
  }
}
```
{: codeblock}

다음 업데이트를 작성하십시오.

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

다음 구문:

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
{: #com.google.gson.JsonArray}

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
{: #date-time}

날짜 및 시간에 대한 작업에 몇 가지 메소드를 사용할 수 있습니다.

사용자 입력에서 날짜 및 시간 정보를 인식하고 추출하는 방법에 대한 정보는 [@sys-date 및 @sys-time 엔티티](system-entities.html#sys-datetime)를 참조하십시오. 

### .after(String date/time)

- 날짜/시간 값이 날짜/시간 인수 다음에 있는지 여부를 판별합니다.
- `.before()`와 비슷합니다.

### .before(String date or time)

- 예:
- @sys-time.before('12:00:00')
- @sys-date.before('2016-11-21')
- 날짜/시간 값이 날짜/시간 인수 앞에 있는지 여부를 판별합니다.
- 여러 항목(`time vs. date`, `date vs. time` 및 `time vs. date and time`)을 비교하는 경우 메소드는 false를 리턴하며 예외가 응답 JSON 로그 `output.log_messages`에 인쇄됩니다.
    - 예를 들어, `@sys-date.before(@sys-time)`입니다.
- `date and time vs. time`을 비교하는 경우 메소드가 날짜는 무시하고 시간만 비교합니다.

### now()

- 정적 함수.
- `yyyy-MM-dd HH:mm:ss` 형식의 현재 날짜 및 시간이 포함된 문자열을 리턴합니다.
- 다른 날짜/시간 메소드는 이 함수를 통해 리턴되는 날짜-시간 값에서 호출될 수 있으며 해당 인수로 전달될 수 있습니다.
- 컨텍스트 변수 `$timezone`이 설정되어 있으면 이 함수가 고객 시간대의 날짜 및 시간을 리턴합니다.

출력 필드에 사용되는 `now()`가 있는 대화 상자 노드 예제:

```json
{
  "conditions": "#what_time_is_it",
  "output": {
    "text": "<? now() ?>"
   }
}
```
{: codeblock}

노드 조건의 `now()` 예제(아직 오전인지 여부 결정):

```json
{
  "conditions": "now().before('12:00:00')",
  "output": {
    "text": "Good morning!"
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

### java.util.Date support
{: #java.util.Date}

내장 메소드 외에도, `java.util.Date` 클래스의 표준 메소드를 사용할 수 있습니다. 

#### 날짜 계산

오늘부터 일주일의 날짜를 가져오려면 다음 구문을 사용할 수 있습니다. 

```json
{
  "context": {
    "week_from_today": "<? new Date(new Date().getTime() +
      (7 * (24*60*60*1000L))) ?>"
  }
}
```
{: codeblock}

이 표현식은 먼저 밀리 초 단위로 현재 날짜를 가져옵니다(1970 년 1 월 1 일 00:00:00 GMT 이후). 또한 7 일간의 밀리 초를 계산합니다. (`(24*60*60*1000L)`은 하루를 밀리 초 단위로 나타냅니다.) 그런 다음 현재 날짜에 7일을 추가합니다. 결과는 오늘부터 일주일에 해당하는 날의 전체 날짜입니다. 예: `Fri Jan 26 16:30:37 UTC 2018`. 시간은 UTC 시간대입니다. 7을 전달할 수 있는 변수(예:`$number_of_days`)로 변경할 수 있습니다. 이 표현식을 평가하기 전에 해당 값이 설정되었는지 확인하십시오. 

나중에 날짜를 서비스에서 생성한 다른 날짜와 비교하려는 경우, 날짜를 재형식화해야 합니다. 시스템 엔티티(`@sys-date`) 및 기타 내장 메소드 (`now()`) 날짜를 `yyyy-MM-dd` 형식으로 변환합니다. 

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

다음 표현식은 지금부터 3 시간을 계산합니다. 

```json
{
  "context": {
    "future_time": "<? new Date(new Date().getTime() + (3 * (60*60*1000L)) -
      (5 * (60*60*1000L))).format('h:mm a') ?>"
  }
}
```
{: codeblock}

`(60*60*1000L)` 값은 밀리 초 단위로 시간을 나타냅니다. 이 표현식은 현재 시간에 3시간을 추가합니다. 그런 다음 UTC 표준 시간대에서 EST 표준 시간대로 5 시간을 빼서 다시 계산합니다. 또한, 날짜 값이 AM 또는 PM에 시간과 분을 포함하도록 재형식화합니다. 

## 숫자
{: #numbers}

이러한 메소드를 사용하여 숫자 값을 가져오고 재형식화할 수 있습니다. 

사용자 입력에서 숫자를 인식하고 추출할 수 있는 시스템 엔티티에 대한 정보는 [@sys-number 엔티티](system-entities.html#sys-number)를 참조하십시오. 

서비스가 사용자 입력에서 특정 숫자 형식을 인식하도록 하려면, 이를 캡처하기 위한 패턴 엔티티 작성을 고려하십시오. 세부사항은 [엔티티 작성](entities.html#creating-entities)을 참조하십시오. 

### toDouble()

  오브젝트 또는 필드를 실수 유형으로 변환합니다. 오브젝트 또는 필드에서 이 메소드를 호출할 수 있습니다. 변환에 실패하면 *null*이 리턴됩니다.

### toInt()

  오브젝트 또는 필드를 정수 유형으로 변환합니다. 오브젝트 또는 필드에서 이 메소드를 호출할 수 있습니다. 변환에 실패하면 *null*이 리턴됩니다.

### toLong()

  오브젝트 또는 필드를 숫자(Long) 유형으로 변환합니다. 오브젝트 또는 필드에서 이 메소드를 호출할 수 있습니다. 변환에 실패하면 *null*이 리턴됩니다.

  SpEL 표현식에서 Long 숫자 유형을 지정하는 경우 숫자에 `L`을 추가해야합니다. 예: `5000000000L`. 이 구문은 32비트 정수 유형에 맞지 않는 숫자에 대해서는 필요합니다. 예를 들어, 2 ^ 31 (2,147,483,648)보다 크거나 -2 ^ 31 (-2,147,483,648)보다 작은 숫자는 Long 숫자 유형으로 간주됩니다. Long 숫자 형식의 최소 값은 -2 ^ 63이고 최대 값은 2 ^ 63-1입니다. 

### Java 숫자 지원
{: #java.lang.Number}

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
    "text": {
      "values": [
        "The bigger number is $bigger_number."
      ],
      "selection_policy": "sequential"
    }
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
    "text": {
      "values": [
        "The smaller number is $smaller_number."
      ],
      "selection_policy": "sequential"
    }
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
    "text": {
      "values": [
        "Your number $base to the second power is $power_of_two."
      ],
      "selection_policy": "sequential"
    }
  }
}
```
{: codeblock}

기타 메소드에 관한 정보는 [java.lang.Math 참조 문서](https://docs.oracle.com/javase/7/docs/api/java/lang/Math.html)를 참조하십시오. 

### java.util.Random()

난수를 리턴합니다. 다음 구문 옵션 중 하나를 사용할 수 있습니다.

- 랜덤 부울 값(true 또는 false)을 리턴하려면 `<?new Random().nextBoolean()?>`를 사용하십시오.
- 0(포함됨)과 1(제외됨) 사이의 실수형 난수를 리턴하려면 `<?new Random().nextDouble()?>`를 사용하십시오.
- 0(포함됨)과 지정한 수 사이의 정수형 난수를 리턴하려면 `<?new Random().nextInt(n)?>`를 사용하십시오. 여기서 n은 숫자 범위(1을 더함)의 최대 수입니다.
  예를 들어, 0과 10 사이의 난수를 리턴하려면 `<?new Random().nextInt(11)?>`를 지정하십시오.
- 전체 정수 값 범위(-2147483648에서 2147483648까지)에서 정수형 난수를 리턴하려면 `<?new Random().nextInt()?>`를 사용하십시오.

예를 들어, #random_number 인텐트로 트리거되는 대화 상자 노드를 작성할 수 있습니다. 첫 번째 응답 조건은 다음과 같습니다.

```json
Condition = @sys-number
{
  "context": {
    "answer": "<? new Random().nextInt(@sys-number.numeric_value + 1) ?>"
  },
  "output": {
    "text": {
      "values": [
        "Here's a random number between 0 and @sys-number.literal: $answer."
      ],
      "selection_policy": "sequential"
    }
  }
}
```
{: codeblock}

기타 메소드에 관한 정보는 [java.util.Random 참조 문서](https://docs.oracle.com/javase/7/docs/api/java/util/Random.html)를 참조하십시오. 

다음 클래스의 표준 방법도 사용할 수 있습니다. 

- `java.lang.Byte`
- `java.lang.Integer`
- `java.lang.Long`
- `java.lang.Double`
- `java.lang.Short`
- `java.lang.Float`

## 오브젝트
{: #objects}

### JSONObject.has(string)

이 메소드는 복합 JSONObject에 입력 이름의 특성이 있는 경우 true를 리턴합니다.

다음 대화 상자 런타임 컨텍스트의 경우:

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

대화 상자 노드 출력:

```json
{
  "conditions": "$user.has('first_name')"
}
```
{: codeblock}

결과: 사용자 오브젝트에 `first_name` 특성이 있으므로 조건은 true입니다.

### JSONObject.remove(string)

이 메소드는 입력 `JSONObject`에서 이름의 특성을 제거합니다. 이 메소드에서 리턴되는 `JSONElement`는 제거되는 `JSONElement`입니다.

다음 대화 상자 런타임 컨텍스트의 경우:

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

대화 상자 노드 출력:

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
{: #com.google.gson.JsonObject}

내장 메소드 외에도, `com.google.gson.JsonObject` 클래스의 표준 메소드를 사용할 수 있습니다. 

## 문자열
{: #strings}

텍스트로 작업하는 데 도움이 되는 메소드가 있습니다. 

사용자 입력에서 특정 유형의 문자열(사용자 이름 및 위치)을 인식하고 추출하는 방법에 대한 정보는, [시스템 엔티티](system-entities.html)를 참조하십시오. 

**참고:** 정규식과 관련된 메소드의 경우, 정규식을 지정할 때 사용할 구문에 대한 세부사항은 [RE2 구문 참조 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/google/re2/wiki/Syntax){: new_window}를 참조하십시오. 

### String.append(오브젝트)

이 메소드는 입력 오브젝트를 문자열에 문자열로 추가하고 수정된 문자열을 리턴합니다.

다음 대화 상자 런타임 컨텍스트의 경우:

```json
{
  "context": {
    "my_text": "This is a text."
  }
}
```
{: codeblock}

다음 구문:

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

### String.contains(string)

이 메소드는 문자열에 입력 하위 문자열이 포함되어 있는 경우 true를 리턴합니다.

입력: "Yes, I'd like to go."

다음 구문:

```json
{
  "conditions": "input.text.contains('Yes')"
}
```
{: codeblock}

결과: 조건이 `true`입니다.

### String.endsWith(string)

이 메소드는 문자열이 입력 하위 문자열로 끝나는 경우 true를 리턴합니다.

다음 입력의 경우:

```
"What is your name?".
```
{: codeblock}

다음 구문:

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

다음 구문:

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

### String.find(string regexp)

이 메소드는 문자열의 세그먼트가 입력 정규식과 일치하는 경우 true를 리턴합니다.  이 메소드를 JSONArray 또는 JSONObject 요소에 대해 호출할 수 있으며, 비교하기 전에 배열 또는 오브젝트를 문자열로 변환합니다.

다음 입력의 경우:

```
"Hello 123456".
```
{: codeblock}

다음 구문:

```json
{
  "conditions": "input.text.find('^[^\d]*[\d]{6}[^\d]*$')"
}
```
{: codeblock}

결과: 입력 텍스트의 숫자 부분이 정규식 `^[^\d]*[\d]{6}[^\d]*$`와 일치하므로 조건은 true입니다.

### String.isEmpty()

이 메소드는 문자열이 널이 아닌 빈 문자열인 경우 true를 리턴합니다.

다음 대화 상자 런타임 컨텍스트의 경우:

```json
{
  "context": {
    "my_text_variable": ""
  }
}
```
{: codeblock}

다음 구문:

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

다음 구문:

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

### String.matches(string regexp)

이 메소드는 문자열이 입력 정규식과 일치하는 경우 true를 리턴합니다.

다음 입력의 경우:

```
"Hello".
```
{: codeblock}

다음 구문:

```json
{
  "conditions": "input.text.matches('^Hello$')"
}
```
{: codeblock}

결과: 입력 텍스트가 정규식 `\^Hello\$`와 일치하므로 조건은 true입니다.

### String.startsWith(string)

이 메소드는 문자열이 입력 하위 문자열로 시작되는 경우 true를 리턴합니다.

다음 입력의 경우:

```
"What is your name?".
```
{: codeblock}

다음 구문:

```json
{
  "conditions": "input.text.startsWith('What')"
}
```
{: codeblock}

결과: 조건이 `true`입니다.

### String.substring(int beginIndex, int endIndex)

이 메소드는 `beginIndex`의 문자와 `endIndex` 앞에 인덱스로 설정된 마지막 문자가 포함된 하위 문자열을 가져옵니다.
endIndex 문자는 포함되지 않습니다.

다음 대화 상자 런타임 컨텍스트의 경우:

```json
{
  "context": {
    "my_text": "This is a text."
  }
}
```
{: codeblock}

다음 구문:

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

다음 구문:

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

다음 구문:

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

다음 대화 상자 런타임 컨텍스트의 경우:

```json
{
  "context": {
    "my_text": "   something is here    "
  }
}
```
{: codeblock}

다음 구문:

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

I내장 메소드 외에도, `java.lang.String` 클래스의 표준 메소드를 사용할 수 있습니다.

#### java.lang.String.format()

표준 Java 문자열 `format()` 메소드를 텍스트에 적용할 수 있습니다. 형식 세부사항을 지정하는 데 사용할 구문에 대한 정보는 [java.util.formatter 참조 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://docs.oracle.com/javase/7/docs/api/java/util/Formatter.html#syntax){: new_window}를 참조하십시오. 

예를 들어, 다음 표현식은 3 진 정수(1, 1 및 2)를 가져와 문장에 추가합니다. 

```json
{
  "formatted String": "<? T(java.lang.String).format('%d + %d equals %d', 1, 1, 2) ?>"
}
```
{: codeblock}

결과: `1 + 1 equals 2`.

## 간접 데이터 유형 변환

예를 들어 노드 응답의 일부로 텍스트 내에 표현식을 포함하면 값은 문자열로 렌더링됩니다. 사용자가 원래의 데이터 유형에 렌더링되는 표현식을 원하는 경우, 텍스트로 묶지 않습니다.

예를 들어, 이 표현식을 대화 상자 노드 응답에 추가하여 사용자 입력에서 인식되는 엔티티를 문자열 형식으로 리턴할 수 있습니다. 

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

엔티티 정보는 배열로서 원시 데이터 유형으로 리턴됩니다. 

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

연습 분할창에서 다음 컨텍스트 변수값을 검사하는 경우 다음과 같이 지정된 값이 표시됩니다.

**$array** : `["one","two"]`

**$array_in_string** : `"this is my array: [\"one\",\"two\"]"`

나중에 `<? $array.removeValue('two') ?>`와 같은 $array 변수에서 배열 메소드를 수행할 수 있지만 $array_in_string 변수에서는 수행할 수 없습니다. 
