---

copyright:
  years: 2015, 2019
lastupdated: "2019-07-17"

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

# 새 시스템 엔티티 ![베타](images/beta.png)
{: #beta-system-entities}

새 시스템 엔티티를 사용하여 IBM이 제공한 숫자 기반 시스템 엔티티에 대한 개선 사항을 활용할 수 있습니다.

현재 이 설정은 영어 또는 독일어로만 작성된 대화 스킬에 사용할 수 있습니다.
{: note}

새 시스템 엔티티는 사용자 입력에서 더 많은 미묘한 멘션을 인식할 수 있습니다. 예를 들어, 이름으로 멘션될 때 `@sys-date`는 `Thanksgiving`과 같은 국경일의 날짜를 계산할 수 있습니다. `@sys-date`는 사용자 입력에 멘션된 날짜의 일부로 연도가 지정되는 시기를 인식할 수 있습니다. 또한 이 개선사항으로 사용자의 어시스턴트가 숫자 기반 시스템 엔티티를 구분하기가 훨씬 쉬워졌습니다. 예를 들어 `@sys-date`로 인식되는 날짜 멘션(예: `May 10`)은 `@sys-number` 멘션으로 식별되지 않습니다. 

`@sys-location` 및 `@sys-person` 시스템 엔티티는 변경되지 않았습니다.
{: note}

## 세 시스템 엔티티 사용
{: #beta-system-entities-enable}

새 시스템 엔티티를 사용으로 설정하려면 다음 단계를 완료하십시오.

1.  스킬 페이지에서 스킬을 여십시오.
1.  **옵션** 탭을 클릭하십시오.
1.  **시스템 엔티티**를 클릭한 후 **베타 시도**를 선택하십시오.
1.  **엔티티** 탭에서 **시스템 엔티티**를 클릭합니다.
1.  대화 상자에서 사용하려는 시스템 엔티티를 켜십시오. 

대화 노드 조건 또는 대화 노드의 조건부 응답 조건에 이들 중 하나 이상을 추가하여 새 시스템 엔티티를 테스트하십시오. 그런 다음, "시험 사용" 분할창에서 추가한 노드를 트리거하는 사용자 발화를 제출하십시오.

시스템 엔티티의 이전 버전 동작을 선호하는 경우 언제든지 베타 버전 사용을 중지할 수 있습니다. **옵션** 탭의 **시스템 엔티티** 페이지로 돌아가서 **기존 유지**를 선택하십시오. Watson이 다시 교육될 시간을 주십시오. 

베타 버전을 계속 사용하기로 결정한 경우 시스템 엔티티 값을 조건화하거나 반환하는 기존 대화 노드를 검토하여 변경할 필요가 있는지 확인하십시오. 예를 들어, 일부 멘션은 이전에 둘 이상의 시스템 엔티티 유형으로 분류되었습니다. 이제 예를 들어 통화가 멘션되면 `@sys-currency`로만 분류됩니다. 이전에 동작에 대해 작업하기 위해 사용한 로직을 단순화할 수 있습니다. 또는 이전 동작에 의존하여 추가한 로직을 수정해야 할 수도 있습니다. 

### 새 시스템 엔티티 특성
{: #beta-system-entities-props}

시스템 엔티티를 개선하기 위해, 숫자 기반 시스템 엔티티의 엔티티 오브젝트에 새 특성이 추가되었습니다.

다음 표에는 추가된 새 특성이 요약되어 있습니다. 현재 시스템 엔티티와 연관된 특성을 보려면 [시스템 엔티티 세부사항](/docs/services/assistant?topic=assistant-system-entities)을 확인하십시오. 

표 1. 새 시스템 엔티티 특성

| 시스템 엔티티 | 특성 | 설명 |
|---------------|----------|-------------|
| `@sys-date` | `alternatives` | 날짜가 명확하게 표시되지 않을 때 사용자가 의미했을 수 있는 `@sys-date` 값으로 저장된 날짜 이외의 날짜를 캡처합니다. 예를 들어 오늘이 2019년 3월 10일이고 사용자가 `on March 1`을 입력하면 내년 3월 1일이 `@sys-date`(`2020-03-01`)로 저장됩니다. 그러나 사용자는 2019년 3월 1일을 의미했을 수 있습니다. 시스템은 또한 올해의 날짜(`2019-03-01`)를 대체 날짜 값으로 저장합니다. 대체 값은 각 대체 날짜의 값 및 신뢰도를 포함하고 JSON 배열에 저장되는 오브젝트로 저장됩니다. 대체 값을 가져오려면 `@sys-date.alternative` 구문을 사용하십시오. 대부분의 경우 미래의 날짜는 `@sys-date` 값으로 저장되고 과거 날짜는 대체 값으로 저장됩니다. 이는 요일이 `on`과 같은 전치사나 `this` 또는 `next`와 같은 형용사 없이 지정될 때 적용됩니다. 예를 들어 `Are you open Monday?`라고 할 수 있습니다. 하지만 사용자가 `on Monday`나 `next Monday`와 같은 구문의 일부로 요일을 지정하면 날짜는 미래 날짜로 간주되고 대체 날짜가 작성되지 않습니다. 예를 들어 오늘이 화요일이고 사용자가 `this Tuesday` 또는 `next Tuesday`라고 입력하면 `@sys-date`가 다음 주 화요일 날짜로 설정되고 대체 날짜가 작성되지 않습니다. |
| `@sys-date` | `datetime_link` | 이는 사용자의 입력이 날짜와 시간을 함께 멘션함을 나타내며 날짜와 시간이 서로 관련되어 있다는 의미입니다. 날짜 및 시간을 포함하는 문자열의 시작 및 종료 인덱스 값은 링크 이름의 일부로 저장됩니다. 예를 들어 입력이 `Are you open today at 5?`이면 `today`는 `[13,18]` 위치에서 날짜 참조로 인식되고 `at 5`는 `[19,23]` 위치에서 시간 참조로 인식됩니다. 날짜 및 시간 멘션 둘 다를 포괄하는 `[13,23]` 위치 값이 결과적으로 datetime_link에 추가됩니다. 이름이 `datetime_link_13_23`으로 지정되고 `@sys-date` 및 관계에 참여하는 `@sys-time` 시스템 엔티티의 출력에 포함됩니다. |
| `@sys-date` | `festival` | 로케일 특정 휴일 이름을 인식하고 `Thanksgiving`, `Christmas` 및 `Halloween`과 같이 다가오는 휴일의 날짜를 리턴합니다. 조건에 휴일 이름을 지정해야 하는 경우 모두 소문자를 사용하십시오(예: `@sys-date.festival == 'thanksgiving'`). |
| `@sys-date` | `granularity` | 시간 범위 멘션(예: `next year` (=`year`))을 인식합니다. 옵션으로는 `day`, `weekend`, `week`, `fortnight`, `month`, `quarter` 및 `year`가 있습니다. |
| `@sys-date` | `interpretation` | 시스템 엔티티 클래스류의 정밀도를 향상시키는 새 필드를 포함하는 어시스턴트에서 리턴하는 오브젝트입니다. 필드를 참조하는 데 사용하는 표현식에서 `interpretation`을 생략할 수 있습니다. 예를 들어 약칭 구문 `<? @sys-date.festival ?>`를 사용하여 해석 오브젝트의 `festival` 필드 값에 액세스할 수 있습니다. |
| `@sys-date` | `range_link` | 이는 사용자의 입력에 날짜 범위가 지정되었음을 나타내는 구문이 포함되어 있음을 의미합니다. 예를 들어 입력은 `I will travel from March 15 to March 22`일 수 있습니다. 발견된 두 `@sys-date` 시스템 엔티티 각각은 출력에 `range_link` 특성을 가집니다. 범위 관계에서 각 `@sys-date`가 하는 역할을 포함하는 추가 정보가 제공됩니다. 예를 들어 시작 날짜는 `date_from`의 역할 유형을, 종료 날짜는 `date_to` 유형의 역할을 갖습니다. 역할 값을 확인하기 위해서 `@sys-date.role?.type == 'date_from'` 구문을 사용할 수 있습니다. 사용자 입력이 범위를 포함하지만 한 개의 날짜만 지정되는 경우 `range_link` 특성은 리턴되지 않지만 한 역할 유형은 리턴됩니다. 예를 들어 사용자가 `Have you been open since yesterday`라고 묻습니다. Yesterday의 날짜는 `@sys-date` 멘션으로 인식되고 `date_from` 유형 역할이 리턴됩니다. 범위 식별을 트리거하는 입력의 단어를 식별하는 `range_modifier`도 리턴됩니다. 이 예에서 수정자는 `since`입니다. |
| `@sys-date` | `relative_year`, `relative_month`, `relative_week`, `relative_weekend`, `relative_day` | 사용자 입력의 상대적 날짜 값 멘션(예: `two weeks ago` (`relative_week = -2`) 또는 `this weekend (relative_weekend = 0)` 또는 `today` (`relative_day = 0`))을 인식하고 캡처합니다. |
| `@sys-date` | `specific_year`, `specific_quarter`, `specific_month`, `specific_day`, `specific_day_of_week` | 사용자 입력의 특정 날짜 값 멘션(예:(`2020` (`specific_year = 2020`), `Q2` (`specific_quarter = 2`), `March`  (`specific_month = 3`), `Monday` (`specific_day_of_week = monday`) 또는 `3rd` (`specific_day = 3`))을 인식하고 캡처합니다. |
| `@sys-number` | `interpretation` | 시스템 엔티티 클래스류의 정밀도를 향상시키는 새 필드를 포함하는 어시스턴트에서 리턴하는 오브젝트입니다. 필드를 참조하는 데 사용하는 표현식에서 `interpretation`을 생략할 수 있습니다. 예를 들어 약칭 구문 `<? @sys-date.range_link ?>`를 사용하여 해석 오브젝트의 `range_link` 필드 값에 액세스할 수 있습니다. |
| `@sys-number` | `range_link` | 이는 사용자의 입력에 숫자 범위가 지정되었음을 나타내는 구문이 포함되어 있음을 의미합니다. 예를 들어 입력은 `I'm interested in 5to 7.`일 수 있습니다. 발견된 두 `@sys-number` 시스템 엔티티 각각은 출력에 `range_link` 특성을 가집니다. 범위 관계에서 각 `@sys-number`가 하는 역할을 포함하는 추가 정보가 제공됩니다. 예를 들어 시작 숫자는 `number_from`의 역할 유형을, 종료 숫자는 `number_to` 유형의 역할을 갖습니다. 역할 값을 확인하기 위해서 `@sys-number.role?.type == 'number_from'` 구문을 사용할 수 있습니다. |
| `@sys-time` | `alternatives` | 시간이 명확하게 표시되지 않을 때 사용자가 의미했을 수 있는 `@sys-time` 값으로 저장된 시간 이외의 시간을 캡처합니다. 예를 들어 사용자가 `The meeting is at 5`라고 말하면 시스템은 시간을 5:00:00 또는 오전 5시로 계산하고 `@sys-time`으로 저장합니다. 그러나 사용자는 17:00:00 또는 오후 5시를 의미했을 수도 있습니다. 시스템은 또한 나중 시간을 대체 시간 값으로 저장합니다. 사용자가 `The meeting is at 5pm`을 입력하면 `@sys-time`이 17:00:00으로 설정되고 오전과 오후의 혼동이 없으므로 대체 값이 작성되지 않습니다. 대체 값은 각 대체 시간의 값 및 신뢰도를 포함하고 JSON 배열에 저장되는 오브젝트로 저장됩니다. 대체 값을 가져오려면 `@sys-time.alternative` 구문을 사용하십시오. |
| `sys-time` | `datetime_link` | 이는 사용자의 입력이 날짜와 시간을 함께 멘션함을 나타내며 날짜와 시간이 서로 관련되어 있다는 의미입니다. 날짜 및 시간을 포함하는 문자열의 시작 및 종료 인덱스 값은 링크 이름의 일부로 저장됩니다. 예를 들어 입력이 `Are you open today at 5?`이면 `today`는 `[13,18]` 위치에서 날짜 참조로 인식되고 `at 5`는 `[19,23]` 위치에서 시간 참조로 인식됩니다. 날짜 및 시간 멘션 둘 다를 포괄하는 `[13,23]` 위치 값이 결과적으로 datetime_link에 추가됩니다. 이름이 `datetime_link_13_23`으로 지정되고 `@sys-date` 및 관계에 참여하는 `@sys-time` 시스템 엔티티의 출력에 포함됩니다. |
| `@sys-time` | `granularity` | 시간 범위의 멘션(예: `now` (=`instant`) 또는 `3 o'clock` 또는 `noon` (=`hour`), 또는 `17:00:00` (=`second`))을 인식합니다. 옵션은 `hour`, `minute`, `second` 및 `instant`입니다. |
| `@sys-time` | `interpretation` | 시스템 엔티티 클래스류의 정밀도를 향상시키는 새 필드를 포함하는 어시스턴트에서 리턴하는 오브젝트입니다. 필드를 참조하는 데 사용하는 표현식에서 `interpretation`을 생략할 수 있습니다. 예를 들어 약칭 구문 `<? @sys-time.granularity ?>`를 사용하여 해석 오브젝트의 `granularity` 필드 값에 액세스할 수 있습니다. |
| `@sys-time` | `part_of_day` | 하루 중 시간을 나타내는 용어(예: `morning`, `afternoon`, `evening`, `night`, 또는 `now`)를 인식합니다. 또한 아침의 경우 `9:00:00`,오후의 경우 `15:00:00`, 저녁의 경우 `18:00:00`, 밤의 경우 `22:00:00`와 같이 임의 시간을 설정합니다. |
| `@sys-time` | `range_link` | 이는 사용자의 입력에 시간 범위가 지정되었음을 나타내는 구문이 포함되어 있음을 의미합니다. 예를 들어 입력은 `Are you open from 9AMto 11AM`일 수 있습니다. 발견된 두 `@sys-time` 시스템 엔티티 각각은 출력에 `range_link` 특성을 가집니다. 범위 관계에서 각 `@sys-time`이 하는 역할을 포함하는 추가 정보가 제공됩니다. 예를 들어 시작 시간은 `time_from`의 역할 유형을, 종료 시간은 `time_to` 유형의 역할을 갖습니다. 역할 값을 확인하기 위해서 `@sys-time.role?.type == 'time_from'` 구문을 사용할 수 있습니다. 사용자 입력이 범위를 포함하지만 한 개의 시간만 지정되는 경우 `range_link` 특성은 리턴되지 않지만 한 역할 유형은 리턴됩니다. 예를 들어 사용자가 `Are you open until 9PM`이라고 물으면 9PM은 `@sys-time` 멘션으로 인식되고 `time_to` 유형 역할이 리턴됩니다. 범위 식별을 트리거하는 입력의 단어를 식별하는 `range_modifier`도 리턴됩니다. 이 예에서 수정자는 `until`입니다. |
| `@sys-time` | `relative_hour`, `relative_minute`, `relative_second` | 상대적 시간의 멘션(예: `5 hours ago` (`relative_hour = -5`),`in two minutes`(`relative_minute = 2`), 또는 `in a second` (`relative_second = 1`))을 인식합니다. |
| `@sys-time` | `specific_hour`, `specific_minute`, `specific_second` | 시간의 구체적 멘션(`at 5 o'clock` (`specific_hour = 5`), `at 2:30`(`specific_minute = 30`), or `23:30:22` (`specific_second = 22`))을 인식합니다. |
{: caption="새 시스템 엔티티 특성" caption-side="top"}

다음 표의 값은 엔티티 오브젝트의 특성이 아닙니다. 새 시스템 엔티티에서 날짜 또는 시간 세부사항을 신속하게 추출하기 위해 제품에서 사용할 수 있는 헬퍼 값입니다. 

표 2. 시스템 엔티티 헬퍼 함수

| 시스템 엔티티 | 헬퍼 값 | 설명 |
|---------------|----------|-------------|
| `@sys-date` | `day` | 날짜에 지정된 일을 숫자 값으로 리턴합니다. 예를 들어 날짜가 `5 December 2019`이면 `5`를 리턴합니다. |
| `@sys-date` | `day_of_week` | 요일을 판별하기 위해 날짜를 평가합니다. 요일을 소문자로 저장합니다. 예를 들어 `Sunday`는 `sunday`로 저장됩니다. 조건의 평일 이름을 확인하려면 소문자 첫자를 사용하십시오. 예: `@sys-date.day_of_week == 'sunday'` |
| `@sys-date` | `month` | 날짜에 지정된 월을 숫자 값으로 리턴합니다. 예를 들어 날짜가 `5 December 2019`이면 `12`를 리턴합니다. |
| `@sys-date` | `year` | 날짜에 지정된 연도를 숫자 값으로 리턴합니다. 예를 들어 날짜가 `5 December 2019`이면 `2019`를 리턴합니다. |
| `@sys-time` | `hour` | 날짜에 지정된 시간을 숫자 값으로 리턴합니다. 예를 들어 시간이 `5:30:10 PM`이면 오후 5시를 표시하기 위해 `17`을 리턴합니다. |
| `@sys-time` | `minute` |시간에 지정된 분 값을 리턴합니다. `30`. 분을 지정하지 않으면 헬퍼가 `0`을 리턴합니다.|
| `@sys-time` | `second` |시간에 지정된 초 값을 리턴합니다. `10`. 초 값을 지정하지 않으면 헬퍼가 `0`을 리턴합니다.|
{: caption="시스템 엔티티 헬퍼 함수" caption-side="top"}

### 사용법 예제
{: #beta-system-entities-examples}

향상된 시스템 엔티티에서 새 특성을 활용하기 위해 다음 유형의 표현식을 사용할 수 있습니다. 이 표는 사용자가 날짜로 `4 July 2019`와 시간으로 `3:30 PM`을 멘션할 때 리턴되는 결과를 표시합니다. You can use these expressions in dialog text responses 이러한 표현식을 `<? ?>` 구문으로 둘러싸지 않고 대화 텍스트 응답에서 사용할 수 있습니다. 

표 3. 새 시스템 엔티티를 사용하여 날짜 및 시간 세부사항을 가져오기

| SpEL 표현식 구문 | 결과 |
|------------------------|---------|
| `@sys-date.year` | `2019` |
| `@sys-date.month` | `7` |
| `@sys-date.day` | `4` |
| `@sys-date.day_of_week` | `thursday` |
| `@sys-time.hour` | `15` |
| `@sys-time.minute` | `30` |
| `@sys-time.second` | `0` |
{: caption="새 시스템 엔티티를 사용하여 데이터 및 시간 세부사항 가져오기" caption-side="top"}

`#Customer_Care_Store_Hours` 인텐트를 조건으로 하는 노드에서 새 시스템 엔티티 특성을 사용하는 조건부 응답을 추가하여 사용자가 요청하는 내용에 따라 상점 시간에 대한 약간 다른 응답을 제공할 수 있습니다.  

실제 대화에서는 이러한 조건부 응답을 모두 사용하고 싶지 않을 수 있습니다. 여기서는 가능한 것을 설명하기 위해 설명되어 있습니다.
{: tip}

표 4. 조건부 응답에서 새 시스템 엔티티 사용

| 조건부 응답 조건 구문 | 설명 | 예제 응답 텍스트 |
|--------------------------------|-------------|----------|
| `@sys-date.festival == 'thanksgiving'` | 고객이 특별히 추수감사절의 시간을 묻고 있는지 확인합니다. | 우리는 추수 감사절에 필요한 사람들에게 무료 식사를 제공합니다. |
| `@sys-date.festival` | 고객이 다른 특정 휴일의 시간을 묻고 있는지 확인합니다. | 우리는 크리스마스 당일, 7월 4일, 대통령의 날에 문을 닫습니다. 다른 휴일에는 오전 10시부터 오후 5시까지 문을 엽니다. |
| `@sys-time.part_of_day == 'night'` | 사용자가 입력에서 하루 중 시간으로 밤을 멘션하는 용어를 포함하는지 여부를 확인합니다. | 우리는 늦게까지 열지 않습니다. 대부분 오후 9시에 닫습니다. |
| `@sys-date.datetime_link && input.text.contains('today') && now().sameOrAfter(@sys-time)` | 입력에 `today at 8`과 같은 구문이 포함되어 있는지 확인합니다. 또한 발견된 `@sys-time`이 현재 시간보다 이전인지도 확인합니다.  `$new_time` 변수를 `<? @sys-time.plusHours(12) ?>` 값과 함께 작성하는 조건부 응답에 컨텍스트 변수를 추가할 수 있습니다. 이 방법을 사용하여 정오에 `Are you open today at 8`라고 묻는 사용자가 의미했을 가능성이 큰 시간을 캡처하는 날짜 변수를 작성할 수 있습니다. 시스템은 사용자가 오후 8시가 아니라 오전 8시를 의미했다고 가정합니다. | You mean today at $new_time, right? |
| `@sys-time.range_link && entities[0].role?.type == 'time_from' && entities[1].role?.type == 'time_to'` | 입력에 시간 범위가 포함되어 있는지 확인합니다. 또한 조건은 엔티티 배열에 나열된 첫 번째 `@sys - time` 시스템 엔티티가 시작 시간이고 두 번째는 종료 시간이라는 것을 응답에 포함하기 전에 확인합니다. | You want to know if we're open between `<? entities[0] ?>` and `<? entities[1] ?>`, is that right? |
| `@sys-time && entities.size() < 2 && entities[0].role?.type == 'time_to'` | 입력이 `time_to` 역할 유형이 있는 하나의 `@sys-time` 멘션을 포함하는지 여부를 확인합니다. 사용자 입력이 이 조건과 일치하면 사용자가 개방형 시간 범위의 종료 시간을 지정했음을 의미합니다. 예를 들어 응답은 `Are you open until 9pm?` 입력으로 트리거됩니다. 이 입력은 시간 범위에서 하나의 시간만 식별하고 멘션에 `time_to` 역할이 있으므로 일치합니다. | Do you mean from now (`<? now().reformatDateTime('h:mm a') ?>`) until `<? @sys-time.reformatDateTime('h:mm a') ?>`? |
| `@sys-date.day_of_week == 'sunday'` | 사용자가 요청하는 특정 날짜가 일요일인지 여부를 확인합니다. | 우리는 일요일에는 문을 닫습니다. |
| `@sys-date.specific_day_of_week == 'monday' && @sys-date.alternative` | 사용자가 조회에서 `Monday` 요일을 언급했는지 여부를 확인합니다. 예를 들어 `Are you open Monday`일 수 있습니다. 조건은 또한 대체 날짜가 저장되었는지를 확인합니다. 대체 날짜는 어시스턴트가 사용자가 어느 월요일을 의미하는지 전적으로 확신하지 못할 때 작성되므로 대체 월요일의 날짜들도 저장합니다. 사용자가 요일을 지정하면 어시스턴트는 사용자가 다가오는 요일을 의미한다고 가정합니다(다음 월요일). 발견된 대체 값을 사용하여 의도한 날짜를 두 번 확인하는 응답을 추가할 수 있습니다. 이 경우 대체 날짜는 이전 월요일의 날짜입니다. | Do you mean `@sys-date` or `@sys-date.alternative`? |
| `true` | 상점 시간 정보에 대한 기타 요청에 응답합니다. | We're open from 9AM to 9PM Monday through Saturday. |
{: caption="조건부 응답에서 새 시스템 엔티티 사용" caption-side="top"}
