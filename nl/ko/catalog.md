---

copyright:
  years: 2015, 2019
lastupdated: "2019-02-28"

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

# 컨텐츠 카탈로그 사용
{: #catalog}

***컨텐츠 카탈로그***는 {{site.data.keyword.conversationshort}} 대화 스킬에 공통 인텐트를 추가하는 쉬운 방법을 제공합니다.
{: shortdesc}

카탈로그에서 추가하는 인텐트는 시작점을 제공하기 위한 것입니다. 카탈로그 인텐트에 추가하거나 편집하여 유스 케이스에 맞게 사용자 조정하십시오.

## 대화 스킬에 컨텐츠 카탈로그 추가
{: #catalog-add}

{{site.data.keyword.conversationshort}} 도구를 사용하여 컨텐츠 카탈로그를 추가하십시오.

1.  {{site.data.keyword.conversationshort}} 도구에서 대화 스킬을 연 다음 **컨텐츠 카탈로그** 탭을 클릭하십시오.

1.  컨텐츠 카탈로그(예: *Banking*)를 선택하여 함께 제공되는 인텐트를 확인하십시오.

    ![사용 가능한 카탈로그를 표시하는 화면 캡처](images/catalog_overview.png)

    카탈로그에 포함된 인텐트에 대한 정보가 표시됩니다.

    ![Banking 카테고리 인텐트를 표시하는 화면 캡처](images/catalog_open.png)

    컨텐츠 카탈로그에서 추가된 인텐트는 이름에 따라 다른 인텐트와 구별할 수 있습니다. 각 인텐트 이름의 앞에는 컨텐츠 카탈로그 이름이 추가됩니다.

1.  ![닫기 화살표](images/close_arrow.png)를 선택하여 **컨텐츠 카탈로그** 탭으로 돌아가십시오.

1.  다음으로 `스킬에 추가` 단추를 클릭하여 대화 스킬에 컨텐츠 카탈로그를 추가하십시오.

1.  이제 **인텐트** 탭을 선택한 후 카탈로그의 인텐트가 추가되었고 사용 가능한지 확인하십시오.

    ![인텐트 탭에 나열된 Banking 인텐트를 표시하는 화면 캡처](images/catalog_intents.png)

시스템에서 새 데이터에 대한 자체 훈련을 시작합니다.

카탈로그를 스킬에 추가하면 인텐트가 훈련 데이터의 일부가 됩니다. IBM에서 컨텐츠 카탈로그에 대한 후속 업데이트를 수행하는 경우 카탈로그에서 추가한 인턴트에 변경사항이 자동으로 적용되지 않습니다.
{: note}

## 컨텐츠 카탈로그 예제 편집
{: #catalog-edit-content}

다른 인텐트와 마찬가지로, 컨텐츠 카탈로그 인텐트를 스킬에 추가한 후 다음과 같이 변경할 수 있습니다.

- 인텐트의 이름을 바꿉니다.
- 인텐트를 삭제합니다.
- 예제를 추가하거나 편집하거나 삭제합니다.
- 다른 인텐트로 예제를 이동합니다.
