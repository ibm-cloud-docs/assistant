---

copyright:
  years: 2015, 2019
lastupdated: "2019-07-26"

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

# 어시스턴트
{: #assistants}

어시스턴트는 비즈니스 요구사항에 맞게 사용자 정의하고 언제 어디서나 필요할 때 고객에게 도움을 줄 수 있도록 여러 채널에서 배치할 수 있는 코그너티브 봇입니다.
{: shortdesc}

![스킬](images/skill-icon.png) 어시스턴트는 적절한 응답을 제공하는 스킬로 고객 조회를 라우팅합니다. 대화 스킬은 사용자가 일반적인 질문에 대답하기 위해 작성한 응답을 리턴하는 반면, 검색 스킬은 기존 셀프 서비스 컨텐츠의 구절을 검색하고 리턴하여 보다 복잡한 질문에 응답합니다. 

## 대화 스킬
{: #assistants-dialog-skill}

대화 스킬은 일반적으로 고객에게 도움이 필요한 질문 또는 요청을 이해하고 처리할 수 있습니다. 사용자가 질문하는 주제 또는 태스크와 질문 방식에 대한 정보를 제공합니다. 그러면 제품이 동일하고 유사한 사용자 요청을 이해하도록 사용자 조정된 기계 학습 모델을 동적으로 빌드합니다.

| 대화 트리 | 그래픽 사용자 인터페이스 |
|-------------|-------------------------:|
| 그래픽 대화 편집기를 사용하여 어시스턴트가 고객과 상호작용할 때 읽어야 하는 일종의 스크립트를 작성할 수 있습니다. 결과는 실제 대화를 시뮬레이션하는 대화입니다. 대화는 사용자가 인식하도록 가르치는 공통 고객 목표를 가져와서 유용한 응답을 제공합니다. | ![예제 컨텐츠가 있는 샘플 대화 트리](images/dialog-depiction.png) |

대화 스킬 자체는 텍스트에 정의되지만 사용자가 어시스턴트와 음성으로 상호작용할 수 있도록 하는 Watson Speech to Text 및 Watson Text to Speech 서비스와 통합할 수 있습니다.

![바로 사용할 수 있는 훈련 데이터](images/oob.png) 빠르게 시작하려면 어시스턴트가 고객에게 기본사항을 지원하기 시작할 수 있도록 사전 빌드된 훈련 데이터를 대화 스킬에 추가하십시오.

## 검색 스킬 ![Plus 또는 Premium 플랜만 해당](images/plus.png)
{: #assistants-search-skill}

검색 스킬은 Plus 또는 Premium 사용자만 사용할 수 있습니다.
{: note}

검색 스킬은 기존의 회사 지식 기반 데이터베이스 또는 주제별 전문가가 작성한 기타 컨텐츠 콜렉션을 활용하여 예기치 않거나 좀더 미묘한 고객 조회를 처리합니다. 

![IBM Cloud](images/cloud.png) 어시스턴트는 {{site.data.keyword.Bluemix_notm}}에서 관리하는 완전히 호스팅되는 봇입니다. 즉, 이를 지원하도록 인프라를 설정하거나 유지보수할 필요가 없습니다.

| 통합       | 채널  |
|--------------------|:----------|
| 몇 가지 단계만으로 Slack 및 Facebook Messenger와 같은 기존 메시징 채널을 포함하여 여러 인터페이스를 통해 어시스턴트를 배치할 수 있습니다. 또는 이를 통합하는 사용자 정의 애플리케이션을 디자인하려는 경우 기본 API를 직접 호출하여 이를 수행할 수 있습니다. | ![Slack, Facebook Messenger, 웹 애플리케이션 또는 휴먼 에이전트 통합을 포함한 통합 방법](images/integrations.png) |

시작하려면 [어시스턴트 작성](/docs/services/assistant?topic=assistant-assistant-add)을 참조하십시오.
