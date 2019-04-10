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

# 마이그레이션
{: #migrate}

현재 Cloud Foundry 조직 및 영역에서 리소스 그룹으로 이동하려면 {{site.data.keyword.conversationshort}} 서비스 인스턴스를 마이그레이션하십시오.
{: shortdesc}

{{site.data.keyword.cloud_notm}}가 Cloud Foundry 사용에서 리소스 그룹 사용으로 이동되었습니다.

리소스 그룹은 Cloud Foundry에 비해 다음과 같은 이점을 제공합니다.

- IBM Cloud IAM(Identity and Access Management)을 사용한 세부 단위의 액세스 제어
- 서비스 인스턴스를 여러 지역의 앱 및 서비스에 연결할 수 있는 기능
- 그룹별 사용량 데이터 캡처 간소화

2018년 11월 이전에 서비스 인스턴스를 작성한 경우 인스턴스가 호스팅되는 위치에 따라 리소스 그룹 대신 Cloud Foundry를 사용 중일 수 있습니다. 각 위치가 새 인스턴스에 IAM을 사용하기 시작한 시기에 대한 정보는 [데이터 센터](/docs/services/assistant?topic=assistant-services-information#services-information-regions)를 참조하십시오.

## 서비스 인스턴스 마이그레이션
{: #migrate-task}

시작하기 전에 마이그레이션 프로세스에 대해 자세히 알아보려면 [IBM Cloud 마이그레이션 문서 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](/docs/resources?topic=resources-migrate)를 참조하십시오.

인스턴스를 작성했거나 인스턴스에 대한 `Developer` 역할 액세스 권한을 부여받은 사용자 또는 그룹만 인스턴스를 마이그레이션할 수 있습니다.
{: note}

서비스 인스턴스를 마이그레이션하려면 다음 단계를 완료하십시오.

1.  먼저 서비스 인스턴스를 이동할 리소스 그룹을 판별하십시오.

    팁은 [리소스 그룹의 리소스 구성을 위한 우수 사례 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](/docs/resources?topic=resources-bp_resourcegroups)를 참조하십시오. 

1.  IBM Cloud Dashboard 서비스 목록에서 마이그레이션할 인스턴스에 대한 마이그레이션 아이콘 ![마이그레이션](images/migrate.svg)을 클릭한 후 팝업에서 **마이그레이션**을 클릭하십시오.

    2018년 5월 7일 이전에 시드니 데이터 센터에서 작성되었거나 2018년 12월 13일 이전에 런던 데이터 센터에서 작성된 서비스 인스턴스가 댈러스 데이터 센터로 신디케이트되었습니다. 시드니 또는 런던 기반 Cloud Foundry 서비스 인스턴스를 마이그레이션할 때 댈러스에서 호스팅되는 리소스로 변환됩니다.
    {: note}

1.  **계속**을 클릭한 후 리소스 그룹을 선택하십시오.

    리소스 그룹을 작성하지 않은 경우에는 지금 작성할 수 있습니다. **기본** 리소스 그룹이 사용 가능합니다. 충분한 시간을 들여 그룹이 사용되는 방법을 이해하고 필요한 경우 그룹을 작성하십시오. 여기서 선택하는 리소스 그룹은 나중에 변경할 수 *없습니다*.

1.  **마이그레이션**을 클릭하십시오.

    프로세스가 완료되면 메시지가 표시됩니다. 마이그레이션할 다른 서비스 인스턴스가 있는 경우 계속해서 다른 서비스 인스턴스를 마이그레이션하거나 **완료**를 클릭할 수 있습니다.

마이그레이션한 이전(Cloud Foundry 조직 기반) 서비스 인스턴스가 대시보드의 Cloud Foundry 서비스 섹션에 계속 나열되며 이제 새(리소스 그룹 기반) 버전의 인스턴스에 대한 *별명*으로 표시됩니다.

![현재 서비스 인스턴스가 이제 리소스 기반 인스턴스의 별명임을 표시합니다.](images/alias.png)

별명에 대한 자세한 정보는 [IBM Cloud Connections 문서 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://cloud.ibm.com/docs/resources/connecting_apps#what_is_alias)를 참조하십시오.

**도구 실행** 단추에 액세스하려면 새로운 리소스 그룹 기반 버전의 서비스 인스턴스를 열어야 합니다. 새 인스턴스는 {{site.data.keyword.Bluemix_notm}} 대시보드의 서비스 섹션에 나열됩니다.

## 인증
{: #migrate-auth-support}

기본 인증을 사용하여 서비스에 액세스하는 기존 애플리케이션이 있는 경우 마이그레이션된 후 계속해서 사용자 이름 및 비밀번호를 전달하여 서비스 인스턴스를 인증할 수 있습니다. 별명의 **서비스 인증 정보** 페이지에서 인증 정보를 볼 수 있습니다.

새 인스턴스는 사용자 이름 및 비밀번호 인증 정보 대신 API 키를 사용하는 향상된 메커니즘인 IBM Cloud IAM(Identity and Access Management)을 사용하여 인증을 관리합니다. 새 서비스 인스턴스의 **서비스 인증 정보** 페이지에서 API 키 정보를 볼 수 있습니다.

새 인증 방법을 사용하여 여기서 제공되는 향상된 보안을 활용할 수 있도록 기존 사용자 정의 애플리케이션을 업데이트하는 것을 고려하십시오. 새 인스턴스에서 대화 스킬을 변경하면 기본 인증의 인증 정보를 사용하는 애플리케이션에도 반영됩니다. 새 API 키 접근 방식을 사용하도록 모든 앱을 업데이트한 후에는 별명이 필요하지 않으며 별명을 삭제할 수 있습니다.
