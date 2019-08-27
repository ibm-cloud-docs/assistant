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

# IBM Cloud 서비스 정보
{: #services-information}

어시스턴트는 {{site.data.keyword.cloud}}에서 관리하는 완전히 호스팅되는 봇입니다. 즉, 이를 지원하도록 인프라를 설정하거나 유지보수할 필요가 없습니다.
{: shortdesc}

## 서비스 플랜 정보
{: #services-information-plans}

{{site.data.keyword.conversationshort}} [서비스 플랜 옵션 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.ibm.com/cloud/watson-assistant/pricing/){: new_window}을 살펴보십시오.

서비스 인스턴스를 작성하기 전에 {{site.data.keyword.cloud_notm}} 계정에서 리소스를 구성하는 방법을 결정하십시오. 고유 리소스 그룹을 정의하지 않으면 **기본** 리소스 그룹이 사용되며 나중에 이를 변경할 수 *없습니다*. 세부사항은 [리소스 그룹의 리소스 구성을 위한 우수 사례 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://cloud.ibm.com/docs/resources/bestpractice_rgs#bp_resourcegroups){: new_window}를 참조하십시오. 모든 사용자에게 운영자라는 플랫폼 액세스 역할이 있어야 합니다. (서비스 액세스 역할은 {{site.data.keyword.conversationshort}}에서 활용되지 않습니다.)

현재 인스턴스가 속한 서비스 플랜을 찾으려면 다음 단계를 완료하십시오.

1.  현재 사용 중인 인스턴스의 이름을 기록하십시오. (기본 스킬 또는 어시스턴트 페이지에서 인스턴스를 찾아 변경할 수 있습니다.)
1.  [IBM Cloud 리소스 목록![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://cloud.ibm.com/resources) 페이지로 이동하십시오.
1.  **서비스** 섹션을 펼치고 이전에 기록한 인스턴스 이름을 찾은 후 클릭하여 연관된 플랜 정보를 보십시오. 

### 아티팩트 유형별 플랜 한계
{: #services-information-limits}

플랜별 아티팩트 한계에 대한 정보는 아티팩트 작성 방법을 설명하는 주제에 있으므로 알아야 하는 경우 한계를 참조할 수 있습니다. 주제에 대한 링크는 다음과 같습니다.

- [어시스턴트](/docs/services/assistant?topic=assistant-assistant-add#assistant-add-limits)
- [대화 노드](/docs/services/assistant?topic=assistant-dialog-build#dialog-build-node-limits)
- [엔티티](/docs/services/assistant?topic=assistant-entities#entities-limits)
- [비활성 제한시간](/docs/services/assistant?topic=assistant-assistant-settings#assistant-settings-session-limits)
- [인텐트](/docs/services/assistant?topic=assistant-intents#intents-limits)
- [통합](/docs/services/assistant?topic=assistant-deploy-integration-add#deploy-integration-add-limits)
- [로그](/docs/services/assistant?topic=assistant-logs#logs-limits)
- [스킬](/docs/services/assistant?topic=assistant-skill-add#skill-add-limits)
- [버전](/docs/services/assistant?topic=assistant-versions#versions-limits)

### API 호출 한계
{: #services-information-api-limits}

인스턴스마다 허용되는 API 호출의 수는 서비스 플랜에 따라 다릅니다. 세부사항은 플랜 설명을 참조하십시오.

Lite 플랜이 있고 API 호출 한계에 도달하지만 예상보다 적은 호출을 작성했다고 로그에 표시되는 경우, Lite 플랜이 7일 동안만 로그 정보를 저장한다는 사실을 기억하십시오.

하나의 플랜에서 다른 플랜으로 업그레이드하려면 [업그레이드](/docs/services/assistant?topic=assistant-upgrade)를 참조하십시오.

### Plus 또는 Premium 플랜 기능 ![Plus 또는 Premium 플랜만 해당](images/plus.png)
{: #services-information-premium}

다음 기능은 Plus 또는 Premium 플랜 사용자만 사용할 수 있습니다.

- [모호성 제거](/docs/services/assistant?topic=assistant-dialog-runtime#dialog-runtime-disambiguation)
- [인텐트 충돌 해결](/docs/services/assistant?topic=assistant-intents#intents-resolve-conflicts)
- [인텐트 권장사항 및 인텐트 사용자 예제 권장사항](/docs/services/assistant?topic=assistant-intent-recommendations)
- [Intercom 통합](/docs/services/assistant?topic=assistant-deploy-intercom)
- [검색 스킬](/docs/services/assistant?topic=assistant-skill-search-add)

### 사용자 기반 플랜
{: #services-information-user-based-plans}

지정된 시간 범위 동안 수행된 API 호출 수를 기준으로 사용량을 측정하는 API 기반 플랜과 다르게 새 Plus 플랜과 업데이트된 Premium 플랜에서는 사용자 기반 청구를 사용합니다. 이러한 플랜은 지정된 시간 범위 동안 어시스턴트와 상호작용한 고유 사용자 수에 따라 사용량을 측정합니다.

{{site.data.keyword.conversationshort}}가 청구를 위해 API 요청에서 다음 정보를 이 순서대로 확인합니다.

  1.  **user_id**: /message API 호출의 컨텍스트 오브젝트에서 전송되는 API에 정의된 특성입니다. 이 특성을 사용하는 것은 /message API 호출을 고유 사용자에게 정확하게 귀속시키도록 하는 가장 좋은 방법입니다. 사용자 ID 특성에 대한 자세한 정보는 API 참조 문서를 참조하십시오.
  
    - `context.global.system.user_id`: [v2 API](https://cloud.ibm.com/apidocs/assistant-v2#send-user-input-to-assistant)
    - `context.metadata.user_id`: [v1 API](https://cloud.ibm.com/apidocs/assistant#get-response-to-user-input)

  1.  **session_id**: 사용자와 어시스턴트 사이의 단일 대화를 식별하는 v2 API에 정의된 특성입니다. 세션 ID는 기본 제공 통합에서 생성된 /message API 호출에서 제공됩니다. 사용자가 대화 창을 닫거나 대화가 60분 동안 비활성 상태이면 세션이 종료됩니다.

  1.  **conversation_id**: /message API 호출의 컨텍스트 오브젝트에 저장되는 v1 API에 정의된 특성입니다. 이 특성은 한 사용자와의 단일 대화 교환과 연관된 다중 /message API 호출을 식별하는 데 사용될 수 있습니다. 그러나 명시적으로 ID를 보유하고 동일한 대화의 일부로 수행된 각 요청과 함께 다시 전달하는 경우에만 동일한 ID가 사용됩니다. 그렇지 않으면 새 /message API 호출마다 새 ID가 생성됩니다.

새로운 사용자 기반 서비스 플랜을 최대한 활용하려면 고유 사용자 ID 또는 세션 ID를 캡처하고 {{site.data.keyword.conversationshort}}에 정보를 전달하도록 어시스턴트를 배치하는 데 사용하는 사용자 정의 애플리케이션을 디자인하십시오.

## API 호출 인증
{: #services-information-authenticate-api-calls}

서비스 인스턴스에서 사용되는 인증 메커니즘은 API 호출을 수행할 때 인증 정보를 제공해야 하는 방법에 영향을 줍니다.

1.  서비스 인증 정보를 가져오십시오.

    - [{{site.data.keyword.Bluemix_notm}} 리소스 목록 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://cloud.ibm.com){: new_window}에서 서비스 인스턴스를 찾아서 클릭하십시오.

    - 클릭하여 서비스 인스턴스를 열고 **서비스 인증 정보**를 클릭한 후 **인증 정보 보기**를 클릭하십시오.

      **Cloud Foundry 인증 정보**

      ![Cloud Foundry에서 호스팅되는 인스턴스에 대한 서비스 인증 정보 페이지를 표시합니다.](images/cf-cred-ui.png)

      **IAM 인증 정보**

      ![IAM에서 호스팅되는 인스턴스에 대한 서비스 인증 정보 페이지를 표시합니다.](images/iam-creds.png)

1.  API 호출에서 이러한 인증 정보를 사용하십시오.

    **Cloud Foundry API 호출**

    사용자 이름 및 비밀번호 인증 정보를 제공하십시오.

    ```curl
    curl -X GET \
    --user {username}:{password} \
    'https://gateway.watson.net/assistant/api/v1/workspaces?version=2018-09-20'
    ```
    {: codeblock}

     **IAM API 호출**

    - 기본 URL에는 위치가 포함되어야 합니다. 서비스 인스턴스를 작성한 위치를 지정하려면 `gateway-<location>.watsonplatform.net` 구문을 사용하십시오. 위치 코드는 *데이터 센터 위치* 표에 나열되어 있습니다.
    - 헤더에 적절한 유형의 토큰을 제공하십시오. 베어러 토큰 또는 API 키를 전달할 수 있습니다.

      - 토큰은 모든 호출에 서비스 인증 정보를 임베드하지 않고 인증된 요청을 지원합니다. 다음 예에서는 사용 중인 베어러 토큰을 보여줍니다.

        ```curl
        curl -X GET \
        'https://gateway-syd.watsonplatform.net/assistant/api/v1/workspaces?version=2018-09-20' \
        --header 'Authorization: Bearer eyJhbGciOiJIUz......sgrKIi8hdFs'
        ```
        {: codeblock}

      - API 키는 기본 인증을 사용합니다. 다음 예에서는 사용 중인 apikey를 보여줍니다.

        ```curl
        curl -X GET -u "apikey:3Df... ...Y7Pc9" \
        'https://gateway-us-east.watsonplatform.net/assistant/api/v1/workspaces?version=2018-09-20' \
        ```
        {: codeblock}

        Watson SDK 중 하나를 사용하는 경우 API 키를 전달하고 SDK에서 토큰의 라이프사이클을 관리하도록 할 수 있습니다.
        {: note}

        IAM 리소스는 Cloud Foundry 명령행 인터페이스(CLI)를 사용하여 관리할 수 없습니다. 예를 들어, 서비스 인스턴스를 작성하거나 관리하는 Cloud Foundry CLI 명령(`cf`로 시작)은 IAM을 사용하는 위치에서 호스팅되는 인스턴스에 대해 작동하지 않습니다. 대신, {{site.data.keyword.cloud_notm}} CLI 및 이와 연관된 명령을 사용해야 합니다. 세부사항은 [리소스 및 리소스 그룹에 대한 작업 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](/docs/cli/reference/ibmcloud?topic=cloud-cli-ibmcloud_commands_resource)을 참조하십시오.

        자세한 정보는 [IAM 토큰을 사용한 인증 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](/docs/services/watson?topic=watson-iam){: new_window}을 참조하십시오.

    예를 들어, API 참조에서 사용자 언어에 해당하는 [인증 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://{DomainName}/apidocs/assistant-v2#authentication){: new_window}을 참조하십시오.

### 데이터 센터
{: #services-information-regions}

{{site.data.keyword.cloud_notm}}에는 클라우드 서비스에 성능 이점을 제공하는 글로벌 데이터 센터의 네트워크가 있습니다. 세부사항은 [{{site.data.keyword.cloud_notm}} 글로벌 데이터 센터 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.ibm.com/cloud/data-centers/){: new_window}를 참조하십시오.

{{site.data.keyword.cloud_notm}}는 Cloud Foundry를 사용하여 사용자 액세스를 관리하는 것에서 토큰 기반 IAM(Identity and Access Management) 인증을 사용하는 것으로 변경되었습니다. IAM이 서로 다른 시간에 여러 위치에 배포되었습니다. 서비스 인스턴스를 마이그레이션하여 현재 Cloud Foundry 조직 및 영역에서 리소스 그룹으로 이동할 수 있습니다. 세부사항은 [마이그레이션](/docs/services/watson?topic=watson-migrate)을 참조하십시오.

다음 데이터 센터 위치에서 호스팅되는 {{site.data.keyword.conversationshort}} 서비스 인스턴스를 작성할 수 있습니다.

| 위치    | 위치 코드 | 인증 유형 | IAM 채택 날짜 | 참고 |
|-------------|---------------|---------------------|-------------------|-------|
| 댈러스      | us-south      | IAM                 | 2018년 10월 30일 | 해당사항 없음 |
| 프랑크푸르트   | eu-de         | IAM                 | 2018년 10월 30일 | 해당사항 없음 |
| 시드니      | au-syd        | IAM                 | 2018년 5월 7일 | 5월 7일 이전에 작성된 인스턴스는 댈러스에 신디케이트됨 |
| 도쿄       | jp-tok        | IAM                 | 2018년 11월 8일 | 해당사항 없음 |
| 런던      | eu-gb, lon    | IAM                 | 2018년 12월 13일 | 12월 13일 이전에 영국 지역에서 작성된 인스턴스는 미국 남부 지역에 신디케이트됨 |
| 워싱턴 DC  | us-east    | IAM                 | 2018년 6월 14일 | 해당사항 없음 |
{: caption="데이터 센터 위치" caption-side="top"}

다른 {{site.data.keyword.cloud_notm}} 서비스가 호스팅되는 데이터 센터에 대한 정보는 [지역별 서비스 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://cloud.ibm.com/docs/resources/services_region#services_region){: new_window}를 참조하십시오.

## 이용 약관 및 보안
{: #services-information-terms}

서비스 이용 약관 및 데이터 보안에 대해 자세히 알아보려면 다음 정보를 읽으십시오.

- [서비스 이용 약관 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://www-03.ibm.com/software/sla/sladb.nsf/sla/home?OpenDocument){: new_window}
- [데이터 보안 및 개인정보 보호 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.ibm.com/software/sla/sladb.nsf/sla/csdsp?OpenDocument){: new_window}
- [정보 보안](/docs/services/assistant?topic=assistant-information-security)

{{site.data.keyword.cloud_notm}}에 대한 자세한 정보는 [플랫폼 개요 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](/docs/overview?topic=overview-whatis-platform){: new_window}를 참조하십시오.

## 아직 궁금하신 사항이 있습니까? 
{: #services-information-sales}

[IBM Sales ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://www-01.ibm.com/marketing/iwm/dre/signup?source=urx-20970){: new_window}에 문의하시기 바랍니다.
