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

# 베타
{: #beta}

![베타](images/beta.png) IBM에서는 베타로 분류된 평가용 서비스, 기능 및 언어 지원을 릴리스합니다. 이러한 기능은 불안정하고 자주 변경될 수 있으며 짧은 통보로 중단될 수 있습니다. 베타 기능은 일반적으로 제공되는 기능과 동일한 수준의 성능이나 호환성을 제공하지 않을 수도 있으며 프로덕션 환경에서 사용하기 위한 것이 아닙니다. 베타 기능은 [IBM Developer 답변 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://developer.ibm.com/answers/topics/watson-assistant/){: new_window}에서만 지원됩니다. 

## 사용 가능한 베타 기능
{: #beta-features}

다음 기능은 베타 프로그램의 참가자만 사용할 수 있습니다. 액세스 권한을 요청하는 방법을 알아보려면 [베타 프로그램에 참여](/docs/services/assistant?topic=assistant-feedback#feedback-beta)를 참조하십시오.

- 검색 스킬에 대해 작업하는 방법이 변경되었습니다. 이제 하나의 검색 스킬과 하나의 대화 스킬을 동일한 어시스턴트에 추가할 수 있습니다. 둘 다 추가하는 경우 대화 스킬의 대화에 있는 노드에서 사용자 입력을 처리할 수 없는 경우에 검색이 트리거됩니다. 다음 주제에서 자세히 알아볼 수 있습니다.

  - [검색 스킬](/docs/services/assistant?topic=assistant-skill-search-add)
  - [대화 스킬](/docs/services/assistant?topic=assistant-beta-skill-dialog-add)

  이 기능이 릴리스되면 플러스 또는 프리미엄 플랜 사용자만 사용할 수 있습니다.

- 대화 빌더의 사용자 인터페이스가 React JavaScript 라이브러리를 사용하도록 업데이트되었습니다. 이제 대화 기능이 고유 상태를 관리하는 캡슐화된 컴포넌트로 제공되어 사용자 경험의 응답성이 향상됩니다.

- 사용자 입력의 맞춤법 오류를 정정하도록 스킬을 구성할 수 있습니다. 세부사항은 [사용자 입력 정정](/docs/services/assistant?topic=assistant-beta-spell-check)을 참조하십시오.

- 기존 고객 지원 대화 기록을 활용하여 어시스턴트를 훈련하는 데 사용할 적절한 인텐트 및 사용자 예제 세트를 찾으십시오. 세부사항은 [인텐트 빌드에 대한 도움 받기](/docs/services/assistant?topic=assistant-beta-intent-recommendations)를 참조하십시오.
