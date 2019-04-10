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

# 데이터 백업 및 복원
{: #backup}

데이터를 내보낸 후 가져와서 데이터를 백업하고 복원하십시오.
{: shortdesc}

{{site.data.keyword.conversationshort}} 서비스 인스턴스에서 다음 데이터를 내보낼 수 있습니다.

- 대화 스킬 훈련 데이터(인텐트 및 엔티티)
- 대화 스킬 대화

다음 데이터는 내보낼 수 없습니다.

<!--- Search skill -->
- 어시스턴트(구성된 통합 포함)

## 로그 보유
{: #backup-retain-logs}

사용자가 어시스턴트와 수행한 대화의 로그를 저장하려는 경우 `/logs` API를 사용하여 로그 데이터를 내보낼 수 있습니다. 세부사항은 [API 참조 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://cloud.ibm.com/apidocs/assistant#list-log-events-in-a-workspace)를 참조하십시오.

스킬에 대한 작업공간 ID를 가져오려면 스킬 타일에서 ![옵션 목록 열기 및 닫기](images/kabob-beta.png) 아이콘을 클릭한 후 **API 세부사항 보기**를 선택하십시오.
{: tip}

서비스 플랜에 따라 다른 시간 동안의 로그가 저장됩니다. 예를 들어, Lite 플랜은 지난 7일 동안의 로그만 제공합니다. 자세한 정보는 [로그 한계](/docs/services/assistant?topic=assistant-logs#logs-limits)를 참조하십시오.

로그를 하나의 스킬에서 다른 스킬로 가져올 수 없습니다.

## 대화 스킬 내보내기
{: #backup-export-skill}

대화 스킬 데이터를 백업하려면 스킬을 JSON 파일로 내보내고 JSON 파일을 저장하십시오.

1.  스킬을 사용하는 어시스턴트의 구성 페이지 또는 스킬 페이지에서 대화 스킬 타일을 찾으십시오.

1.  ![옵션 목록 열기 및 닫기](images/kabob-beta.png) 아이콘을 클릭한 후 **JSON 다운로드**를 선택하십시오.

1.  JSON 파일의 이름과 저장할 위치를 지정한 후 **저장**을 클릭하십시오.

또는 `/workspaces` API를 사용하여 대화 스킬을 내보낼 수 있습니다. GET 작업공간 요청과 함께 `export=true` 매개변수를 포함시키십시오. 세부사항은 [API 참조 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://cloud.ibm.com/apidocs/assistant#get-information-about-a-workspace)를 참조하십시오.

## 대화 스킬 가져오기
{: #backup-import-skill}

다른 서비스 인스턴스 또는 환경에서 내보낸 대화 스킬의 백업 사본을 복구하려면 내보낸 대화 스킬의 JSON 파일을 가져와서 새 대화 스킬을 작성하십시오.

클라우드에서 호스팅되는 지속적 딜리버리 환경에서 정기적으로 인스턴스에 적용되는 기능 업데이트로 인해 스킬을 내보내는 시간과 가져오는 시간 사이에 {{site.data.keyword.conversationshort}} 서비스가 변경된 경우 가져온 스킬이 이전과 다르게 작동할 수 있습니다.
{: important}

1.  **스킬** 탭을 클릭하십시오.

1.  **새로 작성**을 클릭하십시오.

1.  **스킬 가져오기**를 클릭한 다음 **JSON 파일 선택**을 클릭하고 가져올 JSON 파일을 선택하십시오.

    **중요:**

    - 가져온 JSON 파일은 BOM(Byte Order Mark) 인코딩을 사용하지 않고 UTF-8 인코딩을 사용해야 합니다.
    - 스킬 JSON 파일의 최대 크기는 10MB입니다. 더 큰 스킬을 가져와야 하는 경우 스킬을 가져온 후에 별도로 인텐트와 엔티티를 가져오십시오. (REST API를 사용하여 더 큰 스킬을 가져올 수도 있습니다. 자세한 정보는 [API 참조 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://cloud.ibm.com/apidocs/assistant#create-workspace){: new_window}를 참조하십시오.)
    - JSON 파일에는 탭, 줄 바꾸기 또는 캐리지 리턴이 포함될 수 없습니다.

    내보낸 스킬의 전체 사본을 가져오려면 **모두(인텐트, 엔티티 및 대화)**를 선택하십시오.

    **가져오기**를 클릭하십시오. 

    스킬을 가져오는 데 문제가 있는 경우 [스킬 가져오기 문제 해결](/docs/services/assistant?topic=assistant-skill-add#skill-add-import-errors)을 참조하십시오.

1.  스킬에 대한 세부사항을 지정하십시오.

    - **이름**: 길이가 100자 이하인 이름입니다. 이름은 필수입니다.
    - **설명**: 길이가 200자 이하인 선택적 설명입니다.
    - **언어**: 이해를 위해 스킬을 훈련하는 데 사용할 사용자 입력 언어입니다. 기본값은 영어입니다.

스킬을 작성하면 스킬 페이지에 타일로 표시됩니다.

## 어시스턴트 다시 작성
{: #backup-recreate-assistant}

이제 어시스턴트를 다시 작성할 수 있습니다. 그런 다음, 가져온 대화 스킬을 어시스턴트에 링크하고 이에 대한 통합을 구성할 수 있습니다.

세부사항은 [어시스턴트 작성](/docs/services/assistant?topic=assistant-assistant-add) 및 [통합 추가](/docs/services/assistant?topic=assistant-deploy-integration-add#deploy-integration-add-task)를 참조하십시오.

## 클라이언트 애플리케이션 업데이트
{: #backup-update-api}

내보낸 대화 스킬을 가져오면 새 스킬이 작성됩니다. 새 스킬에는 새 작업공간 ID가 있습니다. v1 API를 사용하여 이 스킬에 액세스하는 기존 클라이언트 애플리케이션이 있는 경우 대신 새 작업공간 ID를 사용하도록 작업공간 ID 참조를 업데이트해야 합니다.

어시스턴트를 다시 작성하면 새 어시스턴트 ID가 지정됩니다. v2 API를 사용하여 어시스턴트에 액세스하는 기존 클라이언트 애플리케이션이 있는 경우 대신 새 어시스턴트 ID를 사용하도록 어시스턴트 ID 참조를 업데이트해야 합니다.
