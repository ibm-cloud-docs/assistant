---

copyright:
  years: 2015, 2019
lastupdated: "2019-08-02"

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

스킬은 어시스턴트가 고객을 지원할 수 있게 하는 인공 지능용 컨테이너입니다.
{: shortdesc}

어시스턴트는 요청이 고객 문제점을 해결하기 위한 최적의 경로를 따르도록 지시합니다. 스킬을 추가하여 일반적인 질문에 대한 직접 응답을 제공하거나 보다 복잡한 항목에 대해 더 일반화된 검색 결과를 참조할 수 있습니다.

## 스킬 유형
{: #skills-types}

어시스턴트에 다음 스킬 유형을 추가할 수 있습니다.

- **[대화 스킬](#skills-dialog-skill)**: 사용자로부터 일반적 질문이나 요청을 이해하고 사용자가 스크립트를 작성한 대화에 따라 답변하거나 수행합니다. 

![Plus 또는 Premium 플랜만 해당](images/plus.png) Plus 또는 Premium 플랜 사용자인 경우 다음 유형의 스킬도 작성할 수 있습니다. 

- **[검색 스킬](#skills-search-skill)**: 외부 데이터 소스로부터 관련 정보를 검색하고,구절을 추출하고, 이를 어시스턴트의 응답으로 리턴하여 사용자의 질문에 답변합니다. 

### 대화 스킬
{: #skills-dialog-skill}

대화 스킬(dialog skill)에는 어시스턴트가 고객을 지원할 수 있도록 하는 훈련 데이터와 로직이 포함되어 있습니다. 다음과 같은 유형의 아티팩트가 있습니다.

- [**인텐트(Intents)**](/docs/services/assistant?topic=assistant-intents): *인텐트*는 비즈니스 위치 또는 요금 지불에 대한 질문과 같은 사용자 입력의 목적을 나타냅니다. 애플리케이션에서 지원할 각 사용자 요청 유형에 대한 인텐트를 정의합니다. 인텐트 이름의 앞에는 항상 `#` 문자가 옵니다. 대화 스킬이 인텐트를 인식하는 훈련을 할 수 있도록 사용자 입력에 대한 많은 예제를 제공하고 이러한 예제가 맵핑되는 인텐트를 표시합니다.

  자체적으로 빌드하는 대신 애플리케이션에 추가할 수 있는 사전 빌드된 공통 인텐트를 포함하는 *컨텐츠 카탈로그*가 제공됩니다. 예를 들어, 대부분의 애플리케이션에는 사용자와의 대화를 시작하는 인사 인텐트가 필요합니다. **일반** 컨텐츠 카탈로그를 사용하여 사용자를 환영하고 대화 종료와 같은 다른 유용한 작업을 수행하는 인텐트를 추가할 수 있습니다.

- [**대화(Dialog)**](/docs/services/assistant?topic=assistant-dialog-build): *대화*는 정의된 인텐트 및 엔티티를 인식할 때 애플리케이션이 응답하는 방식을 정의하는 분기 대화 플로우입니다. 대화 편집기를 사용하여 사용자와의 대화를 작성하며, 사용자 입력에서 인식하는 인텐트와 엔티티에 따라 응답을 제공합니다.

  ![인텐트 및 대화만 사용하는 기본 구현의 다이어그램](images/basic-impl.png)

대화 스킬이 더 미묘한 질문을 처리할 수 있도록 하려면 엔티티를 정의하고 대화에서 이를 참조하십시오.

- [**엔티티(Entities)**](/docs/services/assistant?topic=assistant-entities): *엔티티*는 인텐트와 관련되거나 인텐트에 대한 구체적 컨텍스트를 제공하는 용어 또는 오브젝트를 나타냅니다. 예를 들어, 엔티티는 사용자가 비즈니스 위치를 찾으려는 도시 또는 요금 지불 금액을 나타낼 수 있습니다. 엔티티 이름의 앞에는 항상 `@` 문자가 옵니다.

  엔티티 용어 값 및 동의어와 엔티티 패턴을 제공하거나 일반적으로 엔티티가 문장에서 사용되는 컨텍스트를 식별하여 엔티티를 인식하는 스킬을 훈련할 수 있습니다. 대화를 세부적으로 조정하려면 되돌아가서 인텐트 이외에 사용자 입력의 엔티티 멘션을 확인하는 노드를 추가하십시오.

![인텐트, 엔티티 및 대화를 사용하는 더 복잡한 구현의 다이어그램](images/complex-impl.png)

정보를 추가하면 스킬이 이 고유 데이터를 사용하여 이러한 사용자 입력 및 유사한 사용자 입력을 인식할 수 있는 기계 학습 모델을 빌드합니다. 훈련 데이터를 추가하거나 변경할 때마다 훈련 프로세스가 트리거되어 고객 요구사항과 논의하려는 주제가 변경됨에 따라 기본 모델이 최신 상태로 유지될 수 있도록 합니다.

대화 스킬을 작성하는 데 도움을 받으려면 [대화 스킬 작성](/docs/services/assistant?topic=assistant-skill-dialog-add)을 참조하십시오.

### 검색 스킬 ![Plus 또는 Premium 플랜만 해당](images/plus.png)
{: #skills-search-skill}

Watson Assistant가 문제에 대한 명확한 솔루션을 갖고 있지 않는 경우, 사용자 질문을 검색 스킬로 라우팅하여 서로 다른 셀프 서비스 컨텐츠 소스에서 응답을 찾을 수 있습니다. 검색 스킬은 {{site.data.keyword.discoveryfull}} 서비스와 상호작용하여 구성된 데이터 콜렉션에서 이러한 정보를 추출합니다. 

{{site.data.keyword.discoveryshort}} 서비스를 이미 사용하는 경우, 고객과 공유할 수 있는 소스 자료에 대한 기존 데이터 콜렉션을 마이닝하여 해당 질문을 처리할 수 있습니다.

그러나 {{site.data.keyword.discoveryshort}} 서비스 인스턴스를 가질 필요는 없습니다. 검색 스킬을 작성하도록 선택한 경우, {{site.data.keyword.discoveryshort}}의 무료 인스턴스가 프로비저닝됩니다. 그런 다음 데이터 소스에서 콜렉션을 작성하고 이 콜렉션을 검색하기 위해 검색 스킬을 구성하여 고객의 조회에 대한 답변을 찾을 수 있습니다. 

다음 다이어그램은 대화 스킬 및 검색 스킬 둘 다가 어시스턴트에 추가될 때 사용자 입력이 처리되는 방법을 보여줍니다. 대화가 응답하도록 설계되지 않은 질문은 검색 스킬에 전송되며, 검색 스킬이 {{site.data.keyword.discoveryshort}} 데이터 콜렉션에서 관련 응답을 찾습니다. 

![질문이 검색 스킬에 라우트되는 방법을 보여주는 다이어그램](images/search-skill-diagram.png)

검색 스킬을 작성하는 데 도움을 받으려면 [검색 스킬 작성](/docs/services/assistant?topic=assistant-skill-search-add)을 참조하십시오.
