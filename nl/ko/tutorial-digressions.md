---

copyright:
  years: 2015, 2019
lastupdated: "2019-04-12"

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

# 튜토리얼: 다이그레션(digression) 이해
{: #tutorial-digressions}

이 튜토리얼에서는 다이그레션(digression)이 작동하는 방법을 직접 확인할 수 있습니다.
{: shortdesc}

## 학습 목표
{: #tutorial-digressions-objectives}

튜토리얼을 완료할 때까지 다음을 파악합니다.

- 다이그레션(digression)이 작동하도록 디자인되는 방법
- 다이그레션(digression)이 대화 플로우에 미치는 영향
- 대화에 대한 다이그레션(digression) 설정을 테스트하는 방법

### 소요 시간
{: #tutorial-digressions-duration}

이 튜토리얼은 완료하는 데 약 20분이 소요됩니다.

### 전제조건
{: #tutorial-digressions-prereqs}

{{site.data.keyword.conversationshort}} 인스턴스가 없는 경우 [시작하기 튜토리얼](/docs/services/assistant?topic=assistant-getting-started#getting-started-prerequisites)의 **시작하기 전에** 단계를 완료하여 이를 작성하십시오.

## 1단계: 다이그레션(digression) 쇼케이스 대화 스킬 가져오기
{: #tutorial-digressions-import-json}

먼저, *다이그레션(digression) 쇼케이스* 대화 스킬을 {{site.data.keyword.conversationshort}} 인스턴스로 가져와야 합니다.

1.  [digression-showcase.json](https://github.com/watson-developer-cloud/community/raw/master/watson-assistant/digression-showcase.json) 파일을 다운로드하십시오.
1.  {{site.data.keyword.conversationshort}} 인스턴스에서 ![가져오기](images/workspace_import.png) 아이콘을 클릭하십시오.
1.  **파일 선택**을 클릭한 후 이전에 다운로드한 **digression-showcase.json** 파일을 선택하십시오.
1.  **가져오기**를 클릭하여 대화 스킬 가져오기를 완료하십시오.

## 2단계: 임시로 대화에서 다이그레션(digression)
{: #tutorial-digressions-temporarily-digress-away}

다이그레션(digression)을 사용하면 사용자가 원래 대화 플로우로 돌아가기 전에 임시로 주제를 변경하기 위해 대화 분기에서 벗어날 수 있습니다. 이 단계에서는 레스토랑 예약을 시작한 후 레스토랑의 영업 시간을 묻기 위해 다이그레션(digression)합니다. 개점 시간 정보를 제공한 후 어시스턴트가 레스토랑 예약 대화 창으로 돌아옵니다.

1.  **대화**를 클릭하여 인텐트가 있는 페이지에서 대화 트리의 보기로 전환하십시오.

1.  ![시험 사용](images/ask_watson.png) 아이콘을 클릭하여 "시험 사용" 분할창을 엽니다.
1.  텍스트 필드에 `Book me a restaurant`를 입력하십시오.

    어시스턴트가 예약일을 묻는 프롬프트로 응답합니다. `When do you want to go?`

1.  응답 옆에 있는 **위치** ![위치](images/location.png) 아이콘을 클릭하여 대화 트리에서 응답을 트리거한 노드(**Restaurant booking** 노드)를 강조표시하십시오.

    ![강조표시되어 있는 Restaurant booking 노드와 시험 사용 분할창에서 진행 중인 대화를 표시합니다.](images/tut-dig-location.png)
1.  `Tomorrow`를 입력하십시오.

    어시스턴트가 예약 시간을 묻는 프롬프트로 응답합니다. `What time do you want to go?`

1.  레스토랑의 마감 시간을 모르기 때문에 다음과 같이 질문합니다. `What time do you close?`

    봇이 Restaurant booking 노드로부터 다이그레션(digression)하여 **Restaurant opening hours** 노드를 처리합니다. `The restaurant is open from 8:00 AM to 10:00 PM.`으로 응답합니다. 그런 다음, 어시스턴트가 Restaurant booking 노드로 돌아가서 예약 시간을 묻는 프롬프트를 다시 표시합니다.

    ![분할창에서 발생하는 다이그레션(digression)을 표시합니다.](images/tut-dig-digression.png)
1.  **선택사항**: 대화 플로우를 완료하려면 예약 시간을 `8pm`으로 입력하고 게스트 수를 `2`로 입력하십시오.

축하합니다! 대화 플로우에서 다이그레션(digression)한 후 대화 플로우로 돌아왔습니다.

## 3단계: 슬롯 다이그레션(digression) 사용 안함
{: #tutorial-digressions-disable-slot}

이 단계에서는 사용자가 다이그레션(digression)하지 못하도록 Restaurant booking 노드의 다이그레션(digression) 설정을 편집하고 설명 변경이 대화 플로우에 미치는 영향을 확인합니다.

1.  **Restaurant booking** 노드에 대한 현재 다이그레션(digression) 설정을 살펴보겠습니다. 노드를 클릭하여 편집 보기에서 여십시오.

1.  **사용자 정의**를 클릭하고, **다이그레션(digression)** 탭을 클릭하십시오.

    ![Restaurant booking 노드에 대한 다이그레션(digression) 설정을 표시합니다.](images/tut-dig-resto-settings.png)

1.  **다이그레션(digression) 허용** 토글을 켜짐에서 **꺼짐**으로 변경한 후 **적용**을 클릭하십시오.

1.  노드 편집 보기를 닫으려면 ![닫기](images/close.png)를 클릭하십시오.

1.  대화를 재설정하려면 "시험 사용" 분할창에서 **지우기**를 클릭하십시오.

1.  `Book me a restaurant`를 입력하십시오.

    어시스턴트가 예약일을 묻는 프롬프트로 응답합니다. `When do you want to go?`

1.  `Tomorrow`를 입력하십시오.

    어시스턴트가 예약 시간을 묻는 프롬프트로 응답합니다. `What time do you want to go?`

1.  다음과 같이 물으십시오. `What time do you close?`

    어시스턴트에서 이 질문이 #restaurant_opening_hours 인텐트를 트리거한다고 인식하지만 이를 무시하고 대신 @sys-time 슬롯과 연관된 프롬프트를 다시 표시합니다.

사용자가 레스토랑 예약 프로세스에서 다이그레션(digression)하지 못하도록 설정했습니다.

## 4단계: 돌아가지 않는 노드로 다이그레션(digression)
{: #tutorial-digressions-digress-without-return}

현재 노드를 처리하기 위해 어시스턴트가 다이그레션(digression)한 노드로 되돌아가지 않도록 대화 노드를 구성할 수 있습니다. 이를 설명하기 위해 레스토랑 영업 시간 노드의 다이그레션(digression) 설정을 변경합니다. 2단계에서는 어시스턴트가 Restaurant booking 노드로부터 다이그레션(digression)하여 Restaurant opening hours 노드로 이동한 후 Restaurant booking 노드로 되돌아와서 예약 프로세스를 계속 진행하는 과정을 확인했습니다. 이 연습에서는 설정을 변경한 후 **Job opportunities** 대화에서 다이그레션(digression)하여 레스토랑 개점 시간에 대해 묻고 어시스턴트가 중단된 위치로 돌아가지 않는지 확인합니다.

1.  **Restaurant opening hours** 노드를 클릭하여 여십시오.

1.  **사용자 정의**를 클릭하고, **다이그레션(digression)** 탭을 클릭하십시오.

1.  **다이그레션(digression)이 이 노드로 들어올 수 있음** 섹션을 펼치고 **다이그레션(digression) 후 리턴** 선택란을 선택 취소하십시오. **적용**을 클릭한 후 ![닫기](images/close.png)를 클릭하여 노드 편집 보기를 닫으십시오.

1.  대화를 재설정하려면 "시험 사용" 분할창에서 **지우기**를 클릭하십시오.

1.  **Job opportunities**대화 노드를 참여시키려면 `I'm looking for a job`을 입력하십시오.

    어시스턴트가 다음과 같이 응답합니다. `We are always looking for talented people to add to our team. What type of job are you interested in?`

1.  이와 같이 질문하는 대신 봇에게 관련이 없는 질문을 하십시오. 다음과 같이 입력하십시오. `What time do you open?`

    어시스턴트가 Job opportunities 노드에서 Restaurant opening hours 노드로 다이그레션(digression)하여 질문합니다. 어시스턴트가 다음과 같이 응답합니다. `The restaurant is open from 8:00 AM to 10:00 PM.`

    이전 테스트에서와 달리 이번에는 대화가 **Job opportunities** 노드에서 중단된 위치를 선택하지 않습니다. **Restaurant opening hours** 노드의 설정을 돌아가지 않는 것으로 변경했기 때문에 어시스턴트가 진행 중이었던 대화로 돌아가지 않습니다.

    ![다이그레션(digression) 후 돌아가지 않는 대화를 표시합니다.](images/tut-dig-noreturn.png)

축하합니다! 돌아가지 않고 대화에서 다이그레션(digression)했습니다.

## 요약
{: #tutorial-digressions-summary}

이 튜토리얼에서는 다이그레션(digression)이 작동하는 방식을 체험하고 개별 대화 노드 설정이 다이그레션(digression) 동작에 어떤 영향을 미칠 수 있는지 알아봤습니다.

## 다음 단계
{: #tutorial-digressions-next-steps}

자체 대화에 대한 다이그레션(digression)을 구성할 때 도움말을 얻으려면 [다이그레션(digression)](/docs/services/assistant?topic=assistant-dialog-runtime#dialog-runtime-digressions)을 참조하십시오.
