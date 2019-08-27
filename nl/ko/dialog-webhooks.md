---

copyright:
  years: 2015, 2019
lastupdated: "2019-08-09"

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

# 대화 노드에서 프로그래밍 방식 호출 작성
{: #dialog-webhooks}

프로그램 호출을 작성하려면 프로그램 기능을 수행하는 외부 애플리케이션에 POST 요청 콜아웃을 전송하는 웹 후크를 정의하십시오. 그런 다음 하나 이상의 대화 노드에서 웹 후크를 호출할 수 있습니다.

웹 후크는 프로그램에서 발생하는 내용을 기반으로 외부 프로그램에 호출할 수 있는 메커니즘입니다. 대화 스킬에서 사용되는 경우, 웹 후크는 어시스턴트가 웹 후크가 사용되는 노드를 처리할 때 트리거됩니다. 웹 후크는 사용자가 지정하거나 대화 중에 사용자로부터 수집하여 컨텍스트 변수에 저장하는 데이터를 수집합니다. 그런 다음 웹 후크 정의의 일부로 지정한 URL에 HTTP POST 요청의 일부로 이러한 데이터를 전송합니다. 웹 후크를 수신하는 URL은 리스너가 됩니다. 리스너는 웹 후크 정의에 지정된 대로 전달하는 정보를 사용하여 사전 정의된 조치를 수행하며, 선택적으로 응답을 리턴할 수 있습니다.

자세한 내용을 보려면 다음 동영상을 시청하십시오.

<iframe class="embed-responsive-item" id="youtubeplayer" title="웹 후크 데모" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/j8TBqD2rx2o?rel=0" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

웹 후크를 사용하여 다음 유형의 작업을 수행할 수 있습니다.

- 사용자로부터 수집한 정보를 유효성 검증합니다.
- 정보를 얻기 위해 외부 웹 서비스와 상호 작용합니다. 예를 들어, 항공 교통 서비스에서 비행기의 예상 도착 시간을 확인하거나 날씨 서비스에서 날씨 예보를 받을 수 있습니다.
- 식당 예약 사이트와 같은 외부 애플리케이션에 요청을 보내어 사용자 대신 간단한 거래를 완료할 수도 있습니다.
- SMS 알림을 트리거합니다. 
- {{site.data.keyword.openwhisk}} 웹 조치를 트리거합니다.

토큰 기반 IAM(Identity and Access Management) 인증을 사용하는 {{site.data.keyword.openwhisk_short}} 조치를 호출하기 위해 웹 후크를 사용할 수 없습니다.
{: note}

클라이언트 애플리케이션 호출 방법에 대한 정보는 [대화 노드에서 클라이언트 애플리케이션 호출](/docs/services/assistant?topic=assistant-dialog-actions-client)을 참조하십시오.

## 웹 후크 정의
{: #dialog-webhooks-create}

대화 스킬에 대해 하나의 웹 후크 URL을 정의한 다음 하나 이상의 대화 노드에서 웹 후크를 호출할 수 있습니다.

외부 서비스에 대한 프로그램 호출은 다음 요구사항을 충족해야 합니다.

- 호출은 POST HTTP 요청이어야 합니다.
- 요청 및 응답은 JSON 형식이어야 합니다. 예: `Content-Type: application/json`
- 호출은 **5초 이하로**로 리턴되어야 합니다. 

  호출해야 하는 서비스의 효율성이 떨어지는 경우, 클라이언트 애플리케이션을 통해 호출을 관리하고 정보를 대화에 별도의 단계로 전달할 수 있습니다. 자세한 정보는 [대화 노드에서 클라이언트 애플리케이션 호출](/docs/services/assistant?topic=assistant-dialog-actions-client)을 참조하십시오.
  {: tip}

웹 후크 세부사항을 추가하려면 다음 단계를 완료하십시오.

1.  웹 후크를 추가하려는 스킬에서 **옵션** 탭을 클릭하십시오.

1.  **웹 후크**를 클릭하십시오.

1.  **URL** 필드에서 HTTP POST 요청 콜아웃을 전송할 외부 애플리케이션의 URL을 추가하십시오.

    예를 들어, 언어 변환기 서비스를 호출하려면 서비스 인스턴스의 URL을 지정하십시오.

    ```bash
    https://gateway.watsonplatform.net/language-translator/api/v3/translate?version=2018-05-01
    ```
    {: codeblock}

    호출하는 외부 애플리케이션이 응답을 리턴하는 경우에는 응답을 JSON 형식으로 다시 보낼 수 있어야 합니다. 예를 들어, 언어 변환기 서비스의 경우, 결과를 리턴할 형식을 지정해야 합니다. 서비스에 헤더를 전달하여 이를 수행할 수 있습니다.

1.  헤더 섹션에서, **헤더 추가**를 클릭하여 한 번에 하나씩 서비스에 전달할 헤더를 추가하십시오.

    예를 들어, 결과 값이 JSON 형식으로 리턴되어야 함을 나타내는 헤더를 추가하십시오.

    <table>
    <caption>헤더 예제</caption>
      <tr>
      <th>헤더 이름</th>
      <th>헤더 값</th>
      </tr>
      <tr>
      <td>Content-Type</td>
      <td>application/json</td>
      </tr>
    </table>
    
1.  외부 서비스에 요청과 함께 기본 인증의 인증 정보를 전달해야 하는 경우 이를 제공하십시오. **권한 추가**를 클릭하고 인증 정보를 **사용자 이름** 및 **비밀번호** 필드에 추가한 다음 **저장**을 클릭하십시오. 

    이 제품은 인증 정보에서 Base-64 인코딩된 ASCII 문자열을 작성하고 사용자를 위해 페이지에 추가하는 헤더를 생성합니다.  

    <table>
    <caption>헤더 예제</caption>
      <tr>
      <th>헤더 이름</th>
      <th>헤더 값</th>
      </tr>
      <tr>
      <td>Authorization</td>
      <td>Basic `<encoded-api-key>`</td>
      </tr>
    </table>
    
    테스트 용도로 기본 인증을 통해 API 키를 제출할 수 있습니다. **권한 추가**를 클릭한 다음 `apikey`를 **사용자 이름** 필드에 추가하고 API 키 값을 **비밀번호** 필드에 붙여 넣으십시오. **저장**을 클릭하십시오.
    {: tip}

웹 후크 세부사항이 자동으로 저장됩니다.

## 대화 노드에 웹 후크 콜아웃 추가
{: #dialog-webhooks-dialog-node-callout}

대화 노드에서 웹 후크를 사용하려면 노드에서 웹 후크를 사용하도록 설정한 다음 콜아웃에 대한 세부사항을 추가해야 합니다.

1.  **대화** 탭을 클릭합니다.

1.  콜아웃을 추가할 대화 노드를 찾으십시오. 웹 후크에 대한 콜아웃은 이 노드가 사용자와의 대화 중에 트리거될 때마다 발생합니다.

    예를 들어, `#General_Greetings` 노드에서 웹 후크에 콜아웃을 보낼 수 있습니다. 

1.  대화 노드를 클릭하여 연 후 **사용자 정의**를 클릭하십시오.

1.  *웹 후크* 섹션을 아래로 스크롤한 다음 토글을 **On**으로 전환하고 **적용**을 클릭하십시오. 

    아직 사용으로 설정하지 않은 경우 *다중 조건부 응답*이 사용으로 자동 전환되며 이를 사용 안함으로 설정할 수 없습니다. 이 설정은 웹 후크 호출의 성공 또는 실패에 따라 다른 응답을 추가하는 것을 지원하기 위해서 사용으로 설정됩니다. 노드에 지정된 응답이 이미 있는 경우 첫 번째 조건부 응답이 됩니다.     {: note}

1.  외부 애플리케이션에 전달할 데이터를 *매개변수* 섹션의 키 및 값 쌍으로 추가하십시오.

    예를 들어, 언어 변환기 서비스를 호출하는 경우 다음 매개변수에 대한 값을 제공해야 합니다.

    <table>
    <caption>매개변수 예제</caption>
      <tr>
        <th>키</th>
        <th>값</th>
        <th>설명</th>
      </tr>
      <tr>
        <td>model_id</td>
        <td>"en-es"</td>
        <th>입력 및 출력 언어를 식별합니다. 이 예제에서 요청은 영어(en)로 된 텍스트를 스페인어(es)로 변환합니다. </th>
      </tr>
      <tr>
        <td> text</td>
        <td>"How are you?"</td>
        <th>이 매개변수에는 서비스가 변환할 텍스트 문자열이 포함되어 있습니다. 이 값을 하드 코드화하거나 $ssaved_text와 같은 컨텍스트 변수로 전달하거나 `<? input.text ?>`로 지정하여 서비스에 직접 사용자 입력을 전달할 수 있습니다. </th>
      </tr>
    </table>

    더 복잡한 유스 케이스에서는, 예를 들어 여행 계획에 대해 사용자와 대화하는 동안 정보를 수집할 수 있습니다. 날짜 및 목적지 정보를 수집하고 외부 애플리케이션에 매개변수로 전달할 수 있는 컨텍스트 변수에 이를 저장할 수 있습니다.

    <table>
    <caption>여행 매개변수 예제</caption>
      <tr>
        <th>키</th>
        <th>값</th>
      </tr>
      <tr>
        <td>depart_date</td>
        <td>$departure</td>
      </tr>
    <tr>
        <td>arrive_date</td>
        <td>$arrival</td>
      </tr>
    <tr>
        <td>origin</td>
        <td>$origin</td>
      </tr>
    <tr>
        <td>destination</td>
        <td>$destination</td>
      </tr>
    </table>

1.  콜아웃에서 작성된 모든 응답은 리턴 변수에 저장됩니다. 사용자의 **리턴 변수** 필드에 자동으로 추가되는 변수의 이름을 바꿀 수 있습니다. 콜아웃 결과 오류가 발생하면 이 변수는 `널`로 설정됩니다.

    생성된 변수 이름에는 구문 `webde_result_n`이 있으며, 끝에 있는 `_n`이 웹 후크 콜아웃을 대화 노드에 추가할 때마다 증가됩니다. 이 이름 지정 규칙은 대화 상자 스킬에서 컨텍스트 변수 이름이 고유하도록 보장합니다. 이름을 변경하는 경우 고유한 이름을 사용해야 합니다.

1.  조건부 응답 섹션에서, 두 개의 응답 조건이 자동으로 추가되는데, 한 응답은 웹 후크 콜아웃이 성공하고 리턴 변수가 다시 전송될 때 표시됩니다. 다른 응답은 콜아웃이 실패할 때 표시됩니다. 이러한 응답을 편집하고 노드에 더 많은 조건부 응답을 추가할 수 있습니다.

    콜아웃이 응답을 리턴하고 JSON 응답의 형식을 알고 있는 경우에는 사용자와 공유하려는 응답의 섹션만 포함하도록 대화 노드 응답을 편집할 수 있습니다. 
    
    예를 들어, 언어 변환기 서비스는 다음과 같은 오브젝트를 리턴합니다.
    
    ```json
       {
       "translations":[
          {"translation":"¿Cómo estás?"}
       ],
       "word_count":3,
       "character_count":12
       }
    ```
    {: codeblock}
    
    변환된 텍스트 값만 추출하는 SpEL 표현식을 사용하십시오.

    <table>
    <caption>조건부 응답 예제</caption>
      <tr>
        <th>조건</th>
        <th>응답</th>
      </tr>
      <tr>
        <td>$webhook_result_1</td>
        <td>Your words in Spanish: <? $webhook_result_1.translations[0].translation ?>.</td>
      </tr>
      <tr>
        <td>anything_else</td>
        <td>외부 애플리케이션에 대한 호출이 실패했습니다. 나중에 다시 시도하십시오.</td>
      </tr>
    </table>

    응답에 권장되는 형식을 사용하고 이전에 표시된 변환 응답이 리턴되는 경우, 사용자에 대한 어시스턴트의 응답은 다음과 같습니다. `Your words in Spanish: ¿Cómo estás?`

1.  완료되면, X를 클릭하여 노드를 닫으십시오. 변경사항이 자동으로 저장됩니다.

## 웹 후크 테스트
{: #dialog-webhooks-test}

웹 후크 콜아웃을 처음 추가할 때 외부 애플리케이션, 데이터 및 형식에서 응답에 리턴된 내용을 정확하게 확인하는 것이 유용할 수 있습니다. 이를 위해서 성공적인 콜아웃 조건부 응답에 대한 텍스트 응답으로 이 표현식을 사용하십시오.  `$webhook_result_n` 여기서 `n`은 테스트하고 있는 웹 후크의 번호입니다. 

이 응답은 리턴 변수의 전체 본문을 리턴하므로 콜아웃이 다시 전송하는 것을 확인하고 사용자와 공유할 내용을 결정할 수 있습니다. 그런 다음 [표현식 언어 메소드](/docs/services/assistant?topic=assistant-dialog-methods)에 문서화된 메소드를 사용하여 응답에서 관심있는 정보만 추출할 수 있습니다.

특정 사용자 입력이 콜아웃에서 오류를 생성할 수 있는지 여부를 테스트하고 이러한 상황을 처리할 수 있는 방식으로 빌드하십시오. 외부 애플리케이션에서 생성되는 오류는 `output.webhook_error.<result_variable>`에 저장됩니다. 이러한 오류를 캡처하기 위해 테스트하는 동안 다음과 같은 조건부 응답을 사용할 수 있습니다.

|조건 |응답 |
|-----------|----------|
| output.webhook_error | 콜아웃에서 다음 오류가 생성됨: <? output.webhook_error.webhook_result_1 ?>. |

예를 들어, 요청을 적절히 인증하지 않거나 (401) 외부 애플리케이션에서 이미 사용 중인 이름으로 매개변수를 전달하려고 할 수 있습니다. 웹 후크를 배치하기 전에 웹 후크를 테스트하여 이러한 유형의 오류를 발견하고 수정하십시오.

## 웹 후크 제거
{: #dialog-webhooks-delete}

대화 노드에서 웹 후크 호출을 작성하지 않기로 결정한 경우 노드의 *사용자 정의* 페이지를 연 다음 웹 후크를 **Off**로 전환하십시오. 

*매개변수* 섹션 및 **리턴 변수** 필드가 대화 노드 편집기에서 제거됩니다. 그러나 사용자를 대신해 추가했거나 사용자가 추가한 모든 조건부 응답은 남아 있습니다.

*다중 조건부 응답* 섹션을 다시 편집할 수 있습니다. 기능을 끄도록 선택할 수 있습니다. 그렇게 하는 경우, 첫 번째 조건부 응답만이 노드의 유일한 텍스트 응답으로 저장됩니다.

대화 노드에서 호출하는 외부 서비스를 변경하려면 **옵션** 탭의 웹 후크 페이지에 정의된 웹 후크 세부사항을 편집하십시오. 새 서비스에서 다른 매개변수가 전달될 것으로 예상하는 경우 이를 호출하는 대화 노드를 업데이트해야 합니다.

## IBM Cloud Functions 호출
{: #dialog-webhooks-cf}

표준 조치 또는 웹 조치를 호출하는지 여부에 따라 웹 후크 URL을 작성하고 헤더를 다르게 제공합니다.

### 웹 액션 호출
{: #dialog-webhooks-cf-web-action}

다음 팁은 대화 상자에서 {{site.data.keyword.openwhisk_short}} 웹 조치를 호출하는 데 도움이 됩니다. 

1.  스킬에 대한 **옵션** 페이지를 열고 **웹 후크**를 클릭하십시오. 

1.  **URL** 필드에서 HTTP POST 요청 콜아웃을 전송할 외부 애플리케이션의 URL을 추가하십시오.

    예를 들어, {{site.data.keyword.openwhisk_short}} 웹 조치를 호출하려면 공용 웹 조치에 대한 URL을 지정하십시오. 예:

    ```bash
    https://us-south.functions.cloud.ibm.com/api/v1/web/my_org_dev/default/Hello%20World.json
    ```
    {: codeblock}

    호출하는 외부 애플리케이션이 응답을 리턴하는 경우에는 응답을 JSON 형식으로 다시 보낼 수 있어야 합니다. 

    이 예제의 요청 URL은 `.json`으로 끝남을 주의하십시오. 이 확장을 지정하면 원하는 응답의 컨텐츠 유형을 지정할 수 있는 웹 조치의 기능을 이용할 수 있습니다. 이 확장 유형을 지정하면 웹 조치가 둘 이상의 형식으로 응답을 리턴할 수 있는 경우 JSON 응답이 리턴됩니다. 자세한 내용은 [추가 기능](/docs/openwhisk?topic=cloud-functions-actions_web#actions_web_extra){: new_window}을 참조하십시오.
    {: tip}

1.  헤더를 추가할 필요는 없습니다.

    {{site.data.keyword.openwhisk_short}} 웹 조치는 인증이 필요하지 않으므로 권한 헤더를 정의할 필요가 없습니다. 

    웹 후크 세부사항이 자동으로 저장됩니다.

1.  **대화** 탭을 클릭합니다.

1.  웹 액션을 호출할 대화 노드를 클릭하여 연 후 **사용자 정의**를 클릭하십시오.

1.  *웹 후크* 섹션을 아래로 스크롤한 다음 토글을 **On**으로 전환하고 **적용**을 클릭하십시오. 

1.  외부 애플리케이션에 전달할 데이터를 *매개변수* 섹션의 키 및 값 쌍으로 추가하십시오.

    예를 들어, Hello World {{site.data.keyword.openwhisk_short}} 웹 조치를 호출하는 경우, 다음과 같은 정보를 추가하여 해당 애플리케이션에서 허용하는 메시지 매개변수를 전달할 수 있습니다.

    <table>
    <caption>매개변수 예제</caption>
      <tr>
        <th>키</th>
        <th>값</th>
      </tr>
      <tr>
        <td>message</td>
        <td>"hello"</td>
      </tr>
    </table>

    {{site.data.keyword.openwhisk_short}} 웹 조치를 호출할 때 웹 조치의 일부로 정의된 매개변수와 동일한 키로 매개변수를 전달할 수 없습니다. 자세한 내용은 [보호된 매개변수](/docs/openwhisk?topic=cloud-functions-actions_web#actions_web_protect){: new_window}를 참조하십시오. {: note}

1.  사용자에게 표시하려는 응답의 섹션만 포함하도록 대화 노드 응답을 편집할 수 있습니다. 

    Hello World {{site.data.keyword.openwhisk_short}} 웹 조치는 기타 정보와 함께 응답에 메시지 이름 및 값 쌍을 포함합니다. 메시지의 내용만 표시하려면 다음과 같이 `<return-variable>.message` 구문을 사용하여 응답 오브젝트에서 메시지 섹션만 추출할 수 있습니다. 

    <table>
    <caption>조건부 응답 예제</caption>
      <tr>
        <th>조건</th>
        <th>응답</th>
      </tr>
      <tr>
        <td>$webhook_result_1</td>
        <td>애플리케이션이 "$webhook_result_1.message"를 리턴했습니다.</td>
      </tr>
      <tr>
        <td>anything_else</td>
        <td>외부 애플리케이션에 대한 호출이 실패했습니다. 나중에 다시 시도하십시오.</td>
      </tr>
    </table>

1.  완료되면, X를 클릭하여 노드를 닫으십시오. 변경사항이 자동으로 저장됩니다.

### 표준 액션 호출
{: #dialog-webhooks-cf-action}

토큰 기반 IAM(Identity and Access Management) 인증을 사용하는 조치가 아닌, Cloud Foundry에서 관리하는 조치를 호출할 수 있습니다.

{{site.data.keyword.openwhisk_short}} 조치는 동기식 또는 비동기식 호출을 모두 지원합니다. 그러나 웹 후크를 사용하여 {{site.data.keyword.openwhisk_short}} 조치에 대한 비동기 호출을 작성할 수 없습니다. 비동기 요청을 보낼 때 활성화 ID만 리턴됩니다. 

Cloud Foundry에서 관리하는 {{site.data.keyword.openwhisk_short}} 조치에 대한 동기 호출을 작성하려면 다음 단계를 완료하십시오. 

1.  웹 후크를 추가하려는 스킬에서 **옵션** 탭을 클릭하십시오.

1.  **웹 후크**를 클릭하십시오.

1.  **URL 필드** 조치에 대한 URL을 지정하십시오.  

    동기 호출을 작성하도록 조치 URL에 `?blocking=true` 매개변수를 추가하십시오. 

    다음 구문은 동기 요청을 전송합니다. 

    ```bash
    https://us-south.functions.cloud.ibm.com/api/v1/namespaces/my_org_dev/actions/Hello%20World?blocking=true
    ```
    {: codeblock}

    다음 단계에서 설명된 대로, 조치를 호출할 때 조치와 연관된 API 키를 헤더로 제공해야 합니다. 

1.  요청과 함께 권한 세부사항을 제공하십시오.

    Cloud Foundry에서 관리하는 {{site.data.keyword.openwhisk_short}} 조치를 호출하려면 다음 단계를 완료하십시오. 

    1.  API 키를 가져오십시오. {{site.data.keyword.openwhisk_short}} 엔드포인트 페이지에서 CURL 섹션을 찾은 후 눈 아이콘을 클릭하여 키 세부사항을 표시하십시오. curl 명령에서 `-u` 매개변수 뒤에 나열된 `user ID:password` 키를 복사하십시오. 
    
    1.  **권한 추가**를 클릭하고 인증 정보를 **사용자 이름** 및 **비밀번호** 필드에 추가한 다음 **저장**을 클릭하십시오. 

    인증 정보가 인코딩되고 헤더가 생성되어 페이지에 추가됩니다. 
    
    ![옵션 페이지의 URL 필드 및 헤더 섹션을 표시합니다.](images/webhook-to-cfaction.png)

    <!-- - If you are calling a {{site.data.keyword.openwhisk_short}} action that is managed by IBM Cloud Identity and Access Management (IAM) instead of CLoud Foundry, then you must provide an IAM bearer token to authenticate the request. 
    
      1. Follow the instructions in [Passing an IBM Cloud IAM token to authenticate with a service's API](/docs/iam?topic=iam-iamapikeysforservices#token_auth). 
      
      1. To pass the IAM bearer token, use a header like this:
    
         <table>
         <caption>Header example</caption>
           <tr>
             <th>Header name</th>
             <th>Header value</th>
           </tr>
           <tr>
             <td>Authorization</td>
             <td>Bearer `<IAM token>`</td>
           </tr>
         </table>

        For more information about how to manage IAM tokens, see this [IBM Developer article](https://developer.ibm.com/tutorials/accessing-iam-based-services-from-ibm-cloud-functions/){: external}.
    -->
웹 후크 세부사항이 자동으로 저장됩니다.

1.  **대화** 탭을 클릭합니다.

1.  웹 액션을 호출할 대화 노드를 클릭하여 연 후 **사용자 정의**를 클릭하십시오.

1.  *웹 후크* 섹션을 아래로 스크롤한 다음 토글을 **On**으로 전환하고 **적용**을 클릭하십시오. 

1.  외부 애플리케이션에 전달할 데이터를 *매개변수* 섹션의 키 및 값 쌍으로 추가하십시오.

    {{site.data.keyword.openwhisk_short}} 조치를 호출할 때 조치의 일부로 정의된 매개변수와 동일한 키로 매개변수를 *전달할 수 있습니다*. 매개변수는 웹 조치의 경우와 같이 예약되지 않습니다. 

1.  사용자에게 표시하려는 응답의 섹션만 포함하도록 대화 노드 응답을 편집할 수 있습니다. 

    예를 들어, 조건부 응답 섹션에서는 `$webhook_result.response.result.message` 구문으로 표현식을 사용하여 리턴된 메시지만 추출할 수 있습니다. 

    <table>
    <caption>조건부 응답 예제</caption>
      <tr>
        <th>조건</th>
        <th>응답</th>
      </tr>
      <tr>
        <td>$webhook_result_1</td>
        <td>애플리케이션이 "$webhook_result.response.result.message"를 리턴했습니다.</td>
      </tr>
      <tr>
        <td>anything_else</td>
        <td>외부 애플리케이션에 대한 호출이 실패했습니다. 나중에 다시 시도하십시오.</td>
      </tr>
    </table>

1.  완료되면, X를 클릭하여 노드를 닫으십시오. 변경사항이 자동으로 저장됩니다.
