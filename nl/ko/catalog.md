---

copyright:
  years: 2015, 2018
lastupdated: "2018-02-13"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:swift: .ph data-hd-programlang='swift'}

# 카탈로그 사용
{: #catalog}

***카탈로그***는 {{site.data.keyword.conversationshort}} 서비스 작업공간에 공통 인텐트를 추가하는 쉬운 방법을 제공합니다.
{: shortdesc}

## 작업공간에 카탈로그 추가
{: #add-catalog}

카탈로그를 추가하려면 {{site.data.keyword.conversationshort}} 도구를 사용하십시오.

1.  {{site.data.keyword.conversationshort}} 도구에서 작업공간을 연 다음 탐색줄에서 **카탈로그** 탭을 선택하십시오. **카탈로그**가 표시되지 않으면 ![메뉴](images/Menu_16.png) 메뉴를 사용하여 페이지를 여십시오.

1.  *청구*와 같은 카탈로그를 선택하여 함께 제공되는 인텐트를 확인하십시오.

    ![사용 가능한 카탈로그를 표시하는 화면 캡처](images/catalog_overview.png)

    *청구* 카탈로그에서 정의된 인텐트에 관한 정보가 표시됩니다.

    ![청구 카테고리 인텐트를 표시하는 화면 캡처](images/catalog_open.png)

    카탈로그에서 추가된 인텐트는 이름 지정 규칙에 따라 다른 인텐트와 구별할 수 있습니다. 예:`#Billing_ . . .`

1.  **카탈로그** 탭으로 돌아가려면, 선택하십시오.![닫기 화살표](images/close_arrow.png)

1.  다음으로, `봇에 추가` 단추를 클릭하여 작업공간에 *청구*를 추가하십시오. 작업공간에 *청구* 인텐트가 추가되었음을 나타내는 메시지가 표시됩니다.

    ![봇에 추가 단추를 표시하는 화면 캡처](images/catalog_addtobot.png)

1.  이제, **인텐트** 탭을 선택하여, 작업공간에 추가된 *청구* 인텐트를 확인하십시오.

    ![인텐트 탭에 나열된 청구 인텐트를 표시하는 화면 캡처](images/catalog_intents.png)

### 결과

*청구* 카탈로그의 인텐트가 작업공간의 **인텐트** 탭에 추가되고 시스템이 새 데이터에 대해 자체 훈련을 시작합니다.

## 카탈로그 예제 편집

다른 인텐트와 마찬가지로, *청구* 카탈로그의 인텐트가 작업공간에 추가되면 다음과 같이 변경할 수 있습니다.

- 인텐트의 이름을 바꿉니다.
- 인텐트를 삭제합니다.
- 예제를 추가하거나 편집하거나 삭제합니다.
- 다른 인텐트로 예제를 이동합니다.

인텐트 이름에서 각 예제로 탭을 사용하여 이동할 수 있으며, 예제(선택하는 경우)를 편집할 수 있습니다.

예제를 이동하거나 삭제하려면, 선택란을 선택하여 예제를 선택한 다음 **이동** 또는 **삭제**를 선택하십시오.

  ![예제를 이동하거나 삭제하는 방법을 표시하는 화면 캡처](images/catalog_edit.png)
