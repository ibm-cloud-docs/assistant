---

copyright:
  years: 2015, 2019
lastupdated: "2019-08-02"

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

# 검색 스킬 작성 ![Plus 또는 Premium 플랜만 해당](images/plus.png)
{: #skill-search-add}

어시스턴트는 *검색 스킬*을 사용하여 복잡한 고객 문의를 {{site.data.keyword.discoveryfull}} 서비스로 라우팅합니다. {{site.data.keyword.discoveryshort}}에서 사용자 입력을 검색 조회로 처리합니다. 외부 데이터 소스에서 조회와 관련된 정보를 찾아서 이를 어시스턴트에게 리턴합니다.
{: shortdesc}

이 기능은 Plus 또는 Premium 플랜 사용자만 사용할 수 있습니다.
{: note}

어시스턴트가 다음과 같이 말하지 않도록 검색 스킬을 추가하십시오. `I'm sorry. I can't help you with that`. 그 대신, 어시스턴트는 기존 회사 문서나 데이터를 조회하여 유용한 정보를 찾아서 고객과 공유할 수 있는지 여부를 확인할 수 있습니다.

![미리보기 링크 통합의 검색 결과를 표시합니다.](images/search-skill-preview-link.png)

다음 4분짜리 동영상은 검색 스킬에 대한 개요를 제공합니다.

<iframe class="embed-responsive-item" id="youtubeplayer" title="검색 스킬 개요" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/ZcgGf8J2Cfw?rel=0" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

검색 스킬이 비즈니스에 도움을 주는 방법에 대해 자세히 알아보려면 [이 블로그 게시물![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://medium.com/ibm-watson/adding-search-to-watson-assistant-99e4e81839e5){: new_window}을 읽으십시오. 

## 작동 방법
{: #skill-search-add-how}

검색 스킬은 {{site.data.keyword.discoveryshort}} 서비스를 사용하여 작성한 데이터 콜렉션에서 정보를 검색합니다.

{{site.data.keyword.discoveryshort}}는 구조화되지 않은 데이터를 크롤링, 변환 및 표준화하는 서비스입니다. 제품은 데이터 분석 및 인지 직관을 적용하여 나중에 이 정보를 쉽게 찾고 검색할 수 있도록 데이터를 향상시킵니다. {{site.data.keyword.discoveryshort}}에 대해 알아보려면 [제품 문서 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](/docs/services/discovery?topic=discovery-about){: new_window}를 참조하십시오.

일반적으로, 사용자가 {{site.data.keyword.discoveryshort}}에 추가하고 어시스턴트에서 액세스하는 데이터 콜렉션 유형에는 회사가 소유한 정보가 포함됩니다. 이 독점적 정보는 주제별 전문가가 작성한 보고서, 기술 메뉴얼, 영업 자료 또는 자주 묻는 질문(FAQ)을 포함할 수 있습니다. 고객의 질문에 대한 답변을 신속하게 찾을 수 있도록 이 고밀도 독점 정보 콜렉션을 마이닝하십시오. 

다음 다이어그램은 대화 스킬 및 검색 스킬 둘 다가 어시스턴트에 추가될 때 사용자 입력이 처리되는 방법을 보여줍니다.

![대화가 사용자 입력에 응답하고 검색이 다른 질문에 응답하는 방법을 보여주는 다이어그램](images/search-skill-diagram.png)

## 시작하기 전에
{: #skill-search-add-prereqs}

{{site.data.keyword.discoveryshort}} 서비스 인스턴스가 없는 경우에는 이 프로세스의 일부로 무료 Lite 플랜 인스턴스가 프로비저닝됩니다. 기존 {{site.data.keyword.discoveryshort}} 서비스 인스턴스가 있으면 연결하십시오. 이 프로세스의 일부로 새 인스턴스를 작성하지 않습니다. 

먼저 Discovery 인스턴스를 작성하는 경우 *Watson Discovery News*라는 사전 보강된 데이터 소스를 사용자의 인스턴스에 추가하지 마십시오. 이는 {{site.data.keyword.conversationshort}}에서 검색할 수 있는 데이터 유형이 아닙니다.
{: tip}

## 검색 스킬 작성
{: #skill-search-add-task}

1.  **스킬** 탭을 클릭한 후 **스킬 작성**을 클릭하십시오.

1.  *검색 스킬* 타일을 클릭한 후 **다음**을 클릭하십시오.

    Plus 또는 Premium 플랜 사용자인 경우에만 검색 스킬을 선택할 수 있습니다.
    {: note}

1.  새 스킬의 세부사항을 지정하십시오.
    - **이름**: 길이가 100자 이하인 이름입니다. 이름은 필수입니다.
    - **설명**: 길이가 200자 이하인 선택적 설명입니다.

1.  **계속**을 클릭하십시오.

나머지 단계는 작성된 콜렉션이 있는 기존 {{site.data.keyword.discoveryshort}} 서비스 인스턴스에 대한 액세스 권한이 있는지 여부에 따라 다릅니다. 상황에 적합한 프로시저를 따르십시오.

- [기존 Watson Discovery 인스턴스에 연결](#skill-search-add-connect-discovery)
- [Watson Discovery 인스턴스 작성](#skill-search-add-create-discovery)

## 기존 Watson Discovery 서비스 인스턴스에 연결
{: #skill-search-add-connect-discovery}

1.  정보를 추출하려는 {{site.data.keyword.discoveryshort}} 서비스 인스턴스를 선택하십시오.
{: #choose-d-instance}

    액세스 권한이 있는 {{site.data.keyword.discoveryshort}} 서비스 인스턴스가 목록에 표시됩니다.

    일부 {{site.data.keyword.discoveryshort}} 서비스 인스턴스에 인증 정보가 설정되지 않았다는 경고가 표시되는 경우, {{site.data.keyword.cloud_notm}} 대시보드에서 직접 바로 열지 않은 하나 이상의 인스턴스에 액세스할 수 있음을 의미합니다. 작성할 인증 정보에 대한 서비스 인스턴스에 액세스해야 합니다. 인증 정보는 {{site.data.keyword.conversationshort}}에서 사용자를 대신하여 {{site.data.keyword.discoveryshort}} 서비스 인스턴스에 대한 연결을 설정하기 전에 존재해야 합니다. {{site.data.keyword.discoveryshort}} 서비스 인스턴스가 목록에서 누락된 경우, {{site.data.keyword.cloud}} 대시보드에서 인스턴스를 바로 열어 해당 인스턴스에 대한 인증 정보를 생성하십시오.
    {: note}

1.  다음 중 하나를 수행하여 사용할 데이터 콜렉션을 표시하십시오.
{: #pick-data-collection}

    - 기존 데이터 콜렉션을 선택하십시오.

      *Discovery에서 열기* 링크를 클릭하여 사용할 항목을 정하기 전에 데이터 콜렉션의 구성을 검토하십시오.

      [검색 구성](#search-skill-add-configure)으로 이동하십시오.

    - 콜렉션이 없거나 나열된 데이터 콜렉션을 사용하지 않으려는 경우, **새 콜렉션 작성**을 클릭하여 콜렉션을 추가하십시오. [데이터 콜렉션 작성](#search-skill-add-create-discovery-collection)의 프로시저에 따르십시오.

      {{site.data.keyword.discoveryshort}} 서비스 플랜에 따라 작성할 수 있는 콜렉션 수 한계에 도달한 경우에는 **새 콜렉션 작성** 단추가 표시되지 않습니다. 플랜 제한 세부사항은 [{{site.data.keyword.discoveryshort}} 가격 플랜 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](/docs/services/discovery/discovery-about?topic=discovery-discovery-pricing-plans){: new_window}을 참조하십시오.
      {: note}

## Watson Discovery 서비스 인스턴스 작성
{: #skill-search-add-create-discovery}

1.  {{site.data.keyword.discoveryshort}} 서비스 인스턴스를 작성하려면 **새 콜렉션 작성**을 클릭하십시오.

    기존 {{site.data.keyword.discoveryshort}} 서비스 인스턴스가 없는 경우, {{site.data.keyword.discoveryshort}} 서비스의 무료 인스턴스가 작성됩니다.

    보유하고 있는 {{site.data.keyword.conversationshort}} 서비스 플랜 유형에 관계 없이, 서비스의 Lite 플랜 인스턴스가 {{site.data.keyword.Bluemix_notm}}에 프로비저닝됩니다.
    {: note}

1.  인스턴스 사용에 대한 이용 약관을 검토한 다음, 계속하려면 **동의**를 클릭하십시오.

1.  [새 데이터 콜렉션을 작성](#skill-search-add-create-discovery-collection)하십시오.

## 데이터 콜렉션 작성
{: #skill-search-add-create-discovery-collection}

Discovery 서비스 Lite 플랜이 있는 경우 플랜을 업그레이드할 수 있는 기회가 제공됩니다. 지금 바로 업그레이드하지 않으려면 **시작하기**를 클릭하십시오. 

1.  {{site.data.keyword.discoveryshort}} 콜렉션을 작성하려면 다음 중 하나를 수행하십시오.

      - {{site.data.keyword.discoveryshort}}에서 기본 제공 지원을 제공하는 데이터 소스 유형에 저장된 데이터에서 콜렉션을 작성하려면 데이터 소스 유형을 선택하십시오.

        1.  선택한 데이터 소스의 필수 정보를 제공한 후 **연결**을 클릭하십시오.

            자세한 내용은 [데이터 소스에 연결 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](/docs/services/discovery?topic=discovery-sources){: new_window}을 참조하십시오.
        1.  {{site.data.keyword.discoveryshort}}에 작성 중인 콜렉션과 동기화할 데이터 소스의 데이터를 원하는 빈도를 표시하십시오.
        1.  데이터 소스에서 추출할 정보를 지정하고 {{site.data.keyword.discoveryshort}} 콜렉션에 포함하십시오.

            표시되는 옵션은 데이터 소스 유형에 따라 다릅니다.

            - Salesforce 데이터 소스의 경우, 소스 문서에서 추출할 오브젝트 유형을 선택합니다. *케이스*(예: 고객의 문제)를 나타내는 [케이스 오브젝트 유형 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://developer.salesforce.com/docs/atlas.en-us.object_reference.meta/object_reference/sforce_api_objects_case.htm#!)을 선택합니다.
            - Sharepoint 데이터 소스의 경우, 경로를 지정합니다.
            - 파일 저장소의 경우, 디렉토리 또는 파일을 지정합니다.
            - 웹 크롤링 데이터 소스의 경우 크롤링할 웹 사이트의 기본 URL을 지정하십시오. 사용자가 지정하는 웹 페이지 및 해당 웹 페이지가 링크하는 모든 페이지가 크롤링되고 웹 페이지별로 문서가 작성됩니다.

            Watson이 문서 작성을 시작할 시간을 주십시오. 소스가 수집되기 시작하면 {{site.data.keyword.discoveryshort}} 세부사항 페이지에 표시되는 문서 수가 증가합니다. 사용자가 페이지를 새로 고쳐야 합니다. 
            
            데이터 소스 작성에 대해 도움을 받으려면 [문제점 해결](#skill-search-add-troubleshoot)을 참조하십시오.

        1.  **오브젝트 저장 및 동기화**를 클릭하십시오.

            데이터 콜렉션이 작성됩니다. 프로세스가 완료되면 요약 페이지가 별도의 웹 브라우저 탭에서 {{site.data.keyword.discoveryshort}}에 표시됩니다.

      - 문서를 업로드하여 콜렉션을 작성하려면 **문서 업로드**를 클릭하십시오.

        1.  먼저 콜렉션을 정의한 후 문서를 업로드합니다. 다음 정보를 제공하십시오.

            - 콜렉션 이름. 이 서비스 인스턴스의 이름은 고유해야 합니다.
            - 언어. 이 콜렉션에 추가하는 파일의 언어를 선택하십시오. {{site.data.keyword.discoveryshort}}에서 지원하는 언어에 대한 정보는 [언어 지원![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](/docs/services/discovery?topic=discovery-language-support){: new_window}을 참조하십시오.

              PDF 문서를 업로드하는 중이고 당사자, 네이처 및 카테고리 정보를 추출하려는 경우, **고급** 섹션을 펼치고 **이 콜렉션으로 기본 계약 구성 사용**을 클릭하십시오. 자세한 내용은 [콜렉션 요구사항![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](/docs/services/discovery?topic=discovery-element-classification#element-collection){: new_window}을 참조하십시오.
        1.  문서를 업로드합니다.

            지원되는 파일 유형에는 PDF, HTML, JSON 및 DOC 파일이 있습니다. 자세한 내용은 [컨텐츠 추가![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](/docs/services/discovery?topic=discovery-addcontent){: new_window}를 참조하십시오.
            {: note}

            업로드된 문서를 지속적으로 동기화할 수 없습니다. 문서에 대한 변경사항을 선택하려면 문서의 최신 버전을 업로드하십시오.

{{site.data.keyword.conversationshort}}로 돌아가기 전에 콜렉션이 완전히 수집될 때까지 기다리십시오.

### 데이터 콜렉션 작성 예제
{: #skill-search-add-json-collection-example}

예를 들어 다음과 유사한 JSON 파일이 있을 수 있습니다. 

```bash
{
  "Title": "About",
  "Shortdesc": "IBM Watson Assistant is a cognitive bot that you can customize for your business needs, and deploy across multiple channels to bring help to your customers where and when they need it.",
  "Topics": "overview",
  "url": "https://cloud.ibm.com/docs/services/assistant?topic=assistant-index"
}
```
{: codeblock}

반복되는 이름 값을 포함하는 JSON 파일을 업로드하는 경우, 이름 및 값 쌍의 첫 번째 발생만 인덱스화되고 검색에서 리턴합니다. 파일을 여러 JSON 파일로 나누고 설정을 업로드하십시오.
{: tip}

## 검색 구성
{: #skill-search-add-configure}

1.  {{site.data.keyword.discoveryshort}} 인스턴스에서 **Watson Assistant 설정 완료**를 클릭하십시오.

1.  {{site.data.keyword.conversationshort}} 검색 스킬 페이지에서k **구성**을 클릭하십시오.

1.  사용자에게 리턴되는 검색 결과에 포함할 텍스트를 추출하려는 {{site.data.keyword.discoveryshort}} 콜렉션 필드를 선택하십시오. 

    사용 가능한 필드는 수집한 데이터에 따라 다릅니다.

    각 검색 결과는 다음 섹션으로 구성될 수 있습니다.

    - **제목**: 검색 결과 제목. 콜렉션에서 제목, 이름 또는 유사한 유형의 필드를 검색 결과 제목으로 사용하십시오.

      제목으로 사용할 항목을 선택해야 합니다. 그렇지 않으면 Facebook 및 Slack 통합에 검색 결과 응답이 표시되지 않습니다. 
    - **본문**: 검색 결과 설명. 콜렉션에서 요약 또는 강조표시 필드를 검색 결과 본문으로 사용하십시오.

       본문으로 사용할 항목을 선택해야 합니다. 그렇지 않으면 Facebook 및 Slack 통합에 검색 결과 응답이 표시되지 않습니다. 
    - **URL**: 이 필드는 검색 결과 마지막에 포함하려는 바닥글 컨텐츠로 채워질 수 있습니다. 

       예를 들어, 원시 데이터 소스에 있는 원래 데이터 오브젝트에 대한 하이퍼텍스트 링크를 포함할 수 있습니다. 대부분의 온라인 데이터 소스는 직접 액세스를 지원하기 위해 상점의 오브젝트에 대한 자체 참조 공용 URL을 제공합니다. URL을 추가하는 경우에는 URL이 유효하고 액세스가 가능해야 합니다. 그렇지 않으면 Slack 통합이 응답에 URL을 포함하지 않으며 Facebook 통합이 어떤 응답도 리턴하지 않습니다. 

       Facebook 및 Slack 통합은 URL 필드가 비어 있을 때 검색 결과 응답을 제대로 표시할 수 있습니다. 
  
    검색 결과 중 하나 이상에 대해 값을 선택해야 합니다.
    {: important}

    도움말을 보려면 [콜렉션 필드 선택에 대한 팁](#skill-search-add-field-tips)을 참조하십시오.

    드롭 다운 필드에서 옵션을 사용할 수 없는 경우, 콜렉션 작성을 완료하는 데 {{site.data.keyword.discoveryshort}}에 더 많은 시간을 제공하십시오. 기다린 후에 콜렉션이 작성되지 않은 경우 콜렉션에 문서가 포함되지 않거나 우선 처리해야 하는 수집 오류가 있을 수 있습니다. 

    [업로드된 JSON 파일의 예](#skill-search-add-json-collection-example)를 계속하기 위한 좋은 맵핑은 *제목*, *간략한 설명*, 그리고 *url* 필드를 사용하는 것입니다.

    ![제목, 간략한 설명 및 URL 필드가 선택되어 있고 미리보기 검색 카드가 이러한 필드의 정보로 채워져 있음을 표시합니다.](images/search-skill-configure-fields.png)

    필드 맵핑을 추가하면 검색 결과의 미리보기가 데이터 콜렉션의 해당 필드에 있는 정보와 함께 표시됩니다. 이 미리보기는 사용자에게 리턴되는 검색 결과 응답에 포함되는 항목을 표시합니다. 

    검색 구성에 대해 도움을 받으려면 [문제점 해결](#skill-search-add-troubleshoot)을 참조하십시오.

1.  검색 성공을 기반으로 다른 메시지를 사용자와 공유하도록 초안을 작성하십시오.

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
      <td>어떤 이유로 인해 검색을 완료할 수 없음</td>
      <td>사용자 문의를 처리할 수 있는 정보가 있을 수 있지만, 현재 내 지식 베이스를 검색할 수 없습니다.</td>
    </tr>
    </table>

1.  **시험 사용**을 클릭하여 테스트를 위해 "시험 사용" 분할창을 여십시오. 구성 선택사항이 검색에 적용될 때 리턴되는 결과를 확인하기 위한 텍스트 메시지를 입력하십시오. 필요에 따라 조정하십시오.

1.  **작성**을 클릭하십시오.

나중에 검색 결과 카드의 구성을 변경하려면 검색 스킬을 다시 열고 편집하십시오. 변경할 때 변경사항을 저장하지 않아도 됩니다. 자동으로 적용됩니다. 검색 결과에 만족하는 경우 **저장**을 클릭하여 검색 스킬 구성을 완료하십시오.

다른 {{site.data.keyword.discoveryshort}} 서비스 인스턴스 또는 데이터 콜렉션에 연결하려는 경우, 새 검색 스킬을 작성하고 이를 다른 인스턴스에 연결하도록 구성하십시오. 검색 스킬을 작성한 후에는 검색 스킬에 대한 서비스 인스턴스 또는 데이터 콜렉션 세부사항을 변경할 수 **없습니다**.
{: important}

### 콜렉션 필드 선택에 대한 팁
{: #skill-search-add-field-tips}

데이터를 추출할 적절한 콜렉션 필드는 콜렉션의 데이터 소스 및 데이터 소스가 보강된 방법에 따라 달라집니다. 데이터 콜렉션 유형을 선택하면 콜렉션 필드 값이 콜렉션의 데이터 소스 유형에 따라 유용한 정보를 포함할 가능성이 가장 높은 소스 필드로 미리 채워집니다. 하지만, 사용자는 다른 누구보다 데이터를 잘 알고 있습니다. 소스 필드를 사용자 요구에 맞는 최상의 정보를 포함하는 필드로 변경할 수 있습니다. 

추출하려는 정보를 포함하는 필드의 이름을 포함하여 콜렉션에 있는 문서의 구조에 대해 자세히 알아보려면, {{site.data.keyword.discoveryshort}}에서 콜렉션을 열고 데이터 스키마 보기 아이콘 ![데이터 스키마 보기 아이콘](images/icon-view-data-schema.png)을 클릭하십시오. 

콜렉션이 작성될 때 콜렉션 소스 필드가 작성됩니다. `enriched_text.concepts.text`와 같이 생성되는 필드에 대해 자세히 알아보려면 [서비스 구성 > 보강 추가 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](/docs/services/discovery?topic=discovery-configservice#adding-enrichments){: new_window}를 참조하십시오.

## 문제점 해결
{: #skill-search-add-troubleshoot}

공통 태스크 수행에 대한 도움말을 보려면 이 정보를 검토하십시오. 

- **웹 크롤링 데이터 콜렉션**: 웹 크롤링 데이터 소스를 작성할 때 알아야 하는 사항:

    - {{site.data.keyword.discoveryshort}} Lite 플랜의 경우 1,000개가 넘는 문서를 작성할 수 없습니다.  
    - 데이터 콜렉션에 사용할 수 있는 문서 수를 늘리려면 URL 그룹 추가를 클릭하십시오. URL 그룹에는 크롤링이 필요하지만 초기 시드 URL과 링크되지 않는 페이지의 URL을 나열할 수 있습니다. 
    - 데이터 콜렉션에 사용할 수 있는 문서 수를 줄이려면 기본 URL의 하위 도메인을 지정하십시오. 또는 웹 크롤링 설정에서, Watson이 원래 페이지에서 작성할 수 있는 홉 수를 제한하십시오. 크롤링에서 명시적으로 제외하려는 하위 도메인을 지정할 수도 있습니다. 
    - 몇 분 후에 문서가 나열되지 않고 페이지가 새로 고쳐지면 수집하려는 컨텐츠가 URL의 페이지 소스에서 사용 가능한지 확인하십시오. 일부 웹 페이지 컨텐츠는 동적으로 생성되므로 크롤링할 수 없습니다.

- **업로드된 문서에 대한 검색 결과 구성**: 업로드된 문서의 콜렉션을 사용 중이고 올바른 검색 결과를 얻을 수 없거나 검색 결과가 충분하게 간결하지 않은 경우 데이터 콜렉션을 작성할 때 *스마트 문서 이해* 사용을 고려해 보십시오.  

  이 기능을 사용하면 텍스트 형식에 따라 문서에 어노테이션을 작성할 수 있습니다. 예를 들어 {{site.data.keyword.discoveryshort}}에게 28포인트 굵은체 글꼴의 텍스트는 문서 제목이라고 알릴 수 있습니다. 이 정보를 수집할 때 콜렉션에 적용하면, 나중에 검색 결과의 제목 섹션에 대한 소스로 *제목* 필드를 사용할 수 있습니다. 
  
  또한 스마트 문서 이해를 사용하여 대형 문서를 세그먼트로 분할함으로써 보다 쉽게 검색할 수 있습니다. 자세한 정보는 {{site.data.keyword.discoveryshort}} 문서의 [스마트 문서 이해 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](/docs/services/discovery?topic=discovery-sdu) 주제를 참조하십시오. 

- **검색 결과 향상**: 표시되는 결과가 마음에 들지 않는 경우 도움을 받기 위해 이 정보를 검토하십시오. 

  - 대화 노드에서 검색 스킬을 호출하고 필터 세부사항을 지정하십시오. 

    대화 노드 검색 스킬 응답에서 전체 {{site.data.keyword.discoveryshort}} 조회 구문 필터를 지정하여 결과 범위를 좁힐 수 있습니다. 
    
    예를 들어, 문서 제목 또는 일부 다른 메타데이터 필드에서 인텐트를 멘션하지 않는 데이터 콜렉션의 모든 문서를 걸러내는 필터를 정의할 수 있습니다. 또는, 필터가 데이터 콜렉션의 메타데이터에서 엔티티를 알려진 엔티티로서 식별하지 않거나 문서의 전체 텍스트 내 어디서도 엔티티를 멘션하지 않는 문서를 걸러낼 수 있습니다. 검색 스킬 응답 유형을 추가하는 방법에 대한 세부사항은 [보강 응답 추가](https://cloud.ibm.com/docs/services/assistant?topic=assistant-dialog-overview#dialog-overview-multimedia-add)를 참조하십시오.

    결과 향상에 대한 추가 팁은 [Watson Discovery에서 자연어 조회 결과 향상 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://developer.ibm.com/blogs/improving-your-natural-language-query-results-from-watson-discovery/) 블로그 게시물을 읽으십시오. 

## 다음 단계
{: #skill-search-add-next-steps}

스킬을 작성하면 스킬 페이지에 타일로 표시됩니다.

검색 스킬은 어시스턴트에 추가되고 어시스턴트가 배치될 때까지 고객과 상호작용할 수 없습니다. [어시스턴트 작성](/docs/services/assistant?topic=assistant-assistant-add)을 참조하십시오.

### 어시스턴트에 스킬 추가
{: #skill-search-add-to-assistant}

하나의 어시스턴트에 하나의 스킬을 추가할 수 있습니다. 어시스턴트 타일을 열고 어시스턴트에 스킬을 추가하십시오. 스킬 구성 페이지에서 스킬을 사용할 어시스턴트를 선택할 수 없습니다. 

하나 이상의 어시스턴트에서 하나의 검색 스킬을 사용할 수 있습니다.

1.  어시스턴트 탭에서 스킬을 추가하려는 어시스턴트에 대한 타일을 클릭하여 엽니다.

1.  **검색 스킬 추가**를 클릭합니다.

1.  **기존 스킬 추가**를 클릭합니다.

    표시되는 사용 가능한 스킬에서 추가할 스킬을 클릭합니다.

어시스턴트에 검색 스킬을 추가한 후에는 다음과 같이 어시스턴트에 대해 자동으로 사용 설정됩니다. 

- 어시스턴트에 검색 스킬만 있는 경우에는 어시스턴트의 통합 채널 중 하나에 제출된 사용자 입력이 검색 스킬을 트리거합니다.

- 어시스턴트에 대화 스킬 및 검색 스킬이 둘 다 있는 경우 모든 사용자 입력이 먼저 대화 스킬을 트리거합니다. 이 대화는 올바르게 응답할 수 있는 높은 신뢰도를 가진 모든 사용자 입력을 처리합니다. 일반적으로 대화 트리의 `anything_else` 노드를 트리거하는 조회는 대신 검색 스킬로 전송됩니다. 

  [검색 사용 안함](#search-skill-add-disable)의 단계에 따라 `anything_else` 노드에서 검색을 트리거하지 않도록 할 수 있습니다.
  {: note}

- 특정 질문에 대해 특정 검색 조회를 트리거하려면 적절한 대화 노드에 검색 스킬 응답 유형을 추가하십시오. 자세한 내용은 [응답](/docs/services/assistant?topic=assistant-dialog-overview#dialog-overview-multimedia)을 참조하십시오.

## 검색 트리거
{: #skill-search-add-trigger}

검색 스킬은 다음 방법으로 트리거됩니다. 

- **다른 노드**: 사용자의 쿼리를 처리할 수 있는 대화 노드가 없는 경우 외부 데이터 소스에서 관련 응답을 검색합니다.

  표준 메시지(예: `I don't know how to help you with that.`)를 표시하는 대신, 어시스턴트는 `Maybe this information can help:`라고 말한 후에 검색에서 리턴한 구절을 추가할 수 있습니다. 검색 스킬이 어시스턴트와 연결되어 있는 경우, `anything_else` 노드가 트리거될 때마다 노드 응답을 표시하지 않고 대신 검색을 수행합니다. 어시스턴트는 사용자 입력을 조회로 검색 스킬에 전달하고 검색 결과를 응답으로 리턴합니다.

  [검색 사용 안함](#search-skill-add-disable)의 단계에 따라 `anything_else` 노드에서 검색을 트리거하지 않도록 할 수 있습니다.
  {: note}

- **검색 응답 유형**: 검색 응답 유형을 대화 노드에 추가하는 경우, 어시스턴트는 외부 데이터 소스에서 구절을 검색하여 이를 특정 질문에 대한 응답으로 리턴합니다. 이 검색 유형은 개별 대화 노드가 처리될 때만 발생합니다.

  이 접근 방법은 검색을 트리거하기 전에 사용자 조회를 축소하려는 경우에 유용합니다. 예를 들어, 대화 분기는 고객이 구매하려는 장치 유형에 대한 정보를 수집할 수 있습니다. 제조업체 및 모델을 아는 경우, 제출된 조회의 모델 키워드를 검색 스킬에 전송하면 더 나은 결과를 얻을 수 있습니다.
- **검색 스킬 전용**: 검색 스킬만 어시스턴트에 링크되어 있고 어시스턴트에 연결된 대화 스킬이 없는 경우, 사용자 입력이 어시스턴트의 통합 채널 중 하나에서 수신되면 검색 조회가 {{site.data.keyword.discoveryshort}} 서비스에 전송됩니다. 

## 검색 스킬 테스트
{: #search-skill-add-test}

검색을 구성한 후, 검색 스킬의 "시험 사용" 분할창을 사용하여 테스트 조회를 전송하고 {{site.data.keyword.discoveryshort}}에서 리턴되는 검색 결과를 볼수 있습니다.

대화에서 응답하거나 검색을 트리거하는 질문을 할 때 고객이 가질 수 있는 전체 경험을 테스트하려면 미리보기 링크와 같은 채널 통합을 사용하십시오. 

대화 "시험 사용" 분할창에서 전체 엔트 투 엔드 사용자 경험을 테스트할 수 없습니다. 검색 스킬은 별도로 구성되고 어시스턴트에 연결됩니다. 대화 스킬은 검색 세부사항을 알 수 없으므로 검색 결과를 "시험 사용" 분할창에 표시할 수 없습니다.
{: important}

하나 이상의 통합 채널을 구성하여 검색 스킬을 테스트하십시오. 채널에서 검색을 트리거하는 조회를 입력하십시오. 대화에서 모든 유형의 검색을 시작하는 경우, 검색이 예상대로 트리거되는지 확인하려면 대화를 테스트하십시오. 검색 응답 유형을 사용하지 않는 경우, 기존 대화 노드가 사용자 입력을 처리할 수 없는 경우에만 검색이 트리거되는지 테스트하십시오. 검색이 트리거될 때마다 의미 있는 결과를 리턴하는지 확인하십시오.

## 검색 스킬에 추가 요청 전송
{: #search-skill-add-increase-flow}

대화 스킬이 자주 응답하지 않고 그 대신 보다 많은 조회를 검색 스킬에 전송하기를 원하는 경우 이에 맞게 대화를 구성할 수 있습니다.

이러한 접근 방법이 작동하려면 어시스턴트에 대화 스킬과 검색 스킬 둘 다를 추가해야 합니다. 

이 프로시저에 따라 신뢰수준 임계값을 기본 설정인 0.2에서 0.5로 재설정하여 대화가 응답할 가능성을 작게 만드십시오. 신뢰수준 임계값을 0.5로 변경하면 어시스턴트가 대화가 사용자의 인텐트를 이해하고 이를 해결할 수 있다고 50% 이상 확신하지 않는 한 대화의 답변으로 응답하지 않습니다. 

1.  대화 스킬의 *대화* 페이지에서 대화 트리의 마지막 노드에 `anything_else` 조건이 있는지 확인하십시오.

    이 노드가 처리될 때마다 검색 스킬이 트리거됩니다. 

1.  대화에 폴더를 추가하십시오. 강조를 해제하려는 첫 번째 대화 노드 위에 폴더를 놓으십시오. 폴더에 다음 조건을 추가하십시오.

    `intents[0].confidence > 0.5`

    이 조건은 폴더의 모든 노드에 적용됩니다. 조건은 어시스턴트가 사용자의 인텐트를 알고 있음을 적어도 50% 확신하는 경우에만 폴더에서 노드를 처리하도록 지시합니다.

1.  어시스턴트가 자주 처리하는 것을 원하지 않는 대화 노드를 폴더로 이동하십시오. 

대화를 변경한 후에는 어시스턴트를 테스트하여 검색 스킬이 원하는 만큼 자주 트리거되는지 확인하십시오.

대체 접근 방법은 무시할 수 있는 주제에 대해 대화를 교육하는 것입니다. 이를 위해서 어시스턴트가 대화 스킬의 "시험 사용" 분할창에서 테스트 발화로 즉각 검색 스킬을 전송하게 하려는 발화를 추가할 수 있습니다. 그런 다음 "시험 사용" 분할창의 **관련 없음으로 표시** 옵션을 선택하여 대화가 이 발화 또는 유사한 발화에 응답하지 않도록 교육할 수 있습니다. 자세한 정보는 [무시할 수 있는 주제에 대해 어시스턴트 교육](/docs/services/assistant?topic=assistant-logs#logs-mark-irrelevant)을 참조하십시오.

## 검색 사용 안함
{: #search-skill-add-disable}

검색 스킬이 트리거되지 않도록 설정할 수 있습니다.

통합을 설정하는 동안 임시로 이를 수행하고자 할 수 있습니다. 또는 대화에서 식별할 수 있는 특정 사용자 조회에 대해서만 검색을 트리거하고 검색 스킬 응답 유형을 사용하여 응답할 수 있습니다.

검색 스킬이 트리거되지 않도록 하려면 다음 단계를 완료하십시오.

1.  **어시스턴트** 페이지에서 어시스턴트를 위한 메뉴를 클릭한 후 **설정**을 선택하십시오. 
1.  *검색 스킬* 페이지를 열고 클릭하여 **사용 안함**으로 전환하십시오.
