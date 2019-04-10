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

# 스킬
{: #skills}

대화 스킬(dialog skill)에는 어시스턴트가 고객을 지원할 수 있도록 하는 훈련 데이터와 로직이 포함되어 있습니다.
{: shortdesc}

대화 스킬에는 다음과 같은 유형의 아티팩트가 있습니다.

- [**인텐트(Intents)**](/docs/services/assistant?topic=assistant-intents): *인텐트*는 비즈니스 위치 또는 요금 지불에 대한 질문과 같은 사용자 입력의 목적을 나타냅니다. 애플리케이션에서 지원할 각 사용자 요청 유형에 대한 인텐트를 정의합니다. 도구에서 인텐트 이름의 앞에는 항상 `#` 문자가 옵니다. 대화 스킬이 인텐트를 인식하는 훈련을 할 수 있도록 사용자 입력에 대한 많은 예제를 제공하고 이러한 예제가 맵핑되는 인텐트를 표시합니다.

  자체적으로 빌드하는 대신 애플리케이션에 추가할 수 있는 사전 빌드된 공통 인텐트를 포함하는 *컨텐츠 카탈로그*가 제공됩니다. 예를 들어, 대부분의 애플리케이션에는 사용자와의 대화를 시작하는 인사 인텐트가 필요합니다. **일반** 컨텐츠 카탈로그를 사용하여 사용자를 환영하고 대화 종료와 같은 다른 유용한 작업을 수행하는 인텐트를 추가할 수 있습니다.

- [**대화(Dialog)**](/docs/services/assistant?topic=assistant-dialog-build): *대화*는 정의된 인텐트 및 엔티티를 인식할 때 애플리케이션이 응답하는 방식을 정의하는 분기 대화 플로우입니다. 도구에서 대화 편집기를 사용하여 사용자와의 대화를 작성하며, 사용자 입력에서 인식하는 인텐트와 엔티티에 따라 응답을 제공합니다.

  ![인텐트 및 대화만 사용하는 기본 구현의 다이어그램](images/basic-impl.png)

대화 스킬이 더 미묘한 질문을 처리할 수 있도록 하려면 엔티티를 정의하고 대화에서 이를 참조하십시오.

- [**엔티티(Entities)**](/docs/services/assistant?topic=assistant-entities): *엔티티*는 인텐트와 관련되거나 인텐트에 대한 구체적 컨텍스트를 제공하는 용어 또는 오브젝트를 나타냅니다. 예를 들어, 엔티티는 사용자가 비즈니스 위치를 찾으려는 도시 또는 요금 지불 금액을 나타낼 수 있습니다. 도구에서 엔티티 이름의 앞에는 항상 `@` 문자가 옵니다.

  엔티티 용어 값 및 동의어와 엔티티 패턴을 제공하거나 일반적으로 엔티티가 문장에서 사용되는 컨텍스트를 식별하여 엔티티를 인식하는 스킬을 훈련할 수 있습니다. 대화를 세부적으로 조정하려면 되돌아가서 인텐트 이외에 사용자 입력의 엔티티 멘션을 확인하는 노드를 추가하십시오.

![인텐트, 엔티티 및 대화를 사용하는 더 복잡한 구현의 다이어그램](images/complex-impl.png)

정보를 추가하면 스킬이 이 고유 데이터를 사용하여 이러한 사용자 입력 및 유사한 사용자 입력을 인식할 수 있는 기계 학습 모델을 빌드합니다. 훈련 데이터를 추가하거나 변경할 때마다 훈련 프로세스가 트리거되어 고객 요구사항과 논의하려는 주제가 변경됨에 따라 기본 모델이 최신 상태로 유지될 수 있도록 합니다.

대화 스킬을 작성하는 데 도움을 받으려면 [스킬 작성](/docs/services/assistant?topic=assistant-skill-add)을 참조하십시오.
