---

copyright:
  years: 2015, 2019
lastupdated: "2019-05-29"

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

# Slack과 통합
{: #deploy-slack}

Slack은 사람들이 서로 협업하는 데 도움을 주는 클라우드 기반 메시징 애플리케이션입니다.
{: shortdesc}

대화 스킬을 구성하여 어시스턴트에 추가한 후 어시스턴트를 Slack과 통합할 수 있습니다.

통합 시 어시스턴트가 지원하도록 구성하는 이벤트에 따라, 직접 메시지에서 또는 어시스턴트가 직접 멘션되는 채널에서 묻는 질문에 어시스턴트가 응답할 수 있습니다. 

## Slack 통합 추가
{: #deploy-slack-task}

1.  어시스턴트 탭에서 배치할 어시스턴트 타일을 클릭하여 여십시오.

1.  통합 섹션에서 **통합 추가**를 클릭하십시오.

1.  **Slack**을 클릭하십시오.

1.  화면에 제공된 지시사항에 따라 통합 프로세스를 완료하십시오.

    지원하려는 다음 구독 이벤트 중 하나 이상을 선택하십시오.

    - `message.im`: 누군가 어시스턴트와 직접 메시지를 시작할 때 어시스턴트가 응답합니다. 
    - `app_mentions`: 누군가 채널에서 이름으로 어시스턴트를 멘션할 때 어시스턴트가 응답합니다. 

다른 사람이 배치 단계를 진행하는 과정을 따르려면 다음 동영상을 보십시오.

<iframe class="embed-responsive-item" id="youtubeplayer" title="Slack 배치 단계 연습" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/RBGBPJ8h4HQ?rel=0" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

재생 시간: 9.5분

## 대화 고려사항
{: #deploy-slack-dialog}

대화에 추가하는 서식이 있는 응답은 다음을 제외하고 예상대로 Slack 채널에 표시됩니다.

- **휴먼 에이전트에 연결**: 이 응답 유형은 무시됩니다.

- **이미지**: 이미지 제목은 필수입니다. 제목을 제공하지 않으면 이미지가 표시되지 않습니다.

- **옵션**: 이 응답 유형은 사용자가 선택할 수 있는 옵션 목록을 표시합니다.

  - 제목은 **필수**이며 옵션 목록 앞에 표시됩니다.
  - 설명은 지정 여부와 관계없이 표시되지 않습니다.
  - 사용자가 단추 중 하나를 클릭하면 단추 선택사항이 사라지고 사용자의 선택으로 생성된 사용자 입력으로 대체됩니다. 단일 응답에 여러 응답 유형을 포함시키는 경우 옵션 응답 유형을 마지막에 배치하십시오. 그렇지 않으면 출력에 응답과 사용자 입력이 혼합되어 있어서 사용자를 혼란스럽게 할 수 있습니다.

- **일시정지**: 이 응답 유형은 Slack 채널에서 어시스턴트의 활동을 일시정지합니다. 그러나 입력을 표시하도록 선택하는지 여부에 관계없이 어시스턴트가 일시정지되었음을 나타내는 가시적인 표시기가 표시되지 않습니다.

응답 유형에 대한 자세한 내용은 [서식이 있는 응답](/docs/services/assistant?topic=assistant-dialog-overview#dialog-overview-multimedia)을 참조하십시오.

## 어시스턴트와 대화
{: #deploy-slack-try}

어시스턴트와의 대화를 시작하려면 다음 단계를 완료하십시오.

1.  Slack을 열과 앱과 연관된 작업공간으로 이동하십시오.
1.  앱 섹션에서 작성한 애플리케이션을 클릭하십시오.
1.  어시스턴트와 대화하십시오.

대화의 Welcome 노드는 Slack 통합에서 처리되지 않습니다. Slack 채널에서는 "시험 사용" 분할창 또는 미리보기 링크 통합 웹 페이지에 있는 것처럼 환영 메시지가 표시되지 않습니다. 사용자가 시작한 대화 플로우에서는 `welcome` 특수 조건이 있는 노드를 건너뛰기 때문에 여기서 환영 메시지가 트리거되지 않습니다. Slack은 사용자가 대화를 시작할 때까지 대기합니다. 대화 시작 시 컨텍스트 변수에 대한 기본값을 설정해야 하는 경우 Welcome 노드에서 설정하지 마십시오. 자세한 정보는 [대화 시작](/docs/services/assistant?topic=assistant-dialog-start)을 참조하십시오.
{: note}

비활성 상태가 60분(Lite 및 Standard 플랜의 경우 5분)을 넘으면 현재 세션의 대화 플로우가 다시 시작됩니다. 즉, 사용자가 어시스턴트와의 상호작용을 중지하면 60분(또는 5분) 후에 이전 대화 중에 설정된 컨텍스트 변수값이 널로 설정되거나 다시 기본값으로 설정됩니다.
