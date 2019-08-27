---

copyright:
  years: 2015, 2019
lastupdated: "2019-04-12"

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

# 튜토리얼: 슬롯이 있는 노드를 대화에 추가
{: #tutorial-slots}

이 튜토리얼에서는 단일 노드 내에서 사용자로 부터 여러 단편 정보를 수집하여 대화 노드에 슬롯을 추가합니다. 사용자가 작성하는 노드는 식당 예약에 필요한 정보를 수집합니다.
{: shortdesc}

## 학습 목표
{: #tutorial-slots-objectives}

튜토리얼을 완료할 때까지 다음을 수행하는 방법을 파악합니다.

- 대화에 필요한 인텐트와 엔티티 정의
- 대화 노드에 슬롯 추가
- 슬롯이 있는 노드 추가

### 소요 시간
{: #tutorial-slots-duration}

이 튜토리얼을 완료하는 데 대략 30분 정도 소요됩니다.

### 전제조건
{: #tutorial-slots-prereqs}

시작하기 전에 [시작하기 튜토리얼](/docs/services/assistant?topic=assistant-getting-started)을 완료하십시오. 사용자가 작성한 {{site.data.keyword.conversationshort}} 튜토리얼 스킬을 사용하고 시작하기 연습의 일부로 빌드된 단순 대화에 노드를 추가합니다.

원하는 경우 새 대화 스킬로 시작할 수도 있습니다. 이 튜토리얼을 시작하기 전에 스킬을 작성해야 합니다.
{: note}

## 1단계: 인텐트 및 예제 추가
{: #tutorial-slots-add-intent}

인텐트 탭에서 인텐트를 추가하십시오. 인텐트는 사용자 입력에 표현된 목적이나 목표입니다. 사용자가 식당 예약을 하려는 사용자 입력을 인식하는 #reservation 인텐트를 추가하십시오.

1.  튜토리얼 스킬의 **인텐트** 페이지에서 **인텐트 추가**를 클릭하십시오.
1.  다음 인텐트 이름을 추가하고 **인텐트 작성**을 클릭하십시오.

    ```json
    reservation
    ```
    {: screen}

    #reservation 인텐트가 추가되었습니다. 숫자 부호(`#`)는 인텐트로 레이블을 지정하려는 인텐트 이름 앞에 추가됩니다. 이 이름 지정 규칙은 사용자 및 다른 사람에게 인텐트를 인텐트로 인식하도록 도와줍니다. 아직 이와 관련된 사용자 발화(utterance) 예제는 없습니다.
1.  **사용자 예제 추가** 필드에서, 다음 발화를 입력한 다음 **예제 추가**를 클릭하십시오.

    ```json
    i'd like to make a reservation
    ```
    {: screen}

1.  다음과 같은 추가 예제는 Watson이 `#reservation` 인텐트를 인식하는데 도움이 됩니다.

    ```json
    I want to reserve a table for dinner
    Can 3 of us get a table for lunch?
    do you have openings for next Wednesday at 7?
    Is there availability for 4 on Tuesday night?
    i'd like to come in for brunch tomorrow
    can i reserve a table?
    ```
    {: screen}

1.  `#reservation` 인텐트 및 관련 예제 발화 추가를 완료하려면, **닫기** ![닫기 화살표](images/close_arrow.png) 아이콘을 클릭하십시오.

## 2단계: 엔티티 추가
{: #tutorial-slots-add-entity}

엔티티 정의는 주어진 인텐트의 컨텍스트에서 자주 사용되는 어휘를 나타내는 엔티티 *값*의 세트를 포함합니다. 엔티티를 정의하면 관심있는 인텐트와 관련된 사용자 입력의 참조를 어시스턴트에서 식별할 수 있습니다. 이 단계에서는 시간, 날짜 및 숫자에 대한 참조를 인식할 수 있는 시스템 엔티티를 사용합니다.

1.  엔티티 페이지를 열려면 **엔티티**를 클릭하십시오.
1.  사용자 입력에서 날짜, 시간 및 숫자를 인식할 수 있는 시스템 엔티티를 사용하십시오. **시스템 엔티티** 탭을 클릭한 다음 엔티티를 켜십시오.

    - `@sys-time`
    - `@sys-date`
    - `@sys-number`

@sys-date, @sys-time 및 @sys-number 시스템 엔티티가 사용하도록 설정되었습니다. 이제 대화에서 사용할 수 있습니다.

## 3 단계: 슬롯이 있는 대화 노드 추가
{: #tutorial-slots-add-dialog-with-slots}

대화 노드는 어시스턴트와 사용자 간의 대화의 시작을 나타냅니다. 이는 어시스턴트가 처리할 노드에 대해 충족되어야 하는 조건이 포함되어 있습니다. 최소한, 응답도 포함됩니다. 예를 들어, 노드 조건은 사용자 입력에서 `#hello` 인텐트를 찾고 `Hi. How can I help you?`로 응답합니다. 이 예제는 단일 조건 및 응답을 포함하고 있는 가장 간단한 한 개의 대화 노드 양식입니다. 조건부 응답을 단일 노드에 추가하고, 사용자와의 교환을 연장하는 하위 노드를 추가하는 등 훨씬 더 복잡한 대화를 정의할 수 있습니다. (복합 대화에 대한 자세한 정보는 [복합 대화 빌드](/docs/services/assistant?topic=assistant-tutorial) 튜토리얼을 참고하십시오.)

이 단계에서 추가할 노드는 슬롯을 포함하는 노드입니다. 슬롯은 단일 노드 내의 사용자로부터 여러 정보를 요청하고 저장할 수 있는 구조화된 형식을 제공합니다. 슬롯은 생각하고 있는 특정 태스크가 있고 이를 수행하기 전에 사용자의 중요한 정보가 필요할 때 가장 유용합니다. 자세한 정보는 [슬롯을 사용하여 정보 수집](/docs/services/assistant?topic=assistant-dialog-slots)을 참조하십시오.

추가한 노드는 식당에서 예약하는 데 필요한 정보를 수집합니다.

1.  대화 트리를 열려면 **대화** 탭을 클릭하십시오.
1.  **#General_Greetings** 노드에 있는 추가 아이콘![추가 옵션](images/kabob.png)을 클릭한 다음 **아래에 노드 추가**를 선택하십시오.
1.  조건 필드에서 `#reservation`을 입력한 다음 목록에서 선택하십시오.
    이 노드는 사용자 입력이 `#reservation` 인텐트와 일치할 경우, 평가됩니다.
1.  **사용자 정의**를 클릭하고, **슬롯** 토글을 클릭하여 **켠**다음 **적용**을 클릭하십시오.

    ![슬롯을 켜는 사용자 정의 대화 표시.](images/slots-toggle-on.png)
1.  다음 슬롯을 정의하십시오.

    <table>
    <caption>슬롯 세부사항</caption>
    <tr>
      <th>검사 대상</th>
      <th>저장할 이름</th>
      <th>지정되지 않은 경우, 요청</th>
    </tr>
    <tr>
      <td>@sys-date</td>
      <td>$date</td>
      <td>What day would you like to come in?</td>
    </tr>
    <tr>
      <td>@sys-time</td>
      <td>$time</td>
      <td>What time do you want the reservation to be made for?</td>
    </tr>
    </tr>
    <tr>
      <td>@sys-number</td>
      <td>$guests</td>
      <td>How many people will be dining?</td>
    </tr>
    </table>

1.  응답으로 다음과 같이 지정하십시오. `OK. I am making you a reservation for $guests on $date at $time.`

    ![각 슬롯과 응답이 지정된 대로 채워지면 어떻게 보이는지를 표시합니다.](images/slots-simple-node.png)

1.  노드 편집 보기를 닫으려면 ![닫기](images/close.png)를 클릭하십시오.

## 4 단계: 대화 테스트
{: #tutorial-slots-test}

1.  ![Watson에게 질문](images/ask_watson.png) 아이콘을 선택하여 대화 분할창을 여십시오.
1.  `i want to make a reservation`을 입력하십시오.

    어시스턴트가 #reservation 인텐트를 인식하고 첫 번째 슬롯에 대한 프롬프트(`What day would you like to come in?`)로 응답합니다.

1.  `Friday`를 입력하십시오.

    어시스턴트가 값을 인식하고 이 값을 사용하여 첫 번째 슬롯에 대한 $date 컨텍스트 변수를 채웁니다. 그런 다음, 다음 슬롯에 대한 프롬프트(`What time do you want the reservation to be made for?`)를 표시합니다.

1.  `5pm`을 입력하십시오.

    어시스턴트가 값을 인식하고 이 값을 사용하여 두 번째 슬롯에 대한 $time 컨텍스트 변수를 채웁니다. 그런 다음, 다음 슬롯에 대한 프롬프트(`How many people will be dining?`)를 표시합니다.

1.  `6`을 입력하십시오.

    어시스턴트가 값을 인식하고 이 값을 사용하여 세 번째 슬롯에 대한 $guests 컨텍스트 변수를 채웁니다. 이제 모든 슬롯이 채워졌으므로 다음과 같은 노드 응답을 표시합니다. `OK. I am making you a reservation for 6 on 2017-12-29 at 17:00:00.`

![노드 슬롯에 채워진 테스트 대화를 시험 사용 분할창에 표시.](images/slots-test-simple-node.png)

작업이 완료되었습니다! 축하합니다. 슬롯이 있는 노드가 작성되었습니다.

## 요약
{: #tutorial-slots-summary}

이 튜토리얼에서는식당에서 테이블을 예약하는 데 필요한 정보를 캡처할 수 있는 슬롯이 있는 노드를 작성했습니다.

## 다음 단계
{: #tutorial-slots-next-steps}

노드와 상호작용하는 사용자의 경험을 개선합니다. 다음 튜토리얼을 완료하십시오. [슬롯이 있는 노드 개선](/docs/services/assistant?topic=assistant-tutorial-slots-complex). 이 단계에선 시스템에서 리턴한 날짜(2017-12-28) 및 시간(17:00:00) 값을 다시 재형식화하는 방법과 같은 간단한 개선 사항을 다룹니다. 또한, 대화가 슬롯에 대해 예상되는값의 유형을 사용자가 제공하지 않는 경우 수행할 작업과 같은 더 복잡한 작업도 다룹니다.
