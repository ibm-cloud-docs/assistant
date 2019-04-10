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

# 작업공간 액세스
{: #edit-convo-workspace}

이전 버전의 서비스({{site.data.keyword.watson}} Conversation으로 알려진 경우에도)로 작업공간을 작성한 경우 작업공간을 계속 사용할 수 있습니다.
{: shortdesc}

## 대화 작업공간 변경
{: #edit-convo-workspace-task}

작업공간을 계속 사용할 수 있습니다. 이제 작업공간을 *스킬*이라고 합니다. 레거시 작업공간을 변경하려면 다음 단계를 완료하십시오.

1.  {{site.data.keyword.conversationshort}} 홈 페이지에서 **스킬**을 클릭하십시오.

    {{site.data.keyword.conversationshort}} 서비스의 GA 버전으로 작성한 모든 작업공간이 대화 스킬 타일로 표시됩니다.
1.  편집할 작업공간을 나타내는 대화 스킬을 클릭하십시오.
1.  원하는 훈련 데이터 또는 대화에 변경사항 또는 추가사항을 작성하십시오.
1.  변경사항을 저장하십시오.

훈련이 완료되면 서비스를 호출 중인 사용자 정의 애플리케이션에서 업데이트를 사용할 수 있습니다. 자세한 정보는 [Watson Assistant API 개요](/docs/services/assistant?topic=assistant-api-overview)를 참조하십시오.

## 제한사항
{: #edit-convo-workspace-cons}

최신 버전의 서비스를 사용하면 향상된 유연성으로 레거시 서비스에 대해 수행할 수 있었던 모든 작업을 계속 수행할 수 있습니다. 이전 버전의 서비스로 작업공간을 작성했으며 대체하지 않으려는 기존 애플리케이션에서 호출하고 있습니까? 이미 계속 제어하려는 개별 대화 턴(turn)에서 상태를 제어하고 유용한 기능을 수행합니다. 최신 버전의 /message API를 사용하여 호출 간에 조정할 수 있습니다. 장점은 이를 수행할 필요가 없다는 것입니다. 최신 버전에서는 동일한 기본 대화 스킬로 한 번에 둘 이상의 통합 채널을 지원할 수 있습니다.

어시스턴트를 작성하고 해당 어시스턴트에 대화 스킬을 추가하는 단계를 건너뛰도록 선택하면 어시스턴트가 제공하는 단순성을 놓치게 됩니다. 즉, 단일 대화를 사용하여 한 번에 여러 통합 채널을 통해 고객과 상호작용하고 사용자에게 인기를 얻고 있는 새 채널로 빠르게 확장하거나 전환할 수 **없습니다**.

## 스킬 및 작업공간
{: #edit-convo-workspace-names}

도구에 대화 스킬로 제공되는 것은 실질적으로 V1 작업공간의 랩퍼입니다. 현재는 V2 API를 사용하여 스킬을 작성할 수 있는 API 메소드가 없지만 V1 API를 사용하여 계속 작업공간을 작성할 수 있습니다.

- 도구를 열 때 11월 9일 이전에 작성한 작업공간이 스킬로 표시됩니다. 스킬 이름은 작업공간 이름에서 가져옵니다. 그러나 나중에 스킬 이름 또는 설명을 변경하면 이러한 변경사항이 작업공간에 영향을 미치지 않습니다. 마찬가지로 API를 사용하여 작업공간 이름 또는 설명을 편집하는 경우 이러한 변경사항이 스킬에 영향을 미치지 않습니다.
- 도구에서 스킬에 대한 API 세부사항에 스킬 ID와 작업공간 ID가 둘 다 표시됩니다.
