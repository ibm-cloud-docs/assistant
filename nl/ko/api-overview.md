---

copyright:
  years: 2015, 2019
lastupdated: "2019-02-21"

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
{:deprecated: .deprecated}
{:important: .important}
{:note: .note}
{:tip: .tip}

# {{site.data.keyword.conversationshort}} API 개요
{: #api-overview}

{{site.data.keyword.conversationshort}} REST API 및 해당 SDK를 사용하여 서비스와 상호작용하는 애플리케이션을 개발할 수 있습니다. 두 가지 버전의 {{site.data.keyword.conversationshort}} API(v1 및 v2)가 있으며 각각 다른 기능 세트를 지원합니다. 사용해야 하는 API는 애플리케이션에 필요한 메소드의 유형에 따라 다릅니다.

- **런타임 메소드**: 클라이언트 애플리케이션이 기존 어시스턴트 또는 스킬과 상호작용(수정하지는 않음)할 수 있도록 하는 메소드입니다. 이러한 메소드를 사용하여 프로덕션용으로 배치할 수 있는 사용자 측 클라이언트, 어시스턴트와 다른 서비스(예: 대화 서비스 또는 백엔드 시스템) 간의 통신을 중개하는 애플리케이션 또는 테스트 애플리케이션을 개발할 수 있습니다.

  {{site.data.keyword.conversationshort}} v2 API는 런타임 시 어시스턴트와 상호작용하는 데 사용될 수 있는 메소드(예: `/message`)에 대한 액세스 권한을 제공합니다. 이 API는 새 클라이언트 애플리케이션을 개발하는 데 사용하는 기본 API입니다. v2 API에 대한 세부사항은 {{site.data.keyword.conversationshort}} [v2 API 참조 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://{DomainName}/apidocs/assistant-v2){: new_window}를 참조하십시오.

  {{site.data.keyword.conversationshort}} v1 API에는 어시스턴트를 우회하고 대화 스킬에서 사용되는 작업공간에 직접 사용자 입력을 전송하는 `/message` 메소드가 포함되어 있습니다. v1 런타임 API는 주로 이전 버전과의 호환성을 위해 지원됩니다. `/message` 메소드를 사용하는 경우 앱이 어시스턴트의 오케스트레이션 및 상태 관리 기능을 활용할 수 없습니다. 자세한 정보는 [v1 런타임 API 사용](/docs/services/assistant?topic=assistant-api-client#v1-api)을 참조하십시오.

- **작성 메소드**: {{site.data.keyword.conversationshort}} 도구를 사용하여 그래픽으로 스킬을 빌드하는 대신 애플리케이션이 대화 스킬을 작성하거나 수정할 수 있도록 하는 메소드입니다. 작성 애플리케이션은 다양한 메소드를 사용하여 스킬, 인텐트, 엔티티, 대화 노드 및 대화 스킬을 구성하는 기타 아티팩트를 작성하고 수정합니다.

  작성 애플리케이션을 빌드하려면 v1 API를 사용하십시오. 자세한 정보는 [v1 API 참조 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://{DomainName}/apidocs/assistant){: new_window}를 참조하십시오.

  **참고:** v1 작성 메소드는 스킬이 아니라 작업공간과 상호작용합니다. 작업공간은 대화 스킬 내의 대화 및 훈련 데이터(예: 인텐트 및 엔티티)에 대한 컨테이너입니다. 대부분의 목적으로 API를 사용할 때 작업공간과 대화 스킬을 교환 가능한 것으로 생각할 수 있습니다. 예를 들어, API를 사용하여 새 작업공간을 작성하는 경우 {{site.data.keyword.conversationshort}} 도구에서 새 대화 스킬로 표시됩니다.

사용 가능한 API 메소드의 목록은 [API 메소드 요약](/docs/services/assistant?topic=assistant-api-methods)을 참조하십시오.
