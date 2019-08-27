---

copyright:
  years: 2015, 2019
lastupdated: "2019-07-16"

subcollection: assistant


---

{:curl: #curl .ph data-hd-programlang='curl'}
{:javascript: #javascript .ph data-hd-programlang='javascript'}
{:java: #java .ph data-hd-programlang='java'}
{:python: #python .ph data-hd-programlang='python'}
{:swift: data-hd-programlang='swift'}
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:codeblock: .codeblock}
{:download: .download}
{:deprecated: .deprecated}
{:important: .important}
{:note: .note}
{:tip: .tip}

# {{site.data.keyword.conversationshort}} API 개요
{: #api-overview}

{{site.data.keyword.conversationshort}} REST API 및 해당 SDK를 사용하여 서비스와 상호작용하는 애플리케이션을 개발할 수 있습니다.

## 클라이언트 애플리케이션

런타임 시 어시스턴트와 통신하는 가상 어시스턴트 또는 기타 클라이언트 애플리케이션을 빌드하려면 새 v2 API를 사용하십시오. 이 API를 사용하여 프로덕션용으로 배치할 수 있는 사용자 측 클라이언트, 어시스턴트와 다른 서비스(예: 대화 서비스 또는 백엔드 시스템) 간의 통신을 중개하는 애플리케이션 또는 테스트 애플리케이션을 개발할 수 있습니다.

어시스턴트와 통신하기 위해 v2 런타임 API를 사용하면 애플리케이션이 다음과 같은 기능을 활용할 수 있습니다.

- **자동 상태 관리.** v2 런타임 API는 일반 사용자와의 각 세션을 관리하고, 완벽한 대화를 위해 어시스턴트가 필요로 하는 모든 컨텍스트 데이터를 저장 및 유지보수합니다.

- **어시스턴트를 사용한 용이한 배치.** 사용자 정의 클라이언트를 지원하는 것 외에도, Slack 및 Facebook Messenger와 같은 유명한 메시징 채널에 어시스턴트를 쉽게 배치할 수 있다.

- **버전화.** 대화 스킬 버전화를 사용하면 스킬의 스냅샷을 저장하고 해당 특정 버전에 대한 어시스턴트를 링크할 수 있습니다. 그런 다음 프로덕션 어시스턴트에 영향을 주지 않고 개발 버전을 계속 업데이트할 수 있습니다.

- **검색 기능.** v2 런타임 API는 대화 스킬 및 검색 스킬 모두에서 응답을 수신하는 데 사용될 수 있습니다. 대화 스킬이 응답할 수 없는 조회가 제출되면, 어시스턴트는 검색 스킬을 사용하여 구성된 데이터 소스에서 최상의 응답을 찾을 수 있습니다. (검색 스킬은 Plus 또는 Premium 플랜 사용자만 사용할 수 있는 베타 기능입니다. )

v2 API에 대한 세부사항은 {{site.data.keyword.conversationshort}} [v2 API 참조 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://{DomainName}/apidocs/assistant-v2){: new_window}를 참조하십시오.

**참고**: {{site.data.keyword.conversationshort}} v1 API는 여전히 대화 스킬에서 사용하는 작업공간에 사용자 입력을 직접 전송하는 기존의 `/message` 메소드를 지원합니다. v1 런타임 API는 주로 이전 버전과의 호환성을 위해 지원됩니다. v1 `/message` 메소드를 사용하는 경우 자체적인 상태 관리를 구현해야 하며 어시스턴트의 버전화 기능 또는 기타 기능을 이용할 수 없습니다. 

## 작성 애플리케이션

v1 API는 {{site.data.keyword.conversationshort}} 사용자 인터페이스를 사용하여 그래픽으로 스킬을 빌드하는 대신 애플리케이션이 대화 스킬을 작성하거나 수정할 수 있도록 하는 메소드를 제공합니다. 작성 애플리케이션은 API를 사용하여 스킬, 인텐트, 엔티티, 대화 노드 및 대화 스킬을 구성하는 기타 아티팩트를 작성하고 수정합니다. 자세한 정보는 [v1 API 참조 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://{DomainName}/apidocs/assistant){: new_window}를 참조하십시오.

  **참고:** v1 작성 메소드는 스킬이 아니라 작업공간을 작성 및 수정합니다. 작업공간은 대화 스킬 내의 대화 및 훈련 데이터(예: 인텐트 및 엔티티)에 대한 컨테이너입니다. API를 사용하여 새 작업공간을 작성하는 경우 {{site.data.keyword.conversationshort}} 사용자 인터페이스에서 새 대화 스킬로 표시됩니다.

사용 가능한 API 메소드의 목록은 [API 메소드 요약](/docs/services/assistant?topic=assistant-api-methods)을 참조하십시오.
