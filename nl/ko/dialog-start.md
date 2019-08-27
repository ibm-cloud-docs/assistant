---

copyright:
  years: 2015, 2019
lastupdated: "2019-05-28"

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

# 대화 시작
{: #dialog-start}

기본 제공 Welcome 노드를 사용하여 모든 통합에 대해 동일한 방식으로 대화를 시작할 수 없습니다. 대신 이 임시 해결책을 사용하십시오.
{: shortdesc}

대화에서 Welcome 노드에 대해 정의하는 응답은 "시험 사용" 분할창 및 미리보기 링크 통합의 대화 위젯에서 대화를 시작하기 위해 표시됩니다. 그러나 사용자가 시작한 대화 플로우에서는 `welcome` 특수 조건이 있는 노드를 건너뛰기 때문에 많은 다른 채널 통합에서는 표시되지 않습니다. 또한 배치된 어시스턴트는 일반적으로 사용자가 대화를 시작할 때까지 대기하며 그 반대는 아닙니다.

`welcome` 특수 조건과 달리 `conversation_start` 특수 조건은 대화 시작 시 항상 트리거됩니다. 이러한 두 개의 특수 조건(`welcome` 및 `conversation_start`)이 포함된 노드의 조합을 사용하여 대화의 시작을 일관된 방식으로 관리할 수 있습니다.

대화 시작을 관리하려면 다음 단계를 완료하십시오.

1.  대화를 작성할 때 대화 트리의 맨 위에 자동으로 추가되는 Welcome 노드 위에 대화 노드를 추가하십시오.

1.  새로 추가된 이 노드에 대한 노드 조건을 앞에서 설명된 특수 조건인 `conversation_start`로 설정하십시오.

1.  `conversation_start` 노드에서 대화에 대한 기본값으로 설정할 컨텍스트 변수를 정의하십시오.

1.  이 노드에 대한 텍스트 응답을 정의하지 마십시오.

1.  대화 노드에서 바로 아래에 있는 `Welcome` 노드로 건너뛰어 **If bot recognizes(조건)**를 선택하도록 이 노드를 구성하십시오.

![conversation_start 노드가 아래에 있는 Welcome 노드로 건너뛰는 대화의 스크린샷입니다.](images/dialog-start.png)

이 디자인을 통해 대화가 다음과 같이 작동합니다.

- 통합 유형에 관계업이 `conversation_start` 노드가 처리되며, 이는 이 노드에 정의한 컨텍스트 변수가 초기화됨을 의미합니다.
- 어시스턴트가 대화 플로우를 시작하는 통합에서 `Welcome` 노드가 트리거되고 해당 텍스트 응답이 표시됩니다.
- 사용자가 대화를 시작하는 통합에서 사용자의 첫 번째 입력이 평가된 후 최상의 응답을 제공할 수 있는 노드에서 처리됩니다.
