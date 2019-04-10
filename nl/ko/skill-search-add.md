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

# 검색 스킬 빌드
{: #skill-search-add}

어시스턴트는 *검색 스킬*을 사용하여 복잡한 고객 문의를 {{site.data.keyword.discoveryfull}} 서비스로 라우팅합니다.
{{site.data.keyword.discoveryshort}}에서 사용자 입력을 검색 조회로 처리합니다. 외부 데이터 소스에서 조회와 관련된 정보를 찾아서 이를 어시스턴트에게 리턴합니다.
{: shortdesc}

이 기능은 베타 프로그램에 참여한 사용자만 사용할 수 있습니다. 액세스 권한을 요청하는 방법을 알아보려면 [베타 프로그램에 참여](/docs/services/assistant?topic=assistant-feedback#feedback-beta)를 참조하십시오.

![베타](images/beta.png): IBM에서 베타로 분류하여 릴리스한 서비스, 기능 및 언어 지원입니다. 이러한 기능은 불안정하고 자주 변경될 수 있으며 짧은 통보로 중단될 수 있습니다. 베타 기능은 일반적으로 제공되는 기능과 동일한 수준의 성능이나 호환성을 제공하지 않을 수도 있으며 프로덕션 환경에서 사용하기 위한 것이 아닙니다.

하나의 어시스턴트에 하나의 검색 스킬을 추가할 수 있습니다. 플랜별 한계에 대한 정보는 [스킬 한계](/docs/services/assistant?topic=assistant-skill-add#skill-add-limits)를 참조하십시오.

검색 스킬은 {{site.data.keyword.discoveryshort}} 서비스를 사용하여 작성한 데이터 콜렉션에서 정보를 검색합니다.
{{site.data.keyword.discoveryshort}}는 구조화되지 않은 데이터를 크롤링, 변환 및 표준화하는 서비스입니다. 이 서비스는 데이터 분석 및 인지 직관을 적용하여 나중에 이 정보를 쉽게 찾고 검색할 수 있도록 데이터를 향상시킵니다.
{{site.data.keyword.discoveryshort}}에 대해 알아보려면 [제품 문서 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](/docs/services/discovery?topic=discovery-about)를 참조하십시오.

다음은 {{site.data.keyword.discoveryfull}} 서비스가 트리거되는 방식입니다.

- **다른 노드**: 사용자의 쿼리를 처리할 수 있는 대화 노드가 없는 경우 외부 데이터 소스에서 관련 응답을 검색합니다. 표준 메시지(예: `I don't know how to help you with that.`)를 표시하는 대신, 어시스턴트는 `Maybe this information can help:`라고 말한 후에 검색에서 리턴한 구절을 추가할 수 있습니다. 검색 스킬이 어시스턴트와 연결되어 있는 경우, `anything_else` 노드가 트리거될 때마다 노드 응답을 표시하지 않고 대신 검색을 수행합니다. 어시스턴트는 사용자 입력을 조회로 검색 스킬에 전달하고 검색 결과를 응답으로 리턴합니다.
- **검색 응답 유형**: 검색 응답 유형을 대화 노드에 추가하는 경우, 서비스는 외부 데이터 소스에서 구절을 검색하여 이를 특정 질문에 대한 응답으로 리턴합니다. 이 검색 유형은 개별 대화 노드가 처리될 때만 발생합니다. 이 접근 방법은 검색을 수행하기 전에 사용자 조회를 축소하려는 경우에 유용합니다. 예를 들어, 대화 분기는 고객이 구매하려는 장치 유형에 대한 정보를 수집할 수 있습니다. 제조업체 및 모델을 아는 경우, 제출된 조회의 모델 키워드를 검색 스킬에 전송하면 더 나은 결과를 얻을 수 있습니다.
- **검색 스킬 전용**: 검색 스킬만 어시스턴트에 링크되어 있고 어시스턴트에 연결된 대화 스킬이 없는 경우, 사용자 입력이 어시스턴트의 통합 채널 중 하나에서 수신되면 검색 조회가 {{site.data.keyword.discoveryshort}} 서비스에 제출됩니다. 

## 검색 스킬 작성
{: #skill-search-add-task}

이를 수행하지 않은 경우, [시작하기 튜토리얼](/docs/services/assistant?topic=assistant-getting-started#getting-started-prerequisites)에서 필수 단계를 완료하여 {{site.data.keyword.conversationshort}} 서비스 인스턴스를 작성하고 {{site.data.keyword.conversationshort}} 도구를 실행하십시오.

{{site.data.keyword.conversationshort}} 도구를 사용하여 스킬을 작성합니다. 스킬을 작성하려면 다음 단계를 수행하십시오.

1.  **스킬** 탭을 클릭하십시오. 

1.  **새로 작성**을 클릭하십시오.

1.  *검색 스킬*을 작성하려면 **추가**를 클릭하십시오.

1.  새 스킬의 세부사항을 지정하십시오.
    - **이름**: 길이가 100자 이하인 이름입니다. 이름은 필수입니다.
    - **설명**: 길이가 200자 이하인 선택적 설명입니다.

1.  **작성**을 클릭하십시오.

나머지 단계는 작성된 콜렉션이 있는 기존 {{site.data.keyword.discoveryshort}} 서비스 인스턴스에 대한 액세스 권한이 있는지 여부에 따라 다릅니다. 상황에 적합한 프로시저를 따르십시오.

- [기존 Watson Discovery 인스턴스에 연결](#skill-search-add-connect-discovery)
- [Watson Discovery 인스턴스 작성](#skill-search-add-create-discovery)

## 기존 Watson Discovery 서비스 인스턴스에 연결
{: #skill-search-add-connect-discovery}

1.  정보를 추출하려는 {{site.data.keyword.discoveryshort}} 서비스 인스턴스를 선택하십시오.
{: #choose-d-instance}

    액세스 권한이 있는 {{site.data.keyword.discoveryshort}} 서비스 인스턴스가 목록에 표시됩니다.

    일부 {{site.data.keyword.discoveryshort}} 서비스 인스턴스에 인증 정보가 설정되지 않았다는 경고가 표시되는 경우, {{site.data.keyword.cloud_notm}} 대시보드에서 직접 바로 열지 않은 하나 이상의 인스턴스에 액세스할 수 있음을 나타냅니다. 작성할 인증 정보에 대한 서비스 인스턴스에 액세스해야 합니다. 인증 정보는 {{site.data.keyword.conversationshort}}에서 사용자를 대신하여 {{site.data.keyword.discoveryshort}} 서비스 인스턴스에 대한 연결을 설정하기 전에 존재해야 합니다. {{site.data.keyword.discoveryshort}} 서비스 인스턴스가 목록에 없지만 있어야 한다고 생각하는 경우, {{site.data.keyword.cloud_notm}} 대시보드에서 인스턴스를 바로 열어 해당 인스턴스에 대한 인증 정보를 생성하십시오.

1.  다음 중 하나를 수행하여 사용할 데이터 콜렉션을 표시하십시오.
{: #pick-data-collection}

    - 기존 데이터 콜렉션을 선택하십시오.

      *Discovery에서 열기* 링크를 클릭하여 사용할 항목을 정하기 전에 데이터 콜렉션의 구성을 검토하십시오.

      [검색 구성](#beta-search-skill-add-configure)으로 이동하십시오.

    - 콜렉션이 없거나 나열된 데이터 콜렉션을 사용하지 않으려는 경우, **새 콜렉션 작성**을 클릭하여 콜렉션을 추가하십시오. [데이터 콜렉션 작성](#beta-search-skill-add-create-discovery-collection)의 프로시저에 따르십시오.

      {{site.data.keyword.discoveryshort}} 서비스 플랜에 따라 작성할 수 있는 콜렉션 수 한계에 도달한 경우에는 **새 콜렉션 작성** 단추가 표시되지 않습니다. 플랜 제한 세부사항은 [{{site.data.keyword.discoveryshort}} 가격 플랜 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](/docs/services/discovery/discovery-about?topic=discovery-discovery-pricing-plans)을 참조하십시오.

## Watson Discovery 서비스 인스턴스 작성
{: #skill-search-add-create-discovery}

1.  {{site.data.keyword.discoveryshort}} 서비스 인스턴스를 작성하려면 **새 콜렉션 작성**을 클릭하십시오.       

    {{site.data.keyword.discoveryshort}} 서비스의 인스턴스가 사용자를 위해 작성되며 구성 페이지는 새 {{site.data.keyword.discoveryshort}} 서비스 인스턴스에 열립니다.

    사용하는 {{site.data.keyword.conversationshort}} 서비스 플랜에 관계 없이, 서비스의 Lite 플랜 인스턴스가 {{site.data.keyword.Bluemix_notm}}에 프로비저닝됩니다. {{site.data.keyword.discoveryshort}} 서비스 인스턴스를 다른 플랜의 일부로 작성하려면 여기에서 중지하십시오. [{{site.data.keyword.Bluemix_notm}} 카탈로그 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://cloud.ibm.com/catalog/services/discovery)에서 서비스 인스턴스를 바로 작성하고 대신 [기존 {{site.data.keyword.discoveryshort}} 서비스 인스턴스에 연결](#skill-search-add-connect-discovery) 프로시저를 따르십시오.
    {: important}

1.  인스턴스 사용에 대한 이용 약관을 검토한 다음, 계속하려면 **동의**를 클릭하십시오.

1.  [새 데이터 콜렉션을 작성](#skill-search-add-create-discovery-collection)하십시오.

## 데이터 콜렉션 작성
{: #skill-search-add-create-discovery-collection}

사전에 보강된 데이터 소스 *Watson Discovery News*를 인스턴스에 추가하지 마십시오. 이는 {{site.data.keyword.conversationshort}}에서 검색할 수 있는 데이터 유형이 아닙니다.
{: important}

1.  {{site.data.keyword.discoveryshort}} 콜렉션을 작성하려면 다음 중 하나를 수행하십시오.

      - {{site.data.keyword.discoveryshort}}에서 기본 제공 지원을 제공하는 데이터 소스 유형에 저장된 데이터에서 콜렉션을 작성하려면 **데이터 소스에 연결**을 클릭하십시오.

        1.  데이터 소스 유형을 선택하십시오.
        1.  선택한 데이터 소스의 필수 정보를 제공한 후 **연결**을 클릭하십시오.

            자세한 내용은 [데이터 소스에 연결 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](/docs/services/discovery?topic=discovery-sources)을 참조하십시오.
        1.  {{site.data.keyword.discoveryshort}}에 작성 중인 콜렉션과 동기화할 데이터 소스의 데이터를 원하는 빈도를 표시하십시오.
        1.  데이터 소스에서 추출할 정보를 지정하고 {{site.data.keyword.discoveryshort}} 콜렉션에 포함하십시오.

            표시되는 옵션은 데이터 소스 유형에 따라 다릅니다.

            - Salesforce 데이터 소스의 경우, 소스 문서에서 추출할 오브젝트 유형을 선택합니다. *케이스*(예: 고객의 문제)를 나타내는 [케이스 오브젝트 유형 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://developer.salesforce.com/docs/atlas.en-us.object_reference.meta/object_reference/sforce_api_objects_case.htm#!)을 선택합니다.
            - Sharepoint 데이터 소스의 경우, 경로를 지정합니다.
            - 파일 저장소의 경우, 디렉토리 또는 파일을 지정합니다.

        1.  **데이터 저장 및 동기화**를 클릭하십시오.

            데이터 콜렉션이 작성됩니다. 프로세스가 완료되면 요약 페이지가 별도의 웹 브라우저 탭에서 {{site.data.keyword.discoveryshort}} 도구에 표시됩니다.
        1.  {{site.data.keyword.conversationshort}} 도구로 돌아가려면 **{{site.data.keyword.conversationshort}}에서 스킬 구성**을 클릭하십시오. 

      - 문서를 업로드하여 콜렉션을 작성하려면 **사용자 데이터 업로드**를 클릭하십시오.     

        1.  먼저 콜렉션을 정의한 후 문서를 업로드합니다. 다음 정보를 제공하십시오. 

            - 콜렉션 이름. 이 서비스 인스턴스의 이름은 고유해야 합니다.
            - 구성. 기본 구성 템플리트 또는 저장된 구성을 사용할지를 선택할 수 있습니다. 구성에 대한 자세한 내용은 [서비스 구성 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](/docs/services/discovery?topic=discovery-configservice)을 참조하십시오.
            - 언어. 이 콜렉션에 추가할 파일의 언어를 선택하십시오. {{site.data.keyword.discoveryshort}}에서 지원하는 언어에 대한 자세한 내용은 [언어 지원 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](/docs/services/discovery?topic=discovery-language-support)을 참조하십시오.
        1.  문서를 업로드합니다.

            지원되는 파일 유형에는 PDF, HTML, JSON 및 DOC 파일이 있습니다. 자세한 내용은 [컨텐츠 추가![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](/docs/services/discovery?topic=discovery-addcontent)를 참조하십시오.
            {: note}

            업로드된 문서를 지속적으로 동기화할 수 없습니다. 문서에 대한 변경사항을 선택하려면 문서의 최신 버전을 업로드하십시오.

{{site.data.keyword.conversationshort}}로 돌아가기 전에 콜렉션이 완전히 수집될 때까지 기다리십시오.

## 검색 구성
{: #skill-search-add-configure}

1.  {{site.data.keyword.conversationshort}} 검색 스킬 페이지에서 **구성**을 클릭하십시오.

1.  검색 성공에 따라 다른 메시지를 사용자와 공유하도록 초안을 작성하십시오.

    <table>
    <caption>검색 결과 메시지</caption>
    <tr>
      <th>필드 이름</th>
      <th>시나리오</th>
      <th>예제 메시지</th>
    </tr>
    <tr>
      <td>메시지</td>
      <td>검색 결과가 리턴됩니다.</td>
      <td>도움이 될 수 있는 정보를 찾았습니다. </td>
    </tr>
    <tr>
      <td>검색 결과 없음</td>
      <td>검색 결과를 찾을 수 없습니다.</td>
      <td>사용자의 쿼리를 처리할 수 있는 정보를 찾기 위해 지식 베이스를 검색했지만, 공유하는 데 유용한 내용은 찾지 못했습니다.</td>
    </tr>
    <tr>
      <td>오류 메시지</td>
      <td>어떤 이유로 인해 서비스에서 검색을 수행할 수 없음</td>
      <td>사용자 문의를 처리할 수 있는 정보가 있을 수 있지만, 현재 내 지식 베이스를 검색할 수 없습니다.</td>
    </tr>
    </table>

1.  텍스트를 추출할 {{site.data.keyword.discoveryshort}} 서비스 인스턴스를 선택하십시오.

    사용 가능한 필드는 수집한 데이터와 이를 수집하는 데 사용한 구성에 따라 다릅니다.

    각 검색 결과는 다음 정보로 구성될 수 있습니다.

    - **제목**: 검색 결과 제목. 콜렉션에서 제목, 이름 또는 유사한 유형의 필드를 검색 결과 제목으로 사용하십시오.

      응답을 표시하려면 Facebook과 Slack 통합에 대해 `없음`이 아닌 항목을 선택해야 합니다.
    - **본문**: 검색 결과 설명. 콜렉션에서 요약 또는 강조표시 필드를 검색 결과 본문으로 사용하십시오.

      응답을 표시하려면 Facebook과 Slack 통합에 대해 `없음`이 아닌 항목을 선택해야 합니다.
    - **Url**: 원시 데이터 소스에 있는 원래 데이터 오브젝트에 대한 하이퍼텍스트 링크입니다. 대부분의 온라인 데이터 소스는 직접 액세스를 지원하기 위해 상점의 오브젝트에 대한 자체 참조 공용 URL을 제공합니다.

      Slack 통합으로 응답에 URL을 포함하고 Facebook 통합으로 응답을 표시하려면 결과 URL이 올바르고 접근 가능해야 합니다. `없음`은 Facebook 및 Slack 통합에 허용할 수 있는 선택사항입니다.

    도움말을 보려면 [콜렉션 필드 선택에 대한 팁](#skill-search-add-field-tips)을 참조하십시오.
  
    옵션 중 하나 이상에 대해 `없음`이 아닌 값을 선택해야 합니다.

    드롭 다운 필드에서 옵션을 사용할 수 없는 경우, 콜렉션 작성을 완료하는 데 {{site.data.keyword.discoveryshort}}에 더 많은 시간을 제공해야 합니다. 그렇지 않은 경우 콜렉션에 문서가 포함되지 않거나 우선 처리해야 하는 수집 오류가 발생할 수 있습니다.

1.  미리보기 분할창에서 테스트 메시지를 입력하여 구성 선택사항이 검색에 적용될 때 리턴되는 결과를 확인하십시오. 필요에 따라 조정하십시오.

1.  **작성**을 클릭하십시오.

나중에 구성을 변경하려면 검색 스킬을 다시 열고 편집하십시오. 변경할 때 변경사항을 저장하지 않아도 됩니다. 자동으로 적용됩니다. 검색 결과에 만족하는 경우 **저장**을 클릭하여 검색 스킬 구성을 완료하십시오.

## 다음 단계
{: #skill-search-add-next-steps}

스킬을 작성하면 스킬 페이지에 타일로 표시됩니다.

검색 스킬은 어시스턴트에 추가되고 어시스턴트가 배치될 때까지 고객과 상호작용할 수 없습니다. [어시스턴트 작성](/docs/services/assistant?topic=assistant-assistant-add)을 참조하십시오.

대화 스킬 및 검색 스킬을 모두 어시스턴트에 연결하는 경우, 사용자 입력이 해당 대화 스킬에서 처리되어 대화 노드에서 처리할 수 없는 경우 검색 스킬이 자동으로 트리거됩니다. `anything_else` 노드에서 일반 응답으로 회신하는 대신, 사용자 응답을 조회 문자열로 사용하는 검색이 시작됩니다. 

필요한 경우 특정 노드 조건에 대한 응답으로 호출할 특정 검색 조회를 정의할 수 있습니다. 이를 수행하려면 대화 노드에 검색 응답 유형을 추가하십시오. 자세한 내용은 [응답](/docs/services/assistant?topic=assistant-dialog-overview#dialog-overview-multimedia)을 참조하십시오.

대화 스킬에서 모든 유형의 검색을 시작하는 경우, 검색이 예상대로 트리거되는지 확인하려면 대화를 테스트하십시오. 예를 들어, 검색 응답 유형을 사용하지 않는 경우, 기존 대화 노드가 사용자 입력을 처리할 수 없는 경우에만 검색이 트리거되는지 테스트하십시오. 검색이 트리거될 때마다 의미 있는 결과를 리턴하는지 확인하십시오.

### 콜렉션 필드 선택에 대한 팁
{: #skill-search-add-field-tips}

데이터를 추출할 적절한 콜렉션 필드는 데이터 소스를 수집하는 데 사용한 콜렉션의 데이터 소스 및 구성에 따라 달라집니다. 추출하려는 정보를 포함하는 필드의 이름을 포함하여 콜렉션에 있는 문서의 구조에 대해 자세히 알아보려면, {{site.data.keyword.discoveryshort}} 도구에서 콜렉션을 열고 **데이터 스키마 보기**를 클릭하십시오.

다음 표는 시작할 때 시도할 수 있는 콜렉션 필드를 제공합니다. 이 제안사항은 콜렉션을 작성할 때 기본 구성 템플리트를 사용했다고 가정합니다.

| 데이터 소스 유형   | 제목  | 본문 | Url |
|--------------------|-------|------|-----|
| 업로드한 PDF 문서  | enriched_text.concepts.text | text |없음 |
| Box                | name | description | listing_url |

콜렉션이 작성될 때 콜렉션 필드가 작성됩니다. 수집을 위해 기본 구성 템플리트(예: `enriched_text.concepts.text`)를 사용할 때 생성되는 필드에 대해 자세히 알아보려면 [서비스 구성 > 보강 추가 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](/docs/services/discovery?topic=discovery-configservice#adding-enrichments)를 참조하십시오.

### 어시스턴트에 스킬 추가
{: #skill-search-add-to-assistant}

하나의 어시스턴트에 하나의 스킬을 추가할 수 있습니다. 어시스턴트 구성 페이지에서 어시스턴트 타일을 열고 어시스턴트에 스킬을 추가해야 합니다. 스킬 구성 페이지에서 스킬을 사용할 어시스턴트를 선택할 수 없습니다.

하나 이상의 어시스턴트에서 하나의 검색 스킬을 사용할 수 있습니다.

1.  어시스턴트 탭에서 스킬을 추가하려는 어시스턴트에 사용하기 위해 타일을 클릭하여 엽니다.

1.  **검색 스킬 추가**를 클릭합니다.

1.  **기존 스킬 추가**를 클릭합니다.

    표시되는 사용 가능한 스킬에서 추가할 스킬을 클릭합니다.

하나 이상의 테스트 통합 채널을 구성합니다. 시험 사용 분할창에서 검색 스킬을 테스트할 수 없습니다. 검색을 트리거하는 조회를 입력하여 통합 채널에서 스킬을 테스트합니다. 검색이 올바르게 트리거되고 관련 결과를 리턴하는지 확인합니다.

검색 스킬이 있는 어시스턴트에 대해 공유 가능한 링크 통합은 현재 작동하지 않습니다.
{: important}
