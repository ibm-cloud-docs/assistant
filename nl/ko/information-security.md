---

copyright:
  years: 2015, 2019
lastupdated: "2019-07-31"

subcollection: assistant

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:external: target="_blank" .external}
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

# 정보 보안
{: #information-security}

IBM은 고객과 파트너에게 혁신적인 데이터 개인정보 보호, 보안 및 통제 솔루션을 제공하기 위해 노력하고 있습니다.
{: shortdesc}

**주의사항:**
고객은 유럽 연합 일반 개인정보 보호법률(General Data Protection Regulation , GDPR)을 포함한 다양한 법령과 규정을 준수해야 할 책임이 있습니다. 고객은 고객의 비즈니스에 영향을 줄 수 있는 관련 법령 및 규정에 대한 확인과 해석, 그러한 법령 및 규정의 준수를 위해 필요한 고객의 모든 조치와 관련하여 적절한 법률 자문을 받아야 할 단독 책임이 있습니다.

여기에서 설명하는 제품, 서비스 및 기타 기능은 일부 고객의 상황에는 해당되지 않을 수 있으며 그 가용성이 제한될 수 있습니다. IBM은 법률, 회계 또는 감사 관련 자문을 제공하지 않으며, IBM의 서비스나 제품 사용이 고객의 관련 법령이나 규정 준수를 보장한다는 진술이나 보증을 제공하지 않습니다.

작성된 {{site.data.keyword.cloud}} {{site.data.keyword.watson}} 리소스의 GDPR 지원을 요청해야 하는 경우

- 유럽 연합에서는 [유럽 연합에서 작성된 IBM Cloud Watson 리소스에 대한 지원 요청 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://cloud.ibm.com/docs/services/watson/getting-started-gdpr-sar#request-EU){: new_window}을 참조하십시오.
- 유럽 연합 이외의 지역에서는 [유럽 연합 외부의 리소스에 대한 지원 요청 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://cloud.ibm.com/docs/services/watson/getting-started-gdpr-sar#request-non-EU){: new_window}을 참조하십시오.

## 유럽 연합 일반 개인정보 보호법률(General Data Protection Regulation, GDPR)
{: #information-security-gdpr}

IBM은 고객 및 파트너가 GDPR 규제를 준수하는 데 도움을 줄 수 있도록 혁신적인 데이터 개인정보 보호, 보안 및 통제 솔루션을 제공하려 노력하고 있습니다.

IBM의 자체 GDPR 준비 과정 및 사용자의 규제 준수 과정을 지원하기 위한 GDPR 기능과 오퍼링에 대한 자세한 정보는 [여기 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](../../icons/launch-glyph.svg "외부 링크 아이콘")](http://www.ibm.com/gdpr){: new_window}.

## HIPAA(Health Insurance Portability and Accountability Act)
{: #information-security-hipaa}

US Health Insurance Portability and Accountability Act(HIPAA)는 2019년 4월 1일 이후 설립된 Washington, DC에서 호스팅되는 Premium 플랜에서 사용할 수 있습니다. 자세한 정보는 [Enabling EU 및 HIPAA 지원 설정 사용](/docs/account?topic=account-eu-hipaa-supported#eu-hipaa-supported){: external}을 참조하십시오. 

사용자가 작성하는 훈련 데이터(사용자 예제를 포함하는 엔티티 및 인텐트)에 PHI(Personal Health Information)를 추가하지 마십시오. 특히, 인텐트 또는 인텐트 사용자 예제 권장사항을 마이닝하기 위해 업로드하는 실제 사용자 발화(utterance)를 포함하는 파일에서 PHI를 제거해야 합니다.

## Watson Assistant에서 데이터 레이블 지정 및 삭제
{: #information-security-gdpr-wa}

사용자가 작성하는 훈련 데이터(사용자 예제를 포함하는 엔티티 및 인텐트)에 개인 데이터를 추가하지 마십시오. 특히, 사용자 예제 권장사항을 마이닝하기 위해 업로드하는 실제 사용자 발화(utterance)를 포함하는 파일에서 PII(Personally Identifiable Information)를 제거해야 합니다.

**참고:** 시범 및 베타 기능은 프로덕션 환경에서 사용하기 위한 것이 아니므로 데이터 레이블을 지정하거나 데이터를 삭제할 때 예상대로 작동한다고 보장되지 않습니다. 데이터 레이블 지정 및 삭제가 필요한 솔루션을 구현하는 경우 시범 및 베타 기능을 사용하지 않아야 합니다.

{{site.data.keyword.conversationshort}} 인스턴스에서 고객의 메시지 데이터를 제거해야 하는 경우 메시지가 {{site.data.keyword.conversationshort}}에 전송될 때 메시지를 고객 ID와 연관시키면 클라이언트의 고객 ID를 기반으로 이를 수행할 수 있습니다.

**참고:** 미리보기 링크 및 자동 Facebook 통합 기능은 고객 ID 기반의 데이터 레이블 지정 및 삭제를 지원하지 않습니다. 고객 ID를 기반으로 삭제할 수 있는 기능이 필요한 솔루션에서는 이러한 기능을 사용하지 않아야 합니다.

### 시작하기 전에
{: #information-security-delete-user-data-prereqs}

특정 사용자와 연관된 메시지 데이터를 삭제할 수 있으려면 먼저 각 메시지를 각 사용자의 **고객 ID**와 연관시켜야 합니다. `/message` API를 사용하여 전송된 메시지에 대한 **고객 ID**를 지정하려면 `X-Watson-Metadata: customer_id` 특성을 헤더에 포함시키십시오. 예:

```
curl -X POST -u "apikey:3Df... ...Y7Pc9"
 --header
   'Content-Type: application/json'
   'X-Watson-Metadata: customer_id=abc'
 --data
   '{"input":{"text":"hello"}}'
  'https://gateway-eu-de.watsonplatform.net/assistant/api/v1/workspaces/{workspaceID}/message?version=2018-09-20'
```
{: codeblock}

`customer_id` 문자열에는 세미콜론(`;`) 또는 등호(`=`) 문자가 포함될 수 없습니다. 각 `고객 ID` 특성이 고객마다 고유한지 확인해야 합니다.
{: note}

세미콜론으로 구분된 `customer_id={value}` 쌍으로 여러 **고객 ID** 값을 전달할 수 있습니다 (예: `'X-Watson-Metadata: customer_id=abc;customer_id=xyz'`).

어시스턴트에 검색 스킬을 추가하면 어시스턴트에 제출된 사용자 입력이 {{site.data.keyword.discoveryshort}} 서비스에 검색 조회로 전달됩니다. {{site.data.keyword.conversationshort}} 통합이 고객 ID를 제공하는 경우 결과 /message API 요청이 고객 ID를 헤더에 포함시키고 {{site.data.keyword.discoveryshort}} /query API 요청을 통해 ID가 전달됩니다. 특정 고객과 연관된 조회 데이터를 삭제하려면 어시스턴트와 링크된 {{site.data.keyword.discoveryshort}} 서비스 인스턴스에 직접 삭제 요청을 전송해야 합니다. 세부사항은 {{site.data.keyword.discoveryshort}} [정보 보안](https://cloud.ibm.com/docs/services/discovery/information-security#gdpr-discovery) 주제를 참조하십시오.

### 사용자 데이터 조회
{: #information-security-query-customer-id}

특정 사용자 데이터에 대한 애플리케이션 로그를 검색하려면 v1 `/logs` 메소드의 `filter` 매개변수를 사용하십시오. 예를 들어, `my_best_customer`와 일치하는 `customer_id`에 특정한 데이터를 검색하려는 경우 조회는 다음과 같습니다.

``` curl
curl -X GET -u "apikey:3Df... ...Y7Pc9"
'https://gateway-eu-de.watsonplatform.net/assistant/api/v1/workspaces/{workspaceID}/logs?version=2018-09-20&filter=customer_id::my_best_customer'
```
{: codeblock}

세부사항은 [필터 조회 참조](/docs/services/assistant?topic=assistant-filter-reference)를 참조하십시오.

### 데이터 삭제
{: #information-security-delete-data}

어시스턴트에서 저장했을 수 있는 특정 사용자와 연관된 메시지 로그 데이터를 삭제하려면 `DELETE /user_data` v1 메소드를 사용하십시오. 요청과 함께 `customer_id` 매개변수를 전달하여 사용자의 고객 ID를 지정하십시오.

연관된 고객 ID가 있는 `POST /message` API 엔드포인트를 사용하여 추가된 데이터만 이 삭제 메소드를 사용하여 삭제할 수 있습니다. 다른 방법으로 추가된 데이터는 고객 ID를 기반으로 삭제할 수 없습니다. 예를 들어, 고객 대화에서 추가된 엔티티 및 인텐트는 이 방법으로 삭제할 수 없습니다. 개인 데이터는 이러한 메소드에 대해 지원되지 않습니다.

**중요**: `customer_id`를 지정하면 하나의 스킬 내에서만이 아니라 전체 {{site.data.keyword.conversationshort}} 인스턴스에서 삭제 요청 이전에 수신된 해당 `customer_id`의 *모든* 메시지가 삭제됩니다.

예를 들어, 고객 ID가 `abc`인 사용자와 연관된 모든 메시지 데이터를 {{site.data.keyword.conversationshort}} 인스턴스에서 삭제하려면 다음 cURL 명령을 전송하십시오.

```
curl -X DELETE -u "apikey:3Df... ...Y7Pc9"
 'https://gateway-eu-de.watsonplatform.net/assistant/api/v1/user_data?customer_id=abc&version=2018-09-20'
```
{: codeblock}

비어 있는 JSON 오브젝트 `{}`가 리턴됩니다.

자세한 정보는 [API 참조](https://cloud.ibm.com/apidocs/assistant?curl=#delete-labeled-data)를 참조하십시오.

**참고:** 삭제 요청은 일괄처리되며 완료하는 데 최대 24시간이 걸릴 수 있습니다.
