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

# Watson에서 도움 받기
{: #intent-recommendations}

예를 들어, 실제 사용자 발화(utterance)를 포함하는 고객 지원 센터의 대화 로그 기록이 있는 경우 서비스가 기존 데이터에서 인텐트 사용자 예제 후보를 마이닝하도록 하십시오.

## 파일 업로드
{: #intent-recommendations-log-files-add}

시작하기 전에 서비스에 제공할 파일을 작성하십시오. 파일은 행당 하나의 사용자 발화가 있는 쉼표로 구분된 값(CSV)이어야 합니다. 이상적으로, 발화는 실제 고객 질문과 요청을 포함하는 콜 센터 기록에서 추출된 짧은 구문입니다. 각 사용자 발화 파일의 최대 크기는 20MB입니다.

다음과 같은 추가 가이드라인을 따르십시오.

  - 파일에 포함된 발화에서 민감한 데이터를 제거하십시오.

    민감한 데이터에는 이름, 이메일 주소 및 고객 ID와 같은 식별 가능한 자연인에 대한 정보 및 PHI(Protected Health Information)와 같은 규제 데이터가 포함됩니다.
  - 1,024자 길이를 초과하는 사용자 발화를 포함하지 마십시오. 더 긴 발화는 잘립니다.
  - 파일에 100개 이상의 발화가 포함되어야 합니다. 500개 이상의 발화가 포함된 파일이 더 나은 결과를 제공합니다.
  - 발화에 쉼표가 포함되어 있는 경우 발화를 따옴표로 묶으십시오.
  - CSV에는 하나의 열만 포함되어야 합니다.
  - 휴먼 에이전트 응답 또는 참고사항을 포함하여 사용자 생성 발화가 아닌 모든 항목을 제거하십시오.

  예:

  ```
  What happens to my coverage if I trade in my car?
  i'd like to buy a house.
  How do I add a dependent to my plan?
  "first, i want to know if i am already registered."
  ```

업로드하는 파일은 현재 서비스 인스턴스의 모든 스킬에서 공유됩니다. 인텐트 사용자 예제 권장사항 모두를 요청하는 경우 사용 가능한 모든 파일의 발화가 마이닝됩니다.

## 인텐트 사용자 예제 권장사항 가져오기
{: #intent-recommendations-get-example-recommendations}

다음 동영상은 인텐트 사용자 예제 권장사항을 가져오는 방법에 대한 개요(2분 소요)를 제공합니다.

<iframe class="embed-responsive-item" id="youtubeplayer" title="인텐트 사용자 예제 권장사항" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/L3FI8KeZfsc?rel=0" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

업로드하는 파일이 예제를 인텐트와 연관시킬 필요는 없습니다. 단순히 원시 사용자 발화를 제공하고 서비스가 현재 인텐트에 적합한 예제를 선택하는 작업을 수행하도록 합니다. 모든 인텐트에 대해 서비스가 동일한 데이터를 분석하여 해당 인텐트에 적합한 사용자 예제를 찾습니다.

1.  [인텐트 작성](/docs/services/assistant?topic=assistant-intents#intents-creating-intents-task)의 단계에 따라 인텐트를 작성하십시오.

1.  사용자가 이 인텐트를 트리거하기 위해 말할 것으로 예상하는 일반 발화의 전체 범위를 나타내는 5개 이상의 사용자 예제를 추가하십시오.

    이러한 시드 사용자 예제는 업로드하는 파일에서 찾을 수 있는 발화의 유형에 대해 서비스에 알립니다.

1.  **권장사항 표시**를 클릭하십시오.

    사용자 예제 소스 파일은 서비스 인스턴스의 스킬 간에 공유됩니다. 동료가 동일한 인스턴스의 스킬을 소유하고 파일을 업로드하는 경우 해당 파일이 공유 *사용자 예제 소스 파일* 콜렉션에 추가됩니다.

1.  **처음에만**: **파일 업로드**를 클릭한 후 **파일 선택**을 클릭하여 이전에 선택한 CSV 파일을 찾아서 선택하십시오.

    업로드한 파일에 모든 유형의 인텐트에 대한 발화가 포함되어도 괜찮습니다. 서비스가 작업 중인 인텐트를 파악하고 이 특정 인텐트에 대해 권장할 적절한 예제를 찾습니다.

    파일이 업로드되고 서비스에서 처리된 후 권장되는 발화가 표시됩니다. 권장사항이 작성되지 않은 경우 이 인텐트에 적합한 예제가 파일에 포함되지 않습니다.

1.  파일이 인텐트에 대한 유용한 권장사항을 제공할 수 없는 경우 다른 파일을 업로드하여 다른 발화 세트를 시도할 수 있습니다. 세부사항은 [업로드된 파일 관리](#intent-recommendations-manage-files)를 참조하십시오.

1.  서비스에서 권장사항을 표시한 후 이 인텐트에 대한 사용자 예제로 추가할 발화를 선택한 후 **추가**를 클릭하십시오. 또는 **다음 세트**를 클릭하여 추가 발화를 검토하십시오.
1.  CSV 파일의 컨텐츠에서 사용자 예제를 직접 검색하려면 **로그 검색** 탭을 클릭하고 검색 기반이 되는 키워드를 입력한 후 **Enter**를 누르십시오.

    다음과 같은 검색 조회 구문 가이드라인을 따르십시오.

    - 부울 연산자(예: `AND` 및 `OR`)가 지원됩니다.
    - 정확한 텍스트 일치사항을 검색하려면 따옴표가 붙은 텍스트를 추가하십시오("thisstringmustbepresent").
    - 정규식을 사용할 수 있습니다(예: `ly`로 끝나는 모든 용어를 찾기 위한 `*ly`).
    - 다음 문자가 정규식 연산자로 사용됩니다.

      `+ - = && || > < ! ( ) { } [ ] ^ " ~ * ? : \ /`

      연산자로 처리되지 않도록 하여 검색어에 포함하려면 앞에 백슬래시(`\`)를 붙여야 합니다.

이 방식으로 추가하는 사용자 예제는 플랜별 한계가 있는 인텐트 사용자 예제 총계에 포함됩니다. 세부사항은 [인텐트 한계](/docs/services/assistant?topic=assistant-intents#intents-limits)를 참조하십시오.

## 업로드된 파일 관리
{: #intent-recommendations-manage-files}

하나 이상의 파일을 업로드한 후 *사용자 예제 소스 파일* 콜렉션을 열어 파일을 관리할 수 있습니다. 업로드된 파일이 현재 {{site.data.keyword.conversationshort}} 서비스 인스턴스에서 공유되는 파일 콜렉션에 추가됩니다.

1.  콜렉션에 액세스하려면 클릭하여 인텐트를 열고 **권장사항 표시**를 클릭한 후 사이드바에서 **파일 보기**를 클릭하십시오.

1.  다음 조치를 수행할 수 있습니다.

    - 파일을 추가하려면 **파일 추가**를 클릭한 후 파일을 찾아서 선택하십시오.
    - 파일을 삭제하려면 이 서비스 인스턴스와 연관된 스킬에서 업로드된 모든 파일을 제거해야 합니다. 하나의 파일만 삭제할 수는 없습니다. 먼저, 아무도 파일을 사용 중이지 않은지 확인하고 **모두 삭제**를 클릭하여 업로드된 파일을 모두 삭제하십시오.

1.  *사용자 예제 소스 파일* 페이지를 닫으십시오.
