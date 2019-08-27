---

copyright:
  years: 2015, 2019
lastupdated: "2019-08-06"

subcollection: assistant

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:external: target="_blank" .external}
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

# 정보
{: #index}

{{site.data.keyword.conversationfull}}를 사용하여 자체 브랜드의 어시스턴트를 디바이스, 애플리케이션 또는 채널에 빌드하십시오. 어시스턴트는 이미 사용하고 있는 고객 참여 리소스에 연결하여 고객에게 매력적인 통합 문제 해결 경험을 제공합니다.
{: shortdesc}

## 작동 방법
{: #index-how-it-works}

이 다이어그램에서는 제품 작동 방법을 보여줍니다.

![서비스 플로우 다이어그램](images/simple-overview.png)

- 사용자는 다음 **통합** 지점 중 하나 이상을 통해 어시스턴트와 상호작용합니다.

  - Slack 또는 Facebook Messenger와 같은 기존 소셜 미디어 메시징 플랫폼에 직접 공개하는 가상 어시스턴트
  - 사용자가 개발하는 사용자 정의 애플리케이션(예: 음성 인터페이스가 있는 로봇 또는 모바일 앱)

- **어시스턴트**가 사용자 입력을 수신하여 대화 스킬에 라우팅합니다.

- **대화 스킬**이 사용자 입력을 추가로 해석한 후 대화의 플로우를 지시합니다. 대화는 응답하거나 사용자 대신 트랜잭션을 수행하는 데 필요한 정보를 수집합니다.

- 대화 스킬을 사용하여 응답할 수 없는 질문은 해당 용도로 구성한 회사 지식 기반 데이터베이스를 검색하여 관련 응답을 찾는 **검색 스킬**에 전송됩니다. 

## 구현
{: #index-implementation}

이 다이어그램은 구현을 보다 자세하게 표시합니다.

![서비스 플로우 다이어그램](images/arch-overview-search.png)

어시스턴트 구현 방법은 다음과 같습니다.

- **어시스턴트를 작성하십시오.**

- **어시스턴트에 스킬을 추가하십시오.**

  서비스 플랜에 따라 다음 유형의 스킬을 추가할 수 있습니다. 

  - **대화 스킬을 추가하십시오.**  
  
    직관적인 그래픽 제품을 사용하여 어시스턴트와 고객 간의 대화를 위한 훈련 데이터 및 대화를 정의하십시오.훈련 데이터는 다음 아티팩트로 구성됩니다.

    - **인텐트(Intents)**: 사용자가 어시스턴트와 상호작용할 때 갖는 목적입니다. 사용자 입력에서 식별할 수 있는 목적마다 하나의 인텐트를 정의하십시오. 예를 들어 상점 시간에 대한 질문에 응답하는 *store_hours*라는 인텐트를 정의할 수 있습니다. 각 인텐트에 대해 고객이 필요한 정보(예: `What time do you open?` aaaa)를 묻는 데 사용할 수 있는 입력을 반영하는 샘플 발화(utterance)를 추가합니다.

      또는 IBM에서 제공하는 사전 빌드된 **컨텐츠 카탈로그**를 사용하여 공통 고객 목표를 처리하는 데이터로 시작하십시오.

    - **대화(Dialog)**: 대화 편집기를 사용하여 인텐트를 통합하는 대화 플로우를 빌드하십시오. 대화 플로우는 그래픽으로 트리로 표시됩니다. 분기를 추가하여 어시스턴트에서 처리할 각 인텐트를 처리할 수 있습니다.

    - **엔티티(Entities)**: 엔티티는 인텐트의 컨텍스트를 제공하는 용어나 오브젝트를 나타냅니다. 예를 들어, 엔티티는 대화에서 사용자가 운영 시간을 알고 싶어하는 상점을 구분하는 데 도움이 되는 도시 이름이 될 수 있습니다. 엔티티를 추가한 후 이를 사용하도록 대화를 업데이트하십시오. 사용자 입력에서 발견된 엔티티를 기반으로 가능한 많은 요청의 순열을 처리하는 대화 노드를 추가하십시오.

    훈련 데이터를 추가하면 자연어 클래스류가 자동으로 스킬에 추가됩니다. 클래스류 모델은 어시스턴트가 청취하고 응답할 수 있도록 설계하는 요청 유형을 이해하도록 훈련됩니다.

  - **검색 스킬을 추가하십시오.** ![Plus 또는 Premium 플랜만 해당](images/plus.png)

    {{site.data.keyword.discoveryfull}}에서 사용자가 작성한 데이터 콜렉션을 활용하여 고객의 질문에 대한 응답을 제공합니다. 고객이 대화가 응답하도록 설계되지 않은 질문을 하면 어시스턴트는 구성된 데이터 소스에서 관련 정보를 검색하고 정보를 추출하여 이를 어시스턴트의 응답으로 리턴할 수 있습니다.

- **어시스턴트를 통합하십시오.** 기본 제공 채널 통합을 추가하여 구성된 어시스턴트를 소셜 미디어 또는 메시징 채널에 직접 배치하십시오. 또는 어시스턴트에 대한 사용자 인터페이스로 자체적인 클라이언트 애플리케이션을 빌드하십시오. 

  배치된 어시스턴트는 IBM의 클라우드 컴퓨팅 플랫폼인 {{site.data.keyword.cloud}}에서 호스팅됩니다. (자세한 정보는 [플랫폼 개요](/docs/overview/ibm-cloud#overview){: external}를 참조하십시오.)

이 링크를 따라 해당 구현 단계에 대해 자세히 읽어보십시오.

- [어시스턴트 개요](/docs/services/assistant?topic=assistant-assistants)
- [검색 스킬 개요](/docs/services/assistant?topic=assistant-skill-add-search)
- [인텐트 작성 개요](/docs/services/assistant?topic=assistant-intents#intents-described)
- [대화 개요](/docs/services/assistant?topic=assistant-dialog-overview)
- [엔티티 작성 개요](/docs/services/assistant?topic=assistant-entities#entities-described)
- [통합 추가](/docs/services/assistant?topic=assistant-deploy-integration-add)

## 브라우저 지원
{: #index-browser-support}

{{site.data.keyword.conversationshort}}에는 {{site.data.keyword.Bluemix_notm}}에 필요한 것과 동일한 레벨의 브라우저 소프트웨어가 필요합니다. 자세한 정보는 {{site.data.keyword.Bluemix_notm}} [필수 소프트웨어](/docs/overview?topic=overview-prereqs-platform#browsers-platform){: external}를 참조하십시오.

## 언어 지원
{: #index-lang-support}

기능을 통한 언어 지원은 [지원되는 언어](/docs/services/assistant?topic=assistant-language-support) 주제에 자세히 설명되어 있습니다.

## 이용 약관 및 주의사항
{: #index-notices}

서비스 이용 약관에 대한 정보는 [IBM Cloud 이용 약관 및 주의사항](/docs/overview/terms-of-use?topic=overview-terms){: external}을 참조하십시오.

US Health Insurance Portability and Accountability Act(HIPAA)는 2019년 4월 1일 이후 설립된 Washington, DC에서 호스팅되는 Premium 플랜에서 사용할 수 있습니다. 자세한 정보는 [Enabling EU 및 HIPAA 지원 설정 사용](/docs/account?topic=account-eu-hipaa-supported#eu-hipaa-supported){: external}을 참조하십시오. 

## 다음 단계
{: #index-next-steps}

- 제품을 [시작](/docs/services/assistant?topic=assistant-getting-started)하십시오.
- [개발자 리소스](https://www.ibm.com/watson/developer-resources/){: external} 목록을 보십시오.

질문이 있습니까? [IBM 영업](https://www-01.ibm.com/marketing/iwm/dre/signup?source=urx-20970){: external}에 문의하십시오.
