---

copyright:
  years: 2015, 2019
lastupdated: "2019-07-19"

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

# Intercom과 통합![Plus 또는 Premium 플랜만 해당](images/plus.png)
{: #deploy-intercom}

Intercom은 고객의 라이프사이클 전반에 걸쳐 보다 나은 관계를 통해 비즈니스 성장을 촉진하는 고객 메시징 플랫폼입니다.
{: shortdesc}

Intercom은 고객 지원 팀에 새로운 에이전트를 추가하기 위해 IBM과 파트너 관계를 맺고 있습니다. 어시스턴트와 Intercom 애플리케이션을 통합하여 앱이 어시스턴트와 휴먼 에이전트 간의 사용자 대화를 원활하게 전달하도록 할 수 있습니다. 통합에 대한 자세한 내용은 [Watson 블로그 게시글![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://medium.com/@blakemcgregor/contact-center-post-394dff427c8)을 참조하십시오.

이 통합은 Plus 또는 Premium 플랜 사용자만 사용할 수 있습니다. 시험 사용해 보고 싶으면, 무료 Plus Trial 플랜에 등록할 수 있습니다. [Plus Trial 가져오기](https://cloud.ibm.com/registration?target=%2Fdeveloper%2Fwatson%2Flaunch-tool%2Fconversation%3Fplan%3Dplus-trial&cm_mmc=OSocial_Voicestorm-_-Watson+AI_Watson+Core+-+Conversation-_-WW_WW-_-Intercom+Trial+Registration+Link&cm_mmca1=000027BD&cm_mmca2=10004432).
{: note}

Intercom과 어시스턴트를 통합하는 경우, Intercom 애플리케이션이 사용자 스킬을 위한 클라이언트 대면 애플리케이션이 됩니다. 사용자와의 모든 상호 작용은 Intercom을 통해 시작 및 관리됩니다.

현재는 한 통합 채널에서 진행 중인 대화를 다른 채널로 전달하는 방법이 없습니다.

## 일회성 에이전트 작성
{: #deploy-intercom-account-prereq}

Intercom 통합을 어시스턴트에 추가하기 전에 귀하 또는 조직의 사용자가 이와 같은 일회성 전제조건 단계를 완료해야 합니다.

1.  어시스턴트를 위한 기능 이메일 계정을 작성하십시오.

    Intercom에서 어시스턴트를 팀에 추가하기 전에 각 어시스턴트에 올바른 고유 이메일 주소가 있어야 합니다.
1.  Intercom 작업공간에서 팀에 어시스턴트를 새 에이전트로 추가하십시오.

    Intercom 작업공간의 팀 동료 설정 페이지로 이동하여 이전 단계에서 작성한 이메일을 초대 필드에 추가하여 어시스턴트를 새 에이전트로 초대하십시오.

    Intercom 작업공간을 아직 설정하지 않은 경우, [www.intercom.com ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.intercom.com)에서 하나를 작성하십시오. 최소한, 작업공간을 생성할 수 있도록 Intercom에서 *Inbox* 제품에 대한 구독이 필요합니다.

1.  이전에 작성한 어시스턴트 이메일 계정에서 Intercom으로부터 온 초대를 찾으십시오. 팀에 참여하려면 이메일에 있는 링크를 클릭하십시오. 어시스턴트의 기능 이메일 주소를 사용하여 가입하려면 팀에 참여하십시오.

1.  **선택사항**: 어시스턴트의 프로파일을 업데이트하십시오.

    어시스턴트의 이름과 프로파일 사진을 편집할 수 있습니다. 이 프로파일은 작업공간 내 개인 에이전트 통신의 어시스턴트와 Intercom 애플리케이션을 통한 고객과의 공개 상호작용에서의 어시스턴트를 나타냅니다. 귀하의 브랜드를 반영하는 프로파일을 작성하십시오.

    탐색줄에서 Intercom 프로파일 아이콘을 클릭하여 프로파일 및 작업공간 설정에 액세스하십시오.

    ![Intercom 설정 페이지 화면.](images/intercom-settings.png)

## 대화 준비
{: #deploy-intercom-dialog-prereq}

어시스턴트와 연관된 대화 스킬이 없는 경우, 지금 하나를 작성하거나 어시스턴트에 추가하십시오. 자세한 내용은 [대화 빌드](/docs/services/assistant?topic=assistant-dialog-build)를 참조하십시오.

검색 스킬을 통해 검색을 트리거하는 것은 현재 Intercom 통합에서 지원되지 않습니다.
{: note}

어시스턴트가 사용자 요청을 처리하고 고객이 요청할 때 대화를 휴먼 에이전트로 전달할 수 있도록 대화 스킬에서 다음 단계를 완료하십시오.

1.  사람과의 대화에 대한 사용자 요청을 인식할 수 있는 스킬에 인텐트를 추가하십시오.

    사용자 자신의 인텐트를 추가할 수도 있고 IBM이 개발한 **일반** 컨텐츠 카탈로그와 함께 제공되는 사전 빌드된 인텐트(`#General_Connect_to_Agent`)를 추가할 수도 있습니다.

1.  이전 단계에서 작성한 인텐트를 조건으로 하는 대화에 루트 노드를 추가하십시오. **휴먼 에이전트에 연결**을 응답 유형으로 선택하십시오.

1.  Intercom 애플리케이션의 어시스턴트에서 트리거하도록 각 대화 분기를 준비하십시오.

    대화의 모든 루트 대화 노드는 폴더의 루트 노드를 포함하여 Intercom 팀 동료로 기능하는 동안 어시스턴트에 의해 처리될 수 있습니다. Intercom 통합을 구성할 때 어시스턴트가 각 대화 분기에 대해 수행할 조치를 지정합니다. 따라서, Intercom에서 노드를 숨길 수 없지만 노드가 트리거될 때 어시스턴트가 아무 조치도 하지 않도록 구성할 수 있습니다.

    각 대화 분기의 루트 노드에 대한 다음 필드를 채우십시오.

    - **노드 이름**: 노드 이름을 지정하십시오. 이 이름은 나중에 상호작용을 구성할 때 노드를 식별하는 방법입니다. 이름을 추가하지 않으면 노드 ID를 기반으로 노드를 선택해야 합니다.
    - **외부 노드 이름**: 대화 분기의 목적에 대한 요약을 추가하십시오. 예: *상점 찾기*.

      이 정보는 어시스턴트가 사용자 조회에 응답하도록 제안할 때 어시스턴트 팀의 다른 에이전트에 표시됩니다. 조회를 처리할 수 있는 대화 노드가 두 개 이상 있는 경우, 어시스턴트는 휴먼 에이전트 멤버와 응답 옵션 목록을 공유하여 사용할 응답에 대한 조언을 얻습니다.

      ![노드 목적 요약을 추가하는 노드 편집 보기에 있는 필드의 화면.](images/disambig-node-purpose.png)

      2단계에서 작성한 루트 노드에 외부 노드 이름을 추가하지 **마십시오**. 에스컬레이션이 발생하는 경우, 어시스턴트에서 마지막으로 처리된 노드의 외부 노드 이름을 확인하고 충족되지 않은 사용자 목표를 파악합니다. 휴먼 에이전트에 연결 인텐트가 있는 노드에 외부 노드 이름을 포함하는 경우, 문제를 에스컬레이션하기 전에 사용자가 상호작용한 마지막 실제 목표 지향 노드를 어시스턴트가 파악하지 못하도록 합니다.
      {: tip}

1.  분기의 하위 노드에서 어시스턴트가 처리하지 않기를 바라는 후속 요청 또는 질문을 조건으로 하는 경우, 해당 노드에 **휴먼 에이전트에 연결** 응답 유형을 추가하십시오.

    예를 들어, 사용자가 처리해야 하는 중요한 문제를 다루는 노드에 이 응답 유형을 추가하거나, 어시스턴트가 반복적으로 사용자를 이해하지 못하는 경우에 추적하는 노드에 이 응답 유형을 추가해야 합니다.

    런타임 시 대화가 이 하위 노드에 도달하면 대화가 해당 지점에서 사용자 에이전트로 전달됩니다. 나중에 Intercom 통합을 설정할 때 각 분기에 대한 백업으로 휴먼 에이전트를 선택할 수 있습니다.

이제 대화에서 Intercom의 어시스턴트를 지원할 준비가 되었습니다.

### 대화 고려사항
{: #deploy-intercom-dialog}

대화에 추가한 일부 서식이 있는 응답은 Intercom 사용자에게 표시되는 방법과 다르게 "시험 사용" 분할창 내에 표시됩니다. 아래 표에서는 응답 유형이 Intercom에서 처리되는 방법에 대해 설명합니다.

| 응답 유형 | Intercom 사용자에게 표시되는 방법  |
|---------------|---------------------------|
| **선택사항**    | 선택사항은 번호 지정된 목록으로 표시됩니다. **제목** 또는 **설명** 필드에서는, 목록에서 선택사항을 선택하는 방법을 사용자에게 설명하는 지침을 제공합니다. |
| **이미지**     | 이미지 **제목**, **설명** 및 이미지 자체가 렌더링됩니다. |
| **일시정지**     | 사용 설정에 관계 없이, 입력 표시기가 일시정지 기간 동안 표시되지 않습니다. |

응답 유형에 대한 자세한 내용은 [서식이 있는 응답](/docs/services/assistant?topic=assistant-dialog-overview#dialog-overview-multimedia)을 참조하십시오.

## Intercom 통합 추가
{: #deploy-intercom-add-intercom}

1.  어시스턴트 탭에서 배치할 어시스턴트 타일을 클릭하여 여십시오.

1.  통합 섹션에서 **통합 추가**를 클릭하십시오.

1.  **Intercom**을 클릭하십시오.

    화면에 제공된 지시사항을 따르십시오. 다음 절에서는 해당 단계에 대해 설명합니다.

다음 4분 분량의 동영상에서는 단계를 보여줍니다.

<iframe class="embed-responsive-item" id="youtubeplayer" title="빠른 설정" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/SkbFWNScueU" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

## Intercom에 어시스턴트 연결하기
{: #deploy-intercom-connect}

Intercom에 어시스턴트 사용 권한을 부여하는 즉시, 어시스턴트가 Intercom 팀의 지원 가능한 구성원이 됩니다.

휴먼 에이전트는 Intercom의 지정 규칙을 사용하여 어시스턴트에 메시지를 지정할 수 있습니다. 다음 방법으로 어시스턴트에 메시지를 지정할 수 있습니다.

- 일부 기준에 기반하여 팀 동료 또는 팀 받은 편지함에 인바운드 대화 자동 지정

  다음 1분 30초 분량의 동영상에서는 단계를 보여줍니다.

  <iframe class="embed-responsive-item" id="youtubeplayer2" title="자동 지정" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/4M9wu8NHxcY" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

- 런타임 시 휴먼 에이전트가 수행한 수동 재지정입니다.

  다음 3분 분량의 동영상에서는 단계를 보여줍니다.

  <iframe class="embed-responsive-item" id="youtubeplayer3" title="수동 지정" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/jAnolyUJAIA" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

[Intercom 문서![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.intercom.com/help/support-and-retain-customers/work-as-a-team/assign-conversations-to-teammates-and-teams)를 참조하십시오.

1.  대화가 준비되면 **지금 연결**을 클릭합니다.
1.  Intercom 사이트로 이동하려면 **Intercom에 액세스**를 클릭합니다.

     사용자 자신의 정보가 아닌 어시스턴트 이메일 주소 및 비밀번호를 사용하여 Intercom에 로그인하십시오. 조직에서 둘 이상의 구성원이 공유하는 기능 ID에 대한 연결을 설정하려고 합니다.
     {: important}

1.  **액세스 권한 부여**를 클릭합니다.
1.  **개요로 돌아가기**를 클릭합니다.

이제 Intercom 팀 동료로부터 할당을 받을 수 있습니다. 그렇지 않은 경우, [대화를 설정](#deploy-intercom-dialog-prereq)하십시오.
{: important}

## 메시지 라우팅 구성
{: #deploy-intercom-config-backup}

어시스턴트가 진행 중인 대화를 사람에게 전송해야 하는 경우를 대비하여 어시스턴트의 백업으로 직원을 지정하십시오. 각 대화 분기의 백업 담당자로 다른 팀 또는 팀 동료를 선택할 수 있습니다.

어시스턴트에서 사용자로 에스컬레이션하기 위한 라우팅 지정을 설정하려면 다음 단계를 완료하십시오.

1.  Intercom 통합 페이지에서 대화를 준비했는지 확인하려면 **내 대화 스킬이 준비됨**을 클릭하십시오.

    [대화 준비](#deploy-intercom-dialog-prereq) 프로시저를 완료한 경우에만 이 단추를 클릭하십시오.
    {: important}

1.  *설정* 섹션에서 **규칙 관리**를 클릭하십시오.

    변경하지 않은 경우, 모든 노드의 백업 담당자가 지정되지 않은 상태로 남습니다.

1.  **새 규칙**을 클릭하십시오.

1.  *노드 선택* 드롭 다운 목록에서, 구성할 대화 분기의 노드를 선택하십시오.

    분기는 노드 이름으로 식별됩니다. 노드 이름을 지정하지 않은 경우 노드의 ID가 대신 표시됩니다.

1.  이 대화 분기의 백업 담당자로 팀 또는 휴먼 에이전트 팀 동료를 선택하십시오. 어시스턴트가 조회에 응답할 수 없거나 *휴먼 에이전트에 연결* 응답 유형으로 하위 노드를 요청한 경우, 사용자 조회가 이 사용자에게 에스컬레이션됩니다. 이 응답 유형은 휴먼 사용자만 응답해야 함을 나타냅니다.

1.  다른 대화 분기의 라우팅 규칙을 정의하려면 **새 규칙**을 다시 클릭하고 이전 단계를 반복하십시오.

    분기의 하위 노드에 *휴먼 에이전트에 연결* 응답 유형이 있는 루트 노드에 대한 지정을 설정하십시오. 연관된 루트 노드를 특정 사용자 또는 팀에게 전송하지 않는 경우, 중요한 문제가 *지정되지 않음* 받음 편지함으로 전송될 수 있습니다.

1.  규칙을 추가한 후, **개요로 돌아가기**를 클릭하여 페이지를 종료하십시오.

다음 3분 분량의 동영상에서는 단계를 보여줍니다.

<iframe class="embed-responsive-item" id="youtubeplayer0" title="주제 기반 에스컬레이션 라우팅" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/dTwJZOqdzII" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

## 어시스턴트에게 사용자 조회를 모니터하고 이에 응답할 수 있는 권한 부여
{: #deploy-intercom-config-action}

어시스턴트가 자체적으로 Intercom 받은 편지함을 모니터링하고 메시지에 응답하기 시작하도록 하려면 모니터링을 켜십시오.

어시스턴트가 Intercom에 로그인된 사용자 조회를 감시합니다. 어시스턴트가 사용자 조회에 응답하는 방법을 알고 있다고 확신하는 경우, 어시스턴트는 사용자에게 직접 응답합니다. (어시스턴트에서 식별된 최상위 인텐트의 신뢰도 점수가 0.75 이상인 경우에 어시스턴트가 확신합니다.)

어시스턴트가 특정 유형의 사용자 조회에 응답하지 않도록 하려면, 어시스턴트가 대화 분기당 수행할 다른 조치를 지정하는 규칙을 추가할 수 있습니다. 예를 들어, Intercom 팀에 어시스턴트를 더 보수적으로 통합하고 싶을 수도 있습니다. 즉, 어시스턴트가 다른 팀 동료에게 응답하도록 메시지를 전송하므로 어시스턴트는 응답을 제안만 할 수 있습니다. 시간이 지나면서 어시스턴트가 스스로를 증명한 후에는 더 많은 책임을 부여할 수 있습니다.

어시스턴스에서 특정 대화 분기를 처리하는 방법을 구성하려면 규칙을 정의하십시오.

1.  Intercom 통합 페이지의 *어시스턴트에서 받은 편지함을 모니터링하도록 설정* 섹션에서 모니터링을 **켜짐**으로 전환하십시오.

1.  *설정*에서 **규칙 관리**를 클릭하십시오.

    규칙을 정의하지 않는 경우, 어시스턴트가 *지정되지 않음* 받은 편지함을 모니터링하고 처리할 수 있다고 확신하는 사용자 조회에 자동으로 응답하도록 구성됩니다.

1.  *Intercom 받은 편지함 모니터링* 필드에서, 어시스턴트가 모니터링할 Intercom 받은 편지함을 선택하십시오.

1.  특정 대화 분기에 대해 고유한 상호작용 패턴을 정의하려면 **새 규칙**을 클릭하십시오.

1.  *노드 선택* 드롭 다운 목록에서, 구성할 분기의 노드를 선택하십시오.

    분기는 노드 이름으로 식별됩니다. 노드 이름을 지정하지 않은 경우 노드의 ID가 대신 표시됩니다.

1.  이 대화 노드가 트리거될 때 어시스턴트가 실행할 조치 유형을 선택하십시오. 조치 유형 옵션은 다음과 같습니다.

    - **아무 조치 없음**: 어시스턴트가 응답에 관여하지 않습니다. 다른 사람이 처리하도록 사용자의 메시지가 받은 편지함에 남습니다.
    - **팀 또는 팀 동료에게 보내기**: 어시스턴트가 사용자 입력을 평가하여 목표를 결정한 다음, 이를 적합한 팀 동료에게 보냅니다.
    - **팀 또는 팀 동료에게 제안**: Intercom 애플리케이션을 통해 휴먼 에이전트와 참고를 공유함으로써 응답하는 방법에 대한 제안을 팀 동료에게 제공합니다.

      - 사용자 입력이 대화 분기를 트리거하는 경우(즉, 포괄적인 상호작용을 나타내는 하위 노드가 있는 루트 대화 노드를 의미하는 경우), 어시스턴트는 요청을 처리할 수 있음을 나타내고 해당 작업을 수행하겠다고 제안합니다.
      - 사용자 입력이 하위가 없는 루트 노드를 트리거하면, 어시스턴트는 해당 노드의 프로그래밍된 응답을 휴먼 에이전트와 공유하지만, 사용자에게 직접 응답하지는 않습니다.
      - 입력이 높은 신뢰도를 가진 대화 노드를 둘 이상 트리거하는 경우, 어시스턴트가 휴먼 팀 동료에게 가능한 응답을 표시하여 팀 동료가 최상의 응답을 선택하도록 요청합니다.

      모든 경우에 휴먼 에이전트는 어스턴트가 인수하도록 할지 여부를 결정합니다.

    - **답변**: 어시스턴트가 다른 팀 동료와 협의하지 않고 사용자에게 바로 답변합니다.

    팀 동료 관여는 *팀 또는 팀 동료에게 보내기* 또는 *팀 또는 팀 동료에게 제안* 조치 유형을 지정한 모든 노드에 필수입니다. 다시 돌아가서 특별히 이 대화 노드의 백업으로 적합한 개인 또는 팀을 지정하는 규칙을 추가하십시오.

1.  다른 대화 분기의 고유한 상호작용 설정을 정의하려면 **새 규칙**을 다시 클릭하여 이전 단계를 반복하십시오.

1.  규칙을 추가한 후, **개요로 돌아가기**를 클릭하여 페이지를 종료하십시오.

대화가 변경되면 Intercom 통합 페이지로 돌아가서 이 규칙에 대한 증분 변경을 수행합니다.

다음 3분 분량의 동영상에서는 단계를 보여줍니다.

<iframe class="embed-responsive-item" id="youtubeplayer1" title="받은 편지함 모니터링" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/fFKjWUfIftw" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

## 통합 테스트
{: #deploy-intercom-try}

Intercom 통합을 엔드 투 엔드에서 효과적으로 테스트하려면 Intercom 최종 사용자 애플리케이션에 대한 액세스 권한이 있어야 합니다. Intercom 작업공간을 이미 작성하거나 편집했습니다. 작업공간에는 연관된 사용자 인터페이스 클라이언트가 있어야 합니다. 그렇지 않은 경우 이를 설정하는 방법에 대한 도움말을 보려면 [Intercom의 앱![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.intercom.com/help/apps-in-intercom){: new_window}을 참조하십시오.

Intercom 작업공간과 연관된 클라이언트 애플리케이션을 통해 테스트 사용자 조회를 제출하여 Intercom에서 메시지를 처리하는 방법을 확인할 수 있습니다. 어시스턴트가 응답해야 하는 메시지가 적절한 응답을 생성하고 어시스턴트가 응답하도록 구성되지 않은 메시지에 응답하지 않는지 확인하십시오.
