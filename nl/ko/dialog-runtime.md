---

copyright:
  years: 2015, 2019
lastupdated: "2018-02-21"

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
{:gif: data-image-type='gif'}

# 대화 처리 방법
{: #dialog-runtime}

사용자가 실행 중에 배치된 {{site.data.keyword.conversationshort}} 서비스의 인스턴스와 상호작용할 때 대화가 처리하는 방법에 대해 이해합니다.
{: shortdesc}

## 대화 호출 분석
{: #dialog-runtime-message-anatomy}

각 사용자 발화(utterance)는 /message API 호출로 대화에 전달됩니다. 여기에는 자세한 정보를 요청하는 대화에서 사용자가 프롬프트에 대해 응답하는 발화가 포함됩니다. 일부 구독 플랜은 API 호출 세트 번호를 포함하는데, 이것은 호출의 구성을 이해하는 도움이 됩니다. 단일 /message API 호출은 사용자로부터의 입력과 대화에서의 해당 응답으로 구성된 단일 대화 턴(turn)과 동일합니다.

/message API 호출 요청 및 응답의 본문에는 다음 오브젝트가 포함됩니다.

- `context`: 지속될 변수를 포함합니다. 한 호출에서 다음 호출로 정보를 전달하려면 애플리케이션 개발자는 각 후속 API 호출마다 이전 API 호출의 응답 컨텍스트를 전달해야 합니다. 예를 들어, 대화는 사용자의 이름을 수집한 다음 후속 노드에서 사용자를 이름으로 참조할 수 있습니다.

  ```json
  {
    "context" : {
      "user_name" : "<? @sys-person.literal ?>"
    }
  ```
  {: codeblock}

  자세한 정보는 [대화 턴(tuen) 사이의 정보 유지](#dialog-runtime-context)를 참조하십시오.

- `input`: 사용자가 제출한 텍스트 문자열입니다. 텍스트 문자열은 최대 2,048자를 포함할 수 있습니다.

  ```json
  {
    "input": {
      "text" : "Where's your nearest store?"
    }
  ```
  {: codeblock}

- `output`: 사용자에게 대화 응답을 리턴합니다. 

  ```json
  {
  "output": {
    "generic":[
      {
        "values": [
          {
            "text": "This is my response text."
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

결과로 표시되는 API /message 응답에서, 텍스트 응답은 다음과 같이 형식화됩니다.

```json
{
   "text": "This is my response text.",
   "response_type": "text"
}
```

다음과 같은 `output` 오브젝트 형식이 이전 버전과의 호환성을 위해 지원됩니다. 이 형식을 사용하여 텍스트 응답을 지정하는 모든 작업공간이 계속 올바르게 작동합니다. 서식 있는 응답 유형이 도입되었으므로, `output.text` 구조에 `output.generic` 구조가 보강되어 텍스트 외에 기타 유형의 응답을 지원할 수 있습니다. 필요한 경우 응답 유형을 나중에 변경할 수 있으므로 새 노드를 작성할 때 새 형식을 사용하여 더 많은 유연성을 제공하십시오.
{: note}

  ```json
  {
  "output": {
    "text": {
      "values": [
        "This is my response text."
      ]
    }
  }
  ```
  {: codeblock}

정의할 수 있는 텍스트 응답이 아닌 응답 유형이 있습니다. 자세한 내용은 [응답](/docs/services/assistant?topic=assistant-dialog-overview#dialog-overview-responses)을 참조하십시오.

/message API 호출에 관한 자세한 정보는 [API 참조 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://{DomainName}/apidocs/assistant-v2){: new_window}를 참조하십시오.

### 대화 턴(turn) 사이의 정보 유지
{: #dialog-runtime-context}

대화 스킬의 대화는 Stateless이므로 사용자와의 하나의 상호작용에서 다음 상호작용까지 정보를 유지하지 않습니다. 대화 스킬을 어시스턴트에 추가하고 이를 배치하는 경우, 어시스턴트가 하나의 메시지 호출에서 컨텍스트를 저장한 후 현재 세션 동안 다음 요청 시 이를 다시 제출합니다. 현재 세션은 사용자가 어시스턴트와 상호작용하는 동안 지속되고 플러스 또는 프리미엄 플랜의 경우에는 최대 60분(Lite 또는 표준 플랜의 경우 5분)의 비활성 동안 지속됩니다. 대화 스킬을 어시스턴트에 추가하지 않은 경우, 사용자 정의 개발자로서 사용자가 애플리케이션에 필요한 지속적인 정보를 유지해야 할 책임이 있습니다. 애플리케이션은 메시지 API 응답에서 컨텍스트 오브젝트를 찾아서 저장하고 대화 플로우의 일부로 수행된 다음 /message API 요청과 함께 컨텍스트 오브젝트에 전달해야 합니다.

정보를 보존하는 가장 간단한 방법은 예를 들어, 웹 브라우저에서 클라이언트 애플리케이션의 메모리에 전체 컨텍스트 오브젝트를 저장하는 것입니다. 애플리케이션이 점점 더 복잡해짐에 따라, 또는 전달하고 개인 식별 정보를 저장해야 하는 경우, 데이터베이스에서 정보를 저장하고 검색할 수 있습니다. 물론, 가장 간단한 방법은 컨텍스트를 전혀 저장할 수 없도록 만드는 방법입니다. 이 접근 방식을 구현하려면 대화 스킬을 어시스턴트에 추가하고 어시스턴트가 이 컨텍스트를 계속 추적하도록 합니다.

애플리케이션은 대화에 정보를 전달할 수 있고, 대화에서 이 정보를 업데이트하고 애플리케이션 또는 후속 노드에 다시 전달할 수 있습니다. 대화는 *컨텍스트 변수*를 사용하여 이 작업을 수행합니다.

## 컨텍스트 변수
{: #dialog-runtime-context-variables}

컨텍스트 변수는 노드에서 정의하는 변수입니다. 이에 대한 기본값을 선택할 수 있습니다. 기타 노드, 애플리케이션 로직 또는 사용자 입력은 컨텍스트 변수의 값을 나중에 설정하거나 변경할 수 있습니다.

노드 실행 여부를 판별하기 위해 대화 노드 조건에서 컨텍스트 변수를 참조하여 컨텍스트 변수값에 대해 조건을 지정할 수 있습니다. 또한 대화 노드 응답 조건에서 컨텍스트 변수를 참조하여 외부 서비스 또는 사용자가 제공하는 값에 따라 서로 다른 응답을 표시할 수 있습니다.

자세히 보기:

- [애플리케이션에서 컨텍스트 전달](#dialog-runtime-context-from-app)
- [노드 간 컨텍스트 전달](#dialog-runtime-context-node-to-node)
- [컨텍스트 변수 정의](#dialog-runtime-context-var-define)
- [공통 컨텍스트 변수 태스크](#dialog-runtime-context-common-tasks)
- [컨텍스트 변수 삭제](#dialog-runtime-context-delete)
- [컨텍스트 변수 업데이트](#dialog-runtime-context-update)
- [컨텍스트 변수 처리 방식](#dialog-runtime-context-processing)
- [연산 순서](#dialog-runtime-context-order-of-ops)
- [슬롯이 있는 노드에 컨텍스트 변수 추가](#dialog-runtime-context-var-slots)

### 애플리케이션에서 컨텍스트 전달
{: #dialog-runtime-context-from-app}

컨텍스트 변수를 설정하고 대화에 컨텍스트 변수를 전달하여 애플리케이션에서 대화로 정보를 전달하십시오.

예를 들어, 애플리케이션은 $time_of_day 컨텍스트 변수를 설정하고, 정보를 사용하여 사용자에게 표시되는 인사를 사용자 조정할 수 있는 대화에 이 변수를 전달할 수 있습니다.

![애플리케이션에서 대화로 전달되는 $time_of_day 컨텍스트 변수의 값을 검사하기 위해 응답 조건을 사용하는 Welcome 노드 표시](images/set-context.png)

이 예제에서 대화는 애플리케이션이 변수를 다음 값 중 하나로 설정한다고 인식합니다. *morning*, *afternoon* 또는 *evening*. 또한 각 값을 검사하고 존재하는 값에 따라 적절한 인사를 리턴할 수 있습니다. 변수가 전달되지 않거나 예상 값 중 하나와 일치하지 않는 값을 포함하는 경우, 보다 일반적인 인사가 사용자에게 표시됩니다.

### 노드 간 컨텍스트 전달
{: #dialog-runtime-context-node-to-node}

대화는 컨텍스트 변수를 추가하여 하나의 노드에서 다른 노드로 정보를 전달하거나 컨텍스트 변수값을 업데이트할 수 있습니다. 대화가 사용자로부터 정보를 요청하여 가져오는 경우 정보를 추적하고 나중에 대화에서 참조할 수 있습니다.

예를 들어, 하나의 노드에서 사용자에게 이름을 묻고 후속 노드에서 이름별로 주소를 지정할 수 있습니다.

![사용자에게 이름을 묻고 이 이름을 컨텍스트 변수로 저장하는 소개 노드를 표시합니다. 다음 노드는 $username 컨텍스트 변수를 사용하여 이름으로 사용자를 참조합니다.](images/set-context-username.png)

이 예제에서 시스템 엔티티 @sys-person은 입력에서 사용자 이름(사용자가 제공하는 경우)을 추출하는 데 사용됩니다. JSON 편집기에서 username 컨텍스트 변수가 정의되고 @sys-person 값으로 설정됩니다. 후속 노드에서 $username 컨텍스트 변수는 이름별로 사용자 주소를 지정하도록 응답에 포함됩니다.

### 컨텍스트 변수 정의
{: #dialog-runtime-context-var-define}

**변수** 필드에 변수 이름을 추가하고 노드의 편집 보기에 있는 **값** 필드에 변수의 기본값을 추가하여 컨텍스트 변수를 정의합니다.

1.  컨텍스트 변수를 추가할 대화 노드를 클릭하여 엽니다.

1.  노드 응답과 연관된 **옵션**  ![고급 응답](images/kabob.png) 아이콘을 클릭하고 **컨텍스트 편집기 열기**를 클릭합니다.

      ![표준 노드 응답과 관련된 JSON 편집기에 액세스하는 방법 표시.](images/contextvar-json-response.png)

      노드에 대해 **다중 응답** 설정이 **켜져**있으면, 컨텍스트 변수와 연결할 응답에 대해 **응답 편집** ![응답 편집](images/edit-slot.png) 아이콘을 먼저 클릭합니다. 

      ![여러 조건부 응답을 사용하도록 설정된 표준 노드와 연결된 JSON 편집기에 액세스하는 방법 표시.](images/contextvar-json-multi-response.png)

1.  값 이름과 값 쌍을 **변수** 및 **값** 필드에 추가합니다.

    - `name`에는 대소문자 영문자, 숫자 문자(0 - 9) 및 밑줄이 포함될 수 있습니다.

      마침표 및 하이픈과 같은 기타 문자를 이름에 포함할 수 있습니다. 그러나 그런 경우, 변수를 참조할 때마다 단축 구문 `$(variable-name)`을 지정해야 합니다. 자세한 내용은 [오브젝트 액세스에 대한 표현식](/docs/services/assistant?topic=assistant-expression-language#expression-language-shorthand-context)을 참조하십시오.
      {:tip}

    - `value`는 지원되는 JSON 유형일 수 있습니다(예: 단순 문자열 변수, 숫자, JSON 배열 또는 JSON 오브젝트).

다음 표에서는 다양한 값 유형의 이름 및 값 쌍을 정의하는 방법에 대한 몇 가지 예제를 보여줍니다.

| 변수           | 값                            | 값 유형    |
|:---------------|-------------------------------|------------|
| dessert        | "cake"                        | 문자열     |
| age            | 18                            | 숫자       |
| toppings_array | ["onions","olives"]            | JSON 배열 |
| full_name      | {"first":"John","last":"Doe"} | JSON 오브젝트 |

나중에 컨텍스트 변수를 참조하려면 *name*이 정의된 컨텍스트 변수의 이름인 `$name` 구문을 사용하십시오. 

예를 들어, 다음 표현식을 대화 응답으로 지정할 수 있습니다.

`The customer, $age-year-old <? $full_name.first ?>, wants a pizza with <? $toppings_array.join(' and ') ?>, and then $dessert.`

결과 출력은 다음과 같이 표시됩니다.

`The customer, 18-year-old John, wants a pizza with onions and olives, and then cake.`

또한 JSON 편집기를 사용하여 컨텍스트 변수를 정의할 수 있습니다. 복합 표현식을 변수 값으로 추가하려는 경우 JSON 편집기를 사용하는 것이 좋습니다. 자세한 내용은 [JSON 편집기의 컨텍스트 변수](#dialog-runtime-context-var-json)를 참조하십시오.

### 공통 컨텍스트 변수 태스크
{: #dialog-runtime-context-common-tasks}

사용자가 입력한 전체 문자열을 저장하려면 `input.text`를 사용하십시오.

| 변수     | 값               |
|----------|------------------|
| repeat   | `<?input.text?>` |

예를 들어, 사용자 입력은 `I want to order a device pizza`입니다. 노드 응답이 `You said: $repeat`인 경우, 응답은 `You said: I want to order a device`로 표시됩니다.

컨텍스트 변수에 엔티티 값을 저장하려면 다음 구문을 사용하십시오.

| 변수     | 값               |
|----------|------------------|
| place    | `@place`         |

예를 들어, 사용자 입력은 `I want to go to Paris`입니다. @place 엔티티가 `Paris`를 인식하는 경우, 서비스에서 `Paris`를 `$place` 컨텍스트 변수에 저장합니다.

사용자가 사용자 입력에서 추출할 문자열의 값을 저장하려면 정규식을 사용자 입력에 적용하는 데 `extract` 메소드를 사용하는 SpEL 표현식을 포함할 수 있습니다. 다음 표현식은 사용자 입력에서 숫자를 추출하고 `$number` 컨텍스트 변수에 저장합니다.

| 변수     | 값                                  |
|----------|-------------------------------------|
| number   | `<?input.text.extract('[\d]+',0)?>` |

패턴 엔티티의 값을 저장하려면 엔티티 이름에 .literal을 추가하십시오. 이 구문을 사용하면 지정된 패턴과 일치하는 사용자 입력의 정확한 범위의 텍스트가 변수에 저장됩니다.

| 변수     | 값                     |
|----------|------------------------|
| email    | `<? @email.literal ?>` |

예를 들어, 사용자 입력은 `Contact me at joe@example.com`입니다. `@email`이라는 엔티티가 `name@domain.com` 이메일 형식을 인식합니다. `@email.literal`을 저장하도록 컨텍스트 변수를 구성하여, 패턴과 일치하는 입력 부분을 저장할 것임을 표시합니다. 값 표현식에서 `.literal` 특성을 생략하는 경우, 패턴과 일치하는 사용자 입력의 세그먼트 대신 패턴에 지정한 엔티티 값 이름이 리턴됩니다.

이 값 예제의 대부분은 사용자 입력의 다른 파트를 캡처하는 데 메소드를 사용합니다. 사용 가능한 메소드에 대한 자세한 정보는 [표현식 언어 메소드](/docs/services/assistant?topic=assistant-dialog-methods)를 참조하십시오.    

### 컨텍스트 변수 삭제
{: #dialog-runtime-context-delete}

컨텍스트 변수를 삭제하려면, 변수를 널로 설정하십시오.

| 변수       | 값               |
|------------|------------------|
| order_form |`null`         |

또는, 애플리케이션 로직에서 컨텍스트 변수를 삭제할 수 있습니다. 변수를 완전히 제거하는 방법에 대한 정보는 [JSON에서 컨텍스트 변수 삭제](#dialog-runtime-context-delete-json)를 참조하십시오.

### 컨텍스트 변수값 업데이트
{: #dialog-runtime-context-update}

컨텍스트 변수의 값을 업데이트하려면 이전 컨텍스트 변수와 동일한 이름의 컨텍스트 변수를 정의하되, 이번에는 다른 값을 지정하십시오.

둘 이상의 노드가 동일한 컨텍스트 변수의 값을 설정하는 경우, 컨텍스트 변수의 값이 사용자와의 대화 과정에서 변경될 수 있습니다. 지정된 시간에 적용되는 값은 대화 중에 사용자가 트리거하는 노드 종류에 따라 다릅니다. 처리된 마지막 노드의 컨텍스트 변수에 지정된 값이 이전에 처리된 노드가 변수에 설정한 값을 겹쳐씁니다.

값이 JSON 오브젝트 또는 JSON 배열 데이터 유형인 경우 컨텍스트 변수의 값을 업데이트하는 방법에 대한 정보는 [JSON의 컨텍스트 변수 값 업데이트](#dialog-runtime-context-update-json)를 참조하십시오.

### 컨텍스트 변수가 처리되는 방법
{: #dialog-runtime-context-processing}

컨텍스트 변수 문제를 정의하는 위치입니다. 서비스가 컨텍스트 변수를 정의한 대화 노드의 일부를 처리할 때까지 컨텍스트 변수가 작성되지 않고 사용자가 지정하는 값으로 설정됩니다. 대부분의 경우, 노드 응답의 일부로 컨텍스트 변수를 정의합니다. 이를 수행하면 서비스가 노드 응답을 리턴할 때 컨텍스트 변수가 작성되고 변수에 지정된 값이 제공됩니다.

조건부 응답이 있는 노드의 경우, 특정 응답에 대한 조건이 충족되고 응답이 처리될 때 컨텍스트 변수가 작성되고 설정됩니다. 예를 들어, 조건부 응답 #1에 대한 컨텍스트 변수를 정의하고 서비스가 조건부 응답 #2만을 처리하는 경우, 조건부 응답 #1에 대해 정의한 컨텍스트 변수가 작성되지 않고 설정되지도 않습니다.

사용자가 슬롯이 있는 노드와 상호작용할 때 서비스에서 작성하고 설정하려는 컨텍스트 변수를 추가할 위치에 대한 정보는 [슬롯이 있는 노드에 컨텍스트 변수 추가](#dialog-runtime-context-var-slots)를 참조하십시오.

### 연산 순서
{: #dialog-runtime-context-order-of-ops}

함께 처리할 변수를 여러 개 정의하는 경우, 사용자가 정의하는 순서가 서비스에서 평가하는 순서를 결정하지 않습니다. 서비스는 변수를 임의의 순서로 평가합니다. 첫 번째 컨텍스트 변수가 두 번째 변수 전에 처리된다는 보장이 없으므로 목록의 첫 번째 컨텍스트 변수에 값을 설정하고 목록의 두 번째 변수에 이를 사용할 수 있다고 예상하지 마십시오. 예를 들어, 사용자 입력에 단어 `Yes`가 있는지 확인하는 로직을 구현하는 데 두 개의 컨텍스트 변수를 사용하지 마십시오.

| 변수            | 값               |
|-----------------|------------------|
| user_input      | <? input.text ?> |
| contains_yes    | <? $user_input.contains('Yes') ?> |

대신, 약간 더 복잡한 표현식을 사용하면 두 번째 변수(contains_yes)가 평가되기 전에 평가되는 목록의 첫 번째 변수(user_input) 값에 의존하지 않도록 할 수 있습니다. 

| 변수          | 값               |
|---------------|------------------|
| contains_yes  | <? input.text.contains('Yes') ?> |

### 슬롯이 있는 노드에 컨텍스트 변수 추가
{: #dialog-runtime-context-var-slots}

슬롯에 대한 자세한 정보는 [슬롯이 있는 정보 수집](/docs/services/assistant?topic=assistant-dialog-slots)을 참조하십시오.

1.  편집 보기에서 슬롯이 있는 노드를 여십시오.

    - 슬롯의 응답 조건이 충족된 후에 처리되는 컨텍스트 변수를 추가하려면 다음 단계를 수행하십시오.

      1.  **슬롯 편집** ![응답 편집](images/edit-slot.png) 아이콘을 클릭하십시오. 
      1.  **옵션** ![고급 응답](images/kabob.png) 아이콘을 클릭한 후 **조건부 응답 사용**을 선택하십시오.
      1.  컨텍스트 변수를 연관시키려는 응답 옆에 있는 **응답 편집** ![응답 편집](images/edit-slot.png) 아이콘을 클릭하십시오.
      1.  응답 섹션에서 **옵션** ![고급 응답](images/kabob.png) 아이콘을 클릭하고 **컨텍스트 편집기 열기**를 클릭하십시오.
      1.  값 이름과 값 쌍을 **변수** 및 **값** 필드에 추가하십시오.

      ![슬롯에 대한 조건부 응답과 관련된 JSON 편집기에 액세스하는 방법 표시.](images/contextvar-json-slot-multi-response.png)

    - 슬롯 조건이 충족된 후 설정되거나 업데이트된 컨텍스트 변수를 추가하려면 다음 단계를 완료하십시오.

      1.  **슬롯 편집** ![응답 편집](images/edit-slot.png) 아이콘을 클릭하십시오. 
      1.  *슬롯 구성* 보기 머리글에 있는 **옵션** ![고급 응답](images/kabob.png) 메뉴에서, **JSON 편집기 열기**를 클릭하십시오.
      1.  변수 이름과 값 쌍을 JSON 형식으로 추가하십시오.

          ```json
          {
            "time_of_day": "morning"
          }
          ```
          {: codeblock}

      컨텍스트 편집기를 사용하여 대화 노드 평가의 이 단계에서 설정된 컨텍스트 변수를 정의할 수 있는 방법이 현재는 없습니다. 대신 JSON 편집기를 사용해야 합니다. JSON 편집기 사용에 대한 자세한 정보는 [JSON 편집기의 컨텍스트 변수](#dialog-runtime-context-var-json)를 참조하십시오.
      {: note}

      ![슬롯 조건과 관련된 JSON 편집기에 액세스하는 방법 표시.](images/contextvar-json-slot-condition.png)

## JSON 편집기의 컨텍스트 변수
{: #dialog-runtime-context-var-json}

또한 JSON 편집기에서 컨텍스트 변수를 정의할 수 있습니다. 복합 컨텍스트 변수를 정의하고 이를 추가하거나 변경할 때 전체 SpEL 표현식을 표시하려면 JSON 편집기를 사용해야 합니다.

이름 및 값 쌍은 다음 요구사항을 충족해야 합니다.

- `name`에는 대소문자 영문자, 숫자 문자(0 - 9) 및 밑줄이 포함될 수 있습니다.

  마침표 및 하이픈과 같은 기타 문자를 이름에 포함할 수 있습니다. 그러나 그런 경우, 변수를 참조할 때마다 단축 구문 `$(variable-name)`을 지정해야 합니다. 자세한 내용은 [오브젝트 액세스에 대한 표현식](/docs/services/assistant?topic=assistant-expression-language#expression-lanaguage-shorthand-context)을 참조하십시오.
  {:tip}

- `value`는 지원되는 JSON 유형일 수 있습니다(예: 단순 문자열 변수, 숫자, JSON 배열 또는 JSON 오브젝트).

다음 JSON 샘플은 $dessert 문자열, $toppings_array 배열, $age 숫자 및 $full_name 오브젝트 컨텍스트 변수의 값을 정의합니다.

```json
{
  "context": {
    "dessert": "cake",
    "toppings_array": [
      "onions",
      "olives"
    ],
    "age": 18,
    "full_name": {
      "first": "Jane",
      "last": "Doe"
    }
  },
  "output":{}
}
```
{: codeblock}

컨텍스트 변수를 JSON 형식으로 정의하려면 다음 단계를 완료하십시오.

1.  컨텍스트 변수를 추가할 대화 노드를 클릭하여 엽니다.

    이 노드에 정의되어 있는 기존 컨텍스트 변수 값이 해당 **변수** 및 **값** 필드 세트에 표시됩니다. 사용자가 이 노드의 편집 보기에서 표시되지 않게 하려면, 컨텍스트 편집기를 닫아야 합니다. JSON 편집기를 열 때 사용했던 동일한 메뉴에서 편집기를 닫을 수 있습니다. 다음 단계에서는 메뉴에 액세스하는 방법을 설명합니다.
    {: note}

1.  응답과 연관된 **옵션** ![고급 응답](images/kabob.png) 아이콘을 클릭하고 **JSON 편집기 열기**를 클릭합니다.

    ![표준 노드 응답과 관련된 JSON 편집기에 액세스하는 방법 표시.](images/contextvar-json-response.png)

    노드에 대해 **다중 응답** 설정이 **켜져**있으면, 컨텍스트 변수와 연결할 응답에 대해 **응답 편집** ![응답 편집](images/edit-slot.png) 아이콘을 먼저 클릭합니다. 

    ![여러 조건부 응답을 사용하도록 설정된 표준 노드와 연결된 JSON 편집기에 액세스하는 방법 표시.](images/contextvar-json-multi-response.png)

1.  없는 경우, `"context":{}` 블록을 추가합니다.

    ```json
    {
      "context":{},
      "output":{}
    }
    ```
    {: codeblock}

1.  컨텍스트 블록에서, 정의할 각 컨텍스트 변수의 `"name"` 및 `"value"` 쌍을 추가합니다.

    ```json
    {
      "context":{
        "name": "value"
    },
      "output": {}
    }
    ```
    {: codeblock}

    이 예에서, `new_variable` 이름의 변수가 이미 변수가 있는 컨텍스트 블록에 추가되었습니다.

    ```json
    {
      "context":{
        "existing_variable": "value",
        "new_variable":"value"
      }
    }
    ```
    {: codeblock}

    나중에 컨텍스트 변수를 참조하려면 *name*이 정의된 컨텍스트 변수의 이름인 `$name` 구문을 사용하십시오. 예: `$new_variable`.

자세히 보기:

- [JSON에서 컨텍스트 변수 삭제](#dialog-runtime-context-delete-json)
- [JSON에서 컨텍스트 변수 값 업데이트](#dialog-runtime-context-update-json)
- [한 컨텍스트 변수를 다른 변수와 동일하게 설정](#dialog-runtime-var-equals-var)

### JSON에서 컨텍스트 변수 삭제
{: #dialog-runtime-context-delete-json}

컨텍스트 변수를 삭제하려면, 변수를 널로 설정하십시오.

```json
{
  "context": {
    "order_form": null
  }
}
```
{: codeblock}

컨텍스트 변수의 모든 추적을 제거하려는 경우, JSONObject.remove(string) 메소드를 사용하여 컨텍스트 오브젝트에서 삭제할 수 있습니다. 그러나 제거를 수행하려면 변수를 사용해야 합니다. 현재 호출 이후엔 저장되지 않으므로 메시지 출력에 새 변수를 정의하십시오.

```json
{
  "output": {
    "text" : {},
    "deleted_variable" : "<? context.remove('order_form') ?>"
  }
}
```
{: codeblock}

또는, 애플리케이션 로직에서 컨텍스트 변수를 삭제할 수 있습니다.

### JSON에서 컨텍스트 변수 값 업데이트
{: #dialog-runtime-context-update-json}

일반적으로, 노드가 이미 설정된 컨텍스트 변수의 값을 설정하면 새 값이 이전 값을 겹쳐씁니다.

#### 복합 JSON 오브젝트 업데이트

JSON 오브젝트를 제외한 모든 JSON 유형에 대해 이전 값을 겹쳐씁니다. 컨텍스트 변수가 JSON 오브젝트와 같은 복합 유형인 경우 JSON 병합 프로시저가 변수 업데이트에 사용됩니다. 병합 오퍼레이션은 새로 정의된 특성을 추가하고 오브젝트의 기존 특성을 겹쳐씁니다.

이 예제에서는 이름 컨텍스트 변수가 복합 오브젝트로 정의됩니다.

```json
{
  "context": {
    "complex_object": {
      "user_firstname" : "Paul",
      "user_lastname" : "Pan",
      "has_card" : false
    }
  }
}
```
{: codeblock}

대화 노드는 다음 값으로 컨텍스트 변수 JSON 오브젝트를 업데이트합니다.

```json
{
  "complex_object": {
    "user_firstname": "Peter",
    "has_card": true
  }
}
```
{: codeblock}

결과는 다음 컨텍스트입니다.

```json
{
  "complex_object": {
    "user_firstname": "Peter",
    "user_lastname": "Pan",
    "has_card": true
  }
}
```
{: codeblock}

오브젝트에서 수행할 수 있는 메소드에 관한 자세한 정보는 [표현식 언어 메소드](/docs/services/assistant?topic=assistant-dialog-methods#dialog-methods-objects)를 참조하십시오.

#### 배열 업데이트

대화 컨텍스트 데이터에 값 배열이 있는 경우, 값을 추가하거나 값을 제거하거나 값을 모두 바꿔 배열을 업데이트할 수 있습니다.

이러한 조치 중 하나를 선택하여 배열을 업데이트하십시오. 각각의 경우에 조치가 적용되기 전의 배열, 조치, 그리고 조치가 적용된 후의 배열이 표시됩니다.

- **추가**: 배열의 끝에 값을 추가하려면 `append` 메소드를 사용하십시오.

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

- **제거**: 요소를 제거하려면 `remove` 메소드를 사용하고 배열에 해당 값 또는 위치를 지정하십시오.

    - **값을 기준으로 제거**: 값을 기준으로 배열에서 요소를 제거합니다.

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

    - **위치를 기준으로 제거**: 인덱스 위치를 기준으로 배열에서 요소를 제거합니다.

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

- **겹쳐쓰기**: 배열에서 값을 겹쳐쓰려면 배열을 새 값으로 설정하십시오.

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
            "toppings_array": ["ketchup", "tomatoes"]
          }
        }
        ```
        {: codeblock}

    결과:

        ```json
        {
          "context": {
            "toppings_array": ["ketchup", "tomatoes"]
          }
        }
        ```
        {: codeblock}

배열에서 수행할 수 있는 메소드에 관한 자세한 정보는 [표현식 언어 메소드](/docs/services/assistant?topic=assistant-dialog-methods#dialog-methods-arrays)를 참조하십시오.

### 한 컨텍스트 변수를 다른 변수와 동일하게 설정
{: #dialog-runtime-var-equals-var}

하나의 컨텍스트 변수를 다른 컨텍스트 변수와 동일하게 설정하는 경우 한 변수에서 다른 변수까지 포인터를 정의합니다. 변수 중 하나의 값이 이후에 변경되면 다른 변수의 값도 변경됩니다.

예를 들어, 컨텍스트 변수를 다음과 같이 지정하는 경우, `$var1` 또는 `$var2` 중의 값이 나중에 변경되면 다른 변수의 값도 변경됩니다.

| 변수      | 값     |
|-----------|--------|
| var2      | var1   |

특정 시점 값을 캡처하려면 한 변수를 다른 변수와 동일하게 설정하지 마십시오. 예를 들어, 배열을 처리할 때 나중에 사용하기 위해 대화에서 특정 지점에 있는 컨텍스트 변수에 저장된 배열 값을 캡처하려는 경우 대신에 현재 변수 값을 기반으로 새 변수를 작성할 수 있습니다.

예를 들어, 대화 플로우의 특정 지점에서 배열 값의 사본을 작성하려면 기존 배열의 값으로 채워진 새 배열을 추가하십시오. 이를 수행하려면 다음 단계를 수행하십시오. 

```json
{
"context": {
   "var2": "<? output.var2?:new JsonArray().append($var1) ?>"
 }
 }
 ```
{: codeblock}

## 다이그레션(digression)
{: #dialog-runtime-digressions}

다이그레션(digression)은 사용자가 한 목표를 처리하도록 설계된 대화 플로우의 중간에 있는 경우 발생하며 갑자기 다른 목표를 충족하도록 설계된 대화 플로우를 시작하는 주제로 전환합니다. 대화는 항상 사용자가 주제를 변경하는 기능을 지원합니다. 처리중인 대화 분기의 어떤 노드도 사용자의 최신 입력 목표와 일치하지 않으면 대화는 다시 트리로 이동하여 루트 노드 조건에서 적절한 일치 항목을 확인합니다. 노드 당 사용 가능한 다이그레션(digression) 설정에서 이 동작을 훨씬 더 맞춤 설정하는 기능을 제공합니다.

다이그레션(digression) 설정을 사용하면, 다이그레션이 발생했을 때 중단된 대화 플로우로 대화를 되돌릴 수 있습니다. 예를 들어, 사용자가 새 전화를 주문하고 있지만 주제를 전환하여 태블릿에 대해 질문합니다. 대화는 태블릿에 대한 질문에 답변한 다음 전화를 주문하는 과정에서 중단한 위치로 사용자를 되돌릴 수 있습니다. 다이그레션(digression)의 발생 및 돌아가기를 허용하면 사용자는 실행 중에 대화 플로우를 보다 잘 제어할 수 있습니다. 다이그레션(digression)은 주제를 변경할 수 있고, 관련이 없는 주제에 대한 대화 플로우를 끝까지 따라갈 수 있으며, 그런 다음 전에 있었던 곳으로 돌아갈 수 있습니다. 결과는 사용자 간의 대화를 보다 면밀하게 시뮬레이션하는 대화 플로우입니다.

![저녁 식사 예약에 대한 세부 정보를 제공하는 사람이 채식 옵션에 대해 질문하고 답변을 얻은 다음 예약 세부 정보를 제공하는 것으로 돌아가는 것을 표시합니다.](images/digression.gif){: gif}

애니메이션 이미지는 대화 트리 사용자 인터페이스의 모형을 사용하여 다이그레션(digression)의 개념을 설명합니다. 이것은 진행 중인 대화 플로우로 돌아가는 다이그레션(digression)을 허용하도록 구성된 대화 노드와 상호 작용하는 방법을 보여줍니다. 사용자가 저녁 식사 예약에 필요한 정보를 제공하기 시작합니다. #reservation 노드의 슬롯을 채우는 중에 사용자는 채식 메뉴 옵션에 대해 질문합니다. 대화는 루트 노드(#cuisine 인텐트를 조건으로 하는 노드)중에서 노드를 찾아 사용자의 새 질문에 응답합니다. 그런 다음 원래 대화 노드에서 다음 빈 슬롯에 대한 프롬프트를 표시하여 진행 중인 대화로 돌아갑니다.

자세한 내용을 보려면 동영상을 시청하십시오.

<iframe class="embed-responsive-item" id="youtubeplayer" title="다이그레션(digression) 개요" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/I3K7mQ46K3o?rel=0" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

- [시작하기 전에](#dialog-runtime-digression-prereqs)
- [다이그레션(digression) 사용자 정의](#dialog-runtime-enable-digressions)
- [다이그레션(digression) 사용 팁](#dialog-runtime-digress-tips)
- [루트 노드에 다이그레션(digression) 사용 안함](#dialog-runtime-disable-digressions)
- [다이그레션(digression) 튜토리얼](#dialog-runtime-digression-tutorial)
- [디자인 고려사항](#dialog-runtime-digression-design-considerations)

### 시작하기 전에
{: #dialog-runtime-digression-prereqs}

전체 대화를 테스트할 때 다이그레션(digression)을 허용하고 발생해서 돌아오는 시기와 위치를 결정하십시오. 다음 다이그레션(digression) 제어는 노드에 자동으로 적용됩니다. 사용자가 이 기본 동작을 변경하려는 경우에만 조치를 수행하십시오.

- 대화의 모든 루트 노드는 기본적으로 대상을 다이그레션(digression)할 수 있도록 구성됩니다. 하위 노드는 다이그레션(digression)의 대상이 될 수 없습니다.
- 슬롯이 있는 노드는 다이그레션(digression)을 방지하도록 구성됩니다. 기타 모든 노드는 다이그레션(digression)을 허용하도록 구성됩니다. 그러나 다음과 같은 상황에서는 대화가 노드로부터 다이그레션(digression)할 수 없습니다.

  - 현재 노드의 하위 노드 중 하나가 `anything_else` 또는 `true` 조건을 포함하는 경우

    이러한 조건은 항상 true로 평가된다는 점에서 특별합니다. 알려진 동작으로 인해, 대화에서 종종 상위 노드가 특정 하위 노드를 연속적으로 평가하도록 강제합니다. 기존의 대화 플로우 로직을 위반하지 않기 위해, 이런 경우 다이그레션(digression)은 허용되지 않습니다. 그러한 노드로부터 다이그레션(digression)을 사용으로 설정하기 전에 하위 노드의 조건을 다른 것으로 변경해야 합니다.

  - 노드가 다른 노드로 점프하거나 처리 된 후 사용자 입력을 생략하도록 구성된 경우

    노드의 마지막 단계 섹션은 노드가 처리된 후 발생할 일을 지정합니다. 대화가 다른 노드로 바로 이동하도록 구성될 때, 종종 특정 순서를 따르는지 확인하십시오. 그리고 노드가 사용자 입력을 건너뛰도록 구성된 경우, 이는 대화가 현재 노드 다음의 첫 번째 하위 노드를 연속적으로 처리하도록 하는 것과 동일합니다. 기존 대화 플로우 로직을 위반하지 않도록 하기 위해 이런 경우 다이그레션(digression)은 허용되지 않습니다. 이 노드로부터의 다이그레션(digression)을 사용으로 설정하기 전에 최종 단계 섹션에서 지정된 내용을 변경해야 합니다.

### 다이그레션(digression) 사용자 정의
{: #dialog-runtime-enable-digressions}

귀하는 다이그레션(digression)의 시작과 끝을 정의하지 않습니다. 사용자가 실행 중에 다이그레션(digression) 플로우를 완전히 제어합니다. 귀하는 각 노드가 사용자가 주도하는 다이그레션(digression)에 참여할지 여부만 지정합니다. 각 노드에 대해 다음을 구성합니다.

- 다이그레션(digression)은 노드에서 시작하거나 떠날 수 있음
- 다른 곳에서 시작하는 다이그레션(digression)은 대상이 될 수 있으며 노드에 들어갈 수 있음
- 다른 곳에서 시작하여 노드에 들어간 다이그레션(digression)은 현재 대화 플로우가 완료된 후 중단된 대화 플로우로 돌아와야 함

개별 노드의 다이그레션(digression) 동작을 변경하려면, 다음 단계를 완료하십시오.

1.  노드를 클릭하여 편집 보기를 여십시오.

1.  **사용자 정의**를 클릭하고, **다이그레션(digression)** 탭을 클릭하십시오.

    구성 옵션은 편집 중인 노드가 루트 노드, 하위 노드, 하위 노드 또는 슬롯이 있는 노드인지 여부에 따라 다릅니다.

    **이 노드로부터의 다이그레션(digression)**

    이전에 나열된 상황이 적용되지 않으면 다음과 같은 선택을 할 수 있습니다.

    - **모든 노드 유형**: 사용자가 현재 대화 분기의 끝에 도달하기 전에 현재 노드로부터 다이그레션(digression)할 수 있도록 허용할지 여부를 선택합니다.

    - **하위 노드가 있는 모든 노드**: 현재 노드의 응답이 이미 표시되고 하위 노드가 노드의 목표에 부수적인 경우 대화가 다이그레션(digression) 후 현재 노드로 돌아갈지 여부를 선택합니다. 대화가 현재 노드로 돌아가는 것을 방지하고 분기 처리를 계속하려면, *이 노드의 응답 후에 실행된 다이그레션(digression)에서 돌아오기 허용*을 **No**로 설정하십시오.

      예를 들어, 사용자가 `Do you sell cupcakes?`라고 문의하고 사용자가 주제를 변경하기 전에 `We offer cupcakes in a variety of flavors and sizes`라는 응답이 표시되면, 대화가 중단된 위치로 돌아가는 것을 원하지 않을 수도 있습니다. 특히 하위 노드가 사용자로부터 가능한 후속 질문만 처리하고 안전하게 무시할 수 있습니다.

      그러나, 노드가 하위 노드에 의존하여 질문을 처리하는 경우, 강제로 대화로 돌아가고 현재 분기의 노드 처리를 계속하기를 원할 수도 있습니다. 예를 들어, 초기 응답은 `We offer cupcakes in all shapes and sizes. Which menu do you want to see: gluten-free, dairy-free, or regular?`일 수 있습니다. 사용자가 이 시점에서 주제를 변경하면, 사용자가 메뉴 유형을 선택하고 원하는 정보를 얻을 수 있도록 대화로 돌아가기를 원할 수 있습니다.

    - **슬롯이 있는 노드**: 모든 슬롯이 채워지기 전에 사용자가 노드로부터 다이그레션(digression)할 수 있도록 허용할지 여부를 선택하십시오. *슬롯을 채우는 중에 다이그레션(digression) 허용*을 **Yes**로 설정하여 다이그레션(digression)을 사용으로 설정하십시오.

      사용으로 설정된 경우, 대화가 다이그레션(digression)에서 돌아오면 사용자가 계속 정보를 제공할 수 있도록 채워지지 않은 다음 슬롯에 대한 프롬프트가 표시됩니다. 사용 안함으로 설정된 경우, 슬롯을 채울 수 있는 값을 포함하지 않는 사용자 입력 내용은 무시됩니다. 그러나 슬롯 핸들러를 정의하여 사용자가 노드와 상호작용하는 동안 사용자가 요청할 수 있는 원치 않는 질문을 처리 할 수 있습니다. 자세한 정보는 [슬롯 추가](/docs/services/assistant?topic=assistant-dialog-slots#dialog-slots-add)를 참조하십시오.

      다음 이미지는 슬롯이 있는 #reservation 노드(앞의 그림 참조)로부터의 다이그레션(digression)이 구성되는 방법을 보여줍니다.

      ![슬롯이 있는 노드로부터의 다이그레션(digression) 설정 표시.](images/digress-away-slots-full.png)

    - **슬롯이 있는 노드**: **돌아가기를 허용하는 노드로만 슬롯으로부터의 다이그레션(digression) 허용** 선택란을 선택하여 현재 노드로 돌아가는 경우에만 사용자가 다이그레션(digression)하도록 허용되는지 여부를 선택합니다.

      선택한 경우, 대화에서 노드와 관련없는 질문에 답하는 노드를 찾으면 다이그레션(digression) 후 돌아가도록 구성되지 않은 루트 노드는 무시됩니다. 사용자가 필수 슬롯을 채우기 전에 노드를 영구적으로 떠날 수 없도록 하려면 이 선택란을 선택하십시오.

    **이 노드에 다이그레션(digression)**

    노드에 다이그레션(digression)이 작동하는 방법에 대해 다음과 같은 선택을 할 수 있습니다.

    - 사용자가 노드에 들어갈 수 없도록 합니다. 자세한 정보는 [루트 노드에 다이그레션(digression) 사용 안함](#dialog-runtime-disable-digressions)을 참조하십시오.

    - 노드로의 다이그레션(digression)이 사용 가능한 경우, 대화가 멀리 떨어져 있는 대화 플로우로 되돌아가야 하는지 여부를 선택합니다. 선택한 경우, 현재 노드의 분기 처리가 완료된 후, 대화 플로우는 중단된 노드로 다시 이동합니다. 나중에 대화로 돌아가려면, **다이그레션(digression) 후 돌아가기**를 선택하십시오.

    다음 이미지는 #cuisine 노드(앞의 그림 참조)로의 다이그레션(digression)이 구성되는 방법을 보여줍니다.

    ![슬롯이 있는 노드로부터의 다이그레션(digression) 설정 표시.](images/digress-into-cuisine-full.png)

1.  **적용**을 클릭하십시오.

1.  "시험 사용" 분할창을 사용하여 다이그레션(digression) 동작을 테스트하십시오.

    다시 말씀드리지만, 귀하는 다이그레션(digression)의 시작과 끝을 정의할 수 없습니다. 사용자가 다이그레션(digression)이 발생하는 시기와 위치를 제어합니다. 단일 노드가 하나의 노드에 참여하는 방식을 결정하는 설정만 적용할 수 있습니다. 다이그레션(digression)은 예측할 수 없기 때문에 구성 결정이 전반적인 대화에 어떻게 영향을 미치는지 알기 어렵습니다. 사용자가 작성한 선택의 효과를 확인하려면 대화를 테스트해야 합니다.

#reservation 및 #cuisine 노드는 단일 사용자 지정 다이그레션(digression)에 참여할 수 있는 두 개의 대화 분기를 나타냅니다. 각각의 개별 노드에 구성되는 다이그레션(digression) 설정은 실행 중에 이런 유형의 다이그레션(digression)을 가능하게 하는 요소입니다.

![두 개의 대화(reservation 슬롯 노드로부터 다이그레션(digression)을 설정하는 것과 cuisine 노드로 다이그레션(digression)을 설정하는 것)를 보여줍니다.](images/digression-settings.png)

### 다이그레션(digression) 사용 팁
{: #dialog-runtime-digress-tips}

이 절에서는 다이그레션(digression) 사용 시 발생할 수 있는 상황에 대한 솔루션에 대해 설명합니다.

- **사용자 정의 돌아가기 메시지**: 다이그레션(digression)에서의 돌아가기를 사용으로 설정하는 모든 노드에 대해, 이전 대화 플로우에서 종료한 위치로 돌아가는 것을 사용자에게 알려주는 표현을 추가하는 것이 좋습니다. 텍스트 응답에서 두 가지 응답 버전을 추가할 수 있는 특수 구문을 사용하십시오.

  조치를 수행하지 않는 경우, 다이그레션(digression)한 노드로 돌아갔음을 알리기 위해 동일한 텍스트 응답이 다시 표시됩니다. 돌아갈 때 표시할 고유한 메시지를 지정하여 원래 대화 스레드로 돌아갔음을 사용자에게 더 명확하게 알려줄 수 있습니다.

  예를 들어, 해당 노드의 원래 텍스트 응답이 `What's the order number?`인 경우, 노드로 돌아갈 때는 `Now let's get back to where we left off. What is the order number?`와 같은 메시지가 표시되도록 합니다.

  이를 수행하려면 다음 구문을 사용하여 노드 텍스트 응답을 지정하십시오.

  `<? (returning_from_digression)? "post-digression message" : "first-time message" ?>`

  예:

  ```bash
  <? (returning_from_digression)? "Now, let's get back to where we left off.
  What is the order number?" : "What's the order number?" ?>
  ```
  {: codeblock}

  추가하는 텍스트 응답에 SpEL 표현식이나 단축 구문을 포함할 수 없습니다. 사실은, 단축 구문을 전혀 사용할 수 없습니다. 대신, 텍스트 문자열과 전체 스핀 표현식 구문을 함께 연결해 전체 응답을 구성하여 메시지를 빌드해야 합니다.
  {: note}
  
  예를 들어, 다음 구문을 사용하여 일반적으로 `What can I do for you, $username?`으로 지정하는 텍스트 응답에 컨텍스트 변수를 포함하십시오. 

  ```bash
  <? (returning_from_digression)? "Where were we, " +
  context["username"] + "? Oh right, I was asking what can I do
  for you today." : "What can I do for you today, " +
  context["username"] + "?" ?>
  ```

  전체 SpEL 표현식 구문에 대한 자세한 내용은 [오브젝트 액세스를 위한 표현식](/docs/services/assistant?topic=assistant-expression-language#expression-language-shorthand-syntax)을 참조하십시오.

- **돌아가기 방지**: 일부 경우에는 현재 대화 플로우에서 작성하는 선택사항에 따라 중단된 대화 플로우로 돌아가지 않도록 할 수 있습니다. 특수 구문을 사용하여 특정 노드에서 돌아가지 않도록 할 수 있습니다.

  예를 들어, `#General_Connect_To_Agent` 또는 유사한 인텐트를 조건으로 하는 노드가 있을 수 있습니다. 트리거될 때, 외부 서비스로 전송하기 전에 사용자의 확인을 받으려는 경우, `Do you want me to transfer you to an agent now?`와 같은 응답을 추가할 수 있습니다. 그런 다음, `#yes` 및 `#no`를 각각 조건으로 하는 두 개의 하위 노드를 추가할 수 있습니다.
  
  이 유형의 분기에 대한 다이그레션(digression)을 관리하는 가장 좋은 방법은 루트 노드가 다이그레션(digression)  돌아가기를 허용하도록 설정하는 것입니다. 그러나 `#yes` 노드에서, SpEL 표현식` <? clearDialogStack() ?>`을 응답에 포함합니다. 예:
  
    ```bash
  OK. I will transfer you now. <? clearDialogStack() ?>
  ```
  {: codeblock}

  이 SpEL 표현식은 이 노드로부터의 다이그레션(digression)  돌아가기가 발생하지 않도록 합니다. 확인이 요청될 때 사용자가 허용하면 적절한 응답이 표시되고 중단된 대화 플로우가 재개되지 않습니다. 사용자가 허용하지 않는 경우, 사용자는 중단된 플로우로 돌아갑니다.

### 루트 노드로부터의 다이그레션(digression)  사용 안 함
{: #dialog-runtime-disable-digressions}

플로우가 루트 노드로 다이그레션하는 경우, 해당 노드에 대해 구성된 대화 과정을 따릅니다. 따라서 노드 분기의 끝에 도달하기 전에 일련의 하위 노드를 처리한 다음 중단하도록 구성된 경우 중단된 대화 플로우로 돌아갑니다. 대화 테스트를 통해 루트 노드가 너무 자주 또는 예기치 않은 시간에 트리거되거나 대화가 복잡하고 사용자가 너무 멀리 떨어져 임시 다이그레션(digression)의 좋은 후보가 될 수 있음을 알 수 있습니다. 사용자가 다이그레션(digression)하는 것을 허용하지 않도록 결정한 경우 루트 노드가 다이그레션(digression)을 허용하지 않도록 구성할 수 있습니다.

루트 노드 전체에 다이그레션(digression)을 사용 안함으로 설정하려면, 다음 단계를 수행하십시오.

1.  편집하려는 루트 노드를 클릭하여 여십시오.
1.  **사용자 정의**를 클릭한 다음 **다이그레션(digression)** 탭을 클릭하십시오.
1.  *이 노드에 다이그레션(digression) 허용* 토글을 **Off**로 설정하십시오.
1.  **적용**을 클릭하십시오.

여러 루트 노드로 다이그레션(digression)을 방지하고 각 노드를 개별적으로 편집하지 않으려면 노드를 폴더에 추가 할 수 있습니다. 폴더의 *사용자 정의* 페이지에서 *이 노드에 다이그레션(digression) 허용*을 **Off**로 설정하여 모든 노드에 한 번에 구성을 적용할 수 있습니다. 자세한 정보를 보려면 [폴더가 있는 대화 구성](/docs/services/assistant?topic=assistant-dialog-build#dialog-build-folders)을 참조하십시오.

### 다이그레션(digression) 튜토리얼
{: #dialog-runtime-digression-tutorial}

이미 노드 세트가 설정된 작업 공간을 가져오려면 [튜토리얼](/docs/services/assistant?topic=assistant-tutorial-digressions)을 따르십시오. 다이그레션(digression)  작동 방식을 보여주는 몇 가지 연습을 할 수 있습니다.

### 디자인 고려사항
{: #dialog-runtime-digression-design-considerations}

*대체 노드 확산 방지*: 많은 대화 디자이너는 사용자가 분기에 걸리지 않도록 하기 위해 모든 대화 분기 끝에 *true* 또는 *anything_else* 조건이 있는 노드를 포함합니다. 이 디자인은 사용자 입력이 예상한 내용과 일치하지 않고 처리할 특정 대화 노드를 포함할 경우 일반 메시지를 리턴합니다. 그러나, 사용자는 이 접근 방식을 사용하는 대화 플로우에서 다이그레션(digression)할 수 없습니다.

  이 방법을 사용하는 모든 분기를 평가하여 분기로부터의 다이그레션(digression)을 허용하는 것이 더 나은지 여부를 결정하십시오. 사용자 입력이 예상한 내용과 일치하지 않으면 트리에서 완전히 다른 대화 플로우와 일치하는 내용을 찾을 수 있습니다. 일반 메시지에 응답하는 대신 나머지 대화를 효과적으로 사용하여 사용자의 입력을 처리할 수 있습니다. 그리고 루트 레벨 *Anything else* 노드는 다른 루트 노드가 처리할 수 없는 입력에 항상 응답할 수 있습니다.

**종료 노드로 이동 재검토**: 많은 대화가 *오늘 귀하의 질문에 답변을 드렸습니까?*라고 하는 표준 종료 질문을 하도록 설계되어 있습니다. 따라서, 모든 최종 분기 노드를 공통 종료 노드로 점프하도록 구성하면 다이그레션(digression)은 발생하지 않습니다. 메트릭 또는 다른 방법을 통해 사용자 만족 추적을 고려하십시오.

**가능한 다이그레션(digression) 체인 테스트**: 사용자가 현재 노드로부터 다이그레션(digression)을 허용하는 다른 노드로 다이그레션하는 경우, 사용자는 다른 노드로부터 잠재적으로 다이그레션할 수 있으며 이 패턴을 한 번 이상 다시 반복할 수 있습니다. 다이그레션(digression) 체인의 시작 노드가 다이그레션 후 돌아가도록 구성되어 있는 경우, 사용자는 결국 현재 대화 노드로 돌아옵니다. 사실상, 돌아가지 않도록 구성된 체인의 후속 노드는 다이그레션(digression)  대상으로 간주되지 않습니다. 개별 노드가 예상대로 작동하는지 여부를 결정하기 위해 여러 번 다이그레션(digression)하는 테스트 시나리오입니다.

**현재 노드에 우선순위가 있음**: 현재 플로우가 사용자 입력을 처리할 수 없는 경우 현재 플로우 외부의 노드는 다이그레션(digression) 대상으로 간주됩니다. 특히 다이그레션(digression)을 허용하는 슬롯이 있는 노드에서 중요한 것은 사용자에게 필요한 정보를 명확히 하고 사용자가 값을 제공한 후에 표시되는 확인 문장을 추가하는 것입니다.

  모든 슬롯은 슬롯 채우기 처리 중에 채워질 수 있습니다. 따라서, 슬롯이 사용자 입력을 캡처할 수 있습니다. 예를 들어, 저녁 식사 예약을 작성하는 데 필요한 정보를 수집하는 슬롯이 있는 노드가 포함될 수 있습니다. 슬롯 중 하나가 날짜 정보를 수집합니다. 예약 세부사항을 제공하는 중에 사용자는 '내일 날씨는 어떻습니까?'라고 문의할 수 있습니다. 그렇지만, 사용자의 입력에 '내일'이란 단어가 포함되어 있고 슬롯이 있는 예약 노드가 처리 중이기 때문에 서비스는 사용자가 예약 날짜를 제공하거나 업데이트한다고 가정합니다. *현재 노드에 항상 우선순위가 있습니다.* 명확한 확인 문장(예: `Ok, setting the reservation date to tomorrow`를 정의한 경우, 사용자는 잘못된 의사 소통이 있었음을 깨닫고 이를 정정합니다.

  반대로, 슬롯을 채우는 동안 사용자가 슬롯 중 어느 곳에서도 예상하지 않는 값을 제공하면, 사용자가 절대 다이그레션(digression)하려고 하지 않은 전혀 관련없는 루트 노드와 일치할 가능성이 있습니다.

  다이그레션(digression) 동작을 구성하려면 많은 테스트가 필요합니다.

- **슬롯 핸들러 대신 다이그레션(digression)을 사용할 때**: 사용자가 언제든지 문의할 수 있는 일반적인 질문의 경우, 다이그레션(digression)을 허용하는 루트 노드를 사용하고 입력을 처리한 다음 진행 중인 플로우로 되돌아갑니다. 슬롯이 있는 노드의 경우, 사용자가 슬롯을 채우는 동안 문의할 수 있는 관련 질문 유형을 예상하고 노드에 핸들러를 추가하여 해결하십시오.

  예를 들어, 슬롯이 있는 노드가 보험 청구를 작성하는 데 필요한 정보를 수집하는 경우, 보험에 대한 일반적인 질문을 처리하는 핸들러를 추가할 수 있습니다. 그러나 도움말을 가져오는 방법이나 상점 위치 또는 회사의 기록에 대한 질문은 루트 레벨 노드를 사용하십시오.

## 모호성 해제 ![플러스 또는 프리미엄 플랜만 해당](images/premium.png)
{: #dialog-runtime-disambiguation}

이 기능은 플러스 또는 프리미엄 사용자만 사용할 수 있습니다.
{: tip}

모호성 해제를 사용하는 경우, 둘 이상의 대화 노드가 입력에 응답할 수 있음을 발견할 때 서비스 사용자에게 도움을 요청하도록 지시합니다. 처리할 노드를 추측하는 대신, 어시스턴트가 상위 노드 옵션 목록을 사용자와 공유하고 사용자에게 올바른 노드를 선택하도록 요청합니다.

![어시스턴트가 사용자에게 명확성을 요청하는 사용자와 어시스턴트 간의 샘플 대화를 보여줍니다.](images/disambig-demo.png)

사용 가능한 경우, 다음 조건이 충족되지 않으면 모호성 해제가 트리거되지 않습니다

- 사용자 입력에서 발견된 하나 이상의 2위 인텐트의 신뢰도 점수가 최상위 인텐트의 신뢰도 점수의 55% 를 초과합니다.
- 최상위 인텐트의 신뢰도 점수가 0.2 이상입니다.

이 조건이 충족되는 경우에도 대화에 있는 두 개 이상의 독립 노드가 다음 기준을 충족하지 않으면 모호성 해제가 발생하지 않습니다.

- 노드 조건에는 모호성 해제를 트리거한 인텐트 중 하나가 포함됩니다. 또는 다른 경우에는 노드 조건이 true로 평가됩니다. 예를 들어, 노드가 엔티티 유형을 확인하고 엔티티가 사용자 입력에 멘션된 경우 적합합니다.
- 노드의 *외부 노드 이름* 필드에 텍스트가 있습니다.

자세히 보기

- [모호성 해제 예제](#dialog-runtime-disambig-example)
- [모호성 해제 사용](#dialog-runtime-disambig-enable)
- [노드 선택](#dialog-runtime-choose-nodes)
- [위 항목을 처리하지 않음](#dialog-runtime-handle-none)
- [모호성 해제 테스트](#dialog-runtime-disambig-test)

### 모호성 해제 예제
{: #dialog-runtime-disambig-example}

예를 들어, 취소 요청을 처리하는 인텐트 조건이 포함된 두 개의 노드가 있는 대화가 있습니다. 조건은 다음과 같습니다.

- eCommerce_Cancel_Product_Order
- Customer_Care_Cancel_Account

사용자 입력이 `i must cancel it today`인 경우, 다음 인텐트가 입력에서 발견될 수 있습니다

`[`
`{"intent":"Customer_Care_Cancel_Account","confidence":0.6618281841278076},`
`{"intent":"eCommerce_Cancel_Product_Order","confidence":0.4330700159072876},`
`{"intent":"Customer_Care_Appointments","confidence":0.2902342438697815},`
`{"intent":"Customer_Care_Store_Hours","confidence":0.2550420880317688},`
`...]`

이 서비스는 사용자의 목표가 `#Customer_Care_Cancel_Account` 인텐트에 일치한다고 `0.6618281841278076`(66%) 확신합니다. 다른 인텐트의 신뢰도 점수가 55% /66% 보다 큰 경우, 모호성 해제 후보 기준을 충족합니다.

`0.66 x 0.55 = 0.36`

점수가 0.36보다 큰 인텐트가 적합합니다.

이 예제에서는 `#eCommerce_Cancel_Product_Order` 인텐트가 임계값을 초과하며, 신뢰도 점수는 `0.4330700159072876`입니다.

사용자 입력이 `i must cancel it today`인 경우, 두 대화 노드 모두 응답할 수 있는 후보로 간주됩니다. 처리할 대화 노드를 판별하기 위해 어시스턴트가 사용자에게 하나를 선택하도록 요청합니다. 사용자가 이 중에서 선택하도록 지원하기 위해, 어시스턴트가 각 노드가 수행하는 작업에 대한 간단한 요약을 제공합니다. 표시되는 요약 텍스트는 각 노드에 대해 지정된 *외부 노드 이름* 정보에서 바로 추출됩니다.

![서비스에 계정 취소, 상품 주문 취소 및 위 항목 중 없음을 포함하여 대화 옵션 목록에서 선택하라는 메시지가 표시됩니다.] (images/disambig-tryitout.png)

서비스는 사용자 입력에 있는 `today` 용어를 `@sys-date` 엔티티의 멘션인 날짜로 인식합니다. 대화 트리에 `@sys-date` 엔티티를 조건으로 하는 노드가 포함되어 있는 경우, 모호성 해제 선택 목록에도 포함됩니다. 이 이미지에서 목록에 *캡처 날짜 정보* 옵션으로 포함되어 표시됩니다.

![서비스에 캡처 날짜 정보를 포함하여 대화 옵션 목록에서 선택하라는 메시지가 표시됩니다.](images/disambig-tryitout-date.png)

다음 동영상은 모호성 해제의 개요를 제공합니다.

<iframe class="embed-responsive-item" id="youtubeplayer0" title="모호성 해제의 개요" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/VVyklAXlmbA?rel=0" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

### 모호성 해제 사용
{: #dialog-runtime-disambig-enable}

모호성 해제를 사용하려면 다음 단계를 수행하십시오.

1.  대화 페이지에서 **설정**을 클릭합니다.
1.  **모호성 해제**를 클릭합니다.
1.  *모호성 해제* 섹션에서, 토글을 **On**으로 전환합니다.
1.  프롬프트 메시지 필드에서 대화 노드 옵션 목록 앞에 표시할 텍스트를 추가합니다. 예를 들어, *What do you want to do?*
1.  **선택사항**: 위 항목 중 없음 메시지 필드에서, 사용자가 원하는 것을 반영하는 다른 대화 노드가 없는 경우 선택할 수 있는 추가 옵션으로 표시할 텍스트를 추가합니다. 예를 들어, *위 항목 중 없음*이 있습니다.

    메시지를 짧게 유지하여 다른 옵션과 함께 일직선으로 표시되도록 합니다. 메시지는 512자 미만이어야 합니다. 사용자가 이 옵션을 선택하는 경우 서비스가 수행하는 작업에 대한 정보는 [위의 항목을 모두 처리하지 않음](#dialog-runtime-handle-none)을 참조하십시오.

1.  **닫기**를 클릭합니다.
1.  어시스턴트가 지원을 요청해야 하는 대화 노드를 결정합니다.

- 트리 계층 구조의 모든 레벨에서 노드를 선택할 수 있습니다.
    - 인텐트, 엔티티, 특수 조건, 컨텍스트 변수 또는 이 값의 조합을 조건으로 하는 노드를 선택할 수 있습니다.

    팁을 보려면 [노드 선택](#dialog-runtime-choose-nodes)을 참조하십시오.

    모호성 해제를 선택할 각 노드의 경우, 다음 단계를 완료하십시오.
                                            .
    1.  편집 보기에서 노드를 클릭하여 엽니다.
    1.  *외부 노드 이름* 필드에서 이 대화 노드가 처리하도록 설계된 사용자 태스크를 설명합니다. 예를 들어, *계정 취소*가 있습니다.

        ![노드 편집 보기에 외부 노드 이름 정보를 추가할 위치를 표시합니다.](images/disambig-node-purpose.png)

### 노드 선택
{: #dialog-runtime-choose-nodes}

대화의 고유한 분기로 제공되는 노드가 모호성 해제가 되도록 선택합니다. 여기에는 다른 노드의 하위인 노드가 포함될 수 있습니다. 해당 노드와 다른 노드를 구분하는 고유한 값을 노드의 조건으로 해야 합니다.

이 도구는 두 개 이상의 인텐트에 겹치는 사용자 예제가 있을 때 발생하는 인텐트 충돌을 인식할 수 있습니다. [그와 같은 충돌을 해결](/docs/services/assistant?topic=assistant-intents#intents-resolve-conflicts)하십시오. 먼저 인텐트가 고유한지 확인하는 것이 서비스가 더 높은 신뢰도 점수를 획득하는 데 도움이 됩니다.
{: note}

참고:

- 인텐트를 조건으로 하는 노드의 경우, 서비스가 노드의 인텐트 조건과 사용자의 인텐트가 일치한다고 확신하면 노드가 모호성 해제 옵션으로 포함됩니다.
- 부울 조건(true 또는 false로 평가되는 조건)이 있는 노드의 경우, 조건이 true로 평가되면 노드가 모호성 해제 옵션으로 포함됩니다. 예를 들어, 노드가 엔티티 유형을 조건으로 하는 경우, 모호성 해제를 트리거하는 입력에서 엔티티가 멘션되면 노드가 포함됩니다.
- 트리 계층 구조의 노드 순서는 모호성 해제에 영향을 줍니다.

  - 모호성 해제가 트리거되는지 여부에 영향을 줍니다.

    예를 들어, 모호성 해제를 도입하기 위해 이전에 사용된 [시나리오](#dialog-runtime-disambig-example)를 살펴봅니다. `@sys-date`를 조건으로 하는 노드가 `#Customer_Care_Cancel_Account` 및 `#eCommerce_Cancel_Product_Order` 인텐트를 조건으로 하는 노드보다 대화 트리에서 더 높게 배치된 경우, 사용자가 `i must cancel it today`를 입력할 때 모호성 해제가 트리거되지 않습니다. 이는 서비스가 트리에서 해당 노드의 배치로 인한 인텐트 참조보다 날짜 멘션(`today`)을 더 중요하다고 고려하기 때문입니다.

- 모호성 옵션 목록에 포함되는 노드에 영향을 줍니다

    때때로 노드가 예상된 모호성 해제 옵션으로 나열되지 않습니다. 이는 어떤 이유로 인해 모호성 해제 목록에 포함할 수 없는 노드가 조건 값을 참조하는 경우에 발생할 수 있습니다. 예를 들어, 엔티티 멘션은 대화 트리에 이전에 배치되었지만 모호성 해제를 사용할 수 없는 노드를 트리거할 수 있습니다. 동일한 엔티티가 모호성 해제에 대해 사용하도록 설정되었지만 트리에서 더 낮게 배치된 노드의 유일한 조건인 경우, 서비스가 지원되지 않으므로 모호성 해제 옵션으로 추가되지 않습니다. 이전 노드와 비교하여 생략되었으므로 서비스가 이후의 노드를 처리하지 않습니다.

모호성 해제를 선택한 각 노드의 경우, 모호성 해제 옵션 목록에 노드가 포함된 시나리오를 테스트합니다. 테스트는 런타임 시 모호성 해제가 올바르게 작동하는 데 영향을 주는 노드 순서나 기타 요인을 조정할 수 있는 기회를 제공합니다.

### 위의 항목을 처리하지 않음
{: #dialog-runtime-handle-none}

*위 항목 중 없음* 옵션을 클릭하는 경우 서비스가 메시지의 사용자 입력에서 인식된 인텐트를 제거하고 다시 제출합니다. 이 조치는 일반적으로 대화 트리의 다른 노드를 트리거합니다.

이 상황에서 리턴되는 응답을 사용자 정의하기 위해, 인식된 인텐트가 없는(인텐트가 제거됨) 사용자 입력을 확인하고 `suggestion_id` 특성을 포함하는 조건과 함께 루트 노드를 추가할 수 있습니다. 모호성 해제가 트리거될 때 `suggestion_id` 특성이 서비스에 추가됩니다.
{: tip}

다음 조건으로 루트 노드를 추가하십시오.

```json
intents.size()==0 && input.suggestion_id
```
{: codeblock}

이 조건은 사용자가 자신의 목표와 일치하지 않음을 표시한 모호성 해제 옵션 세트를 트리거한 입력으로만 충족됩니다.

제안한 옵션 중 요구사항을 충족하는 옵션이 없음을 이해할 수 있는 응답을 추가하고 적절한 조치를 취합니다.

트리에서 노드의 배치는 중요합니다. 사용자 입력에서 멘션된 엔티티 유형을 조건으로 하는 노드가 이 노드보다 트리에서 더 높은 경우 해당 응답이 대신 표시됩니다.

### 모호성 해제 테스트
{: #dialog-runtime-disambig-test}

모호성 해제를 테스트하려면 다음 단계를 수행하십시오.

1.  "시험 사용" 분할창에서, 모호성 해제에 적합한 후보라고 생각하는 테스트 발화를 입력하고, 둘 이상의 대화 노드가 이와 같은 발화를 처리하도록 구성하십시오.

1.  응답에 예상대로 선택할 수 있는 대화 노드 옵션 목록이 없는 경우, 먼저 각 노드의 외부 노드 이름 필드에 요약 정보를 추가했는지 확인하십시오.

1.  모호성 해제가 여전히 트리거되지 않는 경우, 노드에 대한 신뢰도 점수가 예상한 값에 가깝지 않은 것일 수 있습니다.

    특정 사용자 입력에 대해 리턴되는 인텐트, 엔티티 및 기타 특성에 대한 정보를 얻을 수 있습니다.

    - 사용자 입력에서 발견된 인텐트의 신뢰도 점수를 보려면 일시적으로 `<? intents ?>`를 트리거될 노드의 노드 응답 끝에 추가하십시오.

      이 SpEL 표현식은 사용자 입력에서 배열로 감지된 인텐트를 보여줍니다. 이 배열에는 인텐트 이름과 서비스가 사용자의 의도된 목표를 반영하고 있다는 신뢰도 레벨이 포함됩니다.

    - 사용자 입력에서 발견된 엔티티 종류(있는 경우)를 보려면, 현재 응답을 SpEL 표현식 `<? entities ?>`를 포함하는 단일 텍스트 응답으로 임시로 대체할 수 있습니다. .

      이 SpEL 표현식은 사용자 입력에서 배열로 감지된 엔티티를 보여줍니다. 이 배열에는 인텐트 이름, 사용자 입력 문자열 내 엔티티 멘션의 위치, 엔티티 멘션 문자열 및 서비스가 해당 용어가 지정된 엔티티 유형의 멘션이라고 확신하는 신뢰도 레벨이 포함됩니다.

    - 호출 시 지정된 컨텍스트 변수의 값과 같은 기타 특성을 포함하여 모든 아티팩트에 대한 세부사항을 한 번에 확인하기 위해 전체 API 응답을 검사할 수 있습니다. [API 호출 세부사항 보기](/docs/services/assistant?topic=assistant-dialog-tips#dialog-tips-inspect-api)를 참조하십시오.

1.  모호성 해제 옵션으로 표시될 것으로 예상하는 노드 중 하나 이상의 노드에 대해 *외부 노드 이름* 필드에 추가한 설명을 일시적으로 제거하십시오.

1.  테스트 발화를 "시험 사용" 분할창에 다시 입력하십시오.

    `<? intents ?>` 표현식을 응답에 추가한 경우, 리턴된 텍스트에 서비스가 테스트 발화에서 인식한 인텐트 목록이 포함되고 각각에 대해 신뢰도 점수가 포함됩니다.

    ![서비스에서 Customer_Care_Cancel_Account and eCommerce_Cancel_Product_Order를 포함하여 인텐트 배열을 리턴합니다.](images/disambig-show-intents.png)

테스트를 완료한 후 노드 응답에 추가된 모든 SpEL 표현식을 제거하거나 표현식으로 바꾼 원래 응답을 다시 추가하고, 텍스트를 제거한 모든 *외부 노드 이름* 필드를 다시 채우십시오.
