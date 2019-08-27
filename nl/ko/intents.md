---

copyright:
  years: 2015, 2019
lastupdated: "2019-08-06"

keywords: intent, intent conflicts, annotate

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

# 인텐트 정의
{: #intents}

***인텐트(intent)***는 고객 입력에 표현된 목적입니다(예: 질문에 응답 또는 요금 지불 처리). {{site.data.keyword.conversationshort}} 서비스는 고객 입력에 표현된 인텐트를 인식하여 응답을 위한 올바른 대화 플로우를 선택할 수 있습니다.
{: shortdesc}

<iframe class="embed-responsive-item" id="youtubeplayer" title="인텐트에 대한 작업" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/OPdOCUPGMIQ" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

## 인텐트 생성 개요
{: #intents-described}

- 애플리케이션의 인텐트를 계획합니다.

  고객이 수행할 수 있는 작업과 애플리케이션이 고객을 대신하여 처리할 수 있는 사항을 고려하십시오. 예를 들어, 고객이 구매하는 데 애플리케이션이 도움이 되도록 할 수 있습니다. 그런 경우, `#buy_something` 인텐트를 추가할 수 있습니다. (인텐트 이름 앞에 접두부로 추가된 `#`는 인텐트로 명확하게 식별하는 데 도움을 줍니다.)

- 인텐트에 대해 Watson을 교육합니다.

  애플리케이션이 고객을 위해 처리할 비즈니스 요청을 결정한 후에는 이에 대해 Watson에 알려야 합니다. 각 비즈니스 목표(예: `#buy_something`)의 경우, 고객이 일반적으로 목표를 표시하는 데 사용하는 발화(utterance)의 예를 5개 이상 제공해야 합니다. 예를 들어, `I want to make a purchase`가 있습니다.
  
  이상적으로는, 기존 비즈니스 프로세스에서 추출할 수 있는 실제 사용자 발화 예제를 찾습니다. 사용자 예제는 특정 비즈니스에 맞게 조정해야 합니다. 예를 들어, 사용자가 보험 회사인 경우, 사용자 예제는 다음과 같습니다. `I want to buy a new XYZ insurance plan.`
  
  어시스턴트는 사용자가 제공하는 예제를 사용하여 동일하고 유사한 발화 유형을 인식할 수 있는 기계 학습 모델을 빌드하고 적절한 인텐트에 맵핑합니다.

몇 가지 인텐트로 시작하여 애플리케이션의 범위를 반복적으로 확장할 때 테스트하십시오.

![Plus 또는 Premium 플랜만 해당](images/plus.png)이미 온라인 신청에서 수집한 콜 센터 또는 고객 조회에서 대화 기록이 이미 있는 경우 해당 데이터를 작업에 사용하십시오. Watson과 실제 고객 발화를 공유하여 Watson이 사용자의 요구에 맞는 최적의 인텐트와 인텐트 사용자 예제를 권장할 수 있게 하십시오. 자세한 내용은 [인텐트 정의에 대한 도움말 가져오기](/docs/services/assistant?topic=assistant-intent-recommendations)를 참조하십시오.

## 인텐트 작성
{: #intents-create-task}

1.  대화 스킬을 여십시오. 스킬이 **인텐트** 페이지에 열립니다.

1.  **인텐트 작성**을 선택하십시오.

1.  **인텐트 이름** 필드에 인텐트 이름을 입력하십시오.
    - 인텐트 이름에는 문자(유니코드), 숫자, 밑줄, 하이픈 및 마침표가 포함될 수 있습니다.
    - 이름은 `..` 또는 마침표만으로 이루어진 다른 문자열로 구성될 수 없습니다.
    - 인텐트 이름에는 공백이 포함될 수 없으며 128자를 초과하면 안됩니다. 다음은 인텐트 이름의 예제입니다.
        - `#weather_conditions`
        - `#pay_bill`
        - `#escalate_to_agent`

    숫자 부호(`#`)는 인텐트를 용어를 식별하는 데 도움이 되도록 인텐트 이름 앞에 자동으로 추가됩니다. 사용자가 추가할 필요는 없습니다.{: tip}

    선택적으로 **설명** 필드의 인텐트 설명을 추가하십시오.

1.  인텐트 이름을 저장하려면 **인텐트 작성**을 선택하십시오.

    ![새 인텐트 정의를 표시하는 화면 캡처](images/create_intent.png)

1.  다음으로, **사용자 예제 추가** 필드에 인텐트에 대한 사용자 예제의 텍스트를 입력하십시오. 예제는 길이가 최대 1024자인 문자열일 수 있습니다. 다음 발화는 `#pay_bill` 인텐트의 예제입니다.
    - `I need to pay my bill.`
    - `Pay my account balance`
    - `make a payment`

    사용자 예제에 엔티티에 대한 참조를 포함하여 발생하는 영향에 대해 알아보려면 [엔티티 참조를 처리하는 방법](#intents-entity-references)을 참조하십시오.
    {: tip}

    인텐트 이름 및 예제 텍스트는 애플리케이션이 {{site.data.keyword.conversationshort}}와 상호작용할 때 URL에서 노출될 수 있습니다. 이러한 아티팩트에 민감한 정보 또는 개인 정보를 포함하지 마십시오.
    {: important}

1.  사용자 예제를 저장하려면 **예제 추가**를 클릭하십시오.

1.  동일한 프로세스를 반복하여 예제를 추가하십시오.

    인텐트마다 5개 이상의 예제를 제공하십시오.
    {: important}

    ![Plus 또는 Premium 플랜만 해당](images/plus.png) 사용자 예제 작성에 대한 도움말을 보려면 [인텐트 사용자 예제 추천 가져오기](/docs/services/assistant?topic=assistant-intent-recommendations#intent-recommendations-get-example-recommendations)를 참조하십시오.

1.  예제 추가를 완료하면, ![닫기 화살표](images/close_arrow.png)를 클릭하여 인텐트 작성을 완료하십시오.

시스템은 사용자가 추가한 인텐트 및 사용자 예제에 대해 훈련하기 시작합니다.

## 엔티티 참조가 처리되는 방법
{: #intents-entity-references}

사용자 예제에 엔티티 멘션을 포함하는 경우 기계 학습 모델은 다음과 같은 시나리오에서 다양한 방법으로 정보를 사용합니다.

- [인텐트 예제에서 엔티티 값 및 동의어 참조](#intents-related-entities)
- [어노테이션이 있는 멘션](#intents-annotated-mentions)
- [인텐트 예제에서 엔티티 이름을 직접 참조](#intents-entity-as-example)

### 인텐트 예제에서 엔티티 값 및 동의어 참조
{: #intents-related-entities}

이 인텐트에 해당하는 엔티티를 정의했거나 정의하려는 경우, 일부 예제에서 엔티티 값 또는 동의어를 멘션하십시오. 이렇게 하면 인텐트와 엔티티 간 관계를 설정할 수 있습니다. 이는 약한 관계이지만, 모델에 알립니다.

![인텐트 정의를 표시하는 화면 캡처](images/define_intent.png)

*중요*:

  - 인텐트 예제 데이터는 사용자가 제공하는 데이터의 대표적이고 일반적이어야 합니다. 예제는 실제 사용자 데이터 또는 특정 분야의 전문가로부터 수집할 수 있습니다. 데이터의 대표적이고 정확한 네이처가 중요합니다.
  - 훈련 및 테스트 데이터(평가 목적으로) 모두 실제 사용에서 인텐트 배포를 반영해야 합니다. 일반적으로, 자주 발생하는 인텐트는 상대적으로 더 많은 예제와 보다 나은 응답 범위를 가지고 있습니다.
  - 자연스럽게 나타나는 한 예제 텍스트에 구두점을 포함할 수 있습니다. 일부 사용자가 구두점을 포함하는 예제의 인텐트를 표현하고 일부 사용자는 구두점을 표현하지 않을 것이므로 두 버전을 모두 포함하십시오. 일반적으로, 다양한 패턴에 대한 적용 범위가 클수록 더 나은 응답이 됩니다.

### 어노테이션이 있는 멘션
{: #intents-annotated-mentions}

엔티티를 정의할 때 기존의 인텐트 사용자 예제에서 바로 엔티티의 멘션에 대해 어노테이션을 작성할 수 있습니다. 이런 방식으로 식별하는 인텐트와 엔티티 간의 관계는 인텐트 분류 모델에서 사용되지 *않습니다*. 그러나 엔티티에 멘션을 추가하는 경우 해당 엔티티에 새 값으로 추가됩니다. 기존 엔티티 값에 멘션을 추가하는 경우 해당 엔티티 값에 새 동의어로 추가됩니다. 인텐트 분류는 인텐트 사용자 예제에서 이와 같은 유형의 사전 참조를 사용하여 인텐트와 엔티티 간에 약한 참조를 설정합니다.

컨텍스트 엔티티에 대한 자세한 정보는 [컨텍스트 엔티티 추가](/docs/services/assistant?topic=assistant-entities#entities-create-annotation-based)를 참조하십시오.

### 인텐트 예제에서 엔티티 이름을 직접 참조
{: #intents-entity-as-example}

이러한 접근 방식은 고급 사용자를 위한 것입니다. 사용하는 경우에는 일관되게 사용해야 합니다.
{: note}

인텐트 예제에서 엔티티를 직접 참조하도록 선택할 수 있습니다. 예를 들어, *Galaxy S8*, *Moto Z2*, *LG G6* 및 *Google Pixel 2* 값을 포함하는 `@PhoneModelName`이라는 엔티티가 있습니다. 인텐트를 작성할 때(예: `#order_phone`) 다음과 같이 훈련 데이터를 제공할 수 있습니다.

- Can I get a `@PhoneModelName`?
- Help me order a `@PhoneModelName`.
- Is the `@PhoneModelName` in stock?
- Add a `@PhoneModelName` to my order.

![인텐트 정의를 표시하는 화면 캡처](images/define_intent_entity.png)

현재 정의한 동의어 엔티티만 직접 참조할 수 있습니다(패턴 값은 무시됨). [시스템 엔티티](/docs/services/assistant?topic=assistant-system-entities)를 사용할 수 없습니다.

엔티티를 훈련 데이터의 *어느 곳에서*도 참조하기로 선택한 경우 인텐트 예제(예: `@PhoneModelName`)의 다른 어느 곳에서도 직접 참조(예: *Galaxy S8*)를 사용하는 값을 취소합니다. 그러면 모든 인텐트가 인텐트-예제-엔티티 접근 방식을 사용합니다. 특정 인텐트에만 이 접근 방식을 적용할 수 없습니다.
{: important}

실제로, 이것은 직접 참조(*Galaxy S8*)를 기반으로 하는 대부분의 인텐트를 미리 훈련했고 하나의 인텐트에 대한 엔티티 참조(`@PhoneModelName`)를 사용하면 해당 변경이 이전 훈련에 영향을 미친다는 것을 의미합니다. `@Entity` 참조를 사용하도록 선택하면 이전의 모든 직접 참조를 `@Entity` 참조로 바꿔야 합니다.

10개의 값이 정의된 `@Entity`를 사용하여 하나의 예제 인텐트를 정의하는 것은 해당 예제를 10회 지정하는 것과 동일**하지 않습니다**. {{site.data.keyword.conversationshort}} 서비스는 그 하나의 예제 인텐트 구문에 크게 비중을 두지 않습니다.

## 인텐트 테스트
{: #intents-test}

새 인텐트 작성을 완료한 후 시스템을 테스트하여 인텐트를 예상대로 인식하는지 여부를 볼 수 있습니다.

1.  ![Watson에게 질문](images/ask_watson.png) 아이콘을 클릭하십시오.

1.  "시험 사용" 분할창에서, 질문 또는 기타 텍스트 문자열을 입력한 후 Enter를 눌러 인식되는 인텐트를 확인하십시오. 잘못된 인텐트가 인식되면, 올바른 인텐트에 이 텍스트를 예제로 추가하여 모델을 개선할 수 있습니다.

    최근에 스킬을 변경한 경우, 시스템이 아직 재훈련하고 있음을 나타내는 메시지가 표시됩니다. 이 메시지가 표시되면 테스트 전에 훈련이 완료될 때까지 기다리십시오.
    {: tip}

    ![재훈련 메시지를 표시하는 화면 캡처](images/training.png)

    응답은 입력에서 인식된 인텐트를 표시합니다.

    ![인텐트 테스트 화면 캡처](images/test_intents.png)

1.  시스템이 올바른 인텐트를 인식하지 못하는 경우 이를 정정할 수 있습니다. 인식된 인텐트를 정정하려면 표시된 인텐트를 선택한 다음 목록에서 올바른 인텐트를 선택하십시오. 정정사항이 제출되면 시스템은 자동으로 자체 재훈련하여 새 데이터를 통합합니다.

    ![인식된 인텐트를 정정하는 화면 캡처](images/correct_intent.png)

1.  {: #intents-mark-irrelevant}입력이 애플리케이션의 인텐트와 관련이 없는 경우, 표시된 인텐트를 선택한 다음 **관련 없음으로 표시**를 클릭하여 어시스턴트를 교육할 수 있습니다.

    ![관련 없음으로 표시 화면 캡처](images/irrelevant.png)

    이 조치에 대한 자세한 정보는 [무시할 수 있는 주제에 대해 어시스턴트 교육 ](/docs/services/assistant?topic=assistant-logs#logs-mark-irrelevant)을 참조하십시오.

인텐트가 올바르게 인식되지 않는 경우 다음과 같은 변경을 고려하십시오.

- 올바른 인텐트에 인식되지 않는 텍스트를 예제로 추가하십시오.
- 기존 예제를 하나의 인텐트에서 다른 인텐트로 이동하십시오.
- 인텐트가 너무 비슷한지를 생각하고 이 인텐트를 적절하게 다시 정의하십시오.

## 절대 스코어링
{: #intents-absolute-scoring}

{{site.data.keyword.conversationshort}} 서비스는 다른 인텐트와 관련 없이 독립적으로 각 인텐트의 신뢰도를 점수화합니다. 이 방법은 유연성을 추가하므로, 단일 사용자 입력에서 여러 인텐트를 발견할 수 있습니다. 또한 시스템이 인텐트를 리턴하지 않을 수도 있음을 의미합니다. 최상위 인텐트의 신뢰도 점수가 낮은 경우(0.2미만), 최상위 인텐트가 API에서 리턴하는 인텐트 배열에 포함되지만, 해당 인텐트를 조건으로 하는 노드가 트리거되지 않습니다. 신뢰도 점수가 양호한 인텐트가 검색되지 않은 경우를 발견하려면 대화 노드에서 `irrelevant` 특수 조건을 사용하십시오. 자세한 내용은 [특수 조건](/docs/services/assistant?topic=assistant-dialog-overview#dialog-overview-special-conditions)을 참조하십시오.

인텐트 신뢰도 점수가 변경되면서 대화를 재구성해야 할 수 있습니다. 예를 들어, 대화 노드가 해당 조건에서 인텐트를 사용하고 인텐트의 신뢰도 점수가 0.2 아래로 지속적으로 감소하기 시작하면 대화 노드의 처리가 중지됩니다. 신뢰도 점수가 변경되면 대화의 동작도 변경될 수 있습니다.

## 인텐트 한계
{: #intents-limits}

작성할 수 있는 인텐트 및 예제의 수는 {{site.data.keyword.conversationshort}} 플랜 유형에 따라 다릅니다.

| 플랜     | 스킬당 인텐트 | 스킬당 예제 |
|------------------|------------------:|-------------------:|
| Premium          |             2,000 |             25,000 |
| Plus             |             2,000 |             25,000 |
| 표준         |             2,000 |             25,000 |
| Lite, Plus Trial |               100 |             25,000 |
{: caption="플랜 세부사항" caption-side="top"}

## 인텐트 편집
{: #intents-edit}

목록에서 인텐트를 클릭하여 편집을 위해 열 수 있습니다. 다음과 같이 변경할 수 있습니다.

- 인텐트의 이름을 바꿉니다.
- 인텐트를 삭제합니다.
- 예제를 추가하거나 편집하거나 삭제합니다.
- 다른 인텐트로 예제를 이동합니다.

인텐트 이름에서 각 예제로 탭을 사용하여 이동할 수 있으며, (원하는 경우) 예제를 편집할 수 있습니다.

예제를 이동하거나 삭제하려면 연관된 선택란을 클릭한 다음 **이동** 또는 **삭제**를 클릭하십시오.

  ![예제 이동 또는 삭제 방법을 표시하는 화면 캡처](images/move_example.png)

## 인텐트 검색
{: #intents-search}

사용자 예제, 인텐트 이름 및 설명을 찾으려면 검색 기능을 사용하십시오.

1.  **인텐트** 페이지에서 검색 아이콘을 클릭하십시오.

    ![인텐트 페이지 헤더의 검색 아이콘](images/header-search-icon.png)

1.  검색어 또는 구를 입력하십시오.

    ![인텐트 검색어](images/searchint_1.png)

검색어를 포함한 인텐트가 해당 예제와 함께 표시됩니다.

  ![인텐트 검색 리턴](images/searchint_2.png)

## 내보낸 인텐트
{: #intents-export}

여러 개의 인텐트를 CSV 파일로 내보낼 수 있으므로 다른 {{site.data.keyword.conversationshort}} 애플리케이션으로 가져와서 다시 사용할 수 있습니다.

1.  **인텐트** 페이지에서, 목록에서 원하는 인텐트를 선택하고 **내보내기**를 클릭하십시오.

    ![내보내기 옵션](images/ExportIntent.png)

## 인텐트 및 예제 가져오기
{: #intents-import}

많은 수의 인텐트 및 예제가 있는 경우 이러한 인텐트와 예제를 하나씩 정의하는 것보다 쉼표로 구분된 값(CSV) 파일에서 가져오는 것이 더 쉽습니다. 파일에 포함된 사용자 예제에서 개인 데이터를 제거해야 합니다.

또는 원시 사용자 발화(예: 콜 센터 로그에서)를 사용하여 파일을 업로드하고 Watson이 해당 데이터에서 사용자 예제의 후보를 찾도록 할 수 있습니다. 자세한 내용은 [로그 파일에서 예제 추가](/docs/services/assistant?topic=assistant-intent-recommendations#intent-recommendations-get-example-recommendations)를 참조하십시오. 이 기능은 Plus 또는 Premium 사용자만 사용할 수 있습니다.

1.  CSV 파일에 인텐트와 예제를 수집하거나 스프레드시트에서 CSV 파일로 내보내십시오. 파일의 각 행에 대한 필수 형식은 다음과 같습니다.

    ```
    <example>,<intent>
    ```
    {: screen}

    여기서 `<example>`은 사용자 예제의 텍스트이고, `<intent>`는 예제에서 일치시킬 인텐트의 이름입니다. 예:

    ```
    Tell me the current weather conditions.,weather_conditions
    Is it raining?,weather_conditions
    What's the temperature?,weather_conditions
    Where is your nearest location?,find_location
    Do you have a store in Raleigh?,find_location
    ```
    {: screen}

    **중요:** UTF-8로 인코딩되고 BOM(Byte Order Mark)은 없는 CSV 파일을 저장하십시오.

1.  **인텐트** 페이지에서 *가져오기* 아이콘 ![가져오기 아이콘](images/importGA.png)을 클릭한 다음, 파일을 끌거나 컴퓨터에서 파일을 찾아서 선택하십시오.

    ![가져오기 옵션](images/ImportIntent.png)

    **중요:** 최대 CSV 파일 크기는 10MB입니다. 더 큰 CSV 파일의 경우 여러 개의 파일로 분할하고 개별적으로 가져오는 것을 고려하십시오.

    파일을 유효성 검증하여 가져오며, 시스템은 새 데이터에 대한 자체 훈련을 시작합니다.

가져온 인텐트 및 해당 예제를 **인텐트** 탭에서 볼 수 있습니다. 새 인텐트와 예제를 보려면 페이지를 새로 고쳐야 합니다.

## 인텐트 충돌 해결 ![Plus 또는 Premium만 해당](images/plus.png)
{: #intents-resolve-conflicts}

이 기능은 Plus 또는 Premium 사용자만 사용할 수 있습니다.
{: note}

*개별* 인텐트에서 둘 이상의 인텐트 예제가 매우 유사하여 {{site.data.keyword.conversationshort}}에서 사용할 인텐트에 대해 혼동되는 경우 {{site.data.keyword.conversationshort}} 애플리케이션에서 충돌을 발견합니다.

충돌을 해결하려면 다음을 수행하십시오.

1.  **인텐트** 페이지 페이지에서 충돌이 있는 모든 인텐트를 검토하십시오.

    ![인텐트 목록의 충돌](images/ConflictIntent1.png)

    충돌이 있는 인텐트의 목록을 보려면 **충돌만 표시** 스위치를 전환하십시오.
    {: tip}

    ![충돌만 보기](images/ConflictIntent2.png)

1.  인텐트 충돌을 여십시오. 충돌을 일으키는 인텐트의 경우 **충돌 해결**을 클릭하십시오.

    ![인텐트 충돌 예제](images/ConflictIntent3.png)

1.  이제 충돌 예제를 다른 인텐트로 이동하거나 충돌 예제를 완전히 삭제할 수 있는 옵션이 있습니다.

    이 경우, 예제 `Cancel my order` 및 `I want to cancel my order`가 `#cancel` 인텐트 및 `#eCommerce_Cancel_Product_Order` 인텐트에 표시됩니다.

    ![인텐트 충돌 예제](images/ConflictIntent4.png)

    추가적인 사용자 예제에서는 반드시 충돌하는 것은 아니지만 충돌하는 예제와 유사한 예제를 훈련합니다. 이는 충돌을 해결하는 데 도움이 되는 컨텍스트를 제공하기 위해 표시됩니다.

1.  예제 `Cancel my order` 및 `I want to cancel my order`를 선택하고 이를 `#cancel` 인텐트에서 `#eCommerce_Cancel_Product_Order` 인텐트로 이동하십시오.

    ![인텐트 충돌 예제](images/ConflictIntent5.png)

1.  예제를 배치할 위치를 결정하는 경우, 동일하거나 거의 동일한 예제가 있는 인텐트를 검색하십시오.

    각 인텐트가 고유하도록 유지하고 가능한 목표에 초점을 둡니다. 두 개의 인텐트에 중복되는 사용자 예제가 여러 개 있는 경우, 두 개의 개별 인텐트가 필요하지 않을 수 있습니다. 한 인텐트에 바로 겹쳐지지 않는 사용자 예제를 이동하거나 삭제한 후 다른 예제를 삭제할 수 있습니다.
    {: tip}

    `#cancel` 인텐트에서 다른 예제를 선택한 후 이를 삭제하십시오.

    ![인텐트 충돌 예제](images/ConflictIntent6.png)

1.  충돌을 해결하려면 **제출** 단추를 클릭하십시오.

    ![인텐트 충돌 예제](images/ConflictIntent7.png)

    *재설정* 옵션을 사용하면 충돌 예제를 인텐트로 이동하여 다시 시작할 수 있습니다. *취소*하면 인텐트 페이지로 돌아갑니다.

충돌을 해결했으며 충돌이 있는 다른 인텐트에 대한 검토를 계속할 수 있습니다.

자세한 내용을 보려면 다음 동영상을 시청하십시오.

<iframe class="embed-responsive-item" id="youtubeplayer0" title="인텐트 충돌 해결 개요" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/9gQtjCBxjdc?rel=0" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

## 인텐트 삭제
{: #intents-delete}

삭제할 인텐트 수를 선택할 수 있습니다.

**중요**: 인텐트를 삭제하여 연관된 모든 예제를 삭제하며 이러한 항목은 나중에 검색할 수 없습니다. 삭제된 컨텐츠를 더 이상 참조하지 않도록 이러한 인텐트를 참조하는 모든 대화 노드를 수동으로 업데이트해야 합니다.

1.  **인텐트** 페이지에서, 목록에서 원하는 인텐트를 선택하고 **삭제**를 클릭하십시오.

    ![삭제 옵션](images/DeleteIntent.png)
