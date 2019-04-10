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

# 정보
{: #index}

{{site.data.keyword.conversationfull}}는 비즈니스 요구사항에 맞게 사용자 정의하고 언제 어디서나 필요할 때 고객에게 도움을 줄 수 있도록 여러 채널에서 배치할 수 있는 코그너티브 봇입니다.
{: shortdesc}

## 작동 방법
{: #index-how-it-works}

이 다이어그램은 전체 아키텍처를 표시합니다.

![서비스 플로우 다이어그램](images/arch-overview.png)

- 사용자는 다음 **통합** 지점 중 하나 이상을 통해 어시스턴트와 상호작용합니다.

  - Slack 또는 Facebook Messenger와 같은 기존 소셜 미디어 메시징 플랫폼에 직접 공개하는 챗봇
  - IBM Cloud에서 호스팅되는 단순 챗봇 사용자 인터페이스
  - 사용자가 개발하는 사용자 정의 애플리케이션(예: 음성 인터페이스가 있는 로봇 또는 모바일 앱)

- **어시스턴트**가 사용자 입력을 수신하여 대화 스킬에 라우팅합니다.

- 대화 **스킬**이 사용자 입력을 추가로 해석한 후 대화의 플로우를 지시하고 응답하거나 사용자 대신 트랜잭션을 수행하는 데 필요한 정보를 수집합니다.

## 구현
{: #index-mplementation}

어시스턴트 구현 방법은 다음과 같습니다.

- **대화 스킬을 작성하십시오.** 직관적인 그래픽 도구를 사용하여 어시스턴트와 고객 간의 대화를 위한 훈련 데이터 및 대화를 정의하십시오.

  훈련 데이터는 다음 아티팩트로 구성됩니다.

  - **인텐트(Intents)**: 사용자가 서비스와 상호작용할 때 갖는 목적입니다. 사용자 입력에서 식별할 수 있는 목적마다 하나의 인텐트를 정의하십시오. 예를 들어 상점 시간에 대한 질문에 응답하는 *store_hours*라는 인텐트를 정의할 수 있습니다. 각 인텐트에 대해 고객이 필요한 정보(예: `What time do you open?` aaaa)를 묻는 데 사용할 수 있는 입력을 반영하는 샘플 발화(utterance)를 추가합니다.

    또는 IBM에서 제공하는 사전 빌드된 **컨텐츠 카탈로그**를 사용하여 공통 고객 목표를 처리하는 데이터로 시작하십시오.

  - **대화(Dialog)**: 대화 도구를 사용하여 인텐트를 통합하는 대화 플로우를 빌드하십시오. 대화 플로우는 그래픽으로 도구에 트리로 표시됩니다. 분기를 추가하여 서비스에서 처리할 각 인텐트를 처리할 수 있습니다.

  - **엔티티(Entities)**: 엔티티는 인텐트의 컨텍스트를 제공하는 용어나 오브젝트를 나타냅니다. 예를 들어, 엔티티는 대화에서 사용자가 운영 시간을 알고 싶어하는 상점을 구분하는 데 도움이 되는 도시 이름이 될 수 있습니다. 엔티티를 추가한 후 이를 사용하도록 대화를 업데이트하십시오. 사용자 입력에서 발견된 엔티티를 기반으로 가능한 많은 요청의 순열을 처리하는 대화 노드를 추가하십시오.

    훈련 데이터를 추가하면 자연어 클래스류가 자동으로 스킬에 추가되며 서비스가 청취하고 응답해야 한다고 표시한 요청 유형을 이해하도록 훈련됩니다.

- **어시스턴트를 작성하십시오.**

- **어시스턴트에 대화 스킬을 추가하십시오.**

- **어시스턴트를 통합하십시오.** 구성된 어시스턴트를 소셜 미디어 또는 메시징 채널에 직접 배치하기 위한 채널 통합을 작성하십시오.

  배치된 어시스턴트는 IBM의 클라우드 컴퓨팅 플랫폼인 {{site.data.keyword.cloud_notm}}에서 호스팅됩니다. (자세한 정보는 [플랫폼 개요 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://cloud.ibm.com/docs/overview/ibm-cloud#overview)을 참조하십시오.)

이 링크를 따라 해당 구현 단계에 대해 자세히 읽어보십시오.

- [인텐트 작성 개요](/docs/services/assistant?topic=assistant-intents#intents-described)
- [대화 개요](/docs/services/assistant?topic=assistant-dialog-overview)
- [엔티티 작성 개요](/docs/services/assistant?topic=assistant-entities#entities-described)
- [어시스턴트 개요](/docs/services/assistant?topic=assistant-assistant-add)
- [통합 추가](/docs/services/assistant?topic=assistant-deploy-integration-add)

## 내 작업공간 위치
{: #index-existing-customers}

이전 버전의 서비스에서 *작업공간*을 작성한 경우 걱정하지 마십시오. 계속 사용할 수 있습니다. 이제 작업공간을 *스킬*이라고 합니다. 작업공간으로 이동하려면 **스킬** 탭을 클릭하십시오.

## 브라우저 지원
{: #index-browser-support}

{{site.data.keyword.conversationshort}} 서비스 도구에는 {{site.data.keyword.Bluemix_notm}}에 필요한 것과 동일한 레벨의 브라우저 소프트웨어가 필요합니다. 세부사항은 {{site.data.keyword.Bluemix_notm}} [필수 소프트웨어![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://cloud.ibm.com/docs/overview/prereqs#browsers){: new_window} 주제를 참조하십시오.

## 언어 지원
{: #index-lang-support}

기능을 통한 언어 지원은 [지원되는 언어](/docs/services/assistant?topic=assistant-language-support) 주제에 자세히 설명되어 있습니다.

## 이용 약관 및 주의사항
{: #index-notices}

서비스 이용 약관에 대한 정보는 [IBM Cloud 이용 약관 및 주의사항 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](/docs/overview/terms-of-use?topic=overview-terms)을 참조하십시오.

## 다음 단계
{: #index-next-steps}

- 서비스를 [시작](/docs/services/assistant?topic=assistant-getting-started)하십시오.
- [개발자 리소스 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.ibm.com/watson/developer-resources/){: new_window}를 보십시오.

아직 궁금하신 사항이 있습니까? [IBM Sales ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://www-01.ibm.com/marketing/iwm/dre/signup?source=urx-20970){: new_window}에 문의하시기 바랍니다.
