---

copyright:
  years: 2015, 2019
lastupdated: "2019-07-29"

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

# 비활성 제한시간 설정 변경 ![Plus 또는 Premium 플랜만 해당](images/plus.png)
{: #assistant-settings}

사용자가 기본 제공 통합 중 하나를 통해 어시스턴트와 상호작용하는 경우 사용자가 어시스턴트와 상호작용하지 않는 특정 기간이 경과한 후에 대화 세션이 종료됩니다. 비활성 제한시간 설정을 변경하여 대화 세션이 재설정되기 전에 대기하는 시간을 지정할 수 있습니다.
{: shortdesc}

대화 세션이 재설정되면 사용자와의 이전 교환 중에 저장된 컨텍스트 정보가 대화에서 손실됩니다. 예를 들어 대화가 사용자 이름을 요청하고 대화의 나머지 부분에서 해당 이름으로 사용자를 호출하면 대화 세션이 종료되고 새 대화가 시작되면 사용자 이름을 다시 요청하여 대화가 시작됩니다.

## 세션 한계
{: #assistant-settings-session-limits}

비활성 제한시간에 허용되는 기간은 서비스 인스턴스 플랜 유형별로 다릅니다. 다음 표에는 한계가 나열되어 있습니다.

| 서비스 플랜  | 대화 세션 기본 비활성 기간      | 대화 세션 최대 비활성 기간  |
|--------------|--------------------------------:|----------------------------:|
| Premium     |                            60분 |                      24시간 |
| Plus       |                            60분 |                      24시간 |
| 표준         |                             5분 |                         5분 |
| Plus Trial   |                             5분 |                         5분 |
| Lite*        |                             5분 |                         5분 |
{: caption="서비스 플랜 세부사항" caption-side="top"}

## 설정 변경
{: #assistant-settings-timeout-task}

1.  어시스턴트에 대한 메뉴를 클릭한 후 **설정**을 선택하십시오.

1.  **제한시간 한계** 필드에 지정된 시간을 변경하십시오.

1.  설정 페이지를 닫으십시오. 변경사항이 자동으로 저장됩니다.
